# Changelog

Each entry records what changed and the date the official source was re-checked.
A figure never changes without its `checked_date` changing with it.

## 2026-11-23 — Prometheus/Nginx/HA whitelist expanded; Prometheus + Nginx upgraded to G4b FAIL

- `prometheus-metrics/metrics.json`: 289 → 301 metric names (node_exporter 167 / kube-state-metrics 56 / prometheus self 78; +26 PromQL functions) — added real node_exporter PSI/pressure, `node_netstat`, `node_processes`; real Prometheus self-metrics `prometheus_rule_group_last_evaluation_timestamp_seconds`, `prometheus_scrape_duration_seconds{_bucket}`, `prometheus_tsdb_compaction_duration_seconds{_count}`, plus `prometheus_operator`/`prometheus_storage` component refs and 7 `prometheus_tsdb_*` derived metrics.
- `nginx-directives/directives.json`: 132 → 134 (added `internal`, `down` — real nginx `location`/upstream-peer directives; Caddy directives such as `header`/`encode`/`reverse_proxy` are excluded as non-nginx in the gate).
- `ha-config/ha-config.json`: integrations 124 → 164 (added 40 real core HA integrations: telegram, smtp, sun, time, state, template, numeric_state, rest, push_alerts, email_admin, media_player, camera, weather, mqtt, google_cast, alexa, google_assistant, demo, ...).
- Gate (G4b `check_tech_whitelist`): `prometheus-metrics` and `nginx-directives` upgraded from WARN to **FAIL** (block deploy) — a fabricated metric/directive now blocks; real refs pass. Negative-tested both directions.

## 2026-11-22 — Technical-reference whitelist repos added (vLLM / HA CLI / sysctl / HTTP / Prometheus / Nginx / Kubernetes / iptables / Docker)

New machine-readable reference repos under the subdirectories below. These are **static reference whitelists** distilled from each project's official docs, each with a `checked_date` in `_meta`. They exist so articles can be checked against the official fixed-data surface (commands, flags, metric names, directives, table/chain/target) — a fabricated identifier not on the list is a red flag worth reviewing.

- `vllm-metrics/vllm-metrics.json`: 43 vLLM Prometheus metric names (with `vllm:` prefix, histogram suffixes, deprecation notes) + `vllm-metrics/vllm-flags.json`: 37 vLLM CLI flags.
- `ha-cli/ha-cli.json`: 21 top-level `ha` subcommands + 67 secondary actions + 8 global flags (Home Assistant CLI).
- `linux-sysctl/sysctl.json`: 100+ sysctl parameters across net/vm/fs/kernel.
- `http-status/codes.json`: full HTTP status-code set (RFC 9110 + IANA extensions).
- `prometheus-metrics/metrics.json`: node_exporter / kube-state-metrics / Prometheus-self metric names + PromQL functions.
- `nginx-directives/directives.json`: 80+ common Nginx directives.
- `kubernetes/kubernetes.json`: 37 kubectl subcommands + ~60 resource kinds/abbreviations (incl. Prometheus-Operator CRDs).
- `firewall/firewall.json`: iptables tables / chains / targets (incl. Kubernetes `KUBE` target) + nftables tables.
- `docker/docker.json`: 28 docker subcommands + 50+ common docker / docker-compose flags.
- `network-tools/tools.json`: curl / tcpdump / nmap / ss / ip / dig / netstat / traceroute / ping / mtr common flags (incl. global `ip` flags and combined short-flag notes).
- `log-format/log-format.json`: Nginx log variables / `journalctl` / `grep` / `awk` / `sed` command flags + RFC 5424 syslog fields + Nginx error-log levels.
- `systemd/systemd.json`: 100+ systemd unit/socket directives + 45 `systemctl` subcommands + `journalctl` flags.
- `ci-pipelines/ci-pipelines.json`: Jenkins pipeline blocks + GitLab CI keywords + GitHub Actions keywords.
- `cve-format/cve-format.json`: CVE identifier format/regex rules + top CWE categories + CVSS v3 severity + EPSS note (for cross-checking CVE mentions in articles).
- `prometheus-functions/promql.json`: 60+ built-in PromQL functions + 9 common alerting expression patterns (extension repo, not wired into the build gate yet).
- `http-headers/headers.json`: common HTTP request/response/security/CORS/auth/encoding-negotiation headers (~100 header names, incl. deprecated `X-XSS-Protection` / `Public-Key-Pins`).
- `virtualization/virtualization.json`: KVM/libvirt/Proxmox VE — ~76 `virsh` subcommands + 11 resource types + ~39 Proxmox `qm`/`lxc` subcommands + qemu common tools.
- `database/database.json`: MySQL / PostgreSQL / Redis — admin commands (~40) + psql meta-commands + SQL keywords + ~82 Redis commands.
- `cloud-cli/cloud-cli.json`: AWS / GCP / Azure CLI — ~61 AWS service subcommands + ~26 common AWS actions + ~30 gcloud command groups + ~34 `az` groups + flags.
- `sysstat/sysstat.json`: iostat / vmstat / mpstat / sar — ~17 iostat flags + 11 fields, ~16 vmstat flags + 15 fields, ~11 mpstat flags, ~18 sar flags.
- `openssl/openssl.json`: OpenSSL subcommands + flags — ~49 subcommands (s_client/x509/req/pkcs12/cert…) + ~41 common flags + 3 deprecated notes.
- `helm/helm.json`: Helm 3.x subcommands + flags — ~25 subcommands (install/upgrade/uninstall/rollback/repo…) + ~25 common flags.
- `debug-tools/debug-tools.json`: lsof / strace / perf / fuser — ~28 lsof flags, ~19 strace flags, ~22 perf subcommands, ~10 fuser flags.
- `ha-config/ha-config.json`: Home Assistant **configuration** dimension (complements `ha-cli/`, which covers the CLI dimension) — ~85 core YAML config keys (automation trigger/condition/action/service etc.), ~108 service actions (`light.turn_on`/`climate.set_temperature`/`notify.mobile_app`… domain.action form), ~50 entity domains, ~124 integration names, ~123 official/HACS addon names (453 entries total).
- `ha-api/ha-api.json`: Home Assistant **REST API + Lovelace UI** dimension (complements `ha-cli/` and `ha-config/`) — ~44 HA core REST API endpoints (`/api/states`, `/api/services`, `/api/supervisor/*`, `/api/config/*`, `/api/webhooks/*`) + ~34 Lovelace card types (entities/glance/tile/history-graph/…) + ~6 view types + ~2 storage modes (87 entries total). Third-party API paths (Prometheus `/api/v1/*`, MQTT/zigbee2mqtt device APIs) are intentionally **excluded** to avoid false-positives in monitoring articles.
- `linux-sysctl/sysctl.json`: expanded to **134 params** (net/vm/fs/kernel) with interface-agnostic matching (e.g. `net.ipv4.conf.<iface>.rp_filter` resolves against the `.all.`/`.default.` variant).
- `http-status/codes.json`: **77 codes** (full RFC 9110 + IANA extensions + Nginx `499` + Cloudflare `520`–`530` gateway codes).
- `prometheus-metrics/metrics.json`: **310 entries** (~164 node_exporter incl. derived/alias metrics + ~56 kube-state-metrics + ~64 Prometheus-self + ~26 PromQL functions).
- `nginx-directives/directives.json`: **132 directives** (full HTTP/server/location/events/stream/upstream context).

