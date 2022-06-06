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
data.nlargest(n=10, columns=['points']) # Multiple wines
```

- j) What is the title, price, and variety of the lowest rated wine in the dataset? (choose one if multiple wines
share the lowest points)

```bash
data.nsmallest(n=10, columns=['points']) # Multiple wines
```

##  Question 2: Getting insights from the data (1 point per sub question, 10 points total)

- a) Which country has the most number of wines?

```bash
data.country.value_counts().reset_index(name='variety') # US
----------
US                        42133
France                    16711
Italy                     15069
Spain                      5049
Portugal                   4424
Chile                      3503
Argentina                  2947
Austria                    2562
Australia                  1835
Germany                    1653
South Africa               1111
New Zealand                1086
Israel                      390
Greece                      354
Canada                      173
Bulgaria                    108
Hungary                     107
Romania                      81
Uruguay                      81
Turkey                       67
Slovenia                     64
Georgia                      62
Croatia                      59
Mexico                       57
England                      52
Brazil                       49
Moldova                      43
Lebanon                      30
Morocco                      23
Peru                         12
Czech Republic               11
Ukraine                       9
Macedonia                     9
Cyprus                        8
Serbia                        8
India                         6
Switzerland                   5
Luxembourg                    4
Armenia                       2
Bosnia and Herzegovina        2
Slovakia                      1
```

- b) Which country has the most expensive wine based on mean price?

```bash
data.groupby('country')['price'].mean().nlargest() # 
----------
Switzerland    72.833333
England        52.677966
Germany        43.483325
Hungary        42.234375
France         41.593193



----
country
Argentina                 24.184192
Armenia                   14.500000
Australia                 35.851381
Austria                   31.489272
Bosnia and Herzegovina    12.500000
Brazil                    24.500000
Bulgaria                  14.370370
Canada                    35.847953
Chile                     20.769876
Croatia                   25.929825
Cyprus                    16.250000
Czech Republic            22.363636
England                   52.187500
France                    41.177502
Georgia                   19.116667
Germany                   41.526802
Greece                    22.111748
Hungary                   43.943396
India                     13.166667
Israel                    31.649867
Italy                     39.601105
Lebanon                   29.933333
Luxembourg                22.750000
Macedonia                 15.666667
Mexico                    28.298246
Moldova                   17.837209
Morocco                   19.043478
New Zealand               26.913498
Peru                      18.916667
Portugal                  26.330437
Romania                   12.975309
Serbia                    24.875000
Slovakia                  16.000000
Slovenia                  25.315789
South Africa              24.379960
Spain                     28.251953
Switzerland               83.200000
Turkey                    24.417910
US                        36.462793
Ukraine                    9.555556
Uruguay                   27.851852
```

- c) For the country that has the most expensive wine, how many wines are from that country and what is the
standard deviation of the price?

```bash

data.country.value_counts().reset_index(name='variety') # Switzerland	5
data.groupby('country')['price'].std() # 72.833333
```

- d) Which country has the cheapest wine based on mean price?

```bash
data.groupby('country')['price'].mean().nsmallest()
----------
Ukraine                    9.214286
Bosnia and Herzegovina    12.500000
India                     13.750000
Armenia                   14.500000
Bulgaria                  14.840909
```

- e) For the country that has the cheapest wine, how many wines are from that country and what is the
standard deviation of the price?

```bash
data.country.value_counts().reset_index(name='variety') # Ukraine	9
data.groupby('country')['price'].std() # 1.810463
```

- f) What is the 95% confidence interval of the mean price of wines from the most expensive country?

```bash
Code here soon
```

- g) What is the 95% confidence interval of the mean price of wines from the cheapest country?

```bash
Code here soon
```

- h) What is the average points and 95% confidence interval of wines tasted by Virginie?

```bash
Code here soon
```

- i) What is the average points and 95% confidence interval of wines tasted by Roger?

```bash
Code here soon
```

- j) Do Virginie and Roger give different ratings on average?

```bash
Code here soon
```

## Question 3: Effect of price on rating points (9 points total)

- a) Using multiple regression, estimate the effect that price has on rating points. What is the size of the
coefficient? Is the effect statistically significant? For each dollar increase in price, how many rating
points are gained or lost on average? Include the following explanatory variables in your regression:
price, desc_length, country, and taster_name.

```bash
Code here soon
```
