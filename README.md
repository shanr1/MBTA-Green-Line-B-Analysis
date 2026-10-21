# MBTA Green Line B: Weather and Event Disruptions

**BA780 | Team A08**

An exploratory data analysis of how weather, public events, seasonality, and departure time relate to travel times and reliability on Boston's MBTA Green Line B Branch. The project compares **September and December 2025**, with particular attention to travel between **Boston College and Government Center**.

## Project overview

The Green Line B serves Boston University and surrounding neighborhoods through a combination of surface and underground service. This project investigates when travel takes longer, when it becomes less predictable, and whether weather and event activity help explain those patterns.

The notebook contains the full report, data preparation code, **15 hypothesis-driven questions and visualizations**, conclusions, recommendations, references, and a generative AI disclosure.

**Open the analysis:** [A08-Green-Line-B-Weather-and-Event-Disruptions.ipynb](A08-Green-Line-B-Weather-and-Event-Disruptions.ipynb)

## Main findings

The following results come from the notebook's saved outputs. Most end-to-end comparisons summarize **hourly average travel times**, rather than individual passenger journeys.

| Analysis | Result |
| --- | --- |
| Peak versus off-peak travel | Median end-to-end travel time was **47.8 vs. 46.3 minutes**. Peak periods were slower but had a lower standard deviation: **5.6 vs. 6.6 minutes**. |
| Baseline reliability | On days without precipitation or recorded events, the median standard deviation across surface-involving stop pairs was about **3.2 minutes**, compared with **1.1 minutes** for underground-only stop pairs. |
| Seasonal baseline | On baseline days, median end-to-end travel time was **47.8 minutes in September** and **43.1 minutes in December**. |
| Daily precipitation | The correlation between daily precipitation and average daily end-to-end travel time was weak: **r = 0.094**. |
| Extreme travel times | At a **52.47-minute** threshold, extreme observations represented **11.21%** of event-day records and **6.88%** of non-event-day records. |
| Event differences by month | The event-day versus non-event-day median gap was **-0.48 minutes in September** and **+1.65 minutes in December**, a **2.13-minute** difference between months. |

These are descriptive associations. They do not establish that weather or events caused the observed differences, and the notebook does not report formal statistical significance tests.

## Research questions

The 15 questions cover four areas:

- **Baseline performance (Q1-Q5):** peak periods, variability, travel direction, surface versus underground reliability, and seasonal differences.
- **Weather (Q6-Q9):** precipitation, daily rainfall totals, section-level differences, and minimum temperature.
- **Public events (Q10-Q13):** event-day travel times, extreme observations, station-level changes, and differences between September and December.
- **Combined conditions (Q14-Q15):** weather and events together, including comparisons across surface and underground sections.

## Data sources

| Source | Dataset | Use in this project |
| --- | --- | --- |
| [MBTA Open Data](https://mbta-massdot.opendata.arcgis.com/datasets/5f71a5c035fc4a4dad1b7fa73ba27ef8/about) | Light rail travel times | September and December 2025; filtered to `route_id == 'Green-B'`. |
| [NOAA National Centers for Environmental Information](https://www.ncei.noaa.gov/access/search/datasets/local-climatological-data-v2/) | Local Climatological Data v2, station `USW00014739` | Boston-area hourly precipitation, temperature, wind speed, and snow depth from the 2025 file. |
| [Analyze Boston](https://data.boston.gov/dataset/special-event-license-applications/resource/fb05bfda-a57e-4d97-948b-4f1949ba3c8a) | Special Event License Applications | Events starting or ending in 2025; daily event counts based on event start dates. |

The saved merge output contains **539,798 aggregated rows**. An initial end-to-end subset contains **1,873 hourly records**; individual questions apply additional filters, so sample sizes vary.

Raw CSV files are **not included** in this repository. The notebook currently reads them from the team's Google Cloud Storage bucket.

## Analytical workflow

1. Load the transit, weather, and event CSV files.
2. Filter transit observations to the Green Line B Branch.
3. Parse timestamps and aggregate travel times by date, hour, direction, and stop pair, retaining mean travel time and observation counts.
4. Clean weather values and aggregate readings by date and hour.
5. Filter the event dataset to 2025 and calculate daily event counts.
6. Merge transit and weather by date and hour, then attach event counts by date.
7. Select the relevant trips or sections for each question, calculate summary measures, and visualize the comparisons.

## View or run the notebook

### View the completed report

Open the notebook link above to view its saved text, tables, and charts on GitHub. Viewing the saved results does not require access to the raw data.

### Run the analysis

Use a Python 3 environment with Jupyter and the analysis libraries:

```bash
python -m pip install pandas numpy matplotlib seaborn gcsfs jupyterlab
python -m jupyter lab
```

Open `A08-Green-Line-B-Weather-and-Event-Disruptions.ipynb` and run the cells from top to bottom. Dependencies are not version-pinned in this project.

The data-loading cell expects these four files:

| Variable | Expected CSV file |
| --- | --- |
| `path_sep` | `2025-09_LRTravelTimes.csv` |
| `path_dec` | `2025-12_LRTravelTimes.csv` |
| `weather_path` | `LCD_USW00014739_2025.csv` |
| `event_path` | `fb05bfda-a57e-4d97-948b-4f1949ba3c8a (1).csv` |

To use the existing `gs://teama08/...` paths, your environment must have permission to read the team's bucket. Alternatively, obtain the corresponding source files, place them in a local `data` folder, and replace the four path variables, for example:

```python
path_sep = "data/2025-09_LRTravelTimes.csv"
path_dec = "data/2025-12_LRTravelTimes.csv"
weather_path = "data/LCD_USW00014739_2025.csv"
event_path = "data/fb05bfda-a57e-4d97-948b-4f1949ba3c8a (1).csv"
```

Reproducing the results requires the source data and a compatible Python environment; the saved results have not been rerun as part of preparing this README.

## Limitations

- The analysis covers two months, so season, ridership patterns, and weather can overlap.
- Missing weather values are filled with zero in parts of the cleaning process, which may affect comparisons.
- Event indicators use citywide permit counts by start date; they do not measure attendance or proximity to the B Branch.
- Some comparison groups are small or uneven, and filtering differs across questions.
- Hourly averages and aggregated stop-pair records should not be interpreted as individual rider experiences.
- The notebook includes operational interpretations and recommendations that require further evidence before implementation.

## Team

**BA780-A08:** Susan Huang, Ingrid Felix, Ruining Shan, Tia Theseira, and Kaiser Lu.

## Generative AI disclosure

As documented in the notebook, the team used Claude, Google Gemini, and ChatGPT/GPT to support research planning, data preparation, programming, visualization, interpretation, and writing. The notebook contains the full disclosure and the team's statement of responsibility for the submitted work.

## Data attribution

Dataset ownership and usage terms remain with the original providers. See the notebook's data-source table and references for attribution and licensing information.
