

# MSGA-Net

> Z. Gong, G. Xiao, Z. Shi, R. Chen and J. Yu, "MSGA-Net: Progressive Feature Matching via Multi-Layer Sparse Graph Attention," in *IEEE Transactions on Circuits and Systems for Video Technology*, vol. 34, no. 7, pp. 5765-5775, July 2024, doi: 10.1109/TCSVT.2024.3366912. keywords: {Task analysis;Sieving;Semantics;Circuits and systems;Transformers;Feature extraction;Reliability;Outlier removal;transformer;camera pose estimation;feature matching;deep learning},

这篇文章名为 **《MSGA-Net: Progressive Feature Matching via Multi-Layer Sparse Graph Attention》**，发表于 IEEE Transactions on Circuits and Systems for Video Technology 期刊，提出了一种用于**图像特征匹配**的新型深度学习方法——**MSGA-Net**。以下是对论文内容的系统性解析：

------

## 🧠 一、研究背景与问题

### 1.1 任务背景

**特征匹配**在计算机视觉任务（如图像配准、SLAM、结构光重建等）中极为关键。其过程包括：

1. 特征点与描述子的提取（如 SIFT、SuperPoint）
2. 初始匹配（通常采用最近邻策略）
3. **剔除错误匹配（Outlier Removal）**

### 1.2 问题挑战

- 由于视角、光照、模糊等影响，初始匹配中存在大量**错误匹配点（outliers）**
- 传统剔除方法（如 RANSAC）效率低，易受大量 outliers 影响
- 学习方法逐渐崛起（如 LFGC-Net, OA-Net），但存在两类问题：
  - 忽略**匹配点间的上下文关系**
  - **深层语义信息利用不足**

------

## 🚀 二、MSGA-Net 的主要贡献

### ✳️ 1. Sparse Dynamic Graph Interaction (SDGI)

- 在网络不同层构建稀疏图（使用KNN）
- 引入交叉注意力机制，**融合浅层的细节信息与深层的语义信息**
- 通过点之间边的权重建立更强的连接与鲁棒性

### ✳️ 2. Multiple Sparse Transformer (MSFormer)

- 在空间维度与通道维度分别进行上下文建模（双路径）
- 只保留注意力图中**top-k重要匹配对**，降低噪声影响
- 使用 AFF 模块做通道注意力（优于 SE 模块）

### ✳️ 3. MSGA-Net 架构整体

- 两个**筛选模块（Sieving Modules）**逐步剔除匹配点
- 使用 SDGI 和 MSFormer **交替组合**提炼特征图
- 最终预测每个点对为 inlier 的概率，并估计相机姿态（如本质矩阵）

------

## 🏗️ 三、方法结构概览

### 网络输入

- 输入初始点对集合$S \in \mathbb{R}^{N \times 4}$，每行是 $(x, y, x', y')$

### 流程：

1. 使用MLP和ResNet提取初步特征
2. 构建浅层稀疏图并通过 SDGI 结合深层图
3. 使用 MSFormer 提取全局上下文（通道+空间）
4. 多轮迭代 pruning 得到更可信的点对子集
5. 用 weighted 8-point 算法估计相机模型
6. 对原始点集进行 full-size 验证以恢复被误删的 inlier

------

## 🔬 四、实验结果分析

### 4.1 数据集

- 室外：YFCC100M
- 室内：SUN3D（更具挑战性，遮挡、重复结构多）

### 4.2 性能指标

- **Outlier Removal**: Precision, Recall, F-score
- **Pose Estimation**: mAP under 5°, 10°, 20°

### 4.3 实验亮点

- 在 YFCC100M 和 SUN3D 上都优于当前最优方法（如 CL-Net、MS2DG-Net）
- 对比 SuperGlue，MSGA-Net 可进一步增强精度
- 在使用 RANSAC 作为后处理时，有时会因 MSGA-Net 预测已足够精确而反降性能

## 🎯 MSGA-Net 整体结构概览

MSGA-Net 的主要流程：

> **图像对 → 多尺度特征提取 → 稀疏图构建 → 跨图注意力匹配（MSGA 模块） → 匹配预测**

分为以下几个主要模块：

------

### 🧩 模块一：Backbone Feature Extractor（骨干特征提取器）

#### ✅ 作用：

提取图像的多尺度语义特征。

#### ✅ 实现方式：

- 使用 **共享 CNN**（如 ResNet-34）对输入图像对分别提取特征。
- 提取多个尺度的特征（例如：1/8, 1/4, 1/2 分辨率），形成金字塔。

#### ✅ 输出：

每对图像输出多个尺度的特征对，供 MSGA 模块逐层处理。

------

### 🧩 模块二：Multi-Layer Sparse Graph Attention（MSGA 模块）

这是本文最核心的创新模块！

#### ✅ 目的：

实现跨图像的注意力交互，构建稀疏图结构，高效精准地传播匹配信息。

------

#### ✨ 模块内部结构：

##### **1. 构建图节点：**

- 使用每幅图像的 CNN 特征图中的每个点（或 patch）作为图的节点。

##### **2. 构建稀疏图边：**

- 基于节点之间的特征相似度，选取 **Top-K** 邻居建立边（稀疏图结构）。
- 同时在**同一图像内** 和 **两幅图之间** 都构建边（注意力传播允许跨图）。

> 🌐 跨图连接是本方法的重要点，用来捕捉潜在匹配关系。

##### **3. 执行图注意力机制：**

- 使用类似 GAT 的方式计算邻居注意力权重：

  αij=softmaxj(eij)\alpha_{ij} = \text{softmax}_j(e_{ij})αij=softmaxj(eij)

- 每个节点聚合邻居的加权信息，完成一次图注意力更新。

##### **4. 多层（Multi-Layer）处理：**

