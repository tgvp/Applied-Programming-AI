# Course Schedule and Assessments

This course develops practical skills in data preparation, machine learning pipelines, model selection, evaluation, and reproducible experimentation with Python and scikit-learn.

The sequence progresses from numerical computation and exploratory data analysis to complete machine learning workflows. Particular attention is given to preventing data leakage, selecting models through cross-validation, and evaluating predictions on held-out data. Deep learning and PyTorch are deferred to the advanced course in the following semester.

## Course schedule

The Week 1–6 labels below identify the published teaching materials. The remaining sessions and deadlines are listed by calendar date.

| Materials / Date | Main topic | Practical milestone |
| --- | --- | --- |
| Week 1 | Python environment, NumPy, and vectorised computation | Python and NumPy foundations |
| Week 2 | Pandas: DataFrames, selection, transformation, and data types | Explore a real dataset |
| Week 3 | Data preparation: data types, missing values, categorical data, and features | Clean and validate data |
| Week 4 | Exploratory data analysis: distributions, group comparisons, and relationships | Interpret data using descriptive statistics and visual evidence |
| Week 5 | Scikit-learn preprocessing pipelines: numerical and categorical transformations | Construct a reusable preprocessing pipeline |
| Week 6 | Data cleaning and exploratory analysis of a selected dataset | Work in pairs on Student Performance, Bike Sharing, Auto MPG, or Palmer Penguins |
| 27 Nov 2026 | Mini-project 2 presentations; train/test splits, baselines, metrics, leakage prevention with pipelines, cross-validation, and hyperparameter selection with `GridSearchCV` | Present Mini-project 2 · Compare pipeline configurations · Mini-project 3 released |
| 4 Dec 2026 | Data validation: required columns, data types, missing values, valid ranges, categories, duplicates, and consistency checks | Build a data-quality report · Support work on Mini-project 3 · Define input validation rules for the final project |
| 11 Dec 2026 | Model evaluation; reproducible execution, saving/loading pipelines, and validation of inference inputs | Mini-project 3 due · Discuss modelling results · Prepare a reproducible workflow for the final project |
| 18 Dec 2026 | Final theoretical test and final project briefing | Final test · Scheduled release of Mini-project 4 — Final Project |
| 8 Jan 2027 | Final project submission deadline | Mini-project 4 — Final Project due |

Data validation checks whether input data satisfy defined quality rules; model validation estimates predictive performance on held-out observations. The data-validation material introduced on 4 December prepares the final project and does not add new requirements to the released Mini-project 3 brief.

The January entry is a submission deadline, not a scheduled teaching session. Any presentation arrangements will be announced separately.

## Assessment

The final grade is expressed on the Portuguese 0–20 scale. There are two quizzes, one final theoretical test, and four projects. **Mini-project 4 is the final project**, not an additional assessment.

| Component | Maximum contribution (out of 20) | Percentage | Release / submission |
| --- | ---: | ---: | --- |
| Participation | 1.0 | 5% | Continuous |
| Quiz 1 | 1.0 | 5% | Completed |
| Quiz 2 | 1.0 | 5% | Completed |
| Final theoretical test | 5.0 | 25% | 18 Dec 2026 |
| Mini-project 1 | 1.0 | 5% | Completed |
| Mini-project 2 | 1.5 | 7.5% | As previously announced |
| Mini-project 3 — Model Selection and Evaluation | 2.0 | 10% | Released 27 Nov 2026 · Due 11 Dec 2026 |
| Mini-project 4 — Final Project | 7.5 | 37.5% | Release scheduled for 18 Dec 2026 · Due 8 Jan 2027 |
| **Total** | **20.0** | **100%** | |

### Assessment progression

- Mini-project 1 assesses data preparation and transformation.
- Mini-project 2 retains the objectives and requirements in its previously announced brief.
- Mini-project 3 extends the dataset selected in Week 6 into a scikit-learn modelling experiment: define a prediction task, construct a pipeline, establish a baseline, select hyperparameters using cross-validation, and evaluate on held-out data. Use classification or regression and appropriate metrics for the selected target. Keep learned preprocessing inside the pipeline and choose a split strategy appropriate to the task.
- Mini-project 4 is the final integrative project. Starting from a new messy dataset supplied by the lecturer, complete the workflow from diagnosis, cleaning, and exploration through modelling, model selection, evaluation, and reproducible inference. Include executable code and instructions explaining how to reproduce the solution. PyTorch and deep learning are not required.
- The final theoretical test assesses the taught concepts, including interpretation of code and results, data splitting, leakage prevention, metrics, cross-validation, and model selection. The session on 11 December includes consolidation and revision.

### Grade calculation

Each term below is the component's contribution on the 0–20 final-grade scale, capped at the value listed in the assessment table.

```text
Final grade = Participation
            + Quiz 1 + Quiz 2
            + Final Theoretical Test
            + Mini-project 1 + Mini-project 2
            + Mini-project 3
            + Mini-project 4 (Final Project)

Maximum = 1.0 + 1.0 + 1.0 + 5.0 + 1.0 + 1.5 + 2.0 + 7.5 = 20.0
```
