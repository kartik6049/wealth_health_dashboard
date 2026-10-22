# Lifelines — Wealth & Health of Nations

An animated dashboard showing how 142 countries got richer and lived longer between 1952 and 2007, inspired by Hans Rosling's famous Gapminder chart.

**[View the live dashboard →](https://kartik6049.github.io/wealth_health_dashboard/)**


## What it shows

| View | What you see |
| --- | --- |
| **Animated bubble chart** | Each country as a bubble: income on the x-axis (log scale), life expectancy on the y-axis, size by population |
| **Key figures** | Total population, population-weighted life expectancy, median income and countries shown, each compared with 1952 |
| **Continent trends** | Life expectancy per continent over 55 years, with the world average highlighted |
| **World map** | Life expectancy by country for the current snapshot year |
| **Country table** | Sortable rankings with sparklines, change since 1952 and status badges |

## Things to look for

- **China, 1962:** life expectancy drops from about 50.5 to 44.5 years during the Great Leap Forward famine.
- **Rwanda, 1992:** life expectancy falls to about 23.6 years around the genocide.
- **Zimbabwe:** one of the few countries that ends lower than it started, falling from about 60 years in 1992 to 43.5 in 2007.
- **Japan, 2007:** the highest life expectancy in the dataset, at about 82.6 years.

Tip: press **Trace** on China or Rwanda in the table to draw their path across the decades.

## Interactions

| Action | How |
| --- | --- |
| Play / pause | `Space` or the orange button |
| Step 5 years | `←` `→` |
| Change speed | 1×, 2×, 4× |
| Pin a country | Click its bubble, map region, or **Pin** in the table |
| Draw its path | **Trace** in the table |
| Filter continents | Sidebar; double-click to isolate one |
| Search and sort | Search box and column headers in the table |

Pinned and traced countries are saved in your browser and survive a reload.

## Built with

| Tool | Used for |
| --- | --- |
| [D3.js](https://d3js.org/) | Scales, grouping, medians and data loading |
| HTML Canvas | The animated bubble chart, drawn every frame |
| [CanvasJS](https://canvasjs.com/) | Continent life-expectancy trend lines |
| [Google Charts](https://developers.google.com/chart) | GeoChart world map |
| Inline SVG | Table sparklines and icons |

No frameworks and no build step: the whole project is a single `index.html`.

## Data

The [Gapminder five-year dataset](https://github.com/plotly/datasets/blob/master/gapminderDataFiveYear.csv) (via Plotly's public datasets): 142 countries × 12 snapshots from 1952 to 2007. Columns: country, continent, year, population, life expectancy and GDP per capita (inflation-adjusted US dollars).

Between snapshots, values are interpolated so the animation moves smoothly: linear for life expectancy and population, logarithmic for income. Continent, world and headline figures are population-weighted.

## Architecture

The app is built from small components that all read from one central state store:

```
Inputs (clicks, keys, scrubber, playback engine)
        │  store.set(patch)
        ▼
      Store ──► localStorage (pins, traces)
        │
        ├─► Playback      every change
        ├─► Sidebar       every 150 ms
        ├─► Kpis          every 100 ms
        ├─► BubbleChart   every frame
        ├─► TrendChart    every 100 ms
        ├─► MapChart      per 5-year snapshot
        └─► CountryTable  every 120 ms
```

The store tells each component which keys changed, and each component repaints only when it needs to. Throttling the slower views keeps playback smooth at 60 fps.

## Run it locally

```bash
git clone https://github.com/yourusername/wealth-health-dashboard.git
cd wealth-health-dashboard
# open index.html in your browser, or serve it:
npx serve .
```

An internet connection is needed for the libraries and data.

## Design

A matte ink theme (#0D0E11) with crisp white text, a five-step grey scale for structure, and international orange (#FF4F00) used only for active states, pinned countries and key trend lines. Inter for headline numbers, JetBrains Mono for data, labels and tables. No cards or shadows: just hairline dividers and whitespace.

## Ideas for improvement

- Annual data through recent years from Gapminder
- Shareable links that save the year, filters and pins in the URL
- Choice of metrics for each axis, such as fertility or child mortality
- An accessible data-table fallback and keyboard focus for the bubble chart

## Credits

- Data: [Gapminder](https://www.gapminder.org/), via Plotly's datasets repository
- Inspired by Hans Rosling's *The Joy of Stats* and Mike Bostock's *The Wealth & Health of Nations*
- CanvasJS is free for non-commercial use

Made by **Kartik Sahani** · [LinkedIn](https://www.linkedin.com/in/kartiksahani604900)
