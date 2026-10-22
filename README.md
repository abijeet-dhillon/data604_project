# DATA604 Project

This project explores how movements in the Bank of Canada commodity price index (BCPI) relate to Canadian effective exchange rates (CEER). The planned analysis uses Python for data cleaning, exploration, visualization, and forecasting, with SQL for storing and querying the monthly data.

## Project status

The repository currently contains the source CSV exports and a set of proposed research questions. Python notebooks or scripts, SQL schema and queries, and a dependency file have not been added yet.

## Repository contents

```text
data604_project/
├── data/
│   ├── BCPI_MONTHLY-sd-1972-01-01.csv
│   └── CEER_BROADM,CEER_MCM,CEER_OITPM,CEER_BROAD_XUSM-sd-1990-01-01.csv
├── docs/
│   └── rqs.md
└── README.md
```

## Research questions

The proposed questions are documented in [`docs/rqs.md`](docs/rqs.md). They cover:

1. Months when the total BCPI is above its historical median and the CEER values in those months.
2. Whether CEER changes differ between months when the BCPI rises and falls.
3. Relationships between changes in total, energy, and non-energy BCPI measures and CEER measures.
4. Whether the BCPI–CEER relationship changed across pre-pandemic, pandemic, and post-pandemic periods.
5. Whether lagged BCPI movements improve predictions of next month's CEER change.

These questions are proposals; the analysis and findings have not yet been implemented in this repository.

## Data

Both files are monthly Bank of Canada CSV exports. They include metadata rows before the observation table, so skip those rows when loading the observations.

| File | Measures | Observations in this export |
| --- | --- | --- |
| `BCPI_MONTHLY-sd-1972-01-01.csv` | Total BCPI (`M.BCPI`), BCPI excluding energy (`M.BCNE`), energy (`M.ENER`), metals and minerals (`M.MTLS`), forestry (`M.FOPR`), agriculture (`M.AGRI`), and fish (`M.FISH`) | 1972-01 through 2026-09 (657 months) |
| `CEER_BROADM,CEER_MCM,CEER_OITPM,CEER_BROAD_XUSM-sd-1990-01-01.csv` | Real broad CEER (`CEER_BROADM`), real CEER excluding the US dollar (`CEER_BROAD_XUSM`), real CEER versus major currencies (`CEER_MCM`), and real CEER versus other important trading partners (`CEER_OITPM`) | 1999-01 through 2026-06 (330 months) |

The CEER observations in the supplied file begin in 1999, despite `1990` appearing in its filename. The supplied CEER series are real exchange-rate indices; a nominal CEER series is not included. Questions that specifically require nominal CEER values need an additional data source or revised wording. For joined analysis, align the datasets on `date` and use their overlapping period; the current overlap ends in 2026-06.

The CSV exports carry the Bank of Canada's terms link: <https://www.bankofcanada.ca/terms/>. Check the source and terms when redistributing the data.

### Loading the observation tables with pandas

The observation header starts on line 21 of the BCPI file and line 12 of the CEER file. For pandas, skip the metadata rows before the header:

```python
import pandas as pd

bcpi = pd.read_csv(
    "data/BCPI_MONTHLY-sd-1972-01-01.csv",
    skiprows=20,
    encoding="utf-8-sig",
    parse_dates=["date"],
)

ceer = pd.read_csv(
    "data/CEER_BROADM,CEER_MCM,CEER_OITPM,CEER_BROAD_XUSM-sd-1990-01-01.csv",
    skiprows=11,
    encoding="utf-8-sig",
    parse_dates=["date"],
)

monthly = bcpi.merge(ceer, on="date", how="inner")
```

## Python and SQL workflow

Python analyses can use pandas for loading, cleaning, joining, and feature creation, plus a plotting library for visualizations. A forecasting question may also use a machine-learning library. The repo does not yet pin dependencies or specify a database engine or SQL dialect; those choices should be recorded when the analysis code is added.

For SQL work, load the parsed observation tables into a database, retain `date` as a date type, and join the monthly tables on `date`. Keep the original source CSVs unchanged so the analysis can be traced back to the downloaded data.

## Suggested next steps

1. Add the analysis notebook or Python scripts and record their run instructions.
2. Choose and document the SQL database and add the schema and queries.
3. Add a dependency file such as `requirements.txt` or `pyproject.toml`.
4. Confirm the intended CEER measure for each research question and obtain nominal CEER data if needed.
