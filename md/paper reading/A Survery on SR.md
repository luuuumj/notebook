# A Survery on SR

## Introduction

SR is an ill-posed inverse problem. 

Traditional algorithms are out-performed by deep learning based counterparts

y = LR x = HR, a degradation process:
$$
\mathbf{y}=\mathbf{\Phi}\left(\mathbf{x} ; \theta_{\eta}\right)
$$
$\mathbf{\Phi}$ Can be noise (sensor and speckle), compression, blur (defocus and motion), and other artifacts. 

a SR progress is defined:
$$
\hat{\mathbf{x}}=\Phi^{-1}\left(\mathbf{y}, \theta_{\zeta}\right)
$$

##categories:

###1. Linear networks

#### Early Upsampling Designs

(Bicubic) upsample LR and than get HR through NN

- SRCNN: The first successful attempt ;There are a total of three convolutional and two ReLU layers, stacked together linearly
- VDSR:Very Deep SuperResolution; VGG-net,a residual mapping LR and HR
- DnCNN: like SRCNN with residual
- IRCNN:Image Restoration CNN; a denoiser which can solve SR,deblurring,denoising

####Late Upsampling Designs

LR input, HR output

- FSRCNN: Fast Super-Resolution Convolutional Neural Network;The feature extraction step is similar to SRCNN;upsampling and aggregating deconvolution; prelu
- ESPCN: Efficient sub-pixel convolutional neural network; sub-pixel convolution is essentially similar to convolution transpose or deconvolution operation; fractional kernel stride
- *To do: understand what are deconvolution and sub-pixel conv*

### 2. Residual Networks

#### Single-stage Residual Nets

a single network

- EDSR: Enhanced Deep SR; ResNet architecture; remove BN; 
- CARN: Cascading residual network; ResNet Blocks; local and global cascading modules;cascaded and converged onto a 1×1 convolutional layer.

#### Multi-Stage Residual Nets

A multi-stage design is composed of multiple subnets

- FormResNet: “Formatting layer” with s Euclidean and perceptual loss and “DiffResNet” is fed from the first one ;
- BTSRN: low-resolution stage and a high-resolution stage around a upsampling layer; 
- REDNet: Residual Encoder Decoder Network ; Unet like architecture;  

### 3. Recursive networks

network with recursively connected convolutional layers or recursively linked units

- DRCN: Deep Recursive Convolutional Network; applies the same convolution layers
  multiple times; three parts: embedding net, inference net, and reconstruction net;
- DRRN: **Deep** Recursive Residual Network; deeper architecture with as many as 52 convolutional layers; share weights; 
- MemNet: similar to SRCNN; three parts: feature extraction block, memory blocks,Gated Unit;

### 4. Progressive reconstruction designs

large scaling factors, 2× followed by 4× and so on

- SCN: sparse code; After the LISTA network, the original highresolution patches are reconstructed by multiplying the sparse code and high-resolution dictionary in the successive linear layer
- LapSRN: Deep Laplacian pyramid super-resolution network; The output of the first sub-network is a residue of 2×, the second sub-network provides a 4× residue;

###5. Densely Connected Networks

DenseNet  architecture

- SR-DenseNet: is based on the DenseNet; 
- RDN: Residual Dense Network;
- D-DBPN: Dense deep back-projection network;