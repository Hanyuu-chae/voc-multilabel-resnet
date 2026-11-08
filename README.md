# voc-multilabel-resnet
本项目基于 PyTorch 框架，在 PASCAL VOC 2007 数据集上实现了多标签图像分类。
项目以标准的 ResNet-18 作为基线网络（Baseline），并进行了结构创新与消融实验研究：

1.创新一 (SE-ResNet18)：引入 SE（Squeeze-and-Excitation）通道注意力机制，自适应校准通道特征响应。

2.创新二 (SE-ResNet18 + GELU)：集成 GELU 激活函数，改善深层网络的非线性映射与梯度传播。

# 项目特点
1.完整训练流程：包含数据增广（Resize、ColorJitter、RandomHorizontalFlip）、AdamW 优化器策略及余弦退火学习率调度器（CosineAnnealingLR）。

2.多标签损失函数：采用 BCEWithLogitsLoss（二元交叉熵损失），对多目标共存场景进行独立概率建模。

3.评估指标：计算并实时输出多标签评估的核心指标，包括 mAP (Mean Average Precision)、Precision、Recall 及 F1-Score。

4.代码规范可复现：统一全局随机种子（Seed=42），支持 9:1 的固定随机拆分。

# 网络架构与结构创新
1. SE 通道注意力机制 (Squeeze-and-Excitation Module)
SE 模块通过显式建模特征通道之间的相互依赖关系，自适应地重新校准通道特征响应：
(1)Squeeze：通过全局平均池化（Global Average Pooling）将空间特征压制为通道描述符。
(2)Excitation：通过全理解 Bottleneck 结构学习通道间权重。
(3）Scale：将权重乘回原特征图，显著增强关键物体特征通道、抑制杂乱背景干扰。

3. GELU 激活函数
相比传统基于分段线性的 ReLU 激活函数，GELU 具有平滑连续的非线性特性，允许微小的负梯度通过，避免神经元“坏死”问题，进一步增强了多标签特征的表达能力。

# 环境配置
Python 3.8+ 以及带有 CUDA 支持的 PyTorch 环境。
1. 克隆本仓库
git clone https://github.com/Hanyuu-chae/voc-multilabel-resnet
cd voc-multilabel-resnet

2. 安装依赖库
pip install torch torchvision numpy

# 数据集准备
请下载 [PASCAL VOC 2007](https://www.kaggle.com/datasets/zaraks/pascal-voc-2007/suggestions)
 数据集，并将其解压按以下结构放置在项目目录中：
```text
train/
└── VOCdevkit/
    └── VOC2007/
        ├── Annotations/
        ├── ImageSets/
        ├── JPEGImages/
        └── ...
