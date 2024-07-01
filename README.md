# Pregnancy and postpartum dynamics revealed by millions of lab tests

https://doi.org/10.5061/dryad.1c59zw44t


The dataset contains summary statistics on lab tests from >300K pregnancies is Israel in the period 2003-2020 who are members of "Clalit Healthcare", Israel's largest HMO.\
It spans 110 different tests at weekly resolution.

Where BMI information was available, the measurements were grouped in three: 15-18.5, 18.5-25, 25-30.\
These test results are also included in the ungrouped dataset.

In addition, the dataset contains the same information for 3 common complications: Postpartum hemorrhage (PPH), gestational diabetes mellitus (GDM) and pre-eclampsia.\
The dataset of these pregnancies is brought at a 4-week resolution due to a lower number of measurements.

See _Methods_ for more details on dataset curation.


# Description of the data and file structure
Each individual test result was computed a quantile score from a reference of a healthy, non-pregnant same aged female population. We retained the participant's age and BMI information if available (before pregnancy).

The queried results were binned into weekly intervals starting 60 weeks before delivery until 80 weeks postpartum.\
For the BMI-grouped  dataset, the data is further binned (see above).\
For each such bin with properties (test_name, week [bmi_group]), summary statistics were computed including: mean, standard deviation and the (5,10,25,50,75,90, 95) percentiles.

The dataset consists of CSV files in the following structure:
```

 - /
    - Clalit Data/
        - pregnancy.1w/  # Results at 1w resolution, no BMI groups
            - 17_HYDROXY_PROGESTERONE.csv  # Summary statistics on 17α-OHP
            - ACTH_ADRENOCORTICOTROPIC_HORMONE.csv  # Summary statistics on ACTH
            - ⋮
        - pregnancy.2w/  # Results at 2w resolution, no BMI groups
            - ⋮
        - pregnancy.1w.bmi/  # Results at 1w resolution with BMI groups
            - ⋮
        - pregnancy.gdm.4w/  # Pregnancies with a gestational diabetes diagnosis, 4-week resolution
            - ⋮
        - pregnancy.postpartum_hemorrhage.4w/  # Pregnancies with a postpartum hemorrhage diagnosis, 4-week resolution
            - ⋮
        - pregnancy.pre-eclampsia.4w/  # Pregnancies with a pre-eclampsia diagnosis, 4-week resolution
            - ⋮
        - stats.csv  # Metadata on each lab test: number of pregnancies, first pregnancies, c-section fraction etc.
        - complications.csv  # Number of pregnancies per complication
        - non_healthy_icd_and_cohort.csv  # A list of all medical diagnosis precluded from the dataset
    - Metadata.xlsx  # Table of units and test names
    - LabNorm.csv  # Reference values from a healthy, non-pregnant female population.
```

Each CSV file has the following structure:
| week     | val_n | val_mean  | val_sd    | val_5       | val_10      | val_25      | val_50      | val_75     | val_90      | val_95      | qval_mean | qval_sd   | qval_5    | qval_10   | qval_25   | qval_50   | qval_75   | qval_90   | qval_95   | &#8230; |bmi_n       |&#8230;|
|----------|-------|-----------|-----------|-------------|-------------|-------------|-------------|------------|-------------|-------------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-|-------------|-|
| [-60,-56)| 4668  | 4.274635  | 0.356050  | 3.700000048 | 3.900000095 | 4.099999905 | 4.300000191 | 4.5        | 4.699999809 | 4.800000191 | 0.517748  | 0.300543  | 0.035977  | 0.081233  | 0.279392  | 0.554869  | 0.793191  | 0.924205  | 0.962102  |&#8230;|1376|&#8230;|

\*First row from ```pregnancy.4w/Albumin.csv```

Where:
* week:  Week of the test relative to delivery (=0).
* val_n: Number of lab tests in the weekly interval.
* val_mean: The mean of all tests values in the interval. Units are industry standard and be found in `Metadata.xlsx`
* val_sd: Standard deviation of the test values in the interval.
* val_(5&#183;&#183;&#183;95) - The (5&#183;&#183;&#183;95)th percentiles of the test values in the interval, the 50th percentile being the median.
* qval columns: Same as the above for the quantile score of the test results in the interval.
* bmi_n: Number of test results which had a valid BMI measurement.

Other columns (not shown) are age and BMI columns with the same summary statistics. 





# Sharing/Access information

Dataset and Juptyer notebooks for analysis are available on GitHub:\
[![GitHub](https://img.icons8.com/ios-glyphs/30/000000/github.png)Pregnancy and postpartum dynamics revealed by millions of lab tests](https://github.com/AlonLabWIS/PregnancyMillions)


Data was derived from the following sources:
- Clalit Healthcare
- [LabNorm R package](https://cran.r-project.org/package=labNorm)