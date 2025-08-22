---
title: Number Theory
date: 2025-08-21 17:08:35
tags: math
description: /se
---

## Definations

- 剩余类：记膜 $m$ 为 $r$ 的自然数组成的集合 $k_r$ 为膜 $m$ 的一个剩余类，任意 $a\bmod m=r$ 构成 $k_r$ 的一个代表元。
- 完系：每个 $k_r$ 中取一个代表元构成膜 $m$ 的一个完系。
- 缩系/简化剩余系：对每个与 $m$ 互质的 r，再 $k_r$ 中任取一个代表元构成膜 m 的一个缩系，若 $\gcd(x,m)=1$ 则将缩系中每个数都乘 x 后仍构成膜 m 的缩系。

## Theorems

### Bézout's identity

> $ax+by=c$ 有整数解的充要条件是 $\gcd(a,b)|c$。

$\text{proof.}$

可以利用无穷递降法证明之。

### Legendre theorem

> 记 $v_p(x)$ 表示 $x$ 的质因数分解中 $p$ 的系数，则 $v_p(n!)=\sum_{i=1}\lfloor\frac{n}{p^i}\rfloor=\frac{n-S_p(n)}{p-1}$，其中 $S_p(n)$ 表示 $n$ 在 $p$ 进制下的数位和。

$\text{proof.}$

设 $1,2,\dots,n$ 中 $p$ 的指数为 $r$ 的有 $n_r$ 个，那么

$$
v_p(n!)=n_1+2n_2+3n_3+\dots=\sum_{r\geq 1}rn_r
\\=(n_1+n_2+\dots)+(n_2+n_3+\dots)+(n_3+\dots)=N_1+N_2+\dots
$$

其中 $N_k$ 恰好是 $1,2,\dots,n$ 中能被 $p^k$ 除尽的数的个数，即 $N_k=\lfloor\frac{n}{p^k}\rfloor$。$\text{Q.E.D.}$

#### Kummer's theorem

> $v_p(\binom{n}{m})=\frac{S_p(m)+S_p(n-m)-S_p(n)}{p-1}$

可以用 Legendre theorem 证明之。

### Wilson's theorem

> $(p-1)!\equiv -1 \pmod p$，等价于 $p$ 是素数。

$\text{proof.}$

原式等价于方程 $px+(p-1)!y=-1$，方程显然有解，且若 $p$ 不是质数则方程无解。$\text{Q.E.D.}$

### Fermat's little theorem

> 若 $p$ 为素数，$\gcd(a,p)=1$，则 $a^{p-1}\equiv1\pmod p$.
>
> 另外一种更常用的表示是 $a^{p-2}\equiv a^{-1}\pmod p$.

$\text{proof.}$

归。可知 $1^p\equiv1\pmod p$ 显然成立，若 $a^p\equiv a\pmod p$ 成立，则

$$
(a+1)^p=\sum_{i=0}^p \dbinom{p}{p-i} a^i
$$

由于 $\dbinom{p}{k}=\frac{p(p-1)(p-2)\dots(p-k+1)}{k!}$，于是 $\forall k\in [1\dots n),\dbinom{p}{k}\equiv 0\pmod p$，于是 $(a+1)^p\equiv a^p+1\equiv a+1$，由归纳定理，可知所证成立。$\text{Q.E.D.}$

### Euler's theorem

> 若 $\gcd(a,m)=1$，则 $a^{\varphi(m)}\equiv 1\pmod m$.

$\text{proof.}$

设 $r_1,r_2,\dots,r_{\varphi(m)}$ 是 $m$ 的一个简化剩余系，那么 $ar_1,ar_2,\dots,ar_{\varphi(m)}$ 也是 $m$ 的一个简化剩余系，于是

$$\prod_{i=1}^{\varphi(m)} r_i\equiv \prod_{i=1}^{\varphi(m)}ar_i\pmod m$$

$\prod_{i=1}^{\varphi(m)} r_i$ 可约去，于是命题得证。$\square$

当 $m$ 为质数时，$\varphi(m)=m-1$，于是得到 Fermat's little theorem。

#### extend Euler's theorem

> $$
> a^b\equiv
> \begin{cases}
> a^{b\bmod \varphi(m)}& \gcd(a,m)=1
> \\a^b& \gcd(a,m)\not =1,b<\varphi(m)
> \\a^{(b\bmod \varphi(m))+\varphi(m)} & \text{otherwise}
> \end{cases}
> \pmod m
> $$

### Chinese Remainder Theorem,CRT

