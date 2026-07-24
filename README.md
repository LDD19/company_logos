# company_logos

A repository of company/ticker logo SVGs. Intended as a static asset source that
other projects consume.

## Repository layout

| Path | Contents | Naming convention | Count |
|------|----------|-------------------|-------|
| `tv_logos/tv_logos/` | Logo SVGs sourced from TradingView | `<logoid>.svg`, where `logoid` is TradingView's logo slug (e.g. `apple.svg`, `nvidia.svg`) | ~47k |
| `tv_logos/tickers.csv` | Manifest for US index universe (S&P500 + Nasdaq100 + Dow) | see [Manifest schema](#manifest-schema) | ~518 rows |
| `tv_logos/tickers_world.csv` | Manifest for the worldwide universe | see [Manifest schema](#manifest-schema) | ~180k rows |
| `fey_logos/` | Legacy logo set (scraped from fey.com). One SVG per US listing | `<TICKER>_<MIC>.svg` (e.g. `AAPL_XNAS.svg`) | ~1369 |
| `ticker_logo/` | Legacy set, same images keyed by bare ticker | `<TICKER>.svg` (e.g. `AAPL.svg`) | ~1369 |
| `longname_logo/` | Legacy set keyed by full company name | `<Full_Company_Name>.svg` | ~1359 |
| `shortname_logo/` | Legacy set keyed by short company name | `<Short_Name>.svg` (truncated) | ~1359 |

Notes:
- The four legacy folders (`fey_logos`, `ticker_logo`, `longname_logo`,
  `shortname_logo`) are the **same underlying logos** re-keyed under four naming
  schemes.
- In `tv_logos/`, multiple tickers can share one logo file (e.g. `GOOGL` and
  `GOOG` both → `alphabet.svg`), so join through a manifest rather than assuming
  one file per ticker.
- All logos are SVG. Filenames in the legacy folders may contain spaces/hyphens
  replaced by underscores.

## Manifest schema

`tickers.csv` and `tickers_world.csv` share these columns:

| Column | Meaning |
|--------|---------|
| `ticker` | Symbol. Bare (`NVDA`) in `tickers.csv`; exchange-qualified (`NASDAQ:NVDA`) in `tickers_world.csv` |
| `logoid` | TradingView logo slug; empty if the symbol has no logo |
| `svg_file` | `<logoid>.svg` (the file in `tv_logos/tv_logos/`), or empty if no logo |

To map a ticker → logo file: look up the row in the relevant manifest and open
`tv_logos/tv_logos/<svg_file>`. An empty `svg_file` means TradingView has no logo
for that symbol (expected for some names, e.g. XOM, DD).

## Attribution

Legacy logo sets originate from https://www.fey.com/marketing/logos.
`tv_logos/` are sourced from TradingView's public logo CDN.
