# 随机采样一致

RANSAC： **RAN**dom **SA**mple **C**onsensus 

Reference ：https://zhuanlan.zhihu.com/p/45532306

最基础的RANSAC包括五个步骤：

1.  从所有原始数据中随机选取一个最小子集(如果求解PNP问题，那么显然可以选取3个点(P3P)。但是我使用的是dlt方式，所以选取的是6个点,这种方式**仅供参考，**不过应该没什么问题，OpenCV中solvePnPRansac()也可以选择Epnp解法，需要4个点对)。
2.  使用这个子集拟合一个模型(本文中就是通过DLT求解PNP问题)。
3.  根据上一步求解出的模型去判断所有数据，将数据分为outlier(外点)和inlier(内点)，如果内点太少，那么返回第一步。其中本文求解的是PNP问题，使用的是重投影误差来判断内外点。
4.  根据划分出的所有内点，重新拟合模型。
5.  根据最新拟合出的模型，判断内外点数目，如果达到最终要求，那么跳出循环，如果内点太少，那么返回第一步。如果内点数目有增加但是还没有达到最终要求，那么返回第二步。

## PnP算法

Perspective-n-Point 

PnP求解算法是指通过多对3D与2D匹配点，在已知或者未知相机内参的情况下，利用最小化重投影误差来求解相机外参的算法。PnP求解算法是SLAM前端位姿跟踪部分中常用的算法之一，一般的方法包括P3P、DLT、EPnP、 ...
