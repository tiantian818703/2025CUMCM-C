# NIPT 检测风险评估与推荐

本项目实现了基于 **无创产前检测（NIPT）** 数据的完整分析流程，  
涵盖数据清洗、建模、分组推荐与异常判定，针对竞赛题目 C 题设计。

---

## 📂 项目结构
├── C.ipynb # 主脚本，一键运行所有子问题
├── 附件.xlsx # 输入数据文件
├── nipt_outputs/ # 输出结果目录（运行脚本后自动生成）
│ ├── cleaned_data.csv
│ ├── Q1_ols_summary.txt
│ ├── Q1_pred_curves.png
│ ├── Q2_BMI_bins_recommendations.csv
│ ├── Q3_multifactor_bins.csv
│ ├── Q3_recommendation_CI.csv
│ ├── Q4_simple_rule.csv
│ ├── Q4_eval.csv
│ └── Q4_note.csv (若女胎数据不足则生成提示文件)
└── README.md

---

## 🚀 功能说明

### 问题1：男胎 Y 浓度与孕周、BMI 关系
- 使用 **B 样条回归** 建模 Y 浓度随孕周和 BMI 的变化规律  
- 输出回归结果摘要与预测曲线  
- 在图中绘制 **4% 达标阈值线**

### 问题2：BMI 分组与最佳检测时点
- 计算每位孕妇 **首次达到 Y≥4% 的孕周**  
- 利用回归树自动寻找 BMI 划分区间  
- 各区间取 **95% 分位数** 作为推荐检测时点，降低漏检风险  
- 同时进行 **3.5%/4.5% 阈值敏感性分析**

### 问题3：多因素逻辑回归
- 综合 **孕周、BMI、年龄、身高、体重** 等特征  
- 使用 **正则化逻辑回归** 估计达到 Y≥4% 的概率  
- 推荐 **命中概率 ≥95%** 的最早孕周  
- 采用 **自助法（bootstrap）** 给出推荐时点的置信区间

### 问题4：女胎异常判定
- **规则法**：若 max(Z13, Z18, Z21) ≥ 阈值，则判定异常  
- **逻辑回归法**：融合 Z 值、GC 含量、测序质量、BMI、年龄等特征  
- 输出模型评估指标（ROC-AUC、阈值、混淆矩阵）  
- 若样本不足，自动生成 `Q4_note.csv` 提示文件

---

## ⚙️ 环境依赖

- Python 3.8+
- 需要安装的依赖包：
  ```bash
  pip install pandas numpy matplotlib statsmodels scikit-learn patsy openpyxl
