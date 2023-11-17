# Power outage Data

## Introduction

For our society to function, it is an inevitable truth that we must have an adequite power supply to run every businesses and live our daily life. There are many US citizen claiming that having electricity and internet connection to our homes are a basic necessities and are entitled to have these power sources. As covid-19 pandemic changed the livelihood of many people to work virtually from home, we must depending more to these power and if there is any unfortunate circumstances in the future that takes away lights in our home, I argue that the society will be in chaos and confusion. 
<br>
To make an inference about potential outage in the future, we must first analyze the outage that happened in our nation. We were provided with a dataset from this [article] (https://www.sciencedirect.com/science/article/pii/S2352340918307182) under table 1. This data allows us to analyze each outage that happen through US and look at different features such as when, where, cause, and effects of these outage. With this dataset, I will analyze if the **outage from WECC region comes from the same population of causes in the outage against region that are not WECC.**
<br>
I decided to start with this question because the origional dataframe consists of information all over the US and I want to determine if we should treat these information as one, or separate these data into various regions to investigate further. If the area of outage supplied by WECC and area that is not supplied from WECC illustrates the same population, this can be a great measure for us to invesitgate this data set as a whole, but if they don't represent a same population, it creates a strong argument to query these dataframe by NERC region to investigate more on specific are of US rather than as a whole. 
<br>
When I downloaded the original dataset, there was initially 1534 outage recorded with 14 features. I have created the table of 14 features with description below. 
| Feature              | Description |
| -------------------- | ----------- |
| Year | Year of insident       |
| Month  | Month of insident        |
| U.S._STATE  | US STATE       |
| NERC.REGION  | The North American Electric Reliability Corporation Region involved |
| CLIMATE.REGION       | US Climate regions specified by National Centers for Environmental Information       |
| CLIMATE.CATEGORY     | Climate of that year       |
| CAUSE.CATEGORY	   | Cause of outage        |
| CAUSE.CATEGORY.DETAIL| Cause of outage in detail      |
| HURRICANE.NAMES      | Name of the hurricane if the outage is caused by hurricane        |
| OUTAGE.DURATION(hr)  | How long the outage last in hour       |
| CUSTOMERS.AFFECTED   | Number of customers affected by the outage        |
| OUTAGE.START         | Date of outage start       |
| OUTAGE.RESTORATION   | Date of when outage ended        |

## Cleaning and EDA

## Assessment of Missingness

## Hypothesis Testing


