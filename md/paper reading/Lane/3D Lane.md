

[TOC]

# Monocular BEV Perception with Transformers in Autonomous Driving

https://towardsdatascience.com/monocular-bev-perception-with-transformers-in-autonomous-driving-c41e4a893944

## note:

Over the past year, largely three approaches emerged in monocular BEV perception.

-   **IPM:** This is the simple baseline based on the assumption of a flat ground. [Cam2BEV](https://arxiv.org/abs/2005.04078) is perhaps not the first work to do this but is a fairly recent and relevant work. It uses IPM to perform the feature transformation, and uses CNN to correct the distortion of 3D objects that are not on the 2D road surface.
-   **Lift-splat**: Lift to 3D with monodepth estimation and splat on BEV. This trend is initiated by [Lift-Splat-Shoot](https://arxiv.org/abs/2008.05711), and many follow-up works such as [BEV-Seg](https://arxiv.org/abs/2006.11436), [CaDDN](https://arxiv.org/abs/2103.01100), and [FIERY](https://arxiv.org/abs/2104.10490).
-   **MLP**: Use MLP to model the view transformation. This line of work is initiated by [VPN](https://arxiv.org/abs/1906.03560), and [Fishing Net](https://arxiv.org/abs/2006.09917), and [HDMapNet](https://arxiv.org/abs/2107.06307) to follow.
-   **Transformers**: Use **attention**-based transformers to model the view transformation. Or more specifically, **cross-attention** based transformer module. This trend starts to show initial traction as transformers take the computer vision field by storm since mid-2020 and at least till this moment, as of late-2021.







# 3D-LaneNet: End-to-End 3D Multiple Lane Detection

利用IPM直接变换，such a direct transformation depends on the precision of camera parameters, and the scale of feature might be distorted due to the lack of accurate per-pixel depth information.



# Gen-LaneNet: A Generalized and Scalable Approach for 3D Lane Detection

# PersFormer: 3D Lane Detection via Perspective Transformer and the OpenLane Benchmark

## Data

note：

1.  车道线会标注visibility，shape是(n,1)，n代表点的个数，category，以及uv，xyz，attri，track_id， 值得借鉴：https://github.com/OpenPerceptionX/OpenLane/tree/main/anno_criterion/Lane
2.  原始数据是waymo的公开数据集，但waymo原始并没有标车道线，st自己标了一遍。



## Related work

**Vision Transformer in BEV**

1.  cross attention introduced to serve as a learnable transformation of features is successful instead of simply projecting features via IPM
2.  3D-LaneNet : IPM, such a direct transformation depends on the precision of camera parameters(noise), and the scale of feature might be distorted due to the lack of accurate per-pixel depth information
3.  DETR3D: a learnable 3dto2d query search, No explicit BEV modeling for robust feature representation. 

**Lane Detection Benchmarks**

*   Disvantange: 只有seg，没有lane； 场景单一简单，只有高速； 单帧，没有时序；没有track id等；

**2D Lane Detection**

*   不再赘述

**3D Lane Detection**

*   such as a stereo camera or LiDAR, to get the 3D ground topology. However, these sensors have shortages of high cost in hardware and computation resources
*   some monocular methods take a single image and employ IPM to predict lanes in 3D space



## PersFormer

framework: 其实没什么好说的，主要是利用cross-attention，将naive的IPM做了一些改进，得到IPM based Cross Attention（前面还有self-attention）最后decode成BEV feature，再同时接2d和3d的head（其实就是detr3d+deformable）

![截屏2022-04-07 15.03.24](../../material/%E6%88%AA%E5%B1%8F2022-04-07%2015.03.24.png)

关键的点在于cross-attention是怎么做的，如果直接利用detr里面的cross-attention， Q和K出来的attention和V构成的pair（文中叫key-value）会变得比较大，因此使用了deformable detr。cross-attention的时候 V只和某些相关的points 产生attention

![截屏2022-04-07 15.03.32](../../material/%E6%88%AA%E5%B1%8F2022-04-07%2015.03.32.png)





# Laneformer

paper：https://arxiv.org/pdf/2203.09830.pdf

## Related work

LaneATT. However, the fixed attention routines can not adaptively fit the shape characteristic of lanes. 

## Framework

真正利用了attention is all your need![截屏2022-06-21 15.49.38](../../material/%E6%88%AA%E5%B1%8F2022-06-21%2015.49.38.png)

整个框架分为4个模块：

1.  CNN backbone to extract basic feature
2.  detection  processing module to handle person-vehicle dection
3.  special encoder
4.  special decoder



We further get row features and column features by collapsing the column dimension and row dimension of backbone features,

>   看起来整个框架并没有什么骚操作，就是一堆attention 或者feedforward 之类的，那么同等计算量的cnn和值相比性价比如何呢？如果确实work，那么本质的操作是啥呢。