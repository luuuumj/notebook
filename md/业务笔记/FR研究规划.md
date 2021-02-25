## FR研究规划



1.algorithmic pipeline：人脸检测 → 人脸属性 → 人脸识别

joint learning

- det + rec
- attr + rec (query quality)



2.product pipeline: 

正向：数据采集/清洗/标注 → 人脸识别模型 → 产品交付

数据采集/清洗/标注 → 人脸识别模型 

- 数据长尾分布研究
- 难样本挖掘
- semi-supervised / unsupervised learning

人脸识别模型 → 产品交付

- 超大模型探索

- 模型小型化

- 模型搜索

- 模型量化

- 模型蒸馏

  

反向：产品交付 → 人脸识别模型 → 数据采集/清洗/标注

产品交付 → 人脸识别模型

- badcase driven research

- 64 bytes feature

- feature adaptor

- face clustering

- 标准化 feature 空间

- 模型 inference 简化

  

人脸识别模型 → 数据采集/清洗/标注

- 指导采集什么样的数据
- 提升清洗数据 Recall 和质量
- 指导如何高效标注



3.training pipeline: data → aug → model → feature / loss → training

data:

- 数据长尾分布研究
- 难样本挖掘
- 采样策略

aug:

- 拉伸/变形
- 光照变化
- 美颜/化妆

model:

- basemodel
- siamese network
- 去 STN
- 全脸
- attention

feature / loss:

- short feature
- margin-based classification loss
- large softmax
- metric learning
- non-metric loss
- feature space 标准化
- normalization
- Probabilistic Face Embeddings / uncertainty

training:

- 多机
- 跨卡 BN
- 一步登天
- 量化训练



4.inference pipeline: data → pre-process → model → feature -score

data:

- RGB
- video
- sketch
- RGB-D

pre-process:

- deblur / denoise / 超分辨

model：

- 模型量化

feature：

- PCA
- feature 融合
- 底库融合

score：

- 调分策略
- 动态阈值