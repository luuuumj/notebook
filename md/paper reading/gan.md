## Image-to-Image Translation

pix2pix

* learn translation in a supervised manner using cGAN

UNIT

CycleGAN, DiscoGAN

StarGAN

<img src="../material/屏幕快照 2021-02-03 20.55.49.png" alt="屏幕快照 2021-02-03 20.55.49" style="zoom: 67%;" /><img src="../material/屏幕快照 2021-02-03 22.20.15.png" alt="屏幕快照 2021-02-03 22.20.15" style="zoom:50%;" />

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



StarGAN v2



<img src="../material/屏幕快照 2021-02-03 22.20.28.png" alt="屏幕快照 2021-02-03 22.20.28" style="zoom:50%;" />

* Adversarial objective $\begin{aligned} \mathcal{L}_{a d v}=& \mathbb{E}_{\mathbf{x}, y}\left[\log D_{y}(\mathbf{x})\right]+  \mathbb{E}_{\mathbf{x}, \widetilde{y}, \mathbf{z}}\left[\log \left(1-D_{\widetilde{y}}(G(\mathbf{x}, \widetilde{\mathbf{s}}))\right)\right] \end{aligned}$
* Style reconstruction  $\mathcal{L}_{\text {sty }}=\mathbb{E}_{\mathbf{x}, \widetilde{y}, \mathbf{z}}\left[\left\|\widetilde{\mathbf{s}}-E_{\widetilde{y}}(G(\mathbf{x}, \widetilde{\mathbf{s}}))\right\|_{1}\right]$
* Style diversification $\mathcal{L}_{d s}=\mathbb{E}_{\mathbf{x}, \tilde{y}, \mathbf{z}_{1}, \mathbf{z}_{2}}\left[\left\|G\left(\mathbf{x}, \widetilde{\mathbf{s}}_{1}\right)-G\left(\mathbf{x}, \widetilde{\mathbf{s}}_{2}\right)\right\|_{1}\right]$
* Preserving source characteristics $\mathcal{L}_{c y c}=\mathbb{E}_{\mathbf{x}, y, \tilde{y}, \mathbf{z}}\left[\|\mathbf{x}-G(G(\mathbf{x}, \widetilde{\mathbf{s}}), \hat{\mathbf{s}})\|_{1}\right]$
* Full objective $\begin{array}{rl}\min _{G, F, E} \max _{D} & \mathcal{L}_{a d v}+\lambda_{s t y} \mathcal{L}_{s t y} \\ & -\lambda_{d s} \mathcal{L}_{d s}+\lambda_{c y c} \mathcal{L}_{c y c}\end{array}$







Conditional CycleGAN

