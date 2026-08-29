[README .md](https://github.com/user-attachments/files/31590234/README.md)
# Sales Performance Dashboard — Power BI (TSQLV4)

An interactive Power BI dashboard built on the **TSQLV4** sample database (Itzik Ben-Gan's well-known SQL Server training dataset), analyzing sales performance, customer behavior, product categories, and geographic distribution across 5 report pages.

![Home Page](screenshots/home.png)

## Data Model

Data is imported directly from SQL Server (`Sales`, `HR`, `Production` schemas) via Power Query and modeled into a star-schema structure:

- **Sales Orders** ↔ **HR Employees** (empid), **Sales Customers** (custid)
- **Sales OrderDetails** ↔ **Sales Orders** (orderid), **Production Products** (productid)
- **Production Products** ↔ **Production Suppliers**, **Production Categories**

## DAX Measures

| Measure | Logic |
|---|---|
| `AOV` | `DIVIDE(SUM(totalsales), DISTINCTCOUNT(orderid))` |
| `Total Orders` | `DISTINCTCOUNT(orderid)` |
| `Sales PY` | `CALCULATE(SUM(totalsales), SAMEPERIODLASTYEAR(OrderDate))` |
| `YoY Growth %` | `DIVIDE(SUM(totalsales) - [Sales PY], [Sales PY])` |
| `Customer Rank` | `RANKX(ALL(Sales Customers), CALCULATE(SUM(totalsales)))` |
| `Country Sales %` | Share of each country within the top-10 countries by sales (`TOPN` + `DIVIDE`) |

## Report Pages

1. **Home** — KPI cards (Sales, AOV, Orders, YoY Growth), monthly trend, sales by category.
2. **Sales Performance by Time** — Monthly sales and AOV trend across 2014–2016.
3. **Category Performance** — Category share over time (stacked area charts).
4. **Top 10 Customers** — Ranked customers by sales (pie, bar, table).
5. **Geography** — Map and country-level sales breakdown with `Country Sales %`.

![Top 10 Customers](screenshots/top10-customers.png)

![Geography](screenshots/geography.png)

## Files

- `PROJECT.pbix` — full Power BI report (open in Power BI Desktop)
- `screenshots/` — page previews

## How to Explore

Open `PROJECT.pbix` in [Power BI Desktop](https://powerbi.microsoft.com/desktop/). To point it at your own TSQLV4 instance, update the connection under **Transform Data → Data Source Settings**.

---

*Portfolio project demonstrating SQL-to-BI data modeling, DAX, and dashboard design in Power BI.*
