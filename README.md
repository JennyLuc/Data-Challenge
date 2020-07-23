# Data-Challenge
Hello! This is my submission for my take-home challenge. Answers can be found in this Readme file and also in the Jupyter Notebook named *Data Challenge Code.ipynb*

## Step 1
Creating the randomized data
```
dates = []
rater = []
correct_answer_3 = []
correct_answer_5 = []
three_label = ['Low', 'Average', 'High']
five_label = ['Bad', 'Okay', 'Intermediate', 'Great', 'Exceptional']

rater_answer_3 = []
rater_answer_5 = []
for i in range(10000):
    dates.append(datetime.date(2005, 10,random.randint(1,30)).strftime("%m-%d-%y"))
    rater.append(random.choice('ABCDE'))
    correct_answer_3.append(random.choice(three_label))
    correct_answer_5.append(random.choice(five_label))
    rater_answer_3.append(random.choice(three_label))
    rater_answer_5.append(random.choice(five_label))
    
df = pd.DataFrame()
df['Date Column'] = dates
df['Rater Column'] = rater
df['Correct Answer 3 Label'] = correct_answer_3
df['Correct Answer 5 Label'] = correct_answer_5
df['Rater Answer 3 Label'] = rater_answer_3
df['Rater Answer 5 Label'] = rater_answer_5
df['Task ID'] = range(1,10001)
```
## Step 2
```
df['Three Label Agreement'] = df['Correct Answer 3 Label'] == df['Rater Answer 3 Label']
df['Five Label Agreement'] = df['Correct Answer 5 Label'] == df['Rater Answer 5 Label']
```
### Resulting Table
Displays first 5 rows

Date Column	|Rater Column|	Correct Answer 3 Label	|Correct Answer 5 Label|	Rater Answer 3 Label|	Rater Answer 5 Label|	Task ID	|Three Label Agreement|	Five Label Agreement
------------ | ------------- |------------ | ------------- |------------ | ------------- |------------ | ------------- |------------
10-17-05|	B|	Average|	Intermediate|	Low|	Okay|	1|	False|	False
10-29-05|	A|	High|	Bad|	Average|	Bad|	2|	False|	True
10-21-05|	A|	Average|	Intermediate|	Average|	Exceptional|	3|	True|	False
10-17-05|	C|	Average|	Bad|	Low|	Intermediate|	4|	False|	False
10-01-05|	C|	Average|	Great|	Average|	Bad|	5|	True|	False
## Step 4

### 1. What is the agreement rate between the engineer and all the raters for each day?
Separated agreement rate of each day of all the raters. 
```
(df[['Date Column','Three Label Agreement', 'Five Label Agreement']].groupby('Date Column')
 .mean().plot(title = 'Agreement Rate Separated', kind = 'bar'))
 ```
