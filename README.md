# 电商商业数据分析 · End-to-End Data Analysis Pipeline

> 基于 Kaggle 公开数据集（Olist 巴西电商），搭建从**数据采集 → 清洗 → 探索性分析(EDA) → 业务分析 → 可视化**的端到端数据分析流水线，模拟企业"用数据驱动增长"的典型工作流，输出销售趋势、用户价值分层、类目结构、地域分布与交付质量等决策洞察。
>
> 适用场景：大数据技术专业 · 求职作品集 / 简历项目

[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-blue.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/)
[![Python](https://img.shields.io/badge/Python-3.13-3776AB.svg)](https://www.python.org/)
[![Built with](https://img.shields.io/badge/built%20with-pandas%20%7C%20ECharts-0e9488.svg)]()

---

## 一、项目背景与业务目标

电商日常运营持续产生海量交易、用户与商品数据，但原始数据分散、噪声大，难以直接支撑经营决策。本项目以一套真实结构的电商订单数据集为对象，沉淀一套可复用的分析流程方法论，体现**工程化 + 业务解读**双重能力。

**业务目标**

- 量化销售表现与用户价值，识别高贡献客群与爆款品类；
- 挖掘地域分布规律，为区域化运营与物流策略提供依据；
- 定位交付异常，支撑履约效率优化；
- 产出可交互看板与完整报告，让结论"看得见、讲得清"。

---

## 二、技术栈

| 环节 | 工具 | 说明 |
| --- | --- | --- |
| 语言 | Python 3.13 | 统一管理于隔离虚拟环境 |
| 数据采集 | kagglehub | 匿名下载公开数据集，**无需 Kaggle 账号 / 付费** |
| 数据处理 | pandas / numpy | 清洗、聚合、特征工程 |
| 用户分层 | RFM 模型 | Recency / Frequency / Monetary |
| 数据可视化 | ECharts | 交互式看板，可直接嵌入网页 |
| 工程规范 | 分层目录 | 原始层 / 清洗层 / 分析层 / EDA 层 分离 |

---

## 三、数据来源与规模

- **来源**：Olist Brazilian E-Commerce Dataset（Kaggle: `olistbr/brazilian-ecommerce`），许可 **CC BY-NC-SA 4.0**
- **原始层**：9 张表，合计 **1,550,922** 行
- **清洗层**：以订单为主键聚合为宽表 **99,441 行 / 17 列**
- **时间跨度**：2016-09 ~ 2018-10（约两年）

> 说明：本项目使用公开授权数据，规避了直接爬取商业平台的法律与合规风险；用户 ID 已为平台脱敏字段，未采集任何个人身份信息（PII）。

---

## 四、项目流程架构

```
数据采集(kagglehub)  →  数据清洗(订单宽表)  →  探索分析(EDA)  →  业务分析(RFM/趋势/类目/地域/交付)  →  可视化(ECharts 看板)
   data/olist/              data/clean/           data/eda/*.csv        data/analysis/*.csv                dashboard.html
```

---

## 五、核心分析结论

| 编号 | 业务问题 | 关键结论 |
| --- | --- | --- |
| A1 | 销售趋势与季节性 | 跨度 2016-09~2018-10，总销售额 **R$ 16,008,872**，订单 **99,441** |
| A2 | 用户价值分层（RFM） | 客户 **96,096** 人，五群较均衡；**重要价值客户 15,376**，流失风险 15,293 |
| A3 | 商品类目结构 | Top3：`健康美容`、`手表礼品`、`床品卫浴餐桌` |
| A4 | 地域分布 | 销售集中于东南沿海：Top3 州 **SP / RJ / MG** |
| A5 | 交付与未成交 | **准时率 94.1%**，取消 / 不可用 **1,234** 单 |
| A6 | 交付 × 评分 | **严重迟到(>7天)订单 77.1% 给低分(≤2)**，验证"物流差 → 差评" |

---

## 六、项目结构

```
ecommerce-analysis/
├── data/
│   ├── olist/          # 原始层：9 张 CSV（kagglehub 下载）
│   ├── clean/          # 清洗层：olist_orders_clean.csv（订单宽表）
│   ├── analysis/       # 分析层：聚合 CSV + dashboard.html（看板）
│   ├── eda/            # 探索层：describe + 6 维度分组 CSV + 热力图 + 报告
│   └── refs/           # 参照表：品类英中对照 category_zh.csv
├── scripts/
│   ├── download_olist.py       # ① 采集
│   ├── clean_olist.py          # ② 清洗
│   ├── analyze_olist.py        # ③ 分析 + 可视化（含延迟×评分热力图）
│   ├── eda_olist.py            # ④ 探索性分析(EDA)
│   └── localize_categories.py  # 品类本地化(英→中)
├── report.html           # 完整交互报告（作品集入口）
├── report.md                 # 标准 Markdown 报告（适配 GitHub）
├── 面试口述稿.md               # 2 分钟面试讲述稿
├── 面试题清单.md               # 高频面试题 + 参考回答
├── plan.html
├── collection.html
├── 简历项目描述.md
├── index.html                  # GitHub Pages 门户页
└── README.md
```

---

## 七、快速开始

```bash
# 1. 安装依赖（kagglehub 用官方 PyPI 源更稳）
pip install kagglehub pandas

# 2. 依次运行流水线
python scripts/download_olist.py       # 下载原始数据到 data/olist
python scripts/clean_olist.py          # 清洗为订单宽表 data/clean
python scripts/analyze_olist.py        # 分析并生成看板 data/analysis/dashboard.html
python scripts/eda_olist.py            # 探索性分析，导出 CSV + 报告到 data/eda
python scripts/localize_categories.py  # 品类英文名转中文
```

完成后：
- 看板：`data/analysis/dashboard.html`
- 完整报告：`report.html`
- Markdown 报告：`report.md`
- EDA 报告：`data/eda/eda_report.md`

---

## 八、局限性说明

- **数据地域局限**：数据来自巴西电商市场，结论不能直接套用于国内电商，但分析框架可平移；
- **时间跨度**：样本约两年，难以捕捉更长周期与跨年规律，预测性结论置信度有限；
- **未成交口径**：Olist 无显式退货字段，未成交以 `order_status`（canceled / unavailable）近似，非严格退货；
- **维度缺失**：缺少营销活动、广告投放等标签，归因与因果推断存在盲区；
- **规模边界**：以单机中小数据量为主，重在方法论而非分布式生产环境极限。

---

## 九、发布到 GitHub Pages（作品集站点）

本仓库已配置 `index.html` 作为门户页，发布步骤：

1. 在 GitHub 新建仓库并 `git push` 本仓库内容；
2. 仓库 **Settings → Pages → Build and deployment → Source** 选择 **Deploy from a branch**；
3. **Branch** 选 `main`，目录选 `/ (root)`，保存；
4. 等待一两分钟，访问 `https://<你的用户名>.github.io/<仓库名>/` 即可看到作品集门户页（含完整报告与看板跳转）。

> 提示：`.gitignore` 已排除原始大数据集 `data/olist/` 与本地环境文件，仓库保持轻量、可复现。

---

## 十、许可证与数据声明

- 数据集：© Olist，基于 **CC BY-NC-SA 4.0** 许可使用；
- 本项目代码与文档仅用于**学习研究**，不用于商业用途；
- 分析结论基于脱敏公开样本，落地业务前需结合真实数据复核。

---

*作者：大数据技术专业 · 求职作品集项目*
