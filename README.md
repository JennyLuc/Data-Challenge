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
!()
## Step 4

### 1. What is the agreement rate between the engineer and all the raters for each day?
Separated agreement rate of each day of all the raters. 
```
(df[['Date Column','Three Label Agreement', 'Five Label Agreement']].groupby('Date Column')
 .mean().plot(title = 'Agreement Rate Separated', kind = 'bar'))
 ```
!(https://github.com/JennyLuc/Data-Challenge/graphs/agreement_rate_each_day_separated.png)
 
 Combined Agreement rate of each day of all the raters.
 
```
label_mean_df = df[['Date Column','Three Label Agreement', 'Five Label Agreement']].groupby('Date Column').mean()
total = label_mean_df.mean(axis =1)
total.plot(title = 'Agreement Rate', kind = 'bar')
```
!(https://github.com/JennyLuc/Data-Challenge/graphs/agreementrateeachday.png)
