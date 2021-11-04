> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [zhuanlan.zhihu.com](https://zhuanlan.zhihu.com/p/350552901)

> 表现SOTA！性能优于ProxyNCA++、XBM等网络，结果表明，与基于卷积的方法相比，transformer具有一致且显著的改进！ Transformer杀疯了！近期又有一波视觉Transformer的工作（大都来自大厂和Top高校），比如： 谷歌…

表现 SOTA！性能优于 ProxyNCA++、XBM 等网络，结果表明，与基于卷积的方法相比，transformer 具有一致且显著的改进！

Transformer 杀疯了！近期又有一波视觉 Transformer 的工作（大都来自大厂和 Top 高校），比如：

*   [谷歌提出 ColTran：Colorization Transformer](https://zhuanlan.zhihu.com/p/350384121)
*   [TransUNet：用于医学图像分割的 Transformers 强大编码器](https://zhuanlan.zhihu.com/p/350271375)
*   [TransReID：首个基于 Transformer 的目标 Re-ID](https://zhuanlan.zhihu.com/p/350203995)

**注 1：文末附【Transformer】和【图像检索】交流群**

**注 2：整理不易，欢迎点赞，支持分享！**

**Training Vision Transformers for Image Retrieval**

![](https://pic2.zhimg.com/v2-d6278699d5c87a0d669d88b9d9732595_r.jpg)

> 作者单位：Facebook, ENS/Inria  
> 论文：[https://arxiv.org/abs/2102.05644](https://arxiv.org/abs/2102.05644)

Transformers 在自然语言理解以及最近的图像分类方面显示出了杰出的成果。

我们在这里扩展这项工作，并提出一种基于 Transformer 的图像检索方法：我们采用视觉 transformer 来生成图像描述符，并以度量学习目标训练结果模型，该度量学习目标将对比损失与差分熵正则化器相结合。

![](https://pic2.zhimg.com/v2-a4cbf452eb2049ee48631c4eea328275_r.jpg)

算法细节（建议去看原文）

![](https://pic4.zhimg.com/v2-10217cec0c3c714dd1898ead84409943_r.jpg)![](https://pic1.zhimg.com/v2-edeb15c079cb4e0e3b3f200680b714a4_r.jpg)

**主要贡献：**

![](https://pic3.zhimg.com/v2-d13e06e741cf2f064b919fa5f319455a_r.jpg)

实验结果
----

我们的结果表明，与基于卷积的方法相比，transformer 具有一致且显著的改进。

尤其是，我们的方法在 Stanford Online Product, In-Shop and CUB-200 的几个类别级别检索的公共基准上均优于最新技术。

![](https://pic3.zhimg.com/v2-bb4a9423f9aa6136b63b7188e6cdc5e2_r.jpg)![](https://pic4.zhimg.com/v2-1fa21216f467558b91fc881e7ab9308b_r.jpg)

此外，我们在 ROxford 和 RParis 上进行的实验还表明，在可比较的设置中，transformer 在特定对象检索方面具有竞争力，尤其是在短矢量表示和低分辨率图像方面。

![](https://pic1.zhimg.com/v2-0c5ab9b2e42d55a7ebbf66c760a0a3e0_r.jpg)

CVer-Transformer 交流群
--------------------

建了 CVer-Transformer 交流群！想要进 Transformer 学习交流群的同学，可以直接加微信号：**CVer6666**。加的时候备注一下：**Transformer + 学校 + 昵称**，即可。然后就可以拉你进群了。

CVer - 图像检索交流群
--------------

已建立 CVer - 图像检索微信交流群！想要进图像检索学习交流群的同学，可以直接加微信号：**CVer9999**。加的时候备注一下：**图像检索 + 学校 + 昵称**，即可。然后就可以拉你进群了。

**强烈推荐大家关注** [CVer 知乎](https://www.zhihu.com/people/cver-38)**账号和** [CVer](https://mp.weixin.qq.com/s/moRNkCaBDtYZgXwaZDXBwA) **微信公众号，可以快速了解到最新优质的 CV 论文。**

推荐阅读
----

[谷歌提出 ColTran：Colorization Transformer](https://zhuanlan.zhihu.com/p/350384121)

[TransUNet：用于医学图像分割的 Transformers 强大编码器](https://zhuanlan.zhihu.com/p/350271375)

[TransReID：首个基于 Transformer 的目标 Re-ID](https://zhuanlan.zhihu.com/p/350203995)

[泛化神器！李沐等人提出两种正则化技术：在 CV 和 NLP 均有大幅度提升](https://zhuanlan.zhihu.com/p/350067666)

[沈春华团队提出：使用条件卷积的实例和全景分割](https://zhuanlan.zhihu.com/p/349995990)

[中国成都举办！ACM MM 2021 Call for Papers](https://zhuanlan.zhihu.com/p/349901149)

[效果远超 Transformer！AAAI 2021 最佳论文 Informer：最强最快的序列预测神器](https://zhuanlan.zhihu.com/p/349800169)

[深度学习理论的最新进展](https://zhuanlan.zhihu.com/p/349733790)

[DeepMind 重新设计高性能 ResNet！无需激活归一化层](https://zhuanlan.zhihu.com/p/349575099)

[VisualSparta：首个基于 Transformer 的大规模文本到图像检索](https://zhuanlan.zhihu.com/p/349369775)

[南京大学提出 SA-Net：深度卷积神经网络的 Shuffle 注意力](https://zhuanlan.zhihu.com/p/349122099)

[CV 待解决问题！华中科大提出 OVIS：遮挡视频实例分割（数据集 + 代码）](https://zhuanlan.zhihu.com/p/349018018)

[VTN：视频 Transformer 网络](https://zhuanlan.zhihu.com/p/348849616)

[基于深度学习的图像检索最新综述：全面调研](https://zhuanlan.zhihu.com/p/348222902)

[T2T-ViT：在 ImageNet 上从头训练视觉 Transformer](https://zhuanlan.zhihu.com/p/348055832)

[深度学习中的 3 个秘密：集成，知识蒸馏和蒸馏](https://zhuanlan.zhihu.com/p/348006186)

[国防科大提出 CHPDet：任意方向的船舶检测](https://zhuanlan.zhihu.com/p/347858976)

[84.7％！BoTNet：视觉识别的 Bottleneck Transformers](https://zhuanlan.zhihu.com/p/347602463)

[港中文提出 ResLT：用于长尾识别的残差学习](https://zhuanlan.zhihu.com/p/347545960)

[没有卷积！CPTR：用于图像描述的全 Transformer 网络](https://zhuanlan.zhihu.com/p/347510279)

[华为诺亚提出：AdderNet 及其极简硬件设计](https://zhuanlan.zhihu.com/p/347270442)

[龙泉寺贤超法师：用 AI 为古籍经书识别、断句、翻译](https://zhuanlan.zhihu.com/p/347237973)

[北邮提出 PCA-Net：用于细粒度视觉分类的渐进式协同注意力网络](https://zhuanlan.zhihu.com/p/347001395)

[SSTVOS：基于稀疏时空 Transformers 的视频目标分割网络](https://zhuanlan.zhihu.com/p/346827146)

[南加大和 Intel 提出：基于注意力的图像上采样](https://zhuanlan.zhihu.com/p/346708849)

[攻下 SLAM！用于无监督视觉里程表的 Transformer 引导几何模型](https://zhuanlan.zhihu.com/p/346545778)

[没有自然图像的预训练 | ACCV 2020 最佳论文提名奖](https://zhuanlan.zhihu.com/p/346488531)

[基于深度学习的行人重识别 (Re-ID) 综述：全面调研（2015-2020）](https://zhuanlan.zhihu.com/p/346361954)

[2020 年最先进的 3D 医学图像分割方法](https://zhuanlan.zhihu.com/p/346246175)

[Focal-EIOU Loss：用于精确边界框回归的高效 IOU 损失](https://zhuanlan.zhihu.com/p/346006319)

[旷视提出 Momentum^2 Teacher：用于自监督学习的具有动量统计的动量老师](https://zhuanlan.zhihu.com/p/345762425)

[计算机视觉中的 Transformer](https://zhuanlan.zhihu.com/p/345557138)

[医学图像语义分割最佳方法的全面比较：U-Net 和 U-Net++](https://zhuanlan.zhihu.com/p/344658591)

[生成对抗 U-Net：实现 Domain-free 医学图像增广](https://zhuanlan.zhihu.com/p/344479363)

[80GB 医学影像数据集发布！OCTA-500 公开下载](https://zhuanlan.zhihu.com/p/344381755)

[RepVGG：使 VGG 样式的 ConvNets 再次出色](https://zhuanlan.zhihu.com/p/343660471)

[涨点神器！SoftPool：一种新的池化方法，带你起飞，代码已开源！](https://zhuanlan.zhihu.com/p/343481363)

[又一篇视觉 Transformer 综述来了！](https://zhuanlan.zhihu.com/p/341995737)

[深度神经网络中的池化方法：全面调研（1989-2020）](https://zhuanlan.zhihu.com/p/341820742)

[涨点神器！IC-Conv：具有高效空洞搜索的 Inception 卷积](https://zhuanlan.zhihu.com/p/340482046)

[一文快速回顾 U-Net Family](https://zhuanlan.zhihu.com/p/339934172)

[冠军解决方案！用于脑肿瘤分割的 nnU-Net 改进](https://zhuanlan.zhihu.com/p/325932805)