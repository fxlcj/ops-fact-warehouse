# ops.jiunotes.top 事实数据仓（Fact Warehouse）

机器可读的权威官方源事实沉淀，供 [ops.jiunotes.top](https://ops.jiunotes.top)（运维知识站：network / monitor / homeassistant / ai 四栏目）的文章引用、互相补充。每条事实都带官方源 URL 与核验日期（`checked_date`）。

**口径（诚实，不夸大）：** 本仓只收**权威官方源**且**对得上官方源**的事实；对不上就不收（宁缺毋滥）。数据会变 → 定期重核；官方源内容变了就改字段，源失效就移入 `gaps.md`。这里**不写「已核验 / verified」这类措辞**——官方源页面本身才是最终权威。

## 收录范围（7 个仓）

| 文件 | 主题 | 官方源 | 条数（2026-11-13） |
|---|---|---|---|
| `rfc_facts.json` | RFC 标准（title / date / abstract） | rfc-editor.org | 28 |
| `common_ops_ports.json` | 常用运维端口（**参考辅助**，见下） | 业界通用叫法 | 48（std 23 / vendor 25）|
| `iana_ip_special.json` | 特殊用途 IPv4 / IPv6 地址段 | iana.org | ipv4 18 + ipv6 18 |
| `iana_protocol_numbers.json` | IANA 协议号（含 spec RFC） | iana.org | 96 |
| `iana_tlds.json` | 顶级域名注册表（gTLD + ccTLD） | data.iana.org/TLD | 1436 |
| `cisa_kev.json` | CISA KEV 在野利用漏洞（**快照**，见下） | cisa.gov | 1779（catalog 2026.11.10）|
| `cisa_kev_ops_relevant.json` | KEV 中 ops 高频安全厂商子集 | cisa.gov | 363 |

## 各仓字段

### rfc_facts.json
RFC 编号为 key，取自 rfc-editor.org 官方 `citation_title` / `citation_publication_date` / `description` meta，逐条可回溯官方页。`status` 若官方页未给出则留 `null`（不臆造）。

### common_ops_ports.json（⚠️ 参考辅助，非 IANA 权威断言）
`端口 → 常见用途 + std/vendor 分类`。这是**参考**，**不是 IANA 注册表的「已注册 / 未注册」权威判定**：
- `std` = 业界公认标准端口，引用时可对 iana.org 官方页核实后引 IANA / 协议标准；
- `vendor` = 厂商 / 项目约定（如 9090 Prometheus、3000 Grafana、6379 Redis、8472 WireGuard、11434 Ollama），多为厂商约定而非 IANA 标准服务名，引用时引厂商官方文档。

**为什么不做 IANA 权威判定**：IANA 全量端口注册表（3.8MB / 4 万+ 条）在本机网络反复抓取不完整（`IncompleteRead` / 超时）且服务名错位，任何基于不完整抓取的「未注册」判定都不可信。故本仓只做「端口 → 常见用途」参考。**写文章引用任一端口前，请逐端口对 iana.org 官方页 / 厂商官方源核实**，别直接拿本仓当权威源。

### iana_ip_special.json
每段带定义它的 `spec`（RFC）+ `source` / `destination` / `forwardable` / `global` / `reserved` 五维标志，取自 IANA 官方 XML，逐段可回溯。覆盖私有 / 保留 / CGNAT / 环回 / 链路本地等段（如 `10.0.0.0/8` → rfc1918、`100.64.0.0/10` CGNAT → rfc6598、`240.0.0.0/4` 保留）。

### iana_protocol_numbers.json
协议号 + 名称 + 描述 + spec RFC（如 6 → TCP → rfc0793、17 → UDP、41 → IPv6、50 → ESP、51 → AH、89 → OSPF、132 → SCTP）。

### iana_tlds.json
全量 gTLD + ccTLD，一行一个 TLD。

### cisa_kev.json / cisa_kev_ops_relevant.json（⚠️ 点态快照，非追加事实）
CISA KEV 是**活体 feed**（每周更新）。本仓是某一刻的**快照**：
- `cisa_kev.json` = 全量快照，`catalog_version` / `date_released` / `count` / `checked_date` 标明快照时点；
- `cisa_kev_ops_relevant.json` = 按 ops 高频安全厂商（Ivanti / SonicWall / Fortinet / Palo Alto / Cisco / VMware / MikroTik / Ubiquiti 等）筛出的可直接引用子集。

**引用口径**：某漏洞「是否被在野利用、CISA 要求的处置期限（`requiredAction` / `dueDate`）」以 CISA KEV 为准；CVE 根因 / 补丁引厂商官方公告。**引用时必须注明快照日期**（`checked_date`），因为 feed 每周会变。

## 使用与更新

- **只读使用**：直接把某条事实的 `url` 作为权威链接、`abstract` / 字段值作为一句话定义引用，注明「源自 rfc-editor.org / iana.org / cisa.gov」。
- **随发文增量扩充**：文章新引用了 RFC / 端口 / 厂商，加进采集脚本的 `RFC_IDS` / `IANA_PORTS` / `KEV_OPS_VENDORS` 常量即可增量扩充。
- **定期重核**：周度重核 + 月度逐条核对 `source_url`；官方源内容变了就改字段并记入 `CHANGELOG.md`，源失效就移入 `gaps.md` 并从 JSON 删除。
- **完整性铁律**：对网络抓不全的源，**只信 positive（确实找到了），绝不写 negative（没有）**——「在截断数据里没看到」≠「注册表里没有」。

## 许可

本仓库数据在 **CC BY 4.0** 下提供（见 [LICENSE](LICENSE)）。各条事实的底层官方源（rfc-editor.org / iana.org / cisa.gov）受其各自条款约束，引用时请注明出处。
