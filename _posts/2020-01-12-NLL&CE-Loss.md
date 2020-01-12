

# NLLLoss 和 CE Loss 区别

**损失函数NLLLoss()** 的 输入 是一个对数概率向量和一个目标标签. 它不会为我们计算对数概率，**适合最后一层是log_softmax()的网络.** 损失函数 CrossEntropyLoss() 与 NLLLoss() 类似, 唯一的不同是它为我们去做 softmax.可以理解为：

CrossEntropyLoss()=log_softmax() + NLLLoss() 

 

CrossEntropyLoss 和 NLLLoss 区别就是有没有做softmax

如果网络最后一层已经对输出做了softmax，则可以用NLLLoss ，否则就用交叉熵损失。

 

本质是一样的，基本上都是用交叉熵损失来进行分类任务的loss计算，只是实现上有的喜欢分开实现，分两步骤。

 

对于网络最后一层已经做了softmax的框架

def cal_loss(pred, gold, smoothing=True):

  ''' Calculate cross entropy loss, apply label smoothing if needed. '''

  gold = gold.contiguous().view(-1)

  if smoothing:

​    eps = 0.2

​    n_class = pred.size(1)

​    one_hot = torch.zeros_like(pred).scatter(1, gold.view(-1, 1), 1)

​    print(one_hot)

​    one_hot = one_hot * (1 - eps) + (1 - one_hot) * eps / (n_class - 1)

​    \#log_prb = F.log_softmax(pred, dim=1)

​    log_prb = pred

​    loss = -(one_hot * log_prb).sum(dim=1).mean()

  else:

​    loss = F.cross_entropy(pred, gold, reduction='mean')

  return loss

 

用这部分作为标签平滑的计算

1.背景介绍：

在多分类训练任务中，输入图片经过神级网络的计算，会得到当前输入图片对应于各个类别的置信度分数，这些分数会被softmax进行归一化处理，最终得到当前输入图片属于每个类别的概率。

 

 

之后在使用交叉熵函数来计算损失值：

 

 

最终在训练网络时，最小化预测概率和标签真实概率的交叉熵，从而得到最优的预测概率分布。在此过程中，为了达到最好的拟合效果，最优的预测概率分布为：

 

 

也就是说，网络会驱使自身往正确标签和错误标签差值大的方向学习，在训练数据不足以表征所以的样本特征的情况下，这就会导致网络过拟合。

 

2.label smoothing原理

label smoothing的提出就是为了解决上述问题。最早是在Inception v2中被提出，是一种正则化的策略。其通过"软化"传统的one-hot类型标签，使得在计算损失值时能够有效抑制过拟合现象。如下图所示，label smoothing相当于减少真实样本标签的类别在计算损失函数时的权重，最终起到抑制过拟合的效果。

 

 

1.label smoothing将真实概率分布作如下改变：

 

 

其实更新后的分布就相当于往真实分布中加入了噪声，为了便于计算，该噪声服从简单的均匀分布。

 

2.与之对应，label smoothing将交叉熵损失函数作如下改变：

 

 

3.与之对应，label smoothing将最优的预测概率分布作如下改变：

 

 

阿尔法可以是任意实数，最终通过抑制正负样本输出差值，使得网络能有更好的泛化能力。

————————————————

版权声明：本文为CSDN博主「yuanCruise」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。

原文链接：https://blog.csdn.net/qiu931110/article/details/86684241