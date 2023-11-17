# Power outage Data
<br>
Project created by Jill Luna Nomura

## Introduction

For our society to function, it is an inevitable truth that we must have an adequite power supply to run every businesses and live our daily life. There are many US citizen claiming that having electricity and internet connection to our homes are a basic necessities and are entitled to have these power sources. As covid-19 pandemic changed the livelihood of many people to work virtually from home, we are depending more to these power and if there is any unfortunate circumstances in the future that takes away lights in our home, I argue that the society will be in chaos and confusion. 
<br>
<br>
To make an inference about potential outage in the future, we must first analyze the outage that happened in our nation in the past. We were provided with a dataset from this research paper, **[Data on major power outage events in the continental U.S.](https://www.sciencedirect.com/science/article/pii/S2352340918307182)** under table 1. This data allows us to analyze each outage that happen through US and look at different features such as when, where, cause, and effects of these outage. With this dataset, I will analyze if the **cause of outage from WECC region comes from the same population against region that are not WECC.**
<br>
<br>
I decided to start with this question because the origional dataframe consists of information all over the US and I want to determine if we should treat these information as one, or separate these data into various regions to investigate further. If the cause of the outage from area supplied by WECC and area that is not supplied from WECC illustrates the same population, this can be a great measure for us to invesitgate this data set as a whole, but if they don't represent a same population, it creates a strong argument to query these dataframe by NERC region to investigate more on specific area of US. 
<br>
<br>
When I downloaded the original dataset, there was initially 1534 outage recorded with 56 features. We chose 13 features to investigate futher and the description are below. 

| Feature | Description |
| -------------------- | ----------- |
| Year | Year of insident |
| Month  | Month of insident |
| U.S._STATE | US STATE|
| NERC.REGION  | The North American Electric Reliability Corporation Region involved |
| CLIMATE.REGION | US Climate regions specified by National Centers for Environmental Information |
| CLIMATE.CATEGORY     | Climate of that year       |
| CAUSE.CATEGORY	   | Cause of outage        |
| CAUSE.CATEGORY.DETAIL| Cause of outage in detail      |
| HURRICANE.NAMES      | Name of the hurricane if the outage is caused by hurricane|
| OUTAGE.DURATION(hr)  | How long the outage last in hour |
| CUSTOMERS.AFFECTED   | Number of customers affected by the outage|
| OUTAGE.START         | Date of outage start       |
| OUTAGE.RESTORATION   | Date of when outage ended        |
<br>

## Cleaning and EDA
<br>

### Date Cleaning

We first downloaded the dataframe through exel as a csv file. 
<br>
<br>
The first cleaning I did was dealing with the outage time columns. We had four columns of strings which is outage start date and time, outage end date and time.
I have craeted a function convert_timestamp that takes in dataframe of these two columns and return a series where we combie these two column and converting the type from object to datetime. I assigned this series to a new column name OUTAGE.START and OUTAGE RESTORATION. 
<br>
<br>
The next cleaning I did was looking at OURAGE.DURATION column. This column was initally saved as in minutes, but it came to my attention that there are few outage that lasts over a day, so I divided this column by 60 to create a new column our outage duration in hours. 
<br>
<br>
Additionally, I checked if the relationship between OUTAGE.DURATION(hr) column and OUTAGE.START - OUTAGE.RESTORATION column are consistent. Conceptually, the hours between OUTAGE.START to OUTAGE.RESTORATION column must be consistent with OUTAGE.DURATION(hr) column. 
I first saw that the number of null on these columns are consistent with each other, which signifies that if duration of outage is unknown, the start and endtime of the outage is also unknown. 
Next I realized that there were 31 rows where the OUTAGE.DURATION and OUTAGE.RESTORATION - OUTAGE.START column differed in exactly one hour, but I disregarded this difference since one hour does not have a major impact in these column relationships. I concluded that the relationship between these two columns are consistent. 
<br>
<br>
After looking at the relationship between these columns, I looked into the "NERC.REGION" column. Looking at the unique values in NERC.REGION, there were 14 unique values when **[US energy atlas](https://atlas.eia.gov/datasets/eia::nerc-regions/explore?location=34.013788%2C-95.962606%2C4.59)** stated that there are 7 official NERC REGION and other region as indeterminate. I have created a new function change_indeterminate which replaces value of  reagion that is not listed in this website as "INDETERMINATE" and reassigned this series to the dataframe.
<br>
<br>
Lastly, I looked at every column in the dataframe to check that the type of each column is appropriate. 
<br>
<br>
Here is the fist few row of the cleaned Dataframe
<br>

|   YEAR |   MONTH | U.S._STATE   | NERC.REGION   | CLIMATE.REGION     | CLIMATE.CATEGORY   | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL   |   HURRICANE.NAMES |   OUTAGE.DURATION(hr) |   CUSTOMERS.AFFECTED | OUTAGE.START        | OUTAGE.RESTORATION   |
|-------:|--------:|:-------------|:--------------|:-------------------|:-------------------|:-------------------|:------------------------|------------------:|----------------------:|---------------------:|:--------------------|:---------------------|
|   2011 |       7 | Minnesota    | MRO           | East North Central | normal             | severe weather     | nan                     |               nan |            51         |                70000 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  |
|   2014 |       5 | Minnesota    | MRO           | East North Central | normal             | intentional attack | vandalism               |               nan |             0.0166667 |                  nan | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  |
|   2010 |      10 | Minnesota    | MRO           | East North Central | cold               | severe weather     | heavy wind              |               nan |            50         |                70000 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  |
|   2012 |       6 | Minnesota    | MRO           | East North Central | normal             | severe weather     | thunderstorm            |               nan |            42.5       |                68200 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  |
|   2015 |       7 | Minnesota    | MRO           | East North Central | warm               | severe weather     | nan                     |               nan |            29         |               250000 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  |

<br>

### EDA

<iframe src="assets/cause_histogram.html" width=800 height=600 frameBorder=0></iframe>
This is a histogram that represent the frequency distribution of the causes of the outage. There are 7 causes recorded in these outage and we can observe that outage from severe weather is the most common. 

<br>
<iframe src="assets/duration_effected_customer.html" width=800 height=600 frameBorder=0></iframe>
This scatter plot represent the relationship between the duration of the outage and total number of customers effected during these outage. Although most dots are scattered in one area, the interesting thing to note is that the more customer effected, the shorter the duration of outage and insidents that lasts the longest is where few people are effected by these outage. 

<br>

| NERC.REGION   |   equipment failure |   fuel supply emergency |   intentional attack |   islanding |   public appeal |   severe weather |   system operability disruption |
|:--------------|--------------------:|------------------------:|---------------------:|------------:|----------------:|-----------------:|--------------------------------:|
| INDETERMINATE |                   6 |                       0 |                    3 |           0 |               3 |               58 |                              12 |
| MRO           |                   0 |                       4 |                   16 |           2 |               2 |               21 |                               0 |
| NPCC          |                   2 |                      13 |                   57 |           1 |               4 |               64 |                               9 |
| RFC           |                   8 |                       5 |                  106 |           5 |               3 |              279 |                              12 |
| SERC          |                   7 |                       2 |                   30 |           1 |              13 |              130 |                              18 |
| SPP           |                   1 |                       1 |                    8 |           2 |              16 |               36 |                               2 |
| TRE           |                   2 |                       5 |                    9 |           0 |              16 |               62 |                              17 |
| WECC          |                  31 |                      20 |                  189 |          35 |              12 |              109 |                              55 |

This is a pivot table that looks at the relationship between NERC REGION and the Cause of the outage. You can see that the most prominant reason of the outage depends on the region you reside in. 
<br>

## Assessment of Missingness

Looking at NMAR
<br>
I argue that the possible NMAR in this dataset is in the OUTAGE.RESTORATION column. The main reason why I argue this is because there are 1442 non null value in OUTAGE.START column but there are 1395 non null value in the OUTAGE.RESTORATION column. This means that date of when outage started is recorded when the date the outage ended is not recorded. The possible consideration for this case is if the duration of the outage are significantly small, people may forget to record the date that the outage were restored. Since the data of duration of the outage are related with the OUTAGE.RESTORATION column, I think the additional data that can help you explain the missingness is getting the duration of these outage from different confidnetial source and merge these two together.  

<br>
Looking at MAR
<br>
Since both the NERC.REGION column and CAUSE.CATEGORY column don't have null in every data, there isn't a particular need to interpret result of the missingness with respect of the question I am trying to analyze. 
Although I will not be using this column in my hypothesis testing, there is a column call CAUSE.CATEGORY.DETAIL and HURRICANE.NAMES that do have null value and are related to CAUSE.CATEGORY column, so I decided to analyze HURRICANE.NAMES missingness with relation to CAUSE.CATEGORY.DETAIL column. 
Since the CAUSE.CATEGORY.DETAIL column is categorical, I performd a TVD as a test statistic to make this analysis. 
The empirical distribution of TVD and the observed statistics are shown below. 
<iframe src="assets/MAR_hurricane.html" width=800 height=600 frameBorder=0></iframe>
As you can see, the observed TVD recorded is very unlikely due to chances alone, therefore we will reject the null that missingness of hurricane column are likely to be effected by the detailed cause column.  
<br>

## Hypothesis Testing

The null hypothesis for this analysis is **the sample of NERC REGION of WECC do come from the same population in terms of causes of outage of other NERC REGION**
<br>
The alternative hypothesis is **the sampe of NERC REGION of WECC illustrates a different cause of outage compared to other NERC REGION**
<br>
<iframe src="assets/Permutation_distribution.html" width=800 height=600 frameBorder=0></iframe>
This is the different distribution of causes depending on wheather the outage resulted from WECC region or not.
<br>
Because we are comparing if two sample came from the same population, we will perform a permutation test with 5% significance. 
The resulting p value was 0.0 which concludes that we will reject the null that the different distribution of causes illustrated by WECC region and others are unlikely due to chances alone. 
<br>
By interpretating this result, I will most likely look over this dataframe and focus on different NERC region to make a inference about the future outage rather than looking at every outage in the region. 
