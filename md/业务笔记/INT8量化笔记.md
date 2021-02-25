# INT8量化笔记

参考：[知乎量化1](https://zhuanlan.zhihu.com/p/58182172?utm_source=wechat_session&utm_medium=social) [知乎量化2](https://zhuanlan.zhihu.com/p/58208691)

量化做的事情就是将float32 变成int8

$$  def: scale = (x_{max}-x_{min})/(256-1)$$

$$def: Quant(x_{fp}) = x_{fp}*1/scale -> x_{int}$$

$$ def: DeQuant(x_{int}) = x_{int} * scale -> x_{fp} $$

规定：

conv的输入是

Input: int8(带自己的scale，S1)

W: Int8(带自己的scale，S2)

b：Int8x8x32(带自己的scale，S1*S2)

conv的输出是
Output: float (Input * W * S1 * S2 + b* S1* S2)

astype 成int8(带自己的scale，S3)

迭代