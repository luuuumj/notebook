通行blur最大难点：

1. 既不能过滤太严格，也不能太松。整体考量的是通行体验(时间和准确率)，阈值和评价标准比较难确定

   解决：阈值以图片数量卡完剩1/3为准，评价标准仅以模糊不模糊为主，不考虑模组噪点。

2. 不同光照成像效果差异很大。正常光正常，亮光泛白，暗光噪点明显变多。在blurness想要拉到同一个值(理想中，这些应该都是0.2以内)，比较困难

![屏幕快照 2019-07-17 10.47.30](/Users/lumingjie/Desktop/jietu/屏幕快照 2019-07-17 10.47.30.png)

3. 缺少blur数据，采集中没有刻意采集模糊图片，但实际使用过程中却经常发现很糊的图片

![屏幕快照 2019-07-17 10.57.49](/Users/lumingjie/Desktop/jietu/屏幕快照 2019-07-17 10.57.49.png)



![屏幕快照 2019-07-17 10.59.42](/Users/lumingjie/Desktop/屏幕快照 2019-07-17 10.59.42.png)



4. 通行的低质量和安防的低质量不太一样。安防都是太远而糊，这些样本需不需要做？blur模型需不需要过滤掉。

数据地址：

/unsullied/sharefs/_research_facerec/face-recognition-benchmark/repos/inu-benchmark-v3.0-m5-{query,base}

<https://sharefs.iap.wh-a.brainpp.cn/_research_facerec/pages/results/20190702/training.inu-benchmark-v3.0-m5.20190702211155/>

/unsullied/sharefs/_research_facerec/face-recognition-benchmark/repos/inu-benchmark-v3.0-qiangqiu-{query,base}

<https://sharefs.iap.wh-a.brainpp.cn/_research_facerec/pages/results/20190702/training.inu-benchmark-v3.0-qiangqiu.20190702211201/>



```
/unsullied/sharefs/_research_facerec/face-recognition-benchmark/repos/BBU-blur-mini-bmk-{query,base}
/unsullied/sharefs/_research_facerec/face-recognition-benchmark/repos/BBU-blur-mini-bmk-20-{query,base}
/unsullied/sharefs/_research_facerec/face-recognition-benchmark/repos/BBU-blur-mini-bmk-m5-{query,base}
/unsullied/sharefs/_research_facerec/face-recognition-benchmark/repos/BBU-company-koala-pair-{base, query}
/unsullied/sharefs/_research_facerec/face-recognition-benchmark/repos/megvii-koala-all-20190606-m5-{base,query}
/unsullied/sharefs/_research_facerec/face-recognition-benchmark/repos/megvii-koala-all-20190606-ipc-{base,query}
```





安防端和服务器ppl不一样，无法拉通，那通行端和服务器也不一样，测试方式却没有体现ppl

视频怎么测的(抽帧？)

duxianyan测试过通行bmk吗



可视化怎么做的，attention 的物理含义？对应的channel

V3 和 v3.block.se 区别



填写distractor说明。 所有模型。

blur-mini-bmk抽新的2w 干扰库。

大模型badcase整理。

bbu内部拉通。发版一定要拉齐！！！

大表增加一列编号。



底库多样性，收集20人底库

三期blur分布和自采的区别~~ (有区别)

转正





缺点：几乎没有cv经验，CNN也不了解，但感觉够聪明，表示不想做研发，更愿意做cv算法。

项目：B：主要方向是数据挖掘，做过交通预测，天池财务造假的比赛。比较清楚的描述自己做的任务，机器学习理论比较扎实。缺点：只用过xgboost 和 RF，创新性较小。

数学：A-：问了概率论常识，很快回答上来。max(a,b)会用几何方法解决。问了一道数模题（基因位点预测），用了两种方法解答。整体感觉知识比较扎实

code：B+：电面和时间原因没有online coding，问了几个简单的编程题，最大子序和，O(1)空间复杂度遍历树 都回答的很快很完整。



标准脸 鼻尖是不是在框的中心。