---
title: sqrt-tech
tags: ds
---

先说一下这些题是怎么想到分块的。

首先，这种查询区间问题的东西，肯定得找个数据结构维护。

然后这三个题维护的信息都不满足幺半群性质，更别提什么双半群结构，所以只能用分块。

#### 强制在线求排列区间逆序对

贡献了五页提交哈哈哈哈哈哈。再也不 `#define int long long` 了。

对每个块内排序，预处理整块与整块之间的答案 $F_{i,j}$，预处理 `[st[pos[i]],i]` 与 `[i,ed[pos[i]]]` 的贡献，记为 $pre_i$ 与 $suf_i$。

对于同一块内的情况，答案是 $pre_{r}-pre_{l-1}$，再减去多算的横跨 $[L,l-1]$ 和 $[l,r]$ 之间的情况数。

对于不在同一块内的情况，整块与整块之间的答案已经求出，两个散块对答案的贡献是 $suf_l+pre_r$ 加上两端区间归并排序起来的答案。

考虑如何 $O(\sqrt n)$ 计算散块与整块对答案的贡献。

我们希望求出单点与块的贡献之和，直接做时空超了，考虑在这两维中挑选一位做前缀和，对于块做前缀和效果甚微，于是对单点做前缀和。

预处理 $c_{i,j}$ 表示前 $i$ 个数与第 $j$ 块产生的贡献，于是散块与整块对答案的贡献就可以用前缀和的思想相减从而求出答案。

```cpp
#include<cstdio>
#include<cmath>
#include<algorithm>
inline int rd()
{
	int x=0;
	char c=getchar_unlocked();
	while(c<'0'||c>'9')c=getchar_unlocked();
	while(c>='0'&&c<='9')x=(x<<1)+(x<<3)+(c^48),c=getchar_unlocked();
	return x;
}
inline long long read()
{
	long long x=0;
	char c=getchar_unlocked();
	while(c<'0'||c>'9')c=getchar_unlocked();
	while(c>='0'&&c<='9')x=(x<<1)+(x<<3)+(c^48),c=getchar_unlocked();
	return x;
}
void wt(int x)
{
	if(x>9)wt(x/10);
	putchar_unlocked(x%10+'0');
}
void pr(int x,char c){wt(x),putchar_unlocked(c);}
const int N=1e5+5,T=320;
inline int merge(int *a,int *b,int n,int m)
{
	int ia=1,ib=1,ret=0;
	while(ia<=n&&ib<=m)
	{
		if(a[ia]<b[ib])++ia;
		else ret+=n-ia+1,++ib;
	}
	return ret;
}
struct BIT
{
 int b[N];
 inline void upd(int i,int x){for(;i<N;i+=i&-i)b[i]+=x;}
 inline int qry(int i){int ret=0;for(;i;i^=i&-i)ret+=b[i];return ret;}
}BiT;
int n,m,a[N],st[N],ed[N],pos[N];
int pre[N],suf[N],c[N][T];
long long f[T][T],ans;
struct Pair{
	int first,second;
	bool operator <(const Pair &b)const{return first<b.first;}
}t[N],b[N];
int tx[N],ty[N],lx,ly;
int siz=317;
int main()
{
	n=rd(),m=rd();
	for(int i=1;i<=n;i++)
		a[i]=rd(),t[i].first=b[i].first=a[i],b[i].second=t[i].second=i;
	std::sort(t+1,t+1+n);
	int B=(n-1)/siz+1;
	for(int i=1;i<=B;++i)st[i]=ed[i-1]+1,ed[i]=i*siz;
	ed[B]=n;
	for(int i=1;i<=B;i++)
	{
		for(int j=st[i];j<=ed[i];j++)pos[j]=i;
		std::sort(b+st[i],b+ed[i]+1);
		int tmp=0;
		for(int j=st[i];j<=ed[i];j++)
		{
			BiT.upd(a[j],1);
			tmp+=BiT.qry(n)-BiT.qry(a[j]);
			pre[j]=tmp;
		}
		f[i][i]=tmp;
		for(int j=st[i];j<=ed[i];j++)
		{
			suf[j]=tmp;
			BiT.upd(a[j],-1);
			tmp-=BiT.qry(a[j]-1);
		}
	}
	for(int j=1;j<=B;j++)
	{
		for(int i=1,k=st[j];i<=n;i++)
		{
			const int id=t[i].second;
			while(k<=ed[j]&&b[k].first<t[i].first)k++;
			if(id<st[j])c[id][j]=k-st[j];
			else if(id>ed[j])
			{
				c[id][j]=ed[j]-k+1;
				if(t[i].first==b[k].first&&k<=ed[j])c[id][j]--;
			}
		}
	}
	for(int i=1;i<=n;i++)for(int j=1;j<=B;j++)c[i][j]+=c[i-1][j];
	for(int len=2;len<=B;len++)
	{
		for(int i=1;i+len-1<=B;i++)
		{
			const int j=i+len-1;
			f[i][j]=f[i+1][j]+f[i][j-1]-f[i+1][j-1]+c[ed[i]][j]-c[st[i]-1][j];
		}
	}
	while(m--)
	{
		long long ll=read(),rr=read();
		const int l=ll^ans,r=rr^ans;
		lx=ly=0;
		if(pos[l]==pos[r])
		{
			ans=pre[r];
			if(l!=st[pos[l]])ans-=pre[l-1];
			for(int i=st[pos[l]];i<=ed[pos[l]];i++)
			{
				if(b[i].second<l)tx[++lx]=b[i].first;
				else if(b[i].second<=r) ty[++ly]=b[i].first;
			}
			ans-=merge(tx,ty,lx,ly);
		}
		else
		{
			int p=pos[l],q=pos[r];
			ans=f[p+1][q-1]+pre[r]+suf[l];
			for(int i=p+1;i<q;i++)ans+=c[ed[p]][i]-c[l-1][i]+c[r][i]-c[st[q]-1][i];
			for(int i=st[p];i<=ed[p];i++)if(b[i].second>=l)tx[++lx]=b[i].first;
			for(int i=st[q];i<=ed[q];i++)if(b[i].second<=r)ty[++ly]=b[i].first;
			ans+=merge(tx,ty,lx,ly);
		}
		pr(ans,'\n');
	}
	return 0;
}
```