> 一元线性同余方程组
>
> $$
> \begin{cases}
> x\equiv a_1\pmod{m_1}\\
> x\equiv a_2\pmod{m_2}\\
> \dots\\
> x\equiv a_n\pmod{m_n}
> \end{cases}
> $$
>
> 有解，并且解可以被构造。其中 $n$ 两两互质。
>
> 构造方式是:
>
> 记 $M=\prod_{i=1}^n m_i$，以及 $M_i=M/m_i$，设 $t_i$ 使得 $t_iM_i\equiv 1\pmod{m_i}$。于是通解形式为
>
> $$x_0=\sum_{i=1}^na_it_iM_i$$
>
> 所有解是 $\{kM+x_0;k\in \mathbb Z\}$。

这个东西很漂亮，难的是把构造想出来。

#### exCRT

如果 $m_i$ 并不一一互质，exCRT 告诉我们这种情况下仍然有解且给出了构造方式。

我们发现所有闪光的点子在这里似乎都失效了，于是考虑朴素一点的方法。一个同余方程我们会解，于是考虑把多个同余方程合并成一个。先考虑两个。

$$
\begin{cases}
x\equiv a_i\pmod{m_i}\\
x\equiv a_j\pmod{m_j}
\end{cases}
$$

先将其转化为不定方程的形式

$$
x=a_ip+m_i=a_jq+m_j(p,q\in \mathbb Z)
\\ \iff a_ip-a_jq=m_j-m_i
$$

如果 $(a_i,a_j)\not | (m_j-m_i)$，那么无解，否则可以拿扩欧算。整个过程朴素到没有亮点。

### Lucas' theorem

用途是求很大的 $\dbinom{n}{k}\pmod p$。

> 引理 1：若 $n\not=0,p$，则 $\dbinom{p}{n}\equiv 0\pmod p$

拿阶乘表示出来就行了。

> 引理 2：设 $f(x)=ax^n+bx^m$，则有
> $$f^p(x)\equiv f(x^p) \pmod p$$

$\text{proof.}$

$$
f^p(x)=(ax^n+bx_m)^p
\\=\sum_{k=0}^p \dbinom{p}{k}(ax^n)^k(bx^m)^{p-k}
\\\equiv a^px^{pn}+b^px^{pm}
\\ \equiv a(x^p)^n+b(x^p)^m=f(x^p)
$$

$$\text{Q.E.D.}$$

> Lucas 定理：
> $$\dbinom{n}{k}\equiv \dbinom{\lfloor n/p\rfloor}{\lfloor k/p \rfloor}\dbinom{n\bmod p}{k\bmod p} \pmod p$$

$\text{proof.}$

考察 $\dbinom{n}{k}$ 的生成函数 $(1+x)^n$。

$$(1+x)^n=(1+x)^{p\lfloor n/p\rfloor}(1+x)^{n\bmod p}$$

由引理 2，可知

$$(1+x)^n\equiv(1+x^p)^{\lfloor n/p\rfloor}(1+x)^{n\bmod p}\pmod p$$

$x^k$ 的系数就是 $\dbinom{n}{k}\bmod p$，并且把 $k$ 拆在两边的方式是唯一的，也即

$$k=p\lfloor\frac{n}{p}\rfloor+(n\bmod p)$$

于是定理得证。$\square$

#### exLucas 定理

如果模数 $m$ 不是质数怎么办？

把 $m$ 质因数分解，设

$$m=\prod_{i=1}^s p_i^{\alpha_i}$$

分别计算 $\dbinom{n}{k}\bmod p_i^{\alpha_i}$，得到 $s$ 个同余方程，然后就可以使用 CRT 直接算。

## Functions

### Common productive functions

- 除数函数 $\sigma_{k}(n)=\sum_{d|n}d^k$

- $\text{Euler}$ 函数 $\varphi(n)=\sum_{i=1}^n[\gcd(i,n)=1]$

