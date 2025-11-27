---
title: oi priblems
draft: true
date: 2025-08-08 19:11:21
tags: 做题
archive: true
---

我要写代码，我要贴代码。

# CNOI

## NOIP

包括 CSP-S.

### 2020

#### NOIP

最难评的一集。

---

barrack

首先缩边双得到一个图，考虑在图上 dp，设 $dp(i,j)$ 表示 $i$ 字数内建造了 $j$ 个军营的方案数，发现 $j$ 是不需要的，只需要记 $0/1$ 表示 $i$ 子树内有没有建造军营的方案数，转移是容易的，答案就是 $dp(u,1)$ 的和，但是你发现这样会算重，考虑给状态增加约束，设 $dp(i,0/1)$ 表示强制不选 $(i,fa)$ 的方案数就可以了。

```cpp
int n,m;
vector<int>G[N],T[N];
int dfn[N],low[N],idx,stk[N],Top,ins[N],bcc[N],bcnt,cnt[N],cedg[N];
inline void tarjan(int u,int fa)
{
 cerr<<u<<" "<<fa<<endl;
 dfn[u]=low[u]=++idx,stk[++Top]=u,ins[u]=true;
 for(int v:G[u])if(v!=fa)
 {
  if(!dfn[v])tarjan(v,u),low[u]=min(low[u],low[v]);
  else if(ins[v])low[u]=min(low[u],dfn[v]);
 }
 if(dfn[u]==low[u])
 {
  ++bcnt;int x;
  do{ins[x=stk[Top--]]=0,bcc[x]=bcnt,++cnt[bcnt];}while(x!=u);
 }
}
ll dp[N][2],_2[N],ans;
int E[N];
inline void dfs0(int u,int pa){E[u]=cedg[u];for(int v:T[u])if(v!=pa)dfs0(v,u),E[u]+=E[v]+1;}
inline void dfs(int u,int pa)
{
 for(int v:T[u])if(v!=pa)
 {
  dfs(v,u);
  dp[u][1]=A_(M_(dp[u][1],A_(dp[v][1],dp[v][0]*2)),M_(dp[u][0],dp[v][1]));
  _M(dp[u][0],dp[v][0],2ll);
 }
 if(u==1)_A(ans,dp[u][1]);
 else _A(ans,M_(dp[u][1],_2[E[1]-E[u]-1]));
}
inline void mian()
{
 read(n,m),_2[0]=1;fo(i,1,n+m+1)_2[i]=M_(_2[i-1],2ll);
 fo(i,1,m){int u,v;read(u,v);G[u].push_back(v),G[v].push_back(u);}
 tarjan(1,0);
 fo(u,1,n)for(int v:G[u])
 {
  if(bcc[u]!=bcc[v])T[bcc[u]].push_back(bcc[v]),T[bcc[v]].push_back(bcc[u]);
  else cedg[bcc[u]]++;
 }
 fo(i,1,bcnt)sort(T[i].begin(),T[i].end()),T[i].erase(unique(T[i].begin(),T[i].end()),T[i].end());
 fo(i,1,bcnt)cedg[i]/=2,dp[i][0]=_2[cedg[i]],dp[i][1]=A_(_2[cnt[i]+cedg[i]],mod-_2[cedg[i]]);
 dfs0(1,0),dfs(1,0);
 wr(ans);
}
```

### 2023

#### CSP

struct

直接模拟即可。花了我一坤小时。

https://pastebin.ubuntu.com/p/tYKPPPKsXQ/

### 2024

#### NOIP

assign

转化为不满足的情况。考虑什么样的 $\{a_n\},\{b_n\}$ 是不合适的，相当于是对于 $x_{c_i}=d_i,x_{c_{i+1}}=d_{i+1}$，一连串下来之后与 $x_{c_{i+1}}=d_{i+1}$ 矛盾，也就是说设 $x=c_i,y=c_{i+1}$，那么 $a_i=b_{i-1}$ 对所有 $x<i$ 均成立，并且 $b_{y-1}\not=d_{i+1}$，这样的的方案数是 $v^{len}-v^{len-1}$，$v^{2len}$ 减去之的结果就是这一部分不满足条件的情况，把每个部分的情况乘起来就行了。