Maintenance: these are reference data re-checked manually (not a live feed); `checked_date` refreshes each re-check. The CISA KEV and IANA files above remain the live/re-collected sources.

## 2026-11-19 — CISA KEV snapshot refresh (1779 → 1782)

Weekly re-collection. `cisa_kev.json` is a **point-in-time snapshot of a live feed**,
so its count moves with each official release — always cite it with its `checked_date`.
- `cisa_kev.json`: **1782** entries (was 1779). Catalog version `2026.11.17`, date released 2026-11-17. Three entries added this cycle, none removed, no due-date changes.
- `cisa_kev_ops_relevant.json`: **363**-entry ops-vendor subset (Ivanti / SonicWall / Fortinet / Palo Alto / Cisco / VMware / MikroTik / Ubiquiti, etc.), unchanged.
- Other files re-collected against their official sources this run; counts stable — 28 RFC / 48 common ports (ref) / 18+18 special IP ranges / 96 protocol numbers / 1436 TLDs.

## 2026-11-13 — 2026.11 (initial public dataset)

Initial public dataset for the ops.jiunotes.top fact warehouse: 7 files covering
network / monitor / homeassistant / ai topics —

- `rfc_facts.json`: 28 RFCs (title / date / abstract) from rfc-editor.org `citation_*` meta, every entry traceable to the official page.
- `common_ops_ports.json`: 48 common ops ports (23 std / 25 vendor) as a **reference aid only** — see `gaps.md` for why this is *not* an authoritative IANA registered/unregistered determination.
- `iana_ip_special.json`: 18 IPv4 + 18 IPv6 special-purpose address ranges from iana.org, each with its defining RFC and the five source/destination/forwardable/global/reserved flags.
- `iana_protocol_numbers.json`: 96 protocol numbers with spec RFC.
- `iana_tlds.json`: 1436 TLDs (gTLD + ccTLD).
- `cisa_kev.json`: a point-in-time snapshot of the CISA Known Exploited Vulnerabilities feed (1779, catalog 2026.11.10); `cisa_kev_ops_relevant.json` is the 363-entry subset for high-frequency security vendors (Ivanti / SonicWall / Fortinet / Palo Alto / Cisco / VMware / MikroTik / Ubiquiti, etc.).

Quality rules baked in from the start (see `gaps.md`):
- Incomplete network sources are trusted for **positives only**, never written as **negatives** — "not seen in a truncated fetch" is not "not in the registry".
- The CISA KEV file is a **snapshot of a live feed**: cite it with its `checked_date`.

Maintenance: weekly re-collection + monthly per-entry `source_url` re-check. When an official source changes, the field is updated here; when a source 404s, the entry moves to `gaps.md` and is removed from the JSON.
