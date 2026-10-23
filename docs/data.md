# Data Documentation

This document describes the monthly data used in the DATA604 project. The project examines the relationship between Canadian commodity prices and Canadian effective exchange rates.

The data consist of two Bank of Canada datasets:

1. **Bank of Canada Commodity Price Index (BCPI)**
2. **Canadian Effective Exchange Rates (CEER)**

Both datasets are monthly time series and can be joined using their `date` column.

## Official data sources

- [Bank of Canada Commodity Price Index](https://www.bankofcanada.ca/rates/price-indexes/bcpi/)
- [Bank of Canada Canadian Effective Exchange Rates](https://www.bankofcanada.ca/rates/exchange/canadian-effective-exchange-rates/)

The source data were downloaded as CSV files from the Bank of Canada data pages. The files used in this project are stored in the project's `data/raw/` directory.

## Data files

### BCPI file

```text
data/raw/BCPI_MONTHLY-sd-1972-01-01.csv
```

This file contains monthly BCPI observations beginning in January 1972.

### CEER file

```text
data/raw/CEER_BROADM,CEER_MCM,CEER_OITPM,CEER_BROAD_XUSM-sd-1990-01-01.csv
```

Although the CEER filename includes `1990`, the observations in the supplied file begin in January 1999. The file contains monthly real CEER indices.

## General structure

Both datasets use one row per month. The `date` column identifies the observation month, and the remaining columns contain index values.

The dates are represented as the first day of each month. For example:

```text
1999-01-01
```

represents the monthly observation for January 1999, not a single daily observation for January 1.

The datasets can be joined on `date`:

```python
monthly = bcpi.merge(ceer, on="date", how="inner")
```

Because the BCPI and CEER files cover different historical periods, an inner join produces only the months for which both datasets have observations.

---

# BCPI data

## What the BCPI measures

The Bank of Canada Commodity Price Index measures changes in the prices of commodities produced in Canada and sold in world markets.

The index includes major Canadian commodity groups, including:

- Energy
- Metals and minerals
- Forestry products
- Agricultural products
- Fish products

The BCPI is designed to reflect the importance of Canadian commodities in the Canadian economy and in Canada's international trade. Commodity prices can affect:

- Export revenues
- Corporate profits
- Government revenues
- Investment
- The Canadian trade balance
- The value of the Canadian dollar
- Inflationary and deflationary pressures

The BCPI is an index rather than a dollar-denominated commodity price. Its values show how commodity prices have changed relative to the index's reference period.

## BCPI index interpretation

A BCPI value should be interpreted relative to another period.

For example, if the BCPI increases from 500 to 600:

```text
Percentage change = (600 - 500) / 500 × 100
                 = 20%
```

This indicates that the index increased by 20 percent over that period. It does not mean that an individual commodity increased by 20 percent, because the BCPI combines many commodities using weights.

The total BCPI may change because of:

- Changes in energy prices
- Changes in metal and mineral prices
- Changes in forestry prices
- Changes in agricultural prices
- Changes in fish prices
- Changes in the relative weights assigned to commodities

## BCPI columns

### `date`

The monthly observation date.

The values are stored as dates representing the first day of each month. For example:

```text
1972-01-01
```

represents January 1972.

This column is used to:

- Sort observations chronologically
- Filter observations by date
- Join the BCPI data with CEER data
- Create time-series plots
- Calculate monthly changes and lagged variables

When loading the data into pandas, it is recommended to convert the column to a datetime type:

```python
bcpi["date"] = pd.to_datetime(bcpi["date"])
```

### `M.BCPI`

The total monthly Bank of Canada Commodity Price Index.

This is the broadest BCPI measure in the dataset. It combines the commodity categories represented by the other BCPI columns using Bank of Canada commodity weights.

`M.BCPI` is the primary measure of overall Canadian commodity price conditions used in this project.

A rise in `M.BCPI` generally indicates that the weighted average price of Canadian commodities has increased relative to the index reference period. A decline generally indicates weaker overall commodity prices.

The `M.` prefix identifies the series as monthly.

### `M.BCNE`

The monthly BCPI excluding energy.

This index measures changes in the prices of the non-energy commodities included in the BCPI. It excludes the energy component and therefore provides a measure of commodity price movements that is less directly influenced by oil, natural gas, coal, and other energy prices.

`M.BCNE` is useful for comparing:

- Overall commodity price movements
- Energy commodity price movements
- Non-energy commodity price movements

For example, if `M.BCPI` rises substantially while `M.BCNE` changes only slightly, energy prices may be an important contributor to the overall increase.

### `M.ENER`

The monthly energy component of the BCPI.

This series measures the price movements of energy commodities included in the index. Energy commodities include products such as crude oil and other energy-related commodities represented in the Bank of Canada's commodity basket.

Energy prices can be especially important for Canada because Canada is a major producer and exporter of energy products. Changes in energy prices can affect:

- Canadian export revenues
- The trade balance
- Business investment
- Provincial and federal government revenues
- The Canadian dollar
- Inflation expectations

`M.ENER` may be more volatile than some other BCPI components because energy prices can respond quickly to global economic conditions, geopolitical events, changes in production, and changes in demand.

### `M.MTLS`

The monthly metals and minerals component of the BCPI.

This series measures price movements for metals and minerals produced in Canada and included in the Bank of Canada's commodity basket.

Metals and minerals are used in:

- Construction
- Manufacturing
- Transportation
- Technology
- Infrastructure
- Energy production

This component can be influenced by:

- Global industrial production
- Construction activity
- Chinese and other international demand
- Interest rates
- Exchange rates
- Supply disruptions
- Demand for critical minerals

The `M.MTLS` series allows the analysis to distinguish metal and mineral price movements from energy and other commodity movements.

### `M.FOPR`

The monthly forestry products component of the BCPI.

This series measures price movements for Canadian forestry products included in the commodity basket.

Forestry products can include products associated with:

- Lumber
- Pulp
- Paper
- Other forest products

The forestry component may be affected by:

- Residential construction
- Housing starts
- Renovation activity
- International construction demand
- Pulp and paper demand
- Transportation costs
- Logging and production conditions
- Supply-chain conditions

`M.FOPR` is useful for examining whether forestry prices behave differently from energy, metals, or agricultural prices.

### `M.AGRI`

The monthly agricultural products component of the BCPI.

This series measures price movements for agricultural commodities produced in Canada and included in the Bank of Canada's commodity basket.

Agricultural commodity prices can be affected by:

- Weather
- Crop yields
- Livestock conditions
- Global food demand
- International trade policy
- Fertilizer and input costs
- Transportation costs
- Exchange-rate movements
- Global inventories

Agricultural prices can show seasonal and weather-related variation. A monthly change in `M.AGRI` should therefore be interpreted in the context of agricultural production cycles and global market conditions.

### `M.FISH`

The monthly fish products component of the BCPI.

This series measures price movements for fish and seafood products included in the commodity basket.

Fish prices may be influenced by:

- Catch volumes
- Aquaculture production
- Weather and ocean conditions
- Biological conditions
- International demand
- Export markets
- Quotas and regulations
- Transportation and processing costs

The fish component may be relatively volatile because production and supply can vary substantially from month to month.

## How BCPI data are constructed

The Bank of Canada constructs BCPI weights primarily using Statistics Canada input-output tables. These tables help estimate the importance of individual commodities and commodity groups in Canadian production and trade.

Because input-output tables are produced with a lag, the Bank of Canada also uses additional information to update and project weights. This can include:

- Commodity prices
- Canadian production volumes
- Official production statistics
- Data from Statistics Canada
- Data from Natural Resources Canada
- Data from the National Energy Board and related sources

The weights are intended to represent the economic importance of commodities in the Canadian economy. They are not simply equal weights across commodities.

The Bank of Canada periodically updates the underlying commodity data and may revise historical values when improved source data become available.

## BCPI price frequency

The BCPI is constructed from commodity prices that may originally be available at different frequencies:

- Daily
- Weekly
- Monthly

For the monthly series:

- Daily prices are generally averaged over the month.
- Weekly prices are averaged according to the Bank of Canada's weekly methodology.
- Monthly prices are averaged over the available days in the month.

When data for the reference period are not yet available, the most recent available data may be repeated temporarily. Historical observations can subsequently be revised when the actual data are received.

Therefore, BCPI values should be treated as potentially revisable economic data rather than permanently fixed observations.

## BCPI source revisions

The Bank of Canada documents revisions to certain commodity price sources and methodology. Examples described on the source page include changes affecting:

- Coal
- Barley
- Lumber
- Western Canadian Select

These revisions may be applied retroactively. As a result, a newly downloaded version of the CSV may not exactly match an older downloaded version.

---

# CEER data

## What the CEER measures

The Canadian Effective Exchange Rate is a weighted index that summarizes the value of the Canadian dollar relative to the currencies of Canada's trading partners.

Instead of comparing the Canadian dollar with only one currency, an effective exchange rate combines several bilateral exchange rates using trade-related weights.

The CEER provides a broader measure of the Canadian dollar's external value than a single exchange rate such as CAD/USD.

The CEER dataset used in this project contains **real monthly CEER indices**.

## Nominal versus real CEER

The Bank of Canada publishes both nominal and real CEER measures.

### Nominal CEER

A nominal CEER is based on bilateral nominal exchange rates. It measures movements in the value of the Canadian dollar relative to the currencies in the relevant trading-partner group.

### Real CEER

A real CEER adjusts the nominal exchange rate for relative price changes between Canada and its trading partners.

The real CEER therefore accounts for both:

- Exchange-rate movements
- Differences in consumer price levels or inflation between Canada and trading partners

The real exchange-rate calculation replaces the nominal bilateral exchange rate with a price-adjusted exchange rate. In simplified form, the real bilateral exchange rate is represented as:

```text
Real exchange rate = Nominal exchange rate × Canadian price index / Foreign price index
```

The CEER series in the supplied file are real series. They are not nominal CEER series.

Real CEER data are available only after the required consumer price data are available for the relevant trading partners. This means real CEER observations may be released later than nominal CEER observations.

## CEER index interpretation

The CEER is an index, not a direct quotation such as “Canadian dollars per U.S. dollar.”

Because the Bank of Canada methodology defines the bilateral exchange rate as the price of Canadian dollars in terms of the currency of the trading-partner country, a higher CEER value generally represents an appreciation of the Canadian dollar relative to the relevant weighted basket after accounting for relative prices.

A lower CEER value generally represents a depreciation.

However, interpretation should always be made relative to:

- The index's reference period
- The specific CEER basket
- The distinction between nominal and real values
- The period being analyzed

For analysis, percentage changes or log differences are often more informative than comparing the raw index levels:

```python
ceer["CEER_BROADM_pct_change"] = ceer["CEER_BROADM"].pct_change() * 100
```

## CEER columns

### `date`

The monthly CEER observation date.

The values are represented by the first day of each month. For example:

```text
1999-01-01
```

represents the monthly observation for January 1999.

The `date` column is used to:

- Sort observations chronologically
- Align CEER observations with BCPI observations
- Filter observations by time period
- Create lagged variables
- Calculate monthly exchange-rate changes

Recommended conversion:

```python
ceer["date"] = pd.to_datetime(ceer["date"])
```

### `CEER_BROADM`

The monthly real broad Canadian effective exchange rate.

This is the broadest CEER measure in the supplied dataset. It summarizes the real value of the Canadian dollar against a broad basket of trading-partner currencies.

The broad basket is designed to reflect Canada's international trade relationships. The weighting system considers Canada's:

- Imports
- Exports
- Competition with other countries in third-country markets

`CEER_BROADM` is the principal broad real exchange-rate measure in this project.

A rise in `CEER_BROADM` generally indicates a real appreciation of the Canadian dollar against the broad trading-partner basket. A decline generally indicates a real depreciation.

### `CEER_BROAD_XUSM`

The monthly real broad Canadian effective exchange rate excluding the U.S. dollar.

This series measures the real value of the Canadian dollar against a broad basket of trading-partner currencies after excluding the U.S. dollar.

The United States is Canada's largest trading partner, so a broad CEER measure can be strongly affected by CAD/USD movements. Excluding the U.S. dollar provides a way to examine whether the Canadian dollar's movement was broad-based or primarily driven by its relationship with the United States.

Comparing the two series can be informative:

```text
CEER_BROADM
CEER_BROAD_XUSM
```

For example:

- If both series move similarly, the movement may be broad across Canada's trading partners.
- If `CEER_BROADM` changes substantially but `CEER_BROAD_XUSM` changes less, the U.S. dollar relationship may be an important contributor.
- If `CEER_BROAD_XUSM` changes substantially while `CEER_BROADM` changes less, movements against non-U.S. trading partners may be important.

The `M` at the end of the Bank of Canada series identifier indicates the monthly frequency.

### `CEER_MCM`

The monthly real CEER against major currencies.

This series measures the real value of the Canadian dollar against a basket of major currencies associated with major international trading partners.

It provides a focused measure of Canadian-dollar movements against major currencies rather than the broader set of currencies included in the broad CEER.

The major-currency basket may be affected by:

- Movements in the U.S. dollar
- Movements in the euro
- Movements in the Japanese yen
- Movements in other major currencies included by the Bank of Canada
- Relative inflation or consumer-price changes
- Monetary-policy differences
- Global risk sentiment
- Commodity-market conditions

The exact currency composition and weights can change as Canada's trade relationships change. The Bank of Canada source page should be consulted for the current methodology and basket details.

### `CEER_OITPM`

The monthly real CEER against other important trading partners.

This series measures the real value of the Canadian dollar against a basket of other important trading-partner currencies that are not included in the major-currency grouping.

The series helps capture Canada's exchange-rate relationships beyond the major-currency basket.

It can be influenced by:

- Trade with emerging-market economies
- Regional economic conditions
- Commodity demand
- Changes in trade patterns
- Relative inflation
- Currency movements among Canada's other important trading partners

`CEER_OITPM` can be compared with `CEER_MCM` to examine whether Canadian-dollar movements differ between major-currency economies and other important trading partners.

## How CEER weights are constructed

The Bank of Canada uses trade-related weights to combine bilateral exchange rates.

The total weight assigned to a trading partner reflects three types of trade relationships:

1. Canadian imports from that country
2. Canadian exports to that country
3. Competition between Canada and that country in third markets

The Bank of Canada describes the total weight as an equal combination of the import and export/third-market components. In simplified form:

```text
Total weight
= 1/2 × import weight
+ 1/2 × export and third-market competition weight
```

The export and third-market portion gives weight to:

- Canada's direct exports to the trading partner
- Competition between Canadian and foreign exporters in other markets

This means the CEER is not based only on bilateral trade between Canada and another country. It also accounts for competition in markets where Canadian and foreign producers sell similar goods.

The weights are normalized so that the weights across the included currencies sum to one.

## Nominal and real CEER methodology

The nominal CEER is constructed by combining changes in bilateral exchange rates:

```text
I_t = I_(t-1) × product over j of
      (e_(j,t) / e_(j,t-1)) ^ w_(j,t)
```

Where:

- `I_t` is the effective exchange-rate index at time `t`
- `e_(j,t)` is the bilateral exchange rate with trading partner `j`
- `w_(j,t)` is the weight assigned to trading partner `j`
- `j` identifies the currencies included in the basket
- The weights sum to one

For the real CEER, each bilateral exchange rate is adjusted for relative price levels before the weighted index is calculated.

## Availability and timing of CEER data

Nominal CEER indices are available daily and monthly.

Real CEER indices are available monthly because they depend on consumer price index data for Canada and the relevant trading partners. The real series are released only after the required price data are available for a complete month.

This creates two important considerations:

- The real CEER may have fewer observations than the nominal CEER.
- The most recent real CEER observation may be released later than the corresponding nominal observation.

---

# Data collection and preparation

## Source collection

The data were collected from the Bank of Canada's official data pages and saved as CSV exports.

The project uses:

```text
data/raw/BCPI_MONTHLY-sd-1972-01-01.csv
data/raw/CEER_BROADM,CEER_MCM,CEER_OITPM,CEER_BROAD_XUSM-sd-1990-01-01.csv
```

The raw files should be preserved without modification so that the analysis remains reproducible and traceable to the original downloads.

## Loading the data with pandas

The files can be loaded as follows:

```python
import pandas as pd

bcpi = pd.read_csv(
    "data/raw/BCPI_MONTHLY-sd-1972-01-01.csv",
    encoding="utf-8-sig"
)

ceer = pd.read_csv(
    "data/raw/CEER_BROADM,CEER_MCM,CEER_OITPM,CEER_BROAD_XUSM-sd-1990-01-01.csv",
    encoding="utf-8-sig"
)

bcpi["date"] = pd.to_datetime(bcpi["date"])
ceer["date"] = pd.to_datetime(ceer["date"])
```

The `utf-8-sig` encoding is used because Bank of Canada CSV files may include a UTF-8 byte-order mark at the beginning of the file.

## Setting the date as the index

For time-series analysis, the date can be used as the index:

```python
bcpi = bcpi.set_index("date").sort_index()
ceer = ceer.set_index("date").sort_index()
```

After this step, both data frames have a `DatetimeIndex`.

A date range can then be selected using `.loc[]`:

```python
bcpi_2015_2025 = bcpi.loc["2015-01-01":"2025-12-31"]
ceer_2015_2025 = ceer.loc["2015-01-01":"2025-12-31"]
```

`between_time()` should not be used for this purpose. `between_time()` filters times of day, such as 09:00 through 17:00. Date ranges should be filtered with `.loc[]` or boolean date conditions.

## Joining BCPI and CEER

The datasets can be joined on their monthly date:

```python
monthly = (
    bcpi.reset_index()
        .merge(ceer.reset_index(), on="date", how="inner")
        .sort_values("date")
)
```

Alternatively, if both data frames use `date` as their index:

```python
monthly = bcpi.join(ceer, how="inner")
```

An inner join keeps only months that appear in both datasets.

Because the BCPI data begin earlier than the CEER data, the combined dataset begins when the CEER series begins. Similarly, the combined dataset ends at the latest month available in both files.

## Recommended validation checks

The following checks help confirm that the data were loaded correctly:

```python
print(bcpi.index.dtype)
print(ceer.index.dtype)

print(bcpi.index.min())
print(bcpi.index.max())

print(ceer.index.min())
print(ceer.index.max())

print(bcpi.isna().sum())
print(ceer.isna().sum())

print(bcpi.index.is_monotonic_increasing)
print(ceer.index.is_monotonic_increasing)
```

The date indexes should be datetime indexes, sorted in ascending order, and should not contain unexpected duplicate months.

Duplicate dates can be checked with:

```python
print(bcpi.index.duplicated().sum())
print(ceer.index.duplicated().sum())
```

## Data transformations used in analysis

The raw index levels should be retained. Additional analytical variables can be created separately.

### Monthly percentage changes

```python
bcpi["M.BCPI_pct_change"] = bcpi["M.BCPI"].pct_change() * 100
ceer["CEER_BROADM_pct_change"] = ceer["CEER_BROADM"].pct_change() * 100
```

### Monthly differences

```python
bcpi["M.BCPI_change"] = bcpi["M.BCPI"].diff()
ceer["CEER_BROADM_change"] = ceer["CEER_BROADM"].diff()
```

### Log changes

Log changes are often useful for time-series modelling:

```python
import numpy as np

bcpi["M.BCPI_log_change"] = np.log(bcpi["M.BCPI"]).diff()
ceer["CEER_BROADM_log_change"] = np.log(ceer["CEER_BROADM"]).diff()
```

### Lagged variables

To examine whether commodity prices are associated with future exchange-rate movements:

```python
monthly["M.BCPI_lag_1"] = monthly["M.BCPI"].shift(1)
monthly["M.BCPI_pct_change_lag_1"] = (
    monthly["M.BCPI"].pct_change().shift(1)
)
```

The original columns should not be overwritten when creating transformed variables. Keeping the raw columns allows the analysis to be audited and reproduced.

---

# Period coverage and overlap

The BCPI file contains a substantially longer historical period than the CEER file.

The BCPI data begin in:

```text
1972-01
```

The supplied CEER data begin in:

```text
1999-01
```

Consequently, analyses requiring both BCPI and CEER variables must use their overlapping period.

A safe way to identify the overlap programmatically is:

```python
overlap_start = max(bcpi.index.min(), ceer.index.min())
overlap_end = min(bcpi.index.max(), ceer.index.max())

print(overlap_start)
print(overlap_end)
```

The overlap period may change if the Bank of Canada files are downloaded again in the future because new observations may be added or historical values may be revised.

---

# Important limitations

## Index values are not commodity prices in dollars

BCPI values are index values. They should not be interpreted as the dollar price of oil, lumber, wheat, fish, or any other individual commodity.

The components are weighted indexes that summarize price movements.

## CEER values are not bilateral exchange-rate quotations

A CEER value is not equivalent to a quotation such as:

```text
CAD per USD
```

It is a weighted index that summarizes the Canadian dollar's value relative to a basket of currencies.

## The CEER data are real, not nominal

The supplied CEER columns are real CEER measures. They account for both exchange-rate movements and relative price changes.

They should not be described as nominal exchange rates.

## Historical data may be revised

Both commodity-price and exchange-rate data may be revised by the Bank of Canada.

Revisions can occur because:

- Source data become available after initial publication
- Commodity price sources are updated
- Historical methodology changes are applied
- Input-output data are revised
- Trading weights are updated
- Consumer price data are revised
- Errors in earlier source data are corrected

Analyses should record the download date and preserve the original CSV files used.

## Different series have different economic interpretations

The total BCPI, energy BCPI, non-energy BCPI, and individual commodity-group indexes measure different parts of the commodity market.

Similarly, the broad CEER, broad CEER excluding the U.S. dollar, major-currency CEER, and other-important-trading-partner CEER measure different currency baskets.

These series should not be treated as interchangeable.

## Correlation does not establish causation

A relationship between BCPI and CEER movements does not by itself demonstrate that commodity prices caused exchange-rate movements.

Both variables may be affected by other factors, including:

- Global economic growth
- Interest-rate changes
- Monetary policy
- Inflation
- Geopolitical events
- Trade policy
- Energy-market disruptions
- Financial-market conditions
- Investor risk sentiment
- U.S. dollar movements

Any statistical relationship should therefore be interpreted as an association unless a suitable causal research design is used.

---

# Recommended use in this project

The following variables are suitable for the project's planned analyses:

| Research purpose | Suggested columns |
| --- | --- |
| Overall commodity-price conditions | `M.BCPI` |
| Commodity prices excluding energy | `M.BCNE` |
| Energy-price movements | `M.ENER` |
| Metals and mineral prices | `M.MTLS` |
| Forestry prices | `M.FOPR` |
| Agricultural prices | `M.AGRI` |
| Fish prices | `M.FISH` |
| Broad real Canadian-dollar movement | `CEER_BROADM` |
| Broad real Canadian-dollar movement excluding the U.S. dollar | `CEER_BROAD_XUSM` |
| Real Canadian-dollar movement against major currencies | `CEER_MCM` |
| Real Canadian-dollar movement against other important trading partners | `CEER_OITPM` |

For comparisons between commodity prices and CEER, it is generally preferable to analyze:

- Monthly percentage changes
- Monthly differences
- Log changes
- Lagged changes
- Rolling averages
- Changes across clearly defined periods

Raw index levels can be used for visualization, but regressions and forecasting models should carefully consider whether the series are stationary.

---

# Citation and attribution

The data used in this project are from the Bank of Canada:

> Bank of Canada. “Canadian Effective Exchange Rates.” Available at:  
> <https://www.bankofcanada.ca/rates/exchange/canadian-effective-exchange-rates/>

> Bank of Canada. “Bank of Canada Commodity Price Index.” Available at:  
> <https://www.bankofcanada.ca/rates/price-indexes/bcpi/>

The data should be used and redistributed in accordance with the Bank of Canada's terms of use:

<https://www.bankofcanada.ca/terms/>

For reproducibility, future analyses should record:

- The source URL
- The CSV filename
- The date the data were downloaded
- The date range used
- Any filtering or transformations
- Any revisions to the source data
- The software and package versions used