```cpp
#define int long long
const int N=1e5+5,mod=1e9+7;
int n,m,v;pair<int,int>c[N];
inline int qpow(int a,int b){int res=1;while(b){if(b&1)res=res*a%mod;a=a*a%mod,b>>=1;}return res;}
inline void solve()
{
 read(n,m,v);fo(i,1,m)read(c[i].fi,c[i].se);sort(c+1,c+1+m);int ans=1;
 auto calc=[&](int x)->int{return (qpow(v,2*x)-qpow(v,x)+qpow(v x-1)+mod)%mod;};
 fo(i,2,m)
 {
  if(c[i].fi==c[i-1].fi){if(c[i].se!=c[i-1].se)return puts("0") void();continue;}
  ans=ans*calc(c[i].fi-c[i-1].fi)%mod;
 } 
 ans=ans*qpow(v,2*c[1].fi-2)%mod*qpow(v,2*n-2*c[m].fi)%mod;
 wr(ans),putchar('\n');
}
```

---

traverse

考虑 $k=1$ 的情况，答案就是 $\prod(d_i-1)!$，$d_i$ 表示点 $i$ 的度数。

考虑更多的 $k$，考虑容斥，不管重复情况的话答案就是 $k\prod(d_i-1)!$，考虑什么情况下会生成相同的树。对于一个已经生成的树，考虑其可以被那些遍历起始边生成出来。手摸猜结论是可能的遍历起始边一定在从一个叶子节点到另一个叶子节点的路径上，证明只用考虑一个点周围的平凡的边，它们构成的树一定是一条链，而只有链的两个端点可以确定根的方向。于是可以从链的角度解决问题。假设找出了这一条链，其贡献便是 $\prod_{i\in V}(d_i-1)!\times \prod_{i\in L}(d_i-1)^{-1}$。

这样问题就被转化成对于一棵树，边有 $0/1$ 权值，点权设为 $(d_v-1)^{-1}$，求所有叶子到叶子的链，满足这条链上有至少一条 1 边，求点权的乘积。取一个非叶子节点做根，记 $dp(u)$ 表示点 $u$ 的子树内恰好存在一个链的端点的贡献和。我使用这个方法写死了/tuu

换方法。记 $dp(u,0/1)$ 表示 $u$ 的字数内叶子到他有 1 边/没有 1 边的贡献次数，乘上 $val(u)$ 即可统计答案。

```cpp
#define int long long
const int N=2e5+5;
int n,k,black[N],Inv[N],Pre[N],ans,dp[N][2];
int fi[N],se[N];vector<pair<int,int>>g[N];
void dfs(int u,int fa)
{
 dp[u][0]=dp[u][1]=0;int val=Inv[g[u].size()-1],cnt=0;
 for(auto [v,flg]:g[u])if(v!=fa)
 {
  dfs(v,u);if(flg)(dp[v][1]+=dp[v][0])%=mod,dp[v][0]=0;
  (cnt+=dp[v][1]*(dp[u][0]+dp[u][1])%mod+dp[v][0]*dp[u][1]%mod)%=mod;
  (dp[u][0]+=dp[v][0])%=mod;(dp[u][1]+=dp[v][1])%=mod;
 }
 (ans+=cnt*val%mod)%=mod;if(g[u].size()==1)dp[u][0]++;
 (dp[u][0]*=val)%=mod;(dp[u][1]*=val)%=mod;
}
inline void add(int u,int v,int col){g[u].pb({v,col});g[v].pb({u,col});}
inline void solve()
{
 read(n,k);
	fo(i,1,n-1)read(fi[i]),read(se[i]),black[i]=0;
 fo(i,1,k){int x;read(x);black[x]=1;}
	if(n==2){puts("1");return;}
 fo(i,1,n-1)add(fi[i],se[i],black[i]);
 int rot=0;fo(i,1,n)if(g[i].size()>1)rot=i;ans=0;
 dfs(rot,0);fo(i,1,n)fo(j,1,g[i].size()-1)ans=ans*j%mod;
	wr(ans),pn;fo(i,1,n)g[i].clear();
}
signed main()
{
 auto qpow=[&](int a,int b){int res=1;while(b){if(b&1)res=res*a%mod;a=a*a%mod,b>>=1;}return res;};
 Inv[0]=Inv[1]=1;fo(i,2,N-1)Inv[i]=Inv[mod%i]*(mod-mod/i)%mod;
 int __c,T;read(__c,T);while(T--){solve();}
 return 0;
}
```

## 联合省选

### 2025

追忆

给的是有向图，有向图可达性是 NP 的，只能使用 bitset 做，这样分析复杂度可以除一个 $\omega$。

对于 a 或 b 没有修改的情况我们都有 poly n 的做法，拼起来的时候考虑分块平衡复杂度。对 a 和 b 分别值域分块，整块维护编号集合的 bitset，散块直接维护对应编号，对于查询，假设可达点集为 S，那么找到 a 值域在 [l,r] 中的集合和 S 与起来，然后在 b 的值域中找到最后一个与 S 有交的块，在块中暴力就可以。

