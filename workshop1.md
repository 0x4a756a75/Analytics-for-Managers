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
```

- c) For the country that has the most expensive wine, how many wines are from that country and what is the
standard deviation of the price?

```bash

data.country.value_counts().reset_index(name='variety') # Switzerland	5
----------
country
Argentina                 24.579429
Armenia                   14.500000
Australia                 35.358701
Austria                   31.368817
Bosnia and Herzegovina    12.500000
Brazil                    24.500000
Bulgaria                  14.840909
Canada                    35.776786
Chile                     20.855480
China                     18.000000
Croatia                   25.529412
Cyprus                    16.500000
Czech Republic            22.363636
Egypt                           NaN
England                   52.677966
France                    41.593193
Georgia                   19.364865
Germany                   43.483325
Greece                    22.316038
Hungary                   42.234375
India                     13.750000
Israel                    31.770419
Italy                     40.046586
Lebanon                   30.281250
Luxembourg                23.400000
Macedonia                 15.636364
Mexico                    26.470588
Moldova                   16.732143
Morocco                   18.958333
New Zealand               26.850565
Peru                      18.062500
Portugal                  26.583725
Romania                   15.415842
Serbia                    24.875000
Slovakia                  16.000000
Slovenia                  25.542857
South Africa              24.332218
Spain                     28.363957
Switzerland               72.833333
Turkey                    25.037500
US                        36.757113
Ukraine                    9.214286
Uruguay                   26.326531
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
-----------------------------
data.groupby('country')['price'].std()
---------
country
Argentina                 23.187810
Armenia                    0.707107
Australia                 52.435820
Austria                   29.472353
Bosnia and Herzegovina     0.707107
Brazil                    11.042202
Bulgaria                   6.595938
Canada                    20.449658
Chile                     21.198849
Croatia                   13.424205
Cyprus                     3.195980
Czech Republic            10.623302
England                   15.173684
France                    75.019022
Georgia                    7.796330
Germany                   61.557461
Greece                    10.676626
Hungary                   79.685552
India                      3.488075
Israel                    19.139034
Italy                     37.576379
Lebanon                   16.675262
Luxembourg                 5.737305
Macedonia                  1.658312
Mexico                    18.629820
Moldova                   10.279268
Morocco                    5.973261
New Zealand               16.977601
Peru                      15.831260
Portugal                  42.251228
Romania                   12.424749
Serbia                    10.548358
Slovakia                        NaN
Slovenia                  14.532178
South Africa              21.522429
Spain                     35.511151
Switzerland               70.208262
Turkey                    14.280893
US                        25.421290
Ukraine                    1.810463
Uruguay                   20.424930
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
