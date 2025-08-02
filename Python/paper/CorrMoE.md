## 一、研究背景与动机

**图像对应点匹配（correspondence matching）** 是计算机视觉中的基础问题，广泛应用于3D重建、视觉定位、SLAM等任务。然而现实中图像之间存在较大的风格、视角、光照和遮挡差异，导致**初始匹配点集包含大量错误匹配（outliers）**。

虽然已有许多基于学习的筛选方法，比如 OANet、CLNet、BCLNet 等，但它们往往**假设场景或图像来自同一领域（domain）或风格（style）**，对跨域/跨场景情况泛化能力差。

------

## 二、CorrMoE 方法概述

论文提出了一种新颖的两阶段网络结构：

1. **去风格双分支模块（De-stylization Dual Branch）**：提高跨领域泛化能力；
2. **双融合专家混合模块（Bi-Fusion Mixture of Experts, MoE）**：提升跨场景适应性。

整体流程如下：

- 给定两个图像，提取初始特征点并匹配成一个对应点集；
- 输入网络，经过上述两大模块处理；
- 输出每个点对的“置信度权重”，从而进行对应点的筛选；
- 最后利用几何约束回归相机姿态并验证最终匹配。

------

## 三、关键技术模块

### 1. 去风格双分支模块（De-stylization Dual Branch）

目的：缓解由于**图像风格差异带来的领域偏移（domain shift）**。

包括以下两个分支：

- **Progressive MixStyle 模块（PMix）**：对特征进行风格扰动，使模型逐渐学习去风格化的泛化能力；
  - 相比原始 MixStyle，PMix 逐步提高扰动概率，使训练初期更稳定，后期更具泛化性。
- **隐式图分支（Implicit Branch）**：
  - 使用 DiffPool 和 Order-Aware 模块对局部邻域进行聚合和还原；
  - 得到的隐式图特征通过再次 PMix 处理去风格。
- **显式图分支（Explicit Branch）**：
  - 基于 KNN 构建邻接图；
  - 使用多维注意力机制（空间、邻居、通道）进一步增强图结构；
  - 最后聚合成显式特征，同样进行去风格 PMix 处理。

最终两个分支的特征拼接，送入下一模块。

------

### 2. Bi-Fusion Mixture of Experts 模块

目的：提高模型在不同场景结构中的**动态适应能力**。

主要做法：

- 使用轻量级的线性注意力（FlowAttention）将隐式与显式图融合；
- 加入 **MoE 模块（混合专家结构）**：
  - 每个专家专注于不同的几何/视觉结构；
  - 使用 Top-K gating 机制选择最适合的专家处理当前输入；
  - 最终所有专家结果加权融合，提升对复杂/稀疏场景的适应性。

------

### 3. 损失函数设计

损失由两个部分组成：

- **分类损失（Lcls）**：判断每对匹配点是否为内点（inlier）；
- **几何损失（Less）**：约束预测出的本质矩阵与真实矩阵之间的一致性。

------

## 四、实验与结果分析

### 1. 核心任务性能（YFCC100M, SUN3D）

- CorrMoE 在相机姿态估计和错误匹配点剔除任务中全面超越现有 SOTA 方法：
  - YFCC100M 上 AUC@5° 提升了 2.28；
  - SUN3D 上也保持领先；
  - F1 值、Precision、Recall 均优于 BCLNet 等方法。

### 2. 跨场景测试（Cross-Scene）

- 在 YFCC100M 的子场景（如 BUCKINGHAM、SACRE 等）中，CorrMoE 能适应各种建筑风格与视角变化，表现稳定。

### 3. 跨领域测试（Cross-Domain）

- 使用 ZEB benchmark（12 个不同来源的数据集）：
  - CorrMoE 显著领先，平均 AUC@5° 提高至 **22.80**，比 BCLNet 高出 **8.38**；
  - 尤其在模拟数据集和光照、季节变化场景中表现强劲。

------

## 五、消融实验（Ablation）

- 验证 De-stylization 双分支和 Bi-Fusion MoE 都是性能提升的关键；
- 比较不同 MoE 堆叠次数，发现堆叠 4 层在性能和速度之间取得最佳平衡；
- Progressive MixStyle（PMix）在泛化能力上比传统 InstanceNorm 和 MixStyle 更好。

------

## 六、总结

**CorrMoE 的贡献如下：**

1. 首次将 MoE 引入到两视图对应点筛选任务中；
2. 设计了 De-stylization Dual Branch 和 Bi-Fusion MoE 两个创新模块；
3. 显著提升了跨场景和跨领域的泛化能力，优于所有现有方法。