计算复杂度，设块长为 B，分析可得 $B=\frac{n}{\omega^{1/2}}$ 时有最优时间复杂度 $O(\frac{n^2}{\omega^{1/2}})$。

抄了个手写 bitset，$\omega=64$ 如下。

```cpp
constexpr int SIZ = 1570;
struct my_bitset
{
 u64 a[SIZ];
 inline void reset() { mem(a, 0); }
 inline void set(int x) { a[x >> 6] |= 1ull << (x & 63); }
 inline void flip(int x) { a[x >> 6] ^= 1ull << (x & 63); }
 inline void operator&=(const my_bitset &b) { fo(i, 0, SIZ - 1) a[i] &= b.a[i]; }
 inline void operator|=(const my_bitset &b) { fo(i, 0, SIZ - 1) a[i] |= b.a[i]; }
 inline void operator^=(const my_bitset &b) { fo(i, 0, SIZ - 1) a[i] ^= b.a[i]; }
 inline int val(int x) { return a[x >> 6] >> (x & 63) & 1; }
 inline bool not_empty() { fo(i, 0, SIZ - 1) if (a[i]) return true; return false; }
};
```

发现这并不好用，所以我这道题改用普通 bitset。

```cpp
const int N=1e5+5;
int n,m,q,a[N],b[N],ia[N],ib[N],st[2005],ed[2005],cnt,pos[N],B,ind[N];
vector<int> g[N];
bitset<N> G[N],va[2005],vb[2005],S;
inline void solve()
{
 read(n,m,q);B=max(1,n/8);cnt=n/B;if(n%B!=0)++cnt;
 fo(i,1,n){g[i].clear();ind[i]=0;G[i].reset();G[i][i]=1;}
 while(m--){int u,v;read(u,v);g[v].pb(u);ind[u]++;}
 fo(i,1,n)read(a[i]),ia[a[i]]=i;fo(i,1,n)read(b[i]),ib[b[i]]=i;
 fo(i,1,cnt){st[i]=ed[i-1]+1,ed[i]=min(n,st[i]+B-1);va[i]=0,vb[i]=0;fo(j,st[i],ed[i])pos[j]=i,va[i][ia[j]]=1,vb[i][ib[j]]=1;}
 auto topo=[&]()->void
 {
  queue<int>q;fo(i,1,n)if(!ind[i])q.push(i);
  while(!q.empty()){int u=q.front();q.pop();for(int v:g[u]){ind[v]--,G[v]|=G[u];if(!ind[v])q.push(v);}}
 };
 topo();int op,x,y,z;
 while(q--)
 {
  read(op,x,y);
  if(op==1){va[pos[a[x]]][x]=0,va[pos[a[y]]][y]=0;swap(a[x],a[y]);ia[a[x]]=x,ia[a[y]]=y;va[pos[a[x]]][x]=1,va[pos[a[y]]][y]=1;}
  else if(op==2){vb[pos[b[x]]][x]=0,vb[pos[b[y]]][y]=0;swap(b[x],b[y]);ib[b[x]]=x,ib[b[y]]=y;vb[pos[b[x]]][x]=1,vb[pos[b[y]]][y]=1;}
  else
  {
   auto update=[&](int l,int r)->void
   {
    bitset<N> s;s.reset();
    if(pos[l]==pos[r])fo(i,l,r)s[ia[i]]=1;
    else{fo(i,l,ed[pos[l]])s[ia[i]]=1;fo(i,pos[l]+1,pos[r]-1)s|=va[i];fo(i,st[pos[r]],r)s[ia[i]]=1;}
    S&=s;
   };
   auto calc=[&]()->int
   {
    int now;for(now=cnt;now>=1;--now)if((S&vb[now]).count())break;
    Fo(i,ed[now],st[now])if(S[ib[i]]==1)return i;return 0;
   };
   read(z);S=G[x];update(y,z);wr(calc()),pn;
  }
 }
}
```

很可惜的是这个做法在 QOJ 上只获得了 48pts 的坏成绩。

# POI

可以从 [szkopul](https://szkopul.edu.pl/) 上找，qoj 也很全但是没有 spj，更推荐直接从洛谷上搜。

## 2001

绿色游戏

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

## 2004

Bra

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

## 2005

PUN-Points

把重心平移到一块，最远点放缩到一起向两个方向判断就行。困了写不动了。不行得写。现在先不写，实在困。

---

SAM-Toy Cars

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

---

SKO-Knights

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