- $\text{M\"{o}bius}$ 函数 $\mu(n)=\begin{cases}1&n=1\\(-1)^k&n为k个不同质数之积\\0&\text{otherwise}\end{cases}$

常见的完全积性函数：

- 幂函数 $\text{Id}_k(n)=n^k$，$\text{Id}_1$ 简记为 $\text{Id}$

- 单位函数 $\epsilon(n)=[n=1]$

### Dirichlet convolution

定义 $*$ 运算表示：

$$
(f*g)(n)=\sum_{d|n}f(d)g(\frac{n}{d})
$$

Dirichlet 卷积有以下性质：

- 交换律：$f*g=g*f$

- 结合率：$(f*g)*h=f*(g*h)$

- 分配率：$f*(g+h)=f*g+f*h$

- 单位元：$f*\epsilon=f$，其中 $\epsilon(n)=[n=1]$

- 若 $f,g$ 都是积性函数，则 $f*g$ 也是积性函数

证明是显然的，这里不放了。

常见的卷积有：

- $\mu * 1 = \epsilon$
- $\varphi * \text{Id}=1$
- $\varphi * 1 = \text{Id}$
- $d(i,j)=\sum\limits_{x|i}\sum\limits_{y|i}[\gcd(x,y)=1]$
- $\varphi(xy)=\frac{\varphi(x)\varphi(y)\gcd(x,y)}{\varphi(\gcd(x,y))}$

证明过程放在每个题里。

### $\text{M}\ddot{\text{o}}\text{bius Function}$

定义莫比乌斯函数 $\mu(n)$。设 $n=\prod\limits_{i=1}^kp_i^{\alpha_i}$，有

$$
\mu(n)=\begin{cases}
1 & n=1\\
0 & \exists i,\alpha_{i}\geq 2\\
(-1)^k & \text{otherwise}
\end{cases}
$$

可知这个函数是积性函数，于是可以线性筛求 $\mu$。代码如下：

```cpp
void euler(int n)
{
 mu[1] = 1;
 for (int i = 2; i <= n; i++)
 {
  if (!v[i]) p[++pcnt] = i, mu[i] = -1;
  for (int j = 1; j <= pcnt && i * p[j] <= n; j++)
  {
   v[i * p[j]] = 1;
   if (i % p[j] == 0) break;
   mu[i * p[j]] = -mu[i];
  }
 }
}
```

### $\text{M}\ddot{\text{o}}\text{bius Inversion}$

如果两个数论函数 $f,g$ 满足：

$$
f(n)=\sum_{d|n}g(d)
$$

则它们也同时满足:

$$
g(n)=\sum_{d|n}\mu(d)f(\frac{n}{d})
$$

反之亦然，即 $f=g*1\iff g=\mu*f$。

$\text{proof.}$

左推右，考虑凑出 $f*\mu$，所以对两边都卷上 $\mu$，有

$$
f*\mu=g*1*\mu
$$

于是只要证 $1*\mu=\epsilon$ 就行了。这个引理十分重要，这里证一下。

考虑利用算贡献的方法，考虑每个因数会选出多少个质因数，考虑这个数量的贡献求和，来改写式子左边有，

$$\text{LHS}=\sum_{d|n}\mu(d)=\sum_{i=0}^k(-1)^i\begin{pmatrix} k\\ i\end{pmatrix}=(1-1)^n=\epsilon(n)=\text{RHS}，\square$$

于是有 $f*\mu=g*\epsilon=g$，充分性得证。

必要性只需要把两边都卷上 $1$ 就行了。$\text{Q.E.D.}$

在基础应用中，最常用的是证明过程中顺手证的引理，但是正牌莫反会有更多的用处。但我们先不急着去做题，莫比乌斯反演还有一种非卷积形式：

设 $f,g$ 为数论函数，$t$ 为完全积性函数且 $t(1)=1$，有

$$
f(n)=\sum_{k=1}^n t(k)g(\lfloor\frac{n}{k}\rfloor)\iff g(n)=\sum_{k=1}^nt(k)\mu(k)f(\lfloor\frac{n}{k}\rfloor)
$$

$\text{proof.}$

左推右，有

$$\sum_{k=1}^n\mu(k)t(k)f(\lfloor\frac{n}{k}\rfloor)=\sum_{k=1}^n\mu(k)t(k)\sum_{i=1}^{\lfloor\frac{n}{k}\rfloor}t(i)g(\lfloor\frac{n}{ki}\rfloor)$$

枚举 $ki$，有

$$\sum_{k=1}^n\mu(k)t(k)f(\lfloor\frac{n}{k}\rfloor)=\sum_{j=1}^ng(\lfloor\frac{n}{j}\rfloor)\sum_{i|j}t(i)t(\frac{j}{i})\mu(i)=\sum_{j=1}^ng(\lfloor\frac{n}{j}\rfloor)t(j)\sum_{i|j}\mu(i)$$

由引理，得

$$\sum_{k=1}^n\mu(k)t(k)f(\lfloor\frac{n}{k}\rfloor) = \sum_{j=1}^ng(\lfloor\frac{n}{j}\rfloor)t(j)\epsilon(j)=g(n)t(1)=g(n)$$

反之亦然。$\text{Q.E.D.}$

### Basical Problems

1.GCD SUM

求

$$
\sum_{i=1}^{n}\sum_{j=1}^n\gcd(i,j)
$$

$\text{sol.}$

先证引理：$\varphi(n)=\sum_{d|n}\mu(d)\frac{n}{d}$。

$$
\phi(n)=\sum_{i=1}^n[\gcd(i,n)=1]=\sum_{i=1}^n\sum_{d|\gcd(i,n)}\mu(d)
$$

然后考虑 $\mu(d)$ 的贡献，有

$$\varphi(n)=\sum_{d|n}\mu(d)\frac{n}{d}$$

故引理得证。这个引理也很常用。

$$
\text{Ans}=\sum_{i=1}^n\sum_{j=1}^n\sum_{d=1}^n[\gcd(i,j)=d]d
\\=\sum_{d=1}^nd\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{d}\rfloor}\epsilon(\gcd(i,j))
\\=\sum_{d=1}^nd\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{k|\gcd(i,j)}\mu(k)
\\=\sum_{d=1}^nd\sum_{k=1}^n\mu(k)\lfloor\frac{n}{dk}\rfloor^2
\\=\sum_{D=1}^n\lfloor\frac{n}{D}\rfloor^2\sum_{d|D}d\mu(\frac{D}{d})\\=\sum_{D=1}^n\lfloor\frac{n}{D}\rfloor^2\varphi(n)
$$

