---
layout: post_latex
title: 线性代数之正交矩阵与QR分解
tags: ['matrix','linear algebra']
published: true
---

## 基础知识

标准正交向量组（Orthonormal vectors）的点积(内积)性质：

\\( q\_\{i\}\^\{T\}q\_\{j\} = 0 \\) **if** \\( i\\neq j \\)

\\( q\_\{i\}\^\{T\}q\_\{j\} = 1 \\) **if** \\( i = j \\)

其中每个正交向量的长度\\(||q\_\{i\}||=1\\)。


标准正交向量组成的矩阵是：

<!--more-->

{% assign matA = "q\_\{1\},\\cdots,q\_\{n\}" | split: ',' %}
\\( Q = \\) \\( {% include render_matrix_raw.html mat = matA row = 1 col = 3 %} \\)

注意，列向量的分量数量未知，Q所以不一定是方阵。


{% assign matB = "q\_\{1\}\^\{T\},\\vdots,q\_\{n\}\^\{T\}" | split: ',' %}

\\( Q\^\{T\}Q = {% include render_matrix_raw.html mat = matB row = 3 col = 1 %}{% include render_matrix_raw.html mat = matA row = 1 col = 3 %} = I \\)

当Q是方阵时，显然Q有逆矩阵，且\\( Q\^\{-1\} = Q\^\{T\} \\)。

比如当Q为3阶单位矩阵I的置换矩阵时：


{% assign Q3 = "0,1,0,0,0,1,1,0,0" | split: ',' %}
{% assign Q3T = "0,0,1,1,0,0,0,1,0" | split: ',' %}

\\( Q\^\{T\}Q = {% include render_matrix_raw.html mat = Q3 row = 3 col = 3 %}{% include render_matrix_raw.html mat = Q3T row = 3 col = 3 %} = I \\)

或者三角函数作为元素的二阶Q：


{% assign Q2 = "cos\\theta,-sin\\theta,sin\\theta,cos\\theta" | split: ',' %}
{% assign Q2T = "cos\\theta,sin\\theta,-sin\\theta,cos\\theta" | split: ',' %}

\\( Q\^\{T\}Q = {% include render_matrix_raw.html mat = Q2 row = 2 col = 2 %} {% include  render_matrix_raw.html mat = Q2T row = 2 col = 2 %} = I \\)


## 定义

- 如果实数域上的方阵A满足 \\( A\^\{T\}A = I \\)，则称A为正交矩阵

- 当A的列向量的长度都为1时，称A为标准正交矩阵Q。


## 定理

- 当\\(A\^\{-1\} = A\^\{T\}\\)成立时A是正交矩阵
- A的列(或行)向量组是\\(R\^\{n\}\\)的一组标准正交基时，A是正交矩阵
- 正交矩阵A的行列式为1或-1
- 如果A是正交矩阵，则\\(A\^\{-1\},A\^\{T\},A\^\{*\}\\)都是正交矩阵
- 如果A、B都是正交矩阵，那么AB也是正交矩阵

## 正交矩阵怎么用？

在上一篇文章中，讲到了投影矩阵的各种公式，其中有一条是：

\\[ A\^\{T\}A\\hat\{x\} = A\^\{T\}b \\]

这个戴着帽子的x是未知量，要求它的值，就需要再变换下：

\\[ \\hat\{x\} = (A\^\{T\}A)\^\{-1\}A\^\{T\}b \\]


那么问题来了：右边的式子有点复杂，又要算矩阵乘法又要算逆矩阵，是不是可以简化呢？

答案是可以，且要用到正交矩阵。因为A是由一组线性无关的列向量组成，当把这组向量转换为标准正交向量组时，就得到了标准正交矩阵Q。拿Q代入上面的式子，得到：

\\[ Q\^\{T\}Q\\hat\{x\} = Q\^\{T\}b \\]

再根据上述的Q的公式，干掉左边的2个Q，得到：

\\[ \\hat\{x\} = Q\^\{T\}b \\]

瞬间豁然开朗。


但是还有一个问题，怎么从A得到Q呢？

## 矩阵的正交化算法

因为从A可以得到Q，所以必然有某个矩阵R，使得 A = QR 成立，这个过程叫做A的QR分解（QR decomposition)。

### 先举一个二维的例子：

设 \\(\\mathbf a1 = (3,4)、\\mathbf a2 = (4,3)\\)，显然\\(\\mathbf a1、\\mathbf a2\\)线性无关（不在同一条直线上），且\\(\\mathbf a1、\\mathbf a2\\)的长度都不是1。\\(\\mathbf a1、\\mathbf a2\\)是二维空间的一组基(basis)。

\\(\\mathbf a1、\\mathbf a2\\)对应的矩阵为：

{% assign A1 = "\\mathbf a\_\{1\}, \\mathbf a\_\{2\}" | split: ',' %}
{% assign A2 = "3,4,4,3" | split: ',' %}

