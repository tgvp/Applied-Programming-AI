# Week 6 — Data Cleaning and Exploratory Data Analysis

## Summary

1. Inspect and clean a messy dataset.
2. Calculate descriptive statistics and compare groups.
3. Produce a histogram, boxplot and scatter plot.
4. Explain findings and export a cleaned CSV.

Each notebook lists all 18 required questions at the beginning, provides free-form answer cells and ends with a generic cheatsheet. Choose and justify your methods using the tools from earlier classes. Use Pandas and Matplotlib methods from Weeks 1–5. The focus is cleaning and descriptive analysis; model training, scaling and hypothesis testing are outside this activity.

## Activities

| Dataset | Student notebook | Messy CSV |
|---|---|---|
| Student Performance | [Open notebook](mia-pr-week-6-student-performance-student.ipynb) | [Download CSV](../raw/messy_student_performance.csv) |
| Bike Sharing | [Open notebook](mia-pr-week-6-bike-sharing-student.ipynb) | [Download CSV](../raw/messy_bike_sharing.csv) |
| Auto MPG | [Open notebook](mia-pr-week-6-auto-mpg-student.ipynb) | [Download CSV](../raw/messy_auto_mpg.csv) |
| Palmer Penguins | [Open notebook](mia-pr-week-6-penguins-student.ipynb) | [Download CSV](../raw/messy_penguins.csv) |

## Running an activity

Open a notebook from Week-6 or the repository root. The loader checks `raw/`, `../raw/` and the current folder, following the Week 4 pattern. In Colab, upload the matching CSV first. Write your own code and explanations for the questions in your selected notebook. Use the final cheatsheet as a reference when needed. Save the completed notebook, cleaned CSV and three PNG plots. Outputs go to an `outputs` folder in the current working directory. Keep the messy source CSV unchanged.

## What makes the files messy?

Each file contains a saved CSV index, a trailing space in one column name, inconsistent category case/spacing, a few numeric values with unit suffixes, missing values or `?`, exact duplicated records and a small number of impossible values. The added problems affect a minority of records. Natural missing values and plausible extremes from the sources are retained. A teaching `record_id` is assigned before duplication, so distinct source observations are not mistaken for duplicates.

These are artificial teaching changes. They are not statements about the original datasets. They can be handled with the string operations, conversions, filtering and missing-value methods covered in previous weeks.

## Sources, attribution and preparation

### Student Performance

[Original dataset](https://archive.ics.uci.edu/dataset/320/student+performance)

Cortez, P. (2008). Student Performance. UCI. https://doi.org/10.24432/C5TG7T (CC BY 4.0).

Selected nine columns from student-mat.csv (Mathematics only, 395 records). Renamed grade columns and converted study-time codes to descriptive categories. Portuguese-language records were not combined with Mathematics.

### Bike Sharing

[Original dataset](https://archive.ics.uci.edu/dataset/275/bike+sharing+dataset)

Fanaee-T, H. (2013). Bike Sharing. UCI. https://doi.org/10.24432/C5W894 (CC BY 4.0). Original study: Fanaee-T and Gama, Event labeling combining ensemble detectors and background knowledge, doi:10.1007/s13748-013-0040-3.

Selected the 731 daily records from day.csv, not hour.csv. Converted weather and working-day codes to labels. Temperature is temp × 41 and humidity is hum × 100, following the README packaged with the daily data. These conversions were done before the exercise; no scaling is required.

### Auto MPG

[Original dataset](https://archive.ics.uci.edu/dataset/9/auto+mpg)

Quinlan, R. (1993). Auto MPG. UCI. https://doi.org/10.24432/C5859H (CC BY 4.0).

Selected seven columns from auto-mpg.data (398 rows). Origin codes became usa/europe/japan; model years became four-digit years. Original missing horsepower values were preserved. Displacement and acceleration were omitted to keep the exercise focused.

### Palmer Penguins

[Original dataset](https://allisonhorst.github.io/palmerpenguins/)

Horst, A. M., Hill, A. P., and Gorman, K. B. (2020). palmerpenguins. https://doi.org/10.5281/zenodo.3960218. Data: Gorman, Williams and Fraser (2014), doi:10.1371/journal.pone.0090081, Palmer Station LTER. Dataset distributed under CC0.

Used the simplified penguins.csv from the official palmerpenguins repository (344 records); omitted year. Kept the original missing measurements and sex values.

UCI datasets are adapted under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Palmer Penguins data are distributed under [CC0](https://creativecommons.org/publicdomain/zero/1.0/). All messy CSVs are modified teaching versions; the alterations are described above.
