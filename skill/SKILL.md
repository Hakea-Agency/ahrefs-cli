---
name: ahrefs
description: >
  Ahrefs SEO analytics CLI — domain rating, backlinks, organic keywords,
  keyword research, SERP overview, competitor analysis, and batch domain
  analysis. Use for SEO audits, competitor research, and link building data.
---

# Ahrefs SEO Analytics CLI

Ahrefs data via REST API v3. Every request costs at least 50 API units.
Use `--select` to limit fields and reduce costs.

## CLI

```bash
ahrefs <command> <target|keyword> [options]
```

## Commands

### Site Explorer

```bash
ahrefs dr example.com
ahrefs backlinks example.com
ahrefs backlinks-list example.com --limit 20 --where "domain_rating_source>30"
ahrefs refdomains example.com --limit 20 --order-by "-domain_rating"
ahrefs anchors example.com
ahrefs outlinks example.com
ahrefs metrics example.com
ahrefs organic example.com --country fi --limit 20
ahrefs top-pages example.com --country fi
ahrefs competitors example.com --country fi
ahrefs dr-history example.com --date-from 2024-01-01
ahrefs metrics-history example.com --date-from 2024-01-01
ahrefs pages-by-traffic example.com --country fi
```

### Keywords Explorer

```bash
ahrefs keyword "seo" "hakukoneoptimointi" --country fi
ahrefs volume-history "seo" --country fi
ahrefs matching "seo" --country fi --limit 20
ahrefs related "seo" --country fi --limit 20
ahrefs suggestions "seo" --country fi
```

### SERP & Batch

```bash
ahrefs serp "keyword" --country fi
ahrefs batch example.com competitor.com
```

### Usage

```bash
ahrefs usage
```

## Global Options

- `--country XX` — Country code (default: us)
- `--date YYYY-MM-DD` — Snapshot date (default: today)
- `--date-from YYYY-MM-DD` — Start date for history (default: 1 year ago)
- `--limit N` — Max results (default: 10)
- `--mode` — domain | subdomains | prefix | exact (default: subdomains)
- `--select FIELDS` — Comma-separated fields (overrides defaults, reduces cost)
- `--where FILTER` — Filter expression (e.g. "domain_rating>30")
- `--order-by FIELD` — Sort field (prefix `-` for desc)
- `--format` — table | csv | json (default: table)
- `--json` — Shortcut for --format json
- `--protocol` — both | http | https (default: both)

## Authentication

Set `AHREFS_API_KEY` env var, or configure in `~/.config/ahrefs/config.json`:

```json
{
  "api_key_command": "your-command-to-get-key"
}
```

## Typical SEO Audit Flow

```bash
# 1. Domain overview
ahrefs dr example.com
ahrefs metrics example.com

# 2. Backlink profile
ahrefs backlinks example.com
ahrefs refdomains example.com --limit 20 --order-by "-domain_rating"

# 3. Organic performance
ahrefs organic example.com --country fi --limit 30
ahrefs top-pages example.com --country fi

# 4. Competition
ahrefs competitors example.com --country fi

# 5. Keyword research
ahrefs keyword "target keyword" --country fi
ahrefs serp "target keyword" --country fi
```

## Important

- **Minimum 50 units per request** — use `--select` to reduce costs
- **`--where` filters** reduce noise without extra cost
- **Country codes** are two-letter ISO (fi, se, us, de, etc.)
- **Real cost** is parsed from API response headers and tracked in `~/.config/ahrefs/usage.json`
- **CSV export:** `ahrefs organic example.com --format csv > keywords.csv`
