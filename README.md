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

<p>There are missing values in date series because the stock market is functioanl only on working days during the week. Moreover some stocks are not traded every day. So we decided to make a series with all missing values as sequence in dates and complite daily prices for all stocks. We used pandas library and MultiIndex method to fulfil the series by stocks. Empthy prices of open,	high,	low,	average,	close,	volume and	quantity was fulfilled according to logic that close price will be the same as last traded day but also open, high, low and average because there is no fluctuation. Volume and quantity will be equal to zero.
After cleansing and filling data we can visual time series with all different stocks that we have but with logarithm of close pirce to show them in one range. We can catch the fall in 2009 but also in 2020 due to the pandemic. Also if there is streight line we can notice that the stock is not very often trade on the market.
</p>

<img width="75%" height="75%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/time_series_plot.png"></a></p>

<p>Other very important statistical indicatior is that only few of the stocks close price has lots of outliers and rest of them has normal increment during the period. We can check the statement on the box plot analysis below
<img width="75%" height="75%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/outliers_by_stock.png"></a></p>

<p>On the end we finished with 12 features of which last column start_date give us information about first date of trading with the initial stock.
<img width="75%" height="75%" src="https://github.com/gjurcin/MacedoniaStockMarket/blob/master/images/sample.png"></a></p>


## Feature Engineering

<p align="center"><a href="https://github.com/RistovaIvona/BankClassification/commit/fc01a98eaac76965e19f3df831610e07a148a7b3">
<img width="40%" height="40%" src="https://github.com/RistovaIvona/BankClassification/blob/master/documentation/data%20dist.png"></a></p>

<p> Dataset has unbalanced standard distribution ("Yes" - 12% and "No" - 88%). </p>

<p align="center">
<a href="https://github.com/RistovaIvona/BankClassification/commit/fc01a98eaac76965e19f3df831610e07a148a7b3"><img width="45%" height="45%" src="https://github.com/RistovaIvona/BankClassification/blob/master/documentation/pdf%20of%20age.png"></a>
<img width="45%" height="45%" src="https://github.com/RistovaIvona/BankClassification/blob/master/documentation/sub%20vs%20contact%20rate.png"></a>
</p>
 
<p>The majority of the costumers belong into group from 30 to 40 years. Considering the distribution of the class and the age-variable, we divided the customer in the different age-groups in order to make better analysis how age influance on the subscription rate. 
The bank was more effective with 20s and 60s aged group, which should be the next target. Considering that the term deposits are the most liquid and the most secure investment, the pattern is expected.The oldest aged group want to have cash and youngest do not have experience, knowledge and enough money for better and more sophisticated investments. On other hand, the 30s aged group have more loans and less money for savings.</p>

<p align="center"><a href="https://github.com/RistovaIvona/BankClassification/commit/fc01a98eaac76965e19f3df831610e07a148a7b3"><img width="45%" height="45%" src="https://github.com/RistovaIvona/BankClassification/blob/master/documentation/duration.png"></a></p>

<p> Also the "duration" (last contact duration) can be useful feature for predicting the target variable. It is expected because it is already mentioned in the data overview that this field highely affects the target variable and should only be used for benchmark purposes.</p>

<p align="center">
 <a href="https://github.com/RistovaIvona/BankClassification/commit/fc01a98eaac76965e19f3df831610e07a148a7b3"><img width="30%" height="30%" src="https://github.com/RistovaIvona/Bank-Marketing/blob/master/documentation/marital.png"></a>
 <a href="https://github.com/RistovaIvona/BankClassification/commit/fc01a98eaac76965e19f3df831610e07a148a7b3"><img width="28%" height="28%" src="https://github.com/RistovaIvona/Bank-Marketing/blob/master/documentation/education.png"></a>
 <a href="https://github.com/RistovaIvona/BankClassification/commit/fc01a98eaac76965e19f3df831610e07a148a7b3"><img width="30%" height="30%" src="https://github.com/RistovaIvona/Bank-Marketing/blob/master/documentation/housing.png"></a>
</p>

<p>These categorial fueatures (marital status, education and housing) have not impact on subscription rate. Exception from this conclusion is the feature education, because the subscription rate of the illiterate shows higher rate of subscription. This is expected because this group does not have knowledge for better and sophisticated kinds of investment.<p>

<p align="center"><a href="https://github.com/RistovaIvona/BankClassification/commit/fc01a98eaac76965e19f3df831610e07a148a7b3"><img width="60%" height="60%" src="https://github.com/RistovaIvona/BankClassification/blob/master/documentation/sub%20by%20month.png"></a>

<p>The bank’s contact rate and clients’ response rate in each month have diffrent directions. This can be interpret on two ways. Either the bank starts to contact the clients when the demand of deposits starts to decrease or that the bank has bad timing for contacting. The contact rate is the highest in may and august and on other hand, the highest subscription rate occured in march, september and october</p> 

<p>For more :<q>Build a future where people live in harmony with nature.</q></p>

<p align="center"><a href="https://github.com/RistovaIvona/BankClassification/commit/0582900d7e5cf230ac071cd8c1107a9a8b060483"><img width="60%" height="60%" src="https://github.com/RistovaIvona/BankClassification/blob/master/documentation/heatmap.png"></a>

<p>From the above heatmap we can see that there are some numerical features which share a high correlation between them, e.g nr.employed and euribor3m these features share a correlation value of 0.95, and euribor3m and emp.var.rate share a correlation of 0.97, which is very high compared to the other features that we see in the heatmap.

