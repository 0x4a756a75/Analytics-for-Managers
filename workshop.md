# Workshop 1

Please answer the questions below and submit the report for grading. Only one report with the names of all
group members needs to be submitted (can be submitted by either of you on Moodle). Please make sure to
submit a PDF containing your answers to the questions, and provide support for how you reached the answer in
case of ambiguity. The questions in total account for 29 points, you get 1 free point for handing the assignment
in. If you have any questions feel free to email me or ask in class.

All answers should be based on the winemag_ws1.csv dataset provided on Moodle.

Data description: This dataset contains prices and rating points of over 100,000 wines. Also included are
information about the wines, such as country/province/region of origin, title/description (in text format),
designation, variety, and winery. The name of the taster who gave the rating is also included. This dataset was
originally taken from WineEnthusiast magazine.

## Question 1: Exploring the dataset (1 point per sub question, 10 points total)

- a) How many wines are there in this dataset?

```bash
data.nunique() # 118840
```

- b) How many countries are represented in the dataset?

```bash
data.nunique() # 43
```

- c) How many tasters are there in the dataset?

```bash
data.nunique() # 19
```

- d) How many wine varieties are there in the dataset?

```bash
data.nunique() # 707
```

- e) What is the price of the cheapest wine in the dataset?

```bash
data.nsmallest(n=10, columns=['price']) # Lowest = 4
```

- f) What is the price of the most expensive wine in the dataset?

```bash
data.nlargest(n=10, columns=['price']) # Highest = 3300.0	
```

- g) How many points are earned by the lowest rated wine in the dataset?

```bash
data.nsmallest(n=10, columns=['points']) # Lowest = 80
```

- h) How many points are earned by the highest rated wine in the dataset?

```bash
data.nlargest(n=10, columns=['points']) # Highest = 100
```

- i) What is the title, price, and variety of the highest rated wine in the dataset? (choose one if multiple
wines share the highest points)

```bash
data.nlargest(n=10, columns=['points']) # title = Chambers Rosewood Vineyards NV Rare Muscat, price = 350.0, variety = Muscat
```

- j) What is the title, price, and variety of the lowest rated wine in the dataset? (choose one if multiple wines
share the lowest points)

```bash
data.nsmallest(n=10, columns=['points']) # title = Viña Tarapacá 2015 Gran Reserva Chardonnay, price = 9.0, variety = Chardonnay
```

##  Question 2: Getting insights from the data (1 point per sub question, 10 points total)

- a) Which country has the most number of wines?

```bash
data.groupby("country")["variety"].count().sort_values(ascending=False) # US 
```

- b) Which country has the most expensive wine based on mean price?

```bash
data.groupby("country")["price"].mean().nlargest() # Switzerland 72.833333  
```

- c) For the country that has the most expensive wine, how many wines are from that country and what is the
standard deviation of the price?

```bash
data.groupby("country")["variety"].count().sort_values(ascending=False) # 6
data.groupby("country")["price"].std() # 67.736007
```

- d) Which country has the cheapest wine based on mean price?

```bash
data.groupby("country")['price'].mean().nsmallest() # Ukraine 9.214286
```

- e) For the country that has the cheapest wine, how many wines are from that country and what is the
standard deviation of the price?

```bash
data.groupby("country")["variety"].count().sort_values(ascending=False) # 14
data.groupby("country")['price'].std() # 2.190138
```

- f) What is the 95% confidence interval of the mean price of wines from the most expensive country?

```bash
[18.634 – 127.032]
```

- g) What is the 95% confidence interval of the mean price of wines from the cheapest country?

```bash
[8.067 – 10.362]
```

- h) What is the average points and 95% confidence interval of wines tasted by Virginie?

```bash
data.groupby('taster_name')['points'].mean() # 89.220890
95% confidence interval # [89.157 – 89.285]
```

- i) What is the average points and 95% confidence interval of wines tasted by Roger?

```bash
data.groupby('taster_name')['points'].mean() # 88.726156
95% confidence interval # [88.686 – 88.766]
```

- j) Do Virginie and Roger give different ratings on average?

```bash
Yes # Roger and Virginie did not review any of the same wines, 88.686 < 89.157
```

## Question 3: Effect of price on rating points (9 points total)

