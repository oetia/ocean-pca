# Ocean PCA

## Files

pca.ipynb - contains data cleaning code & 3d plot of principal components  
sample-rows.ipynb - samples 10k rows from the full dataset (https://www.kaggle.com/datasets/tunguz/big-five-personality-test)

## Data Cleaning

The five dimensions of the data set are described by the columns:

1. EXT, EXT_E
2. EST, EST_E
3. AGR, AGR_E
4. CSN, CSN_E
5. OPN, OPN_E

Referencing the data dictionary provided (data/codebook.txt):  
"The time spent on each question is also recorded in milliseconds. These are the variables ending in \_E. This was calculated by taking the time when the button for the question was clicked minus the time of the most recent other button click."

NOTE: There's a lot more than 5 columns:

<div markdown="1" style="
    display: block; 
    width: 100%; 
    overflow-x:auto
">

|     | EXT1_E | EXT2_E | EXT3_E | EXT4_E | EXT5_E | EXT6_E | EXT7_E | EXT8_E | EXT9_E | EXT10_E | EST1_E | EST2_E | EST3_E | EST4_E | EST5_E | EST6_E | EST7_E | EST8_E | EST9_E | EST10_E | AGR1_E | AGR2_E | AGR3_E | AGR4_E | AGR5_E | AGR6_E | AGR7_E | AGR8_E | AGR9_E | AGR10_E | CSN1_E | CSN2_E | CSN3_E | CSN4_E | CSN5_E | CSN6_E | CSN7_E | CSN8_E | CSN9_E | CSN10_E | OPN1_E | OPN2_E | OPN3_E | OPN4_E | OPN5_E | OPN6_E | OPN7_E | OPN8_E | OPN9_E | OPN10_E |
| --: | -----: | -----: | -----: | -----: | -----: | -----: | -----: | -----: | -----: | ------: | -----: | -----: | -----: | -----: | -----: | -----: | -----: | -----: | -----: | ------: | -----: | -----: | -----: | -----: | -----: | -----: | -----: | -----: | -----: | ------: | -----: | -----: | -----: | -----: | -----: | -----: | -----: | -----: | -----: | ------: | -----: | -----: | -----: | -----: | -----: | -----: | -----: | -----: | -----: | ------: |
|   0 |   3504 |    777 |    792 |   4037 |   1550 |   3172 |    718 |    751 |    929 |    1576 |   1853 |   2438 |    918 |   1432 |   3773 |   1943 |   1192 |    784 |   1132 |    1269 |   1457 |   1808 |   1940 |   1607 |   4445 |  18342 |   1296 |   1341 |   1928 |    2843 |    839 |  40453 |    999 |   3050 |   2377 |   1312 |   2884 |    977 |    991 |    1024 |    879 |   1896 |   2151 |   3589 |   1040 |   1838 |   1832 |   1635 |   1449 |    4236 |
|   1 |   7369 |   9328 |  32352 |   2440 |      0 |   2793 |   2952 |   6757 |   5376 |    2081 |   3130 |   2432 |   2759 |   2681 |   9033 |   2253 |   2583 |   2800 |   3056 |    2688 |   4598 |   1985 |   3320 |   1769 |   5095 |   4263 |   2832 |   4650 |   2840 |    6135 |   2433 |  11713 |   1952 |   4116 |   8713 |   4706 |   2712 |   1558 |   1301 |    3312 |  10551 |   3032 |   2188 |   3320 |   4817 |   8166 |   3644 |   4408 |   2570 |    1282 |
|   2 |   5743 |   2483 |   3179 |   1928 |   2386 |   4312 |  37930 |   5480 |   3312 |   11932 |   2809 |   3813 |  13646 |      0 |  15644 |   2676 |   1912 |  18584 |  12081 |    1986 |   1340 |   1557 |   3060 |  11422 |   1715 |   2189 |   1233 |   2328 |    977 |   50345 |   2057 |  10867 |   1400 |   1879 |   1575 |   2452 |   1692 |  19053 |      0 |    2968 |   1866 |   1665 |   3831 |   2367 |   1979 |   1782 |   4436 |   1963 |   2788 |    2051 |
|   3 |   2615 |   6207 |   4718 |   9277 |   4268 |   9874 |   5894 |   3154 |   2754 |    5625 |   8257 |   3382 |   5822 |   4442 |   4076 |   2926 |   5760 |   2297 |   2098 |    1634 |   4905 |   2982 |   9363 |   2516 |   3424 |  14042 |   3368 |   3818 |   7256 |    2721 |   7107 |   4277 |   7453 |   2594 |   3134 |   4094 |   4124 |   3320 |   4600 |    3518 |   6517 |   3408 |   4282 |   2696 |   5798 |   5680 |   3068 |   2467 |   2626 |    2012 |
|   4 |  11476 |   3147 |   3962 |   6104 |   3065 |   6817 |   3354 |   2286 |   5751 |    3625 |   2626 |   2425 |   2551 |   2457 |   3908 |   3169 |   1978 |   4317 |   4046 |    8610 |   5178 |   4551 |   3530 |   5492 |   7056 |   4090 |   3303 |   5084 |   3331 |    2700 |   5341 |   1299 |   3175 |   4061 |   5031 |   4757 |   2654 |   3521 |   4262 |    2985 |   3547 |   3545 |   2431 |   3839 |   9352 |   5067 |   2448 |   2119 |   1825 |    1643 |

</div>

I'm not sure how I'm supposed to deal with this, so I just created a new \_E column for each trait with an individual's average time for said trait.

## Data Used

HEAD OF CLEANED DATAFRAME

<div markdown="1" style="
    display: block; 
    width: 100%; 
    overflow-x:auto
">

|     |  EXT_E |  EST_E |  AGR_E |  CSN_E |  OPN_E |
| --: | -----: | -----: | -----: | -----: | -----: |
|   0 | 1780.6 | 1673.4 | 3700.7 | 5490.6 | 2054.5 |
|   1 | 7144.8 | 3341.5 | 3748.7 | 4251.6 | 4397.8 |
|   2 | 7868.5 | 7315.1 | 7616.6 | 4394.3 | 2472.8 |
|   3 | 5438.6 | 4069.4 | 5439.5 | 4422.1 | 3855.4 |
|   4 | 4958.7 | 3608.7 | 4431.5 | 3708.6 | 3581.6 |

</div>

SOME SUMMARY STATS  
Note: Outliers gave heavy right skew (some people took on avg 7 hours to respond to each question). Table below describes data after dropping entries above 3 SD. Not sure if '0' should be considered for missingness.

<div markdown="1" style="
    display: block; 
    width: 100%; 
    overflow-x:auto
">

|       |   EXT_E |   EST_E |   AGR_E |   CSN_E |   OPN_E |
| :---- | ------: | ------: | ------: | ------: | ------: |
| count |    9923 |    9923 |    9923 |    9923 |    9923 |
| mean  |  7855.5 | 5226.46 | 5638.68 | 6031.77 | 4809.95 |
| std   | 28778.2 | 10946.5 | 12456.3 | 10317.7 | 4612.46 |
| min   |       0 |       0 |       0 |       0 |       0 |
| 25%   |  3480.1 | 2819.75 |  3135.9 |  3239.5 |  2827.7 |
| 50%   |  4574.3 |  3725.9 |  4112.6 |  4330.1 |  3754.4 |
| 75%   | 6423.85 |  5254.9 | 5698.55 | 6150.85 |  5203.7 |
| max   |  906174 |  412451 |  507171 |  294300 | 88025.8 |

</div>

## PCA

"Hypothesis: The 5-dimensional OCEAN data set can be represented in 3 dimensions visualized by 27 clusters of points in a 3x3x3 cube."

¯\\\_(ツ)\_/¯

<iframe src="images/pca.html" width="100%" height="500px" frameBorder=0></iframe>

Proportion of Variance Explained by Principal Components:

<div markdown="1" style="
    display: block; 
    width: 100%; 
    overflow-x:auto
">

| Principal Component | Proportion of Variance Explained |
| :------------------ | -------------------------------: |
| PC1                 |                       0.67373785 |
| PC2                 |                       0.13383938 |
| PC3                 |                       0.10128083 |
| Total               |                       0.90885803 |

</div>

Principal Component Loadings

<div markdown="1" style="
    display: block; 
    width: 100%; 
    overflow-x:auto
">

| Principal Component |       EXT_E |      EST_E | AGR_E       | CSN_E      | OPN_E      |
| :------------------ | ----------: | ---------: | ----------- | ---------- | ---------- |
| PC1                 |  0.99918581 | 0.01746287 | 0.01771672  | 0.02861412 | 0.01378835 |
| PC2                 | -0.03205508 | 0.35582254 | 0.87481955  | 0.30975886 | 0.10537073 |
| PC3                 |  -0.0189014 | 0.74401244 | -0.47647344 | 0.46040521 | 0.08419229 |

</div>
