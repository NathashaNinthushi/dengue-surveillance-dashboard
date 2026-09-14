# Exploring Dengue in Sri Lanka: An Interactive Surveillance Dashboard

An interactive dashboard, built in R and Quarto, that turns Sri Lanka's national weekly dengue surveillance record into an accessible exploratory tool — supporting both high-level national summaries and district-level investigation.

**Live dashboard:** https://nathashaninthushi-denguedashboard.share.connect.posit.cloud

**Report:** `AS2022467.pdf` (accompanying written report with methodology and key findings)

## Overview

Dengue is one of Sri Lanka's most significant public health challenges, producing a heavy and recurring case burden that demands continuous surveillance to anticipate seasonal rises and detect outbreaks early. This project explores the national weekly surveillance data through trend and seasonality decomposition and automatic outbreak detection, and presents the results as a six-page interactive dashboard.

## Data

The dashboard draws entirely on the `srilanka_weekly_data` dataset from the [denguedatahub](https://github.com) R package, which compiles the Weekly Epidemiological Reports of the Epidemiology Unit, Ministry of Health, Sri Lanka.

- **Format:** long format, one row per reporting area per epidemiological week
- **Fields:** year, week number, week start/end dates, reporting area, notified suspected dengue cases
- **Coverage:** 25,766 weekly observations across 26 reporting areas, 2006 to December 2025
- **Total:** 803,074 notified cases

Note: one reporting area (Kalmunai) has no matching administrative boundary and appears as unmapped ("NA") on the choropleth map, though it is retained in all totals, rankings and time series. Case counts are not standardised by population.

## Dashboard Structure

| Page | Contents |
|---|---|
| **Overview** | Headline indicators, national weekly curve, annual totals |
| **Districts** | Cumulative cases choropleth, ranking table, overlayable weekly comparison |
| **By Year** | District-by-year heatmap, small-multiple annual maps, annual district trends |
| **Seasonality & Trend** | STL decomposition, weekly heatmap |
| **Outbreaks** | Automatic anomaly detection, flagged outbreak weeks |
| **Data** | Searchable browser of raw records |

## Methods

- **Data preparation:** `tidyverse` and `lubridate` for manipulation, date parsing, and aggregation; national series placed on a regular weekly `tsibble` index (missing weeks filled as zero)
- **Trend/seasonality:** STL (Seasonal–Trend decomposition using Loess) with an annual 52-week period, via `feasts` and `fabletools`
- **Outbreak detection:** IQR-based anomaly screening of the STL remainder — flags weeks lying far above the expected trend-plus-seasonal level, rather than against a fixed case threshold
- **Visualisation:** `plotly` and `dygraphs` for interactive charts, `leaflet` and `sf` for spatial maps, `DT` for searchable tables, `viridis` for perceptually uniform colour scales, `crosstalk` for linked district selectors
- **Maps/heatmaps:** square-root colour transform to keep moderate districts legible against the extreme skew of epidemic years

## Key Findings

- **2017 epidemic:** the dominant event in the record, with 174,687 cases and a record national weekly incidence of 10,590 cases (week ending 15 July 2017), widely attributed to flooding after the 2017 southwest monsoon
- **Spatial concentration:** burden is heavily concentrated in the Western Province, led by Colombo (174,701 cumulative cases) and Gampaha (102,397)
- **Seasonality:** a broadly bimodal annual pattern — a dominant mid-year rise (weeks 20–31, southwest monsoon) and a smaller late-year rise (northeast monsoon) — with seasonal amplitude growing over time
- **Outbreak periods:** anomaly detection flags three clusters — 2017 (by far the largest), 2019–2020, and 2023–2024 — showing that both monsoon seasons can drive major outbreaks

## Tech Stack

R · Quarto · tidyverse · lubridate · feasts · fabletools · tsibble · plotly · dygraphs · leaflet · sf · DT · viridis · crosstalk · xts