:star: For more visualization and relationships between the features read our file  <a href="https://github.com/RistovaIvona/BankClassification/blob/master/DataVisualization.ipynb"><b>"Data Visualization".</b></a>

## Models implementation 

<p>In order to be able to discuss the models and their accuracy first must be completed, the data processing, reprocessing, cleaning and data analysis. 
To make better results we compare some techniques and here we present the best of them.
Before we build and train models we make feature engineering task and split the data into train and test.
 
One very common step in any feature engineering task is converting categorical features into numerical. This is called encoding and although there are several encoding techniques, there’s one in particular that we use here — mean encoding. 
Unlike label encoding, which gets the work done efficiently but in a random way, mean encoding tries to approach the problem more logically. In a nutshell, it uses the target variable as the basis to generate the new encoded feature. So, this is the first part that we want to present something different from basic encoding, and we get better results.

The problem with this dataset is that we have imbalanced classification problem and we can not use simple split to dataset. The challenge of working with imbalanced datasets is that most machine learning techniques will ignore, and in turn have poor performance on, the minority class, although typically it is performance on the minority class that is most important. One way to solve this problem is to oversample the examples in the minority class. This can be achieved by simply duplicating examples from the minority class in the training dataset prior to fitting a model. This can balance the class distribution but does not provide any additional information to the model. The most widely used approach to synthesizing new examples is called the Synthetic Minority Oversampling Technique, or SMOTE for short. This technique is used in our dataset too and with its usage the results are better.  

Several models are defined and in the next picture, so we can see the progress and improvement of the accuracy of the models.</p>

<p align="center"><a href="https://github.com/RistovaIvona/BankClassification/commit/fc01a98eaac76965e19f3df831610e07a148a7b3"><img width="40%" height="40%" src="https://github.com/RistovaIvona/BankClassification/blob/master/documentation/models.png"></a></p>
 
<p>From what has been shown,<b>it is clear that Gradient Boosting and XGBoost give the best results.</b> For example, XGBoost derived from chain of tree based models and has it roots in the very first Decision Tree Model. Each model had improvements over the previous one, making XGBoost and Gradient Boost most advanced and sophisticated models so far.</p>

<p align="center"><a href="https://github.com/RistovaIvona/BankClassification/commit/fc01a98eaac76965e19f3df831610e07a148a7b3"><img width="50%" height="50%" src="https://github.com/RistovaIvona/BankClassification/blob/master/documentation/roc.png"></a>

<p>Both models have some predefined parameters and using them is not always best practice.
In our case the models with parameter adjustment give some slight improvement less than 0.2. Therefore, models with predefined parameters are used so that a trade-off can be made because the time spent on setup does not give us any significant improvements.</p>

</p>XGBoost gives us the best results but beside that there is always a way to get better results. Maybe we need more data or we need to modify what we have. The data science process never ends. In the picture below we present the final classification report from the selected best model. </p>

<p align="center"><a href="https://github.com/RistovaIvona/BankClassification/commit/000a3f1e33a793d9aca8660065f7c13883d67910">
<img width="40%" height="40%" src="https://github.com/RistovaIvona/BankClassification/blob/master/documentation/classification_report.PNG"></a></p>

:star: To see all the models that we have tried please read our file <a href="https://github.com/RistovaIvona/BankClassification/blob/master/Models.ipynb"><b>"Models"</b></a>.

## Conclusions and recommendations for future research

<p>
First when you get the datase, it is really important to read and investigate different kind of  papers with marketing analysis. These are links of papers that we have read and helped us: 
 <ul>
   <li><a href="http://media.salford-systems.com/video/tutorial/2015/targeted_marketing.pdf">http://media.salfordsystems.com/video/tutorial/2015/targeted_marketing.pdf</a></li>
   <li><a href="https://pdfs.semanticscholar.org/1999/417377ec21ecf7f7f55af62975065f785fb2.pdf">https://pdfs.semanticscholar.org/1999/417377ec21ecf7f7f55af62975065f785fb2.pdf</a></li>
   <li><a href="http://archive.ics.uci.edu/ml/datasets/Bank+Marketing#">http://archive.ics.uci.edu/ml/datasets/Bank+Marketing#</a></li>
   <li><a href="https://github.com/yfsui/Bank-Telemarketing-ML-Project/blob/master/Portuguese%20Bank%20Telemarketing%20Analysis.ipynb"></a>https://github.com/yfsui/Bank-Telemarketing-ML-Project/blob/master/Portuguese%20Bank%20Telemarketing%20Analysis.ipynb</li>
   <li><a href="https://github.com/naveen-chauhan/Loan-Prediction-Classification/blob/master/Loan%2BPrediction.ipynb">https://github.com/naveen-chauhan/Loan-Prediction-Classification/blob/master/Loan%2BPrediction.ipynb</a></li>
 </ul>

Second, when you will load the data you need to understand each attribute meaning. The meaning of the feature is essential to have view how the feature impact on the data and what you can expect.
Third, clean data. We decided to replace the 'unknown' data. There are so many preprocess ways to clean and it is your way  to decide what you will do, but you have to be sure that the data are good enough for modeling. For this data set preprocessing and feature understanding is the most important step.
And last, try diffrent kind of models and compare the results and time.
 
As George E. P. Box said: “all models are wrong, but some are useful”.
</p>

  
</body>
</html>