![Agreement Rate Separated](https://github.com/JennyLuc/Data-Challenge/blob/master/graphs/agreement_rate_each_day_separated.png)
 
 Combined Agreement rate of each day of all the raters.
 
```
label_mean_df = df[['Date Column','Three Label Agreement', 'Five Label Agreement']].groupby('Date Column').mean()
total = label_mean_df.mean(axis =1)
total.plot(title = 'Agreement Rate', kind = 'bar')
```
Date | Agreement Rate
------------ | -------------
10-01-05 |   0.245714
10-02-05 |   0.229299
10-03-05 |   0.268595
10-04-05 |   0.263554
10-05-05 |   0.200608
10-06-05 |   0.254412
10-07-05 |   0.231322
10-08-05 |   0.282209
10-09-05 |   0.266369
10-10-05 |   0.287975
10-11-05 |   0.258462
10-12-05 |   0.258730
10-13-05 |   0.264946
10-14-05 |   0.242857
10-15-05 |   0.295588
10-16-05 |   0.247041
10-17-05 |   0.264610
10-18-05 |   0.280374
10-19-05 |   0.230435
10-20-05 |   0.292264
10-21-05 |   0.280186
10-22-05 |   0.253521
10-23-05 |   0.268987
10-24-05 |   0.263393
10-25-05 |   0.271307
10-26-05 |   0.267213
10-27-05 |   0.253918
10-28-05 |   0.224924
10-29-05 |   0.244475
10-30-05 |   0.246154
![Agreement Rate Combined](https://github.com/JennyLuc/Data-Challenge/blob/master/graphs/agreementrateeachday.png)

### 2. What is the agreement rate between the engineer and all the raters for each week?

#### Separated Ratings
```
week_dict = {'39': '10-01-05 to 10-02-05', '40': '10-03-05 to 10-09-05',
            '41': '10-10-05 to 10-16-05', '42': '10-17-05 to 10-23-05',
            '43': '10-24-05 to 10-30-05'}
week_dates = []
week = []
for i in df['Date Column'].astype('datetime64[ns]'):
    week.append(i.strftime('%V'))
    week_dates.append(week_dict[i.strftime('%V')])
weekly_df = df[['Three Label Agreement', 'Five Label Agreement']]
weekly_df['Week'] = week_dates
weekly_df.groupby('Week').mean().plot(title = 'Weekly Agreement Rate Separated', kind = 'bar')
```
Week |Three Label Agreement Rate|  Five Label Agreement Rate
------------ | ------------- |  -------------                                             
10-01-05 to 10-02-05 | 0.299699    |  0.176205
10-03-05 to 10-09-05 | 0.310447    |  0.194608
10-10-05 to 10-16-05 | 0.335779    |  0.194648
10-17-05 to 10-23-05 | 0.328874    |  0.205006
10-24-05 to 10-30-05 | 0.307990    |  0.198024

![seaparted_weekly_agreement_rate](https://github.com/JennyLuc/Data-Challenge/blob/master/graphs/weeklyagreementrate.png)

#### Combined Ratings
```
total_weekly = weekly_df.groupby('Week').mean()
weekly_bar = total_weekly.mean(axis = 1)
weekly_bar.plot(title= 'Total Agreement Rate', kind = 'bar')
```
Week | Agreement Rate
------------ | -------------
10-01-05 to 10-02-05 |   0.237952
10-03-05 to 10-09-05 |   0.252527
10-10-05 to 10-16-05 |   0.265214
10-17-05 to 10-23-05 |   0.266940
10-24-05 to 10-30-05 |   0.253007

![Combined Agreement Rating](https://github.com/JennyLuc/Data-Challenge/blob/master/graphs/test.png)

### 3. Identify raters that have the highest agreement rates with the engineer.
```
rater_agreement_df = df[['Rater Column', 'Three Label Agreement', 'Five Label Agreement']].groupby('Rater Column').mean()
agreement_rate = rater_agreement_df.mean(axis =1 )
```
Rater | Column
---------- | ----------
A  |  0.240399
B  |  0.260138
C  |  0.262266
D  |  0.260660
E  |  0.266288

On average, Rater E has the highest agreement rates with the engineers. However Rater B has the highest agreement rates for the three labels.

### 4. Identify raters that have the lowest agreement rates with the engineer.
Based on the previous analysis, we can see that the lower agreement rater is Rater A with 0.240.

### 5. What is the precision for each of the 5 labels?
```
precision = df[['Rater Answer 5 Label', 'Five Label Agreement']].groupby('Rater Answer 5 Label').mean()
precision.columns = ['Precision']
```

Rater Answer 5 Label | Precision	
---------- | ----------
Bad	|0.207081
Exceptional|0.188737
Great|0.197964
Intermediate|0.195233
Okay|0.193596

### 6. What is the recall for each of the 5 labels?
```
ratings_grouped = df[['Rater Answer 5 Label', 'Five Label Agreement']].groupby('Rater Answer 5 Label').sum()
recall_bad = (ratings_grouped.loc[(ratings_grouped.index == "Bad")]['Five Label Agreement'][0]/
              ratings_grouped['Five Label Agreement'].sum())

recall_exceptional = (ratings_grouped.loc[(ratings_grouped.index == "Exceptional")]['Five Label Agreement'][0]/
              ratings_grouped['Five Label Agreement'].sum())

recall_great = (ratings_grouped.loc[(ratings_grouped.index == "Great")]['Five Label Agreement'][0]/
              ratings_grouped['Five Label Agreement'].sum())

recall_intermediate = (ratings_grouped.loc[(ratings_grouped.index == "Intermediate")]['Five Label Agreement'][0]/
              ratings_grouped['Five Label Agreement'].sum())

recall_okay = (ratings_grouped.loc[(ratings_grouped.index == "Okay")]['Five Label Agreement'][0]/
              ratings_grouped['Five Label Agreement'].sum())
              
pd.DataFrame(index = ['Bad', 'Exceptional', 'Great', 'Intermediate', 'Okay'], 
             data = [recall_bad, recall_exceptional, recall_great, recall_intermediate, recall_okay ],
            columns = ['Recall'])
```	
Rater Answer 5 Label |Recall
---------- | ----------
Bad	|0.217192
Exceptional	|0.189217
Great	|0.197864
Intermediate	|0.195829
Okay	|0.199898

### 7. What is the recall for each of the 3 labels?
```
ratings_grouped_3 = df[['Rater Answer 3 Label', 'Three Label Agreement']].groupby('Rater Answer 3 Label').sum()
recall_average = (ratings_grouped_3.loc[(ratings_grouped_3.index == "Average")]['Three Label Agreement'][0]/
              ratings_grouped_3['Three Label Agreement'].sum())
recall_high = (ratings_grouped_3.loc[(ratings_grouped_3.index == "High")]['Three Label Agreement'][0]/
              ratings_grouped_3['Three Label Agreement'].sum())
recall_low = (ratings_grouped_3.loc[(ratings_grouped_3.index == "Low")]['Three Label Agreement'][0]/
              ratings_grouped_3['Three Label Agreement'].sum())
```

Three Label Agreement| Recall
---------- | ----------
Low	|0.333855
Average	|0.321954
High	|0.344190

### What approaches do you recommend you need to take to improve your metrics, if the metric has not met engineering standards?

Approaches that should be done is to ensure raters and engineers are on the same page on forming their results and conclusions. Because there is such a wide disparity between the ratings done by raters and ratings done by engineers, it shows that there are differences in how engineers and raters conclude their answers. 
Another idea for raters to have a higher matches of ratings is to have the engineers and raters collaboratively rate a couple of samples before splitting them off to rate separately.
Through this method, engineers are able to see each other's point of view which would help lead to smaller discrepancy in ratings. 
Some raters may perform better than others could be due to a myriad of factors like background and personal preferences.

## Step 5
1. What is the F1 score for each rater for 3-label ratings? For 5 label ratings?
2. What is the MCC score for each rater for 3-label ratings? For 5 label ratings?
3. What is the average rating made by raters for each, three label ratings and five label ratings, compared to the ratings made by engineers? Given that the ratings are numbered {Low: 1, Average: 2, High: 3}, and {Bad:1, Okay:2, Intermediate:3, Great:4, Exceptional:5}.

## Step 6
```
SELECT `Rater Column`, (AVG(`Three Label Agreement`)+AVG(`Five Label Agreement`))/2 
FROM df 
WHERE `Date Column` = '10-06-05' 
GROUP BY `Rater Column`
```
Rater Column | Agreement Rates
---------- | ----------
A       |  0.242857           
B       | 0.250000           
C       |   0.219697           
D       |   0.306667           
E       |     0.246377 
