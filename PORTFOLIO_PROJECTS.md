# 数据专员项目组合

本工作区包含三个可直接运行、可验证、可写进简历的数据项目，均围绕数据专员岗位要求设计。

## 项目总览

| 项目 | 目录 | 核心技术 | 主要交付物 | 验证结果 |
|---|---|---|---|---|
| SQL 数据仓库 + Tableau 经营看板 | `01_sql_dwh_tableau_dashboard` | Python、SQLite、SQL、Tableau SDK | SQLite 仓库、指标视图、Tableau `.twb/.twbx`、4 个看板、HTML 预览 | 113,306 条事实数据；9 项质量校验通过；6 项测试通过 |
| Excel + Power Query 自动化经营报表 | `02_excel_power_query_report` | Excel、Power Query M、Python | 可刷新 Excel 报表、6 条 M 查询、数据透视表、KPI 看板、异常表、字段口径 | 41,280 行干净事实表；6/6 查询实际刷新通过；4 项测试通过 |
| 通用数据质量监控工具 | `03_data_quality_monitor` | Python、Pandas、SQLite、MySQL 可选、HTML | 规则引擎、历史库、问题清单、HTML 报告、Webhook 告警 | 28 项检查；35 个问题准确检出；4 项测试通过 |

## 简历表述

### SQL 数据仓库 + Tableau 经营看板

- 使用 Python 清洗 11 万余条销售明细，设计日期、客户、商品和销售事实星型模型。
- 编写 KPI、同比/环比、品类排名、区域贡献和 RFM SQL 分析视图。
- 使用 Tableau SDK 生成四页经营看板并打包 `.twbx`，输出指标字典和可复现数据文件。
- 完成主外键、金额公式和指标对账测试，9 项质量校验全部通过。

### Excel + Power Query 自动化经营报表

- 将销售、商品、退货和目标四类数据清洗、合并为 41,280 行事实表。
- 编写 6 条 Power Query M 查询，实现销售额、成本、毛利、退货率、目标完成率和环比自动化计算。
- 建立经营看板、字段口径、异常清单和可刷新报表，并通过 Mashup OLEDB 实际刷新验证全部查询。
- 问题定位覆盖无效商品、异常数量、折扣越界和商品匹配失败。

### 通用数据质量监控工具

- 实现空值率、唯一性、格式、数值范围、主外键、跨表一致性和数据新鲜度等 12 类规则。
- 配置 28 项检查，在 3 张样例表中准确发现 35 个问题，其中高严重度 31 个。
- 使用 SQLite 保存运行历史、问题首次/最近出现时间和闭环状态，输出 HTML、CSV 和 JSON 报告。
- 支持 CSV/MySQL 数据源和可选 Webhook 告警。

## 一键运行

```powershell
$env:PYTHON_EXE = "C:\Users\27800\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe"
$env:NODE_EXE = "C:\Users\27800\.cache\codex-runtimes\codex-primary-runtime\dependencies\node\bin\node.exe"

.\01_sql_dwh_tableau_dashboard\run.ps1
.\02_excel_power_query_report\run.ps1
.\03_data_quality_monitor\run.ps1
```

## 主要验收证据

- `01_sql_dwh_tableau_dashboard/output/validation_report.json`
- `01_sql_dwh_tableau_dashboard/output/tableau/tableau_validation.json`
- `02_excel_power_query_report/output/data_validation_report.json`
- `02_excel_power_query_report/output/powerquery_refresh_report.json`
- `03_data_quality_monitor/output/run_summary.json`
- `03_data_quality_monitor/output/dq_history.sqlite`

## 已知边界

- 当前机器没有 Tableau Desktop，因此 `.twbx` 已完成 XML、工作表、仪表板和 ZIP 结构验证，Tableau 内的最终视觉样式仍需在安装 Tableau 的电脑上打开确认。
- Excel 工作簿中的 Power Query 已通过 Excel Mashup OLEDB 连接实际刷新，验证的是查询执行结果，不依赖静态预览数据。
- MySQL 数据源适配器已实现，但实际运行使用 SQLite/CSV 进行可重复测试；MySQL 示例配置位于 `03_data_quality_monitor/config/mysql.example.json`。