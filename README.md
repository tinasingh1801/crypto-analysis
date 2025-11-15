# crypto-analysis

This repository contains data and results for an exploratory analysis of cryptocurrency markets.

## Project Overview

The goal of this project is to collect, clean, and analyze cryptocurrency market data to produce insights and visualizations that help understand price movements, volume, and other market dynamics.

## Repository Structure

- `Raw data/` : Original data files and source links (see `link.txt`).
- `cleaned Data/` : Processed and cleaned datasets ready for analysis (CSV or parquet files).
- `Result/` : Analysis outputs such as plots, tables, and exported reports.

## Data

- Source links and notes are stored in `Raw data/link.txt`.
- Cleaned data should live in `cleaned Data/` and be documented with the filename and schema where possible.

### Data Source — CoinGecko API

All data for this project is sourced from the CoinGecko public API. Typical endpoints used:

- `GET /coins/markets` — current market data (price, market cap, volume) for multiple coins.
- `GET /coins/{id}/market_chart` — historical price/volume for a single coin (by days).
- `GET /coins/{id}/market_chart/range` — historical data between two UNIX timestamps.

Notes and best practices:

- CoinGecko rate limits are generous for non-commercial use but respect their terms; batch requests and cache results.
- Prefer `vs_currency=usd` (or your chosen fiat) for normalized time series.
- Store raw JSON responses in `Raw data/` (with a short note in `link.txt`) before cleaning.

Example Python snippet to fetch market data (replace `requests` usage with your preferred HTTP client):

```
import requests
import csv

url = 'https://api.coingecko.com/api/v3/coins/markets'
params = {'vs_currency':'usd','order':'market_cap_desc','per_page':250,'page':1,'sparkline':'false'}
resp = requests.get(url, params=params)
data = resp.json()

# Write a simple CSV to cleaned Data/
with open('cleaned Data/markets_page1.csv','w',newline='',encoding='utf-8') as f:
	writer = csv.writer(f)
	writer.writerow(['id','symbol','name','current_price','market_cap','total_volume','last_updated'])
	for d in data:
		writer.writerow([d['id'], d['symbol'], d['name'], d['current_price'], d['market_cap'], d['total_volume'], d['last_updated']])
```

Save these CSVs into `cleaned Data/` and keep a small manifest describing the file, timestamp, and endpoint used.

## Quickstart (recommended)

1. Install Python 3.10+ and create a virtual environment (PowerShell):

```
python -m venv .venv
.venv\Scripts\Activate.ps1
```

2. Install commonly used analysis libraries (or create a `requirements.txt`):

```
pip install pandas numpy matplotlib seaborn jupyterlab requests
```

3. Open a JupyterLab session to explore data interactively:

```
jupyter lab
```

4. Start by inspecting the cleaned datasets in `cleaned Data/` and run exploratory notebooks or scripts. Save resulting figures and reports to `Result/`.

## Power BI — Editor & Analysis (step-by-step)

This project uses Power BI Desktop for the analysis and visualizations. Typical workflow:

1. Prepare CSVs in `cleaned Data/` (one row per observation, ISO timestamps where possible).
2. In Power BI Desktop: `Get Data` → `Text/CSV` or `Web` (if serving files via URL).
3. Use Power Query Editor to:
	- parse timestamps (set timezone if needed),
	- convert numeric columns (price, volume, market cap) to correct types,
	- remove or flag null/incomplete rows,
	- aggregate or pivot data if building cross-coin summaries.
4. Build measures in DAX for rolling metrics (examples):
	- 7-day and 30-day moving averages,
	- daily returns and volatility (stddev of returns),
	- market-cap weighted aggregates.
5. Save `.pbix` files (Power BI files) in `Result/` or a separate `powerbi/` folder and export visuals or PDF reports into `Result/`.

Refresh notes:

- For scheduled refreshes, publish to Power BI Service and configure a gateway if data is on-prem or in a local CSV folder.
- Alternatively, script data pulls to a cloud storage location that Power BI can connect to (Azure Blob, S3 with presigned URLs, etc.).

## Analyses Completed (summary)

- Price history and trend charts for selected coins.
- Volume and liquidity analysis.
- Moving averages and crossover signals.
- Volatility estimates and return distributions.
- Correlation matrices between coin returns.
- Market-cap rankings and share-of-market over time.

If you want these documented in the repository, I can add a short report or notebook summarizing the specific visuals and pages in the `.pbix` file.

## Suggested Files to Add

- `requirements.txt` — pinned Python dependencies for reproducibility.
- `notebooks/` — Jupyter notebooks for exploratory analysis and visualization.
- `scripts/` — data processing scripts (ingest, clean, transform).
- `docs/` — documentation or exported reports.

## Reproducibility Checklist

- Store raw JSON responses in `Raw data/` with a timestamped filename and note in `Raw data/link.txt` the endpoint and parameters used.
- Keep cleaned CSVs in `cleaned Data/` with a simple manifest (`cleaned Data/manifest.csv`) containing: filename, source_endpoint, fetched_at, rows, columns.
- Save analysis workbooks (Power BI `.pbix`) and exported reports to `Result/`.

## Next Steps

- Add a small `scripts/fetch_coingecko.py` script to automate data pulls and write to `Raw data/` + `cleaned Data/`.
- Create `notebooks/` with exploratory analysis and a summary report exported to `Result/`.
- Add `requirements.txt` and a short `CONTRIBUTING.md` describing how to update data and refresh Power BI.

---

If you'd like, I can now:

- create `scripts/fetch_coingecko.py` to fetch and save market data,
- scaffold a `notebooks/` starter notebook that loads cleaned CSVs and computes basic metrics,
- or add `requirements.txt` and `cleaned Data/manifest.csv` now.

Tell me which action to take next and I'll implement it.

## Next Steps

- Add a `requirements.txt` so contributors can reproduce the environment easily.
- Add at least one example notebook in `notebooks/` demonstrating a simple analysis pipeline (load cleaned data, plot price history, compute basic statistics).
- Add README sections documenting any non-obvious preprocessing steps applied to the data.

## License & Contact

If this repository is public, add a license file (e.g., `LICENSE`) and a contact or maintainer note.

---

If you'd like, I can also:

- create a `requirements.txt` with the packages above,
- scaffold a `notebooks/` folder with a starter notebook,
- or add a small data ingestion script to demonstrate the pipeline.

Tell me which of those you'd like next.
