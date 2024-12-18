# Outages
Final Project for DSC 80
## Introduction
### Question

What effect does the cause of an outage have?

### Data

We will be exploring this question through the use of the Outages dataset provided by Purdue University in which a raw preview is provided below.

|   YEAR |   MONTH | CAUSE.CATEGORY     | CLIMATE.REGION     | CLIMATE.CATEGORY   | OUTAGE.DURATION   | OUTAGE.START.DATE                | OUTAGE.START.TIME            | OUTAGE.RESTORATION.DATE          | OUTAGE.RESTORATION.TIME      | DEMAND.LOSS.MW   |   TOTAL.CUSTOMERS | POPPCT_URBAN   | POPDEN_URBAN            | PCT_LAND         | PC.REALGSP.STATE   | CAUSE.CATEGORY.DETAIL   |   COM.CUSTOMERS |
|-------:|--------:|:-------------------|:-------------------|:-------------------|:------------------|:---------------------------------|:-----------------------------|:---------------------------------|:-----------------------------|:-----------------|------------------:|:---------------|:------------------------|:-----------------|:-------------------|:------------------------|----------------:|
|    nan |     nan | nan                | nan                | nan                | mins              | Day of the week, Month Day, Year | Hour:Minute:Second (AM / PM) | Day of the week, Month Day, Year | Hour:Minute:Second (AM / PM) | Megawatt         |     nan           | %              | persons per square mile | %                | USD                | nan                     |             nan |
|   2011 |       7 | severe weather     | East North Central | normal             | 3060              | 2011-07-01 00:00:00              | 17:00:00                     | 2011-07-03 00:00:00              | 20:00:00                     | nan              |       2.5957e+06  | 73.27          | 2279                    | 91.5926658691451 | 51268              | nan                     |          276286 |
|   2014 |       5 | intentional attack | East North Central | normal             | 1                 | 2014-05-11 00:00:00              | 18:38:00                     | 2014-05-11 00:00:00              | 18:39:00                     | nan              |       2.64074e+06 | 73.27          | 2279                    | 91.5926658691451 | 53499              | vandalism               |          284978 |
|   2010 |      10 | severe weather     | East North Central | cold               | 3000              | 2010-10-26 00:00:00              | 20:00:00                     | 2010-10-28 00:00:00              | 22:00:00                     | nan              |       2.5869e+06  | 73.27          | 2279                    | 91.5926658691451 | 50447              | heavy wind              |          276463 |
|   2012 |       6 | severe weather     | East North Central | normal             | 2550              | 2012-06-19 00:00:00              | 04:30:00                     | 2012-06-20 00:00:00              | 23:00:00                     | nan              |       2.60681e+06 | 73.27          | 2279                    | 91.5926658691451 | 51598              | thunderstorm            |          278466 |

The dataset will be used further identify how different factors of an outage and where the outage occured affect the duration of an outage. The dataset has 1535 rows with the first row containing the units for the columns. The columns we we will be taking a closer look at include:

**CAUSE.CATEGORY:** Categories of all the events causing the major power outages

**OUTAGE.DURATION:** Duration of outage events (in minutes)

**DEMAND.LOSS.MW:** Amount of peak demand lost during an outage event (in Megawatt) [but in many cases, total demand is reported]

**CLIMATE.REGION:** U.S. Climate regions as specified by National Centers for Environmental Information (nine climatically consistent regions in continental U.S.A.)

**TOTAL.CUSTOMERS:** Annual number of total customers served in the U.S. state

**POPPCT_URBAN:** Percentage of the total population of the U.S. state represented by the urban population (in %)

**POPDEN_URBAN:** 	Population density of the urban areas (persons per square mile)

## Data Cleaning

To clean the data first we removed the first row that contained the units of each column and put it in its own dictionary separate from the rest of the data. Then we took the Outage start time column and date column and combining them and converting the time into a pd.Timestamp to be used as timestamp value. We additionally did this same process for when the restoration columns so that the start of the outage and when the outage was restored were in the correct and accurate format and dropped the old start/restoration columns. Next we made the OUTAGE.DURATION column the difference between the restoration column and when the outage started. Additionally we scaled the duration column to hours instead of minutes so that the numbers were easier to interpret and visualize. The first 5 rows of the cleaned dataframe are shown below.

