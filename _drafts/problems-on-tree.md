---
title: problems-on-tree
tags: graph
---

咕咕咕咕了好久啊。

### DSU on tree

来自 CF 的科技，适合求解答案与当前节点与其子树有关的问题。

参考了[这篇文章](https://codeforces.com/blog/entry/44351)。

> 典例：一棵树，树上每个点有颜色，多次询问节点 $u$ 的子树内不同颜色数。

记 $cnt[i]$ 表示点 $i$ 的出现次数，$ans[u]$ 表示点 $u$ 的答案。

考虑对每个点求出重儿子和轻儿子，先遍历其轻儿子，计算答案，但不保留遍历后它对 cnt 的影响；然后遍历重儿子，保留其对 cnt 的影响；最后再次遍历 u 的轻儿子的子树节点，加入这些结点的贡献得到 ans。

第一步和第三步不能合并的原因是因为 cnt 数组不能被重复运用，如果合并空间就炸了。

实现：

```cpp
int n, col[N];
vector<int> g[N];
int siz[N], hson[N], dfn[N], w[N], Time;
int ans[N], cnt[N], tot;
void dfs0(int u, int fath)
{
 dfn[u] = ++Time, w[Time] = u, siz[u] = 1;
 for (int v : g[u])
 {
  if (v == fath) continue;
  dfs0(v, u), siz[u] += siz[v];
  if (!hson[u] || siz[v] > siz[hson[u]]) hson[u] = v;
 }
}
inline void add(int u) { tot += (cnt[col[u]] == 0), cnt[col[u]]++; }
inline void del(int u) { tot -= (cnt[col[u]] == 1), cnt[col[u]]--; }
void dfs(int u, int fath, bool keep)
{
 for (int v : g[u])
  if (v != hson[u] && v != fath) dfs(v, u, 0);
 if (hson[u]) dfs(hson[u], u, true);
 for (int v : g[u])
  if (v != hson[u] && v != fath)
   for (int i = dfn[v]; i < dfn[v] + siz[v]; ++i) add(w[i]);
 add(u), ans[u] = tot;
 if (!keep)
  for (int i = dfn[u]; i < dfn[u] + siz[u]; ++i) del(w[i]);
}
```

时间复杂度容易证明是 $O(n\log n)$ 的，但写丑了会变成 $O(n\log^2 n)$。

### 虚树（Virtual Tree）

一般用来解决多次询问树上点集的问题，其特点一般是关键点分布稀疏。对于指定点集，虚树是包含点集中点及其 LCA 的最小的树。

考虑构造。维护点集，先将关键点扔到一个点集里，按 dfn 排序，再把相邻的点的 lca 扔到这个点集里，再按 dfn 排序，对于相邻点 $x,y$，连接 $(\operatorname{lca}(x,y),y)$.这样做对的原因是显然的。

上面的构造方法简单但是常数巨大,有一种小常数的做法。我们用单调栈维护虚树上的一条链，栈顶为 dfn 最大的那个点，首先栈里放入 1，然后按 dfn 从小到大放入关键点。如果当前点和栈顶的 lca 不是栈顶，那么不断弹知道栈顶和当前点 lca 是栈顶然后扔进去。

这样虚树的点数就是 $O(k)$ 的，其中 $k$ 表示给定点集的大小。
