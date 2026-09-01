# Bridging the Digital Divide — Power BI Capstone

An interactive Power BI dashboard analysing internet and mobile adoption across South Africa, five African peers, and three global benchmarks, using 24 years of World Bank data.

Final capstone project for the AnalystLab Africa data analytics internship (Week 8).

---

## Problem Statement

Mobile phone ownership across Africa has grown explosively, but internet usage has not kept pace. This project examines that disconnect — why mobile adoption leapfrogged while internet access lagged — and identifies what separates the more-connected countries from the less-connected ones.

It answers four questions:

- How wide is the gap between mobile subscriptions and actual internet usage?
- How does South Africa compare to its peers and to global benchmarks?
- What factors — wealth, electricity, infrastructure — explain the differences?
- How many people, in absolute terms, remain offline?

---

## Repository Contents

| File | Description |
|---|---|
| `Capstone_Project.pbix` | The interactive Power BI dashboard |
| `Capstone_Report.pdf` | Full written report (objective, methodology, findings, recommendations) |
| `README.md` | This file |

---

## Dataset

**Source:** [World Bank World Development Indicators](https://datatopics.worldbank.org/world-development-indicators/) (bulk CSV download)

**Scope after cleaning:** 1,264 records — 9 entities × 6 indicators × 25 years (2000–2024).

**Indicators used:**

| Indicator | Code | Role |
|---|---|---|
| Individuals using the internet (%) | `IT.NET.USER.ZS` | Primary connectivity measure |
| Mobile cellular subscriptions (per 100) | `IT.CEL.SETS.P2` | The mobile leapfrog |
| Fixed broadband subscriptions (per 100) | `IT.NET.BBND.P2` | Infrastructure that didn't leapfrog |
| Access to electricity (%) | `EG.ELC.ACCS.ZS` | Physical precondition |
| GDP per capita (US$) | `NY.GDP.PCAP.CD` | Tests wealth as a driver |
| Population, total | `SP.POP.TOTL` | Converts % into people |

**Entities:** South Africa, Nigeria, Kenya, Egypt, Ghana, Rwanda, plus Sub-Saharan Africa, World, and High income aggregates.

---

## ETL and Modelling

All transformation was done in **Power Query**; all measures in **DAX**.

**Cleaning steps:**

1. Promoted first row to headers
2. Filtered ~380,000 rows to the six indicators
3. Filtered to the nine entities
4. Removed pre-2000 and empty 2025 columns
5. **Unpivoted** year columns from wide to long format (54 rows → 1,264)
6. Converted `Year` to whole number so time-series sort chronologically

**Key measures:**

```
Internet Users % = CALCULATE(AVERAGE(DigitalDivide[Value]), DigitalDivide[Indicator Code] = "IT.NET.USER.ZS")

Adoption Gap = [Mobile Subs per 100] - [Internet Users %]

People Offline = [Population] * (1 - DIVIDE([Internet Users %], 100))
```

`Adoption Gap` quantifies the leapfrog effect; `People Offline` converts a percentage into absolute human scale.

---

## Dashboard

Three interactive pages, driven by country and year slicers:

- **Overview** — KPI cards, internet-usage trend line (2000–2024), and a mobile-vs-internet comparison
- **Country Comparison** — ranked bar chart, a GDP-vs-internet scatter (bubble-sized by population), electricity comparison, and a full data table
- **Insights** — written summary of the key findings

---

## Key Findings (2024)

**The mobile-internet gap is large and universal.** South Africa has 179 mobile subscriptions per 100 people but only 78% internet usage — a gap of 101. Kenya: 127 mobile subscriptions, 35% online.

**The scale of exclusion is enormous.** Nigeria has ~137 million people offline. Across Sub-Saharan Africa, ~857 million remain unconnected.

**Fixed broadband never arrived.** South Africa has 5.3 broadband subscriptions per 100 people; Nigeria 0.1; high-income countries 37.8. Africa's connectivity is almost entirely mobile.

**Wealth and electricity track connectivity.** GDP per capita and internet usage correlate at 0.77 across the six African countries. Electricity access sets the ceiling.

**South Africa is near saturation** at 78%, above the world average of 71%, but still has 14 million people offline.

---

## Recommendations

1. Prioritise mobile-data **affordability** over new network build-out — coverage already exists
2. Tie digital-inclusion programmes to **electrification** programmes
3. Invest in **digital-skills training** for the newly-reachable
4. Treat South Africa as a **near-saturation market** requiring targeted rural access
5. Set policy targets in **absolute numbers of people**, not percentages

---

## Tools

Microsoft Power BI (Power Query, DAX) · World Bank WDI dataset

---

*Data source: World Bank World Development Indicators. This is an academic project and not policy advice.*
