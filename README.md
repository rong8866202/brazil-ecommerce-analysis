巴西电商用户行为分析 - 项目 README

📌 项目简介

本项目使用巴西电商（Olist）公开数据集，对用户行为、订单特征、产品表现进行端到端的数据分析。通过数据清洗、特征工程和可视化分析，探索用户消费模式、订单配送时效、产品品类表现等核心业务问题，并为电商运营提供数据驱动的洞察建议。

📁 数据来源

数据集来自 Kaggle：Brazilian E-Commerce Public Dataset by Olist。包含约 150 万条订单记录，涵盖客户、订单、支付、评价、商品、卖家等维度。

🛠️ 分析流程

1. 数据清洗（00_notebooks/ecommerce_analysis_01_data_cleaning.ipynb）

加载原始表（customers、orders、order_items、order_payments、order_reviews、products、sellers、geolocation、product_category_name_translation）。
探查各表基本信息、缺失值、异常值。
缺失值处理：

orders 表：根据订单状态保留合理缺失，新增 has_approved、has_shipped、has_delivered 布尔特征，并标记异常订单。
products 表：对类别缺失填充 unknown，用 0 填充描述长度，用同类均值填充重量/尺寸。
reviews 表：保留文本缺失，不处理。
数据类型检查：确保金额字段为 float，数量字段为 int，日期字段为 datetime。
异常值检查：确认金额无负数。

保存清洗后数据至 cleaned_orders/cleaned_products/。

2. 特征工程（00_notebooks/ecommerce_analysis_02_feature_engineering.ipynb）

构建核心宽表：

customer_order：客户与订单表连接，用于用户维度分析。
order_merge：订单明细宽表（订单 + 商品 + 支付 + 评价），用于订单维度分析。
product_seller_merge：商品与商家宽表，用于产品维度分析。

保存宽表至：product_seller_merge/order_merge/customer_order_merge

用户维度特征：
订单数、是否复购、总消费额、平均客单价、首次/末次购买时间。

订单维度特征：
订单总金额（含运费）、商品件数、配送时长、是否延迟。

产品维度特征：
品类销售额、销量、平均评分、各品类销售额最高的城市。

保存特征表至 product_features/order_features/user_features 。

3. 可视化分析与业务洞察（00_notebooks/ecommerce_analysis_03_visualization.ipynb）

用户行为：
用户订单分布（验证多数用户仅一单）。
客单价分布（箱线图+直方图），识别并处理极端值（企业采购）和0元订单（取消或异常）。

用户活跃时间：
每月订单量趋势（折线图），发现快速增长趋势与季节性。

订单表现：
订单金额分布（直方图+描述统计），指出右偏分布和极值影响。
（待续）更多分析如配送时效分布、评分分析、品类销售排行等。

📊 核心发现

用户特征：

绝大多数用户（99%以上）仅下单一次，复购率极低，平台以新客为主。
客单价中位数为 110 元，均值 206 元，呈右偏分布；存在少量极高客单价（10 万元以上），多为企业级采购（固定电话、汽车配件等）。
0 元订单主要来自已取消订单，仅一条异常订单（已送达但支付缺失）已剔除。

订单趋势：

订单量从 2016 年 9 月到 2018 年 10 月增长近 10 倍，其中 2017 年初起进入快速增长期。
2016 年 11 月订单量为 0，可能是数据缺失或业务暂停。

订单金额：

90% 的订单金额在 388 元以下，95% 在 644 元以下，集中于 0–500 元区间。
少量高额订单显著拉高均值，但不影响主流消费区间。

产品品类（部分分析）：

销售额最高的品类为 beleza_saude（美容健康）、relogios_presentes（手表礼品）等，圣保罗是绝大多数品类的消费中心。

📁 文件结构

text
brazil_ecommerce_project/
├── brazil_ecommerce_project/
│   ├── cleaned_data/          # 清洗后的数据
│   │   ├── cleaned_orders.csv
│   │   └── cleaned_products.csv
│   ├── features/              # 维度特征表
│   │   ├── product_features/
│   │   ├── order_features/
│   │   └── user_features/
│   ├── 00_notebooks/             # 分析代码
│   │   ├── ecommerce_analysis_01_data_cleaning.ipynb
│   │   ├── ecommerce_analysis_02_feature_engineering.ipynb
│   │   └── ecommerce_analysis_03_visualization.ipynb
│   ├── merges/                # 合并后的宽表
│   │   ├── product_seller_merge.csv
│   │   ├── order_merge.csv
│   │   └── customer_order_merge.csv
│   ├── README.md
│   └── requirements.txt       # Python依赖
└── 🔧 运行环境


Python 3.8+
依赖库：pandas, numpy, matplotlib, seaborn, jupyter

📌 后续可扩展分析:
配送时效与延迟率的地理分布。
评分与订单金额、配送时长的关系。
用户复购的预测模型。
高价值用户画像。

说明：本项目为数据分析练习，数据集来自公开来源，分析结果仅用于学习和展示。如需复现，请确保路径与文件命名一致。
