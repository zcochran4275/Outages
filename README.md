# Outages
Final Project for DSC 80
## Introduction
### Question

What effect does the cause of an outage have?

### Data

We will be exploring this question through the use of the Outages dataset provided by Purdue University in which a raw preview is provided below.

|   YEAR |   MONTH | CAUSE.CATEGORY     | CLIMATE.REGION     | CLIMATE.CATEGORY   | OUTAGE.DURATION   | OUTAGE.START.DATE                | OUTAGE.START.TIME            | OUTAGE.RESTORATION.DATE          | OUTAGE.RESTORATION.TIME      | DEMAND.LOSS.MW   |   TOTAL.CUSTOMERS | POPPCT_URBAN   | POPDEN_URBAN            | PCT_LAND         |   COM.CUSTOMERS |
|-------:|--------:|:-------------------|:-------------------|:-------------------|:------------------|:---------------------------------|:-----------------------------|:---------------------------------|:-----------------------------|:-----------------|------------------:|:---------------|:------------------------|:-----------------|----------------:|
|    nan |     nan | nan                | nan                | nan                | mins              | Day of the week, Month Day, Year | Hour:Minute:Second (AM / PM) | Day of the week, Month Day, Year | Hour:Minute:Second (AM / PM) | Megawatt         |     nan           | %              | persons per square mile | %                |             nan |
|   2011 |       7 | severe weather     | East North Central | normal             | 3060              | 2011-07-01 00:00:00              | 17:00:00                     | 2011-07-03 00:00:00              | 20:00:00                     | nan              |       2.5957e+06  | 73.27          | 2279                    | 91.5926658691451 |          276286 |
|   2014 |       5 | intentional attack | East North Central | normal             | 1                 | 2014-05-11 00:00:00              | 18:38:00                     | 2014-05-11 00:00:00              | 18:39:00                     | nan              |       2.64074e+06 | 73.27          | 2279                    | 91.5926658691451 |          284978 |
|   2010 |      10 | severe weather     | East North Central | cold               | 3000              | 2010-10-26 00:00:00              | 20:00:00                     | 2010-10-28 00:00:00              | 22:00:00                     | nan              |       2.5869e+06  | 73.27          | 2279                    | 91.5926658691451 |          276463 |
|   2012 |       6 | severe weather     | East North Central | normal             | 2550              | 2012-06-19 00:00:00              | 04:30:00                     | 2012-06-20 00:00:00              | 23:00:00                     | nan              |       2.60681e+06 | 73.27          | 2279                    | 91.5926658691451 |          278466 |

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

|   YEAR |   MONTH | CAUSE.CATEGORY     | CLIMATE.REGION     | CLIMATE.CATEGORY   |   OUTAGE.DURATION |   DEMAND.LOSS.MW |   TOTAL.CUSTOMERS |   POPPCT_URBAN |   POPDEN_URBAN |   PCT_LAND |   COM.CUSTOMERS | OUTAGE START        | RESTORATION START   |
|-------:|--------:|:-------------------|:-------------------|:-------------------|------------------:|-----------------:|------------------:|---------------:|---------------:|-----------:|----------------:|:--------------------|:--------------------|
|   2011 |       7 | severe weather     | East North Central | normal             |        51         |              nan |       2.5957e+06  |          73.27 |           2279 |    91.5927 |          276286 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00 |
|   2014 |       5 | intentional attack | East North Central | normal             |         0.0166667 |              nan |       2.64074e+06 |          73.27 |           2279 |    91.5927 |          284978 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00 |
|   2010 |      10 | severe weather     | East North Central | cold               |        50         |              nan |       2.5869e+06  |          73.27 |           2279 |    91.5927 |          276463 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00 |
|   2012 |       6 | severe weather     | East North Central | normal             |        42.5       |              nan |       2.60681e+06 |          73.27 |           2279 |    91.5927 |          278466 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00 |
|   2015 |       7 | severe weather     | East North Central | warm               |        29         |              250 |       2.67353e+06 |          73.27 |           2279 |    91.5927 |          289044 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00 |

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