\\[ A = {% include render_matrix_raw.html mat = A1 row = 1 col = 2 %} = {% include render_matrix_raw.html mat = A2 row = 2 col = 2 %} \\]

显然，A是一个方阵。接下来就是实现A的QR分解。

标准正交矩阵，可以分为2个步骤实现：

1. 正交化(orthogonal)
2. 标准化(normalize)

二维空间下，让2个向量正交化，即等于是要找出2个互相垂直的向量。怎么找最简单呢？因为这样的向量组无限多，于是\\(\\mathbf a1 或 \\mathbf a2\\)都可以是某组正交向量的其中一个向量。

那么，我们可以让\\(\\mathbf a1\\)作为一个正交向量，然后再找出一个与\\(\\mathbf a1\\)垂直的向量，就得到一组正交向量了。

设\\(\\mathbf u1、\\mathbf u2\\)为要求的标准正交向量组，\\(\\mathbf n1、\\mathbf n2\\)代表\\(\\mathbf a1、\\mathbf a2\\)的单位向量，那么可以有：

\\[ \\mathbf u\_\{1\} = \\mathbf n\_\{1\} \\]

\\(\\mathbf u2\\)怎么求？很简单，利用投影矩阵的知识即可完成。因为\\(\\mathbf a2 和 \\mathbf a1\\)线性无关，那么\\(\\mathbf a2 在 \\mathbf a1\\)上有且只有一个投影点\\(\\mathbf p2\\)，算出这个投影点\\(\\mathbf p2\\)，就能快速得到\\(\\mathbf a2\\)关于\\(\\mathbf a1\\)的error向量：

\\[ \\mathbf e\_\{2\} = \\mathbf a\_\{2\} - \\mathbf p\_\{2\} \\]

为什么要算\\(\\mathbf e2\\)向量？因为\\(\\mathbf e2\\)向量的一个重要性质是，\\(\\mathbf e2\\)和\\(\\mathbf a1\\)是互相垂直的，换句话说就是，**\\(\\mathbf e2\\)和\\(\\mathbf a1\\)正交!**。有了\\(\\mathbf e2\\)，将其单位化后，就是\\(\\mathbf u2\\)了。

求\\(\\mathbf u2\\)的步骤：

- 先求\\(\\mathbf n1\\)，即\\(\\mathbf a1\\)的单位向量

\\[ \\mathbf n\_\{1\} = \\dfrac \{ \\mathbf a\_\{1\} \}\{ ||\\mathbf a\_\{1\}|| \} \\] 

