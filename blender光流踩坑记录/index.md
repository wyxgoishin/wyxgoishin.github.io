# Blender光流踩坑记录


本文章主要记录利用 Blender 生成带光流标签数据集时遇到的各种天坑。

<!--more-->

## 前言

因为课题需要，需要用 Blender 生成一批特定场景的带光流标签数据集用作网络训练，于是就开始了我漫长的踩坑实录，这篇文章也是因此而来。

## 正文

首先是关于 Blender 渲染方面。通过查找得知要得到光流输出需要设置渲染引擎为 Cycle，同时关闭 Motion Blur 选项，即：

![](https://s2.loli.net/2022/05/17/5beYtUFMpDI4yuh.png)

但注意仅此操作，上图第一个红色选框处的 GPU Compute 会是灰色（图中已经有了额外操作），这表明最终渲染还是用 CPU（<del>天知道之前一张 1080P 的场景我渲了 8 小时？？？</del>）。实际上在设置中（Edit -> Preference）中还要额外设置如下图所示。其中 Cycles Render Devices 下面的四个选项是让你选择用哪种方式进行渲染，而不是对应渲染方式的设置（<del>这样一来速度啪一下变成了秒级</del>）。

![](https://s2.loli.net/2022/05/17/2XqLDZRcm9Ppk6C.png)

最后是关于光流输出，一开始不懂直接 Github 查找到一个插件 [Vision Blender](https://github.com/Cartucho/vision_blender)，但这插件还有版本适配问题，好多版本并不能按描述的那样得到 npy 格式的光流。然后就 Google，首先要在 Passes 里勾选 Vector 输出如下图：

![](https://s2.loli.net/2022/05/17/9h3ULdCAoN46vQi.png)

然后在 Compositing 选栏下勾选 Use Nodes，Shift + A 搜索添加一个 File Output，并将 Render Layers 层的 Vector 输出与其名字（默认 image）相连（这点与  [Vision Blender](https://github.com/Cartucho/vision_blender) 代码中表现一致）。然后修改点击右侧选栏 Node 更改 File Output 的属性，最主要是改修改输出格式和文件前缀名和输出文件夹，注意对于光流最好输出 OpenExr 格式，色彩选择 RGBA，比如：

![](https://s2.loli.net/2022/05/17/YAfPV9Ocp2UWoMG.png)

![](https://s2.loli.net/2022/05/17/YcK6jQOfZVXHmUt.png)

然后根据 [Blender 官方文档解释](https://docs.blender.org/manual/en/latest/render/layers/passes.html)（<del>太含糊了</del>）Vector（光流）输出代表当前帧到前一帧和下一帧的光流。但这话这么说也没讲出四维色彩

> The four components consist of 2D vectors giving the motion towards the next and previous frame position in pixel space.

通道值和具体光流值的对应方式。那么继续 Google，找到一个 [教程](http://www.tobias-weis.de/groundtruth-data-for-computer-vision-with-blender/)，里面指出 R, G 通道对应当前帧到下一帧的光流的 X 和 Y 方向，但 Y 方向的值需要取反，B，A  对应下一帧到当前帧的光流，同理 Y 方向需要取反。

> Important information: The flow-values are saved in the R/G-channel of the output-image, where the R-channel contains the movement of each pixel in x-direction, the G-channel the y-direction. The offsets are encoded from the current to the previous frame, so the flow-information from the very first frame will not yield usable values. Also, in blender the y-axis points upwards. (The B/A-channels contain the offsets from the next to the current frame).

那么就试试开搞，然后发现如此渲染出来的光流总是没有 Alpha 通道，继续 Google，查到一个没有正确解得开放回答（<del>找不到链接了</del>），经验证后选取一种方法是有效的，即先拆分再合并 RGBA：

![](https://s2.loli.net/2022/05/17/Hpej7BruPCXTVhJ.png)

然后再经过代码验证，发现对于 B，A 通道的光流含义解释依旧有误。对于 Blender 3.1.2 版本，其 B，A 通道分别为当前帧到下一帧的 X 方向光流的相反数以及 Y 方向光流。

最后，这里给出了一个 [简单的运动场景](https://pan.baidu.com/s/1iZduoSOy2sjxY2mhnIBHgA?pwd=t5dg) 和一段代码用于验证，有兴趣的读者可以自行尝试：

```python
# warp 部分代码摘自 ARFlow(https://github.com/lliuz/ARFlow)
import torch
import torch.nn as nn
import inspect
import numpy as np
import cv2
import Imath
import array
import OpenEXR
import matplotlib.pyplot as plt


def mesh_grid(B, H, W):
    # mesh grid
    x_base = torch.arange(0, W).repeat(B, H, 1)  # BHW
    y_base = torch.arange(0, H).repeat(B, W, 1).transpose(1, 2)  # BHW

    base_grid = torch.stack([x_base, y_base], 1)  # B2HW
    return base_grid


def norm_grid(v_grid):
    _, _, H, W = v_grid.size()

    # scale grid to [-1,1]
    v_grid_norm = torch.zeros_like(v_grid)
    v_grid_norm[:, 0, :, :] = 2.0 * v_grid[:, 0, :, :] / (W - 1) - 1.0
    v_grid_norm[:, 1, :, :] = 2.0 * v_grid[:, 1, :, :] / (H - 1) - 1.0
    return v_grid_norm.permute(0, 2, 3, 1)  # BHW2


    """
    :param data: unnormalized coordinates Bx2xHxW
    :return: Bx1xHxW
    """
    B, _, H, W = data.size()

    # x = data[:, 0, :, :].view(B, -1).clamp(0, W - 1)  # BxN (N=H*W)
    # y = data[:, 1, :, :].view(B, -1).clamp(0, H - 1)

    x = data[:, 0, :, :].view(B, -1)  # BxN (N=H*W)
    y = data[:, 1, :, :].view(B, -1)

    # invalid = (x < 0) | (x > W - 1) | (y < 0) | (y > H - 1)   # BxN
    # invalid = invalid.repeat([1, 4])

    x1 = torch.floor(x)
    x_floor = x1.clamp(0, W - 1)
    y1 = torch.floor(y)
    y_floor = y1.clamp(0, H - 1)
    x0 = x1 + 1
    x_ceil = x0.clamp(0, W - 1)
    y0 = y1 + 1
    y_ceil = y0.clamp(0, H - 1)

    x_ceil_out = x0 != x_ceil
    y_ceil_out = y0 != y_ceil
    x_floor_out = x1 != x_floor
    y_floor_out = y1 != y_floor
    invalid = torch.cat([x_ceil_out | y_ceil_out,
                         x_ceil_out | y_floor_out,
                         x_floor_out | y_ceil_out,
                         x_floor_out | y_floor_out], dim=1)

    # encode coordinates, since the scatter function can only index along one axis
    corresponding_map = torch.zeros(B, H * W).type_as(data)
    indices = torch.cat([x_ceil + y_ceil * W,
                         x_ceil + y_floor * W,
                         x_floor + y_ceil * W,
                         x_floor + y_floor * W], 1).long()  # BxN   (N=4*H*W)
    values = torch.cat([(1 - torch.abs(x - x_ceil)) * (1 - torch.abs(y - y_ceil)),
                        (1 - torch.abs(x - x_ceil)) * (1 - torch.abs(y - y_floor)),
                        (1 - torch.abs(x - x_floor)) * (1 - torch.abs(y - y_ceil)),
                        (1 - torch.abs(x - x_floor)) * (1 - torch.abs(y - y_floor))],
                       1)
    # values = torch.ones_like(values)

    values[invalid] = 0

    corresponding_map.scatter_add_(1, indices, values)
    # decode coordinates
    corresponding_map = corresponding_map.view(B, H, W)

    return corresponding_map.unsqueeze(1)


def flow_warp(x, flow12, pad='border', mode='bilinear'):
    B, _, H, W = x.size()

    base_grid = mesh_grid(B, H, W).type_as(x)  # B2HW

    v_grid = norm_grid(base_grid + flow12)  # BHW2
    if 'align_corners' in inspect.getfullargspec(torch.nn.functional.grid_sample).args:
        im1_recons = nn.functional.grid_sample(x, v_grid, mode=mode, padding_mode=pad, align_corners=True)
    else:
        im1_recons = nn.functional.grid_sample(x, v_grid, mode=mode, padding_mode=pad)
    return im1_recons


def warp_vis(cur, nxt, flow, pad='border', mode='bilinear'):
	"""
	cur, nxt, flow: ndarray, H * W * C
	"""

	device = 'cuda' if torch.cuda.is_available() else 'cpu'
	cur = torch.from_numpy(cur).double()[None].permute(0, 3, 1, 2).to(device)
	nxt = torch.from_numpy(nxt).double()[None].permute(0, 3, 1, 2).to(device)
	flow = torch.from_numpy(flow).double()[None].permute(0, 3, 1, 2).to(device)
	nxt_warp = flow_warp(nxt, flow, pad, mode)
	nxt_warp = nxt_warp[0].permute(1, 2, 0).cpu().numpy().astype(np.uint8)
	cur = cur[0].permute(1, 2, 0).cpu().numpy().astype(np.uint8)
	nxt = nxt[0].permute(1, 2, 0).cpu().numpy().astype(np.uint8)
	fig, axs = plt.subplots(1, 3)
	axs[0].imshow(cur)
	axs[0].set_title('cur')
	axs[1].imshow(nxt)
	axs[1].set_title('nxt')
	axs[2].imshow(nxt_warp)
	axs[2].set_title('nxt_warp')
	plt.show()


def exr2flow(exr_path, w, h):
	file = OpenEXR.InputFile(exr_path)
	FLOAT = Imath.PixelType(Imath.PixelType.FLOAT)
	(R,G,B,A) = [array.array('f', file.channel(Chan, FLOAT)).tolist() for Chan in ("R", "G", "B","A") ]
	flow = np.zeros((h,w,4), np.float64)
	flow[:,:,0] = np.array(R).reshape(flow.shape[0],-1)
	flow[:,:,1] = -np.array(G).reshape(flow.shape[0],-1)
	flow[:,:,2] = np.array(B).reshape(flow.shape[0],-1)
	flow[:,:,3] = -np.array(A).reshape(flow.shape[0],-1)
	return flow


if __name__ == '__main__':
    im1, im2, im3 = cv2.imread('0001.png'), cv2.imread('0002.png'), cv2.imread('0003.png')
    h, w = im1.shape[:2]
    flow = exr2flow('Flow0002.exr', w, h)
    warp_vis(im2, im1, flow[:,:,:2]) # R,G
    warp_vis(im2, im3, -flow[:,:,2:]) # B,A
```




