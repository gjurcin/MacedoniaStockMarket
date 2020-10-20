<!DOCTYPE html>
<html>
 <head>
 </head>
<body>

<h2>Macedonian Stock Market Exchange Prediction</h2>

:star: Star us on GitHub — it helps!

<h3> Introduction </h3>

<small>
 <p>
 <i>"Wall Street makes its money on activity, you make your money on inactivity." – Warren Buffett </i>
 
Inspired by the most famous dynamic market on earth, Wall Street, we started to analyze our domain stock market exchange in Macedonia. For most of the brokers, stock market exchange is one of the biggest mysteries and through the science still there is not found some algorithm which can predict some great depression. </p>

<p>This project is about forecasting stock prices of the most famous companies on Macedonian Stock Market Exchange. We will preprocess all data for 26 companies, but after cleansing we will make a prediction only on 4. Dataset is collected from 1997 till today.

The aim of the project is to build a model which will predict the future price.
The project is done in a few phases:

- [Data Cleaning](#data-cleaning)
- [Feature Engineering](#feature-engineering)
- [Models implementation](#models-implementation)
- [Conclusion](#conclusion)

</small></p>


## Data Cleaning

<p>
There are missing values in date series because the stock market is functional only on working days during the week. Moreover, some stocks are not traded every day. So we decided to make a series with all missing values as a sequence in dates and complete daily prices for all stocks. We used pandas library and MultiIndex method to fulfil the series by stocks. Empty prices of open,	high,	low,	average,	close,	volume and	quantity was fulfilled according to the logic that close price will be the same as last traded day, but also open, high, low and average because there is no fluctuation. Volume and quantity will be equal to zero.
After cleansing and filling data, we can visual time series with all different stocks that we have but with the logarithm of close pairs to show them in one range. We can catch the fall of 2009 but also in 2020 due to the pandemic. Also, if there is straight line, we can notice that the stock is not very often trade on the market.
</p>

<img width="75%" height="75%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/time_series_plot.png"></a></p>

<p>
Another very important statistical indicator is that only a few of the stocks close price has lots of outliers and the rest of them has normal increment during the period. We can check the statement on the box plot analysis below:

<img width="75%" height="75%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/outliers_by_stock.png"></a></p>

<p>
In the end we finished with 12 features of which last column start_date give us information about first date of trading with the initial stock.

<img width="75%" height="75%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/sample.png"></a>
</p>


## Feature Engineering

<p>Right off the bat, time-series data is not your average dataset. In all other datasets, the common thing is that all features, in general, are independent of each other. What sets time-series apart from regular, there is some inherent ordering of the data.
After loading the preprocessd dataset, we started to extract all possible feature from Timestamp column. Date column is already converted to datetime64[ns] type. Though, within extraction we found 14 new features. On the image below you can check the code of extraction.</p>

```python
df_final['sale_year'] = df_final.date.dt.year
df_final['sale_month'] = df_final.date.dt.month
df_final['sale_week'] = df_final.date.dt.week
df_final['sale_day'] = df_final.date.dt.day
df_final['sale_dayofweek'] = df_final.date.dt.dayofweek
df_final['sale_dayofyear'] = df_final.date.dt.dayofyear
df_final['is_month_end'] = df_final.date.dt.is_month_end
df_final['is_month_start'] = df_final.date.dt.is_month_start
df_final['is_quarter_end'] = df_final.date.dt.is_quarter_end
df_final['is_quarter_start'] = df_final.date.dt.is_quarter_start
df_final['is_year_end'] = df_final.date.dt.is_year_end
df_final['is_year_start'] = df_final.date.dt.is_year_start
df_final['days_in_month'] = df_final.date.dt.days_in_month
df_final['is_leap_year'] = df_final.date.dt.is_leap_year
```

<h3>Rolling windows</h3>

<p>One of the most important process that we have done is creating features with time lags or adding some explanation features to target value such as the mean of the last 7 days of the close price. Before all we set the Date column as an index. Moreover, in the rolling windows process, we added 31 explainable features. Though, on the sample of exploring for one of the stocks we found lots of insights, which feature explain the most of the model. On the image below you can check the order. Most of the rolling windows are made on prices open, average and close, but also the ratios to explain growth by time.</p>

<p align="center">
 <a href="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/feature_importans.PNG"><img width="70%" height="70%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/feature_importans.PNG"></a>
</p>

## Models implementation

<h3>using regressors</h3>

<p>In order to be able to discuss the models and their loss first must be completed, the data processing, reprocessing, cleaning and data analysis. To make better results we compare some techniques and here we present the best of them.
Before we build and train models we make feature engineering task and split the data into training and test, but also we shift the label column by 7 days so we can find the price after 7 days. Label column will be close price.

We used two ensemble regreossor and one RNN model with LSTM. If we are talking about regoressors, first one was Random Forest and the second one was XGBoost. Before using them for different stocks also we have made some grid search algorithms to find best hyperparametars. This tuning helped us to create surly precise models because we have already created validation and test set of sample. The most important thing to mention here is creating new features to use labeled data with more than 50 features. We will show the results for 4 different stocks. </p>

### Alkaloid (ALK)

<p>After splitting the dataset by each stock, and also split on training and validation set we calculate the loss of root mean squared error. On training set we get RMSE with 6.9 and RMSE on validation set with 38.79. Another parameter that we measured was the coefficient of determination and on train we get 0.99 but on the validation we get 0.89. It is obvious that when you predicting price with lots of uncertainty there will be some difference between training and validation measures. With XGBoost 0.15 RMSE on training set and 28.43 RMSE on validation set. R^2 on training set was 0.99 and on validation set 0.94.</p>

<p align="center">
 <a href="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/alkaloid_regoressor.PNG"><img width="70%" height="70%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/alkaloid_regoressor.PNG"></a>
</p>

<p>In this case when we compare on training and validation we can conclude that XGBosst give us better results than Randrom Forest. Forecast for 7th day or on 2020-09-01 with Random Forest is 11994.0 and with XGBoost is 11869.0. After a while, the real price was 12200.0 so Random Forest get better prediction with 206 MKD difference less than actual. This makes 0.02 errors on prediction.</p>

### Granit (GRNT)

<p>On the training set for Granit Stock we get RMSE with 1.95 and RMSE on validation set with 1.38. Other parameter that we measured was the coefficient of determination and on train we get 0.99 but on the validation we get 0.94. This calculation is done with Random Forest. Calculation with XGBoost was a little bit worse.</p>

<p align="center">
 <a href="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/granit_regoressor.PNG"><img width="70%" height="70%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/granit_regoressor.PNG"></a>
</p>

<p>In this case when we compare on training and validation we can conclude that Random Forest gives us better results than XGBoost. Forecast for 7th day or on 2020-09-01 with Random Forest is 950.0 and with XGBoost is 952.0. After a while, the real price was 975.0 so XGBoost get better prediction with negative 23 MKD difference than actual. This makes 0.02 error on prediction.</p>


### Komercijalna Banka (KMB)

<p>On the training set for Granit Stock we get RMSE with 9.30 and RMSE on validation set with 10.69. Other parameter that we measured was the coefficient of determination and on train we get 0.99 but on the validation we get 0.91. This calculation is done with Random Forest. Calculation with XGBoost was better on training set, but a little bit worse on validation set.</p>

<p align="center">
 <a href="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/kmb_regoressor.PNG"><img width="70%" height="70%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/kmb_regoressor.PNG"></a>
</p>

<p>In this case when we compare on training and validation we can conclude that Random Forest gives us better results than XGBoost. Forecast for 7th day or on 2020-09-01 with Random Forest is 5891.0 and with XGBoost is 5886.0. After a while, the real price was 6299.0 so Random FOrest get better prediction with negative 408 MKD difference than actual. This makes 0.06 errors in prediction.</p>

### Makpetrol (MPT)

<p>On the training set for Granit Stock we get RMSE with 97.14 and RMSE on validation set with 119.34. Other parameter that we measured was the coefficient of determination and on train we get 0.99 but on the validation we get 0.97. This calculation is done with Random Forest. Calculation with XGBoost was better on training set, but a little bit worse on validation set.</p>

<p align="center">
 <a href="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/makpetrol_regoressor.PNG"><img width="70%" height="70%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/makpetrol_regoressor.PNG"></a>
</p>

<p>In this case when we compare on training and validation we can conclude that Random Forest gives us better results than XGBoost. Forecast for 7th day or on 2020-09-01 with Random Forest is 64382.0 and with XGBoost is 64705.0. After a while, the real price was 62500.0 so Random Forest get better prediction with 2205.0 MKD greater than actual. This makes 0.04 error on prediction.</p>

<h3>using RNN with LSTM</h3>

<p>Another algorithm that we used was RNN many to many model with LSTM. On the input we lookback with 180 days and on the output we create 7 day sequence on close price. The model that we created has the following code:</p>

```python
model_many_to_many = Sequential()
model_many_to_many.add(LSTM(30, activation='relu', input_shape=(n_steps_in, n_features)))
model_many_to_many.add(RepeatVector(n_steps_out))
model_many_to_many.add(Dropout(0.2))
model_many_to_many.add(Dense(25))
model_many_to_many.add(Dense(1))
model_many_to_many.compile(optimizer='adam', loss='mse')
```

### Alkaliod (ALK)
<p>For Alkaloid training was finished in 2 epochs with smooth in both loss train and validation. Train RMSE for ALK is: 226.83 MKD and Validation RMSE for ALK is: 411.21 MKD. On the image below, we can check loss while the model was trained.</p>

<p align="center">
 <a href="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/alkaloid_loss_rnn.png"><img width="40%" height="40%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/alkaloid_loss_rnn.png"></a>
</p>

<p>Other mesure that we add was test set and how much loss we get. RMSE on the 7th day for ALK was 233.69 MKD or on the image below, we can check sequence of 7 days with actual and predicted prices on test set.</p>

<p align="center">
 <a href="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/alk_rnn_actual_predicted.png"><img width="40%" height="40%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/alk_rnn_actual_predicted.png"></a>
</p>

<p>In the end, we try to predict the price on 2020-09-01. The price for ALK after 7 days will be will be 12151.0 which make a difference of incredibly less 49 MKD. The actual price is 12200.0 which makes 0.004 error on prediction.</p>

### Granit (GRNT)

<p>For Granit training was finished in 4 epochs with smooth in both loss train and validation. Train RMSE for GRNT is: 40.44 MKD and Validation RMSE for GRNT is: 8.26 MKD. On the image below, we can check loss while the model was trained.</p>

<p align="center">
 <a href="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/granit_loss_rnn.png"><img width="40%" height="40%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/granit_loss_rnn.png"></a>
</p>

<p>Other mesure that we add was test set and how much loss we get. RMSE on the 7th day for GRNT is: 4.82 MKD or on the image bellow we can check sequence of 7 days with actual and predicted prices on test set.</p>

<p align="center">
 <a href="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/grnt_rnn_actual_predicted.png"><img width="40%" height="40%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/grnt_rnn_actual_predicted.png"></a>
</p>

<p>In the end, we try to predict the price on 2020-09-01. The price for GRNT after 7 days will be will be 949.0 which make a difference of negative 26 MKD. The actual price is 975.0 which makes 0.03 error in prediction.</p>

### Komercijalna Banka (KMB)

<p>About Komercijalna Banka training was finished in 3 epochs with smooth in both loss train and validation. Train RMSE for KMB is: 248.51 MKD and Validation RMSE for KMB is: 117.63 MKD. On the image bellow we can check loss while the model was trained.</p>

<p align="center">
 <a href="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/komercijalna_loss_rnn.png"><img width="40%" height="40%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/komercijalna_loss_rnn.png"></a>
</p>

<p>Other mesure that we add was test set and how much loss we get. RMSE on the 7th day for KMB is: 110.08 MKD or on the image bellow we can check sequence of 7 days with actual and predicted prices on test set.</p>

<p align="center">
 <a href="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/kmb_rnn_actual_predicted.png"><img width="40%" height="40%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/kmb_rnn_actual_predicted.png"></a>
</p>

<p>In the end, we try to predict the price on 2020-09-01. The price for KMB after 7 days will be will be 6252.0 which make a difference of incredible 47 MKD less. The actual price is 6299.0 which makes 0.007 error in prediction.</p>

### Makpetrol (MPT)

<p>About Makpetrol training was finished in 2 epochs with smooth in both loss train and validation. Train RMSE for MPT is: 2150.37 MKD and Validation RMSE for MPT is: 832.82 MKD. On the image bellow we can check loss while the model was trained.</p>

<p align="center">
 <a href="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/makpetrol_loss_rnn.png"><img width="40%" height="40%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/makpetrol_loss_rnn.png"></a>
</p>

<p>Other mesure that we add was test set and how much loss we get. RMSE on the 7th day for MPT is: 258.32 MKD or on the image bellow we can check sequence of 7 days with actual and predicted prices on test set.</p>

<p align="center">
 <a href="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/mpt_rnn_actual_predicted.png"><img width="40%" height="40%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/mpt_rnn_actual_predicted.png"></a>
</p>

<p>In the end, we try to predict the price on 2020-09-01. The price for KMB after 7 days will be will be 61660.0 which make a difference of incredibly 840 MKD less. The actual price is 62500.0 which makes 0.013 error in prediction.</p>

## Conclusions and recommendations

<p>As an overview of the algorithms that we used for this project all of them gave us very good results. Regressors like Random Forest and XGBoost get to the point where we can explain the forecast with creating features like ratios, time lags or some deviation through the time. Like this we understand which feature is most important about the forecast. Moreover we can see that quantity did not reflect a lot in the result by predicting the price.
On the other hand using RNN gave us flexibility to lookback in the past. After crating the model with LSTM they show their power and gave us much better results than regressors. Approximate error for seven days prediction was almost 1% which is incredible. We must mention here that in RNN with LSTM we used many to many model or sequence to sequence with 180 inputs and 7 outputs. This mean we predicted 7 days in a row but use only the last day.
As a recommendation from experience that we gain through this time-series project RNN with LSTM are much more powerful than regular regressors.</p>
  
</body>
</html>
