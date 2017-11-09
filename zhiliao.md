摘要
==
1.==NatureDeepReview ==   LetNet5* 
==目标分类：输入图像以及期望输出(此处是图像对应的标签)==
参考博客：深度学习-LeCun、Bengio和Hinton的联合综述
http://www.csdn.net/article/2015-06-01/2824811
卷积神经网络**4**个关键想法：
a.局部链接
b.全值共享
c.池化 
d.多网络层的使用

2.==imagenet-classification-with-deep-convolutional-neural-networks==  *AlexNet*
参考博客：Alexnet的网络结构 讲解了数据转换过程
==目标分类：输入图像以及期望输出(此处是图像对应的标签)==
http://blog.csdn.net/langb2014/article/details/48286501
参考博客：mageNet Classification with Deep Convolutional Neural Networks（译文）转载 文章的翻译
http://www.cnblogs.com/zf-blog/p/6432709.html
核心思想：
该神经网络有6000万个参数和650,000个神经元，由五个卷积层，以及某些卷积层后跟着的max-pooling层，和三个全连接层，还有排在最后的1000-way的softmax层组成。==为了使训练速度更快，我们使用了非饱和的神经元和一个非常高效的GPU关于卷积运算的工具。为了减少全连接层的过拟合，我们采用了最新开发的正则化方法，称为“dropout”==
介绍该网络体系结构的一些新颖独特的功能**4**条：
a.ReLU非线性:称这种不饱和非线性的神经元为修正线性单元,可加速训练
b.在多个GPU上训练：采用的并行方案基本上是在每个GPU中放置一半核（或神经元），还有一个额外的技巧：GPU间的通讯只在某些层进行
c.局部响应归一化:局部归一化方案有助于一般化
d.重叠Pooling

3.==RCNN==
参考博客：
Rich feature hierarchies for accurate object detection and semantic segmentation（阅读翻译）丰富多级特征用于精准对象检测和语义分割
http://www.cnblogs.com/lucky3206/p/6591637.html
