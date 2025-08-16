---
title: fwt
tags: math, poly
---

### FWT

用来求解位运算卷积。位运算卷积就是

$$C_p=\sum_{i \operatorname{bitwork}j=p}A_iB_j$$

直接做慢的一批。类比 FFT，考虑转化 $A * B = C$ 为 $FWT_A\cdot FWT_B=FWT_C$，其中 $FWT_A(i)=\sum_{j=0}^{n-1}c(i,j)A_j$，这样只要迅速求出 $FWT(A)$ 与 $IFWT(A)$ 就可以了。

考虑 $c(i,j)$ 具有的性质。由 FWT 的点积式子可得

$$\sum_j \sum_k c(i,j)c(i,k)A_jB_k=\sum_p c(i,p)C_p$$

再有卷积式子

$$C_p=\sum_{x \operatorname{bitwork}y=p}A_xB_y$$

于是

$$
\sum_j\sum_kc(i,j)c(i,k)A_jB_k\\=\sum_p\sum_{t1\operatorname{bitwork}t2=p}A_{t1}B_{t2}c(i,t1\operatorname{bitwork}t2)
\\=\sum_x\sum_y A_xB_yc(i,x\operatorname{bitwork}y)
$$

于是有性质

$$c(i,j)c(i,k)=c(i,j\operatorname{bitwork}k)$$

又由位运算的性质可知 $c(i,j)$ 可以拆位考虑，即

$$c(i,j)=\prod_{k} c(i_k,j_k)$$

也即将每一位的变换系数乘起来。于是我们需要的就只有 $c([0,1],[0,1])$。这样所需的性质就足够了。

考虑加速求 FWT 的过程。把式子拆开，得

$$
FWT_A(i)=\sum_{j=0}^{\lfloor\frac{n}{2}\rfloor-1}c(i_0,j_0)c(i',j')A_j+\sum_{j=\lfloor\frac{n}{2}\rfloor}^{n-1}c(i_0,j_0)c(i',j')A_j
$$

这样拆的原因是左半块的式子的系数都是 $c(i_0,0)$，右半块的都是 $c(i_0,1)$。这样就像 FFT 把式子拆成奇偶两段一样，我们可以 $O(n\log n)$ 求解 FWT。IFWT 只需要列出 $c$ 的逆就行了。实现的时候可以利用迭代的方式而非递归。

```cpp
inline void FWT(ll *f, const ll c[2][2], int n)
{
 for (int len = 1; len < n; len <<= 1)
  for (int p = 0; p < n; p += len + len)
   for (int i = p; i < p + len; ++i)
   {
    int tmp = f[i];
    f[i] = (c[0][0] * f[i] + c[0][1] * f[i + len]) % mod;
    f[i + len] = (c[1][0] * tmp + c[1][1] * f[i + len]) % mod;
   }
}
```

然后对与 $\text{or, and, xor}$ 三种运算，我们试图求出这三种运算的 $c$ 矩阵。构造的方式依靠的是上面的性质。懒得写 Latex 的矩阵了，直接贴代码。

```cpp
const ll Cor[2][2] = {{1, 0}, {1, 1}};
const ll ICor[2][2] = {{1, 0}, {mod - 1, 1}};
const ll Cand[2][2] = {{1, 1}, {0, 1}};
const ll ICand[2][2] = {{1, mod - 1}, {0, 1}};
const ll Cxor[2][2] = {{1, 1}, {1, mod - 1}};
const ll ICxor[2][2] = {{inv2, inv2}, {inv2, mod - inv2}};
```
