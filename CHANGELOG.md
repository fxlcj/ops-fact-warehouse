# Changelog

Each entry records what changed and the date the official source was re-checked.
A figure never changes without its `checked_date` changing with it.

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
