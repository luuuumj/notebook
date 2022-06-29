[TOC]

# BEVDet

High-Performance Multi-Camera 3D Object Detection in Bird-Eye-View

paper: https://arxiv.org/pdf/2112.11790.pdf

code：https://github.com/HuangJunJie2017/BEVDet

缝合怪，主要工作可看LSS

引申出3d检测（主要在感知领域）两个流派，1就是bevdet系列，利用相机内外参，将range view的feature 转换到bev（或者可以理解为3d），另一个流派是fcos3d，detr3d等，直接出obj的三维框信息。引申一点比如petr，会decode 3d position

LSS部分 code：https://github.com/HuangJunJie2017/BEVDet/blob/master/mmdet3d/models/necks/view_transformer.py 有点难读

# BEVDet4D







# LSS

Lift, Splat, Shoot: Encoding Images from Arbitrary Camera Rigs by Implicitly Unprojecting to 3D

paper: https://arxiv.org/pdf/2008.05711.pdf

code: https://nv-tlabs.github.io/lift-splat-shoot/



https://github.com/nv-tlabs/lift-splat-shoot/blob/master/src/models.py#L129  感觉很简洁

流程都是

create_frustum -> get_geometry -> voxel_pooling

self.create_frustum()
geom = self.get_geometry(rots, trans, intrins, post_rots, post_trans) 
x = self.get_cam_feats(x) 
x = self.voxel_pooling(geom, x) 

# Fcos3D

# DETR3D

见detr.md DETR3D



# Caddn

Categorical Depth Distribution Network for Monocular 3D Object Detection

paper： https://arxiv.org/pdf/2103.01100.pdf

![截屏2022-06-29 16.25.02](../../material/%E6%88%AA%E5%B1%8F2022-06-29%2016.25.02.png)



精髓在于f2v，具体做法是，先用sampling grid generator来生成uvd和xyz的对应关系，其中d，z并不是均匀的，方便节省稀疏的空间。通过这个grid，利用torch的gird_sample，可以得到坐标的映射。

最后z的collapse也挺有意思的，通过\sigma(feature * p(z)) 将z collapse掉。得到bev的feature。



# BEVdepth

paper： https://arxiv.org/pdf/2206.10092.pdf

知乎：https://zhuanlan.zhihu.com/p/533816895

![截屏2022-06-29 14.05.12](../../material/%E6%88%AA%E5%B1%8F2022-06-29%2014.05.12.png)

整体框架和caddn基本一致，唯一不同的是voxel pooling那里。

所谓的voxel pooling，做法是将uv+d按某个step排列成 u\*v\*d的向量，将这个坐标直接乘以内外参，得到3维坐标的xyz，遍历这个uvd，将output feature 补全。

~~~~c++
__global__ void voxel_pooling_forward_kernel(int batch_size, int num_points, int num_channels, int num_voxel_x,
                                             int num_voxel_y, int num_voxel_z, const int *geom_xyz,
                                             const float *input_features, float *output_features, int *pos_memo) {
  // Each thread process only one channel of one voxel.
  int blk_idx = blockIdx.x;
  int thd_idx = threadIdx.x;
  if (blk_idx >= batch_size * num_points) {
    return;
  } else {
    int batch_idx = blk_idx / num_points;
    int x = geom_xyz[blk_idx * 3];
    int y = geom_xyz[blk_idx * 3 + 1];
    int z = geom_xyz[blk_idx * 3 + 2];
    // if coord of current voxel is out of boundry, return.
    if (x < 0 || x >= num_voxel_x || y < 0 || y >= num_voxel_y || z < 0 || z >= num_voxel_z) {
      return;
    }
    pos_memo[blk_idx * 3] = batch_idx;
    pos_memo[blk_idx * 3 + 1] = y;
    pos_memo[blk_idx * 3 + 2] = x;
    for (int i = 0; i < DIVUP(num_channels, blockDim.x); i++) {
      int channel_idx = i * blockDim.x + thd_idx;
      if (channel_idx >= num_channels) {
        break;
      }
      atomicAdd(
          &output_features[(batch_idx * num_voxel_y * num_voxel_x + y * num_voxel_x + x) * num_channels + channel_idx],
          input_features[blk_idx * num_channels + channel_idx]);
    }
  }
}

~~~~



这里可以讨论一个问题，坐标转换过程中，会出现多个3xyz对应同一个，因为xzy会做取整操作，那么就会出现即有重叠又有空洞的情况。感觉上并不如caddn里面的f2v（利用grid_sample来做坐标变换）。可能是效率的问题，voxel pooling不用考虑插值的事情，因为xyz坐标取整了。

```python
geom_xyz = ((geom_xyz - (self.voxel_coord - self.voxel_size / 2.0)) / self.voxel_size).int()
```





Camera-aware depth prediction, 感觉太tricky，不是很本质。

$D_{i}^{\text {pred }}=\psi\left(S E\left(F_{i}^{2 d} \mid M L P\left(\xi\left(R_{i}\right) \oplus \xi\left(\boldsymbol{t}_{i}\right) \oplus \xi\left(K_{i}\right)\right)\right)\right)$,

# Voxel

Unifying Voxel-based Representation with Transformer for 3D Object Detection

paper：https://arxiv.org/pdf/2206.00630.pdf

code：https://github.com/dvlab-research/UVTR



# BEVFormer

BEVFormer: Learning Bird’s-Eye-View Representation from Multi-Camera Images via Spatiotemporal Transformers

paper：https://arxiv.org/pdf/2203.17270.pdf

code：https://github.com/zhiqi-li/BEVFormer

讨论：https://www.zhihu.com/question/521842610/answer/2431585901



keywords: multi-camera inputs 对标HDMap

BEV 分类：

1.  detr3d类（petr等）的，没有显式引入BEV特征（所谓bev特征，我理解的就是2d特征做一定变换得到的bev特征），而是引入3d feature（比如petr中的position embedding，detr3d中通过所谓坐标变换找到的2d坐标，将3dfeature 和2d feature 加在一起，这其实是所谓的MLP类的2d转3d的通用做法）。bev特征的缺点是【如果先生成BEV,再利用BEV进行检测容易产生复合错误（？）】，但其实cross-attention已经 经过多方证明，是当前比较好的方法。
2.  很少有人做时空融合

## Related work

**Camera-based 3D Perception**

IPM, LSS, MLP,corss-attention(not suitable for multi-camera due to the cost)

## Architecture



![截屏2022-06-27 17.59.16](../../material/%E6%88%AA%E5%B1%8F2022-06-27%2017.59.16.png)



**Spatial Cross-Attention**

$\operatorname{SCA}\left(Q_{p}, F_{t}\right)=\frac{1}{\left|\mathcal{V}_{\text {hit }}\right|} \sum_{i \in \mathcal{V}_{\text {hit }}} \sum_{j=1}^{N_{\text {ref }}} \operatorname{DeformAttn}\left(Q_{p}, \mathcal{P}(p, i, j), F_{t}^{i}\right)$

做的是multi camera 的deform attention，按理比单个的好

**Temporal Self-Attention**

$\operatorname{TSA}\left(Q_{p},\left\{Q, B_{t-1}^{\prime}\right\}\right)=\sum_{V \in\left\{Q, B_{t-1}^{\prime}\right\}} \operatorname{DeformAttn}\left(Q_{p}, p, V\right)$

用self attention 代替类似RNN的东西， 有点巧妙



## Q&A

>   history bev feature 和current queries shape是多少。为什么可以方便的self-attention，以及后续的downstream task。以及二维的feature 怎么attention



# BEVFusion

