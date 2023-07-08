# Ocean PCA

<iframe src="images/pca-e-full.html" width="100%" height="500px" frameBorder=0></iframe>

## Files

pca.ipynb - contains data cleaning code & 3d plot of principal components  
sample-rows.ipynb - samples 10k rows from the full dataset (https://www.kaggle.com/datasets/tunguz/big-five-personality-test)

## Preprocessing

Columns of Interest

<div markdown="1" style="
    display: block; 
    width: 100%; 
    overflow-x:auto
">

|     | EXT1 | EXT2 | EXT3 | EXT4 | EXT5 | EXT6 | EXT7 | EXT8 | EXT9 | EXT10 | EST1 | EST2 | EST3 | EST4 | EST5 | EST6 | EST7 | EST8 | EST9 | EST10 | AGR1 | AGR2 | AGR3 | AGR4 | AGR5 | AGR6 | AGR7 | AGR8 | AGR9 | AGR10 | CSN1 | CSN2 | CSN3 | CSN4 | CSN5 | CSN6 | CSN7 | CSN8 | CSN9 | CSN10 | OPN1 | OPN2 | OPN3 | OPN4 | OPN5 | OPN6 | OPN7 | OPN8 | OPN9 | OPN10 |
| --: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ----: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ----: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ----: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ----: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ----: |
|   0 |    1 |    5 |    1 |    5 |    1 |    1 |    1 |    5 |    1 |     5 |    4 |    2 |    4 |    2 |    4 |    4 |    4 |    4 |    4 |     4 |    4 |    4 |    2 |    1 |    4 |    2 |    5 |    2 |    1 |     2 |    2 |    4 |    4 |    3 |    1 |    4 |    4 |    4 |    2 |     4 |    4 |    2 |    4 |    2 |    5 |    1 |    5 |    4 |    4 |     4 |
|   1 |    5 |    2 |    4 |    2 |    0 |    2 |    5 |    2 |    5 |     4 |    5 |    5 |    5 |    5 |    2 |    4 |    2 |    2 |    2 |     1 |    2 |    4 |    1 |    4 |    2 |    2 |    1 |    4 |    2 |     3 |    1 |    4 |    4 |    1 |    1 |    1 |    4 |    3 |    4 |     2 |    3 |    1 |    3 |    1 |    4 |    4 |    4 |    2 |    4 |     2 |
|   2 |    4 |    1 |    5 |    2 |    5 |    2 |    4 |    4 |    4 |     2 |    2 |    5 |    2 |    0 |    1 |    2 |    4 |    1 |    2 |     3 |    1 |    5 |    1 |    5 |    1 |    5 |    1 |    5 |    5 |     5 |    1 |    5 |    2 |    4 |    4 |    1 |    2 |    2 |    0 |     2 |    4 |    5 |    5 |    4 |    4 |    1 |    2 |    2 |    4 |     4 |
|   3 |    1 |    3 |    3 |    4 |    4 |    3 |    2 |    5 |    1 |     5 |    5 |    3 |    5 |    1 |    1 |    4 |    2 |    2 |    5 |     5 |    2 |    3 |    3 |    4 |    2 |    3 |    2 |    3 |    3 |     3 |    2 |    5 |    4 |    5 |    1 |    3 |    4 |    4 |    2 |     3 |    4 |    3 |    4 |    2 |    3 |    2 |    3 |    3 |    3 |     4 |
|   4 |    2 |    1 |    4 |    2 |    4 |    2 |    4 |    3 |    2 |     4 |    3 |    3 |    4 |    2 |    4 |    4 |    4 |    3 |    3 |     3 |    1 |    5 |    1 |    4 |    1 |    4 |    1 |    4 |    5 |     4 |    4 |    4 |    4 |    2 |    2 |    4 |    2 |    2 |    2 |     2 |    4 |    1 |    5 |    1 |    5 |    1 |    5 |    2 |    4 |     4 |

</div>

Range of data should fall between 1-5. There are a number of 0 values present in the data. I'm assuming that these are also missing values.

Values are normalized to be between 1-5 through this mapping:  
1 -> -1  
2 -> -0.5  
3 -> 0  
4 -> 0.5  
5 -> 1

To account for polarity, questions with negative polarity have their corresponding columns multiplied by -1.

\_E columns are created through taking the the mean for a trait's 1-10 question responses.  
NOTE: NaN values are completely ignored when calculating mean. If a person only responded to 9 questions, the sum is divided by 9.

<div markdown="1" style="
    display: block; 
    width: 100%; 
    overflow-x:auto
">

|     |      EXT |   EST |   AGR |       CSN |  OPN |
| --: | -------: | ----: | ----: | --------: | ---: |
|   0 |     -0.8 |   0.5 | -0.45 |      -0.2 | 0.65 |
|   1 | 0.555556 | -0.25 |  0.35 |      0.05 |  0.2 |
|   2 |     0.55 |  -0.5 |     1 | -0.222222 | 0.15 |
|   3 |    -0.45 |  0.35 |   0.2 |     -0.35 | 0.25 |
|   4 |      0.2 |  0.25 |   0.8 |      -0.1 |  0.7 |

</div>

## Data Cleaning

There are still NaN values in the data. This means that some responses were missing data for every question in a trait section.

These rows will be dropped.

SUMMARY OF DATA

<div markdown="1" style="
    display: block; 
    width: 100%; 
    overflow-x:auto
">

|       |      EXT |       EST |      AGR |      CSN |      OPN |
| :---- | -------: | --------: | -------: | -------: | -------: |
| count |     9952 |      9952 |     9952 |     9952 |     9952 |
| mean  | -0.01278 |  0.029529 | 0.385407 | 0.181257 | 0.437619 |
| std   | 0.454289 |  0.429102 | 0.365382 |  0.36919 | 0.318982 |
| min   |       -1 |        -1 |       -1 |       -1 |    -0.95 |
| 25%   |    -0.35 | -0.277778 |     0.15 |     -0.1 |      0.2 |
| 50%   |        0 |      0.05 |     0.45 |      0.2 |     0.45 |
| 75%   |      0.3 |      0.35 |     0.65 |     0.45 |      0.7 |
| max   |        1 |         1 |        1 |        1 |        1 |

</div>

Since PCA aims to maximize variance explained, columns with higher variance will be prioritized.
Data should still be normalized.

Variance-Covariance Matrix After Normalization:

<div markdown="1" style="
    display: block; 
    width: 100%; 
    overflow-x:auto
">

| type |       EXT |        EST |        AGR |       CSN |       OPN |
| :--- | --------: | ---------: | ---------: | --------: | --------: |
| EXT  |         1 |  -0.215521 |   0.304984 | 0.0461086 |  0.149932 |
| EST  | -0.215521 |          1 | -0.0573839 | -0.220419 | -0.090065 |
| AGR  |  0.304984 | -0.0573839 |          1 |   0.15626 |  0.114062 |
| CSN  | 0.0461086 |  -0.220419 |    0.15626 |         1 | 0.0663948 |
| OPN  |  0.149932 |  -0.090065 |   0.114062 | 0.0663948 |         1 |

</div>

## PCA

"Hypothesis: The 5-dimensional OCEAN data set can be represented in 3 dimensions visualized by 27 clusters of points in a 3x3x3 cube."

9k+ datapoints

<iframe src="images/pca-e-full.html" width="100%" height="500px" frameBorder=0></iframe>

250 datapoints

<iframe src="images/pca-e-250.html" width="100%" height="500px" frameBorder=0></iframe>

Proportion of Variance Explained by Principal Components:

<div markdown="1" style="
    display: block; 
    width: 100%; 
    overflow-x:auto
">

|     | Proportion of Variance Explained | Cumulative Proportion |
| :-- | -------------------------------: | --------------------: |
| PC1 |                         0.317035 |              0.317035 |
| PC2 |                         0.206601 |              0.523636 |
| PC3 |                          0.18338 |              0.707016 |

</div>

Only 70% of variance explained. Notable information loss.

Principal Component Loadings

<div markdown="1" style="
    display: block; 
    width: 100%; 
    overflow-x:auto
">

|     |       EXT |      EST |       AGR |       CSN |       OPN |
| :-- | --------: | -------: | --------: | --------: | --------: |
| PC1 | -0.540406 | 0.444568 | -0.494664 | -0.381453 | -0.346586 |
| PC2 | -0.364322 | -0.51093 |  -0.36901 |  0.634714 |  -0.25921 |
| PC3 |    0.1468 | 0.160795 |  0.467782 |  0.140572 | -0.844994 |

</div>
