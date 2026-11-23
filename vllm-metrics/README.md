# vLLM 官方指标 / CLI flag 白名单库

2026-11-22 立项｜数据仓库「权威官方源机器可读事实」第 8 仓。

## 定位
- 本目录 = **vLLM 官方指标/flag 白名单权威源**，供 `deploy/validate_gate.py` 的 G4 闸门核验：
  文章引用的 `vllm:*` 指标名、`--flag` 必须在白名单内，否则 G4 直接 FAIL 阻断部署（源头遏制编造）。
- 物理隔离：不进 `src/site/`，`deploy_cf_pages.py` 只传 `src/site/`，**绝不进部署产物**。
- 只收**权威官方源**且**对得上官方源**的事实；对不上就不收（宁缺毋滥）。
- 每条事实带 `source_url` + `checked_date`；数据会变 → 定期重核。

## 文件
| 文件 | 内容 | 源 | 维护 |
|---|---|---|---|
| `vllm-metrics.json` | vLLM 官方 Prometheus 指标全量（Gauge/Counter/Histogram） | docs.vllm.com.cn/en/latest/usage/metrics + vllm-project/vllm PR #2316/#2730 | `fact_warehouse_collect.py --vllm-metrics` 定期重抓 |
| `vllm-flags.json` | vLLM 合法 CLI flag 白名单 | `vllm serve --help` + docs.vllm.cc CLI guide | 人工维护 + 官方 release notes |

## 用法
- **G4 闸门**：`validate_gate.py` 扫正文 `vllm:*` 指标名 → 不在 `vllm-metrics.json` 且不在 histogram 后缀白名单 → FAIL。
- **文章写作**：引用前先查白名单；新增 vLLM 指标/flag 先加进白名单再写进文章。
- **数据仓采集器**：`--vllm-metrics` 定期重抓官方源，同步刷新 checked_date。