- 每个尺度的 CNN 特征图都会经过 MSGA 处理
- 每层 MSGA 输出 refined 特征，供上层更精细的匹配计算

------

### 🧩 模块三：Progressive Matching Decoder（渐进式匹配解码器）

#### ✅ 目的：

融合各层 MSGA 输出的特征，逐步 refine 匹配结果。

#### ✅ 特点：

- 解码过程从粗尺度（低分辨率）逐层向高尺度推进
- 每一层 refine 匹配矩阵，并引导 finer 层的图 attention

#### ✅ 匹配计算：

- 计算图像对之间的点对相似度矩阵（如余弦相似度）
- 使用 softmax + mutual nearest neighbor 策略输出匹配分布

------

### 🧩 模块四：Matching Confidence Estimator（匹配置信度估计器）

#### ✅ 目的：

为预测的匹配对打分，提升鲁棒性（比如去除错配）。

#### ✅ 实现方式：

- 使用预测的匹配分布，构建 matching confidence（如类似 Sinkhorn、或者 mutual softmax）

------

### 🧩 模块五：Loss Function 设计

#### ✅ 包括：

- Supervision loss：预测的匹配概率与 GT 对应关系之间的 KL 损失
- Confidence loss：预测匹配置信度与 ground truth 一致性的对比损失

------

## ✅ 模块关系总结图：

```
text复制编辑        ┌────────────┐
        │ Image Pair │
        └────┬───────┘
             ↓
 ┌────────────────────┐
 │ Backbone (CNN)     │
 │ Multi-scale feats  │
 └────┬───────┬───────┘
      ↓       ↓
  scale 1   scale 2 ...
   ↓           ↓
┌────────────┐
│ MSGA Block │   ← 关键注意力交互模块（跨图 + 稀疏图）
└────┬───────┘
     ↓
 Progressive Matching Decoder
     ↓
 Confidence Estimator
     ↓
   Final Matches
```

------

## ✅ 每个模块的贡献总结：

| 模块                 | 作用                 | 亮点                              |
| -------------------- | -------------------- | --------------------------------- |
| CNN Backbone         | 提取多尺度特征       | 轻量、共享参数                    |
| MSGA                 | 高效图注意力匹配     | 跨图 + 稀疏图结构，大幅降低计算量 |
| Progressive Matching | 从粗到细 refine 匹配 | 渐进式策略提升鲁棒性              |
| Confidence Estimator | 保证匹配可信度       | 消除弱匹配、错匹配点              |
| Loss Design          | 精细监督             | 结合分布预测与置信监督            |



------

如果你希望我用图解释 MSGA 的 attention 如何操作、或者你想对照代码还原 MSGA-Net 的实现流程，我也可以帮你画出模块结构图或写 PyTorch 原型。需要吗？





------

## 📊 五、消融实验结论（Ablation Studies）

| 模块           | 性能影响                                   |
| -------------- | ------------------------------------------ |
| SDGI           | 融合层间信息，提升 mAP5° 高达 6%           |
| MSFormer       | 通过 Masked Attention 去除弱匹配，提升效果 |
| Layer 数量     | MSFormer 用2层效果最佳，过深则过拟合       |
| L 值（图大小） | L=16 是最佳平衡                            |
| 全图验证       | 可提升 F-score 约 3%                       |



------

## ✅ 六、总结与前景展望

### 主要优势

- 引入跨层稀疏图交互机制
- 注意力机制结合剪枝策略显著降低outlier干扰
- 可作为独立剔除模块与 SuperGlue 等方法结合使用

### 不足与改进方向

- 推理速度仍有优化空间
- 层间语义信息融合方式还可更精细化

------

## 🔗 附录链接

代码仓库（如文中提到）：[GitHub - MSGA-Net](https://github.com/gongzhepeng/MSGA-Net)



## SDGI

### 原因

> 先前的工作[21]，[22]遵循DGCNN [23]的方法在网络的浅层层构造稀疏的动态图，以捕获局部拓扑信息。但是，他们忽略了网络较深层的稀疏动态图，其中包含丰富的语义信息。结果，未能有效地在浅层和更深的动态图之间建立连接。

### 🧠 SDGI 的特别之处

| 特性                   | 解释                                            |
| ---------------------- | ----------------------------------------------- |
| 稀疏性（Sparse）       | 仅关注 top-k 邻居，节省内存计算                 |
| 动态性（Dynamic）      | 每一轮 forward 都重新计算图结构，更贴合当前特征 |
| 跨图交互（Cross-view） | 信息从图像 A ↔ B 之间双向传播，便于匹配建模     |
| 图结构                 | 和传统图卷积不同，这里构建的是**异构匹配图**    |

### 🧪 效果与价值

- SDGI 模块能够 **精确选择重要匹配候选点**，避免注意力扩散到冗余区域
- 在消融实验中，去掉 SDGI 模块会显著降低匹配准确率
- 保证效率的同时，保持了**全局匹配建模能力**

### 🧩 总结：SDGI 模块的贡献

| 模块名 | SDGI（Sparse Dynamic Graph Interaction） |
| ------ | ---------------------------------------- |
| 作用   | 构建跨图稀疏图 & 实现信息交互            |
| 特点   | 动态构图、top-k 相似邻居、跨图 attention |
| 目的   | 高效建模图像之间的精细匹配关系           |
| 好处   | 降低计算、提升匹配质量                   |

## MSFormer

### 问题

>剔除outlier一般是尽可能的获取上下文信息，尽管这些方法达到了良好的性能，但它们都不可差地处理每个对应关系，这在很大程度上增加了离群撤离的难度。
>
>这通过掩盖低量的对应关系来产生高质量的多重稀疏注意图，同时从空间和通道维度捕获全局上下文，以更好地删除更加离群值。