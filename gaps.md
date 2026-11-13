# Known gaps and deliberate exclusions

This file is **not a dataset**. Nothing here is a verified fact and nothing here may be quoted as one. It records the figures we wanted to include but could not verify against an official source, together with entries we refused because the evidence did not hold. It exists so that a later maintainer does not quietly fill these holes with a guess.

| Missing or excluded | Why it is not in the data files | Evidence |
|---|---|---|
| An authoritative IANA "registered / unregistered" determination for each port in `common_ops_ports.json` | The full IANA service-names-and-port-numbers registry (3.8 MB / 40,000+ records) could not be fetched completely on the build network (repeated `IncompleteRead` / timeouts) and returned misaligned service names. Any "unregistered" claim based on an incomplete fetch would be a false negative. | Repo scope, 2026-11-13. `common_ops_ports.json` is therefore a **reference aid only** — verify each port against the official IANA page or the vendor's docs before citing it. |
| A per-port IANA assignment date / allocation status | Same root cause: only a complete registry fetch would make this reliable, and we do not publish figures we could not fully verify. | Repo scope, 2026-11-13. |
| A live (always-current) view of CISA KEV | CISA KEV is a live feed that changes weekly. This repo publishes a **point-in-time snapshot** (`cisa_kev.json` with `catalog_version` / `checked_date`), not a live mirror. Cite it with its snapshot date; for the current list, use the official CISA KEV feed. | Repo scope, 2026-11-13. |
| CVE root-cause / vendor patch details | Out of scope for this dataset: KEV entries here state only that a vulnerability is known to be exploited and the CISA-required action / due date. Root cause and patches belong to the vendor's official advisory. | Repo scope, 2026-11-13. |

If an official page becomes available (e.g. a full registry fetch that completes cleanly), add the entry through the maintenance task with its official source URL and check date, record the change in `CHANGELOG.md`, and remove the row above. The official page — not this file and not a third-party mirror — is always the authority.