|   YEAR |   MONTH | CAUSE.CATEGORY     | CLIMATE.REGION     | CLIMATE.CATEGORY   |   OUTAGE.DURATION |   DEMAND.LOSS.MW |   TOTAL.CUSTOMERS |   POPPCT_URBAN |   POPDEN_URBAN |   PCT_LAND |   PC.REALGSP.STATE | CAUSE.CATEGORY.DETAIL   |   COM.CUSTOMERS | OUTAGE START        | RESTORATION START   | is_storm   |
|-------:|--------:|:-------------------|:-------------------|:-------------------|------------------:|-----------------:|------------------:|---------------:|---------------:|-----------:|-------------------:|:------------------------|----------------:|:--------------------|:--------------------|:-----------|
|   2011 |       7 | severe weather     | East North Central | normal             |        51         |              nan |       2.5957e+06  |          73.27 |           2279 |    91.5927 |              51268 | nan                     |          276286 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00 | True       |
|   2014 |       5 | intentional attack | East North Central | normal             |         0.0166667 |              nan |       2.64074e+06 |          73.27 |           2279 |    91.5927 |              53499 | vandalism               |          284978 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00 | False      |
|   2010 |      10 | severe weather     | East North Central | cold               |        50         |              nan |       2.5869e+06  |          73.27 |           2279 |    91.5927 |              50447 | heavy wind              |          276463 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00 | True       |
|   2012 |       6 | severe weather     | East North Central | normal             |        42.5       |              nan |       2.60681e+06 |          73.27 |           2279 |    91.5927 |              51598 | thunderstorm            |          278466 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00 | True       |
|   2015 |       7 | severe weather     | East North Central | warm               |        29         |              250 |       2.67353e+06 |          73.27 |           2279 |    91.5927 |              54431 | nan                     |          289044 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00 | True       |

## Univariate Analysis

<iframe
  src="assets/DistOfDuration.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This distrubtion shows how a majority of the outage durations are quite small and take less than 1 day (24hrs) to restore. However, due to the distribution being right tailed there are a few extreme outage durations that took around 25+ days to restore. 

<iframe
  src="assets/distOfCauses.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This pie chart shows how a majority (approximately 50%) of outages are caused by severe weather and another 25% being caused by intentional attacks. All the other causes of outages happen evenly at around 5% for each of the other causes. 

## Bivariate Analysis

<iframe
  src="assets/durationByCause.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This bar chart shows that the outages that are caused by fuel emergencies have a significantly higher outage duration than any of the causes. However, it is important to note that fuel emergencies are only the cause of about 3% of outages. Additionally severe weather outages tend to have a higher outage duration than the other causes and happen quite often too (50%). 

<iframe
  src="assets/countMonthCause.html"
  width="1000"
  height="600"
  frameborder="0"
></iframe>

This bar chart shows that there is an increase in outages during the summer and fall months. Additionally there tended to be more storm related outages than non-storm outages during the summer months while winter and spring tended to have equal or more non-storm related outages than storm related outages. 

<iframe
  src="assets/durationByMonth.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This graph shows how during the late fall months the outage durations tended to be the largest and that late spring/early summer months had a lower average outage duration.

## Aggregate Analysis

