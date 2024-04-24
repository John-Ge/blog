---
title: 'ConvLLaVA: Hierarchical Backbones as Visual Encoder for Large MultiModal Models'
date: 2024-04-24
permalink: /posts/2024/04/blog-post-4/
tags:
  - Large Multimodal Models
  - ConvNeXt
  - Vision and Language
---

# ConvLLaVA: Hierarchical Backbones as Visual Encoder for Large Multi-modal Models

Large Multimodal Models (LMMs) have been used in many areas: understanding, digital world agent and robotics. These visual scenarios among these applications are quite diverse. Images are of different resolutions. Hence, a suitable visual encoder structure is quite important to comprehensive the different scenarios (training data and tasks have been inverstigated in previous researches).

There are two major structures for visual encoders: isotropic and hierarchical ones. Hierarchical visual encoders are divides into several stages and between stages feature maps are downsampled. We assume that hierarchical visual encoders are better choice for visual encoder for large multimodal models. Three reasons are given:

1. **Linear Sptial Complexity** Hierarchical visual encoders are often designed to have high resolution images. This is important for large multimodal models since it could provide more detailed information for the model. Linear complexity is important for large multimodal models since it could scale up to high resolution images.
2. **Information Compression** Hierarchical visual encoders are designed to compress the information between stages. This is important for large multimodal models since it could compress the number of visual tokens and reduce the complexity of the model.

We assume these two factors fits into current high resolution understanding dillema:

- Quadratic spatial complexity for high resolution images. The spatial complexity for ViT is $\mathcal{O}(N^2)$, where $N$ is the number of visual tokens. When scaling up resolution of ViT, The number of visual tokens goes up quadratically, which slows down the training and inference. On the other hand, the number of visual tokens also
- Large number of visual tokens. The number of visual tokens also goes up quadratically when scaling up resolution of ViT. This would add up training and inference bordens for LLM.

Several methods to extend the resolution of Visual Encoder:

- Scale up resolution of ViT: mPLUG-owl-2, Qwen-VL. The upper bound is low due to the quadratic complexity of visual encoder ($448\times 448$).
- Cross Attention: CogAgent. It make use of an extra visual encoder to extract visual features, and then use cross attention to combine the visual features with text features. It does not add up visual tokens but the extra visual encoder still suffers from the quadratic complexity.
- Crop and Local Attention: Monkey, Sphinx, LLaVA-1.5-HD, LLaVA-1.6. Crop images into patches break the integrity of the image and restrict the ability of the visual encoder.
- Unified Model: OtterHD. It suffers from quadratic complexity.
- Adapter: LLaVA-HR. It uses an extra visual encoder to extract visual features and then use adapter to combine the visual features with the major visual encoder (ViT). The major visual encoder still keep the low resolution and restrict the performance.


## Choose of different visual encoder

There are major two types of visual encoder: isotropic and hierarchical ones. Isotropic visual encoder, such as ViT, is simple and easy to train. However, it is not suitable for high resolution images due to its quadratic complexity. Hierarchical visual encoder, such as Swin Transformer and ConvNeXt. Hierarchical visual encoders are often designed with linear spatial complexity since they are applied in high resolution senarios, such as object detection and segmentation.

| Vision Encoder | Resolution | \#Layers   | Width              | \#Patches          |
| -------------- | ---------- | ---------- | ------------------ | ------------------ |
| ViT-L/14       | 224        | 24         | 1024               | 256                |
| ConvNeXt-Large | 256        | [3,3,27,3] | [192,384,768,1536] | [4096,1024,256,64] |

There is no conclution whether isotropic visual encoder or hierarchical visual encoders are better. ViT is a isotropic visual encoder and it has been used in many LMMs. Swin Transformer and ConvNeXt are hierarchical visual encoders. The difference between ConvNeXt and ViT is that ConvNeXt uses depth wise convolutional layers rather than self attention for spatial token mixing.

I choose for ConvNeXt following merits:

- ConvNeXt is a simple yet effective design with linear complexity.  
- It is also designed to have arbitrary shape of visual tokens.
- Another advantage of ConvNeXt is that it is easy to adapt to high resolution images since it does not have postional embeddings.
- ConvNeXt has the CLIP trained weights. We could directly use it as a visual encoder for LMMs.

<div align="center">
  <img src="images/convllava/swin-resnet-convnext.png" width="300" />
  <figcaption>Comparisons between Swin Transformer, ResNet and ConvNeXt.</figcaption>
</div>

## Acknowledgement

We thank Jun Song for providing the chance to work on this project. We thank Ziming Wang, Jiale Yuan, Hangyu Guo, Wenhui Dong and Mingyang Han for helpful comments and discussion.

```bibtex
@misc{convllava2024ge,
  title={ConvLLaVA: Scaling up Resoltion of Visual Assistant},
  author={Chunjiang Ge},
  month={March},
  year={2024},
  address={Beijing, China}
}
```

<!-- url={https://llava-vl.github.io/blog/2024-01-30-llava-next/},
 -->
