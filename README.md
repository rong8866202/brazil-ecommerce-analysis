# 巴西电商用户行为分析项目

## 项目背景
本项目基于巴西 Olist 公开电商数据集，通过**数据清洗 → 特征工程 → 可视化分析**的完整流程，挖掘用户消费行为、订单表现和产品销售特征，为电商平台运营、品类管理和用户增长提供数据驱动的决策建议。

## 数据来源
- 数据集来自 Kaggle 公开的 [Brazilian E-Commerce Public Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
- 包含 9 张核心表：用户表、订单表、订单明细表、支付表、评价表、商品表、商家表、地理位置表、品类翻译表
- 数据时间范围：2016年9月 – 2018年10月

---

## 核心分析流程

### 1. **数据清洗（ecommerce_analysis_01_data_cleaning.ipynb）**
- **缺失值处理**：
  - orders 表：根据订单状态保留合理缺失，新增 has_approved、has_shipped、has_delivered 布尔特征，标记异常订单。
  - products 表：填充类别缺失为 unknown，用 0 填充描述长度，用同类均值填充重量/尺寸（各 2 条）。
  - reviews 表：保留评论文本缺失，不处理。
- **异常值处理**：
  - 检查金额字段无负数。
  - 处理0元订单：共4笔，其中3笔为正常取消订单，1笔为已送达但支付缺失（已删除，避免干扰指标计算）。
  - 处理配送时长缺失值：占比2.98%，删除后不影响样本分布。
- **数据类型检查**：确保金额为 float，数量为 int，日期为 datetime。

- **数据合并**：构建‘customer_order_merge.csv‘、’order_merge.csv‘、’product_seller_merge.csv‘，为后续分析奠定基础

### 2. **特征工程（ecommerce_analysis_02_feature_engineering.ipynb）**
- **用户维度特征**：计算每个用户的订单数、总消费金额、平均客单价、首次/末次购买时间等。
- **订单维度特征**：计算订单总金额（含运费）、商品件数、配送时长、是否延迟送达等。
- **商品维度特征**：聚合品类销售额、销量、平均评分、各品类销售额最高城市。

- 输出 3 张核心特征表：`user_features.csv`、`order_features.csv`、`category_features.csv`。

### 3. **可视化分析与业务洞察（ecommerce_analysis_03_visualization.ipynb）**
#### 3.1 用户行为分析
- 验证用户仅下过1次订单，复购率为0，平台以一次性消费为主。
- 客单价呈右偏分布：0–200元订单占比超75%，少量高客单价订单来自企业级采购（如工业/商业品类）。
- 平台订单量高速增长：2017–2018年月订单量从800单攀升至7000+单，增长近10倍。

#### 3.2 订单表现分析
- 订单金额集中在0–500元区间（占比超95%），中位数106.97元，均值167.37元，小额消费为核心场景。
- 配送时效稳定：92.13%订单按时送达，延迟订单仅占7.87%，配送时长中位数10天。

#### 3.3 产品表现分析
- 品类头部效应显著：销售额/销量Top10品类合计贡献超62%的整体业绩。
- 核心品类：`beleza_saude`（美容健康）在销售额位列第一，销量位列第二；cama_mesa_banho（床单浴室）在销量位列第一，销售额位列第三，是平台高价值高频品类。
- 评分表现：整体评分偏高（中位数4.1分），消费者对平台普遍满意，头部品类评分高度集中。
- 地域销售集中：所有品类销售额最高的城市均为圣保罗（sao paulo），符合巴西商业核心城市特征。

### 查看完整报告
- 项目完整分析报告已合并为 `ecommerce_analysis_01_02_03.pdf`，因 GitHub 对 Jupyter 导出 PDF 的预览兼容性有限制，请点击文件名 → 选择「Download」下载后本地查看，报告包含完整的数据分析流程与业务洞察。

---


### 核心结论
1. 用户粘性弱，需加强复购激励。
2. 小额消费主导，可通过满减包邮提升客单价。
3. 物流时效稳定，延迟率 7.87%，仍有优化空间。
4. 头部品类集中，应重点维护并挖掘潜力品类。
5. 高评分小众品类适合口碑营销，低分品类需改进质量。
6. 圣保罗是消费中心，可向其他城市拓展。

### 业务建议
1. **用户运营**：搭建复购激励体系（优惠券/会员积分），针对企业采购用户提供定制化服务。
2. **订单与物流**：针对0–200元核心区间设计满减/包邮活动，优化延迟订单的地域配送方案。
3. **品类管理**：深耕头部高价值品类，整改低评分品类，向里约热内卢等大城市拓展销售。
4. **数据建设**：补全2016年11月缺失数据，搭建核心指标监控看板，实现业务异常实时预警。

---

## 运行说明
1. 克隆项目到本地：`git clone https://github.com/[你的用户名]/brazil_ecommerce_project.git`
2. 安装依赖：`pip install pandas numpy matplotlib seaborn nbconvert[webpdf]`
3. 运行 Notebook：在 `00_notebooks/` 目录下执行 `jupyter notebook`

---

## 技术栈
- **数据处理**：Python (pandas, numpy)
- **可视化分析**：matplotlib, seaborn
- **开发环境**：Jupyter Notebook
- **版本控制**：Git / GitHub
- **文档导出**：Jupyter nbconvert (WebPDF)

## 📁 项目结构
```text
brazil_ecommerce_project/
├── brazil_ecommerce_project/
│   ├── cleaned_data/          # 清洗后的数据
│   │   ├── cleaned_orders.csv
│   │   └── cleaned_products.csv
│   ├── features/              # 维度特征表
│   │   ├── product_features/
│   │   ├── order_features/
│   │   └── user_features/
│   ├── 00_notebooks/          # 分析代码
│   │   ├── ecommerce_analysis_01_data_cleaning.ipynb
│   │   ├── ecommerce_analysis_02_feature_engineering.ipynb
│   │   └── ecommerce_analysis_03_visualization.ipynb
│   ├── merges/                # 合并后的宽表
│   │   ├── product_seller_merge.csv
│   │   ├── order_merge.csv
│   │   └── customer_order_merge.csv
│   ├── README.md
│   └── requirements.txt       # Python依赖
└── 运行环境
