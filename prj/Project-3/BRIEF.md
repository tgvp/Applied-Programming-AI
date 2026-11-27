# Mini-project 3 — Model Selection and Evaluation

**Applied Programming for AI | Contribution: 2.0/20 (10%)**

**Released:** 27 November 2026  
**Submission deadline:** 11 December 2026  
**Working arrangement:** continue with your Week 6 pair and the same selected dataset.

The exact submission time and platform will be announced by the lecturer. See the [course schedule](../../rsc/Schedule.md).

## From exploration to prediction

In [Week 6](../../Week-6/BRIEF.md), you cleaned and explored a dataset. Now use it to build a prediction model with scikit-learn.

Your goal is to answer:

> Can a k-nearest neighbours model predict the target better than a simple baseline, when evaluated fairly on unseen observations?

Follow this sequence:

**Define the task → split the data → build a pipeline → compare configurations with cross-validation → evaluate on the reserved test.**

Use the [Week 7 solution](../../Week-7/mia-pr-week-7-solution.ipynb) as a reference for the procedure. Adapt it to your dataset; do not submit the movie example.

## 1. Use your Week 6 dataset

| Dataset | Source file | Prediction task | Model | Main metric |
| --- | --- | --- | --- | --- |
| Student Performance | [CSV](../../raw/messy_student_performance.csv) | Predict `final_grade` | `KNeighborsRegressor` | MAE |
| Bike Sharing | [CSV](../../raw/messy_bike_sharing.csv) | Predict `rentals` for later dates | `KNeighborsRegressor` | MAE |
| Auto MPG | [CSV](../../raw/messy_auto_mpg.csv) | Predict `mpg` for similar vehicles | `KNeighborsRegressor` | MAE |
| Palmer Penguins | [CSV](../../raw/messy_penguins.csv) | Predict `species` from physical measurements | `KNeighborsClassifier` | Macro-F1 |

Clean column names before using them: some supplied headers contain extra spaces. Keep the raw file unchanged and make your preparation reproducible in the notebook.

### Choose inputs that make sense

- **Student Performance:** define when the prediction is made. For a prediction at the start of the course, exclude `grade_1` and `grade_2`. If predicting after the second assessment, you may include them, but state that this is a different prediction task. Use only other information available at the chosen moment; for example, total course absences are not known at the start.
- **Bike Sharing:** exclude `casual` and `registered`, because they are components of the target `rentals`. Use date/calendar features and justify weather inputs. If using observed weather, describe the task as estimating rentals conditional on that weather, not as a forecast with information unavailable in advance.
- **Auto MPG:** use vehicle characteristics such as cylinders, horsepower, weight and origin. Exclude `record_id`, exported index columns and `car_name` from predictors for this exercise.
- **Penguins:** use bill length, bill depth, flipper length and body mass. Do not include `species`, or any encoding of it, in the input features.

Exclude record IDs and exported index columns from predictors in all datasets. Keep identifiers separately if useful for checking records or explaining errors.

## 2. Review your cleaning

Reuse the code, rules and understanding developed in Week 6. You do not need to repeat all 18 exploration questions or all earlier plots.

**Using scikit-learn pipelines is required.** If your previous work used manual preprocessing steps in Pandas or NumPy, adapt that code into a scikit-learn workflow rather than simply passing an already imputed or scaled table to the model.

- Keep useful loading, inspection and fixed cleaning rules from your previous notebook.
- Move learned preprocessing, such as imputation and scaling, into `Pipeline` steps. Use `ColumnTransformer` when numeric and categorical inputs need different transformations.
- Add the estimator as the final step of the complete pipeline, and pass that pipeline to cross-validation and `GridSearchCV`.
- If you already used pipelines, review and extend them: confirm that the target is excluded and that preprocessing is fitted only inside the appropriate training partitions.

Briefly identify what you reused, what you changed, and why. You do not need to force every Pandas operation into a pipeline; the required change is to integrate learned preprocessing and modelling into the evaluated scikit-learn pipeline.

Distinguish two types of preparation:

| Preparation | Where it happens |
| --- | --- |
| Fixed rules: strip spaces, standardise known labels, convert numeric strings, identify invalid values | May be applied before splitting |
| Learned rules: calculate medians, most-frequent categories, means and scaling values | Inside the pipeline, fitted only on the training rows of each fold |

Do not start from a table already imputed or scaled using all observations. Recreate those learned steps inside the pipeline.

Investigate duplicates before splitting. Remove confirmed duplicate observations, not different observations that merely share some values. If repeated IDs have conflicting values, document how you resolve or exclude them. The same underlying observation must not appear in both training and evaluation.

Rows with missing or invalid targets cannot be used for supervised training or scoring. Exclude and count them; do not impute the target. For Bike Sharing, also resolve or exclude unusable dates before the chronological split.

## 3. Reserve the final test

