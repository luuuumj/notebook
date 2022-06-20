# Scaling Up Your Kernels to 31x31: Revisiting Large Kernel Design in CNNs

paper：https://arxiv.org/pdf/2203.06717.pdf

code： https://github.com/megvii-research/RepLKNet



## Note：

1.  *Why ViTs super powerful? MSA plays a key role which is more flexible, capable (less inductive bias), more robust to distortions, able to model long-range dependencies and etc.* However, some recent researches challenge the necessity of MSA.
2.  *ConvMixer [86] uses up to 9×9 convolutions to replace the “mixer” component of ViTs [34] or MLPs [83, 84]. MetaFormer [104] suggests pooling layer is an alternate to self-attention. ConvNeXt [61] employs 7×7 depth-wise convolutions to design strong architectures, pushing the limit of CNN performances. Although those works show excellent performances, they do not show benefits from much larger convolutions (e.g., 31×31).*



## Guideline

Guideline 1: large **depth-wise** convolutions can be efficient in practice.

Guideline 2: identity shortcut is vital especially for networks with very large kernels.(repvgg)

Guideline 3: re-parameterizing [30] with small kernels helps to make up the optimization issue.

Guideline 4: large convolutions boost downstream tasks much more than ImageNet classification.

Guideline 5: large kernel (e.g., 13×13) is useful even on small feature maps (e.g., 7×7).

## Q&A：

1.  shape bias rather than texture bias 是指啥

    







匹配