其中第一行到第二行我们把 $d$ 提到前面去，改枚举 $i,j$ 为 $\frac{i}{d},\frac{j}{d}$，把 $[\gcd(i,j)=d]$ 转化为 $\gcd(\frac{i}{d},\frac{j}{d})=1$，就可以利用莫反了。所以第二步到第三步就是生套莫反公式，第三步到第四步是考虑 $\mu(k)$ 的贡献，然后再改求和顺序。这样线性筛 $\varphi$，求前缀和，然后整除分块就可以 $O(n)$ 做了，瓶颈是线性筛。

请格外注意过程中对和式的处理。

---

2.Crash 的数字表格 / JZPTAB

求
$$\sum_{i=1}^n\sum_{j=1}^m\operatorname{lcm}(i,j)$$

$\text{sol.}$

$$
\text{Ans}=\sum_{i=1}^n\sum_{j=1}^m\operatorname{lcm}(i,j)=\sum_{i=1}^n\sum_{j=1}^m\frac{ij}{\gcd(i,j)}
$$

然后利用经典算贡献套路把 $d$ 提出来，枚举 $i,j$ 改为枚举 $di,dj$ 来枚举 $\gcd(i,j)$，不妨设 $n\leq m$，有

$$
\text{Ans}=\sum_{d=1}^n d\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{m}{d}\rfloor}[\gcd(i,j)=1]ij=\sum_{d=1}^n d\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{m}{d}\rfloor}\epsilon(\gcd(i,j))ij
$$

由莫比乌斯反演公式，我们有 $\sum_{d|n}\mu(d)=\epsilon(n)$，于是原式转化为

$$
\sum_{d=1}^n \sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{m}{d}\rfloor}\sum_{x|\gcd(i,j)}\mu(x)ij
$$

然后再利用枚举 $\gcd$ 的套路，有