| CAUSE.CATEGORY     |   equipment failure |   fuel supply emergency |   intentional attack |   islanding |   public appeal |   severe weather |   system operability disruption |
|:-------------------|--------------------:|------------------------:|---------------------:|------------:|----------------:|-----------------:|--------------------------------:|
| CLIMATE.REGION     |                     |                         |                      |             |                 |                  |                                 |
| Central            |             5.36667 |             167.254     |             5.76765  |   2.08889   |         23.5    |          54.1668 |                        44.92    |
| East North Central |           440.589   |             566.187     |            39.6008   |   0.0166667 |         12.2167 |          73.9136 |                        43.5     |
| Northeast          |             3.59667 |             243.826     |             3.26641  |  14.6833    |         44.25   |          73.8317 |                        12.8917  |
| Northwest          |            11.7     |               0.0166667 |             6.2302   |   1.22222   |         14.9667 |          80.6333 |                         2.35    |
| South              |             4.92963 |             291.375     |             5.42679  |   8.225     |         19.3996 |          73.1892 |                        14.4346  |
| Southeast          |             9.24167 |             nan         |             8.41111  | nan         |         47.7567 |          44.376  |                         2.82187 |
| Southwest          |             1.89667 |               1.26667   |             4.42787  |   0.0333333 |         37.9167 |         192.882  |                         5.48704 |
| West               |             8.74683 |             102.577     |            14.2946   |   3.58095   |         33.8019 |          48.8062 |                         6.06111 |
| West North Central |             1.01667 |             nan         |             0.391667 |   1.13667   |          7.325  |          40.7083 |                       nan       |

This table shows the average outage duration for each outage cause and climate region. This is significant because it can help identify regions that are more prone to have worse outages for specific causes. For example, the East North Central region has a significantly higher outage duration for equipment failure than other regionds. Also, The southwest region also seems to have a higher outage duration for severe weather. Given this information these regions can develop specific plans for how to address these areas and find what may be causing the difference. 

## Assesment of Missingness

### Non-Missing at Random (NMAR)

The missingness of the CAUSE.CATEGORY.DETAIL is most likely NMAR because the fact that a value is missing most likely depends on the detail itself. 
If the detail is very complicated/long then it is more likely to be missing than outage that can be simply explained by lightning.

### Additional Data Needed
Exploring the general data reporting practices and how comprehensive or timely the data collection process is for specific causes of outages could clarify whether missingness is due to the nature of the event (suggesting NMAR) or is more related to logistical reporting factors (suggesting MAR).
- Level of technology used by utility reporting data
- Collecting information on each utility's data reporting practices and procedures

### Missingness of the DEMAND.LOSS.MW
The missingness mechanism for DEMAND.LOSS.MW could be correlated to the other columns in our dataframe as the loss may be missing based on how many customers a utility has. To test this we will perform permutation tests on the missingness of DEMAND.LOSS.MW for different columns in our dataset to see which ones if any DEMAND.LOSS.MW may be dependently missing on.
### Results of Missingness Permutation Tests

#### COM.CUSTOMERS
The permutation test for COM.CUSTOMERS yielded a p-value of 0.0. This indicates a strong dependency between the missingness of DEMAND.LOSS.MW and the number of commercial customers (COM.CUSTOMERS). The absolute difference in means is significantly larger than what would be expected under the null hypothesis, meaning that missing demand loss values are not random with respect to COM.CUSTOMERS.

<iframe
  src="assets/ptestCustomers.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This finding suggests that outages affecting higher numbers of commercial customers are more likely to have demand loss values recorded, while smaller outages (affecting fewer commercial customers) may have missing values due to prioritization or reporting biases.

#### PCT_LAND
The permutation test for PCT_LAND yielded a p-value of 0.128, suggesting no significant dependency between the missingness of DEMAND.LOSS.MW and the percentage of land area (PCT_LAND). The absolute difference in means lies well within the range of test statistics under the null hypothesis, supporting the conclusion that missingness in DEMAND.LOSS.MW is independent of PCT_LAND.

<iframe
  src="assets/ptestPCTLand.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This result implies that geographic land area does not influence whether demand loss values are missing. The missingness is likely influenced by other variables or processes unrelated to land area.

## Do Severe Weather Outages Have Longer Outage Durations

### Hypotheses
- ***Null Hypothesis***: The mean outage duration is the same for outages caused by severe weather and those not caused by severe weather. Any observed difference is due to random chance.
- ***Alternative Hypothesis***: The mean outage duration is greater for outages caused by severe weather compared to those not caused by severe weather.

### Test Statistic
The test statistic is the difference in mean outage durations between outages caused by severe weather and those caused by other reasons
This test statistic was chosen because it directly measures the relationship between the type of cause and the resulting outage duration, which is the focus of our question.

