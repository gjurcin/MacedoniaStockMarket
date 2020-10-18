<!DOCTYPE html>
<html>
 <head>
 </head>
<body>

<h2>Macedonian Stock Market Exchange Prediction</h2>

:star: Star us on GitHub — it helps!

<h3> Introduction </h3>

<small><p>
 <i>"Wall Street makes its money on activity, you make your money on inactivity." – Warren Buffett </i>
 
Inspired by the most famous dynamic market on earth, Wall Street, we started to analyze our domain stock market exchange in Macedonia. For most of the brokers stock market exchange is one of the biggest mystery and through the science still there is not found some algorithm which can predict some great depression. </p>

<p>This project is about forcasting stock prices of the most famous companies on Macedonian Stock Market Exchange. We will preprocess all data for 26 companies but after cleansing we will make prediction only on 4. Dataset is colected from 1997 till today.

The aim of the project is to build a model which will predict the future price.
The project is done in few phases:

- [Data Cleaning](#data-cleaning)
- [Feature Engineering](#feature-engineering)
- [Models implementation](#models-implementation)
- [Conclusion](#conclusion)

</small></p>


## Data Cleaning

<p>
There are missing values in date series because the stock market is functioanl only on working days during the week. Moreover some stocks are not traded every day. So we decided to make a series with all missing values as sequence in dates and complite daily prices for all stocks. We used pandas library and MultiIndex method to fulfil the series by stocks. Empthy prices of open,	high,	low,	average,	close,	volume and	quantity was fulfilled according to logic that close price will be the same as last traded day but also open, high, low and average because there is no fluctuation. Volume and quantity will be equal to zero.
After cleansing and filling data we can visual time series with all different stocks that we have but with logarithm of close pirce to show them in one range. We can catch the fall in 2009 but also in 2020 due to the pandemic. Also if there is streight line we can notice that the stock is not very often trade on the market.
</p>

<img width="75%" height="75%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/time_series_plot.png"></a></p>

<p>
Other very important statistical indicatior is that only few of the stocks close price has lots of outliers and rest of them has normal increment during the period. We can check the statement on the box plot analysis below

<img width="75%" height="75%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/outliers_by_stock.png"></a></p>

<p>
On the end we finished with 12 features of which last column start_date give us information about first date of trading with the initial stock.

<img width="75%" height="75%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/sample.png"></a>
</p>


## Feature Engineering

<p>Right of the bat, time-series daata is not yout average dataset. In all other datasets, the common thing is that all features, in general, are independent of each other. What sets time-series apart from regular, there is some inherent ordering to the data.</p>
<p>After loading the preprocessd dataset, we started to extract all possible feature from Timestamp column. Date column is already converted to datetime64[ns] type. Though, within extraction we found 14 new features. On the image bellow you can check the code of extraction.</p>

<img width="75%" height="40%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/date_fe.PNG"></a></p>

<h3>### Rolling windows</h3>

<p>One of the most important proccess that we done is creating features with time lags or adding some explanation features to target value such as mean of the lasy 7 days of the close price. Before all we set the Date column as index. Moreover in the rolling windows process we added 31 explainable features. Though, on the sample of exploring for one of the stocks we found lots of insights, which feature explain the most of the model. On the image bellow you can check the order. Most of the rolling windows are made on prices open, average and close but also ratios to explain growth by time.</p>

<p align="center">
 <a href="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/feature_importans.PNG"><img width="70%" height="70%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/feature_importans.PNG"></a>
</p>

## Models implementation

<h3>using regressors</h3>

<p>In order to be able to discuss the models and their loss first must be completed, the data processing, reprocessing, cleaning and data analysis. To make better results we compare some techniques and here we present the best of them.
Before we build and train models we make feature engineering task and split the data into train and test but also we shift the label column by 7 days so we can find the price after 7 days. Label column will be close price.

We used two ensamble regreossor and one RNN model with LSTM. If we talking about regoressors, fisrt one was Ransdom Forest and the second one was XGBoost. Before using them for different stocks also we made some grid search algorithms to find best hyperparametars. This tuning helped us to create surly prcise models beacuse we already create validation and test set from sample. Most important thing to mention here is creating new features to use labeled data with more than 50 features. We will show the results for 4 different stocks. </p>

### Alkaloid (ALK)

<p>After spliting the dataset by each stock, and also split on training and validation set we calculate the loss by root mean squared error. On training set we get RMSE with 6.9 and RMSE on validation set with 38.79. Other parametar that we mesure was coefficient of determination and on train we get 0.99 but on validation we get 0.89. It is obvious that when you predicting price with lots of uncertainty there will be some difference between traning and validation mesures. With XGBoost 0.15 RMSE on training set and 28.43 RMSE on validation set. R^2 on training set was 0.99 and on validation set 0.94.</p>

<p align="center">
 <a href="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/alkaloid_regoressor.PNG"><img width="70%" height="70%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/alkaloid_regoressor.PNG"></a>
</p>

<p>In this case when we compare on training and validation we can conclude that XGBosst give us better results than Randrom Forest. Forcast for 7th day or on 2020-09-01 with Random Forest is 11994.0 and with XGBoost is 11869.0. After a while the real price was 12200.0 so Random Forest get better predition with 206 MKD difference less than actual.</p>

### Granit (GRNT)

<p>On training set for Granit Stock we get RMSE with 1.95 and RMSE on validation set with 1.38. Other parametar that we mesure was coefficient of determination and on train we get 0.99 but on validation we get 0.94. This calculation is done with Random Forest. Calculation with XGBoost was little bit worse.</p>

<p align="center">
 <a href="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/granit_regoressor.PNG"><img width="70%" height="70%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/granit_regoressor.PNG"></a>
</p>

<p>In this case when we compare on training and validation we can conclude that Random Forest give us better results than XGBoost. Forcast for 7th day or on 2020-09-01 with Random Forest is 950.0 and with XGBoost is 952.0. After a while the real price was 975.0 so XGBoost get better predition with negative 23 MKD difference than actual.</p>


### Komercijalna Banka (KMB)

<p>On training set for Granit Stock we get RMSE with 9.30 and RMSE on validation set with 10.69. Other parametar that we mesure was coefficient of determination and on train we get 0.99 but on validation we get 0.91. This calculation is done with Random Forest. Calculation with XGBoost was better on traning set but little bit worse on validation set.</p>

<p align="center">
 <a href="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/kmb_regoressor.PNG"><img width="70%" height="70%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/kmb_regoressor.PNG"></a>
</p>

<p>In this case when we compare on training and validation we can conclude that Random Forest give us better results than XGBoost. Forcast for 7th day or on 2020-09-01 with Random Forest is 5891.0 and with XGBoost is 5886.0. After a while the real price was 6299.0 so Random FOrest get better predition with negative 408 MKD difference than actual.</p>

### Makpetrol (MPT)

<p>On training set for Granit Stock we get RMSE with 97.14 and RMSE on validation set with 119.34. Other parametar that we mesure was coefficient of determination and on train we get 0.99 but on validation we get 0.97. This calculation is done with Random Forest. Calculation with XGBoost was better on traning set but little bit worse on validation set.</p>

<p align="center">
 <a href="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/makpetrol_regoressor.PNG"><img width="70%" height="70%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/makpetrol_regoressor.PNG"></a>
</p>

<p>In this case when we compare on training and validation we can conclude that Random Forest give us better results than XGBoost. Forcast for 7th day or on 2020-09-01 with Random Forest is 64382.0 and with XGBoost is 64705.0. After a while the real price was 62500.0 so Random Forest get better predition with 2205.0 MKD grater than actual.</p>

<h3>using RNN with LSTM</h3>

## Conclusions and recommendations

  
</body>
</html>
