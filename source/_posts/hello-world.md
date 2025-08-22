---
title: Hello World
description: Hello,World!
sticky: 2
---

大家好！欢迎来看我的博客。

想认识我请移步 About 界面，想看所有博客请移步 Archive 界面，谢谢大家！

---

一些记号：

- $\{a_n\}$ 表示长度为 $n$ 的序列 $a$。
- $(l\dots r)$ 表示 $l$ 到 $r$ 的开区间，这是为了与平面直角坐标系上点的表示做区分。

---

关于代码：

- 缺省源（太长了所以贴代码就不放了，放在这里也算是一个约定）

```cpp
#include <bits/stdc++.h>
#define INF 0x3f3f3f3f3f3f3f3f
#define inf 0x3f3f3f3f
#define pn putchar(10)
#define fi first
#define se second
#define pp putchar(' ')
#define pii pair<int,int>
#define pdi pair<ll,ll>
#define mem(aa,bb) memset(aa,bb,sizeof(aa))
#define fo(i,a,b) for(int i=(a);i<=(b);++i)
#define Fo(i,a,b) for(int i=(a);i>=(b);--i)
#define pb push_back
#define reg register
#define eb emplace_back
#define bct __builtin_popcount
#define mk make_pair
#define IT iterator
#define all(x) x.begin(),x.end()
#define lbd lower_bound
#define ubd upper_bound
#define lowbit(x) (x&(-x))
#define gb(x,i) ((x>>i)&1)
#define IOS ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)
// #pragma GCC optimize(2)
using namespace std;

typedef int64_t i64;
typedef uint32_t u32;
typedef uint64_t u64;
typedef __int128_t i128;
typedef __uint128_t u128;
typedef double db;

// typedef int ll;
typedef long long ll;
// typedef __int128 ll;
const ll mod=1e9+7;

// #define getchar getchar_unlocked
// #define putchar putchar_unlocked

struct FastMod
{
 using ull=unsigned long long;
 using L=__int128;	ull b,m;
 FastMod(ull b):b(b),m(ull((L(1)<<64)/b)){}
 ull reduce(ull a) const{ull q=(ull)((L(m)*a)>>64),r=a-q*b;return r>=b?r-b:r;}
} Mod(2);
template <typename T>using _is_integral=typename enable_if<is_integral<T>::value>::type;
template <typename T,typename =_is_integral<T>>T operator%(T x, const FastMod &Mod){return Mod.reduce(x);}
template <typename T,typename =_is_integral<T>>T &operator%=(T &x,const FastMod &Mod){return x=Mod.reduce(x);}

template <class T> inline bool ckmn(T &a,T b){return a>b?a=b,1:0;}
template <class T> inline bool ckmx(T &a,T b){return a<b?a=b,1:0;}
template <class T> inline void _A(T &x,T y){x=(x+y+mod)%Mod;}
template <class T> inline void _M(T &x,T y){x=1ll*x*y%Mod;}
template <class T> inline ll A_(T x,T y){return (x+y+mod)%Mod;}
template <class T> inline ll M_(T x,T y){return 1ll*x*y%Mod;}
template <class T> inline void read(T &s)
{
 s=0;int f=1;char c=getchar();while(!isdigit(c)){if(c=='-')f=-1;c=getchar();}
 while(isdigit(c))s=(s<<3)+(s<<1)+(c^48),c=getchar();s*=f;return;
}
template <class T> inline void wr(T x)
{
 if(x<0)putchar('-'),x=-x;int buf[21],top=0;while(x)buf[++top]=x%10,x/=10;
 if(!top)buf[++top]=0;while(top)putchar(buf[top--]^'0');return;
}
template <class T, class ...A> inline void read(T &x,A &...a){read(x);read(a...);}
template <class T, class ...A> inline bool ckmn(T &x,T y,A ...a){return ckmn(x,y)|ckmn(x,a...);}
template <class T, class ...A> inline bool ckmx(T &x,T y,A ...a){return ckmx(x,y)|ckmx(x,a...);}
template <class T, class ...A> inline void _A(T &x,T y,A ...a){_A(x,y),_A(x,a...);}
template <class T, class ...A> inline void _M(T &x,T y,A ...a){_M(x,y),_M(x,a...);}
template <class T, class ...A> inline ll A_(T x,T y,A ...a){return A_(A_(x,y),a...);}
template <class T, class ...A> inline ll M_(T x,T y,A ...a){return M_(M_(x,y),a...);}

const ll NN=1e6+5;
ll Pre[NN],Inv[NN];
inline ll qpow(ll a,ll b){return !b?1ll:qpow(1ll*a*a%Mod,b>>1ll)*((b&1ll)?a:1ll)%Mod;}
inline ll C(ll n,ll m){return (n<m||n<0||m<0)?0ll:Pre[n]*Inv[m]%Mod*Inv[n-m]%Mod;}
inline void InitC(ll n){Pre[0]=1;fo(i,1,n) Pre[i]=M_(Pre[i-1],ll(i));Inv[n]=qpow(Pre[n],mod-2);Fo(i,n-1,1)Inv[i]=M_(Inv[i+1],(ll)i+1);Inv[0]=1;}

signed main()
{
 return 0;
}

```

- 关于码风：

1. 大括号换行，tabsize=1。
2. 一般不格式化，有的时候为了好看会格式化一下，格式化选项 Base on LLVM，有所改动。
