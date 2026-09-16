# China Manufacturing Intelligence — Data Hub

Structured knowledge graph of China's manufacturing geography — industrial
clusters, cities, industries, products, ports, and verified manufacturers —
rendered as a MkDocs Material documentation site.

**Live site:** `https://data.chinamanufacturingintel.example` (placeholder domain)

This repo is the **data layer** of the China Manufacturing Intelligence project.
The editorial main site lives in a separate repo (`China-Manufacturing-Intelligence`).

## Repo layout

| Path | Purpose |
|---|---|
| `data/entities/` | Single source of truth — structured YAML entities (clusters, cities, industries, products, ports, manufacturers, industrial-parks) |
| `hub/gen_hub.py` | Generator: reads `data/entities/*.yaml` → emits `hub/docs/` MkDocs pages |
| `hub/build_manufacturers.py` | Extracts verified manufacturers from cluster YAML → `data/entities/manufacturers/` |
| `hub/build_ports.py` | Builds port entities from search results → `data/entities/ports/` |
| `hub/mkdocs.yml` | MkDocs Material config |
| `hub/docs/` | **Generated** (gitignored) — produced by `gen_hub.py` |
| `hub/site/` | **Build output** (gitignored) — produced by `mkdocs build` |

## Build

```bash
# regenerate hub/docs from data/entities, then build site
cd hub
python3 gen_hub.py
mkdocs build
# output: hub/site/
```

Cloudflare Pages build command:
`python3 gen_hub.py && mkdocs build` (output directory `hub/site`, requires `mkdocs-material` + `pyyaml`).

## Evidence discipline

Every fact rendered in the hub comes from `verified_facts` in the YAML entities.
No invented data. Manufacturer `entity_role` defaults to `manufacturer` only when
cluster research asserts a manufacturing role (fabless → brand, software → excluded).
