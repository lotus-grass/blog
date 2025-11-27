---
title: kosaraju
tags: graph
---

Kosaraju 算法最早在 1978 年由 S. Rao Kosaraju 在一篇未发表的论文上提出，但 Micha Sharir 最早发表了它。感觉上要比 tarjan 求 SCC 要自然的多，但是会比 tarjan 慢。

建反图，跑两次 dfs。第一次 dfs 对节点进行后序遍历，也就是在回溯之前给节点编号。第二次 dfs 对于反图，从没被跑到过的编号最大的节点开始跑，跑到的顶点集合就是一个 SCC。

考虑对于一个点，编号比它小的也就是它在原图上能跑到的，如果在反图上还能跑到这些点，说明这些点和这个点在同一个强连通分量中。

```cpp
vector<int> G[N], rG[N], s;
bool vis[N];
int sccno[N], scnt;
void dfs1(int u)
{
 vis[u] = true;
 for (int v : G[u])
  if (!vis[v]) dfs1(v);
 s.emplace_back(u);
}
void dfs2(int u, int val)
{
 sccno[u] = val;
 for (int v : rG[N])
  if (!sccno[v]) dfs2(v, val);
}
void kosaraju()
{
 scnt = 0;
 for (int i = 1; i <= n; i++)
  if (!vis[i]) dfs1(i);
 for (int i = n - 1; i >= 0; i--)
  if (!sccno[s[i]]) dfs2(s[i], ++scnt);
}
```
