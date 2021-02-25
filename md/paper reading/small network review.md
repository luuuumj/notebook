small network review

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
  * mix up kernel size
* AdderNet
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