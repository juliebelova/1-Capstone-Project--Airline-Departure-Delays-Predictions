# Capstone: Predicting Airlines Flight Delays

Author: Julie Vovchenko


## Content:
- [Problem Statement](#Problem-Statement)
- [Executive Summary](#Executive-Summary)
- [Conclusions and Recommendations](#Conclusions-and-Recommendations)
- [Next Steps](#Next-Steps)
- [References](#References)


## Datasets:
- [Collection of all flights for 2018(w/preliminary cleaning)](https://drive.google.com/open?id=1jAT2eYlOb6iwaCs48KP1Tqo9uRdMzYWg)
- [Collection of all flights for 2018(after final cleaning)](https://drive.google.com/open?id=1JBU4TLi5_JbYPWrBWycGbkM4DvN8mPrS)
- [Collection of all flights for 2018(ready for modeling)](https://drive.google.com/open?id=13xUDZlglltxO7j7fe-C4RGEjcCIqq1mA)


## Problem Statement
Sometimes we catch ourselves in situations that we are concerned if our flight will be delayed, especially if there is a connecting flight. Many of us travel with our entire family or are traveling for a business trip and every second counts. Money plays a major factor in deciding between a direct flight or connecting flight and we have all experienced the stress of a connecting flight. We can relate or imagine the aggravation of missing a connection and then having to spend more money during the waiting period. Before booking a flight, we tend to consider the following: will it departure on time, will it arrive on time, will we be able to make connecting flight or train on time? Sometimes, the next flight is in an hour, but other times it may take a whole day to get on the next flight. Even with flights between the same carrier, communication between pilots are nonexistent, which can cause passengers to miss their connecting flights by minutes. 

By utilizing a public dataset, provided by the [Bureau of Transportation Statistics](https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236) (a subdivision of the US Department of Transportation), on local flights in the United States from 2018, we strive to predict whether a flight will be delayed by 15 minutes or more. After completing our classification models, we can analyze the average precision scores to determine how effective certain factors are on flight departures. Ultimately, we want to ease the minds of potential passengers and provide them with the knowledge of timing their connections. 
 

## Executive Summary
[First notebook](../code/1_data_cleaning.ipynb) begins by gathering 12 downloaded files for each month, January through December, in 2018 gathered from [Bureau of Transportation Statistics](https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236).  We were able to collect a little over 7 million local flights for the whole year. Since we working on airline departure delay predictions, we decided to delete all instances where the flight was canceled. Then we dropped those columns that were mostly incomplete and variables were impossible to calculate or otherwise impute. Finally, our target variable was declared as those flights with delays with more than 15 minutes.

During our EDA in [Second notebook](../code/2_eda.ipynb) we learned the distribution of each feature and depending on the month, day, hour and origin airport of a flight we can predict whether the plane will departure on time or delayed. 

In [Third notebook](../code/2_modeling.ipynb) we implemented some feature engineering: 
1. Collected information from the prior flight on that day for each plane specifically, including same day prior flights count, prior origin and whether the plane had departure delay from previous flight,
2. Converted cyclical features into sine and cosine alternatives,
3. Implemented interactions with features with highest correlation to the target,  
4. Ordinal features, e.g. origin and destination airports, were transformed through LabelEncoder, instead of DummyRegressor. It saved us memory space dramatically and predictive models ran much faster. 

Since our dependent variable, departure flight delay, is categorical (1-delayed, 0-departured on time), we can fit data into several classifiers, including Logistic Regression, Random Forest, AdaBoost and Feed Forward Neural Network. We complemented these classifiers with some necessary hyperparameters in order to select the best classifier, based on average precision score. 

In comparison with our baseline average precision score (0.18) all of our complex models performed much better.  Based on our models average precision scores, we decided to choose Random Forest (0.57). 


## Conclusion and Recommendations
Using detailed files on domestic flights for each month in 2018, downloaded from Bureau of Transportation Statistics, we able to gather all files into one dataframe. After doing the initial cleanup we were able identify features that are appropriate to use in predicting flight departure delay. Then, we implemented some feature engineering and ran our data through several classifiers, using average precision score as our metric.

Scores of our models are as following: 

|   Classifier   | Train Score | Test Score |  |
|------------|------------|-------------|------------|
|      Base Line      | 0.18  | 0.18       |            |
| Logistic Regression | 0.493| 0.492        |        |
|  **Random Forest**  |**0.711** | **0.568**        |       |
|    AdaBoost   |0.485 | 0.483      |      |
|    Feed Forward Neural Network   | 0.530 | 0.531     |       |

All the models we have trained outperformed the Base Line model. Random Forest had the highest Train and Test average precision score. The scores were 0.711 and 0.5682 on the Train and Test respectfully. Even though our Random Forest model is overfit it performed better than other models we still think there is a scope of improvement. We need to tune it by reducing maximum features and maximum depth of each tree.

Features that mostly help in identifying whether the departure flight be delayed or not, are prior flight departure delays, scheduled departure hour and scheduled arrival hour, along with prior departure hour and prior flight count for the day. 

We noticed that our model has limitations. Since we have collected local flights for one year 2018, it only works for local flights and local carriers and it does not take into consideration any economic factors. For example, corona virus outbreak  lead to most flights to be canceled. Other factors that might change over time are (1) airport architectural restructure, that leads to a better internal airport workflow and minimizes departure delays, (2) updating the fleet with newer airplane models that have higher performance rate and require less maintenance.


## Next Steps
According to the [Federal Aviation Administration](https://www.sciencedirect.com/science/article/pii/S2212012218300753), most of the delays in winter are due to surface winds, low ceiling and low visibility, whereas during summer the majority of delays is attributable to convective weather, low ceiling and associated low visibility (Federal Aviation Administration, 2017).  
It will be wise to include daily weather data, like wind speed and precipitation rate, for each origin and destination location for each flight. Here are some resources that can help with that:  
1. [noaa.gov](https://www.ncdc.noaa.gov/cdo-web/)
2. [weather.gov](https://www.weather.gov/help-past-weather)

Additionally, we would also use data about each plane used for the flight. [flightradar24.com](https://www.flightradar24.com/data/aircraft/) website has extensive dataset for almost all planes that are in use by local airlines. This might help to identify those flight delays that occur due manufacture quality or age of the plane. 


## References
www.transtats.bts.gov  
www.noaa.gov  
www.weather.gov  
www.flightradar24.com 
www.faa.gov
https://medium.com/ai%C2%B3-theory-practice-business/top-6-errors-novice-machine-learning-engineers-make-e82273d394db  

