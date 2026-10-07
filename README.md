# Student Test Score Prediction

基于 Kaggle Playground Series S6E1 的学生考试成绩预测项目。

## 项目简介

本项目使用学生的人口统计、学习行为与考试环境数据，预测学生的考试成绩（回归任务）。数据包含约 63 万条训练样本和 27 万条测试样本，目标变量为 `exam_score`。

## 数据字段

- **人口统计**：age、gender
- **学习行为**：study_hours、class_attendance、sleep_hours、study_method
- **环境因素**：course、internet_access、sleep_quality、facility_rating、exam_difficulty

## 方法与流程

1. **特征工程**：构造 4 个新特征
   - `study_attendance` = study_hours × class_attendance
   - `study_sleep_ratio` = study_hours / (sleep_hours + 1)
   - `attendance_study_diff` = class_attendance - study_hours
   - `sleep_hours_sq` = sleep_hours²

2. **预处理**：数值特征标准化，分类特征 One-Hot 编码。

3. **模型融合**：
   - 先用 Ridge 回归建立线性基准。
   - 再用 LightGBM 学习 Ridge 的残差。
   - 最终预测 = Ridge 预测 + LightGBM 残差修正。

4. **交叉验证**：5 折 KFold，每折分别训练 Ridge 和 LightGBM，避免数据泄露。

## 结果

- 5 折 OOF RMSE：**8.7657**
- 每折 RMSE 稳定在 8.75–8.79 之间，模型泛化能力稳定。

## 文件说明

| 文件 | 说明 |
|---|---|
| `student-score-prediction.ipynb` | 完整流程代码 |
| `submission.csv` | 提交文件（id + exam_score） |
| `requirements.txt` | 依赖库 |

## 如何运行

1. 在 Kaggle 挂载 Playground S6E1 数据集（路径：`/kaggle/input/competitions/playground-series-s6e1`）。
2. 安装依赖：`pip install -r requirements.txt`
3. 按顺序运行 Notebook 代码块。
4. 生成 `submission.csv` 并提交到竞赛页面。

## 链接

- Kaggle 竞赛：https://www.kaggle.com/competitions/playground-series-s6e1
- 我的 Kaggle 主页：https://www.kaggle.com/sunnyrainy0212
