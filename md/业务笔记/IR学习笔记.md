IR学习笔记

时间线：

2020.4.1 – 2020.7.1 https://fr-discourse.iap.wh-a.brainpp.cn/t/topic/7569/2

2020.7.1 – 2020.12.1 ：

1. domain transfer 方差和均值 可以尝试 https://fr-discourse.iap.wh-a.brainpp.cn/t/topic/6659/28

   1. 顺便可以探究 模长和方差是否有相关性。（出发点在于归一化的时候这两个操作都是乘法，那是否方差大部分来源都是模长呢）

2. 「RGB1 IR1 RGB2 IR2 」这样四元组的认知，看模型对于对齐图像的风格差异（IR/RGB）和 相同风格类内差异（同一ID两张图片）到底是多少 https://fr-discourse.iap.wh-a.brainpp.cn/t/topic/6659/30

   1. 结论是 风格差距< 图片内容差距< 风格差距+图片内容差距
      RGB VS RGB的类内距离均值 
   2. IR vs IR 并不比RGB VS RGB的类内聚集性能要差
   3. 目的是怎么让模型对 「对齐图像」的feature差异降到最低。

3. IR数据探索

   1. done

4. RGB和IR的切换条件可能不在光线的因素上，而是在图像质量指数上面。

   1. 暗光IR大部分比RGB好很多，但容易出现过曝，而逆光，正常光则差于RGB

5. 长短feature 对IR的影响是不一样的。

6. Scale STN

7. double feature 类似我们之前思考的后处理 统计IR和RGB的类平均中心，两种风格的中心作为后处理residual ，这里考虑的是模型学习一个residual出来。

   1. 结论： 红外和口罩 residual 比较大，而其他BMK上面（安防和通行场景，中国人采集以及外国人跨年龄）这种差距并不是很大。 比较特殊奇怪的女性真实场景下去掉residual支路的feature的效果是最好的。（噪声？）

8. IR RGB 特征融合

9. merge：

   1. 最终merge结果

      设置：stage1使用新TA+N-pair dis策略 + teaRGBdisIR + STN-scale; stage2使用IRdp训练; stage3冻住主干网络使用dual feature训练。最终相比于Nemo模型，**红外Recall涨点6.51个点，风险点在于FPR从千三涨点到千六**。

10. HR to LR 蒸馏实验

11. 切换策略包络线没看懂。。

12. 风格矩阵对齐 gram矩阵

13. IR数据相关性研究

    1. 感觉是种更科学的方式，统计不同dataset在loss上的表现

14. ensemble teacher蒸馏

    1. 不考虑

15. 双目融合

16. feature 插值

    1. 关于插值后renorm的问题，为什么renorm不影响是个二次曲线的事实？
       1. 猜测1，模长基本没变
       2. 猜测2，非严格norm，所以非线性的部分改变不大。
    2. 为什么高维空间插值最近点发生在IR-RGB线段上，而不是端点上 https://fr-discourse.iap.wh-a.brainpp.cn/t/topic/6659/90
       1. 高维空间特性？
       2. 可以观察一下norm之后的feature，每个维度是不是还满足高斯？
       3. 猜测：feature embedding 本身的问题，base并不在IR-RGB向量的延长线上
       4. 是否IR-RGB融合后类间也会变化

17. double feature （支路实验）

    1. 明显涨点，相复现

18. 双支路实验（IR， 口罩）





Q&A:

1. 模长和方差是否有相关性？
2. 最work的方法是？
3. 双目融合，输入融合为啥也是✅形状曲线？







action

1. IRRGB 融合 RGB RGB融合的区别
   1. 不同模型肯能结论不一样 （端上IR和base拉过）







1. IR落地
2. hard mining
3. backbone探索
4. 业务支持-面板机识别
5. 工程开发
6. snapxxx
7. 算法量产交付
8. 算法量产开发
9. 女性优化
10. 遮挡优化
11. 地铁
12. mBG识别
13. w5k属性
14. MGB属性
15. 数据工作