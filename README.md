# Pregnancy and postpartum dynamics revealed by millions of lab tests

<!--https://doi.org/10.5061/dryad.1c59zw44t-->
[![DOI](https://zenodo.org/badge/822709739.svg)](https://doi.org/10.5281/zenodo.13996864)



The dataset contains summary statistics on lab tests from >300K pregnancies is Israel in the period 2003-2020 who are members of "Clalit Healthcare", Israel's largest HMO.\
It spans 110 different tests at weekly resolution.

Where BMI information was available, the measurements were grouped in three: 15-18.5, 18.5-25, 25-30.\
These test results are also included in the ungrouped dataset.

In addition, the dataset contains the same information for 3 common complications: Postpartum hemorrhage (PPH), gestational diabetes mellitus (GDM) and pre-eclampsia.\
The dataset of these pregnancies is brought at a 4-week resolution due to a lower number of measurements.

See [_Methods_](#methods) for more details on dataset curation.


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
        - excluded_icd9_codes.csv  # ICD9 codes of all medical diagnoses exlcuded, "Chronic disease" as discussed in the methods.
        - Metadata.xlsx  # Table of units and test names
        - LabNorm.csv  # Reference values from a healthy, non-pregnant female population.
```

Each CSV file has the following structure:
| week     | val_n | val_mean  | val_sd    | val_5       | val_10      | val_25      | val_50      | val_75     | val_90      | val_95      | qval_mean | qval_sd   | qval_5    | qval_10   | qval_25   | qval_50   | qval_75   | qval_90   | qval_95   | &#8230; |bmi_n       |&#8230;|
|----------|-------|-----------|-----------|-------------|-------------|-------------|-------------|------------|-------------|-------------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-|-------------|-|
| \[-60,-56\)| 4668  | 4.274635  | 0.356050  | 3.700000048 | 3.900000095 | 4.099999905 | 4.300000191 | 4.5        | 4.699999809 | 4.800000191 | 0.517748  | 0.300543  | 0.035977  | 0.081233  | 0.279392  | 0.554869  | 0.793191  | 0.924205  | 0.962102  |&#8230;|1376|&#8230;|

\*First row from ```pregnancy.4w/Albumin.csv```

Where:
* week:  Week of the test relative to delivery (=0).
* val_n: Number of lab tests in the weekly interval.
* val_mean: The mean of all tests values in the interval. Units are industry standard and be found in `Metadata.xlsx`
* val_sd: Standard deviation of the test values in the interval.
* val_(5&#183;&#183;&#183;95) - The (5&#183;&#183;&#183;95)<sup>th</sup> percentiles of the test values in the interval, the 50<sup>th</sup> percentile being the median.
* qval columns: Same as the above for the quantile score of the test results in the interval.
* bmi_n: Number of test results which had a valid BMI measurement.

Other columns (not shown) are age and BMI columns with the same summary statistics. 

The file `stats.csv` provides some metadata statistics about the dataset:

* `gw_X` column names are the `X`<sup>th</sup> percentile of delivery time, unit is gestational week.
* Fraction of first pregnancies was calculated based on the records after 2010 only, since pregnancies before 2002 are not included and therefore we cannot know about earlier pregnancies.
* Preterm pregnancies are those with deliver at or before the 37<sup>th</sup> gestational week.

# Methods
## Study Population

The study population consisted of individuals from the Clalit healthcare database, Israel's largest health maintenance organization (HMO). We considered all pregnancies of females aged 20 to 35 between 2003 and 2020. Information about pregnancies before 2003 is not available. We estimated the fraction of first pregnancies for the years 2010-2020 to reduce the influence of first pregnancies before 2003 which we cannot account for. For more information, see “stats.csv”.

## Data Collection

Medical records were pseudonymized by hashing of personal identifiers and randomization of dates by a random number of weeks uniformly sampled between 0 and 13 weeks for each patient and adding it to all dates in the patient diagnoses, laboratory, and medication records. This randomization does not affect timing relative to delivery.

We examined the timeframe of 60 weeks before delivery to 80 weeks after delivery for all documented labors within our study population. 0 is denoted as week of delivery. We identified deliveries by ICD9 code V27 and confirmed a childbirth record for the individual. We excluded preterm deliveries (≤37 weeks, ICD9 code 644) stillbirths and labors with more than one newborn. Nonetheless, 12% of deliveries were found to be ≤37 weeks and missing the 644 code.

To mitigate ascertainment bias of the test results, for each test, we removed data from individuals with chronic disease that affects the test if the onset of the disease was up to 6 months after the test. We also removed data from individuals who purchased drugs which affected the tests in the 6 months before the tests. Chronic diseases are defined as non-pediatric ICD9 codes with a Kaplan−Meyer survival drop of >10% over 5 years and are assigned above a minimal average drop of 1/3 per y. Drugs that affect a test were defined as drugs with significant effect on the test (false discovery rate < 0.01). This step allowed us to focus on a relatively healthy subset of the pregnant population, reducing the confounding effects associated with specific health conditions listed above or medication usage. 

 

To exclude the potential effect of follow-up pregnancies in the 80 weeks following delivery, we excluded lab values from individuals with another delivery within 40 weeks following the measurement.

For each pregnancy, we gathered all available test values including standard blood count, kidney and liver function tests, blood coagulation tests, lipid panel, inflammation markers and hormones. We then discretized test values into time points relative to the time of birth in weekly intervals for each test. In addition to test values, we also extracted data on patients including age (at measurement, mean and interquartile range) and BMI (the most proximal BMI measurement in medical records outside pregnancy, mean and interquartile range, if available).


# Running the notebooks

Jupyter notebook was used to run the analyses and to create the graphs. To re-run, [install anaconda or miniconda](https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html) and download the repository.
Open the command prompt/terminal in the `src` directory of the repository and install the envinment:
```
conda env create -f environment.yml
```
Start the conda environment and the jupyter server:
```
conda activate pregnancy
jupyter notebook
```
And run the notebooks from the new browser window. For support regarding running Jupyter notebooks, please refer to [Jupyter's support page](https://docs.jupyter.org/en/latest/running.html).



# Sharing/Access information


Data was derived from the following sources:
- Clalit Healthcare
- [LabNorm R package](https://cran.r-project.org/package=labNorm)