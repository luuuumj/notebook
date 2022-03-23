[TOC]



# Image-to-Image Translation

## Problem Definition

- **Basic** **definition of** **I2I:** **converting** **an image from one** **source** **domain to another** **target** **domain**

- **Applications:** **semantic** **image** **synthesis,** **image** **segmentation,** **style** **transfer,** **image** **inpainting** **, 3D** **pose** **estimation,** **image/video colorization,** **image super-resolution, domain** **adaptation,** **cartoon** **generation** **and image** **registration** **…**

## Overview

![image-20210629160305065](../material/image-20210629160305065.png)



## pix2pix

* learn translation in a supervised manner using cGAN
* <img src="../material/image-20210629160351836.png" alt="image-20210629160351836" style="zoom: 25%;" />

## Cycle GAN

<img src="../material/image-20210629163349646.png" alt="image-20210629163349646" style="zoom:25%;" /><img src="../material/image-20210629163210868.png" alt="image-20210629163210868" style="zoom: 25%;" />

<img src="../material/image-20210629163447328.png" alt="image-20210629163447328" style="zoom:25%;" />

孪生三兄弟 CycleGAN，DiscoGAN，DualGAN

https://blog.csdn.net/qq_37995260/article/details/90510052



## Conditional CycleGAN

 <img src="../material/image-20210629165811266.png" alt="image-20210629165811266" style="zoom:25%;" /><img src="../material/image-20210629165845760.png" alt="image-20210629165845760" style="zoom:25%;" />



## StarGAN

<img src="../material/屏幕快照%202021-02-03%2020.55.49.png" alt="屏幕快照 2021-02-03 20.55.49" style="zoom: 67%;" /><img src="../material/屏幕快照%202021-02-03%2022.20.15.png" alt="屏幕快照 2021-02-03 22.20.15" style="zoom:50%;" />

* Adversarial Loss:    $\begin{aligned}
  \mathcal{L}_{a d v}=& \mathbb{E}_{x}\left[\log D_{s r c}(x)\right]+\
  \mathbb{E}_{x, c}\left[\log \left(1-D_{s r c}(G(x, c))\right)\right]
  \end{aligned}$
* Domain Classification Loss: $\mathcal{L}_{c l s}^{r}=\mathbb{E}_{x, c^{\prime}}\left[-\log D_{c l s}\left(c^{\prime} \mid x\right)\right]$
* Reconstruction Loss:  $\mathcal{L}_{r e c}=\mathbb{E}_{x, c, c^{\prime}}\left[\left\|x-G\left(G(x, c), c^{\prime}\right)\right\|_{1}\right]$
* Full Loss: $\begin{array}{c}
  \mathcal{L}_{D}=-\mathcal{L}_{a d v}+\lambda_{c l s} \mathcal{L}_{c l s}^{r} \\
  \mathcal{L}_{G}=\mathcal{L}_{a d v}+\lambda_{c l s} \mathcal{L}_{c l s}^{f}+\lambda_{r e c} \mathcal{L}_{r e c}
  \end{array}$



## StarGAN v2



<img src="../material/屏幕快照%202021-02-03%2022.20.28.png" alt="屏幕快照 2021-02-03 22.20.28" style="zoom:50%;" />

* Adversarial objective $\begin{aligned} \mathcal{L}_{a d v}=& \mathbb{E}_{\mathbf{x}, y}\left[\log D_{y}(\mathbf{x})\right]+  \mathbb{E}_{\mathbf{x}, \widetilde{y}, \mathbf{z}}\left[\log \left(1-D_{\widetilde{y}}(G(\mathbf{x}, \widetilde{\mathbf{s}}))\right)\right] \end{aligned}$
* Style reconstruction  $\mathcal{L}_{\text {sty }}=\mathbb{E}_{\mathbf{x}, \widetilde{y}, \mathbf{z}}\left[\left\|\widetilde{\mathbf{s}}-E_{\widetilde{y}}(G(\mathbf{x}, \widetilde{\mathbf{s}}))\right\|_{1}\right]$
* Style diversification $\mathcal{L}_{d s}=\mathbb{E}_{\mathbf{x}, \tilde{y}, \mathbf{z}_{1}, \mathbf{z}_{2}}\left[\left\|G\left(\mathbf{x}, \widetilde{\mathbf{s}}_{1}\right)-G\left(\mathbf{x}, \widetilde{\mathbf{s}}_{2}\right)\right\|_{1}\right]$
* Preserving source characteristics $\mathcal{L}_{c y c}=\mathbb{E}_{\mathbf{x}, y, \tilde{y}, \mathbf{z}}\left[\|\mathbf{x}-G(G(\mathbf{x}, \widetilde{\mathbf{s}}), \hat{\mathbf{s}})\|_{1}\right]$
* Full objective $\begin{array}{rl}\min _{G, F, E} \max _{D} & \mathcal{L}_{a d v}+\lambda_{s t y} \mathcal{L}_{s t y} \\ & -\lambda_{d s} \mathcal{L}_{d s}+\lambda_{c y c} \mathcal{L}_{c y c}\end{array}$



## UNIT

<img src="../material/image-20210629170135985.png" alt="image-20210629170135985" style="zoom:25%;" />

已知两个domain的边缘分布 $P_{X_{1}}(x_{1}),P_{X_{2}}(x_{2})$，想要得到两个domain转换的联合分布$P_{X_{1},X_{2}}(x_{1},{x_{2}})$

很自然就假设这两个domain共享同一个latent code

共享latent code其实就是 $E_{1}(x_{1}) = z = E_{2}(x_{2})$

综合起来就是本文使用了两个VAE-GAN



## MUNIT

<img src="../material/image-20210629170630409.png" alt="image-20210629170630409" style="zoom:25%;" /><img src="../material/image-20210629170639050.png" alt="image-20210629170639050" style="zoom: 25%;" />



思想就是x1和x2encode两个latent code，交叉后decode两种图片，构建重建loss

<img src="../material/image-20210629170657079.png" alt="image-20210629170657079" style="zoom: 33%;" />



## DRIT，DRIT++

和MUNIT 思路基本一样：

<img src="../material/image-20210629165957101.png" alt="image-20210629165957101" style="zoom: 25%;" />

pipeline：





<img src="../material/image-20210629170914653.png" alt="image-20210629170914653" style="zoom:25%;" /> 

[image-2-image translation.pptx](../../../../Desktop/image-2-image translation.pptx) 

