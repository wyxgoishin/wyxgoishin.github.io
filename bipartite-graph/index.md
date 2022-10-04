# 【图论系列】二分树


今天学了二分图，暂且先记录下。

<!--more-->

- [二分图的定义](https://oi-wiki.org/graph/bi-graph/)

二分图，又称二部图，英文名叫 Bipartite graph。对于某个二分图 G = {V, E}，存在一种关于分割方案，G1 = {V1, E1}, G2 = {V2, E2}，即 V1 + V2 = V 且 E1 + E2 = E，使得 ${\forall} e_{uv} \in E1 \ or \ E2, \text{u,v不同时属于V1或V2}$。 下面是一张示意图：

![二分图](https://oi-wiki.org/graph/images/bi-graph.svg)

- [二分图的性质](https://oi-wiki.org/graph/bi-graph/)

1. 如果两个集合中的点分别染成黑色和白色，可以发现二分图中的每一条边都一定是连接一个黑色点和一个白色点。
2. 二分图不存在长度为奇数的环（因为每一条边都是从一个集合走到另一个集合，只有走偶数次才可能回到同一个集合）

- [二分图的判定](https://oi-wiki.org/graph/bi-graph/)

从上面的性质可以看到，我们只需要判定是否可以将图中的顶点分成两个满足条件的集合出来。那么采用染色的方法，遍历尝试是否能将整张图染成两种颜色。其步骤如下：

1. 遍历所有节点，首先将所有孤立节点标记为颜色A
2. 将所有非独立节点加入某集合，而后随机选取某个节点T，赋予颜色A并加入队列
3. 若队列为空，则先从2中集合弹出某随机节点T，赋予颜色A并加入队列；反之直接从队列中弹出某随机节点S
4. 对于所有与节点S相连的节点T
   1. 若尚未遍历节点T，则赋予其与S相反的颜色
   2. 反之若T的颜色与S颜色相同，说明同一颜色的集合内部存在连接，因而存在奇数环，遍历结束
   3. 最后将节点T从2中的集合中删除

对应的 `java` 代码如下：

```java
public class Solution{
    private final static Random rand = new Random();
    
    // 随机初始化一个矩阵表示的图
    public static boolean[][] initRandomGraph(int vNum){
        boolean[][] graph = new boolean[vNum][vNum];
        for(int i = 0; i < vNum; ++i){
            for(int j = 0; j < vNum; ++j){
                if(rand.nextInt() > 0){
                    graph[i][j] = true;
                }
            }
        }
        return graph;
    }
    
    public static boolean isBGraph(boolean[][] graph, int vNum){
        // 颜色记录
        int[] colors = new int[vNum];
        Arrays.fill(colors, -1);
        
        // 记录未访问的节点
        Set<Integer> unseen = new HashSet<>();
        for(int i = 0; i < vNum; ++i){
            unseen.add(i);
        }
        
        // 染色所有孤立节点
        int start = 0;
        while(start < vNum && isAlone(graph, start, vNum)){
            unseen.remove(start);
            colors[start++] = 1;
        }
        
        // 所有点都是孤立节点也是二分图
        if(start == vNum){
            return true;
        }
        
        Deque<Integer> dq = new ArrayDeque<>();
        colors[start] = 1;
        dq.offer(start);
        
        while(unseen.size() > 0 || dq.size() > 0){
            // 存在多个孤立的子图的图的情况
            if(dq.size() == 0){
                for(int num : unseen){
                    start = num;
                    break;
                }
                unseen.remove(start);
                colors[start] = 1;
                dq.offer(start);
            }else{ // 验证单个子图是否存在奇环
                start = dq.poll();
                
                // 自环
                if(graph[start][start]){
                    return false;
                }
                
                for(int i = 0; i < vNum; ++i){
                    if(graph[start][i]){
                        // 对于未访问的节点，标记其为另一个颜色(集合)
                        if(colors[i] == -1){
                            colors[i] = 1 - colors[start];
                            dq.offer(i);
                            unseen.remove(i);
                        }
                        // 同颜色(集合)的节点间存在边，证明存在奇环
                        if(colors[start] == colors[i]){
                            return false;
                        }
                    }
                }
            }
        }
        
        return true;
    }
    
    // 判断是否为孤立节点
    public static boolean isAlone(boolean[][] graph, int loc, int vNum){
        for(int i = 0; i < vNum; ++i){
            if(graph[loc][i]){
                return false;
            }
        }
        return true;
    }
}
```




