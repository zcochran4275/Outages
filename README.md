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

## Univariate Analysis
<iframe
  src="assets/DistOfDuration.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
<iframe
  src="assets/distOfCauses.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Bivariate Analysis
<iframe
  src="assets/durationByMonth.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
<iframe
  src="assets/durationByCause.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
<iframe
  src="assets/countMonthCause.html"
  width="1000"
  height="600"
  frameborder="0"
></iframe>

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
