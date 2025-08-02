# DeMatch



这篇CVPR 2024论文《**DeMatch: Deep Decomposition of Motion Field for Two-View Correspondence Learning**》提出了一种新颖的两视图匹配方法，称为 **DeMatch**，核心思想是将**稀疏运动矢量分解为多个“低频”子场（sub-fields）**，进而**重建一个更光滑、更干净的运动场（motion field）**，以提升匹配鲁棒性与效率。下面我帮你做一个中文总结和分析：

------

## 🧠 一、研究背景与挑战

### 两视图匹配任务：

- 给定两张图像，找出表示**同一物理点**的匹配点对。
- 关键问题：**初始匹配集合中存在大量误匹配（outliers）**。

### 现有方法存在的问题：

1. **基于全局正则化的方法（如LMCNet）**：
   - 利用稀疏图或Laplacian正则项保证全局一致性。
   - 问题：**计算复杂度高**。
2. **基于局部滤波的方法（如ConvMatch）**：
   - 用CNN做局部平滑。
   - 问题：容易**过度平滑，无法保留不连续性**，尤其在大视差场景。

## 🧱 三、方法结构分析（DeMatch Architecture）

### 整体流程（图2）：

1. **输入**：N个稀疏匹配点对 $(x_i, y_i)$，生成运动向量 $m_i = (x_i, y_i - x_i)$。
2. **初始化**：使用embedding模块将 $m_i\in \mathbb{R}^4$ 映射到高维空间 $f_i \in \mathbb{R}^C$。
3. **Decomposition**：
   - 学习K个可训练的“motion basis” $b_k$。
   - 用注意力机制计算每个 $f_i$ 与 $b_k$ 的相似度，得到软分配权重。
   - 聚类得到多个平滑子场 $F_k$，每个由一类运动模式支持。
   - 使用**masked decomposition策略**：引导去除低置信度匹配。
4. **Global Enhancement**：通过图注意力网络（GAT）强化全局上下文。
5. **Recovery**：
   - 从子场 FkF_kFk 的表示 zkz_kzk 恢复新的运动场 F~F̃F~。
   - 形成新的运动向量 fi′f_i'fi′，与原始向量对比可检测 outlier。
6. **预测 Inlier**：
   - 计算 ∣fi′−fi∣|f_i' - f_i|∣fi′−fi∣ 并通过预测器估算 inlier 概率 pip_ipi。