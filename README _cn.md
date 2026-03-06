# TripAdvisor 推荐系统项目

基于评论文本相似度的 TripAdvisor 地点推荐系统

## 项目概述

本项目构建了一个文本驱动的地点推荐系统，通过分析 TripAdvisor 评论文本，为用户推荐相似的旅游地点。

**核心假设**: 相似的旅游体验会用相似的词汇描述

**项目成果**:
- 实现了 BM25 baseline 模型和 TF-IDF+Cosine 改进模型
- 达到 85%+ 的推荐准确率（Success@1）
- 改进模型在所有指标上优于 baseline

---

## 数据集

- **评论数量**: 340,385 条
- **地点数量**: 2,161 个
- **语言**: 多语言（英语、法语为主）
- **地点类型**: 酒店（H）、餐厅（R）、景点（A）、景点产品（AP）

**数据文件**:
```
reviews83325.csv          # 评论数据
Tripadvisor.csv          # 地点元数据
AttractionSubCategorie.csv
AttractionSubType.csv
cuisine.csv
dietary_restrictions.csv
restaurantType.csv
```

---

## 快速开始

### 1. 环境配置

```bash
# 安装依赖
pip install -r requirements.txt

# NLTK 数据下载
python -c "import nltk; nltk.download('stopwords'); nltk.download('wordnet'); nltk.download('punkt')"
```

### 2. 运行流程

```bash
# Step 0: 数据诊断（可选）
python 0_diagnose_data.py

# Step 1: 数据探索（可选）
python 1_data_exploration.py

# Step 2: 数据预处理
python 2_preprocessing.py
# 输出: documents_by_place.csv

# Step 3: TF-IDF 平衡
python 3_balance_tfidf.py
# 输出: documents_by_place_tfidf.csv

# Step 4: 训练 BM25 模型
python 4_baseline_bm25.py
# 输出: bm25_model.pkl

# Step 5: 训练 TF-IDF+Cosine 模型
python 5_tfidf_cosine_model.py
# 输出: tfidf_cosine_model.pkl

# Step 6: 评估 BM25（可选）
python 6_evaluation_standalone.py
# 输出: evaluation_results_bm25.json

# Step 7: 对比两个模型
python 7_compare_models.py
# 输出: model_comparison.json
```

---

## 项目结构

```
.
├── 0_diagnose_data.py              # 数据诊断工具
├── 1_data_exploration.py           # 数据探索与可视化
├── 2_preprocessing.py              # 文本预处理
├── 3_balance_tfidf.py              # TF-IDF 平衡（关键步骤）
├── 4_baseline_bm25.py              # BM25 baseline 模型
├── 5_tfidf_cosine_model.py         # TF-IDF+Cosine 改进模型
├── 6_evaluation_standalone.py      # 单独评估 BM25
├── 7_compare_models.py             # 模型对比评估
├── requirements.txt                # Python 依赖
├── README.md                       # 本文件
└── 报告/
    ├── TripAdvisor_完整报告_中文版.docx
    └── TripAdvisor_Report_English.docx
```

---

## 数据处理流程

### 1. 文本预处理
- 转小写
- 去除 HTML、URL、标点、数字
- 分词
- 去停用词（英语 + 法语）
- 词形还原

### 2. 文档聚合
- 将每个地点的所有评论合并为一个文档

### 3. TF-IDF 平衡 (关键步骤)
- 使用 TF-IDF 为每个地点选择 Top 100 关键词
- 解决评论数量不平衡问题（1 ~ 71,257 条/地点）
- 确保公平比较

### 4. 数据分割
- 训练集: 50% (1,080 地点)
- 测试集: 50% (1,081 地点)

---

## 模型说明

### BM25 (Baseline)
- **原理**: 概率排序函数
- **特点**: 词频 + IDF + 长度归一化
- **参数**: k1=1.5, b=0.75

### TF-IDF + Cosine Similarity (改进模型)
- **原理**: 向量空间模型 + 余弦相似度
- **特点**: 
  - 使用 bigram (1,2) 捕捉短语
  - max_features = 5000
  - 更好的语义匹配
- **优势**: 在两个评估级别上均优于 BM25

---

## 评估结果

### Level 1 (类型匹配: H/R/A/AP)

| 模型 | Mean Ranking Error | Success@1 | Success@10 |
|------|-------------------|-----------|------------|
| BM25 | 0.435 | 85.09% | 98.98% |
| TF-IDF+Cosine | **0.400** | **85.66%** | **99.26%** |

### Level 2 (细分类别匹配)

| 模型 | Mean Ranking Error | Success@1 | Success@10 |
|------|-------------------|-----------|------------|
| BM25 | 0.726 | 81.08% | 97.95% |
| TF-IDF+Cosine | **0.694** | **82.67%** | **98.23%** |

**结论**: TF-IDF+Cosine 在所有指标上优于 BM25

---

## 依赖包

```txt
pandas>=1.5.0
numpy>=1.23.0
scikit-learn>=1.2.0
rank-bm25>=0.2.2
nltk>=3.8
tqdm>=4.65.0
matplotlib>=3.6.0
seaborn>=0.12.0
```

---

## 关键技术点

1. **TF-IDF 平衡策略** (遵循教师建议)
   - 每个地点选择 Top 100 关键词
   - 解决数据不平衡问题
   - 提升推荐质量

2. **Bigram 特征**
   - 捕捉多词短语（art museum, Italian restaurant）
   - 提升细分类别匹配准确率

3. **两级评估体系**
   - Level 1: 粗粒度类型匹配
   - Level 2: 细粒度属性匹配

---

## 主要成果

1. 实现并对比了两个推荐模型
2. 达到 85%+ 的推荐准确率
3. 验证了文本相似度推荐的有效性
4. TF-IDF+Cosine 模型优于 BM25 baseline

---

## 报告

完整的项目报告见 `报告/` 目录:
- `TripAdvisor_完整报告_中文版.docx`
- `TripAdvisor_Report_English.docx`

报告包含:
- 详细的数据处理流程
- 模型设计与参数说明
- 完整的评估结果与分析
- 模型对比与讨论

---

## 注意事项

1. **数据文件**: 确保所有 CSV 文件在同一目录
2. **内存**: 预处理可能需要 2-4 GB 内存
3. **时间**: 完整流程约 15-20 分钟
4. **NLTK**: 首次运行需要下载语言资源

---

## 常见问题

**Q: 如何只测试部分数据?**
```python
# 在 2_preprocessing.py 修改
reviews_df = pd.read_csv('reviews83325.csv', nrows=10000)
```

**Q: 如何调整 TF-IDF 选择的词数?**
```python
# 在 3_balance_tfidf.py 修改
TOP_N_WORDS = 100  # 改为 50, 150, 200 等
```

**Q: 如何只使用英语评论?**
```python
# 在 2_preprocessing.py 修改
USE_ENGLISH_ONLY = True
```
