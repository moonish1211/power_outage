# Power outage Data

## Introduction

For our society to function, it is an inevitable truth that we must have an adequite power supply to run every businesses and live our daily life. There are many US citizen claiming that having electricity and internet connection to our homes are a basic necessities and are entitled to have these power sources. As covid-19 pandemic changed the livelihood of many people to work virtually from home, we are depending more to these power and if there is any unfortunate circumstances in the future that takes away lights in our home, I argue that the society will be in chaos and confusion. 
<br>
<br>
To make an inference about potential outage in the future, we must first analyze the outage that happened in our nation. We were provided with a dataset from this **[paper](https://www.sciencedirect.com/science/article/pii/S2352340918307182)** under table 1. This data allows us to analyze each outage that happen through US and look at different features such as when, where, cause, and effects of these outage. With this dataset, I will analyze if the **outage from WECC region comes from the same population of causes in the outage against region that are not WECC.**
<br>
<br>
I decided to start with this question because the origional dataframe consists of information all over the US and I want to determine if we should treat these information as one, or separate these data into various regions to investigate further. If the cause of the outage comes from area of power supplied by WECC and area that is not supplied from WECC to illustrate the same population, this can be a great measure for us to invesitgate this data set as a whole, but if they don't represent a same population, it creates a strong argument to query these dataframe by NERC region to investigate more on specific are of US rather than as a whole. 
<br>
<br>
When I downloaded the original dataset, there was initially 1534 outage recorded with 13 features. I have created the table of 14 features with description below. 
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


## Cleaning and EDA

### Date Cleaning

We first downloaded the dataframe through exel as a csv file. 
<br>
The first cleaning I did was dealing with the outage time columns. We had four columns of strings which is outage start date and time, outage end date and time.
I have craeted a function convert_timestamp that takes in dataframe of these two columns and return a series where we combie these two column and converting the type from object to datetime. I assigned this series to a new column name OUTAGE.START and OUTAGE RESTORATION. 
<br>
The next cleaning I did was looking at OURAGE.DURATION column. This column was initally saved as in minutes, but it came to my attention that there are few outage that lasts over a day, so I divided this column by 60 to create a new column our outage duration in hours. 
<br>
Additionally, I checked if the relationship between OUTAGE.DURATION(hr) column and OUTAGE.START - OUTAGE.RESTORATION column are consistent. Conceptually, the hours between OUTAGE.START to OUTAGE.RESTORATION column must be consistent with OUTAGE.DURATION(hr) column. 
I first saw that the number of null on these columns are consistent with each other, which signifies that if duration of outage is unknown, the start and endtime of the outage is also unknown. 
Next I realized that there were 31 rows where the OUTAGE.DURATION and OUTAGE.RESTORATION - OUTAGE.START column differed in exactly one hour, but I disregarded this difference since one hour does not have a major impact in these column relationships. 
<br>
After looking at the relationship between these columns, I looked into the "NERC.REGION" column. Looking at the unique values in NERC.REGION, there were 14 unique values when **[US energy atlas](https://atlas.eia.gov/datasets/eia::nerc-regions/explore?location=34.013788%2C-95.962606%2C4.59)** stated that there are 7 official NERC REGION and other region as indeterminate. I have created a new function change_indeterminate which replace value of every reagion that is not listed as "INDETERMINATE" and reassigned this series to the dataframe.
<br>
Lastly, I looked at every column in the dataframe to check that the type of each column is appropriate. 
<br>
Here is the fist few row of the cleaned Dataframe

```py
print(outage.head().to_markdown(index=False))
```

<br>

### EDA

<iframe src="assets/cause_histogram.html" width=800 height=600 frameBorder=0></iframe>
This is a histogram that represent the distribution of the causes of the outage. There are 7 causes in these outage and we can observe that outage from severe weather is the most common. 

<br>
<iframe src="assets/duration_effected_customer.html" width=800 height=600 frameBorder=0></iframe>
This scatter plot represent the relationship between the duration of the outage and total number of customers effected during these outage. The interesting thing to note is that although most dots are scattered in one area, we can see that the more customer effected, the shorter the duration of outage and vice versa. 



## Assessment of Missingness

## Hypothesis Testing


