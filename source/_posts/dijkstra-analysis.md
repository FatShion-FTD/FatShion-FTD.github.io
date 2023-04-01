---
title: dijkstra 迪杰斯特拉算法 浅析
date: 2022-12-06 00:08:38
categories: 学习
tags: [算法, 刷题, 找工, 图论]
keywords:
---

# Dijkstra 迪杰斯特拉算法
## 前言
这个算法我第一次是在大一接触的, 当时的C语言课程, 最后的 final project 要求实现一个火车票购买系统, 给了好多单向的火车票, 要求目标两站之间最小花费, 我记得当时我用的极其过分的O($N^N$)遍历实现, 所以老师呢, 也给了我极其过分的 $及格^{及格}$ 分. 不出所料的, 那课 GPA 挺炸.

好啦, 那这个问题和 Dijkstra有什么关系呢? 实际上


## 原理
最小路径优先的贪心思想 + 

## 算法实现
```java
import java.util.Arrays;
import java.util.PriorityQueue;

public class Dijkstra {
    // 图中节点的数量
    private int n;
    // 邻接矩阵，存储图中的边的权值
    private int[][] adj;
    // dist[i] 表示源点到节点 i 的最短路径距离
    private int[] dist;
    // pre[i] 表示源点到节点 i 的最短路径中，节点 i 的前驱节点
    private int[] pre;
    // 存储图中未被访问过的节点
    private PriorityQueue<Integer> pq;

    public Dijkstra(int n) {
        this.n = n;
        adj = new int[n][n];       
        // 初始化所有边为无穷大
        for (int[] arr:adj)
            Arrays.fill(arr, Integer.MAX_VALUE);
        dist = new int[n];
        pre = new int[n];
        // 初始化距离数组，所有节点到源点的距离都设为无穷大
        Arrays.fill(dist, Integer.MAX_VALUE);
        // 初始化前驱节点数组，所有节点的前驱节点都设为源点
        Arrays.fill(pre, 0);
        // 使用优先队列存储未访问过的节点
        pq = new PriorityQueue<>((a, b) -> dist[a] - dist[b]);
    }

    // 计算源点到目标节点的最短路径
    public void dijkstra(int source, int target) {
        // 将源点加入集合 S 中
        pq.offer(source);
        // 将源点到源点的距离初始化为 0
        dist[source] = 0;

        while (!pq.isEmpty()) {
            // 取出当前最短路径最小的节点 u
            int u = pq.poll();
            // 如果已遍历到目标节点，就退出循环
            if (u == target) {
                break;
            }
            // 遍历节点 u 的所有邻接节点 v
            for (int v = 0; v < n; v++) {
                if (adj[u][v] == Integer.MAX_VALUE)     
                    continue;
                // 如果节点 v 未被访问过，并且从源点到节点 v 的距离比从源点到节点 u 再到节点 v 的距离更短
                if (dist[v] > dist[u] + adj[u][v]) {
                    // 更新源点到节点 v 的距离
                    dist[v] = dist[u] + adj[u][v];
                    // 设置节点 v 的前驱节点为 u
                    pre[v] = u;
                    // 将节点 v 加入集合 S 中
                    pq.offer(v);
                }
            }
        }
    }

    // 获取源点到目标节点的最短路径
    public String getPath(int target) {
        StringBuilder sb = new StringBuilder();
        // 从目标节点开始，不断查找它的前驱节点，直到找到源点为止
        while (target != 0) {
            sb.append(target + " <- ");
            target = pre[target];
        }
        sb.append(0);
        return sb.toString();
    }

    // 获取源点到目标节点的最短路径距离
    public int getDistance(int target) {
        return dist[target];
    }

    public static void main(String[] args) {
        Dijkstra dijkstra = new Dijkstra(6);
        // 添加边，图中的边都是单向边
        dijkstra.adj[0][1] = 2;
        dijkstra.adj[0][2] = 4;
        dijkstra.adj[1][2] = 1;
        dijkstra.adj[1][3] = 7;
        dijkstra.adj[2][3] = 3;
        dijkstra.adj[2][4] = 2;
        dijkstra.adj[3][4] = 5;
        dijkstra.adj[3][5] = 1;
        dijkstra.adj[4][5] = 1;
        // 计算源点到目标节点的最短路径
        dijkstra.dijkstra(0, 5);
        // 输出源点到目标节点的最短路径
        System.out.println(dijkstra.getPath(5));
        // 输出源点到目标节点的最短路径距离
        System.out.println(dijkstra.getDistance(5));
    }
}
```

输出结果:
```cmd
5 <- 4 <- 2 <- 1 <- 0
6
```