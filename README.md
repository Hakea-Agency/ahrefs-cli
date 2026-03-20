# Ahrefs CLI

SEO analytics from the command line. Domain analysis, keyword research, backlink data, and SERP overview — powered by the [Ahrefs API v3](https://docs.ahrefs.com/api).

Python 3.10+, zero dependencies (stdlib only).

## Features

- **Site Explorer** — Domain Rating, backlinks, referring domains, organic keywords, top pages, competitors
- **Keywords Explorer** — keyword metrics, volume history, matching/related terms, suggestions
- **SERP Overview** — top 100 results for any keyword
- **Batch Analysis** — analyze multiple domains at once
- **Usage Tracking** — real API unit costs from response headers, tracked per month
- **Flexible Output** — table, CSV, or JSON
- **Filtering & Sorting** — `--where` and `--order-by` for precise queries
- **Field Selection** — `--select` to choose fields and reduce API costs

## Installation

```bash
git clone https://github.com/Hakea-Agency/ahrefs-cli.git
cd ahrefs-cli

chmod +x ahrefs
sudo ln -s "$(pwd)/ahrefs" /usr/local/bin/ahrefs
```

## Authentication

Set your Ahrefs API key as an environment variable:

```bash
export AHREFS_API_KEY="your-api-key"
```

Get your API key from [Ahrefs Account Settings → API keys](https://app.ahrefs.com/account/api-keys).

Or configure a command in `~/.config/ahrefs/config.json`:

```json
{
  "api_key_command": "your-secret-manager-command-here"
}
```

## Usage

### Site Explorer

```bash
# Domain Rating
ahrefs dr example.com

# Backlink statistics
ahrefs backlinks example.com

# List individual backlinks (with filtering)
ahrefs backlinks-list example.com --limit 20 --where "domain_rating_source>30"

# Referring domains sorted by DR
ahrefs refdomains example.com --limit 20 --order-by "-domain_rating"

# Anchor texts
ahrefs anchors example.com

# All site metrics
ahrefs metrics example.com

# Organic keywords
ahrefs organic example.com --country fi --limit 20

# Top pages by traffic
ahrefs top-pages example.com --country fi

# Organic competitors
ahrefs competitors example.com --country fi

# DR history over time
ahrefs dr-history example.com --date-from 2024-01-01

# Metrics history
ahrefs metrics-history example.com --date-from 2024-01-01

# Pages ranked by organic traffic
ahrefs pages-by-traffic example.com --country fi
```

### Keywords Explorer

```bash
# Keyword overview (one or more keywords)
ahrefs keyword "seo" "hakukoneoptimointi" --country fi

# Search volume history
ahrefs volume-history "seo" --country fi

# Matching terms (contains keyword)
ahrefs matching "seo" --country fi --limit 20

# Related terms
ahrefs related "seo" --country fi --limit 20

# Search suggestions
ahrefs suggestions "seo" --country fi
```

### SERP & Batch

```bash
# SERP overview — top 100 results
ahrefs serp "hakukoneoptimointi" --country fi

# Batch analysis — multiple domains
ahrefs batch example.com competitor.com another.com
```

### Usage Tracking

```bash
# Show API usage this month
ahrefs usage
```

### Global Options

| Option | Description | Default |
|---|---|---|
| `--country XX` | Country code | `us` |
| `--date YYYY-MM-DD` | Snapshot date | today |
| `--date-from YYYY-MM-DD` | Start date (history commands) | 1 year ago |
| `--limit N` | Max results | `10` |
| `--mode MODE` | `domain\|subdomains\|prefix\|exact` | `subdomains` |
| `--select FIELDS` | Comma-separated fields (overrides defaults) | per-command |
| `--where FILTER` | Filter expression | none |
| `--order-by FIELD` | Sort field (prefix `-` for desc) | none |
| `--format FMT` | `table\|csv\|json` | `table` |
| `--json` | Shortcut for `--format json` | off |
| `--protocol` | `both\|http\|https` | `both` |

### Controlling API Costs

Every request costs at least 50 units. Costs scale with rows returned and fields requested.

```bash
# Fewer fields = lower cost
ahrefs organic example.com --select "keyword,volume,position" --limit 5

# Export to CSV for further analysis
ahrefs organic example.com --country fi --limit 100 --format csv > keywords.csv

# Filter to reduce noise
ahrefs backlinks-list example.com --where "domain_rating_source>20" --limit 50
```

Check your plan's monthly limit in [Ahrefs dashboard](https://app.ahrefs.com/account/limits-and-usage/web).

## OpenClaw Skill

The `skill/` directory contains an [OpenClaw](https://openclaw.ai) agent skill for integrating the CLI into AI agent workflows.

## License

MIT
