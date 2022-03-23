

**A Survey on Visual Transformer** https://arxiv.org/pdf/2012.12556.pdf

**Transformers in Vision: A Survey** https://arxiv.org/pdf/2101.01169.pdf

* https://zhuanlan.zhihu.com/p/341995737



* [ ] **Deformable - DETR**
* [ ] **ViT** https://arxiv.org/pdf/2006.03677.pdf
* [ ] MLP-Mixer https://arxiv.org/pdf/2105.01601.pdf  
* [ ] ConvMixer
* [ ] local self attention 
* [ ] 

多种VIT变体

https://zhuanlan.zhihu.com/p/452246732

-   [Vision Transformer - Pytorch](https://github.com/lucidrains/vit-pytorch#vision-transformer---pytorch)
-   [Install](https://github.com/lucidrains/vit-pytorch#install)
-   [Usage](https://github.com/lucidrains/vit-pytorch#usage)
-   [Parameters](https://github.com/lucidrains/vit-pytorch#parameters)
-   [Distillation](https://github.com/lucidrains/vit-pytorch#distillation)
-   [Deep ViT](https://github.com/lucidrains/vit-pytorch#deep-vit)
-   [CaiT](https://github.com/lucidrains/vit-pytorch#cait)
-   [Token-to-Token ViT](https://github.com/lucidrains/vit-pytorch#token-to-token-vit)
-   [CCT](https://github.com/lucidrains/vit-pytorch#cct)
-   [Cross ViT](https://github.com/lucidrains/vit-pytorch#cross-vit)
-   [PiT](https://github.com/lucidrains/vit-pytorch#pit)
-   [LeViT](https://github.com/lucidrains/vit-pytorch#levit)
-   [CvT](https://github.com/lucidrains/vit-pytorch#cvt)
-   [Twins SVT](https://github.com/lucidrains/vit-pytorch#twins-svt)
-   [CrossFormer](https://github.com/lucidrains/vit-pytorch#crossformer)
-   [RegionViT](https://github.com/lucidrains/vit-pytorch#regionvit)
-   [NesT](https://github.com/lucidrains/vit-pytorch#nest)
-   [MobileViT](https://github.com/lucidrains/vit-pytorch#mobilevit)
-   [Masked Autoencoder](https://github.com/lucidrains/vit-pytorch#masked-autoencoder)
-   [Simple Masked Image Modeling](https://github.com/lucidrains/vit-pytorch#simple-masked-image-modeling)
-   [Masked Patch Prediction](https://github.com/lucidrains/vit-pytorch#masked-patch-prediction)
-   [Adaptive Token Sampling](https://github.com/lucidrains/vit-pytorch#adaptive-token-sampling)
-   [Patch Merger](https://github.com/lucidrains/vit-pytorch#patch-merger)
-   [Vision Transformer for Small Datasets](https://github.com/lucidrains/vit-pytorch#vision-transformer-for-small-datasets)
-   [Dino](https://github.com/lucidrains/vit-pytorch#dino)
-   [Accessing Attention](https://github.com/lucidrains/vit-pytorch#accessing-attention)



# 目录：

[TOC]

## ViT

The results of ViT on image classification are encouraging, but its architecture is unsuitable for use as a general-purpose backbone network on dense vision tasks or when the input image resolution is high, due to its low-resolution feature maps and the quadratic increase in complexity with image size.

## MLP-Mixer

## ConvMixer

https://arxiv.org/pdf/2201.09792.pdf

Patches Are All You Need?

different operations replacing the self-attention and MLP operations. For example, MLP-Mixer replaces them both with MLPs applied across different dimensions

![截屏2022-03-17 17.15.09](../../material/截屏2022-03-17%2017.15.09.png)

## ConViT 

https://arxiv.org/pdf/2103.10697.pdf

### introduction

* Convolutional architectures:Their hard inductive biases enable sample-efficient learning, but come at the cost of a potentially lower performance ceiling

- ViTs: outperformed CNNs. However, they require costly pre-training on large external datasets or distillation from pretrained convolutional networks 

Soft inductive biases / Hard inductive biases 

 a self-attention based model, which has a lower floor but a higher ceiling

![屏幕快照 2021-06-17 10.53.33](../../material/屏幕快照%202021-06-17%2010.53.33.png)



### psa

$$
\boldsymbol{A}_{i j}^{h}:=\operatorname{softmax}\left(\boldsymbol{Q}_{i}^{h} \boldsymbol{K}_{j}^{h \top}+\boldsymbol{v}_{p o s}^{h \top} \boldsymbol{r}_{i j}\right)
$$

比SA来说多了一个跟位置相关的项，可以严格退化成conv（没看懂）
$$
\left\{\begin{array}{l}
\boldsymbol{v}_{\text {pos }}^{h}:=-\alpha^{h}\left(1,-2 \Delta_{1}^{h},-2 \Delta_{2}^{h}, 0, \ldots 0\right) \\
\boldsymbol{r}_{\delta}:=\left(\|\boldsymbol{\delta}\|^{2}, \delta_{1}, \delta_{2}, 0, \ldots 0\right) \\
\boldsymbol{W}_{q r y}=\boldsymbol{W}_{k e y}:=\mathbf{0}, \quad \boldsymbol{W}_{v a l}:=\boldsymbol{I}
\end{array}\right.
$$
根据原作者来说这样只能加速收敛，最高点反而还不如DeiT，于是做了改进，两项先各自softmax然后做了个gate相加，gate里面的权重是可学的。

![屏幕快照 2021-06-17 14.30.44](../../material/屏幕快照%202021-06-17%2014.30.44.png)



### local attention & global attention

reference: https://zhuanlan.zhihu.com/p/80692530

global和local的区别：



其实就是个分组，但可能会抽取不同的feature

Q：不确定分组后weights是否会共享。类似local conv的思想， 计算量不变，只变参数量？

## Swin Transformer

Abstract： different from **language to vision** : such as large variations in the scale of visual entities and the high resolution of pixels in images compared to words in text.



**Architecture：**

![屏幕快照 2021-07-15 11.38.41](../../material/屏幕快照%202021-07-15%2011.38.41.png)
**W-MSA and SW-MSA：**

$\Omega(\mathrm{MSA})=4 h w C^{2}+2(h w)^{2} C$
$\Omega(\mathrm{W}-\mathrm{MSA})=4 h w C^{2}+2 M^{2} h w C$

$\hat{\mathbf{z}}^{l}=\mathrm{W}-\mathrm{MSA}\left(\mathrm{LN}\left(\mathbf{z}^{l-1}\right)\right)+\mathbf{z}^{l-1}$
$\mathbf{z}^{l}=\mathrm{MLP}\left(\mathrm{LN}\left(\hat{\mathbf{z}}^{l}\right)\right)+\hat{\mathbf{z}}^{l}$,
$\hat{\mathbf{z}}^{l+1}=\mathrm{SW}-\mathrm{MSA}\left(\mathrm{LN}\left(\mathbf{z}^{l}\right)\right)+\mathbf{z}^{l}$
$\mathbf{z}^{l+1}=\mathrm{MLP}\left(\mathrm{LN}\left(\hat{\mathbf{z}}^{l+1}\right)\right)+\hat{\mathbf{z}}^{l+1}$

1. Hight resolution： $ H/4*W/4 $  VS  $H/16*W/16 $

2. local attention

3. patch merging 类似pooling，但用的是linear layer 来降维






## MobileVit

Zhihu: https://zhuanlan.zhihu.com/p/417649239

Paper: https://arxiv.org/pdf/2110.02178.pdf

苹果，未开源



![截屏2021-10-26 11.47.28](../../material/截屏2021-10-26%2011.47.28.png)



整个framework基本就是mbv2 嵌入了几个MobileViT的block，因此，主要解释一下MobileViT的结构

motivation： 在原始mbv2上加入一些long-range，non-local的模块，

*   一种long-range的方法是dilated convolutions，但需要careful selection of dilation rates。otherwise，weights pad 0 instead of the valid spatial region。

*   self-attention， 即ViT， heavy， sub-standard optimizability

*   MobileViT:  conv3x3+conv1x1 -> unfold (将HW展开，P是win size， P=HW/N, P一般小于conv size) -> transformer（per

     p in P） -> fold

    *   Computational cost. The computational cost of multi-headed self-attention in MobileViT and ViTs  is O(N2P d) and O(N2d)，However, in practice, MobileViT is more efficient than ViTs (Table 3; §4.3). We believe that this is because of similar reasons as for the light-weight design (discussed above)





## Q&A

1. cls token 是怎么用的？ 丢进去迭代？那inference过程是怎么样的？
   1. cls token作为参数，inference过程固定。最后取f[:,0] 或者 f.mean(1)作为feature。

