> # **BIKE SHARING DEMAND**
---
**SECTION**:
<BR> [1. DATA UNDERSTANDING](#1-data-understanding)
<BR> [2. DATA PROCESSING](#3-data-processing)
<BR> [3. CONCLUSION](#4-conclusion)
<BR> [4. RECOMMENDATION](#5-recommendation)


___
# **1. BUSINESS PROBLEM UNDERSTANDING**
___
## CONTEXT

Bike sharing systems are a means of renting bicycles where the process of obtaining membership, rental, and bike return is automated via a network of kiosk locations throughout a city. Using these systems, people are able rent a bike from a one location and return it to a different place on an as-needed basis. Currently, there are over 500 bike-sharing programs around the world.

## DATABASE INFORMATION

Source: [Bike_Share](https://www.kaggle.com/competitions/bike-sharing-demand/overview/description)
<br><br>
This Database contains **11** Columns as the following descriptions:

**ATTRIBUTE INFORMATION**

| **Attribute** | **Description** |
|---------------|-----------------|
| **dteday**    |date             |
| **hum**       |relative humidity|
| **weathersit**|weather classification|
|               |1. Clear, Few clouds, Partly cloudy, Partly cloudy|
|               |2. Mist + Cloudy, Mist + Broken clouds, Mist + Few clouds, Mist|
|               |3. Light Snow, Light Rain + Thunderstorm + Scattered clouds, Light Rain + Scattered clouds|
|               |4. Heavy Rain + Ice Pallets + Thunderstorm + Mist, Snow + Fog|
|**holiday**    |whether the day is considered a holiday|
|**season**     |season classification|
|               |1. spring|
|               |2. summer|
|               |3. fall  |
|               |4. winter|
|**atemp**      |“feels like” temperature|
|**temp**       |temperature|
|**hr**         |rental time stamps (hours)|
|**casual**     |number of non-registered user rentals initiated|
|**registered** |number of registered user rentals initiated|
|**cnt**        |cumulative total rentals of casual & registered users|

  
## PROBELM STATEMENT

Several bike/scooter ride sharing facilities (e.g., Bird, Capital Bikeshare, Citi Bike) have recently launched, particularly in major cities such as San Francisco, New York, Chicago, and Los Angeles. One of the most significant commercial challenges is predicting bike demand on any given day. While having too many bikes wastes resources (both in terms of bike maintenance and the land/bike stand required for parking and security), having too few bikes results in revenue loss (varying from a short-term loss due to missing out on immediate clients to a potential longer-term loss due to a loss in future customer base). Thus, having an estimate of the demands would allow these businesses to operate more efficiently.

## GOALS
The purpose of this study is to estimate bike rental demand by combining past bike usage patterns with weather data. The data set includes two years' worth of hourly rental data.

## ANALYTIC APPROACH
The data set contains 12165 observations with 10 column variables (excluding the datetime column, which has been utilized as an index). In this project, 7 columns utilize as the feature set to predict the value of `count`, they are `datetime`, `season`, `holiday`, `weather`, `temp`, `atemp`, `humidity`. The split-up of the goal column `count` is represented by the other two columns (`casual` and `registered`).

## METRIC EVALUATION

The RMSE, MAE, and MAPE evaluation metrics will be employed, where RMSE is the mean value of the square root of the error, MAE is the average absolute value of the error, and MAPE is the average percentage error generated by the regression model. According to the limits of the features utilized, the smaller the RMSE, MAE, and MAPE values, the model is more accurate in predicting rental prices.

In addition, if the final model is a linear model, we can use the value of R-squared or adj. R-squared. The R-squared number is used to assess how well the model can capture the data's total variation. The closer the model gets to 1, the better it fits the data. This metric, however, does not apply to non-linear models.

  
---
# **2. DATA PROCESSING**
---
## Model Selection
  
  ### Cross Validation Score
  
|   | Model	                  |Score_CV RMSE	|Score_CV MAE	|Score_CV MAPE |Score_CV R2|
|---|-------------------------|---------------|-------------|--------------|-----------|  
| 0 |	Linear Regression	      |[-139.505, -168.457, -158.56, -131.927, -143.52]	| [-96.202, -109.251, -103.993, -88.561, -98.586]	| [-1.734, -1.539, -1.85, -1.851, -1.836]	| [0.184, -0.116, -0.058, 0.297, 0.135] |
| 1 |	KNN Regressor	          |[-142.378, -150.482, -142.847, -137.49, -147.608]	| [-98.798, -108.054, -99.942, -96.289, -105.547]	| [-2.191, -2.014, -2.313, -2.545, -2.347] | [0.15, 0.109, 0.141, 0.237, 0.085]
| 2 |	DecisionTree Regressor	|[-91.851, -83.438, -88.392, -72.609, -99.44]	| [-56.735, -49.563, -52.066, -43.742, -63.347]	| [-0.779, -0.65, -0.939, -0.505, -0.936]	| [0.646, 0.726, 0.671, 0.787, 0.585] |
| 3 |	RandomForest Regressor	|[-72.447, -59.688, -75.031, -57.455, -82.904]	| [-46.683, -37.223, -45.784, -35.77, -53.674]	| [-0.53, -0.387, -0.495, -0.373, -0.599]	| [0.78, 0.86, 0.763, 0.867, 0.711] |
| 4 |	XGBoost Regressor	      |[-70.517, -66.237, -62.235, -64.32, -64.247]	| [-44.776, -42.13, -38.384, -40.244, -41.594]	| [-0.447, -0.394, -0.384, -0.402, -0.404]	| [0.791, 0.827, 0.837, 0.833, 0.827] |

  <br><br>
  
|   |Model|	Mean_RMSE	| Std_RMSE	| Mean_MAE	| Std_MAE	| Mean_MAPE	| Std_MAPE	| Mean_R2	| Std_R2 |
|---|-----|-----------|-----------|-----------|---------|-----------|-----------|---------|--------|
|0|	Linear Regression	|-148.393725	|13.265038	|-99.318584	|7.020875	|-1.761955	|0.119577	|0.088373	|0.153796|
|1|	KNN Regressor	|-144.160891	|4.499723	|-101.726154	|4.380824	|-2.282005	|0.175986	|0.144453	|0.051505|
|2|	DecisionTree Regressor	|-87.146046	|8.943278	|-53.090831	|6.623725	|-0.761692	|0.167544	|0.683100	|0.069007|
|3|	RandomForest Regressor	|-69.504828	|9.594850	|-43.826792	|6.595133	|-0.476730	|0.086280	|0.796198	|0.059262|
|4|	XGBoost Regressor	|-65.510946	|2.804905	|-41.425673	|2.116611	|-0.406203	|0.021666	|0.823106	|0.016257|  
   
  <br>
  
  The RMSE and MAE numbers differ significantly, with the RMSE being greater since the residuals or errors are squared before being averaged. As a result, RMSE assigns a higher 'weight' to significant error numbers. In other words, because all of the algorithms used create huge error numbers, there is a significant discrepancy between the RMSE and MAE values. For all metrics, RMSE, MAE, MAPE, the smaller number indicate better model. Meanwhile for R2, the higher number indicate better model.

- Based on all of the metrics above, XGBoost is the best model while RandomForest show close model performace to XGBoost compared to other models. 
- The test set will then be predicted using the top benchmark models, XGBoost.
  
<br>
  
With hyperparameter adjustment, the model's performance slightly improved where RMSE, MAE, and MAPE values decrease. Moreover, model was also evaluate based on feature importance to see which attribute has significant impact on model performance.
|    METRICS   |    RSME   |    MAE    |   MAPE   |    R2    |
|--------------|-----------|-----------|----------|----------|
| BEFORE TUNING| 62.292755 | 45.170614 | 1.202913 |	0.83967  |
| AFTER TUNING | 63.8764   | 44.886791 | 1.066701 | 0.831414 |

  <br>
  
After implementing feature selection the model performance slightly decrease. However, after deploying hyperparameter on feature selection model, then the performance increase even better than the initial model. It can be found as follows:

|    METRICS   |    RSME   |    MAE    |   MAPE   |    R2    |
|--------------|-----------|-----------|----------|----------|
|    MODEL 1   |
| BEFORE TUNING| 62.292755 | 45.170614 | 1.202913 |	0.83967  |
| AFTER TUNING | 63.8764   | 44.886791 | 1.066701 | 0.831414 |
|    MODEL 2   |
| BEFORE TUNING| 66.207004 | 47.45046  | 1.297088 |	0.818888 |
| AFTER TUNING | 63.437227 | 43.918059 | 1.044236 | 0.833724 |
  
---
# **3. CONCLUSION**
---
According to the modeling, the `binary_hour_4` and `binary_hour_3` attributes had the most influence on `count`. This information can be used by operational department to assess the appropriate time for periodical maintenance on the bike that will not significantly affect the bikes availability

The evaluation metrics used in the model are RMSE, MAE & MAPE values. I prefer to use MAE to interpret this model since RMSE is more sensitive to outliers. On the other hand, MAE returns values that are more interpretable as it is simply the average of all errors.

It can be concluded from the MAE value generated by the model after hyperparameter tuning, which is 42.4, that when our model is used to estimate the bike rental demand in the range of values as trained on the model (maximum users count is 970), the average users prediction will miss by approximately 42 people from the demand it should have. However, it is probable that the forecast will miss even more because the model's bias remains fairly significant when viewed from the visualization of the actual and predicted prices. This model's bias can be caused by lack of data samples or require more features.

---
# **4. RECOMMENDATION**
---
Some recommendation that can be used to improved the model, such as:

1. Adding more feature may help to improve this model, such as pollution level and traffic.

1. Reviewing the parameter used in modeling, such as using `TimeSeriesSplit` for cross validation method.

1. Dig more on outlier handling that may caused the bias in the model.

1. Deploy more feature engineering process, such as data binning, polynomial, PCA, etc.


Find more information about this project on the following [Jupyter Notebook](https://github.com/zrfajri/Capstone3_Bike_Sharing_Demand/blob/37108d5363ad18e254f3e590d40892aec957a2ef/M3S20-5%20CAP%20MODULE%203.ipynb)
___

