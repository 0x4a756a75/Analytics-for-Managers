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

Appendix: Description of the columns
OrderId – Identifier of the order.
StoreId – Identifier of the store.
OrderPrice – Total gross value of the order in Euros.
DeliveryType – Type of delivery; 1 = Delivery, 2 = Take Out, 3 = Pick-up
OrdersInQueue – Number of orders in queue to be prepared at the moment the new order is received.
OvenProductsInQueue – Number of products in the queue that needs to go through the oven at the moment
the new order is received.
OrdersInOven – Number of orders in the oven at the moment the new order is received.
OrdersReadyForDispatch – Number of orders that are ready to get on the way at the moment the new order is
received.
DriversClockedIn – Number of drivers clocked in at the moment the new order is received.
DriversOnTheWay – Number of drivers on the road at the moment the new order is received.
ClockedInEmployees – Number of total employees clocked in, regardless of their role.
ReceivedTimestamp – The moment the new order is received by the store.
CompletedTimestamp – The moment the order is completed.
OriginalEstimatedDrivingTimeSeconds – The on beforehand estimated driving time, based on a Google API
call.
EstimatedOrderCompletionTime – The time in seconds that the restaurant estimated would take to complete
the order.
ActualOrderCompletionTime – The actual time in seconds the restaurant took to complete the order.

## Question 1: Multiple regression for prediction (10 points total, 6 points for part a, 4 points for part b)

- a) Using the following columns as predictors (OrderPrice, DeliveryType, OrdersInQueue,
OvenProductsInQueue, OrdersInOven, OrdersReadyForDispatch, DriversClockedIn,
DriversOnTheWay, ClockedInEmployees), train a multiple linear regression model to predict
ActualOrderCompletionTime. Use data_train.csv to fit the model, and also predict the
ActualOrderCompletionTime for each order in this dataset. What is the R-squared value of this model?
What is the mean absolute error (MAE) of your predictions? What is the root mean squared error
(RMSE) of your predictions? Consider EstimatedOrderCompletionTime as a benchmark, what MAE
and RMSE does this benchmark give? Are your model predictions better than this benchmark?

```bash
Code here soon
```

- b) Using the same predictors as part (a), train a new multiple linear regression model using all orders from
data_train.csv received before 2018-08-31 23:59:59, and evaluate your model using all orders from
data_train.csv after 2018-09-01 00:00:00. Report the MAE and RMSE of your model predictions, and
the MAE and RMSE of EstimatedOrderCompletionTime as a benchmark. 

```bash
Code here soon
```

## Question 2: Feature engineering (10 points total, 5 points for part a, 5 points for part b)

For this question, continue to train multiple linear regression models using all orders from data_train.csv
received before 2018-08-31 23:59:59, and evaluate your model using all orders from data_train.csv after 2018-
09-01 00:00:00

- a) Perform five modifications to the model from Question 1. Describe each modification (i.e., what you
did). Did any of the modification improve the MAE of your model predictions?

```bash
Code here soon
```


- b) Describe five modifications to the model from Question 1 that improved the MAE of your model
predictions. A modification is only considered an improvement if it improved the MAE of your model
predictions compared to the model without only this modification.

```bash
Code here soon
```

## Question 3: Prediction competition (9 points total)

- a) Let’s simulate how well your model(s) would perform had you deployed it on 2018-09-25. For this
question, please predict the ActualOrderCompletionTime of all orders in data_test.csv. Your predictions �
should be submitted as a .csv file, following the same format as that of sample_submission.csv. Your
submission file should have two columns, OrderId and PredictedOrderCompletionTime. Note that the
ActualOrderCompletionTime column for data_test.csv is not provided, but I do have it and will use it for
evaluating your predictions. Your predictions will be evaluated using the MAE metric (lower is better).
Keep in mind that you may perform any kind of modifications you want, and you may also use any
model(s) you like. However, you may only submit once and the MAE will be scored on that submission.
The grading for this question is based on your prediction performance as measured by MAE. The top
25% of the submissions will get 9 points, the next 25% will get 7 points, the third 25% will get 5 points,
and the bottom 25% will get 3 points. 

```bash
Code here soon
```

