# DATA604 Project

## Project overview

This project examines the relationship between Canadian commodity prices and the value of the Canadian dollar.

Canada is a major exporter of commodities such as energy products, metals, forestry products, agricultural goods, and fish. Because these commodities are important to Canada's economy and international trade, changes in their prices may be associated with changes in the value of the Canadian dollar.

The project uses historical monthly data from the Bank of Canada to investigate whether movements in Canadian commodity prices are related to movements in Canada's effective exchange rate.

The goal is to identify broad economic relationships that may help explain how commodity-market conditions and the Canadian dollar have moved together over time.

## Main research questions

The project investigates the following questions:

1. How does the Canadian dollar's effective exchange rate behave during periods when commodity prices are relatively high?
2. Are changes in the Canadian dollar different during months when commodity prices increase compared with months when they decrease?
3. Are energy prices more strongly associated with exchange-rate movements than non-energy commodity prices?
4. How are metals, forestry products, agricultural products, and fish prices related to the Canadian dollar?
5. Did the relationship between commodity prices and the Canadian dollar change before, during, and after the COVID-19 pandemic?
6. Do commodity-price movements appear to be associated with exchange-rate movements in the following month?
7. Does the relationship differ depending on whether the Canadian dollar is compared with a broad group of trading partners, major currencies, or trading partners other than the United States?

These questions are documented in [`docs/rqs.md`](docs/rqs.md).

## Data sources

The project uses two official Bank of Canada datasets.

### Bank of Canada Commodity Price Index

The Bank of Canada Commodity Price Index, or BCPI, summarizes changes in the prices of commodities produced in Canada and sold in international markets.

The BCPI includes measures related to:

- Overall commodity prices
- Commodity prices excluding energy
- Energy
- Metals and minerals
- Forestry products
- Agricultural products
- Fish products

The BCPI data used in this project begin in January 1972.

Source:

[Bank of Canada Commodity Price Index](https://www.bankofcanada.ca/rates/price-indexes/bcpi/)

### Canadian Effective Exchange Rates

The Canadian Effective Exchange Rate, or CEER, summarizes the value of the Canadian dollar against a weighted group of currencies belonging to Canada's trading partners.

The CEER data used in this project include real exchange-rate measures for:

- A broad group of trading partners
- A broad group excluding the United States
- Major currencies
- Other important trading partners

The CEER data used in this project begin in January 1999.

Source:

[Bank of Canada Canadian Effective Exchange Rates](https://www.bankofcanada.ca/rates/exchange/canadian-effective-exchange-rates/)

## What the data represent

The BCPI and CEER are indexes. They are designed to show how prices or exchange rates change over time relative to a reference period.

They are not direct dollar amounts.

For example:

- A BCPI value is not the price of a specific commodity.
- A CEER value is not a direct Canadian-dollar-to-U.S.-dollar exchange-rate quotation.
- A higher or lower index value should be interpreted in relation to other periods.
- Changes in the indexes are more meaningful than comparing the index values as if they were dollar amounts.

The CEER data used in this project are real exchange-rate indexes. This means that they account for both currency movements and differences in price levels between Canada and its trading partners.

## Project data files

The raw data files are stored in the `data/raw/` directory.

The project contains:

- `BCPI_MONTHLY-sd-1972-01-01.csv`
- `CEER_BROADM,CEER_MCM,CEER_OITPM,CEER_BROAD_XUSM-sd-1990-01-01.csv`

The CEER filename includes `1990`, but the observations in the supplied file begin in January 1999.

The original downloaded files are retained so that the analysis can be traced back to the Bank of Canada source data.

## Project structure

```text
data604_project/
├── data/
│   └── raw/
│       ├── BCPI_MONTHLY-sd-1972-01-01.csv
│       └── CEER_BROADM,CEER_MCM,CEER_OITPM,CEER_BROAD_XUSM-sd-1990-01-01.csv
├── docs/
│   ├── data.md
│   └── rqs.md
├── notebooks/
│   └── project.ipynb
└── README.md
```

## Period covered by the analysis

The BCPI dataset begins earlier than the CEER dataset.

The available data cover approximately:

- BCPI: January 1972 through September 2026
- CEER: January 1999 through June 2026

When both datasets are analyzed together, the analysis is limited to the period for which both datasets contain observations. Based on the current files, this common period begins in January 1999 and ends in June 2026.

The available period may change when updated data are downloaded from the Bank of Canada.

## How the project supports decision-making

The project is intended to help a non-technical audience understand whether Canadian commodity-market conditions and the Canadian dollar tend to move together.

Possible uses of the findings include:

- Improving understanding of Canada's exposure to commodity-price cycles
- Supporting discussions about export revenues and trade conditions
- Providing context for movements in the Canadian dollar
- Comparing energy-related effects with non-energy commodity effects
- Identifying whether relationships changed during unusual economic periods
- Supporting future economic forecasting and research

The results are intended to provide context and evidence. They should not be interpreted as a complete forecast of the Canadian dollar or as proof that commodity prices are the only cause of exchange-rate movements.

## Important limitations

### Association does not prove cause

If commodity prices and the Canadian dollar move together, this does not necessarily mean that one directly caused the other.

Both may be affected by other factors, including:

- Global economic growth
- Interest rates
- Monetary policy
- Inflation
- Geopolitical events
- International trade policies
- Energy-market disruptions
- Investor confidence
- Financial-market conditions
- Movements in the U.S. dollar

The project therefore focuses on relationships and associations rather than claiming that commodity prices alone determine the value of the Canadian dollar.

### The datasets cover different periods

The BCPI data begin in 1972, while the CEER data begin in 1999. Comparisons involving both datasets cannot include the earlier BCPI-only period.

### The CEER data are real exchange-rate indexes

The CEER data are adjusted for relative price changes. They should not be interpreted as nominal exchange rates or as direct currency quotations.

### Index values are not individual prices

The BCPI combines many commodities using economic weights. A change in the total BCPI does not mean that every commodity changed by the same amount.

Likewise, the CEER combines multiple currencies using trade-related weights. A change in the broad CEER does not mean that the Canadian dollar moved by the same amount against every trading partner.

### Historical values may change

The Bank of Canada may revise historical observations when new information becomes available or when source data, commodity weights, exchange-rate weights, or methods are updated.

The findings should therefore be understood as based on the versions of the datasets available when they were collected.

## Documentation

Detailed information about the datasets, including descriptions of each column, how the data are constructed, how the indexes should be interpreted, and the main limitations, is available in [`docs/data.md`](docs/data.md).

The project's research questions are available in [`docs/rqs.md`](docs/rqs.md).

## Data attribution

The data are provided by the Bank of Canada.

Sources:

- [Canadian Effective Exchange Rates](https://www.bankofcanada.ca/rates/exchange/canadian-effective-exchange-rates/)
- [Bank of Canada Commodity Price Index](https://www.bankofcanada.ca/rates/price-indexes/bcpi/)

The Bank of Canada's terms of use are available at:

[Bank of Canada Terms of Use](https://www.bankofcanada.ca/terms/)