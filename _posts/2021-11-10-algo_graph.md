
# 图算法

[TOC]

权当是对自己学习的一个简单整理了

施工中

## 1.最短路径

### 1.intro

关于图最短路径的一些提前要说的内容
1. 最优子结构：**最短路径的子路仍然是最短路径**，即若`a->b->c->d`是点a到点d的最短路径，则`b->c->d`是点b到点d的最短路径
2. 负权环：当存在负权环时，不存在最短路的概念。（但是存在最短简单路径，某些算法可以通过特判得到）

### 1.1.Floyd - Warshall

弗洛伊德算法又有一个非常形象的名字——插点法。这个名字比较形象地体现了其运行过程：向起点和终点之间一个节点一个节点判断是否插入结点并操作。弗洛伊德算法是一个典型的动态规划(Dynamic Programming)算法，计算任意两点之间的最短路，并且可以处理带有负权的图。

弗洛伊德算法时间复杂度为$O(vertex^3)$， 空间复杂度为$O(vertex^2)$（优化后）

####1.1.1.Dynamic Programming

Floyd算法的本质是动态规划


状态转移方程：
$$
f[k][i][j] = min(f[k-1][i][j], f[k-1][i][k] + f[k-1][k][j])
$$
其中$f[k][i][j]$ 为 i 到 j 只经过前 k 个结点的最短路长


无后效性：由状态转移方程，$f[i][j][k]$只由状态k-1决定，所以只需将关于k的循环放在最外层就可以保证无后效性

####1.1.2.algorithm

伪代码：
```
# @params D[i][j]i到j的最短距离
# 		init as:D[i][j] <- edge[i][j] 即边i -> j的边权

for k from 1 to cnt(vertex)
	for i from 0 to cnt(vertex) - 1
		for j from 0 to cnt(vertex) - 1
			G[i][j] <- min(G[i][j], G[i][k] + G[j][k])
#访问i -> j的最短路长：G[i][j]
```

优化说明：
由于Floyd的状态转移状态$k$只与状态$k-1$有关，所以可以直接在$f[k]$的基础上进行更改，可以将空间复杂度从$O(vertex^3)$优化为$O(vertex^2)$。（滚动数组）
以上伪码已经经过优化。

####1.1.3.求路径

我们可以我们用`path[i][j]`记录从`i`到`j`松弛的节点`k`，那么从`i`到`j`,肯定是先从`i`到`k`，然后再从`k`到`j`， 那么我们在找出`path[i][k]` , `path[k][j]`即可，即 `i`到`k`的最短路是` i -> path[i][k] -> k -> path[k][j] -> k `然后求`path[i][k]`和`path[k][j]` ，一直到某两个节点没有中间节点为止。

```
# 用path[][]记录, 元素初始化为-1
path[cnt(vertex)][cnt(vertex)]
```

在更新最短距离的数组的同时更新：

```
# 更新最短距离数组说明在i, j两个点之间插入了一个结点
path[i][j] <- k
```

得到点a到点b的路径：

```
# 递归
getPath(path, s, e) -> []:
	if path[s][e] = -1
		return []
	else
		return [s] + getPath(path, s, path[s][e]) + path[s][e] + getPath(path,path[s][e], e) + [e]
```



### 1.2.Dijkstra

unfinished



## 参考

1. \[[算法系列——四种最短路算法](https:// zhuanlan.zhihu.com/p/33162490) ] 吕清海
2. \[[图最短路径算法之弗洛伊德算法(Floyd)](https://houbb.github.io/2020/01/23/data-struct-learn-03-graph-floyd)\] houbb
