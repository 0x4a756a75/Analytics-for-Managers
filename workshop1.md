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
- b) How many countries are represented in the dataset?
- c) How many tasters are there in the dataset?
- d) How many wine varieties are there in the dataset?

```bash
import pandas as pd
data = pd.read_csv("winemag_ws1.csv") # Import file
data.head() # Check header
data.nunique() # Get count for each header
----------
country                      43
description              118807
designation               37977
points                       21
price                       386
province                    425
region_1                   1229
region_2                     17
taster_name                  19
taster_twitter_handle        15
title                    118840
variety                     707
winery                    16757
desc_length                 576
dtype: int64
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
data.nlargest(n=10, columns=['points']) # title = Chambers Rosewood Vineyards NV Rare Muscat ,rice = 350.0, variety = Muscat
```

- j) What is the title, price, and variety of the lowest rated wine in the dataset? (choose one if multiple wines
share the lowest points)

```bash
data.nsmallest(n=10, columns=['points']) # title = Viña Tarapacá 2015 Gran Reserva Chardonnay ,rice = 9.0, variety = Chardonnay
```

##  Question 2: Getting insights from the data (1 point per sub question, 10 points total)

- a) Which country has the most number of wines?

```bash
data.groupby("country")["variety"].count().sort_values(ascending=False) # US 42133
```

- b) Which country has the most expensive wine based on mean price?

```bash
data.groupby("country")["price"].mean().nlargest() # Switzerland    72.833333
```

- c) For the country that has the most expensive wine, how many wines are from that country and what is the
standard deviation of the price?

```bash
data.groupby("country")["variety"].count().sort_values(ascending=False) # Switzerland	5
data.groupby("country")["price"].std() # 72.833333
```

- d) Which country has the cheapest wine based on mean price?

```bash
data.groupby("country")['price'].mean().nsmallest() # Ukraine 9.214286
```

- e) For the country that has the cheapest wine, how many wines are from that country and what is the
standard deviation of the price?

```bash
data.groupby("country")["variety"].count().sort_values(ascending=False) # Ukraine	9
data.groupby("country")['price'].std() # 1.810463
```

- f) What is the 95% confidence interval of the mean price of wines from the most expensive country?

```bash
Confidence Interval: 83.2 ±63.840 (±76.7%) [19.360 – 147.040]
```

- g) What is the 95% confidence interval of the mean price of wines from the cheapest country?

```bash
Confidence Interval: 9.555556 ±1.183 (±12.4%) [8.373 – 10.738]
```

- h) What is the average points and 95% confidence interval of wines tasted by Virginie?

```bash
data.groupby('taster_name')['points'].mean() # Virginie     89.222391
Confidence Interval: 89.222391 ±0.0692 (±0.08%) [89.153 – 89.292]
```

- i) What is the average points and 95% confidence interval of wines tasted by Roger?

```bash
data.groupby('taster_name')['points'].mean() # Roger        88.698220
Confidence Interval: 88.698220 ±0.0430 (±0.05%) [88.655 – 88.741]
```

- j) Do Virginie and Roger give different ratings on average?

```bash
Yes # Roger and Virginie did not review any of the same wines, 88.741 < 89.153
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
![Screenshot 2022-06-09 at 3 15 34 PM](https://user-images.githubusercontent.com/96379191/172788073-ee560e31-6872-457e-a0f6-e9179ffacf76.png)


```bash
data[["price", "points"]].corr() # For each dollar increase in price, 0.418126 rating points are gained or lost on average
```
![Screenshot 2022-06-09 at 3 20 04 PM](https://user-images.githubusercontent.com/96379191/172788462-fd15b57c-b199-48b3-9ddd-09a770da93fa.png)

```bash
import statsmodels.api as sm
import statsmodels.formula.api as smf

model = smf.ols(' points ~ price', data=data).fit()
print(model.summary()) # The size of the coefficient = 0.0314
```
![Screenshot 2022-06-09 at 3 21 22 PM](https://user-images.githubusercontent.com/96379191/172788697-792629ef-35cb-48b2-8765-04559446ca13.png)

```bash
model = smf.ols('points ~ price + desc_length + country + taster_name ', data=data).fit()
print(model.summary())
```
![Screenshot 2022-06-09 at 3 26 06 PM](https://user-images.githubusercontent.com/96379191/172789536-9a6e56c5-20ca-4753-82ac-c94a7863bbcb.png)

