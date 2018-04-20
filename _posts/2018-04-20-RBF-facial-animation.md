---
layout: post
title: RBF在人脸动画中的运用
date: 2018-4-20
categories: blog
tags: [RBF,插值]
description: 插值算法
---

标签（空格分隔）： 插值

---

## RBF怎么用于facial animation

在通过face-fit获取人脸的特征点后，需要驱动模型运动来实现facial animation。今天和师兄讨论了一下，先实现三维点的驱动，通过face-fit获取到的特征点，然后人工添加其他点，通过特征点的运动，来插值出其他点的运动。

RBF插值的目的不是插值出特征点外的其他点，而是通过特征点的运动(e.g. $(\overrightarrow{\Delta  x}，\overrightarrow{\Delta y})$)来插值出其他非特征点的运动。

这几天一直卡在特征点的运动和插值上，怎么驱动三维人脸模型运动？这个问题先放到后面解决吧，这几天把rbf弄好。

初步的想法：

1. 记录当前帧$(x_{current},y_{current},z_{current})$处相较于前一帧$(x_{pre},y_{pre},z_{pre})$的变化量，即$(\overrightarrow{\Delta  x}，\overrightarrow{\Delta y})$， 我们考虑由于变化幅度比较小，Z轴坐标不变化，$(\overrightarrow{\Delta  x}，\overrightarrow{\Delta y}) = (x_{current} - x_{pre}, y_{current} - y_{pre})$

2. 可以得到一个$(x,y)$向$(\overrightarrow{\Delta  x}，\overrightarrow{\Delta y})$的映射函数，因为考虑到人脸的运动是可以分解为(x,y)的移动，两者应该是独立的（有个专业术语叫什么来着），所以可以分别对$f(x) = \overrightarrow{\Delta  x}$ 和$f(y) = \overrightarrow{\Delta  y}$进行一次一维rbf就可以得到拟合后的rbf函数，即可预测出任意(x,y)处的位移向量，但是这个方法的局限是，(x,y)的取值范围必须覆盖预测的点，比如x，从左边脸颊到右边脸颊的点可以覆盖到，但是y，只能覆盖从下巴到眉毛的y值，所以不能用来预测额头部位的特征点位移，但是由于本模型不考虑额头，所以问题应该不大，以上都是最近的一点想法，还没有得到实际验证，近段时间尽力完成吧！！

由于跟踪的特征点的数目不是很多，所以要对面部三维曲面的点进行扩充，由于人脸具有一定的对称性，先处理半边， 可以把两个对称点看做一组，然后进行线性插值即可，步长可以根据所需要插值的点的个数确定。这样可以获取到构成三维面部模型当前帧的所有点（特征点+插值点）。