- **Student Performance and Auto MPG:** reserve 20% with `train_test_split(..., test_size=0.20, random_state=42)` for the stated same-population task.
- **Penguins:** use the same split with `stratify=y` to preserve class proportions.
- **Bike Sharing:** sort by date and reserve the latest 20% of observations. Do not shuffle. Use the earlier observations for training and model selection.

Report the number of training and test rows. Keep the final test out of preprocessing fitting and model selection.

You do not need a separate `X_valid`: cross-validation creates the internal validation partitions.

## 4. Build a baseline and a pipeline

Use `DummyRegressor(strategy="median")` for regression, or `DummyClassifier(strategy="most_frequent")` for classification.

Build one complete k-NN pipeline containing the required preprocessing and the estimator:

- Numeric inputs: impute missing values and scale them.
- Categorical inputs, when used: impute missing values and encode them with `OneHotEncoder(handle_unknown="ignore")`.
- Combine numeric and categorical branches with `ColumnTransformer` when needed.
- Finish with the appropriate k-NN estimator, in a step named `model`.

Pass this **entire pipeline** to cross-validation and GridSearchCV. Do not call `fit_transform` on the whole training table before cross-validation.

## 5. Select k with GridSearchCV

Use a small search:

```python
param_grid = {"model__n_neighbors": [3, 5, 9, 15]}
```

Keep other model settings fixed. Comparing additional model families is not required.

| Task | Cross-validation within training | Search scoring |
| --- | --- | --- |
| Student Performance / Auto MPG | `KFold(n_splits=5, shuffle=True, random_state=42)` | `"neg_mean_absolute_error"` |
| Penguins | `StratifiedKFold(n_splits=5, shuffle=True, random_state=42)` | `"f1_macro"` |
| Bike Sharing | `TimeSeriesSplit(n_splits=4)` on date-sorted training rows | `"neg_mean_absolute_error"` |

Check that every fitting fold has enough rows for the largest k. For Penguins, check that each class has enough training examples for the chosen number of folds. If cleaning makes these settings unsuitable, explain the necessary adjustment.

Evaluate the baseline using the same training data, CV splits and metric. Present a table with k and its mean validation score; include a simple comparison plot. For MAE, convert negative scores back to positive errors when reporting: lower is better. For macro-F1, higher is better.

Record the selected k and whether the baseline or k-NN wins the CV comparison **before** examining final test results. GridSearchCV refits its selected pipeline on all training data by default.

## 6. Evaluate and explain

Fit the baseline on all training rows. Evaluate the baseline and selected k-NN configuration on the reserved test, keeping the model choice already made using CV.

- **Regression:** report test MAE in the target's units and show a few actual/predicted values with large errors.
- **Classification:** report test macro-F1 and a confusion matrix. Describe which species are confused.

Compare CV and test results. Explain at least one error and two limitations. Do not change k, features or the split in response to test performance. A model that does not beat the baseline can still earn full credit for a correct and well-explained experiment.

## Deliverable

Submit **one completed notebook**, named `project_3_<dataset>.ipynb`, with both students' names and student numbers. Include:

1. Task, target and input choices.
2. Reproducible data loading and necessary cleaning, with row counts and decisions.
3. Train/test split and justification.
4. Baseline, complete scikit-learn pipeline and GridSearchCV code, with a short explanation of how you adapted your previous implementation.
5. CV results table and comparison plot.
6. Final test results and error analysis.
7. A short conclusion and instructions for running the notebook.

Run the notebook from a clean kernel and save the relevant outputs. Use the supplied source CSV with a clear relative-path or upload instruction. Keep explanations beside the code; a separate report, web application, PyTorch model or saved-model deployment is not required.

Both students should understand the complete procedure and be able to explain their choices. Submission platform, exact deadline time and any presentation arrangements will be announced separately.

## Assessment rubric

| Criterion | Weight | Maximum contribution |
| --- | ---: | ---: |
| Clear prediction task, input choices and appropriate cleaning | 20% | 0.4/20 |
| Correct split and prevention of data leakage | 25% | 0.5/20 |
| Baseline, complete pipeline and appropriate CV/GridSearchCV | 30% | 0.6/20 |
| Interpretation of results, errors and limitations | 15% | 0.3/20 |
| Reproducible execution and clear communication | 10% | 0.2/20 |
| **Total** | **100%** | **2.0/20** |

The quality of the experimental procedure and your explanation matter more than obtaining the highest score.

## Before submitting

- Can you explain what one row represents and when each input is available?
- Are the target and inappropriate identifiers excluded from X?
- Are medians and scaling values fitted inside each training fold?
- Does the split respect time or related observations where relevant?
- Was k selected without using the final test?
- Are baseline and k-NN compared using the same protocol?
- Can another person run the notebook with the supplied CSV?

No `assert` statements are required; clear tables, counts and explanations are sufficient for the checks requested here.
