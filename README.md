# Sales-Analysis Project Steps:
1. Create a Problem Statement
3. Identify the data you want to analyze.
4. Explore and Clean the data.
5. Analyze the data to get useful insights.
6. Present the data in terms of reports or dashboards using visualization.
7. Drop Unwanted / Blank columns
8. Finding Null Values
9. Check Data Type Each Column
    
# import python libraries
1.import numpy as np 

2.import pandas as pd 

3.import matplotlib.pyplot as plt # visualizing data
  %matplotlib inline
  
4.import seaborn as sns

# import csv file
df = pd.read_csv('Sales Analysis Data.csv', encoding= 'unicode_escape')

df.shape

df.head()

df.info()


# drop unrelated/blank columns
df.drop(['Status', 'unnamed1'], axis=1, inplace=True)

# check for null values
pd.isnull(df).sum()

User_ID              0

Cust_name            0

Product_ID           0

Gender               0

Age Group            0

Age                  0

Marital_Status       0

State                0

Zone                 0

Occupation           0

Product_Category     0

Orders               0

Amount              12

dtype: int64

# drop null values
df.dropna(inplace=True)

# change data type
df['Amount'] = df['Amount'].astype('int')

df['Amount'].dtypes

dtype('int32')

# rename column
df.rename(columns= {'Marital_Status':'Shaadi'})

# describe() method returns description of the data in the DataFrame (i.e. count, mean, std, etc)

df.describe()

User_ID	Age	Marital_Status	Orders	Amount

count	1.123900e+04	11239.000000	11239.000000	11239.000000	11239.000000

mean	1.003004e+06	35.410357	0.420055	2.489634	9453.610553

std	1.716039e+03	12.753866	0.493589	1.114967	5222.355168

min	1.000001e+06	12.000000	0.000000	1.000000	188.000000

25%	1.001492e+06	27.000000	0.000000	2.000000	5443.000000

50%	1.003064e+06	33.000000	0.000000	2.000000	8109.000000

75%	1.004426e+06	43.000000	1.000000	3.000000	12675.000000

max	1.006040e+06	92.000000	1.000000	4.000000	23952.000000

# ---"Exploratory Data Analysis"---

# Gender
# plotting a bar chart for Gender and it's count

ax = sns.countplot(x = 'Gender',data = df)

for bars in ax.containers:

    ax.bar_label(bars)

# plotting a bar chart for gender vs total amount

sales_gen = df.groupby(['Gender'], as_index=False)['Amount'].sum().sort_values(by='Amount', ascending=False)

sns.barplot(x = 'Gender',y= 'Amount' ,data = sales_gen)

<Axes: xlabel='Gender', ylabel='Amount'>

# "From above graphs we can see that most of the buyers are females and even the purchasing power of females are greater than men"

# Age
ax = sns.countplot(data = df, x = 'Age Group', hue = 'Gender')

for bars in ax.containers:

    ax.bar_label(bars)

# Total Amount vs Age Group

sales_age = df.groupby(['Age Group'], as_index=False)['Amount'].sum().sort_values(by='Amount', ascending=False)

sns.barplot(x = 'Age Group',y= 'Amount' ,data = sales_age)

<Axes: xlabel='Age Group', ylabel='Amount'>

# "From above graphs we can see that most of the buyers are of age group between 26-35 yrs female"

#State
# total number of orders from top 10 states

sales_state = df.groupby(['State'], as_index=False)['Orders'].sum().sort_values(by='Orders', ascending=False).head(10)

sns.set(rc={'figure.figsize':(15,5)})

sns.barplot(data = sales_state, x = 'State',y= 'Orders')

<Axes: xlabel='State', ylabel='Orders'>

# total amount/sales from top 10 states

sales_state = df.groupby(['State'], as_index=False)['Amount'].sum().sort_values(by='Amount', ascending=False).head(10)

sns.set(rc={'figure.figsize':(15,5)})

sns.barplot(data = sales_state, x = 'State',y= 'Amount')

<Axes: xlabel='State', ylabel='Amount'>

# "From above graphs we can see that most of the orders & total sales/amount are from Uttar Pradesh, Maharashtra and Karnataka respectively"

# Marital Status

ax = sns.countplot(data = df, x = 'Marital_Status')

sns.set(rc={'figure.figsize':(7,5)})

for bars in ax.containers:

    ax.bar_label(bars)

sales_state = df.groupby(['Marital_Status', 'Gender'], as_index=False)['Amount'].sum().sort_values(by='Amount', ascending=False)

sns.set(rc={'figure.figsize':(6,5)})

sns.barplot(data = sales_state, x = 'Marital_Status',y= 'Amount', hue='Gender')

<Axes: xlabel='Marital_Status', ylabel='Amount'>

# "From above graphs we can see that most of the buyers are married (women) and they have high purchasing power"

# Occupation
sns.set(rc={'figure.figsize':(20,5)})

ax = sns.countplot(data = df, x = 'Occupation')

for bars in ax.containers:

    ax.bar_label(bars)

sales_state = df.groupby(['Occupation'], as_index=False)['Amount'].sum().sort_values(by='Amount', ascending=False)

sns.set(rc={'figure.figsize':(20,5)})

sns.barplot(data = sales_state, x = 'Occupation',y= 'Amount')

<Axes: xlabel='Occupation', ylabel='Amount'>

# "From above graphs we can see that most of the buyers are working in IT, Healthcare and Aviation sector"

# Product Category

sns.set(rc={'figure.figsize':(20,5)})

ax = sns.countplot(data = df, x = 'Product_Category')

for bars in ax.containers:

    ax.bar_label(bars)

sales_state = df.groupby(['Product_Category'], as_index=False)['Amount'].sum().sort_values(by='Amount', ascending=False).head(10)

sns.set(rc={'figure.figsize':(20,5)})

sns.barplot(data = sales_state, x = 'Product_Category',y= 'Amount')

<Axes: xlabel='Product_Category', ylabel='Amount'>

# "From above graphs we can see that most of the sold products are from Food, Clothing and Electronics category"

sales_state = df.groupby(['Product_ID'], as_index=False)['Orders'].sum().sort_values(by='Orders', ascending=False).head(10)

sns.set(rc={'figure.figsize':(20,5)})

sns.barplot(data = sales_state, x = 'Product_ID',y= 'Orders')

<Axes: xlabel='Product_ID', ylabel='Orders'>

# top 10 most sold products (same thing as above)

fig1, ax1 = plt.subplots(figsize=(12,7))

df.groupby('Product_ID')['Orders'].sum().nlargest(10).sort_values(ascending=False).plot(kind='bar')

<Axes: xlabel='Product_ID'>

# Conclusion:
# "Married women age group 26-35 yrs from UP, Maharastra and Karnataka working in IT, Healthcare and Aviation are more likely to buy products from Food, Clothing and Electronics category"
