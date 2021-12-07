# Q&A:

1. fpn怎么work的
2. top-down bottom-up什么意思
    1. 全局特征，roi pooling， 框 从全局到局部 就是top-down
    2. 关键点， 聚类形成框 叫bottom-up
3. 一阶段是不是没有roi pooling?
4. 一阶段同一位置对应的不同anchor有什么区别？ 用的是同样的feat？





## FCOS

1.  IOU loss 和 smooth L1 的区别？ 以及改进版的GIOU等等
2.  为什么模型输出的centerness是conv出来的，而不是用同样的lrtb算出来的呢？