### Test Details
- Significance Level: 0.05
- **Permutation Test**:
  - We randomly shuffle the is_storm labels (True for severe weather, False for other causes) 10,000 times.
  - For each permutation, we calculate the difference in mean outage durations, building a null distribution of the test statistic.

### Results

<iframe
  src="assets/ptestIsStorm.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

- Observed Difference in Means: 42.297
- P-Value: 0.0

### Conclusion
At a significance level of 0.05:
-  We reject the null hypothesis and conclude that there is evidence to suggest that severe weather tends to have higher durations for outages.
- the observed difference in mean outage durations between outages caused by severe weather and those caused by other reasons is statistically significant

### Justification
The choice of a permutation test is appropriate because it makes no assumptions about the distribution of the data, and it allows us to test the specific question of whether the observed difference in means could have arisen by chance. By focusing on the mean difference, the test is directly aligned with our research question.

## Prediction Problem

### Type of Prediction Problem
We want to predict outage duration using a subset of features from the dataset that we would know at the time of prediction. The time of prediction would most likely be right after an outage occurs and this model will be used to determine how long it might take to get restored

### Response Variable
The response variable is OUTAGE.DURATION, which measures the length of an outage in hours. We chose this variable because understanding and accurately predicting outage durations can help in resource allocation and emergency planning during outages.

### Features Used for Prediction
The features used to predict OUTAGE.DURATION are:
- CLIMATE.REGION (Nominal): The region where the outage occurred.
- CLIMATE.CATEGORY (NOMINAL): The category of the climate i.e. warm, cold, or normal.
- PCT_LAND (Quantitative): The percentage of the area covered by land in the affected region.
- TOTAL.CUSTOMERS (Quantitative): The number of total customers covered by the utility that had the outage.
- MONTH (Ordinal): the month the outage occured in.
- POPDEN_URBAN (Quantitative): The population density of the urban parts that the utitlity covers.

These features are justified because they are all known at the time of prediction and are likely to influence outage duration.

### Justification of Features
At the time of prediction (when an outage begins), we would have information about the geographical and environmental context (CLIMATE.REGION, CLIMATE.CATEGORY, PCT_LAND, POPDEN_URBAN), as well as the month the outage is occuring in, and the how much customers are currently being serviced (TOTAL.CUSTOMERS). Using these features ensures that the model is realistic and does not use data that would only become available after the outage ends.

### Evaluation Metric
The evaluation metric chosen for this regression problem is the **Root Mean Squared Error (RMSE)**. We selected RMSE because:
- It penalizes larger errors more heavily, making it sensitive to significant deviations in predicted vs. actual durations.
- It provides results in the same units as the response variable (hours), which is easier to interpret compared to other metrics like Mean Absolute Error (MAE).

### Why RMSE?
While Mean Absolute Error is less sensitive to outliers, we chose RMSE because it highlights situations where the model performs poorly for extreme outage durations. For this application, it’s important to minimize large errors as these could represent severe misjudgments in resource allocation during outages.

[Link to prediction-related visualizations]

## Baseline Model

### Model Overview
The baseline model is a **linear regression model** trained to predict OUTAGE.DURATION in hours using three categorical features: CAUSE.CATEGORY, CLIMATE.CATEGORY, and CLIMATE.REGION. These features were encoded using **one-hot encoding**, dropping the first category in each column to ensure proper model interpretability.

### Features
- **Quantitative/Ordinal Features**:
  - MONTH
  - POPDEN_URBAN
  - TOTAL.CUSTOMERS
  - PCT_LAND
- **Nominal Features**: 
  - CLIMATE.CATEGORY (one-hot encoded)
  - CLIMATE.REGION (one-hot encoded)

### Model Performance
- ***R^2 Scores***:
  - Training Set: 0.044
  - Test Set: 0.048
- **Root Mean Squared Error (RMSE)***:
  - Training Set: 95.07
  - Test Set: 103.84

### Evaluation of Model
The R^2 values for both the training and test sets indicate that the baseline model explains only a small portion of the variance in OUTAGE.DURATION. However, the RMSE values suggest that the model is moderately accurate in predicting the average duration of an outage, with a difference of approximately 104 hours on unseen data. This baseline model provides a simple starting point for prediction and will serve as a reference for evaluating the improvements made in the final model.

