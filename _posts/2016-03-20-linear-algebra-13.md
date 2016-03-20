---
layout: post_latex
title: 线性代数之视角矩阵Lookat Matrix
tags: ['matrix','linear algebra']
published: true
---

# 引言

我对视角矩阵的理解是这样子的，假设3维空间有一个观察者（摄像机），这个观察者必然有它的坐标位置、视角、焦点，根据这3个参数，可以建立一个正交化、规范化的坐标系（一个正交化、单位化的3x3矩阵），这个坐标系对应的矩阵就是Lookat矩阵。

<!--more-->


根据上面这个我自己创造的定义，可以知道，Lookat矩阵只和观察者的坐标、焦点、视角有关，和被观察的东西完全无关，也就是说，Lookat矩阵是independent的，这个性质的好处是，这个观察者的Lookat矩阵，可以应用到任意目标上。

观察者的坐标、焦点、视角，可以进一步抽象。观察者坐标位置设为eye向量，焦点位置设为focal向量，视角呢，比较特殊，是设为一个up向量，含义是这个观察者的头顶朝向。

可以想象成，观察者就是你自己，你站在地面上，盯着远处一个美女，可以是盯着她的腿、她的腰，都无妨。

看的过程中，你可以往左侧着头看，也可以倒立着看，不会影响focal向量，因为你的眼睛还是能看见美女。

但是当你低头时，情况就有些变化了。第一种情况：你对着美女弯腰90度，focal向量变成指向了地面上的某个点，up向量虽然还是沿着你头部的方向，不过因为弯腰90度的关系，已经不是朝着上方了(大约是指向了美女的方向)；第二种情况是，你只是微微低头(大于15度这样子)，眼睛还是能看到美女（低头15度的视野和完全直立时一致），不过，up向量被改变了，因为你的头转了15度。


通过这个例子，可以知道，focal向量和up向量之间是存在联系的，而eye向量则和focal、up向量没有关系，eye向量决定的是你所在的位置。

唠嗑到这里，下面进入数学环节。

# 推导

当eye、focal、up三个向量的值确定后，就可以构造Lookat矩阵了。


首先明确2点：一，Lookat矩阵是正交且规范化的；二，我们使用的是右手坐标系。

这个Lookat矩阵，相当于是一个坐标系，那么可以设三个坐标轴的方向向量分别为\\(\\vec r\\)、\\(\\vec u\\)、\\(\\vec f\\)，分别的含义是，观察者坐标系的right、up、forward方向。

\\(\\vec f\\)可以轻松得到：它的朝向是focal的反方向。为什么呢？很简单，focal是指从观察者位置到焦点位置的方向向量，又因为我们用的是右手坐标系，那么观察者坐标系的f轴朝向当然是focal的反方向了。

\\[\\vec f = - \\frac \{\\overrightarrow \{focal\} \}\{\|\\overrightarrow \{focal\}\|\} \\]

接着是\\(\\vec r\\)。显然，\\(\\vec r\\)指的方向是，focal和up所构成的平面的垂线的正方向，即focal和up的叉积。

\\[\\vec r = \\frac \{\\overrightarrow \{focal\} \\times \\overrightarrow \{up\}\}\{\|\\overrightarrow \{focal\} \\times \\overrightarrow \{up\}\|\} \\]

\\(\\vec r\\)、\\(\\vec f\\)都得到后，\\(\\vec u\\)就简单了，因为\\(\\vec r\\)、\\(\\vec f\\)已经规范化、正交化了的，那么\\(\\vec u\\)就是他们的叉积：

\\[\\vec u = \\vec f \\times \\vec r \\]

设Lookat矩阵为M，则M等于：

{% assign matM = "r\_\{x\},r\_\{y\},r\_\{z\},0, u\_\{x\},u\_\{y\},u\_\{z\},0, f\_\{x\},f\_\{y\},f\_\{z\},0, 0,0,0,1" | split: ',' %}

\\[ M = {% include render_matrix_raw.html mat = matM row = 4 col = 4 %} \\]


拿这个M和单位矩阵I对比下：

{% assign matI = "1,0,0,0,0,1,0,0,0,0,1,0,0,0,0,1" | split: ',' %}

\\[ I = {% include render_matrix_raw.html mat = matI row = 4 col = 4 %} \\]

可以发现，单位矩阵I相当于是把观察者放在世界空间的原点。因为I的\\(\\vec r\\)、\\(\\vec u\\)、\\(\\vec f\\)已经是规范化、正交化的，且和世界坐标系一致。

所以上面的M可以理解为：M等于M乘以I。含义是，把世界坐标系变换到观察者坐标系。也即相当于调整了对world的观察角度。


到了这里，事情还没完，因为这个M并不能体现出观察者的**位置**，为什么呢？因为\\(\\vec r\\)、\\(\\vec u\\)、\\(\\vec f\\)是规范化的向量，长度都为1，并不包含位置信息。

和单位矩阵I对比的话就清楚了，单位矩阵I之所以不需要位置信息，是因为单位矩阵I已经隐含了一个信息：观察位置就在(0,0,0)。

观察位置，上面已经定义过了，它就是eye向量。

把观察者放到eye位置，反过来想，相当于是把被观察的东西偏移-eye的距离。实际上，我们正在构造的Lookat矩阵，不是要作用到观察者身上，而是要作用到被观察者（world）身上的。

因此，现在可以根据eye向量构造一个移动矩阵T(Translate)了：


{% assign matT = "1,0,0,-eye\_\{x\},0,1,0,-eye\_\{y\},0,0,1,-eye\_\{z\},0,0,0,1" | split: ',' %}

\\[ T = {% include render_matrix_raw.html mat = matT row = 4 col = 4 %} \\]

然后把M和T合并，即得到了Lookat矩阵：


\\[ Lookat = MT \\]

\\[ = {% include render_matrix_raw.html mat = matM row = 4 col = 4 %}{% include render_matrix_raw.html mat = matT row = 4 col = 4 %} \\]

{% assign matL =  "r\_\{x\},r\_\{y\},r\_\{z\},-r\_\{x\}eye\_\{x\}-r\_\{y\}eye\_\{y\}-r\_\{z\}eye\_\{z\}, u\_\{x\},u\_\{y\},u\_\{z\},-u\_\{x\}eye\_\{x\}-u\_\{y\}eye\_\{y\}-u\_\{z\}eye\_\{z\}, f\_\{x\},f\_\{y\},f\_\{z\},-f\_\{x\}eye\_\{x\}-f\_\{y\}eye\_\{y\}-f\_\{z\}eye\_\{z\}, 0,0,0,1" | split: ',' %}

\\[ = {% include render_matrix_raw.html mat = matL row = 4 col = 4 %} \\]

简化下：

{% assign matL2 =  "r\_\{x\},r\_\{y\},r\_\{z\},-(\\vec r\\cdot \\overrightarrow \{eye\}), u\_\{x\},u\_\{y\},u\_\{z\},-(\\vec u\\cdot \\overrightarrow \{eye\}), f\_\{x\},f\_\{y\},f\_\{z\},-(\\vec f\\cdot \\overrightarrow \{eye\}), 0,0,0,1" | split: ',' %}

\\[ Lookat = {% include render_matrix_raw.html mat = matL2 row = 4 col = 4 %} \\]