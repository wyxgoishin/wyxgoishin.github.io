# 利用 GitHub Actions 来自动化 Hugo 博客的部署


<!--more-->

## 前置准备

在部署 `Hugo` 博客前，我们首先需要有一个 `Hugo` 博客，因此需要预先安装 `Hugo`、 `Go`（`Go` 是 `Hugo` 的运行依赖）和 `Git`（用于版本管理，你也可以选择其他 `SVN`）。

### Go 和 Git 的安装

[Go官网 ](https://golang.google.cn/)和 [Git官网](https://git-scm.com/)下载安装包后一直点下一步即可。

### Hugo 的安装

[官方发布](https://github.com/gohugoio/hugo/releases)处选择适合你电脑的版本下载，需要注意的是有些 `Hugo` 主题需要 `Hugo_extended` 才能使用。

对于 `Windows` 用户，下载下来的压缩包解压后仅包含 `License` 文件、`ReadMe` 和一个可执行文件。为了方便起见，我们可以将其所在的文件夹路径添加系统变量的 `Path` 变量中，这样即可直接在 `cmd` 中使用 `hugo xxx` 命令。

## 搭建博客

### Hugo 简单命令

首先介绍几个简单的 `Hugo` 命令，更多信息可参考[官方文档](https://www.gohugo.org/)。

1. `hugo new site dst`

该命令会在 `dst` 处新建一个 `Hugo` 博客工程文件，`dst` 为 `.` 时则会在当前路径新建。需要注意的是，当 `dst` 对应路径的文件夹已存在，`Hugo` 会报错并提示你在命令中增加 `-force` 参数来强制创建，此时按照提示添加即可创建成功。新建的 `Hugo` 博客工程文件夹下应该类似下图：

```asp
刚完成配置的Hugo文件夹
├── config.toml 总配置文件，可以在里面更改各类配置
├── archetypes 文章原型
│   └── default.md 
├── content 存放文章内容的文件夹
├── data 
├── layouts 布局相关文件
├── static 用于存放你在 content 中的文章会用到的图片等资料
├── public 渲染出来的文章会放在该文件夹下
└── themes 用于存放 Hugo 的主题
```

2. `hugo new dst` 

该命令会在 `content` 文件夹下创建 `dst` 文章，一般该文件应当以 `.md` 结尾，同时根据语言类型在正式文件名和后缀名间加上语言代号以方便后期管理，例如 `post.zh-cn.md`。`dst` 也可以是多级相对路径，例如 `posts/post_1.zh-cn.md`，此时会在 `content/posts` 路径下创建 `post_1.zh-cn.md`。

3. `hugo server -D`

该命令会启动服务器，并使你能在本地预览 `Hugo` 博客，预览地址默认为 `localhost:1313`。

4. `hugo`

该命令会在 `public` 文件夹下生成可部署的静态网页文件。

### 引入 Hugo 主题

假设你已经搭建了一个 `Hugo` 博客工程文件，现在我们可以引入 `Hugo` 主题来简化后续网站的风格搭建工作（<del>当然你是大佬或者你想自己配置风格除外</del>）。`Hugo` 主题可以在[这个网站](https://themes.gohugo.io/)找到，你可以在其中挑选一个你喜欢的，我这里使用的是 [FixIt](https://github.com/Lruihao/FixIt)，同时也推荐几个个人认为比较适合的主题：[Clarity](https://github.com/chipzoller/hugo-clarity)、[Even](https://github.com/olOwOlo/hugo-theme-even)、[Meme](https://github.com/reuixiy/hugo-theme-meme)、[Octopress](https://github.com/parsiya/Hugo-Octopress)。

当你挑选好主题后，你可以直接将其克隆到你的 `themes` 文件下，或者初始化你的项目为 `git` 仓库，并且把主题仓库作为你的网站目录的子模块。

```bash
git init
git submodule add https://github.com/Lruihao/FixIt.git themes/FixIt
```

由于后续我们希望使用 `Github Action` 来便捷部署 `Hugo` 博客，因此这里需要使用第二种方法。下载完主题后，你应当根据主题中的 `ReadMe` 或相关文档配置你的 `config.toml` 文件，另外为了后续部署，你应当将你的主题作为一个子模块挂载到你的博客并修改 `baseURL`，具体来说是应当在你的 `config.toml` 文件中添加修改：

```toml
baseURL = "xxx" # 后续你的博客的地址
[module]
[[module.imports]]
  # xxx 为 themes 文件下你的主题文件夹的名称
  path = 'xxx'
```

另外需要注意的是，如果该行配置应当写在 `theme=xxx` 配置前，同时如果该行写在 `defaultContentLanguage=xxx` 前，则会使得`defaultContentLanguage=xxx`配置失效而导致一些莫名其妙的错误。

## GitHub 配置

首先说说我们自动化部署的最终目的，即希望在本地预览无误后，直接推送工程文件到 `Github` 的某个工程仓库后，我们的主页仓库（即`https://github.com/xxx/xxx.github.io` ）就会自动更新，使得访问 `xxx.github.io` 或者你配置的域名时得到的就是更新的内容，而不用每次手动将工程文件下的`public` 文件夹 `push` 到我们的主页仓库。

为此我们需要使用 `Github Actions` 来完成这一过程，它可以监听围绕代码仓库管理的各种事件，比如 `push` 和 `pull_request` 事件，触发提前计划好的一系列步骤，即工作流。这些步骤用 `YAML `文档记录。触发工作流包括了构建和测试。这些工作流可以在 `GitHub` 的服务器上执行，开发者也可以在自己的服务器响应从 `GitHub` 触发的事件来执行工作流。

### 创建并配置 GitHub 工程仓库

首先创建一个私有的 `Github` 仓库用于存储工程文件，而后在该仓库页 `setting -> Secrets -> Actions` 下通过 `new repository secret` 创建一个新的动作密钥，并且注意**一定要勾选** `Allow write access`。密钥可以采用 `ssh-keygen` 命令生成密钥对，并将私钥填入，下面是参考密钥对生成代码。另外并假设这里你创建的动作密钥名称为 `ACTIONS_DEPLOY_KEY`。

```bash
ssh-keygen -t rsa -b 4096 -C "$(git config user.email)" -f gh-pages -N ""
# You will get 2 files:
#   gh-pages.pub (public key)
#   gh-pages     (private key)
```

### 创建并配置 GitHub 主页仓库

而后创建你的主页仓库，它应当是公开的，同时其仓库名应当是 `xxx.github.io`，其中 `xxx` 为你的 `Github` 用户名。在该仓库页 `setting -> Pages` 你可以看到你的 `Github Page` 地址，同时下面的 `Custom Domain` 可以用以自定义地址的域名（你需要先购买一个并配好相关设置）。然后在 `Deploy keys -> Add deploy key` 中添加对应的部署密钥，其值为先前 `ssh-keygen` 生成的公钥。

### 创建并配置 GitHub Actions

在工程仓库对应的本地文件夹下，创建`.github/workflows/github-pages.yml` 文件，文件名你可以自定义，然后修改文件内容如下：

```yml
# This is a basic workflow to help you get started with Actions

name: Github Pages

# Controls when the workflow will run
on:
  # Triggers the workflow on push or pull request events but only for the main branch
  push:
    branches: [ master ]
  pull_request:
    branches: 

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v2 # 后续版本可能有更新，请选择合适的版本
        with:
          submodules: true # 根据你是否添加子模块而定
          fetch-depth: 0

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2 # 后续版本可能有更新，请选择合适的版本
        with:
          hugo-version: '0.93.2' # 根据你的 Hugo 版本而定
          extended: true # 根据你的 Hugo 版本而定
          
      - name: Build
        run: hugo -D
        
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3 # 后续版本可能有更新，请选择合适的版本
        with:
          DEPLOY_KEY: ${{ secrets.ACTIONS_DEPLOY_KEY }} # 这里对应上面创建的动作密钥名称
          EXTERNAL_REPOSITORY: xxx/xxx.github.io # 部署的主页仓库
          PUBLISH_BRANCH: master
          PUBLISH_DIR: ./public
```

而后通过 `git` 或其他工具将该工程项目 `push` 到 对应`GitHub` 仓库中，等待一小段时间再次访问 `xxx.github.io`，即会发现对应的网页已经自动发生修改。如果发生错误，你会收到一封通知邮件，你可以通过 `Actions` 进入对应的 `workflow` 查找错误原因，并自行 `debug ` 或通过 [谷歌](https://www.google.com/) 和 [百度](www.baidu.com) 搜索相关经验。后续我们要对博客进行修改，也只需要修改工程文件然后 `push` 即可。


