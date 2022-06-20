[TOC]





# HRNet



reference: https://zhuanlan.zhihu.com/p/60535056

https://blog.csdn.net/weixin_37993251/article/details/88043650



# SOLO

https://www.zhihu.com/question/360594484/answer/936591301



Instance seg 和 semantic seg 的区别就不用多说了。完全不同的两个任务，难度也不一样。

本文思考的一个点是：*What are the fundamental differences between object instances in an image?*

感觉下来，这两个任务是完全不同的，看起来都是分割，但一个是像素级别的分类（bottom-并没有up），一个是类似检测的任务，定位。（top-down）。引用原作者知乎里的一句话：图片里物体的什么属性可以用来区分实例呢？—— 位置和形状



![截屏2022-06-20 15.41.46](../../material/%E6%88%AA%E5%B1%8F2022-06-20%2015.41.46.png)

基本思想就是一个分支用来预测类别，其中feature map被划分成 s\*s 个格子，每个格子负责预测在范围内的object类别。另一个分支用来做instance seg，其中，每个channel代表一个instance，总数也是 s\*s，和上边spatial上的分块一一对应。这样既能得到instance级别的分割，又能得到该instance的类别。上面分支负责位置（顺便给出类别），下面分支负责细节的形状。



评价：

最大的贡献是框架上的，作者把实例分割问题转化为分类问题，利用location去约束label-assign过程

# SOLOv2

mask generation is decoupled into a mask kernel prediction and mask feature learning

其实就是一个dynamic conv

![截屏2022-06-20 15.50.40](../../material/%E6%88%AA%E5%B1%8F2022-06-20%2015.50.40.png)

其他的形如，matrix NMS，值得研究一下。

# CondInst