- 求出\\(\\mathbf a2\\)在\\(\\mathbf a1\\)上的投影点\\(\\mathbf p2\\) [vector projection](https://en.wikipedia.org/wiki/Vector_projection)

\\[ \\mathbf p\_\{2\} = \\mathbf n\_\{1\}\\dfrac \{ \\mathbf n\_\{1\}\\cdot \\mathbf a\_\{2\} \}\{  \\mathbf n\_\{1\}\\cdot \\mathbf n\_\{1\} \} = \\mathbf n\_\{1\}(\\mathbf n\_\{1\}\\cdot \\mathbf a\_\{2\}) \\] 

那个分母为什么可以去掉呢？这是因为\\(\\mathbf n1\\)是单位向量，\\( \\mathbf n\_\{1\}\\cdot \\mathbf n\_\{1\} \\)是\\(\\mathbf n1\\)的内积，显然这个内积等于1。

- 求\\(\\mathbf e2\\)

\\[ \\mathbf e\_\{2\} = \\mathbf a\_\{2\} - \\mathbf p\_\{2\} = \\mathbf a\_\{2\} - \\mathbf n\_\{1\}(\\mathbf n\_\{1\}\\cdot \\mathbf a\_\{2\}) \\]

- 单位化\\(\\mathbf e2\\)，得到\\(\\mathbf u2\\)

\\[ \\mathbf u\_\{2\} = \\dfrac \{ \\mathbf e\_\{2\} \}\{ ||\\mathbf e\_\{2\}|| \} \\]


好了，公式出来了，拿上面的例子验证下：

\\[ \\mathbf u\_\{1\} = \\mathbf n\_\{1\} = \\dfrac \{ \\mathbf a\_\{1\} \}\{ ||\\mathbf a\_\{1\}|| \} = (\\dfrac \{3\}\{5\},\\dfrac \{4\}\{5\} ) \\]

\\[ \\mathbf e\_\{2\} = \\mathbf a\_\{2\} - \\mathbf p\_\{2\} = \\mathbf a\_\{2\} - \\mathbf n\_\{1\}(\\mathbf n\_\{1\}\\cdot \\mathbf a\_\{2\}) = (\\dfrac \{28\}\{25\},\\dfrac \{-21\}\{25\}) \\]

\\[ \\mathbf u\_\{2\} = \\dfrac \{ \\mathbf e\_\{2\} \}\{ ||\\mathbf e\_\{2\}|| \} = (\\dfrac \{4\}\{5\},\\dfrac \{-3\}\{5\}) \\]

\\[ \\mathbf u\_\{1\} \\cdot \\mathbf u\_\{2\} = (\\dfrac \{3\}\{5\},\\dfrac \{4\}\{5\}) \\cdot (\\dfrac \{4\}\{5\},\\dfrac \{-3\}\{5\}) = 0 \\]

u1和u2的内积为0，且长度均为1，所以U1和u2是一组标准正交向量。

{% assign Q = "\\mathbf u\_\{1\}, \\mathbf u\_\{2\}" | split: ',' %}

\\[ Q = {% include render_matrix_raw.html mat = Q row = 1 col = 2 %} \\]

### 高维时的通用QR解法

上面是维度为2时的QR分解过程，接下来谈谈3维以及n维的情况。

当维度为3时，还是比较好想象的，我们分步构想下QR分解过程：

- A矩阵由3个线性无关的向量构成，A的列空间是一个三维空间，即三维空间的任意一个点都可以通过线性组合这3个向量得到
- 设3个向量为a1、a2、a3，先对a1、a2进行正交化运算，过程和上述的一致，除了\\(\\mathbf a\_\{i\}\\)变成了3个分量。
- 正交化a1、a2，就得到了a1、a2对应的平面空间的一组标准正交基u1、u2
- 于是原问题变成：求解u1、u2、a3的正交化。u1和u2已经是标准正交向量，不用管它们，问题就是求出u3。
- a3是在u1、u2对应的平面**之外**的一个点，a3在这个平面上有且只有一个投影点p3，将它求出来
- 再求出\\( e\_\{3\} = a\_\{3\} - p\_\{3\} \\)就得到了a3的error向量，该向量垂直于u1、u2平面！
- 将e3标准化后，就得到了u3

n>3维的情况，可以用归纳法解出。

高维情况下的QR分解，公式如下：
{% assign A = "a\_\{1\},a\_\{2\},\\cdots,a\_\{n\}" | split: ',' %}
{% assign Q = "u\_\{1\},u\_\{2\},\\cdots,u\_\{n\}" | split: ',' %}

{% assign R = "a\_\{1\}\\cdot u\_\{1\},a\_\{2\}\\cdot u\_\{1\},\\cdots ,a\_\{n\}\\cdot u\_\{1\},0,a\_\{2\}\\cdot u\_\{2\},\\cdots ,a\_\{n\}\\cdot u\_\{2\},\\vdots ,\\vdots ,\\ddots ,\\vdots,0,0,\\cdots ,a\_\{n\}\\cdot u\_\{n\}" | split: ',' %}

\\[ A = {% include render_matrix_raw.html mat = A row = 1 col = 4 %} \\]
\\[ A = {% include render_matrix_raw.html mat = Q row = 1 col = 4 %}{% include render_matrix_raw.html mat = R row = 4 col = 4 %} = QR \\]

Q是标准正交矩阵，R是上三角矩阵。

证明过程略，不过可以简单分析下这公式的正确性。

Q乘以R的结果是A矩阵，那么可以得到：

\\[ a\_\{1\} = u\_\{1\}(a\_\{1\}\\cdot u\_\{1\}) \\]

这个式子，是不是很眼熟，其实就是投影公式：

\\[ p\_\{1\} = a\_\{1\} = u\_\{1\}\\dfrac \{ u\_\{1\}\^\{T\}a\_\{1\} \}\{  u\_\{1\}\^\{T\} u\_\{1\} \} = u\_\{1\}(u\_\{1\}\^\{T\}a\_\{1\}) = u\_\{1\}(a\_\{1\}\\cdot u\_\{1\}) \\] 

（后面的内积有2种写法，同个意思，不用在意）

这个式子说明，经过QR分解后，a1的投影点还是a1，且在u1上。（但a1不一定等于u1）

我把a2也写出来：

\\[ a\_\{2\} = u\_\{1\}(a\_\{2\}\\cdot u\_\{1\}) + u\_\{2\}(a\_\{2\}\\cdot u\_\{2\}) \\]

类似a1，a2 = a2在u1的投影 + a2在u2的投影。（注意：a2一定不在u上，因为这样a1和a2就线性有关了；a2不一定在u2上）

然后a3，就不用写了，显然a3 = a3在u1的投影 + a3在u2的投影 + a3在u2的投影。

\\(a\_\{n\}\\)以此类推。所以这个公式是OK的。

总结一下：

要对A做QR分解，得从A本身出发，算出各个标准正交向量，并且这是一个迭代的过程：从a1算出u1，再通过a2和u1得到u2，接着，\\(u\_\{n\}\\)都可以通过\\(a\_\{n\}\\)和前面算出来的\\(u\_\{i\}\ \ (i<n)\\)得到。


如果QR分解的目的只是为了得到Q矩阵，那么R矩阵是没什么卵用的，因为R矩阵本身也包含了目标变量n，所以没办法用\\(Q=AR\^\{-1\}\\)公式求Q矩阵。