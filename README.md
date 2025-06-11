# Telco Customer Churn Prediction

本项目旨在预测电信客户是否会流失（Churn），通过数据清洗、特征工程和多种机器学习模型的训练与优化，最终构建一个高效的分类器。

## 项目结构

- `Telco Customer Churn Prediction.ipynb`：主代码文件，包含数据处理、可视化、模型训练与评估。
- `WA_Fn-UseC_-Telco-Customer-Churn.csv`：原始数据文件。
- `WA_Fn-UseC_-Telco-Customer-Churn.xls`：原始数据的 Excel 格式。
- `solution - customer-churn-prediction.ipynb`：可能是另一个解决方案的笔记本文件。

## 数据集描述

数据集包含电信客户的相关信息，目标是预测客户是否会流失（`Churn`）。以下是主要字段的分类：

### 客户信息
- `customerID`：客户唯一标识符。
- `gender`：性别。
- `SeniorCitizen`：是否为老年人。
- `Partner`：是否有配偶。
- `Dependents`：是否有家属。

### 服务使用情况
- `tenure`：使用时长（以月为单位）。
- `PhoneService`：是否有电话服务。
- `MultipleLines`：是否有多条电话线。
- `InternetService`：上网服务类型。
- `OnlineSecurity`、`DeviceProtection`、`TechSupport` 等：是否订购相关服务。

### 合同与付费信息
- `Contract`：合同类型（月度、一年或两年）。
- `PaperlessBilling`：是否无纸化账单。
- `PaymentMethod`：支付方式。
- `MonthlyCharges`：每月费用。
- `TotalCharges`：总费用。

## 数据处理

1. **缺失值处理**：
   - `TotalCharges` 列中存在缺失值，原因是部分客户刚注册（`tenure=0`）。
   - 删除了这些缺失值行。

2. **特征工程**：
   - 将 `customerID` 设置为索引。
   - 将 `SeniorCitizen` 转换为分类变量。
   - 对分类变量进行独热编码（One-Hot Encoding）。
   - 对数值变量（`tenure`、`MonthlyCharges`、`TotalCharges`）进行标准化处理。

3. **数据可视化**：
   - 绘制了流失率的饼图。
   - 分析了不同特征与流失率的关系。
   - 使用 KDE 图比较了数值变量在流失与未流失客户中的分布。
   - 绘制了相关性热力图。

## 模型训练与评估

使用了多种机器学习模型进行训练和评估，包括：

1. **Logistic Regression**：
   - 使用网格搜索优化了正则化参数。
   - 绘制了 ROC 曲线。

2. **KNN**：
   - 基础模型，未进行超参数调优。

3. **SVC**：
   - 使用加权分类处理类别不平衡问题。

4. **Random Forest**：
   - 使用网格搜索优化了超参数。
   - 最佳参数：`{'bootstrap': True, 'max_depth': 8, 'max_features': 'sqrt', 'min_samples_leaf': 2, 'min_samples_split': 2, 'n_estimators': 100}`。

5. **AdaBoost**：
   - 使用网格搜索优化了学习率和基分类器数量。

6. **LightGBM**：
   - 使用随机搜索优化了超参数。

7. **Voting Classifier**：
   - 集成了 Logistic Regression、Random Forest 和 AdaBoost 模型，采用软投票方式。

### 最终模型表现

使用 Voting Classifier 集成模型，最终的分类报告如下：

Final Accuracy Score 
0.8170616113744076
Classification Report:
| Metric / Class        | Precision | Recall   | F1-score | Support   |
| --------------------- | --------- | -------- | -------- | --------- |
| **False (Not Churn)** | **0.85**  | **0.91** | **0.88** | **1 549** |
| **True (Churn)**      | 0.70      | 0.55     | 0.62     | 561       |
| **Accuracy**          | –         | –        | **0.82** | **2 110** |
| **Macro Avg**         | 0.77      | 0.73     | 0.75     | 2 110     |
| **Weighted Avg**      | 0.81      | 0.82     | 0.81     | 2 110     |

# 未来改进方向
尝试更多的特征工程方法，例如特征交互或特征选择。
使用更多的集成学习方法（如 Stacking）。
进一步优化类别不平衡问题。
部署模型并集成到实际业务流程中