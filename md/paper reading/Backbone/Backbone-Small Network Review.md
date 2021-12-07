# Small Network Review

## Efficient Networks:

* MobileNets 
  * Decompose kxk into a depthwise and a pointwise
* Shufflenets
  * group conv and channel shuffle
* Butterfly transform
  * approximate pointwise conv:  n**2 个edge -> nlogn edge
* EfficientNet
  * find a relationship between resolution and width/depth
* MixNet
  * mix up kernel size(1x1，3x3，5x5) ，inception-like + NAS 不是mixup aug的那个意思
* AdderNet
  * replace conv（dot product） with L1
* GhostNet
  * cheap linear operator (depthwise conv) generate ghost feature map
* Sandglass

## Efficient Inference:

* select a proper sub-network when inference
* use reinforcement learing to skip part of an existing model
* MSDNet
  * Early-exit for easy sample

## Dynamic Neural Networks:

* HyperNet ?
* SENet
  * reweight channels by squeezing global context
* SKNet?
* SCC
* Dynamic convolution
* Dynamic relu
  * relu with params  
  * adapts slopes and intercepts of two linear functions
* WeightNet
  * Group fc to generate convoluition weight
  * a general format for scc and se

## MicroNet

* circumventing the reduction of network width through lowering node connectivity :  Factorizing both pointwise and depthwise convolutions
* compensating for the reduction of network depth by improving nonlinearity per layer : Dynamic Shift-Max.

文章整体思想是

1. 降低pointwise conv的连接数（而不是降低feature map的channel数），采取的手段是
   a. 使用group conv
   b. squeeze and excitation
2. channelwise conv的优化
   a. k x k 拆分成k x 1 和 1 x k
3. 增加网络里面的非线性作为补偿
   a. group 间的shuffle（shift）
   b. 不同group取max 作为当前channel的feature map

直观感觉 Dynamic Shift-Max 会很耗时。。。