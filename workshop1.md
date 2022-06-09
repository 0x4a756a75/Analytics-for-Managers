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
data[["price", "points"]].corr() 
```
![Screenshot 2022-06-09 at 7 53 54 PM](https://user-images.githubusercontent.com/96379191/172841022-87e37bbd-f676-4cf8-a7cf-1e52fa4e7d7e.png)


```bash
import statsmodels.api as sm
import statsmodels.formula.api as smf

model = smf.ols(' points ~ price', data=data).fit()
print(model.summary()) # The size of the coefficient = 0.0309
```
![Screenshot 2022-06-09 at 7 54 32 PM](https://user-images.githubusercontent.com/96379191/172841196-ca2c9005-fa0e-471e-9430-75fc128c4d82.png)


```bash
model = smf.ols('points ~ price + desc_length + country + taster_name ', data=data).fit()
print(model.summary()) # The size of the coefficient = 0.0172
```

