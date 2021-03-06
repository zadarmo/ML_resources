# Task03

### 时间

:calendar:2021年10月22日9:00~2021年10月24日21:00

### 内容

西瓜书第4章4.1~4.2

### 目标

学习决策树

### 暂时可忽略内容

无



# 笔记

## 一、信息论基本概念

### 1. 自信息

自信息定义为：
$$
I(X)=-log_bp(x)
$$
当$b=2$时，单位为bit；当$b=e$时，单位为nat

### 2. 信息熵

信息熵反映了随机变量$X$的不确定性。信息熵越大，表明不确定性越大。信息熵 = 自信息的期望，即
$$
H(X)=E(I(X))=-\sum_x p(x)log_bp(x)（以离散型为例）
$$
其中，$x_i$表示某一类样本。

信息熵的3点说明（详细证明可以看**南瓜书4.1式**）：

- 若$p(x)=0$，则$p(x)log_bp(x)=0$
- 当$X$的某个取值概率为1时，**信息熵最小**（最确定），为0
- 当$X$在各个取值的概率相等时，**信息熵最大**（最不确定），为$log_b|X|$，其中$|X|$表示$X$可能取值的个数

### 3. 条件熵

条件上（$Y$的信息熵关于概率分布$X$的期望），在已知$X$后$Y$的不确定性
$$
H(Y|X)=\sum_a p(x)H(Y|X=a)
$$
从单个属性$a$的角度看，假设其取值为$\{ a^1,a^2,...,a^V \}$，$D^v$表示属性$a$取值为$a^v \in  \{ a^1,a^2,...,a^V \}$的样本集合，$\frac {|D^v|} {D}$表示占比，那么已知属性$a$的取值后，样本集合$D$的条件熵为
$$
\sum_{v=1}^V \frac {|D^v|}{|D|}Ent(D^v)
$$

### 4. 信息增益

**信息熵**反映了：不按任务属性进行划分时，样本集合的熵值

**条件熵**反映了：按某种属性$a$划分后，样本集合的熵值

将二者之差定义为信息增益，即
$$
Gain(D,a)=Ent(D)-\sum_{v=1}^V \frac {|D^v|}{|D|}Ent(D^v)
$$
该式的含义是：**在已知属性$a$的情况下，$y$熵值的减少量，即不确定性的减少量，也即纯度的提升**

## 二、决策树

### 1. 算法流程

### ![IMG_20211025_004212](C:\Users\destiny\Documents\Tencent Files\240412375\FileRecv\MobileFile\IMG_20211025_004212.jpg)

其中最主要的就是“从A中选择最优化分属性$a_*$”这一步。

### 2. 最优属性$a_*$的选择标准

根据不同的选择标准，可以分为3种算法

#### ID3决策树

ID3决策树用**信息增益**作为划分属性的准则
$$
a_* ={\underset {a \in A}{\operatorname {arg\,min} }}\,Gain(D,a)
$$
其中$A=\{ a_1,a_2,...,a_n \}$表示数据的属性集合。比如对于西瓜来说，$a_1$可以为大小，$a_2$可以为色泽。

#### C4.5决策树

信息增益准则对可能取值数目较多的属性有所偏好（例如“编号”这个极端的例子，不过其本质原因不是取值过多，而是每个取值的样本量太少）。

**C4.5决策树**采用**增益率**来代替信息增益。增益率定义为
$$
Gain\_ratio(D,a)=\frac {Gain(D,a)}{IV(a)}
$$
其中
$$
IV(a)=-\sum_{v=1}^V \frac{|D^v|}{|D|}log_2\frac{|D^v|}{|D|}
$$
称为属性$a$的**固有值**，$a$的可能取值个数$V$越大，通常$IV(a)$也越大。但是，增益率对可能取值数目较少的属性有所偏好

#### CART决策树

CART采用基尼指数最小的属性作为最优属性划分，那么什么是基尼指数呢？

首先介绍**基尼值**：从样本集合$D$中随机抽取两个样本，其类别标记不一致的概率。因此，基尼值越小，碰到异类的概率越小，纯度自然就越高。其公式为
$$
Gini(D)=\sum_{k=1}^{|y|}\sum_{k'\neq k}p_kp_{k'}=\sum_{k=1}^{|y|}p_k(1-p_k)=1-\sum_{k=1}^{|y|}p_k^2
$$
其中$|y|$表示样本所有类别的个数。

而基尼指数定义为
$$
Gini\_index(D,a)=\sum_{v=1}^V \frac {|D^v|}{|D|}Gini(D^v)
$$
选择基尼指数最小的属性作为划分属性的准则
$$
a_* ={\underset {a \in A}{\operatorname {arg\,min} }}\,Gain\_index(D,a)
$$