#### 不强制在线求区间逆序对

考虑暴力莫队就是在值域上维护值的出现次数，每次扫端点的时候更新，时间复杂度 $O(n\sqrt m \log n)$，并不优。

考虑优化，假设当前做了一次指针右移，那么相当于查询 $[l\dots r]$ 内 $\geq a_r$ 的数量，那么差分一下，问题转化为 $[1\dots r]$ 内 $\geq a_r$ 的数量减去 $[1\dots l)$ 内 $\geq a_r$ 的数量。前面一部分易于在 $O(n\log n)$ 的时间内处理，后面一部分的形式容易想到再次莫队。考虑在 $l$ 上把所有 $r$ 记下来，然后扫 $l$，只需要做到 $O(\sqrt n)$ 修改，$O(1)$ 查询就可以满足 $O(n\sqrt m)$ 的数据量了，发现值域分块正好可以。我们维护值域内的前缀和与值域快之间的前缀和，就可以满足需求了，时间复杂度 $O(n\sqrt m+n\sqrt n)$，但是记下来 $O(n\sqrt m)$ 的修改还是太超标了，但是注意到满足条件的右端点是连续的，所以完全可以只存两端点，这样空间复杂度就是 $O(n)$ 的了。

#### 强制在线求区间众数的出现次数

一个类似的题目是[[Violet]蒲公英](https://www.luogu.com.cn/problem/P4168)，在这个题中，我们用 `vector` 对每个数维护出现位置的集合，然后分块做。这道题我们思路类似，提前预处理块与块之间的答案，然后从内往外扫散块，如果当前位置的值可以使 $ans\leftarrow ans+1$,那么更新答案，否则跳过。

```cpp
#include <algorithm>
#include <cmath>
#include <cstdio>
#include <cstring>
#include <iostream>
#include <vector>
using namespace std;
const int T = 710, N = 500005;
int n, m, a[N], p[N], mn, b[N], lastans;
vector<int> pos[N];
int L[T], R[T], bel[N], c[T][T], cnt[N];
int main()
{
 cin >> n >> m;
 int t = sqrt(n);
 for (int i = 1; i <= n; i++) cin >> a[i], b[i] = a[i];
 sort(b + 1, b + 1 + n);
 int w = unique(b + 1, b + 1 + n) - (b + 1);
 for (int i = 1; i <= n; i++)
 {
  a[i] = lower_bound(b + 1, b + 1 + w, a[i]) - b;
  pos[a[i]].push_back(i);
  p[i] = pos[a[i]].size() - 1;
 }
 for (int i = 1; i <= t; i++) L[i] = (i - 1) * t + 1, R[i] = i * t;
 R[t] = n;
 for (int i = 1; i <= t; i++)
  for (int j = L[i]; j <= R[i]; j++) bel[j] = i;
 for (int i = 1; i <= t; i++)
 {
  memset(cnt, 0, sizeof(cnt));
  int mx = 0;
  for (int j = i; j <= t; j++)
  {
   for (int k = L[j]; k <= R[j]; k++) cnt[a[k]]++, mx = max(mx, cnt[a[k]]);
   c[i][j] = mx;
  }
 }
 memset(cnt, 0, sizeof(cnt));
 while (m--)
 {
  int l, r;
  cin >> l >> r;
  l ^= lastans, r ^= lastans;
  if (bel[l] == bel[r])
  {
   int res = 0;
   for (int i = l; i <= r; i++) res = max(res, ++cnt[a[i]]);
   for (int i = l; i <= r; i++) cnt[a[i]] = 0;
   cout << res << "\n";
   lastans = res;
  }
  else
  {
   int ans = c[bel[l] + 1][bel[r] - 1];
   for (int i = l; i <= R[bel[l]]; i++)
    while (p[i] + ans < (signed)pos[a[i]].size() && pos[a[i]][p[i] + ans] <= r) ans++;
   for (int i = L[bel[r]]; i <= r; i++)
    while (p[i] - ans >= 0 && pos[a[i]][p[i] - ans] >= l) ans++;
   cout << ans << "\n";
   lastans = ans;
  }
 }
 return 0;
}
```
