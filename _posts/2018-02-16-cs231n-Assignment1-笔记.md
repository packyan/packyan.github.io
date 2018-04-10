---
layout: post
title: cs231n Assignment1 笔记
date: 2018-2-16
categories: blog
tags: [Python,cs231n]
description: 入坑cs231n卷积神经网络入门，好多数学公式，赶快记起来。
---
 入坑cs231n卷积神经网络入门，好多数学公式，赶快记起来。
 
标签（空格分隔）： Python cs231n

---

[智能单元笔记翻译](https://zhuanlan.zhihu.com/p/21930884)
[github参考1](https://github.com/Halfish/cs231n)
[github参考2](https://github.com/lightaime/cs231n)

## compute_distances_no_loops

主要是一个数学公式推导问题，也体现了numpy broadcast功能的强大~ 好好利用broadcast可以写出vectorized code。当我们对一个标量或者向量进行broadcast时，一定是指按元素（elementwise）操作，不像矩阵乘法那样的操作。

1. 假设测试集矩阵$P$大小为$M \times D$, 训练集矩阵$C$ 大小为$N \times D$
2. $记P_i是P的第i行，同理C_j是C的第j行$：

\begin{equation}
P_i = [P_{i1}  \quad P_{i2} \quad \cdots \quad P_{iD}] \\
C_j = [C_{j1} \quad C_{j2} \quad \cdots \quad C_{jD}]
\end{equation}

3. 计算一下$P_i$到 $C_j$的距离：

\begin{equation}
d(P_i, C_j) =\sqrt{(P_{i1} - P_{j1})^2 + (P_{i2} - P_{j2})^2 + \cdots + (P_{iD} - P_{jD})^2} \\
 =\sqrt{(P_{i1}^2 + P_{i2}^2 + \cdots + P_{iD}^2) + (C_{j1}^2 + C_{j2}^2 + \cdots + C_{jD}^2) - 2 * (P_{i1}C_{j1} + P_{i2}C_{j2} + \cdots + P_{iD}C_{jD})} \\
 =\sqrt{||P_i||^2+||C_j||^2 - 2 * P_i C_j'}
\end{equation}

4. 结果矩阵的每行元素：

\begin{equation}
Res(i) = \sqrt{(||P_i||^2 \quad ||P_i||^2 \quad \cdots ||P_i||^2) + (||C_1||^2 \quad ||C_2||^2 \quad \cdots \quad ||C_N||^2) - 2 * P_i (C_1' \quad C_2' \quad \cdots \quad C_N')} \\
=\sqrt{(||P_i||^2 \quad ||P_i||^2 \quad \cdots ||P_i||^2) + (||C_1||^2 \quad ||C_2||^2 \quad \cdots \quad ||C_N||^2) - 2 * P_iC'}
\end{equation}

5. 结果矩阵为：
\begin{equation}
Res = \sqrt{
\begin{pmatrix} 
||P_1||^2 && ||P_1||^2 && \cdots && ||P_1||^2 \\
||P_2||^2 && ||P_2||^2 && \cdots && ||P_2||^2 \\
\vdots      && \vdots && \ddots &&\vdots\\
||P_M||^2 && ||P_M||^2 && \cdots && ||P_M||^2
\end{pmatrix} + \begin{pmatrix} 
||C_1||^2 && ||C_2||^2 && \cdots && ||C_N||^2 \\
||C_1||^2 && ||C_2||^2 && \cdots && ||C_N||^2 \\
\vdots      && \vdots && \ddots &&\vdots\\
||C_1||^2 && ||C_2||^2 && \cdots && ||C_N||^2
\end{pmatrix}
-2PC'
} \\
=\sqrt{
\begin{pmatrix} 
||P_1||^2  \\
||P_2||^2 \\
\vdots  \\
||P_M||^2 
\end{pmatrix}_{M\times 1}
*
\begin{pmatrix} 
1 && 1 && \cdots && 1
\end{pmatrix}_{1 \times N}
+
\begin{pmatrix} 
1  \\
1 \\
\vdots  \\
1 
\end{pmatrix}_{M\times 1}
*
\begin{pmatrix} 
||C_1||^2 && ||C_2||^2 && \cdots && ||C_N||^2
\end{pmatrix}_{1 \times N}
-2P_{M\times D}C'_{N\times D}
}
\end{equation}

python代码可以这样写：

```py
M = np.dot(X, self.X_train.T)
te = np.square(X).sum(axis = 1) #水平延伸，对3072维数据进行平方后求和
tr = np.square(self.X_train).sum(axis = 1)
dists = np.sqrt(-2*M+tr+np.matrix(te).T)
```

其中最后一步公式转换成相同维度可以相加的过程，被broadcast特性给省略了。

矩阵大小为：
```py
    ('X.type', <type 'numpy.ndarray'>)
    ('X.shape = ', (500L, 3072L))
    ('self.X_train.shape = ', (5000L, 3072L))
    ('M.shape = ', (500L, 5000L))
    ('te.shape = ', (500L,))
    ('tr.shape = ', (5000L,))
    ('dists.shape = ', (500L, 5000L))
```