## Final Model

### Model Overview
The final model is a **support vector regression model** trained to predict OUTAGE.DURATION in hours. This model expands upon the baseline by including an additional categorical feature, CATEGORY.CAUSE, and One hot encoded this feature. We also added additional engineered features. We transformed TOTAL.CUSTOMERS and PCT_LAND into 4 quantiles as to limit the extremes of these two features and reduce variability. Another engineered feature is binarizing POPDEN_URBAN with a thresshold of 2600 to also reduce variability and extreme values and split values into either not dense or dense values. We believe that these new features each have their own benefit that will drive the error down for our new model.

### Features
- **Quantitative/Ordinal Features**:
  - TOTAL.CUSTOMERS - split into 4 quantiles
      - Outages where a utility has a lot of people to cover may take longer than others
  - PCT_LAND - split into 4 quantiles
      - Outages that occur where there may be a lot of water can take longer due to limited access
      - PCT_LAND is one of the columns most linearly correlated to OUTAGE.DURATION
  - POPDEN_URBAN - Binarized with a threshold of 2600
      - Outages in more dense areas may take longer to restore
  - MONTH - Passed through
      - Month appears to have a relationship with OUTAGE.DURATION as shown in the bivariate graph above
- **Nominal Features**:
  - CAUSE.CATEGORY (one-hot encoded)
      - Cause appears to have a relationship with OUTAGE.DURATION as explored in the hypothesis tests above
  - CLIMATE.CATEGORY (one-hot encoded)
      - A colder climate may take longer to restore an outage than a warmer climate
  - CLIMATE.REGION (one-hot encoded)
      - Certain regionds may have higher outage durations than others or be better at dealing with outages
### Hyperparameters
  - Kernel: Specifies the kernel type to be used in the algorithm
      - Values: Linear or Rbf
  - C: Regularization parameter. The strength of the regularization is inversely proportional to C
      - Values: .1, 1, 10, 100, 200, or 500
  - Gamma: Kernel coefficient for ‘rbf’, ‘poly’ and ‘sigmoid’.
      - Vales: scale, auto, .1, 1, 10, or 100
  - Epsilon: Epsilon in the epsilon-SVR model. It specifies the epsilon-tube within which no penalty is associated in the training loss function with points predicted within a distance epsilon from the actual value.
      - Values: .01, .1, .2, .3, .4
  
Used GridSearchCV to find the best hyperparameters using 5 fold cross-validation. The best hyperparameters were C: 100, Epsilon: .4, Gamma: 1, Kernel: "rbf.
    
### Model Performance
- **R^2 Scores**:
  - Training Set: .394
  - Test Set: .115
- **Root Mean Squared Error (RMSE)**:
  - Training Set: 75.68
  - Test Set: 100.13



[Link to final model visualizations]

## Does Our Model Perform the Same for Regions with High GSP and for Regions with Low GSP
Below is the distribution for Regional GSP. The threshold decided for low vs. high gsp was decided to be 52k.
<iframe
  src="assets/distGSP.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Hypotheses
- ***Null Hypothesis***: The RMSE of outage duration is the same for our models predictions of outage duration for regions with high gsp and regions with low gsp. Any observed difference is due to random chance.
- ***Alternative Hypothesis***: The RMSE of outage duration is greater for regions with low gsp.

### Test Statistic
The test statistic is the difference in RMSE. We chose this test statistic because categorizes the error of our model's predictions for each group and includes directionality that corresponds
with our hypothesis test.

### Test Details
- Significance Level: 0.05
- **Permutation Test**:
  - We randomly shuffle the high_state_gsp labels (True for regions with state_gsp > 52k, False for other causes) 1,000 times.
  - For each permutation, we calculate the difference in RMSE between the two groups, building a null distribution of the test statistic.

### Results

<iframe
  src="assets/ptestFair.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

- Observed Difference in Means: -31
- P-Value: 0.187

### Conclusion
At a significance level of 0.05:
-  We fail to reject the null hypothesis and conclude that the difference in RMSE for regions with low gsp and regions with high gsp is due to chance and that our model appears to be fair in this regard.



