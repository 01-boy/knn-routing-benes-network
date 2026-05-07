# Intelligent Machine Learning-Based Routing with Feature Extraction in Optical Benes Networks

本项目是论文 **"Intelligent Machine Learning-Based Routing with Feature Extraction in Optical Benes Networks"** 的官方开源仓库。该研究提出了一种结合特征提取模块与 K-最近邻（KNN）算法的智能路由框架，旨在优化 8x8 光学 Benes 网络中的路径质量并规避最坏情况下的路径损耗。

## 核心研究成果

* **高准确率**：通过特征提取，路由预测准确率从 55% 显著提升至 **72.85%**。
* **低计算开销**：单次迭代的计算时间仅为 **0.1-1 ms**，显著低于 CNN (5-10 ms) 和 SVM (1-2 ms) 模型。
* **信号质量提升**：相比传统路由算法，该方案有效降低了功率惩罚，提升了消光比（EXT），并显著降低了符号错误率（SER）。
* **仿真验证**：基于 30 Gbps PAM4 传输系统，在 Lumerical INTERCONNECT (2020R2) 平台上进行了完整验证。

## 数据集说明 (Dataset Description)

本仓库包含 10,000 条从 8x8 Benes 网络全状态空间（约一百万种配置）中均匀抽样的数据。

### 文件列表

| 原始文件名 | 建议命名 | 说明 |
| :--- | :--- | :--- |
| `processed_10000_features_avg.txt` | `benes8x8_features_and_labels_10k.txt` | **核心数据**：包含提取的 f1, f2, f3 特征及对应的分类标签。 |
| `data10000_avg.txt` | `benes8x8_ext_measurements_10k.txt` | 原始仿真测量数据：包含每条路径的平均消光比（EXT）数值。 |
| `processed_10000_data.txt` | `benes8x8_routing_configs_10k.txt` | 路由配置数据：记录 10,000 组开关状态序列。 |
| `X8c.txt` | `raw_8x8_switch_states.txt` | 拓扑映射与原始开关状态参考文件。 |

### 数据字典 (Data Dictionary)

* **物理状态映射**：
    * `0`：代表开关处于 **Bar** 状态。
    * `1`：代表开关处于 **Cross** 状态。
* **提取特征定义**：
    * **f1 (Crosstalk)**：对应信号首次经历严重交叉串扰的阶段。
    * **f2 (Power Efficiency)**：网络中 Cross 状态的总数，用以评估整体功率效率。
    * **f3 (Path Dependency)**：代表路径重叠减少的情况，用以降低路径依赖。

## 运行环境

* Python 3.8+
* Pandas
* Scikit-learn

## 使用方法

运行仓库中的 Python 脚本以复现论文中的 KNN 分类结果（参数设定：K=5, Test Size=30%）：

```python
# 核心逻辑示例
from sklearn.neighbors import KNeighborsClassifier
knn = KNeighborsClassifier(n_neighbors=5, metric='euclidean')
