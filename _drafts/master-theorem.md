---
title: master-theorem
tags: math
---

### 定理形式

对于形如 $T(n)=aT(n/b)+O(n^d)$ 的时间复杂度递推式，我们有：

$$
T(n)=\left\{
\begin{aligned}
O(n^{\log_b a}) &&  \log_b a > d \\
O(n^d \log n) &&  \log_b a = d \\
O(n^d) && \log_b a < d
\end{aligned}
\right.
$$

### 定理证明

把原形式展开得到：

$$
T(n)=a^{\log_b n} + \sum_{i=0}^{\log_b n-1}a^i (\frac{n}{b^i})^d
\\= a^{\frac{\log_a n}{\log_a b}}+\sum_{i=0}^{\log_b n-1}a^i (\frac{n}{b^i})^d
\\= n^{\frac{1}{\log_a b}}+n^d \sum_{i=1}^{\log_b n-1} (\frac{a}{b^d})^i
\\= n^{\frac{\log_{a}a}{\log_{a}b}}+n^d \sum_{i=1}^{\log_b n-1} (\frac{a}{b^d})^i
$$

方便起见我们设 $h=\log_b n,c=\log_b a$，则

$$
T(n)=n^c+n^d\frac{a(b^d)^{h-1}-a^h}{(b^d)^{h-1}(b^d-a)}
\\= n^c + \frac{1}{b^d-a}(an^d-b^dn^c)
$$

然后讨就完了。

参考：[Forward Star 大佬的博客](https://zhuanlan.zhihu.com/p/196781010)

感谢 xuyunpeng，happyglx。
