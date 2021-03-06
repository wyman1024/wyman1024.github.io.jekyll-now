---
layout: post_latex
title: 线性代数之伪逆矩阵(pseudoinverse matrix)
tags: ['matrix']
published: true
---

众所周知只有方阵才有逆矩阵，非方阵没有逆矩阵。这个不和谐的问题已在20世纪初被数学家[E. H. Moore](https://en.wikipedia.org/wiki/E._H._Moore)等人解决掉了，因为他们发明了**一般化的逆矩阵(generalized inverse)**，也称为**伪逆矩阵(Moore–Penrose pseudoinverse)**。

<!--more-->

# 定义

对于任意一个矩阵A，A的伪逆矩阵\\(A \^\{+\} \\)必然存在，且\\(A \^\{+\} \\)必然满足以下四个条件：

1. \\( AA \^\{+\}A = A \\)

2. \\( A \^\{+\}AA \^\{+\} = A \^\{+\} \\)

3. \\( (AA \^\{+\})\^\{*\} = AA \^\{+\} \\)

4. \\( (A \^\{+\}A)\^\{*\} = A \^\{+\}A \\)

这四个条件(性质)蕴含了一个事情：\\(AA \^\{+\} \\)必然是一个效果等同单位矩阵I、但又不是单位矩阵I的矩阵。

伪逆矩阵\\( A \^\{+\} \\)的极限形式定义：

\\[ A\^\{+\} = \\lim \_\{\\delta \\searrow 0\} ( A\^\{\*\}A + \\delta I ) \^\{-1\}A\^\{*\} \\]

\\[  = \\lim \_\{\\delta \\searrow 0\}A\^\{*\} ( A\^\{\*\}A + \\delta I ) \^\{-1\} \\]

伪逆矩阵更加常用的定义（基于SVD奇异值分解）：

- SVD公式：

\\[ A = UΣV\^\{*\} \\]

- 伪逆矩阵公式：

\\[ A\^\{+\} = VΣ\^\{+\}U\^\{*\} \\]

这个公式要注意的是中间的\\(Σ\^\{+\}\\)的求法。因为\\(Σ\_\{m\\times n\}\\)是一个对角线矩阵，但又不一定是方阵，所以计算它的伪逆矩阵的步骤是特殊又简单的：

1. 将对角线上的元素取倒数

2. 再将整个矩阵转置一次

# 性质

- 当A可逆时，A的伪逆矩阵等于A的逆矩阵

- 零矩阵的伪逆矩阵是它的转置矩阵

- \\( (A\^\{+\})\^\{+\} = A \\)

- \\( (A\^\{+\})\^\{T\} = (A\^\{T\})\^\{+\} \\)

- \\( ( \\overline \{A\} )\^\{+\} =  \\overline \{ A\^\{+\} \} \\)

- \\( (A\^\{\*\})\^\{+\}  = (A\^\{+\})\^\{\*\} \\)

- \\( (\\alpha A)\^\{+\}  = \\alpha \^\{-1\}A\^\{+\} \\)，\\(\\alpha \\)不等于0



# 应用：最小二乘估计

在我的[最小二乘估计(Least Squares Estimator)的公式的推导](http://www.qiujiawei.com/linear-algebra-15/)一文中，提到了一个小问题，**当矩阵X不是方阵时，最小二乘估计公式必须为**：

\\[ \\vec a = (X\^\{T\}X)\^\{-1\}X\^\{T\}\\vec y  \\]

不能进一步简化，除非X是有逆矩阵的方阵。

这个事情也可以用伪逆矩阵来讨论一遍。

先回到问题本源——最小二乘估计的本质是解决下面的方程：

\\[ \\vec y = X\\vec a + \\vec e \\]

其中\\(\\vec y\\)和\\(X\\)是已知量，\\(\\vec a\\)和\\(\\vec e\\)是要求的量，这可能有0到n个解，而我们的目标是想找使得\\( ||\\vec e||\_\{2\}\\)最小的\\(\\vec a\\)。

当我们求得理想的\\(\\vec a\\)、\\(\\vec e\\)后，可以让\\(\\vec e = \\vec 0\\)，并把\\(\\vec a\\)、\\(\\vec e\\)代入原方程，从而得到下面的等式：

\\[ \\widehat \{\\vec y\} = X\\vec a \\]

求得的\\( \\widehat \{\\vec y\} \\)就是\\( \\vec y  \\)的最佳近似。


再换个角度想——如果我们一开始就默认方程\\( \\vec y = X\\vec a  \\)有解，那么这个解就是：

\\[ \\vec a = X\^\{-1\}\\vec y \\]

慢着！\\(  X\^\{-1\} \\)可不一定存在的，因为X不一定是方阵，所以上面这个等式是错误的。怎么办？这时候伪逆矩阵就派上用场了：

\\[ \\vec a = X\^\{+\}\\vec y \\]

因为伪逆矩阵对任意矩阵都存在，所以这个等式才是合理的。


# 参考资料

[Moore–Penrose pseudoinverse](https://en.wikipedia.org/wiki/Moore%E2%80%93Penrose_pseudoinverse#Singular_value_decomposition_.28SVD.29)


