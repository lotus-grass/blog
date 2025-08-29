---
title: oi priblems
draft: true
date: 2025-08-08 19:11:21
tags: 做题
archive: true
---

我要写代码，我要贴代码。

## POI

可以从 [szkopul](https://szkopul.edu.pl/) 上找，qoj 也很全但是没有 spj，更推荐直接从洛谷上搜。

### 2001

#### 绿色游戏

对于 Ann 的点，如果能称为必胜点那么其出边中存在一个绿点；对于 Bily 的点，如果能成为必胜点则其后继点必然全部是绿点；绿点不一定是必胜点。我们不妨假设绿点都是必胜点，从绿点开始跑来 check 上面的条件，如果有绿点不符合那就把这些绿点扔了继续跑，可以说明最多跑 $a+b$ 遍，对于本题是可以接受的。实现的时候可以从后继点到当前点连边，每次 check 可以执行一个类似 bfs 的框架。代码：

```cpp
const int N = 6005;
int n, m, col[N], siz[N], tmp[N], vis[N];
vector<int> g[N];
queue<int> q;
inline bool work()
{
 memcpy(tmp, siz, sizeof(tmp));
 memcpy(vis, col, sizeof(vis));
 fo(i, 1, n + m) if (vis[i]) q.push(i), vis[i] = true;
 while (!q.empty())
 {
  int u = q.front();
  q.pop();
  for (int v : g[u])
  {
   if (vis[v]) continue;
   --tmp[v];
   if (!tmp[v] || v <= n) vis[v] = 1, q.push(v);
  }
 }
 memcpy(tmp, siz, sizeof(tmp));
 fo(i, 1, n + m) if (!vis[i]) q.push(i);
 while (!q.empty())
 {
  int u = q.front();
  q.pop();
  for (int v : g[u])
   if (vis[v])
   {
    --tmp[v];
    if (!tmp[v] || v > n) vis[v] = 0, q.push(v);
   }
 }
 bool flg = false;
 for (int i = 1; i <= n + m; i++)
  if (!vis[i] && col[i]) col[i] = 0, flg = true;
 return flg;
}
signed main()
{
 read(n, m);
 fo(i, 1, n + m)
 {
  int v;
  read(col[i], siz[i]);
  fo(j, 1, siz[i]) read(v), g[v].pb(i);
 }
 while (work());
 int ans = 0;
 fo(i, 1, n + m) ans += vis[i];
 wr(ans), pr;
 fo(i, 1, n + m) if (vis[i]) wr(i), pr;
}
```

### 2004

#### Bra

神秘。考虑一个点点值的范围，两端不同就是未知了。贪心地假设每个点点权都是 1 然后如果有点不满足约束就把它改合法这样就能求上界，求下界同理。

```cpp
const int N = 1e4 + 5, M = 2e5 + 5;
int n, ind[N], x;
vector<int> g[N];
queue<int> q;
int c[N][3], v0[N], v1[N];
signed main()
{
 read(n);
 fo(i, 2, n - 1)
 {
  read(ind[i]);
  fo(j, 1, ind[i]) read(x), g[x].pb(i);
 }
 auto calc = [&](int u) -> int
 {
  if (c[u][0] > c[u][1]) return 0;
  if (c[u][1] > c[u][0]) return 1;
  return 2;
 };
 fo(i, 0, n - 1) c[i][0] = ind[i], c[i][1] = 0;
 v0[1] = 1, q.push(1);
 for (int v : g[1]) c[v][0]--, c[v][1]++;
 while (!q.empty())
 {
  int u = q.front();
  q.pop();
  for (int v : g[u])
  {
   int t = calc(v);
   if (v0[v] != t)
   {
    for (int w : g[v]) --c[w][v0[v]], ++c[w][t];
    q.push(v), v0[v] = t;
   }
  }
 }
 fo(i, 0, n - 1) c[i][0] = c[i][2] = 0, c[i][1] = ind[i];
 fill(v1 + 1, v1 + n, 1), v1[0] = 0, q.push(0);
 for (int v : g[0]) c[v][0]++, c[v][1]--;
 while (!q.empty())
 {
  int u = q.front(), t = calc(u);
  q.pop();
  for (int v : g[u])
  {
   int t = calc(v);
   if (v1[v] != t)
   {
    for (int w : g[v]) --c[w][v1[v]], ++c[w][t];
    q.push(v), v1[v] = t;
   }
  }
 }
 fo(i, 0, n - 1)
 {
  if (v1[i] != v0[i]) puts("?");
  else
  {
   if (v0[i] == 2) puts("1/2");
   else wr(v0[i]), pn;
  }
 }
}
```

### 2005

#### PUN-Points

把重心平移到一块，最远点放缩到一起向两个方向判断就行。困了写不动了。不行得写。现在先不写，实在困。

#### SAM-Toy Cars

直接猜每一次撤销 $nxt$ 最大的那一个。

```cpp
const int N = 1e5 + 5, M = 5e5 + 5;
int n, k, p, a[M], nxt[M], ans, pre[N];
struct cmp
{
 bool operator()(const int &x, const int &y) { return nxt[x] < nxt[y]; }
};
priority_queue<int, vector<int>, cmp> q;
bool inq[N];
signed main()
{
 read(n, k, p);
 fo(i, 1, p) read(a[i]);
 Fo(i, p, 1)
 {
  if (pre[a[i]] == 0) nxt[i] = M;
  else nxt[i] = pre[a[i]];
  pre[a[i]] = i;
 }
 fo(i, 1, p)
 {
  if (inq[a[i]]) q.push(i), k++;
  else
  {
   if (q.size() == k) inq[a[q.top()]] = false, q.pop();
   ans++, q.push(i), inq[a[i]] = true;
  }
 }
 return wr(ans), 0;
}
```

#### SKO-Knights

记 $\operatorname{span}(S)$ 表示 $S$ 内集合 **整系数**线性组合得到的线性空间（张成），即

$$\operatorname{span}(S)=\{\sum_{a_i\in S}c_ia_i:c_i\in \mathbb{Z}\}$$

考虑合并三个向量 $a,b,c$，发现倍加操作对张成没有影响，将 $a,b$ 在 $x$ 维上辗转相减，$a,c$ 在 $x$ 维上辗转相减这样 $b,c$ 就共线，取 $x=x_b,y=\gcd(b_y,c_y)$ 即可三变 2，$n$ 变 2 也就可以 $O(n\log V)$ 做了。

```cpp
const int N = 105;
int n, a[N], b[N];
signed main()
{
 cin >> n >> a[1] >> b[1];
 for (int i = 2; i <= n; ++i)
 {
  cin >> a[i] >> b[i];
  while (a[i])
  {
   if (abs(a[1]) < abs(a[i])) swap(a[1], a[i]), swap(b[1], b[i]);
   else b[1] -= b[i] * (a[1] / a[i]), a[1] %= a[i];
  }
  if (i > 2 && b[i]) b[2] = __gcd(b[2], b[i]), b[1] %= b[2];
 }
 cout << a[1] << ' ' << b[1] << '\n' << a[2] << ' ' << b[2] << '\n';
}
```