- a) Using multiple regression, estimate the effect that price has on rating points. What is the size of the
coefficient? Is the effect statistically significant? For each dollar increase in price, how many rating
points are gained or lost on average? Include the following explanatory variables in your regression:
price, desc_length, country, and taster_name.

```bash
import pandas as pd
import numpy as np
data = pd.read_csv("winemag_ws1.csv")
%matplotlib inline
data.plot.scatter("points", "price") # More expensive, better rating points
```

![Screenshot 2022-06-09 at 7 53 25 PM](https://user-images.githubusercontent.com/96379191/172840989-33fd632f-025d-4954-b7e7-85fe8fab03f2.png)


```bash
data[["price", "points"]].corr() # Price =1.000000	> Points = 0.416954
```

```bash
import statsmodels.api as sm
import statsmodels.formula.api as smf

model = smf.ols(' points ~ price', data=data).fit()
print(model.summary()) # The size of the coefficient = 0.0309
```

```bash
model = smf.ols('points ~ price + desc_length + country + taster_name ', data=data).fit()
print(model.summary()) # The size of the coefficient = 0.0172
```


# Workshop 2

Please answer the questions below and submit the report for grading. Only one report with the names of all
group members needs to be submitted (can be submitted by either of you on Moodle). Please make sure to
submit a PDF containing your answers to the questions, and provide support for how you reached the answer in
case of ambiguity. Since Question 3 asks you to submit a separate .csv file, please submit both your report and
the .csv file for Question 3 to Moodle in a single zipped package. The questions in total account for 29 points,
you get 1 free point for handing the assignment in. If you have any questions feel free to email me or ask in
class. All answers should be based on the datasets provided.

Background: A European pizza chain wants to do a better job of communicating the expected order completion
time to their customers. Currently, when customers order a pizza online, the restaurant’s system outputs the
same amount of expected waiting time (depending on order type) regardless of the actual situation. As a result,
the restaurant may output 30 minutes as the expected waiting time even though the restaurant is very busy and
the order would actually take 50 minutes to complete. This fails to meet customer expectations and results in a
loss of goodwill. The pizza chain has asked you to design a predictive model to more accurately predict the time
required to complete the order, which they will then output for the customers when ordering.

Data description: This dataset contains pizza orders fulfilled by a European pizza chain. The data is split into
two files, data_train.csv and data_test.csv. The split is based on time, with data_train.csv containing orders from
2018-06-01 to 2018-09-24, and data_test.csv containing orders from 2018-09-25 to 2018-10-31. You can find a
description of the columns in the appendix on the next page.

| Variable | Description |
| --- | --- |
| OrderId | Identifier of the order. |
| StoreId | Identifier of the store. |
| OrderPrice | Total gross value of the order in Euros. |
| DeliveryType  |  Type of delivery; 1 = Delivery, 2 = Take Out, 3 = Pick-up |
| OrdersInQueue  |  Number of orders in queue to be prepared at the moment the new order is received. |
| OvenProductsInQueue  |  Number of products in the queue that needs to go through the oven at the moment the new order is received. |
| OrdersInOven  |  Number of orders in the oven at the moment the new order is received. |
| OrdersReadyForDispatch  |  Number of orders that are ready to get on the way at the moment the new order is received. |
| DriversClockedIn  |  Number of drivers clocked in at the moment the new order is received. |
| DriversOnTheWay  |  Number of drivers on the road at the moment the new order is received. |
| ClockedInEmployees  |  Number of total employees clocked in, regardless of their role. |
| ReceivedTimestamp  |  The moment the new order is received by the store. |
| CompletedTimestamp  |  The moment the order is completed. | 
| OriginalEstimatedDrivingTimeSeconds  |  The on beforehand estimated driving time, based on a Google API call. |
| EstimatedOrderCompletionTime  |  The time in seconds that the restaurant estimated would take to complete the order. |
| ActualOrderCompletionTime  |  The actual time in seconds the restaurant took to complete the order. |

## Question 1: Multiple regression for prediction (10 points total, 6 points for part a, 4 points for part b)

### - a) Using the following columns as predictors (OrderPrice, DeliveryType, OrdersInQueue, OvenProductsInQueue, OrdersInOven, OrdersReadyForDispatch, DriversClockedIn, DriversOnTheWay, ClockedInEmployees), train a multiple linear regression model to predict ActualOrderCompletionTime. Use data_train.csv to fit the model, and also predict the ActualOrderCompletionTime for each order in this dataset. What is the R-squared value of this model? What is the mean absolute error (MAE) of your predictions? What is the root mean squared error (RMSE) of your predictions? Consider EstimatedOrderCompletionTime as a benchmark, what MAE and RMSE does this benchmark give? Are your model predictions better than this benchmark?

```bash
import pandas as pd
import numpy as np

data_train_inputs = pd.read_csv("data_train.csv")

import statsmodels.api as sm
import statsmodels.formula.api as smf

model_train = smf.ols('ActualOrderCompletionTime ~ OrderPrice + DeliveryType +  OrdersInQueue +  \
                OvenProductsInQueue +  OrdersInOven +  OrdersReadyForDispatch +  DriversClockedIn \
                +  DriversOnTheWay +  ClockedInEmployees', data=data_train_inputs).fit()
print(model_train.summary()) # 1652.6303 sec. so around 27 minutes and 30 sec.
```
![Screenshot 2022-06-15 at 6 06 19 PM](https://user-images.githubusercontent.com/96379191/173802091-9c590639-2e07-4bd8-9dff-94683d04caa4.png)


```bash
predictions_model_train = model_train.predict(data_train_inputs)
data_train_inputs["PredictActualOrderCompletionTime"] = predictions_model_train 
data_train_inputs["PredictActualOrderCompletionTime"].mean() # 1792.5438166708318 so around 30 minutes
```


```bash
import matplotlib.pyplot as plt
%matplotlib inline
y_line = 1*np.linspace(0, 1600, 1600)
x_line = np.linspace(0, 1600, 1600)
fig, ax = plt.subplots(figsize=(8,5))
data_train_inputs.iloc[::1000].plot.scatter("PredictActualOrderCompletionTime", "ActualOrderCompletionTime", ax=ax) 
ax.plot(x_line, y_line, c="red")
```

![Screenshot 2022-06-15 at 7 41 19 PM](https://user-images.githubusercontent.com/96379191/173819020-3f190207-ea81-4a99-bc18-de82f392837e.png)


```bash
predictions = model_train.predict(data_train_inputs)
data_train_inputs["PredictActualOrderCompletionTime"] = predictions
data_train_inputs["error"] = data_train_inputs["ActualOrderCompletionTime"] - data_train_inputs["PredictActualOrderCompletionTime"]
data_train_inputs[["ActualOrderCompletionTime", "PredictActualOrderCompletionTime", "error"]].iloc[::20000]

data_train_inputs["error"].sum() # -8.71066004037857e-06
data_train_inputs["error"].mean() # -3.049062947926712e-11

data_train_inputs["error_abs"] = np.abs(data_train_inputs["error"])
data_train_inputs["error_squared"] = data_train_inputs["error"]**2
data_train_inputs[["error", "error_abs", "error_squared"]].describe()

rmse = np.sqrt(data_train_inputs["error_squared"].mean())
rmse # 1660.4688518530606

data_cheapest[["error_abs", "EstimatedOrderCompletionTime"]].describe()
```

```bash
data_train_inputs["benchmark"] = np.abs(data_train_inputs["ActualOrderCompletionTime"] - data_train_inputs["EstimatedOrderCompletionTime"])
data_train_inputs["benchmark"].sum() # 205957719.0
data_train_inputs["benchmark"].mean() # 730.797438845245
rmse = np.sqrt(data_train_inputs["benchmark"].mean())
rmse # 27.033265412177734 - Our model predication is better because than benchmark because close to 0 so close to red line
```



### - b) Using the same predictors as part (a), train a new multiple linear regression model using all orders from data_train.csv received before 2018-08-31 23:59:59, and evaluate your model using all orders from data_train.csv after 2018-09-01 00:00:00. Report the MAE and RMSE of your model predictions, and the MAE and RMSE of EstimatedOrderCompletionTime as a benchmark. 

#### Before 2018-08-31 23:59:59

```bash
data3 = data2[data2["ReceivedTimestamp"]<="2018-08-31"]
model3 = smf.ols('ActualOrderCompletionTime ~ OrderPrice + DeliveryType + OrdersInQueue + OvenProductsInQueue + OrdersInOven + OrdersReadyForDispatch + DriversClockedIn + DriversOnTheWay + ClockedInEmployees', data=data3).fit()
print(model3.summary())
---
predictions3 = model3.predict(data3)
data3["predicted_price3"] = predictions3
data3["predicted_price3"].mean() # 1761.3031669212153
data3["error"] = data3["ActualOrderCompletionTime"] - data3["predicted_price3"]
data3["error"].sum() # -2.8814247343689203e-06
data3["error"].mean() # -1.6318264865792765e-11
---
data3["error_abs"] = np.abs(data3["error"])
data3["error_squared"] = data3["error"]**2
rmse = np.sqrt(data3["error_squared"].mean())
rmse # 1479.0345830890592
---
data3["error_historical_avg"] = np.abs(data3["ActualOrderCompletionTime"] - data3["EstimatedOrderCompletionTime"])
data3["error_historical_avg"].mean() # 677.2355146806681

```
![Screenshot 2022-06-19 at 10 32 01 AM](https://user-images.githubusercontent.com/96379191/174463423-8613a78e-b4a1-4e3e-943b-9de08826d152.png)


#### After 2018-09-01 00:00

```bash
data4 = data2[data2["ReceivedTimestamp"]>="2018-09-01"]
model4 = smf.ols('ActualOrderCompletionTime ~ OrderPrice + DeliveryType + OrdersInQueue + OvenProductsInQueue + OrdersInOven + OrdersReadyForDispatch + DriversClockedIn + DriversOnTheWay + ClockedInEmployees', data=data4).fit()
print(model4.summary())
---
predictions4 = model4.predict(data4)
data4["predicted_price4"] = predictions4
data4["predicted_price4"].mean() # 1853.1150707592346
data4["error"] = data4["ActualOrderCompletionTime"] - data4["predicted_price4"]
data4["error"].sum() # 1.1130468919873238e-06
data4["error"].mean() # 1.104869693350235e-11
---
data4["error_abs"] = np.abs(data4["error"])
data4["error_squared"] = data4["error"]**2
rmse = np.sqrt(data4["error_squared"].mean())
rmse # 1946.8781933045764
---
data4["error_historical_avg"] = np.abs(data4["ActualOrderCompletionTime"] - data4["EstimatedOrderCompletionTime"])
data4["error_historical_avg"].mean() # 832.3758964685946
```
![Screenshot 2022-06-19 at 10 45 50 AM](https://user-images.githubusercontent.com/96379191/174463730-cca42f65-9a3b-4d8a-b06a-6ccf550730a3.png)



## Question 2: Feature engineering (10 points total, 5 points for part a, 5 points for part b)

For this question, continue to train multiple linear regression models using all orders from data_train.csv
received before 2018-08-31 23:59:59, and evaluate your model using all orders from data_train.csv after 2018-
09-01 00:00:00

### - a) Perform five modifications to the model from Question 1. Describe each modification (i.e., what you did). Did any of the modification improve the MAE of your model predictions?

```bash
Code here soon
```


### - b) Describe five modifications to the model from Question 1 that improved the MAE of your model predictions. A modification is only considered an improvement if it improved the MAE of your model predictions compared to the model without only this modification.

```bash
Code here soon
```

## Question 3: Prediction competition (9 points total)

### - a) Let’s simulate how well your model(s) would perform had you deployed it on 2018-09-25. For this question, please predict the ActualOrderCompletionTime of all orders in data_test.csv. Your predictions � should be submitted as a .csv file, following the same format as that of sample_submission.csv. Your submission file should have two columns, OrderId and PredictedOrderCompletionTime. Note that the ActualOrderCompletionTime column for data_test.csv is not provided, but I do have it and will use it for evaluating your predictions. Your predictions will be evaluated using the MAE metric (lower is better). Keep in mind that you may perform any kind of modifications you want, and you may also use any model(s) you like. However, you may only submit once and the MAE will be scored on that submission. The grading for this question is based on your prediction performance as measured by MAE. The top 25% of the submissions will get 9 points, the next 25% will get 7 points, the third 25% will get 5 points, and the bottom 25% will get 3 points. 

```bash
Code here soon
```


