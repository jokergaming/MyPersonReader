> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/xbinworld/article/details/104723588)

> 我们平时ML任务的时候，用的最多的是cross entropy loss或者MSE loss。需要有一个明确的目标，比如一个具体的数值或者是一个具体的分类类别。但是ranking loss实际上是一种metric learning,他们学习的相对距离，相关关系，而对具体数值不是很关心。ranking loss 有非常多的叫法，但是他们的公式实际上非常一致的。大概有两类，一类是输入pair 对，另外一种是输入三元组结构。

### 文章目录

*   [1. Contrastive Loss (对比 loss)](#1_Contrastive_Loss_loss_6)
*   [2. Triplet Loss（三元 loss）](#2_Triplet_Lossloss_19)
*   [3. Focal Loss[5]](#3_Focal_Loss5_54)
*   *   [3.1 引申讨论：其他形式的 Focal Loss](#31_Focal_Loss_75)
*   [参考资料](#_95)

本文记录一下三种常用的 loss function：Contrastive Loss，Triplet Loss，Focal Loss。其中前面两个可以认为是 ranking loss 类型，Focal Loss 是针对正负样本极其不均衡情况下的一种 cross entropy loss 的升级版。本文主要参考资料是 [2][3][5]。

我们平时 ML 任务的时候，用的最多的是 cross entropy loss 或者 MSE loss。需要有一个明确的目标，比如一个具体的数值或者是一个具体的分类类别。但是 ranking loss 实际上是一种 metric learning, 他们学习的相对距离，相关关系，而对具体数值不是很关心。ranking loss 有非常多的叫法，但是他们的公式实际上非常一致的。大概有两类，一类是输入 pair 对，另外一种是输入三元组结构。

1. Contrastive Loss (对比 loss)
=============================

![](https://img-blog.csdnimg.cn/20200314134344769.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hiaW53b3JsZA==,size_16,color_FFFFFF,t_70#pic_center)

在孪生神经网络（siamese network）中，其采用的损失函数是 contrastive loss，这种损失函数可以有效的处理孪生神经网络中的 paired data 的关系（形式上并不一定是两个 Net，也可以是一个 Net 两个 Out，可以认为上面示意图的 Network1 和 2 是同一个，或者不是同一个）。contrastive loss 的表达式如下：  
L = 1 2 N ∑ n = 1 N y d 2 + ( 1 − y ) m a x ( m a r g i n − d , 0 ) 2 L=\frac{1}{2N}\sum_{n=1}^Nyd^2+(1-y)max(margin-d,0)^2 L=2N1​n=1∑N​yd2+(1−y)max(margin−d,0)2

其中 d = ∣ ∣ a n − b n ∣ ∣ 2 d=||a_n - b_n||_2 d=∣∣an​−bn​∣∣2​，代表两个样本特征的欧氏距离，y 为两个样本是否匹配的标签，y=1 代表两个样本相似或者匹配，y=0 则代表不匹配，margin 为设定的阈值。这种损失函数最初来源于 Yann LeCun 的 Dimensionality Reduction by Learning an Invariant Mapping，主要是用在降维中，即本来相似的样本，在经过降维（特征提取）后，在特征空间中，两个样本仍旧相似；而原本不相似的样本，在经过降维后，在特征空间中，两个样本仍旧不相似。

观察上述的 contrastive loss 的表达式可以发现，这种损失函数可以很好的表达成对样本的匹配程度，也能够很好用于训练提取特征的模型。当 y=1（即样本相似）时，损失函数只剩下 ∑ y d 2 \sum yd^2 ∑yd2，即原本相似的样本，如果在特征空间的欧式距离较大，则说明当前的模型不好，因此加大损失。而当 y=0 时（即样本不相似）时，损失函数为 ∑ ( 1 − y ) m a x ( m a r g i n − d , 0 ) 2 \sum (1-y)max(margin-d,0)^2 ∑(1−y)max(margin−d,0)2，即当样本不相似时，其特征空间的欧式距离反而小的话，损失值会变大，这也正好符号我们的要求。其中 margin 是一个超参，相当于是给 loss 定了一个上届（margin 平方），如果 d 大于等于 margin，那么说明已经优化的很好了，loss=0 了。

这张图表示的就是损失函数值与样本特征的欧式距离之间的关系，其中红色虚线表示的是相似样本的损失值，蓝色实线表示的不相似样本的损失值。  
![](https://img-blog.csdnimg.cn/20200307222715111.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hiaW53b3JsZA==,size_16,color_FFFFFF,t_70#pic_center)

2. Triplet Loss（三元 loss）
========================

Triplet loss 最初是在 FaceNet: A Unified Embedding for Face Recognition and Clustering 论文中提出的，可以学到较好的人脸的 embedding。

Softmax 是确定的分类，需要有真实的标注 label。而有的时候我们不一定知道 label，但是知道正样本对和负样本对——比如两张照片是同一个人，或者不是同一个人。

![](https://img-blog.csdnimg.cn/20200307223618510.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hiaW53b3JsZA==,size_16,color_FFFFFF,t_70#pic_center)  
输入是一个三元组 <a, p, n>

*   a： anchor，表示一个基准样本
*   p： positive, 与 a 是同一类别的样本，比如就是同一个人的照片
*   n： negative, 与 a 是不同类别的样本，比如就是不同人的照片

**Triplet Loss 形式**：  
L = m a x ( d ( a , p ) − d ( a , n ) + margin , 0 ) L=max(d(a,p)−d(a,n)+\text{margin},0) L=max(d(a,p)−d(a,n)+margin,0)

其中 d d d 表示距离函数，一般指在 Embedding 下的欧式距离计算。很显然，Triplet-Loss 是希望让 a 和 p 的距离尽可能小，而 a 和 n 的距离尽可能大，但是具体而言 d ( a , p ) d(a,p) d(a,p) 和 d ( a , n ) d(a,n) d(a,n) 的数值是多少，并没有规定，只要考察他们之间的相对距离。网上有一张图片说明了几种相对关系。

如果我们给定了一个 a 和 p，以及参数 margin > 0 \text{margin}>0 margin>0，那么我们就可以考察 negative 点的位置，会出现三种 case（如果是 Easy negative 的三元组我们叫做 Easy triplet，其他类似）：

*   Easy negatives（绿色区域）： L = 0 L=0 L=0 即 d ( a , p ) + margin ≤ d ( a , n ) d(a,p)+\text{margin} \leq d(a,n) d(a,p)+margin≤d(a,n)，这种情况不需要优化（无法优化，Loss 为 0），天然 a, p 的距离很近， a, n 的距离远。
*   hard negatives（红色区域）： d ( a , n ) ≤ d ( a , p ) d(a,n) \leq d(a,p) d(a,n)≤d(a,p)，也就是说 negative 点反而比较近，说明距离估计的不准，这个时候 loss 比较大。
*   Semi-hard negatives（橙色区域）： d ( a , p ) < d ( a , n ) < d ( a , p ) + margin d(a,p)\lt d(a,n)\lt d(a,p)+\text{margin} d(a,p)<d(a,n)<d(a,p)+margin, 即 a, n 的距离靠的很近，但是因为我们有一个 margin，使得 loss 依然是正的。这种情况下，其实是说在这个三元组里面比较 p 和 n 离 a 的距离差不多，比较容易混淆。

FaceNet 论文中是随机选取 semi-hard triplets 进行训练的, （也可以选择 hard triplets 或者两者一起进行训练）  
![](https://img-blog.csdnimg.cn/20200308113901898.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hiaW53b3JsZA==,size_16,color_FFFFFF,t_70#pic_center)

**在线训练时产生样本**：  
虽然可以离线把 triplet 数据都产生（配对）好，但实际使用采用此方法，即在线对一个 Batch 去产生。产生时又分为两种策略 Batch All 和 Batch Hard （是在一篇行人重识别的论文中提到的 [7]，假设一个 batch 中有 B = P K B=PK B=PK 张图片, 其中 P P P 个身份的人，每个身份的人 K K K 张图片（比如 K = 4 K=4 K=4）。

*   Batch All：计算 batch_size 中所有 valid 的 hard triplet 和 semi-hard triplet（valid 是指 a,p,n 三个都不能相同，需要是不同图片）， 然后取平均得到 Loss。理论上最多可以产生 P K ( K − 1 ) ( P K − K ) PK(K−1)(PK−K) PK(K−1)(PK−K) 个 triplets：PK 个 anchor，K-1 个 positive，PK-K 个 negative。但是因为很多是 easy triplets 的情况，所以平均会导致 Loss 很小，easy triplets 对我们是不需要的。所以是对所有 valid 的 hard triplet 和 semi-hard triplet 对求平均。
    
*   Batch Hard：对于每一个 anchor，选择距离最大的 d ( a , p ) d(a, p) d(a,p) 和距离最大的 d ( a , n ) d(a, n) d(a,n)，所以只有 P K PK PK 个三元组 triplets 来求 loss。
    

3. Focal Loss[5]
================

Focal Loss 的提出是用来解决一阶段目标检测算法面对的极端不平衡前景和背景目标（框）数量，作者表示可能有 1：1000。原论文主要是关心处理简单的二分类问题，即前景背景检测，但是 loss 本身也非常容易扩展到多分类问题。

**基本的交叉熵 loss**（针对一个样本，其中 p t p_t pt​表示模型的输出概率，且只要针对 ground truth 那个类别的输出概率）：

CE ( p t ) = − log ⁡ ( p t ) \text{CE}(p_t) = -\log(p_t) CE(pt​)=−log(pt​)

**α-balanced focal lossloss**：

F L ( p t ) = − α t ( 1 − p t ) γ log ⁡ ( p t ) FL(p_t) = -\alpha_t (1-p_t)^{\gamma}\log (p_t) FL(pt​)=−αt​(1−pt​)γlog(pt​)

其中，我们发现在原来的 cross entropy loss 基础上，加上了一个调节因子 ( 1 − p t ) γ (1-p_t)^{\gamma} (1−pt​)γ， γ ≥ 0 \gamma \geq 0 γ≥0，这个调节因子的作用是：

*   When an example is misclassified and pt  
    is small, the modulating factor is near 1 and the loss is unaffected. As p t → 1 p_t \rightarrow 1 pt​→1, the factor goes to 0 and the loss for well-classified examples is **down-weighted**
*   The focusing parameter γ \gamma γ smoothly adjusts the rate at which easy examples are downweighted.

实验中发现，一般 γ = 2 \gamma = 2 γ=2 是最有效的。这个调节因子可以让容易分的样本的 loss 降低重要性。比如，当 γ = 2 \gamma = 2 γ=2，以及 p t = 0.9 p_t = 0.9 pt​=0.9，那么这个样本就是比较容易分对，它的 loss 较 Cross Entropy Loss 就降低了 100 倍；而 p t = 0.968 p_t = 0.968 pt​=0.968，loss 就小了 1000 倍；而对 p t = 0.5 p_t = 0.5 pt​=0.5 的难分样本，loss 只是降低了 4 倍，相当于重要性变大了。另外，还多了一个 weighting factor α ∈ [ 0 , 1 ] \alpha \in [0,1] α∈[0,1]，作者表示：In practice α \alpha α may be set by inverse class frequency or treated as a hyperparameter to set by cross validation。实际在实验中，作者是看成一个超参的，需要调节，比如用 0.25, 0.5, 0.75 都有试过。如果二分类，就设 α t \alpha_t αt​ for class 1 and 1 − α t 1−\alpha_t 1−αt​ for class −1。

3.1 引申讨论：其他形式的 Focal Loss
-------------------------

作者提出，实际上并不只是上面的 Focal Loss 可以有效，应该存在一批定义方法，他们的效果是类似的。在 [5] 附录中作者又给出了一种 Focal Loss * 的定义方法：

我们定义： x t = y x x_t = yx xt​=yx

其中， y ∈ { − 1 , + 1 } y \in \{-1,+1 \} y∈{−1,+1} 表示样本分对 or 分错。给出了一种新的 loss 形式：: Focal Loss*

p t ∗ = σ ( γ x t + β ) F L ∗ = − log ⁡ ( p t ∗ ) / γ p_t^* = \sigma(\gamma x_t + \beta)\\ FL^* = -\log (p_t^*)/\gamma pt∗​=σ(γxt​+β)FL∗=−log(pt∗​)/γ

下图是本文中几种 loss 曲线，以及他们的梯度：  
![](https://img-blog.csdnimg.cn/20200308211616115.JPG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hiaW53b3JsZA==,size_16,color_FFFFFF,t_70)  
![](https://img-blog.csdnimg.cn/20200308211655811.JPG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hiaW53b3JsZA==,size_16,color_FFFFFF,t_70)  
具体的梯度公式推导是：

![](https://img-blog.csdnimg.cn/2020030821175728.JPG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hiaW53b3JsZA==,size_16,color_FFFFFF,t_70)  
最后作者给出的结果是，几种 Focal Loss 的作用差不了太多，但是比原始的 Cross Entropy 显著要好一些。作者认为和 Focal Loss 有相同作用的其他 loss 应该是等效效果的。

参考资料
====

[1] [Triplet Loss, Ranking Loss, Margin Loss](https://zhuanlan.zhihu.com/p/101143469)  
[2] [Contrastive Loss (对比损失)](https://blog.csdn.net/autocyz/article/details/53149760)  
[3] [Triplet-Loss 原理及其实现、应用](https://blog.csdn.net/u013082989/article/details/83537370)  
[4] Loss Rank Mining: A General Hard Example Mining Method for Real-time Detectors  
[5] Focal Loss for Dense Object Detection  
[6] FaceNet: A Unified Embedding for Face Recognition and Clustering  
[7] In Defense of the Triplet Loss for Person Re-Identification  
[8] [文献阅读 - Dimensionality Reduction by Learning an Invariant Mapping](https://blog.csdn.net/zhaoyin214/article/details/94396243)  
[9] [目标检测 focal loss 和 loss rank mining 笔记](https://blog.csdn.net/xiaozhshi/article/details/83547490)