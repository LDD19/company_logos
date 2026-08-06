# company_logos

A repository of company/ticker logo SVGs. Intended as a static asset source that
other projects consume.

## Repository layout

| Path | Contents | Naming convention | Count |
|------|----------|-------------------|-------|
| `tv_logos/tv_logos/` | Logo SVGs sourced from TradingView, small size | `<logoid>.svg`, where `logoid` is TradingView's logo slug (e.g. `apple.svg`, `nvidia.svg`) | 59,589 |
| `tv_logos/tv_logos_big/` | The same logos at TradingView's large size | identical to the above — same slugs, same subfolders | 59,589 |
| `tv_logos/tickers.csv` | Manifest for US index universe (S&P500 + Nasdaq100 + Dow) | see [Manifest schema](#manifest-schema) | 518 rows |
| `tv_logos/tickers_world.csv` | Manifest for the worldwide universe | see [Manifest schema](#manifest-schema) | ~1.32M rows |
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
- `tv_logos_big/` is a **drop-in swap** for `tv_logos/`: the two trees hold the
  same 59,589 slugs under the same names, so a manifest's `svg_file` resolves in
  either one and picking a size is just choosing the folder. It is a separate
  asset, not an upscale — TradingView redraws it on a larger grid (`apple.svg`
  is `18×18`, its big form `56×56`, with more path detail). A handful of big
  files are *smaller in bytes* than their small counterparts despite this; that's
  just simpler path encoding, not a wrong file.
- Some slugs are nested (`crypto/XTVCBTC.svg`, `country/US.svg`,
  `source/NASDAQ.svg`) because TradingView namespaces non-equity logos. Both
  trees mirror that structure, so treat `logoid` as a path, not a bare filename.
- All logos are SVG. Filenames in the legacy folders may contain spaces/hyphens
  replaced by underscores.

## Manifest schema

`tickers.csv` and `tickers_world.csv` share these columns:

| Column | Meaning |
|--------|---------|
| `ticker` | Symbol. Bare (`NVDA`) in `tickers.csv`; exchange-qualified (`NASDAQ:NVDA`) in `tickers_world.csv` |
| `logoid` | TradingView logo slug; empty if the symbol has no logo |
| `svg_file` | `<logoid>.svg`, or empty if no logo. Resolves against **either** size folder |

To map a ticker → logo file: look up the row in the relevant manifest and open
`tv_logos/tv_logos/<svg_file>` for the small logo or
`tv_logos/tv_logos_big/<svg_file>` for the large one. An empty `svg_file` means
TradingView has no logo for that symbol (expected for some names, e.g. XOM, DD).

There is deliberately **no separate column for the big variant**. Coverage is
1:1 — every one of the 59,584 non-empty logoids in `tickers_world.csv` (and all
509 in `tickers.csv`) has a file in both trees — so a column would encode a
constant. Size is a folder choice made by the consumer, not a property of a row.

## Attribution

Legacy logo sets originate from https://www.fey.com/marketing/logos.
`tv_logos/` are sourced from TradingView's public logo CDN.
