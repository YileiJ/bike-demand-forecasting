# bike-demand-forecasting
For CS539 Project: Next-hour bike-sharing demand forecasting using environmental prediction and neural networks.

Project Goal：Our project aims to help bike-sharing operations planners anticipate next-hour rental demand. Using the previous three hours of temperature, humidity, wind speed, and rental counts, together with the target hour’s calendar information, our system will first predict the next hour’s environmental conditions and then estimate total bike rentals for that hour. 

The First part (Neural Networks 1),environmental prediction: we would use the previous 3 hours: temperature, humidity and wind speed to predict next hours’ temperature, humidity and wind speed.

Input								                                        Target Output
[								                                            [ 
  [temp_0, hum_0, windspeed_0],         model               temp_3,
  [temp_1, hum_1, windspeed_1],       —---->                hum_3,
  [temp_2, hum_2, windspeed_2]                               windspeed_3
]                                                           ]
e.g.
[							                                  [
  [20.0, 70, 10],   // 08:00				            24,	
  [21.0, 65, 12],   // 09:00          —----> 		55,
  [22.5, 60, 14]    // 10:00				            15,
]							                                   ]

But here, because we are doing the Time Series Forecasting, we will compare three approaches to next-hour environmental forecasting:
Standard MLP
Persistence Baseline (directly use the previous 1 hour’s data as predicted data)
Residual MLP

The Second Part: 
	Compare the results of three models and decide which model is more suitable for our next part. 

The Third Part(Neural Networks 2):  here we would use the predicted environmental conditions, total bike rentals for the previous hour and target hour’s calendar information to predict the total bike rentals for the next hour.

Input

 [temp_predicted, hum_predicted, windspeed_predicted, cnt_previous, hr, month, weekday, workingday, holiday ] 
e.g. [
  0.23,  // temp_predicted
  0.78,  // hum_predicted
  0.01,  // windspeed_predicted
  32,    // cnt_previous
  3,     // target time: 3 am
  1,     // target month: January
  6,     // Saturday
  0,     // non Workday
  0      // non Holiday
]

	
	output
[cnt_predicted]  // e.g. 56
Why We Chose This Direction: We chose this project because it addresses a concrete planning need and uses publicly available hourly data with existing numerical targets, avoiding manual annotation. The task is manageable for a student team and allows us to investigate whether predicted environmental conditions improve demand forecasting beyond using recent observations alone.



Existing Projects Survey

open source project: https://github.com/udacity/deep-learning/tree/master/first-neural-network
This educational project provides a NumPy-based neural-network template for predicting hourly bike rentals using weather and calendar features. Its notebook demonstrates categorical encoding, numerical scaling, chronological data splitting, and prediction evaluation. It informs the design of our second neural network and provides a useful starting point for a demand-prediction baseline. 
original data source: https://archive.ics.uci.edu/dataset/275/bike+sharing+dataset
This dataset contains hourly and daily rental counts from the Capital Bikeshare system during 2011–2012, together with weather and calendar information. We will use the hourly records, particularly temperature, humidity, wind speed, and total rental count. These observations allow us to automatically construct training pairs for both next-hour environmental prediction and rental-demand prediction without manual annotation.
https://arxiv.org/abs/1512.03385





Dataset Exploration
We inspected hour.csv from the UCI Bike Sharing Dataset. Each row corresponds to one hour and includes weather and the number of bike rentals. The following are three actual examples:

Date  Hour  Temperature  Humidity  Wind Speed  Total Rentals
2011-01–01  12  0.42  0.77  0.2836  84
2011-01–01  13  0.46  0.72  0.2985  94
2011-01–01  14  0.46  0.72  0.2836  106

(Temperature, humidity, and wind speed are normalized values.)

Historical fields contain temperature, humidity, and wind speed, which can be used to predict the weather of the next hour. The historical rental-count and predicted weather can be used in next-hour rental-count prediction. The actual recorded weather and rental-count of the next hour can be used as training labels. The weather and rental records have been matched by the hour, which is suitable for connecting the two prediction models. 
One uncertainty may be that the dataset records the actual rental but does not include station inventory information. Therefore, it can predict rental counts but it is not sufficient to directly determine how many bikes should be deployed at each station.


original data source: https://archive.ics.uci.edu/dataset/275/bike+sharing+dataset

open source project: https://github.com/udacity/deep-learning/tree/master/first-neural-network