$$
\text{Ans}=\sum_{d=1}^n d\sum_{x=1}^{\lfloor\frac{n}{d}\rfloor}\mu(x)\sum_{xi=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{xj=1}^{\lfloor\frac{m}{d}\rfloor}x^2ij=\sum_{d=1}^nd\sum_{x=1}^{\lfloor\frac{n}{d}\rfloor}\mu(x)x^2(\sum_{i=1}^{\lfloor\frac{n}{dx}\rfloor}i)(\sum_{j=1}^{\lfloor\frac{n}{dx}\rfloor}j)
$$

记 $s(x)=\sum_{i=1}^x i$，于是有

$$
\text{Ans}=\sum_{d=1}^n\sum_{x=1}^{\lfloor\frac{n}{d}\rfloor}\mu(x)x^2s(\lfloor\frac{n}{dx}\rfloor)s(\lfloor\frac{m}{dx}\rfloor)
$$

把 $s(\lfloor\frac{n}{dx}\rfloor)s(\lfloor\frac{m}{dx}\rfloor)$ 提前面，就有

$$
\text{Ans}=\sum_{D=1}^ns(\lfloor\frac{n}{D}\rfloor)s(\lfloor\frac{m}{D}\rfloor)\sum_{db=D}db^2\mu(b)=\sum_{D=1}^ns(\lfloor\frac{n}{D}\rfloor)s(\lfloor\frac{m}{D}\rfloor)D\sum_{b|D}b\mu(b)
$$

然后预处理 $b\mu(b)$ 的 Dirichlet 前缀和，然后整除分块就能做了。

---

3.约数个数和

设 $d(x)$ 表示 $x$ 的约数个数，求

$$\sum_{i=1}^n\sum_{j=1}^md(ij)$$

$\text{sol.}$

先想着利用 $d$ 的积性，把 $d(ij)$ 转化为 $\frac{d(i)d(j)}{d(\gcd(i,j))}$，发现推得

$$\text{Ans}=\sum_{i=1}^m\frac{1}{d(i)}\cdot(\sum_{j=1}^nd(j))\cdot(\sum_{j=1}^md(j))$$

如果所求有取模，那直接预处理 $sum_{i=1}^nd(i)$ 和 $\sum_{i=1}^n\frac{1}{d(i)}$ 就可以做到 $O(\sqrt{n})$ 了，但这个题没有取模的要求，考虑直接求 $d(ij)$。我们有引理：

$$d(xy)=\sum_{i|x}\sum_{j|y}[\gcd(i,j)=1]$$

$\text{proof.}$ 不会，抄的题解，但感觉很有用，记一下以后常看看。

然后莫反的过程就比较平凡了。

加强一下：[SDOI2018] 旧试题

求
$$\sum_{i=1}^A\sum_{j=1}^B\sum_{k=1}^Cd(ijk)$$

$\text{sol.}$

还不会

### Du's Algorithm

设 $f$ 为数论函数，要求

$$S(n)=\sum_{i=1}^nf(i)$$

考虑构造一个数论函数 $g$，我们有恒等式

$$\sum_{i=1}^n(f*g)(i)=\sum_{i=1}^ng(i)S(\lfloor\frac{n}{i}\rfloor)$$

得到递归式

$$g(1)S(n)=\sum_{i=1}^n(f*g)(i)-\sum_{i=2}^ng(i)S(\lfloor\frac{n}{i}\rfloor)$$

熟知 $\lfloor\frac{n}{i}\rfloor$ 的取值为 $O(\sqrt{n})$ 的，假如可以快速对 $(f*g)(i)$ 与 $g(i)$ 的和，可以根据 $\lfloor\frac{n}{i}\rfloor$ 的值进行数论分块，进行一遍求和的时间复杂度就是 $O(\sqrt n)$，然后总的复杂度就是

$$\sum_{i=1}^{\lfloor\sqrt{n}\rfloor}O(\sqrt i)+\sum_{i=1}^{\lfloor\sqrt n\rfloor}O(\sqrt{\frac{n}{i}})$$

（温情提示：前方积分预警）

后面一部分明显比前一部分大，所以考虑算后面的。

$$\sum_{i=1}^{\lfloor\sqrt n\rfloor}O(\sqrt{\frac{n}{i}})\approx O(\int_{0}^{\sqrt n}\sqrt{\frac{n}{x}}{\rm d}x)=O(n^{\frac{3}{4}})$$

你可能说积分算大了，但你舍掉左边那一部分本来就算小了啊。

考虑能否优化这个过程，我们考虑预处理一部分，然后剩下部分直接调用算好的。我们设预处理了前 $k$ 个 $S$ 并假设预处理的复杂度为 $O(n)$，那么复杂度就是

$$O(k)+O(\sum_{i=2}^{\lfloor\frac{n}{k}\rfloor}\sqrt{\frac{n}{i}})\approx O(k+\int_{0}^{\frac{n}{k}}\sqrt{\frac{n}{x}}{\rm d}x)=O(k+\frac{n}{\sqrt k})$$

由均值不等式可知当 $k$ 取 $n^{\frac{2}{3}}$ 是复杂度最小，为 $O(n^{\frac{2}{3}})$。

#### A Simple Math Problem

求

$$\sum_{i=1}^n\sum_{j=1}^nij\gcd(i,j)$$

$\text{sol.}$

利用莫反常见套路推出来

$$\text{Ans}=\sum_{T=1}^n(\sum_{i=1}^{\lfloor\frac{n}{T}\rfloor}i)^2T^2\varphi(T)$$

然后考虑算 $S(n)=\sum_{i=1}^ni^2\varphi(i)$。我们套用杜教筛板子

$$g(1)S(n)=\sum_{i=1}^n(f*g)(i)-\sum_{i=2}^ng(i)S(\lfloor\frac{n}{i}\rfloor)$$

我们想到 $\varphi$ 的常见卷积

$$\varphi*1=\text{Id}$$

于是令 $g(x)=x^2$，有

$$(g*f)(i)=\sum_{d|n}d^2\varphi(d)\frac{i^2}{d^2}=i^2\sum_{d|n}\varphi(d)=i^3$$

然后就可以快速地进行杜教筛。

## Technologies

### Miller–Rabin test

### Pollard Rho Algorithm
