# Week 7 movie data

Derived from [GroupLens MovieLens latest-small](https://grouplens.org/datasets/movielens/latest/), downloaded 26 November 2026 from https://files.grouplens.org/datasets/movielens/ml-latest-small.zip.
The archive README identifies the snapshot as generated on 26 September 2018. Archive SHA-256: `696d65a3dfceac7c45750ad32df2c259311949efec81f0f144fdfb91ebc9e436`.

The original archive contains 9,742 movies and 100,836 ratings. The teaching table contains **1,297 movies**, one row per movie with at least 20 ratings. No artificial corruption or invented ratings were added.

Preparation: aggregate ratings by movieId into mean and count; join movie metadata; retain count >= 20; extract the trailing four-digit year; preserve the full pipe-separated genre combination as one categorical value; count genres; sort by movieId. Missing genre labels become empty CSV fields. No imputation or scaling is performed.

| Column | Meaning / role |
| --- | --- |
| movieId | Source identifier, for auditing only |
| title | Film label, for interpretation only |
| release_year | Numeric predictor extracted from title |
| genre_profile | Categorical predictor: full genre combination, not a single primary genre |
| genre_count | Numeric predictor: number of listed genres |
| rating_count | Number of source ratings; selection/audit field, excluded from predictors |
| mean_rating | Regression target: observed mean rating on the 0.5–5 scale |

This is a classroom movie-level regression task, not personalised recommendation or a prospective box-office/success forecast. Ratings were collected over time and are not representative of all viewers. Selecting movies with >=20 ratings changes the target population. A random movie split estimates performance on held-out movies from this selected snapshot; it does not establish performance on future releases. Full genre combinations are intentionally a simple, imperfect categorical representation; unseen combinations are possible.

Use and redistribution remain subject to `MOVIELENS_LICENSE.txt`, included in full. Educational/research use only under those conditions; do not imply GroupLens endorsement. Attribution: F. Maxwell Harper and Joseph A. Konstan. 2015. *The MovieLens Datasets: History and Context*. ACM TiiS 5(4), Article 19. https://doi.org/10.1145/2827872.
