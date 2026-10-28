# Data Documentation

This project uses two monthly Bank of Canada datasets:

1. **Bank of Canada Commodity Price Index (BCPI)**, which measures changes
   in the prices of commodities produced in Canada and sold in world markets.
2. **Canadian Effective Exchange Rates (CEER)**, which measure changes in the
   Canadian dollar relative to weighted baskets of trading-partner currencies.

The CEER data used in this project are **real** exchange-rate indexes. They
account for both exchange-rate movements and relative price changes between
Canada and its trading partners. They are not nominal exchange rates or direct
currency quotations.

## Data sources and files

The source data were downloaded as CSV files from the Bank of Canada's
official data pages:

- [Bank of Canada Commodity Price Index](https://www.bankofcanada.ca/rates/price-indexes/bcpi/)
- [Bank of Canada Canadian Effective Exchange Rates](https://www.bankofcanada.ca/rates/exchange/canadian-effective-exchange-rates/)

The raw files used in the notebook are:

```text
data/raw/BCPI_MONTHLY-sd-1972-01-01.csv
data/raw/CEER_BROADM,CEER_MCM,CEER_OITPM,CEER_BROAD_XUSM-sd-1990-01-01.csv
```

The BCPI file contains monthly observations beginning in January 1972. The
CEER filename includes `1990`, but the observations in the supplied file
begin in January 1999.

## BCPI data

The BCPI dataset has one row for each month and seven index-value columns.
Each value summarizes the price movement of Canadian commodities in that
category. The values are not prices in dollars. They are index values, so a
value in one month is meaningful when compared with the value in another
month.

| Column | Description |
|---|---|
| `date` | The month of the observation. In the source data it is represented by the first day of the month, such as `1972-01-01`. In the filtered and combined data it is formatted as `YYYY-MM`, such as `2015-01`. |
| `M.BCPI` | The total BCPI. This is the overall measure of price movements across the commodity groups included in the BCPI. It combines the groups below using Bank of Canada weights. |
| `M.BCNE` | The BCPI excluding energy. It summarizes the price movements of the non-energy commodity groups, allowing the non-energy part of the commodity index to be viewed separately from energy. |
| `M.ENER` | The energy component of the BCPI. It displays price movements for the energy commodities included in the index. |
| `M.MTLS` | The metals and minerals component. It displays price movements for the metals and minerals included in the index. |
| `M.FOPR` | The forestry products component. It displays price movements for forestry products included in the index, such as lumber, pulp, and paper-related products. |
| `M.AGRI` | The agricultural products component. It displays price movements for agricultural commodities included in the index. |
| `M.FISH` | The fish products component. It displays price movements for fish and seafood products included in the index. |

The `M.` prefix identifies the columns as monthly series. A higher index
value means that the relevant weighted price measure is higher relative to
the index's reference period; it does not mean that every commodity in that
group increased by the same amount. `M.BCPI` is the overall measure, while
the other six columns show its component groups or, in the case of `M.BCNE`,
the non-energy total.

### How the BCPI numbers are formed

The BCPI is built in two stages:

1. The Bank of Canada collects price information for the commodities in each
   commodity group and summarizes the monthly price movement for that group.
2. The group indexes are combined into the total `M.BCPI` using weights. A
   group with a larger weight has a larger effect on the total index than a
   group with a smaller weight.

The weights are intended to reflect the importance of the commodity groups in
Canada's commodity production and trade. Therefore, `M.BCPI` is not the
simple average of `M.ENER`, `M.MTLS`, `M.FOPR`, `M.AGRI`, and `M.FISH`.
`M.BCNE` is calculated from the non-energy commodity groups rather than
including the energy component.

For example, if an index moves from 500 to 550, its value has increased by
10 percent:

```text
(550 - 500) / 500 × 100 = 10%
```

This means the weighted price measure represented by that index increased by
10 percent between the two months. It does not mean that every commodity
increased by 10 percent, and the number 550 is not a price of $550.

An index value such as `300` is not automatically good or bad. Its meaning
depends on the series, the comparison month, and the perspective of the
reader. Higher commodity prices may benefit producers and exporters while
also increasing costs for consumers and businesses. The value should
therefore be used to describe change over time, not as a score.

## CEER data

The CEER dataset has one row for each month and four real exchange-rate index
columns. Each column summarizes the Canadian dollar's movement against a
different weighted group of trading-partner currencies. These are real
indexes, meaning that the exchange-rate movement is adjusted for relative
price changes between Canada and the countries in the relevant group. They
are not dollar exchange-rate quotations such as Canadian dollars per U.S.
dollar.

| Column | Description |
|---|---|
| `date` | The month of the observation. In the source data it is represented by the first day of the month, such as `1999-01-01`. In the filtered and combined data it is formatted as `YYYY-MM`, such as `2015-01`. |
| `CEER_BROADM` | The broad real CEER. It summarizes the Canadian dollar's real movement against the full broad basket of trading-partner currencies represented in the Bank of Canada series. |
| `CEER_BROAD_XUSM` | The broad real CEER excluding the U.S. dollar. It summarizes the Canadian dollar's real movement against the broad basket after the U.S. dollar is excluded. |
| `CEER_MCM` | The real CEER against major currencies. It summarizes the Canadian dollar's real movement against the major-currency group represented in the series. |
| `CEER_OITPM` | The real CEER against other important trading partners. It summarizes the Canadian dollar's real movement against the group of other important trading-partner currencies represented in the series. |

The `M` suffix identifies the CEER columns as monthly series. The CEER
values are index levels rather than currency amounts. A change in one of
these columns shows a change in the Canadian dollar's real value relative to
the particular currency group represented by that column. The four columns
describe different currency groups, so they should not be treated as four
different measurements of the same basket.

### What the CEER numbers mean

Each CEER column is set to **100 for the base year 1999**. The number in a
later month is therefore a comparison with the average level for that
currency group in 1999:

- A value of `100` means the index is at its 1999 base-year level.
- A value of `110` means the index is 10 percent above its 1999 base-year
  level.
- A value of `90` means the index is 10 percent below its 1999 base-year
  level.

For example, the January 2015 value of `112.83` in `CEER_BROADM` means that
the broad real CEER was 12.83 percent above its 1999 base-year level:

```text
(112.83 - 100) / 100 × 100 = 12.83%
```

The value does **not** mean that one Canadian dollar bought 112.83 units of
another currency. It is an index comparison. Because these are real CEER
indexes, the comparison includes both exchange-rate movements and relative
price changes between Canada and the countries in the relevant basket.

Within the Bank of Canada convention used for these series, an increase in a
CEER index generally represents a real appreciation of the Canadian dollar
against that index's currency basket, while a decrease generally represents
a real depreciation. This interpretation applies separately to each column's
basket. For example, `CEER_BROADM` and `CEER_BROAD_XUSM` can move differently
because the second measure excludes the U.S. dollar.

## Combined data

The processed dataset is saved as:

```text
data/processed/df_combined.csv
```

It contains the BCPI columns, the CEER columns, the monthly `date` column,
and a `geography` column. The `geography` value is `Canada` for every
observation.

The combined dataset covers January 2015 through December 2025. Because the
notebook uses an outer merge, the combined data retain every date appearing
in either filtered source dataset.

Each row represents one month. The BCPI and CEER values in a row correspond
to the same `date`, and `geography` identifies the coverage as Canada. The
`date` column is stored in `YYYY-MM` format, such as:

```text
2015-01
```

## Work completed in `project.ipynb`

The notebook performs the following data preparation steps:

1. Reads the BCPI and CEER CSV files with pandas.
2. Checks each source dataset for duplicate values in the `date` column.
3. Converts both `date` columns to pandas datetime values.
4. Formats both date columns as `YYYY-MM`.
5. Filters both datasets to dates from `2015-01` through `2025-12`.
6. Displays the filtered BCPI and CEER data.
7. Combines the filtered datasets with an outer merge on `date`.
8. Adds `geography` with the value `Canada`.
9. Displays the combined dataset.
10. Saves the combined dataset to `data/processed/df_combined.csv`.

The main operations in the notebook are represented by:

```python
df_bcpi = pd.read_csv(
    "data/raw/BCPI_MONTHLY-sd-1972-01-01.csv"
)
df_ceer = pd.read_csv(
    "data/raw/CEER_BROADM,CEER_MCM,CEER_OITPM,CEER_BROAD_XUSM-sd-1990-01-01.csv"
)

duplicates_bcpi = df_bcpi[df_bcpi["date"].duplicated()]
duplicated_ceer = df_ceer[df_ceer["date"].duplicated()]

df_bcpi["date"] = pd.to_datetime(df_bcpi["date"]).dt.strftime("%Y-%m")
df_ceer["date"] = pd.to_datetime(df_ceer["date"]).dt.strftime("%Y-%m")

df_bcpi = df_bcpi.loc[
    (df_bcpi["date"] >= "2015-01") &
    (df_bcpi["date"] <= "2025-12")
]
df_ceer = df_ceer.loc[
    (df_ceer["date"] >= "2015-01") &
    (df_ceer["date"] <= "2025-12")
]

df_combined = df_bcpi.merge(df_ceer, on="date", how="outer")
df_combined["geography"] = "Canada"
df_combined.to_csv("../data/processed/df_combined.csv")
```

## Citation and attribution

The data used in this project are from the Bank of Canada:

> Bank of Canada. “Canadian Effective Exchange Rates.” Available at:  
> <https://www.bankofcanada.ca/rates/exchange/canadian-effective-exchange-rates/>

> Bank of Canada. “Bank of Canada Commodity Price Index.” Available at:  
> <https://www.bankofcanada.ca/rates/price-indexes/bcpi/>

The data should be used and redistributed in accordance with the Bank of
Canada's terms of use:

<https://www.bankofcanada.ca/terms/>
