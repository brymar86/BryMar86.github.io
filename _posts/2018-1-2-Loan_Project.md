---
layout: post
title: Titanic Post
date: 2017-11-27 04:00:00
tags: MachineLearning GradientBoost RandomForrest
author: Bryan Marty
---
## Clean Data 
- See Below

## Feature Creation:

1) Check Relationshiop and create Categorical Vector ST and LT loans
    - (Defaults as % of ST loans = A) => (Defaults as a % LT Loans = B) =>(No defaults = C)
    - Categroical Vector of A, B, C.
     
2) Check Relationshiop and Create feature for (monthly pmts/monthly income) 
    - We can create monthly income by way of dividing annual income by 12
    - Drop Monthly Pmts
    - Vector lenth of integer variables
3) Create new feature of (Current Credit Balance/ Annual Income).  
    - Essentially creating a personal debt to equity ratio feature for the individual.
    - Vector column that should be int/continuous in nature. 
    - Here we will need to drop dups before applying as this would be unqiue to one customer
   
    
### AFTER CLEAN AND FEATURE PORTION: 

1) IDEA ONE:  Split data frame into loan default and non default
    - Within defaut split the data again into train and test. 
    - We can fit an esemble model onto the data and predict on the default test 

2) IDEA 2:  Split data into two classes, one being train and one being test of entire data set
    - Predict Loan defaults with the entire data set and created feature columns

### We will observe results and apply the best results
   


```python
import pandas as pd
import numpy as np


df = pd.read_csv("LoansTrainingSet.csv")
print (df.shape)
df.head(2)

```

    (256984, 19)


    /anaconda2/lib/python2.7/site-packages/IPython/core/interactiveshell.py:2714: DtypeWarning: Columns (16) have mixed types. Specify dtype option on import or set low_memory=False.
      interactivity=interactivity, compiler=compiler, result=result)





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Loan ID</th>
      <th>Customer ID</th>
      <th>Loan Status</th>
      <th>Current Loan Amount</th>
      <th>Term</th>
      <th>Credit Score</th>
      <th>Years in current job</th>
      <th>Home Ownership</th>
      <th>Annual Income</th>
      <th>Purpose</th>
      <th>Monthly Debt</th>
      <th>Years of Credit History</th>
      <th>Months since last delinquent</th>
      <th>Number of Open Accounts</th>
      <th>Number of Credit Problems</th>
      <th>Current Credit Balance</th>
      <th>Maximum Open Credit</th>
      <th>Bankruptcies</th>
      <th>Tax Liens</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>000025bb-5694-4cff-b17d-192b1a98ba44</td>
      <td>5ebc8bb1-5eb9-4404-b11b-a6eebc401a19</td>
      <td>Fully Paid</td>
      <td>11520</td>
      <td>Short Term</td>
      <td>741.0</td>
      <td>10+ years</td>
      <td>Home Mortgage</td>
      <td>33694.0</td>
      <td>Debt Consolidation</td>
      <td>$584.03</td>
      <td>12.3</td>
      <td>41.0</td>
      <td>10</td>
      <td>0</td>
      <td>6760</td>
      <td>16056</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>00002c49-3a29-4bd4-8f67-c8f8fbc1048c</td>
      <td>927b388d-2e01-423f-a8dc-f7e42d668f46</td>
      <td>Fully Paid</td>
      <td>3441</td>
      <td>Short Term</td>
      <td>734.0</td>
      <td>4 years</td>
      <td>Home Mortgage</td>
      <td>42269.0</td>
      <td>other</td>
      <td>$1,106.04</td>
      <td>26.3</td>
      <td>NaN</td>
      <td>17</td>
      <td>0</td>
      <td>6262</td>
      <td>19149</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Lets inspect the data
print (df.info())
print(" ")
print("BREAK .    ")
print("    ")
print("Continuous & Categroical Check")
print(df.shape)
print(df.nunique())
print("Break .   ")
print("  ")
print("Check notnull ")
print(df.isnull().sum())
print("")
print("Colunms where NaN Exist")
print(df.isnull().any())

```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 256984 entries, 0 to 256983
    Data columns (total 19 columns):
    Loan ID                         256984 non-null object
    Customer ID                     256984 non-null object
    Loan Status                     256984 non-null object
    Current Loan Amount             256984 non-null int64
    Term                            256984 non-null object
    Credit Score                    195308 non-null float64
    Years in current job            245508 non-null object
    Home Ownership                  256984 non-null object
    Annual Income                   195308 non-null float64
    Purpose                         256984 non-null object
    Monthly Debt                    256984 non-null object
    Years of Credit History         256984 non-null float64
    Months since last delinquent    116601 non-null float64
    Number of Open Accounts         256984 non-null int64
    Number of Credit Problems       256984 non-null int64
    Current Credit Balance          256984 non-null int64
    Maximum Open Credit             256984 non-null object
    Bankruptcies                    256455 non-null float64
    Tax Liens                       256961 non-null float64
    dtypes: float64(6), int64(4), object(9)
    memory usage: 37.3+ MB
    None
     
    BREAK .    
        
    Continuous & Categroical Check
    (256984, 19)
    Loan ID                         215700
    Customer ID                     215700
    Loan Status                          2
    Current Loan Amount              27347
    Term                                 2
    Credit Score                       334
    Years in current job                11
    Home Ownership                       4
    Annual Income                    60558
    Purpose                             10
    Monthly Debt                    129115
    Years of Credit History            541
    Months since last delinquent       131
    Number of Open Accounts             59
    Number of Credit Problems           12
    Current Credit Balance           45704
    Maximum Open Credit              87188
    Bankruptcies                         8
    Tax Liens                           12
    dtype: int64
    Break .   
      
    Check notnull 
    Loan ID                              0
    Customer ID                          0
    Loan Status                          0
    Current Loan Amount                  0
    Term                                 0
    Credit Score                     61676
    Years in current job             11476
    Home Ownership                       0
    Annual Income                    61676
    Purpose                              0
    Monthly Debt                         0
    Years of Credit History              0
    Months since last delinquent    140383
    Number of Open Accounts              0
    Number of Credit Problems            0
    Current Credit Balance               0
    Maximum Open Credit                  0
    Bankruptcies                       529
    Tax Liens                           23
    dtype: int64
    
    Colunms where NaN Exist
    Loan ID                         False
    Customer ID                     False
    Loan Status                     False
    Current Loan Amount             False
    Term                            False
    Credit Score                     True
    Years in current job             True
    Home Ownership                  False
    Annual Income                    True
    Purpose                         False
    Monthly Debt                    False
    Years of Credit History         False
    Months since last delinquent     True
    Number of Open Accounts         False
    Number of Credit Problems       False
    Current Credit Balance          False
    Maximum Open Credit             False
    Bankruptcies                     True
    Tax Liens                        True
    dtype: bool


### Continous Variables Appear to be
 - Current loan amount
 - Annual Income
 - Monthly Debt
 - Current Credit Balance
 - Max Open Credit
 
### The rest appear Categorical or Identifiers 
    - "loan ID", "Customer ID" AND "# of open accounts" (for example)


```python
# Lets visualize the data a bit 
import seaborn as sns
sns.set(style= "ticks", palette = "pastel")

da = df[["Credit Score", "Annual Income", "Monthly Debt", "Current Credit Balance","Home Ownership","Maximum Open Credit","Years of Credit History",
        "Loan Status"]]
sns.boxplot(x = "Home Ownership", y = "Credit Score",
           hue = "Loan Status", palette = ["m", "g"], data = da)
sns.despine(offset = 10, trim = True)
```

## Clear indication of Data Cleaning required given above chart


```python
# Lets visualize the data a bit 
import seaborn as sns
sns.set(style= "ticks", palette = "pastel")

da = df[["Credit Score", "Annual Income", "Monthly Debt", "Current Credit Balance","Home Ownership","Maximum Open Credit","Years of Credit History",
        "Loan Status", "Number of Credit Problems", "Number of Open Accounts", "Purpose"]]
sns.boxplot(x = "Years of Credit History", y = "Purpose",
           hue = "Loan Status", palette = ["m", "g"], data = da)
sns.despine(offset = 10, trim = True)
```


![png](output_6_0.png)


### Below code needs to be Normalized. 



```python
# import seaborn as sns
# sns.set(style= "ticks")

# dx = df[["Current Loan Amount", "Number of Credit Problems", "Current Credit Balance", "Maximum Open Credit", "Monthly Debt", "Current Loan Amount",
#         "Loan Status"]]
# sns.pairplot(dx, hue = "Loan Status")
```

### Below graph did not work



```python
# import seaborn as sns
# import matplotlib.pyplot as plt

# sns.set(style="ticks")

# # Initialize the figure with a logarithmic x axis
# f, ax = plt.subplots(figsize = (9, 8))

# ax.set_xscale("log")

# #Plot the obital period with the horizontal boxes
# sns.boxplot(x = "Annual Income", y = "Home Ownership", data = df, whis= "range", palette= "vlag")

# #Add in points to show each observation
# sns.swarmplot(x = "Current Credit Balance", y = "Number of Credit Problems", data = df, size = 2, color = ".3", linewidth = 0)

# #Tweak the visual presentation
# ax.xaxis.grid(True)
# ax.set(ylabel="")
# sns.despine(trim = True, left = True)
```

### Data had Zeros and negative values thus log scale did not work. 


```python
df["Home Ownership"].unique()
```




    array(['Home Mortgage', 'Own Home', 'Rent', 'HaveMortgage'], dtype=object)



### Right Away:
- Replace "HaveMortgage" with "Home Mortgage"


```python
df["Home Ownership"][(df["Home Ownership"] == "HaveMortgage")].count()
```




    574




```python
df["Home Ownership"].replace("HaveMortgage", "Home Mortgage", inplace = True)
df["Home Ownership"][(df["Home Ownership"] == "HaveMortgage")].count()
```




    0




```python
df[0:2]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Loan ID</th>
      <th>Customer ID</th>
      <th>Loan Status</th>
      <th>Current Loan Amount</th>
      <th>Term</th>
      <th>Credit Score</th>
      <th>Years in current job</th>
      <th>Home Ownership</th>
      <th>Annual Income</th>
      <th>Purpose</th>
      <th>Monthly Debt</th>
      <th>Years of Credit History</th>
      <th>Months since last delinquent</th>
      <th>Number of Open Accounts</th>
      <th>Number of Credit Problems</th>
      <th>Current Credit Balance</th>
      <th>Maximum Open Credit</th>
      <th>Bankruptcies</th>
      <th>Tax Liens</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>000025bb-5694-4cff-b17d-192b1a98ba44</td>
      <td>5ebc8bb1-5eb9-4404-b11b-a6eebc401a19</td>
      <td>Fully Paid</td>
      <td>11520</td>
      <td>Short Term</td>
      <td>741.0</td>
      <td>10+ years</td>
      <td>Home Mortgage</td>
      <td>33694.0</td>
      <td>Debt Consolidation</td>
      <td>$584.03</td>
      <td>12.3</td>
      <td>41.0</td>
      <td>10</td>
      <td>0</td>
      <td>6760</td>
      <td>16056</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>00002c49-3a29-4bd4-8f67-c8f8fbc1048c</td>
      <td>927b388d-2e01-423f-a8dc-f7e42d668f46</td>
      <td>Fully Paid</td>
      <td>3441</td>
      <td>Short Term</td>
      <td>734.0</td>
      <td>4 years</td>
      <td>Home Mortgage</td>
      <td>42269.0</td>
      <td>other</td>
      <td>$1,106.04</td>
      <td>26.3</td>
      <td>NaN</td>
      <td>17</td>
      <td>0</td>
      <td>6262</td>
      <td>19149</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>



### More Visualization



```python
import matplotlib.pyplot as plt
import numpy as np

# Recall more bins allow syou to put more buckets into the graph
print (plt.hist((df["Annual Income"][df["Annual Income"] < 4000000]), bins = 100))

```

    (array([4.6110e+03, 4.7732e+04, 5.9860e+04, 3.9789e+04, 2.0797e+04,
           9.9950e+03, 5.3380e+03, 2.5210e+03, 1.6390e+03, 8.0500e+02,
           6.3000e+02, 4.0500e+02, 2.2300e+02, 2.6600e+02, 1.1300e+02,
           1.2300e+02, 5.5000e+01, 9.2000e+01, 4.2000e+01, 4.3000e+01,
           1.7000e+01, 3.9000e+01, 1.8000e+01, 1.3000e+01, 1.1000e+01,
           9.0000e+00, 1.6000e+01, 6.0000e+00, 9.0000e+00, 8.0000e+00,
           5.0000e+00, 1.0000e+01, 7.0000e+00, 7.0000e+00, 5.0000e+00,
           5.0000e+00, 0.0000e+00, 3.0000e+00, 5.0000e+00, 3.0000e+00,
           2.0000e+00, 2.0000e+00, 1.0000e+00, 0.0000e+00, 2.0000e+00,
           1.0000e+00, 1.0000e+00, 0.0000e+00, 1.0000e+00, 1.0000e+00,
           0.0000e+00, 1.0000e+00, 2.0000e+00, 2.0000e+00, 1.0000e+00,
           2.0000e+00, 1.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00,
           1.0000e+00, 1.0000e+00, 1.0000e+00, 0.0000e+00, 1.0000e+00,
           1.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00,
           0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00,
           0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00,
           0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00, 1.0000e+00,
           0.0000e+00, 0.0000e+00, 2.0000e+00, 0.0000e+00, 0.0000e+00,
           0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00,
           0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00, 1.0000e+00]), array([      0. ,   22675.7,   45351.4,   68027.1,   90702.8,  113378.5,
            136054.2,  158729.9,  181405.6,  204081.3,  226757. ,  249432.7,
            272108.4,  294784.1,  317459.8,  340135.5,  362811.2,  385486.9,
            408162.6,  430838.3,  453514. ,  476189.7,  498865.4,  521541.1,
            544216.8,  566892.5,  589568.2,  612243.9,  634919.6,  657595.3,
            680271. ,  702946.7,  725622.4,  748298.1,  770973.8,  793649.5,
            816325.2,  839000.9,  861676.6,  884352.3,  907028. ,  929703.7,
            952379.4,  975055.1,  997730.8, 1020406.5, 1043082.2, 1065757.9,
           1088433.6, 1111109.3, 1133785. , 1156460.7, 1179136.4, 1201812.1,
           1224487.8, 1247163.5, 1269839.2, 1292514.9, 1315190.6, 1337866.3,
           1360542. , 1383217.7, 1405893.4, 1428569.1, 1451244.8, 1473920.5,
           1496596.2, 1519271.9, 1541947.6, 1564623.3, 1587299. , 1609974.7,
           1632650.4, 1655326.1, 1678001.8, 1700677.5, 1723353.2, 1746028.9,
           1768704.6, 1791380.3, 1814056. , 1836731.7, 1859407.4, 1882083.1,
           1904758.8, 1927434.5, 1950110.2, 1972785.9, 1995461.6, 2018137.3,
           2040813. , 2063488.7, 2086164.4, 2108840.1, 2131515.8, 2154191.5,
           2176867.2, 2199542.9, 2222218.6, 2244894.3, 2267570. ]), <a list of 100 Patch objects>)



![png](output_18_1.png)


### Lets Clean More.  
- Given the data, after we clean we can Visualize better. 
- strings => integers and we can view


```python
df.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Current Loan Amount</th>
      <th>Credit Score</th>
      <th>Annual Income</th>
      <th>Years of Credit History</th>
      <th>Months since last delinquent</th>
      <th>Number of Open Accounts</th>
      <th>Number of Credit Problems</th>
      <th>Current Credit Balance</th>
      <th>Bankruptcies</th>
      <th>Tax Liens</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>2.569840e+05</td>
      <td>195308.000000</td>
      <td>1.953080e+05</td>
      <td>256984.000000</td>
      <td>116601.000000</td>
      <td>256984.000000</td>
      <td>256984.000000</td>
      <td>2.569840e+05</td>
      <td>256455.000000</td>
      <td>256961.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>1.371331e+07</td>
      <td>1251.116099</td>
      <td>7.195272e+04</td>
      <td>18.290195</td>
      <td>34.881450</td>
      <td>11.106267</td>
      <td>0.156628</td>
      <td>1.540656e+04</td>
      <td>0.110316</td>
      <td>0.027203</td>
    </tr>
    <tr>
      <th>std</th>
      <td>3.438131e+07</td>
      <td>1762.016848</td>
      <td>5.887757e+04</td>
      <td>7.075747</td>
      <td>21.854165</td>
      <td>4.982982</td>
      <td>0.460731</td>
      <td>1.966506e+04</td>
      <td>0.336229</td>
      <td>0.245950</td>
    </tr>
    <tr>
      <th>min</th>
      <td>5.050000e+02</td>
      <td>585.000000</td>
      <td>0.000000e+00</td>
      <td>3.400000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000e+00</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>8.299000e+03</td>
      <td>714.000000</td>
      <td>4.432100e+04</td>
      <td>13.500000</td>
      <td>16.000000</td>
      <td>8.000000</td>
      <td>0.000000</td>
      <td>5.974000e+03</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>1.429800e+04</td>
      <td>733.000000</td>
      <td>6.124200e+04</td>
      <td>17.000000</td>
      <td>32.000000</td>
      <td>10.000000</td>
      <td>0.000000</td>
      <td>1.107800e+04</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>2.436700e+04</td>
      <td>744.000000</td>
      <td>8.646200e+04</td>
      <td>21.700000</td>
      <td>51.000000</td>
      <td>14.000000</td>
      <td>0.000000</td>
      <td>1.931900e+04</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1.000000e+08</td>
      <td>7510.000000</td>
      <td>8.713547e+06</td>
      <td>70.500000</td>
      <td>176.000000</td>
      <td>76.000000</td>
      <td>11.000000</td>
      <td>1.731412e+06</td>
      <td>7.000000</td>
      <td>11.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
print(df.shape)
# We Have some duplicates here. 
df.duplicated(subset = ["Loan ID"]).sum()

```

    (256984, 19)





    41284




```python
# df["Loan ID"].value_counts()
```


```python
type(df.isnull().sum())
```




    pandas.core.series.Series



### Check which columns have null values and amount


```python
# variable thats a series where index rows labeled as colunm names  
# and the column names are comprised of a 
# boolean True or False of whether there are elements w/w/out Nan 
columns_null_values = df.isnull().sum()
# lets filter this series. Here is not null:
print (columns_null_values[columns_null_values == 0])
print(" ")
# Here is what we want. Null 
columns_null_values[columns_null_values != 0]
```

    Loan ID                      0
    Customer ID                  0
    Loan Status                  0
    Current Loan Amount          0
    Term                         0
    Home Ownership               0
    Purpose                      0
    Monthly Debt                 0
    Years of Credit History      0
    Number of Open Accounts      0
    Number of Credit Problems    0
    Current Credit Balance       0
    Maximum Open Credit          0
    dtype: int64
     





    Credit Score                     61676
    Years in current job             11476
    Annual Income                    61676
    Months since last delinquent    140383
    Bankruptcies                       529
    Tax Liens                           23
    dtype: int64



## Data Cleaning

- there appear to be duplicate entries in certain colunms
- Colunms with missing values include ***"Credit Score", "Years in Current Job", "Annual Income"
  "Months since las deliquent", "bankruptcies", and "tax liens"***
- We SHOULD expect Naans for bankruptcies, tax liens, and days since last delinquincy given the data.  

- We need to make decisions on how we will fill these. 
    - duplicate rows can be resolved by properly filling Nan's the best that we can. 
    - We may input a dummy variable or 0 for certain columns in areas where a 0 value indicates that "event did not occur" liken to a bankruptcy

### Bankruptcy clean NaN decision
- We will filter by defaults and no defaults 
- We will split df defaults and No defaults
- We will observe # of bankruptcies in both buckets
- We will fill with average defaults per bucket


```python
#This is what we are predicting 
df["Loan Status"].nunique()
```




    2




```python
# Splitting Data Frame from default and No default
df_no_default = df[(df["Loan Status"] == "Fully Paid")]
df_default = df[(df["Loan Status"] == "Charged Off")]
print(df_no_default.shape)
print(df_default.shape)

```

    (176191, 19)
    (80793, 19)



```python
# 68% more non defaults than defaults
print((float(176191)/(176191 + 80793)))
print (df_no_default["Bankruptcies"].count())
df_default["Bankruptcies"].count()
```

    0.685610777325
    175812





    80643




```python
df_default["Bankruptcies"].shape
```




    (80793,)




```python
print (len (df_default[(df_default["Bankruptcies"] >= 1)]))
print("")
print (df_default["Bankruptcies"][(df_default["Bankruptcies"] >= 1)].mean())
# Percentage with at least one BR in the default list
print("")
print(float(8408)/80793)


```

    8408
    
    1.05256898192
    
    0.104068421769



```python
df_default[(df_default["Bankruptcies"] >= 5)].count()
```




    Loan ID                         4
    Customer ID                     4
    Loan Status                     4
    Current Loan Amount             4
    Term                            4
    Credit Score                    2
    Years in current job            4
    Home Ownership                  4
    Annual Income                   2
    Purpose                         4
    Monthly Debt                    4
    Years of Credit History         4
    Months since last delinquent    2
    Number of Open Accounts         4
    Number of Credit Problems       4
    Current Credit Balance          4
    Maximum Open Credit             4
    Bankruptcies                    4
    Tax Liens                       4
    dtype: int64




```python
len (df_default[(df_default["Bankruptcies"] >= 1)])
```




    8408



### Default DataFrame.
 - appears that 10 % of DefauT df had bankruptcy
 - average number of bankrupcies of those that had them was a little over 1.  
 - The risk Distribution Tail has 4 people that have had 5 or greater defaults 
 - Given that the people in this DF have defaulted lets be safe and assign NaN values 1

# No Default DataFrame


```python
# A bit of Tail Risk
df_no_default[(df_no_default["Bankruptcies"] >= 5)].count()
```




    Loan ID                         15
    Customer ID                     15
    Loan Status                     15
    Current Loan Amount             15
    Term                            15
    Credit Score                    12
    Years in current job            12
    Home Ownership                  15
    Annual Income                   12
    Purpose                         15
    Monthly Debt                    15
    Years of Credit History         15
    Months since last delinquent    14
    Number of Open Accounts         15
    Number of Credit Problems       15
    Current Credit Balance          15
    Maximum Open Credit             15
    Bankruptcies                    15
    Tax Liens                       15
    dtype: int64




```python
df_no_default["Bankruptcies"][(df_no_default["Bankruptcies"] >= 5)].max()
```




    7.0




```python
print (len (df_no_default[(df_no_default["Bankruptcies"] >= 1)]))
print("")
# Percentage with at least one BR in the no default list
print (df_no_default["Bankruptcies"][(df_no_default["Bankruptcies"] >= 1)].mean())
print(" ")
print (float(18386)/176191)

```

    18386
    
    1.05738061569
     
    0.104352662735


### No Default DF.
 - appears that only 10 % of Non defaults had bankruptcy
 - average number of bankrupcies of those that had them was a little over 1.  
 - Even though there is only 10% with a bankruptcy, the right tail still does have 15 elements at 5 or greater.  Therefore we will fillNA here with 1 

### Fillna Bankruptcy and remerge DF



```python
df_default["Bankruptcies"].isnull().sum()
```




    150




```python
df_default["Bankruptcies"].fillna(1, inplace = True)
df_default["Bankruptcies"].isnull().sum()
```

    /anaconda2/lib/python2.7/site-packages/pandas/core/generic.py:5430: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      self._update_inplace(new_data)





    0




```python
df_no_default["Bankruptcies"].isnull().sum()
```




    379




```python
df_no_default["Bankruptcies"].fillna(1, inplace = True)
df_no_default["Bankruptcies"].isnull().sum()
```




    0



# NOTE!
- I put the dataFrame back together but should have just left it apart and cleaned both. 
- FYI in case this causes confusion.  
- For time purposes I did not want to redo anything I already did. 


```python
Bankruptcies = [df_no_default, df_default]
df_a = pd.concat(Bankruptcies, ignore_index = True)
df_a.shape
```




    (256984, 19)




```python
#Just revieweing the whole data frame again to ensure Accuracy
columns_null_values = df_a.isnull().sum()

print (columns_null_values[columns_null_values == 0])
print(" ")
# Here is what we want. Null 
columns_null_values[columns_null_values != 0]
```

    Loan ID                      0
    Customer ID                  0
    Loan Status                  0
    Current Loan Amount          0
    Term                         0
    Home Ownership               0
    Purpose                      0
    Monthly Debt                 0
    Years of Credit History      0
    Number of Open Accounts      0
    Number of Credit Problems    0
    Current Credit Balance       0
    Maximum Open Credit          0
    Bankruptcies                 0
    dtype: int64
     





    Credit Score                     61676
    Years in current job             11476
    Annual Income                    61676
    Months since last delinquent    140383
    Tax Liens                           23
    dtype: int64



# Lets Address Tax Liens


```python

df_a["Tax Liens"].isnull().sum()
```




    23




```python
df_a["Tax Liens"].nunique()

```




    12




```python
df_a["Tax Liens"].mean()
```




    0.027202571596467946




```python
df_a["Tax Liens"].describe()
```




    count    256961.000000
    mean          0.027203
    std           0.245950
    min           0.000000
    25%           0.000000
    50%           0.000000
    75%           0.000000
    max          11.000000
    Name: Tax Liens, dtype: float64



### Tax liens:
- They are such a small proportion of Naans + average is almost 0
- Lets fill with 0 


```python
df_a["Tax Liens"].fillna(0, inplace = True)
df_a.isnull().sum()
```




    Loan ID                              0
    Customer ID                          0
    Loan Status                          0
    Current Loan Amount                  0
    Term                                 0
    Credit Score                     61676
    Years in current job             11476
    Home Ownership                       0
    Annual Income                    61676
    Purpose                              0
    Monthly Debt                         0
    Years of Credit History              0
    Months since last delinquent    140383
    Number of Open Accounts              0
    Number of Credit Problems            0
    Current Credit Balance               0
    Maximum Open Credit                  0
    Bankruptcies                         0
    Tax Liens                            0
    dtype: int64




```python
# Lets just view a duplicate row to ensure we are doing the right thing. 
df_a[df_a["Loan ID"] == '56f59a9d-6b62-48d0-a121-82b64760fe6c']
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Loan ID</th>
      <th>Customer ID</th>
      <th>Loan Status</th>
      <th>Current Loan Amount</th>
      <th>Term</th>
      <th>Credit Score</th>
      <th>Years in current job</th>
      <th>Home Ownership</th>
      <th>Annual Income</th>
      <th>Purpose</th>
      <th>Monthly Debt</th>
      <th>Years of Credit History</th>
      <th>Months since last delinquent</th>
      <th>Number of Open Accounts</th>
      <th>Number of Credit Problems</th>
      <th>Current Credit Balance</th>
      <th>Maximum Open Credit</th>
      <th>Bankruptcies</th>
      <th>Tax Liens</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>203987</th>
      <td>56f59a9d-6b62-48d0-a121-82b64760fe6c</td>
      <td>0006b5d2-2fc7-41f8-85ca-8402226940bb</td>
      <td>Charged Off</td>
      <td>20812</td>
      <td>Long Term</td>
      <td>6530.0</td>
      <td>9 years</td>
      <td>Home Mortgage</td>
      <td>78352.0</td>
      <td>other</td>
      <td>$1,031.63</td>
      <td>15.1</td>
      <td>NaN</td>
      <td>20</td>
      <td>0</td>
      <td>11825</td>
      <td>19291</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>203988</th>
      <td>56f59a9d-6b62-48d0-a121-82b64760fe6c</td>
      <td>0006b5d2-2fc7-41f8-85ca-8402226940bb</td>
      <td>Charged Off</td>
      <td>20812</td>
      <td>Long Term</td>
      <td>653.0</td>
      <td>9 years</td>
      <td>Home Mortgage</td>
      <td>78352.0</td>
      <td>other</td>
      <td>$1,031.63</td>
      <td>15.1</td>
      <td>NaN</td>
      <td>20</td>
      <td>0</td>
      <td>11825</td>
      <td>19291</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
#This isn't Good.  Bad data. needs to be dropped and replaced
df_a["Credit Score"][(df_a["Credit Score"] >= 800)].count()
```




    16187




```python
df_a[0:2]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Loan ID</th>
      <th>Customer ID</th>
      <th>Loan Status</th>
      <th>Current Loan Amount</th>
      <th>Term</th>
      <th>Credit Score</th>
      <th>Years in current job</th>
      <th>Home Ownership</th>
      <th>Annual Income</th>
      <th>Purpose</th>
      <th>Monthly Debt</th>
      <th>Years of Credit History</th>
      <th>Months since last delinquent</th>
      <th>Number of Open Accounts</th>
      <th>Number of Credit Problems</th>
      <th>Current Credit Balance</th>
      <th>Maximum Open Credit</th>
      <th>Bankruptcies</th>
      <th>Tax Liens</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>000025bb-5694-4cff-b17d-192b1a98ba44</td>
      <td>5ebc8bb1-5eb9-4404-b11b-a6eebc401a19</td>
      <td>Fully Paid</td>
      <td>11520</td>
      <td>Short Term</td>
      <td>741.0</td>
      <td>10+ years</td>
      <td>Home Mortgage</td>
      <td>33694.0</td>
      <td>Debt Consolidation</td>
      <td>$584.03</td>
      <td>12.3</td>
      <td>41.0</td>
      <td>10</td>
      <td>0</td>
      <td>6760</td>
      <td>16056</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>00002c49-3a29-4bd4-8f67-c8f8fbc1048c</td>
      <td>927b388d-2e01-423f-a8dc-f7e42d668f46</td>
      <td>Fully Paid</td>
      <td>3441</td>
      <td>Short Term</td>
      <td>734.0</td>
      <td>4 years</td>
      <td>Home Mortgage</td>
      <td>42269.0</td>
      <td>other</td>
      <td>$1,106.04</td>
      <td>26.3</td>
      <td>NaN</td>
      <td>17</td>
      <td>0</td>
      <td>6262</td>
      <td>19149</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>



# Breaking apart DataFrame to fill NaN & Replace
- assumption is that defaulted loan matrix provides unique information relative to non-defaulted loan information


```python

no_default = df_a[(df_a["Loan Status"] == "Fully Paid")]
default = df_a[(df_a["Loan Status"] == "Charged Off")]
print(no_default.shape)
print(default.shape)

```

    (176191, 19)
    (80793, 19)


### Lets Check:
- Real Credit Score Stats of Default and No Default 


```python
# Lets Check median Credit Score now  between the two groups. 
print(" This is the Real Credit Score Median for folks that didnt default")
print (no_default["Credit Score"][(no_default["Credit Score"] <= 800)].median())
print(" ")
print (" ")
print("This is the Real Credit Score Median for Folks that did default")
print (default["Credit Score"][(default["Credit Score"] <= 800)].median())
```

     This is the Real Credit Score Median for folks that didnt default
    734.0
     
     
    This is the Real Credit Score Median for Folks that did default
    719.0


### Interesting.
- 734 vs 719


```python
no_default["Credit Score"][(no_default["Credit Score"] <= 800)].describe()
```




    count    133982.000000
    mean        726.231919
    std          24.347235
    min         585.000000
    25%         717.000000
    50%         734.000000
    75%         743.000000
    max         751.000000
    Name: Credit Score, dtype: float64




```python
print (plt.hist(no_default["Credit Score"][(no_default["Credit Score"] <= 800)], bins = 60))

```

    (array([   19.,    23.,    28.,    52.,    25.,    43.,    41.,    52.,
              47.,    72.,    69.,    90.,    73.,   100.,   122.,   126.,
             119.,    89.,   172.,   191.,   234.,   155.,   249.,   304.,
             354.,   281.,   430.,   509.,   539.,   342.,   636.,   647.,
             808.,   794.,   659.,  1019.,  1084.,  1125.,   882.,  1572.,
            1732.,  2033.,  1564.,  2457.,  2703.,  2892.,  3427.,  2591.,
            4144.,  4908.,  5503.,  4082.,  7199.,  8122.,  9374.,  7898.,
           13616., 12836., 14351.,  8374.]), array([585.        , 587.76666667, 590.53333333, 593.3       ,
           596.06666667, 598.83333333, 601.6       , 604.36666667,
           607.13333333, 609.9       , 612.66666667, 615.43333333,
           618.2       , 620.96666667, 623.73333333, 626.5       ,
           629.26666667, 632.03333333, 634.8       , 637.56666667,
           640.33333333, 643.1       , 645.86666667, 648.63333333,
           651.4       , 654.16666667, 656.93333333, 659.7       ,
           662.46666667, 665.23333333, 668.        , 670.76666667,
           673.53333333, 676.3       , 679.06666667, 681.83333333,
           684.6       , 687.36666667, 690.13333333, 692.9       ,
           695.66666667, 698.43333333, 701.2       , 703.96666667,
           706.73333333, 709.5       , 712.26666667, 715.03333333,
           717.8       , 720.56666667, 723.33333333, 726.1       ,
           728.86666667, 731.63333333, 734.4       , 737.16666667,
           739.93333333, 742.7       , 745.46666667, 748.23333333,
           751.        ]), <a list of 60 Patch objects>)



![png](output_65_1.png)



```python
default["Credit Score"][(default["Credit Score"] <= 800)].describe()
```




    count    45139.000000
    mean       710.143512
    std         31.244368
    min        585.000000
    25%        694.000000
    50%        719.000000
    75%        734.000000
    max        751.000000
    Name: Credit Score, dtype: float64




```python
print (plt.hist(default["Credit Score"][(default["Credit Score"] <= 800)], bins = 60))

```

    (array([  47.,   30.,   27.,   37.,   28.,   55.,   52.,   42.,   40.,
             70.,   78.,  121.,   58.,  108.,  153.,  155.,  141.,  140.,
            172.,  223.,  230.,  167.,  284.,  307.,  334.,  255.,  373.,
            402.,  461.,  350.,  545.,  558.,  660.,  572.,  520.,  822.,
            746.,  802.,  618.,  994., 1055., 1241.,  806., 1297., 1448.,
           1451., 1677., 1163., 1884., 2060., 2078., 1459., 2271., 2481.,
           2426., 1805., 2639., 2046., 1577.,  498.]), array([585.        , 587.76666667, 590.53333333, 593.3       ,
           596.06666667, 598.83333333, 601.6       , 604.36666667,
           607.13333333, 609.9       , 612.66666667, 615.43333333,
           618.2       , 620.96666667, 623.73333333, 626.5       ,
           629.26666667, 632.03333333, 634.8       , 637.56666667,
           640.33333333, 643.1       , 645.86666667, 648.63333333,
           651.4       , 654.16666667, 656.93333333, 659.7       ,
           662.46666667, 665.23333333, 668.        , 670.76666667,
           673.53333333, 676.3       , 679.06666667, 681.83333333,
           684.6       , 687.36666667, 690.13333333, 692.9       ,
           695.66666667, 698.43333333, 701.2       , 703.96666667,
           706.73333333, 709.5       , 712.26666667, 715.03333333,
           717.8       , 720.56666667, 723.33333333, 726.1       ,
           728.86666667, 731.63333333, 734.4       , 737.16666667,
           739.93333333, 742.7       , 745.46666667, 748.23333333,
           751.        ]), <a list of 60 Patch objects>)



![png](output_67_1.png)


### Interesting Results for Credit Score:
- Surprisingly high Credit Scores for defaults 
- fill na with 728 for no default
- Making subjective choice to Replace x > 800 and Nan with 700 for Default Matrix given the distribution.



```python
real_cs = default["Credit Score"][default["Credit Score"] <= 800]
{"Index": real_cs.index, "Credit Score": real_cs.values}
df_RealCS = pd.DataFrame({"Index": real_cs.index, "Credit Score": real_cs.values})

df_RealCS.set_index("Index", inplace = True)
df_RealCS[0:10]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Credit Score</th>
    </tr>
    <tr>
      <th>Index</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>176194</th>
      <td>701.0</td>
    </tr>
    <tr>
      <th>176195</th>
      <td>701.0</td>
    </tr>
    <tr>
      <th>176196</th>
      <td>701.0</td>
    </tr>
    <tr>
      <th>176197</th>
      <td>729.0</td>
    </tr>
    <tr>
      <th>176198</th>
      <td>729.0</td>
    </tr>
    <tr>
      <th>176200</th>
      <td>734.0</td>
    </tr>
    <tr>
      <th>176201</th>
      <td>740.0</td>
    </tr>
    <tr>
      <th>176202</th>
      <td>740.0</td>
    </tr>
    <tr>
      <th>176203</th>
      <td>623.0</td>
    </tr>
    <tr>
      <th>176204</th>
      <td>623.0</td>
    </tr>
  </tbody>
</table>
</div>



### But First to adjust Credit Scores > 800


```python
# Stuff Real Credit Scores into a variable
real_cs = default["Credit Score"][default["Credit Score"] <= 800]
# Create a DataFrame from a dictionary of the series. 
df_RealCS = pd.DataFrame({"Index": real_cs.index, "Credit Score": real_cs.values})
# Lets set Index 
df_RealCS.set_index("Index", inplace = True)

# Lets now pass the column credit score and say that is the df["Credit Score"]
default["Credit Score"] = df_RealCS["Credit Score"]
# Now we should have 16187 null values. 
df["Credit Score"].isnull().sum()
```

    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:9: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      if __name__ == '__main__':





    61676




```python
#lets fill it in with "700"
default["Credit Score"].fillna(700, inplace = True)
default["Credit Score"].isnull().sum()
```




    0



### Now to fill the no_default Matrix


```python
# Repeat this process with no_default
real_cs = no_default["Credit Score"][no_default["Credit Score"] <= 800]
# Create a DataFrame with a dicitonary
df_RealCS = pd.DataFrame({"Index": real_cs.index, "Credit Score": real_cs.values})
# Lets set Index 
df_RealCS.set_index("Index", inplace = True)
# Lets now pass the column credit score and say that is the df["Credit Score"]
no_default["Credit Score"] = df_RealCS["Credit Score"]
# Now we should have 16187 null values. 
no_default["Credit Score"].isnull().sum()
```

    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:8: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      





    42209




```python
# I guess there were no x > 800 credit scores. gah!
no_default["Credit Score"][no_default["Credit Score"] > 800]
```




    Series([], Name: Credit Score, dtype: float64)




```python
#Filling the NaN in no Default with 728
no_default["Credit Score"].fillna(728, inplace = True)
no_default["Credit Score"].isnull().sum()
```




    0




```python
print(default.shape)
print(no_default.shape)
```

    (80793, 19)
    (176191, 19)


### Lets Keep DataFrames Separate
- Lets fill Nan'S of the rest of the columns
- After we fill Nan's we can combine again. 
- After Nan's filled we can create features
- min max normalize variables
- Dummies on categoriclas
- cluster Data (credit Score) 



```python
no_default.head(2)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Loan ID</th>
      <th>Customer ID</th>
      <th>Loan Status</th>
      <th>Current Loan Amount</th>
      <th>Term</th>
      <th>Credit Score</th>
      <th>Years in current job</th>
      <th>Home Ownership</th>
      <th>Annual Income</th>
      <th>Purpose</th>
      <th>Monthly Debt</th>
      <th>Years of Credit History</th>
      <th>Months since last delinquent</th>
      <th>Number of Open Accounts</th>
      <th>Number of Credit Problems</th>
      <th>Current Credit Balance</th>
      <th>Maximum Open Credit</th>
      <th>Bankruptcies</th>
      <th>Tax Liens</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>000025bb-5694-4cff-b17d-192b1a98ba44</td>
      <td>5ebc8bb1-5eb9-4404-b11b-a6eebc401a19</td>
      <td>Fully Paid</td>
      <td>11520</td>
      <td>Short Term</td>
      <td>741.0</td>
      <td>10+ years</td>
      <td>Home Mortgage</td>
      <td>33694.0</td>
      <td>Debt Consolidation</td>
      <td>$584.03</td>
      <td>12.3</td>
      <td>41.0</td>
      <td>10</td>
      <td>0</td>
      <td>6760</td>
      <td>16056</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>00002c49-3a29-4bd4-8f67-c8f8fbc1048c</td>
      <td>927b388d-2e01-423f-a8dc-f7e42d668f46</td>
      <td>Fully Paid</td>
      <td>3441</td>
      <td>Short Term</td>
      <td>734.0</td>
      <td>4 years</td>
      <td>Home Mortgage</td>
      <td>42269.0</td>
      <td>other</td>
      <td>$1,106.04</td>
      <td>26.3</td>
      <td>NaN</td>
      <td>17</td>
      <td>0</td>
      <td>6262</td>
      <td>19149</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>



### Years in Current Job Feature


```python
#This is a string.
# Top result 10+ years 55,033 => 32% of data
print (no_default["Years in current job"].describe())
no_default["Years in current job"].isnull().sum()

```

    count        169547
    unique           11
    top       10+ years
    freq          55033
    Name: Years in current job, dtype: object





    6644




```python
print(no_default["Years in current job"][(no_default["Years in current job"] == "10+ years")].count())
print(no_default["Years in current job"][(no_default["Years in current job"] == "9 years")].count())

print(no_default["Years in current job"][(no_default["Years in current job"] == "8 years")].count())
print(no_default["Years in current job"][(no_default["Years in current job"] == "7 years")].count())
print(no_default["Years in current job"][(no_default["Years in current job"] == "6 years")].count())
print(no_default["Years in current job"][(no_default["Years in current job"] == "4 years")].count())
print(no_default["Years in current job"][(no_default["Years in current job"] == "2 years")].count())
print(no_default["Years in current job"][(no_default["Years in current job"] == "1 year")].count())
print(no_default["Years in current job"][(no_default["Years in current job"] == "< 1 year")].count())



```

    55033
    6696
    8323
    9529
    9960
    11220
    16349
    11606
    14230



```python
no_default["Years in current job"].unique()

```




    array(['10+ years', '4 years', '5 years', nan, '3 years', '2 years',
           '6 years', '1 year', '< 1 year', '7 years', '9 years', '8 years'],
          dtype=object)



### Years Fill
- 53% [6years <= x <= 10 +years]
- 31% [x <= 4 years]
- Since 10 years had a higher weight in these calculations and because 10+ could have multiple members with years much higher than 10 we will put the 
median to 9 years for the next calculation
- Avg/median distribution appears fairly close for 0-4 years so we will use 2 years.
- Decision simple weighted average: = 9(.53) + 2(.31) = 5.39
- 5.39(.15) = .809 . (this is the weight I didn't allocate for above)
- .809 + 5.39 = 6.2 . => fill with "6 years"


```python
no_default["Years in current job"].fillna("6 years", inplace = True)
no_default["Years in current job"].isnull().sum()

```




    0



### Lets do Default Column Fill


```python
#This is a string.
# Top result 10+ years 55,033 => 32% of data
print (default["Years in current job"].describe())
default["Years in current job"].isnull().sum()

```

    count         75961
    unique           11
    top       10+ years
    freq          23863
    Name: Years in current job, dtype: object





    4832




```python
default["Years in current job"].unique()

```




    array(['6 years', '< 1 year', '3 years', '10+ years', '5 years', '1 year',
           '2 years', '9 years', '7 years', nan, '4 years', '8 years'],
          dtype=object)




```python
print(default["Years in current job"][(default["Years in current job"] == "10+ years")].count())
print(default["Years in current job"][(default["Years in current job"] == "9 years")].count())

print(default["Years in current job"][(default["Years in current job"] == "8 years")].count())
print(default["Years in current job"][(default["Years in current job"] == "7 years")].count())
print(default["Years in current job"][(default["Years in current job"] == "6 years")].count())
print(default["Years in current job"][(default["Years in current job"] == "4 years")].count())
print(default["Years in current job"][(default["Years in current job"] == "2 years")].count())
print(default["Years in current job"][(default["Years in current job"] == "1 year")].count())
print(default["Years in current job"][(default["Years in current job"] == "< 1 year")].count())



```

    23863
    3236
    3883
    4439
    4637
    4946
    7113
    5140
    6782


### Default weights very similar.  
- Did Math off notebook.  Will fill with 6 years here too. 


```python
default["Years in current job"].fillna("6 years", inplace = True)
default["Years in current job"].isnull().sum()


```




    0



### Annual Income Fill 



```python
no_default["Annual Income"].isnull().sum()
```




    42209




```python
no_default["Annual Income"].describe()
```




    count    1.339820e+05
    mean     7.475413e+04
    std      5.619210e+04
    min      4.134000e+03
    25%      4.573400e+04
    50%      6.425250e+04
    75%      8.999800e+04
    max      7.523240e+06
    Name: Annual Income, dtype: float64




```python
no_default["Annual Income"].median()
```




    64252.5




```python
no_default["Annual Income"].std()
```




    56192.09776319183




```python
#Lets fill at 70k
no_default["Annual Income"].fillna(70000, inplace = True)
no_default["Annual Income"].isnull().sum()
```




    0




```python
print (plt.hist((no_default["Annual Income"][no_default["Annual Income"] < 4000000]), bins = 100))

```

    (array([5.5200e+03, 3.4265e+04, 8.1652e+04, 2.5444e+04, 1.3297e+04,
           7.1930e+03, 3.6050e+03, 1.8030e+03, 1.1540e+03, 5.9900e+02,
           4.7200e+02, 2.5400e+02, 1.9800e+02, 1.7700e+02, 9.4000e+01,
           9.7000e+01, 3.5000e+01, 7.9000e+01, 3.0000e+01, 3.7000e+01,
           1.4000e+01, 3.5000e+01, 1.3000e+01, 1.0000e+01, 7.0000e+00,
           1.2000e+01, 1.3000e+01, 4.0000e+00, 8.0000e+00, 6.0000e+00,
           6.0000e+00, 7.0000e+00, 5.0000e+00, 5.0000e+00, 3.0000e+00,
           3.0000e+00, 0.0000e+00, 2.0000e+00, 4.0000e+00, 1.0000e+00,
           1.0000e+00, 2.0000e+00, 1.0000e+00, 0.0000e+00, 2.0000e+00,
           1.0000e+00, 1.0000e+00, 0.0000e+00, 2.0000e+00, 0.0000e+00,
           0.0000e+00, 1.0000e+00, 3.0000e+00, 1.0000e+00, 1.0000e+00,
           0.0000e+00, 1.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00,
           1.0000e+00, 1.0000e+00, 1.0000e+00, 0.0000e+00, 1.0000e+00,
           1.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00,
           0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00,
           0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00,
           0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00, 1.0000e+00,
           0.0000e+00, 0.0000e+00, 2.0000e+00, 0.0000e+00, 0.0000e+00,
           0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00,
           0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00, 1.0000e+00]), array([   4134.  ,   26768.36,   49402.72,   72037.08,   94671.44,
            117305.8 ,  139940.16,  162574.52,  185208.88,  207843.24,
            230477.6 ,  253111.96,  275746.32,  298380.68,  321015.04,
            343649.4 ,  366283.76,  388918.12,  411552.48,  434186.84,
            456821.2 ,  479455.56,  502089.92,  524724.28,  547358.64,
            569993.  ,  592627.36,  615261.72,  637896.08,  660530.44,
            683164.8 ,  705799.16,  728433.52,  751067.88,  773702.24,
            796336.6 ,  818970.96,  841605.32,  864239.68,  886874.04,
            909508.4 ,  932142.76,  954777.12,  977411.48, 1000045.84,
           1022680.2 , 1045314.56, 1067948.92, 1090583.28, 1113217.64,
           1135852.  , 1158486.36, 1181120.72, 1203755.08, 1226389.44,
           1249023.8 , 1271658.16, 1294292.52, 1316926.88, 1339561.24,
           1362195.6 , 1384829.96, 1407464.32, 1430098.68, 1452733.04,
           1475367.4 , 1498001.76, 1520636.12, 1543270.48, 1565904.84,
           1588539.2 , 1611173.56, 1633807.92, 1656442.28, 1679076.64,
           1701711.  , 1724345.36, 1746979.72, 1769614.08, 1792248.44,
           1814882.8 , 1837517.16, 1860151.52, 1882785.88, 1905420.24,
           1928054.6 , 1950688.96, 1973323.32, 1995957.68, 2018592.04,
           2041226.4 , 2063860.76, 2086495.12, 2109129.48, 2131763.84,
           2154398.2 , 2177032.56, 2199666.92, 2222301.28, 2244935.64,
           2267570.  ]), <a list of 100 Patch objects>)



![png](output_98_1.png)


### Lets Fill Default now


```python
default["Annual Income"].isnull().sum()
```




    19467




```python
default["Annual Income"].describe()
```




    count    6.132600e+04
    mean     6.583232e+04
    std      6.393082e+04
    min      0.000000e+00
    25%      4.087200e+04
    50%      5.659650e+04
    75%      7.898400e+04
    max      8.713547e+06
    Name: Annual Income, dtype: float64




```python
default["Annual Income"].median()
```




    56596.5




```python
#The outliers are messing up the Standard Deviations in both cases
# This distribution is wider.  Also they make a little less here it appears
# Lets fill closer to the median.  58000 fillna

default["Annual Income"].fillna(58000, inplace = True)
default["Annual Income"].isnull().sum()
```




    0




```python
print (plt.hist((default["Annual Income"][default["Annual Income"] < 4000000]), bins = 100))

```

    (array([2.2300e+02, 2.8080e+03, 8.8370e+03, 1.3105e+04, 3.0576e+04,
           8.3460e+03, 5.4910e+03, 3.9550e+03, 2.2360e+03, 1.6060e+03,
           9.4300e+02, 7.5900e+02, 5.0900e+02, 3.0600e+02, 2.3800e+02,
           1.8100e+02, 1.1900e+02, 9.6000e+01, 6.2000e+01, 1.1300e+02,
           5.6000e+01, 2.0000e+01, 2.3000e+01, 3.3000e+01, 2.5000e+01,
           9.0000e+00, 1.4000e+01, 1.2000e+01, 1.0000e+01, 8.0000e+00,
           1.0000e+01, 1.5000e+01, 4.0000e+00, 1.0000e+00, 1.0000e+00,
           4.0000e+00, 1.0000e+00, 3.0000e+00, 2.0000e+00, 4.0000e+00,
           1.0000e+00, 0.0000e+00, 2.0000e+00, 1.0000e+00, 1.0000e+00,
           0.0000e+00, 0.0000e+00, 2.0000e+00, 1.0000e+00, 0.0000e+00,
           0.0000e+00, 2.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00,
           2.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00, 4.0000e+00,
           0.0000e+00, 0.0000e+00, 2.0000e+00, 2.0000e+00, 0.0000e+00,
           0.0000e+00, 0.0000e+00, 2.0000e+00, 0.0000e+00, 0.0000e+00,
           2.0000e+00, 0.0000e+00, 1.0000e+00, 0.0000e+00, 0.0000e+00,
           0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00,
           0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00,
           0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00,
           0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00,
           0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00, 2.0000e+00]), array([      0.  ,   12621.25,   25242.5 ,   37863.75,   50485.  ,
             63106.25,   75727.5 ,   88348.75,  100970.  ,  113591.25,
            126212.5 ,  138833.75,  151455.  ,  164076.25,  176697.5 ,
            189318.75,  201940.  ,  214561.25,  227182.5 ,  239803.75,
            252425.  ,  265046.25,  277667.5 ,  290288.75,  302910.  ,
            315531.25,  328152.5 ,  340773.75,  353395.  ,  366016.25,
            378637.5 ,  391258.75,  403880.  ,  416501.25,  429122.5 ,
            441743.75,  454365.  ,  466986.25,  479607.5 ,  492228.75,
            504850.  ,  517471.25,  530092.5 ,  542713.75,  555335.  ,
            567956.25,  580577.5 ,  593198.75,  605820.  ,  618441.25,
            631062.5 ,  643683.75,  656305.  ,  668926.25,  681547.5 ,
            694168.75,  706790.  ,  719411.25,  732032.5 ,  744653.75,
            757275.  ,  769896.25,  782517.5 ,  795138.75,  807760.  ,
            820381.25,  833002.5 ,  845623.75,  858245.  ,  870866.25,
            883487.5 ,  896108.75,  908730.  ,  921351.25,  933972.5 ,
            946593.75,  959215.  ,  971836.25,  984457.5 ,  997078.75,
           1009700.  , 1022321.25, 1034942.5 , 1047563.75, 1060185.  ,
           1072806.25, 1085427.5 , 1098048.75, 1110670.  , 1123291.25,
           1135912.5 , 1148533.75, 1161155.  , 1173776.25, 1186397.5 ,
           1199018.75, 1211640.  , 1224261.25, 1236882.5 , 1249503.75,
           1262125.  ]), <a list of 100 Patch objects>)



![png](output_104_1.png)



```python
default[0:2]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Loan ID</th>
      <th>Customer ID</th>
      <th>Loan Status</th>
      <th>Current Loan Amount</th>
      <th>Term</th>
      <th>Credit Score</th>
      <th>Years in current job</th>
      <th>Home Ownership</th>
      <th>Annual Income</th>
      <th>Purpose</th>
      <th>Monthly Debt</th>
      <th>Years of Credit History</th>
      <th>Months since last delinquent</th>
      <th>Number of Open Accounts</th>
      <th>Number of Credit Problems</th>
      <th>Current Credit Balance</th>
      <th>Maximum Open Credit</th>
      <th>Bankruptcies</th>
      <th>Tax Liens</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>176191</th>
      <td>0000afa6-8902-4f8f-b870-25a8fdad0aeb</td>
      <td>e49c1a82-a0f7-45e8-9f46-2f75c43f9fbc</td>
      <td>Charged Off</td>
      <td>24613</td>
      <td>Long Term</td>
      <td>700.0</td>
      <td>6 years</td>
      <td>Rent</td>
      <td>49225.0</td>
      <td>Business Loan</td>
      <td>$542.29</td>
      <td>17.6</td>
      <td>73.0</td>
      <td>7</td>
      <td>0</td>
      <td>14123</td>
      <td>16954</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>176192</th>
      <td>0000afa6-8902-4f8f-b870-25a8fdad0aeb</td>
      <td>e49c1a82-a0f7-45e8-9f46-2f75c43f9fbc</td>
      <td>Charged Off</td>
      <td>24613</td>
      <td>Long Term</td>
      <td>700.0</td>
      <td>6 years</td>
      <td>Rent</td>
      <td>58000.0</td>
      <td>Business Loan</td>
      <td>$542.29</td>
      <td>17.6</td>
      <td>73.0</td>
      <td>7</td>
      <td>0</td>
      <td>14123</td>
      <td>16954</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
no_default["Months since last delinquent"].shape
```




    (176191,)



### Months since last delinquent FILLNA
- Must be observant with this.  
- Many SHOULD HAVE 0 since many in this distribution have not defaulted
- 55% of values in this Vector have NaN values


```python
no_default["Months since last delinquent"].isnull().sum()
```




    97078




```python
no_default["Months since last delinquent"][(no_default["Months since last delinquent"].isnull() == True)].sum()
```




    0.0




```python
no_default["Months since last delinquent"].describe()
```




    count    79113.000000
    mean        35.235587
    std         21.728500
    min          0.000000
    25%         17.000000
    50%         32.000000
    75%         51.000000
    max        176.000000
    Name: Months since last delinquent, dtype: float64




```python
print (no_default["Months since last delinquent"][(no_default["Months since last delinquent"] >= 60)].count())
print (no_default["Months since last delinquent"][(no_default["Months since last delinquent"] <= 60)].count())

print("% of elements that had more than or equal to 60 months since last delinquency")
print (float(13726)/176191)
print(" ")
print("% of elements that had less than or equal to 60 months since last delinquency")
print (float(66100)/176191)
```

    13726
    66100
    % of elements that had more than or equal to 60 months since last delinquency
    0.0779040927176
     
    % of elements that had less than or equal to 60 months since last delinquency
    0.375161046819


### Decision
- Less than 10% have x >= 60 months since last delinquincy 
- We also fathom Many of these folks have never had a delinquincy. 
- A low fill here would also indicate a high number however given that there is not a lot data points related to no defaults with high days since 
last delinquency we can't be sure that will effect the results in the desired manner.
- Givent the data set has over half with NaN we are not going assume/pennalize them and fillna with zero 


```python
no_default["Months since last delinquent"].fillna(0, inplace = True)
no_default["Months since last delinquent"].isnull().sum()

```




    0



### Now to fill Default


```python
default.shape
```




    (80793, 19)




```python
default["Months since last delinquent"].isnull().sum()
```




    43305




```python
#The Defaults have a VERY DIFFERENT Data Set. 
float(4305)/80793
```




    0.0532843191860681




```python
default["Months since last delinquent"].describe()
```




    count    37488.000000
    mean        34.134096
    std         22.098695
    min          0.000000
    25%         15.000000
    50%         31.000000
    75%         50.000000
    max        152.000000
    Name: Months since last delinquent, dtype: float64




```python
print (default["Months since last delinquent"][(default["Months since last delinquent"] >= 31)].count())
print (default["Months since last delinquent"][(default["Months since last delinquent"] <= 31)].count())


```

    18790
    19233



```python
print("% of elements that had more than or equal to 31 months since last delinquency")
print (float(18790)/80793)
print(" ")
print("% of elements that had less than or equal to 31 months since last delinquency")
print (float(19233)/80793)
```

    % of elements that had more than or equal to 31 months since last delinquency
    0.232569653312
     
    % of elements that had less than or equal to 31 months since last delinquency
    0.238052801604


### Decision
- With only 5% NAN in this data its very different than no_default
- We could put Nan to zero or leverage the Median/mean.  given the data is so lower on all terms for months since delinquency 
it seems reasonable that this is a different data set.  
- In efforts to mimic the characteristics of this data we will fill with the Mean though could have just as easily put 0.
- This will aid us with like values especialy if we split the Data set and only use the defaults data to predict on


```python
default["Months since last delinquent"].fillna(34, inplace = True)
default["Months since last delinquent"].isnull().sum()
```




    0




```python
default.isnull().sum()
```




    Loan ID                         0
    Customer ID                     0
    Loan Status                     0
    Current Loan Amount             0
    Term                            0
    Credit Score                    0
    Years in current job            0
    Home Ownership                  0
    Annual Income                   0
    Purpose                         0
    Monthly Debt                    0
    Years of Credit History         0
    Months since last delinquent    0
    Number of Open Accounts         0
    Number of Credit Problems       0
    Current Credit Balance          0
    Maximum Open Credit             0
    Bankruptcies                    0
    Tax Liens                       0
    dtype: int64



### Feature 1:
-Make categorical vector for Short Term and long Term Debt
- Default Short Term = 2 
- Default Long Term = 1
- No Default = 0


```python
print (default["Term"][(default["Term"] == "Short Term")].count())
print (default["Term"][(default["Term"] == "Long Term")].count())
```

    51060
    29733



```python
# BOOM! Replace String with Integer
print (default["Term"][(default["Term"] == "Short Term")].replace("Short Term", 1)[0:5])
print(" ")
# And now with 2
print(default["Term"][(default["Term"] == "Long Term")].replace("Long Term", 2)[0:5])
print(" ")


```

    176193    1
    176194    1
    176195    1
    176196    1
    176197    1
    Name: Term, dtype: int64
     
    176191    2
    176192    2
    176199    2
    176200    2
    176203    2
    Name: Term, dtype: int64
     


### Lets address the No Default Colunm


```python
# I'm trying to do this without using replace and splitting no_default series into two data frames and concating them again. 
a = []
for i in no_default["Term"]:
    if i == "Short Term":
        a.append(0)
    elif i == "Long Term":
        a.append(0)
#set the list as a variable
column = pd.Series(a)
#name column
column.rename("Term")
column[0:4]
```




    0    0
    1    0
    2    0
    3    0
    dtype: int64




```python
#Use the dictionary to create a data frame for a single vector 
df_term = pd.DataFrame({"Index": column.index, "Term": column.values})
df_term.Term[0:10]
```




    0    0
    1    0
    2    0
    3    0
    4    0
    5    0
    6    0
    7    0
    8    0
    9    0
    Name: Term, dtype: int64




```python
#Now set the relevaen Vector = to the original vector of interest
#no_default["Term"] = df_term["term"] replaces st,lt string elements with 0 elements from the df_term["term"]
no_default["Term"] = df_term["Term"]
print(no_default["Term"].isnull().sum())
print(" ")
print(no_default["Term"].nunique())
no_default.head(2)
#Because we were practicing on this DF we have fillna with 0. (this wouldn't be necessary)
no_default["Term"].fillna(0, inplace = True)
print(no_default["Term"].isnull().sum())

```

    0
     
    1
    0


    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:3: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      This is separate from the ipykernel package so we can avoid doing imports until


### Graph Jose

### Feature Creation 2: 
monthly debt/monthly income:
 


```python
# create monthly income variable stuffing the Annual Income Vector into. 
column = default["Annual Income"]
# Dividing the Annual income by 12 and callint it "monthly income"
default["Monthly Income"] = map(lambda x: x/12, column)
# Lets check it out
print (default["Monthly Income"].shape)
print (default["Monthly Income"].isnull().sum())
print (default["Monthly Income"][0:10])

```

    (80793,)
    0
    176191    4102.083333
    176192    4833.333333
    176193    4833.333333
    176194    4421.083333
    176195    4421.083333
    176196    4426.000000
    176197    2942.916667
    176198    2942.916667
    176199    4833.333333
    176200    3359.000000
    Name: Monthly Income, dtype: float64


    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:4: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      after removing the cwd from sys.path.



```python
default.dtypes
```




    Loan ID                          object
    Customer ID                      object
    Loan Status                      object
    Current Loan Amount               int64
    Term                             object
    Credit Score                    float64
    Years in current job             object
    Home Ownership                   object
    Annual Income                   float64
    Purpose                          object
    Monthly Debt                     object
    Years of Credit History         float64
    Months since last delinquent    float64
    Number of Open Accounts           int64
    Number of Credit Problems         int64
    Current Credit Balance            int64
    Maximum Open Credit              object
    Bankruptcies                    float64
    Tax Liens                       float64
    Monthly Income                  float64
    Monthly Debt/Equity             float64
    Total Debt/Annual Income        float64
    dtype: object




```python
pd.to_numeric(default["Monthly Income"], errors='coerce')[0:2]
```




    176191    4102.083333
    176192    4833.333333
    Name: Monthly Income, dtype: float64




```python
# interesting look here
default.iloc[0:10, [8,19]]


```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Annual Income</th>
      <th>Monthly Income</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>176191</th>
      <td>49225.0</td>
      <td>4102.083333</td>
    </tr>
    <tr>
      <th>176192</th>
      <td>58000.0</td>
      <td>4833.333333</td>
    </tr>
    <tr>
      <th>176193</th>
      <td>58000.0</td>
      <td>4833.333333</td>
    </tr>
    <tr>
      <th>176194</th>
      <td>53053.0</td>
      <td>4421.083333</td>
    </tr>
    <tr>
      <th>176195</th>
      <td>53053.0</td>
      <td>4421.083333</td>
    </tr>
    <tr>
      <th>176196</th>
      <td>53112.0</td>
      <td>4426.000000</td>
    </tr>
    <tr>
      <th>176197</th>
      <td>35315.0</td>
      <td>2942.916667</td>
    </tr>
    <tr>
      <th>176198</th>
      <td>35315.0</td>
      <td>2942.916667</td>
    </tr>
    <tr>
      <th>176199</th>
      <td>58000.0</td>
      <td>4833.333333</td>
    </tr>
    <tr>
      <th>176200</th>
      <td>40308.0</td>
      <td>3359.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Some of these people have little cash flow or they are not reporting everything.  that said we must use the information that's presented
default.sort_values("Monthly Income")[0:5]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Loan ID</th>
      <th>Customer ID</th>
      <th>Loan Status</th>
      <th>Current Loan Amount</th>
      <th>Term</th>
      <th>Credit Score</th>
      <th>Years in current job</th>
      <th>Home Ownership</th>
      <th>Annual Income</th>
      <th>Purpose</th>
      <th>Monthly Debt</th>
      <th>Years of Credit History</th>
      <th>Months since last delinquent</th>
      <th>Number of Open Accounts</th>
      <th>Number of Credit Problems</th>
      <th>Current Credit Balance</th>
      <th>Maximum Open Credit</th>
      <th>Bankruptcies</th>
      <th>Tax Liens</th>
      <th>Monthly Income</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>186271</th>
      <td>2025adf8-360c-4229-afd6-d282b6f1e29e</td>
      <td>77538d62-ee55-4f45-9949-1b91f3acf420</td>
      <td>Charged Off</td>
      <td>3622</td>
      <td>Short Term</td>
      <td>731.0</td>
      <td>6 years</td>
      <td>Home Mortgage</td>
      <td>0.0</td>
      <td>Debt Consolidation</td>
      <td>$0.00</td>
      <td>38.0</td>
      <td>10.0</td>
      <td>8</td>
      <td>0</td>
      <td>10423</td>
      <td>18318</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>256312</th>
      <td>fdda52f3-bbfa-45da-80b5-2bd9b68638ed</td>
      <td>e3dedb71-dfc5-4872-a6b0-03aa22a74272</td>
      <td>Charged Off</td>
      <td>1384</td>
      <td>Short Term</td>
      <td>736.0</td>
      <td>&lt; 1 year</td>
      <td>Rent</td>
      <td>4033.0</td>
      <td>Debt Consolidation</td>
      <td>$44.50</td>
      <td>9.5</td>
      <td>34.0</td>
      <td>8</td>
      <td>0</td>
      <td>1483</td>
      <td>6709</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>336.083333</td>
    </tr>
    <tr>
      <th>256311</th>
      <td>fdda52f3-bbfa-45da-80b5-2bd9b68638ed</td>
      <td>e3dedb71-dfc5-4872-a6b0-03aa22a74272</td>
      <td>Charged Off</td>
      <td>1384</td>
      <td>Short Term</td>
      <td>700.0</td>
      <td>&lt; 1 year</td>
      <td>Rent</td>
      <td>4033.0</td>
      <td>Debt Consolidation</td>
      <td>$44.50</td>
      <td>9.5</td>
      <td>34.0</td>
      <td>8</td>
      <td>0</td>
      <td>1483</td>
      <td>6709</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>336.083333</td>
    </tr>
    <tr>
      <th>193883</th>
      <td>376f7eba-6e51-47ef-936a-064871a16e48</td>
      <td>0f7f0c8d-f0fd-4788-8491-2ea6b09e5419</td>
      <td>Charged Off</td>
      <td>1204</td>
      <td>Short Term</td>
      <td>700.0</td>
      <td>&lt; 1 year</td>
      <td>Rent</td>
      <td>4815.0</td>
      <td>other</td>
      <td>$55.38</td>
      <td>13.9</td>
      <td>34.0</td>
      <td>7</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>401.250000</td>
    </tr>
    <tr>
      <th>193884</th>
      <td>376f7eba-6e51-47ef-936a-064871a16e48</td>
      <td>0f7f0c8d-f0fd-4788-8491-2ea6b09e5419</td>
      <td>Charged Off</td>
      <td>1204</td>
      <td>Short Term</td>
      <td>717.0</td>
      <td>&lt; 1 year</td>
      <td>Rent</td>
      <td>4815.0</td>
      <td>other</td>
      <td>$55.38</td>
      <td>13.9</td>
      <td>34.0</td>
      <td>7</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>401.250000</td>
    </tr>
  </tbody>
</table>
</div>



### Creating Monthly Income NO Default


```python
columns = no_default["Annual Income"]

no_default["Monthly Income"] = map(lambda x: x/12, columns)

no_default.loc[0:10, ["Annual Income", "Monthly Income"]]
```

    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:3: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      This is separate from the ipykernel package so we can avoid doing imports until





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Annual Income</th>
      <th>Monthly Income</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>33694.0</td>
      <td>2807.833333</td>
    </tr>
    <tr>
      <th>1</th>
      <td>42269.0</td>
      <td>3522.416667</td>
    </tr>
    <tr>
      <th>2</th>
      <td>90126.0</td>
      <td>7510.500000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>38072.0</td>
      <td>3172.666667</td>
    </tr>
    <tr>
      <th>4</th>
      <td>50025.0</td>
      <td>4168.750000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>41853.0</td>
      <td>3487.750000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>70000.0</td>
      <td>5833.333333</td>
    </tr>
    <tr>
      <th>7</th>
      <td>55985.0</td>
      <td>4665.416667</td>
    </tr>
    <tr>
      <th>8</th>
      <td>64760.0</td>
      <td>5396.666667</td>
    </tr>
    <tr>
      <th>9</th>
      <td>153495.0</td>
      <td>12791.250000</td>
    </tr>
    <tr>
      <th>10</th>
      <td>65605.0</td>
      <td>5467.083333</td>
    </tr>
  </tbody>
</table>
</div>




```python
print (no_default["Monthly Income"].isnull().sum())
print(no_default["Monthly Income"].shape)
```

    0
    (176191,)


### Now we must Address Monthly Debt 
-its got a string and it has "$" and "," that needs to be removed


```python
# This shows me I can strip select multiple items within an element
y = "15,000"
float(y.replace("$", "").replace(",",""))
```




    15000.0




```python

# Lets leverage this technique and apply this to the entire column of both default and no devault Data Frames
default["Monthly Debt"] = default["Monthly Debt"].map(lambda x: x.replace("$","").replace(",", " "))
default["Monthly Debt"]
```

    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:3: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      This is separate from the ipykernel package so we can avoid doing imports until





    176191       542.29
    176192       542.29
    176193       597.50
    176194       596.85
    176195       596.85
    176196       597.50
    176197       662.16
    176198       662.16
    176199       745.70
    176200       745.70
    176201    1 083.91 
    176202    1 083.91 
    176203       738.08
    176204       738.08
    176205       706.58
    176206       706.58
    176207    1 290.98 
    176208    1 290.98 
    176209       633.29
    176210       633.29
    176211       538.15
    176212       538.15
    176213    1 931.07 
    176214    1 931.07 
    176215       711.03
    176216       711.03
    176217       522.84
    176218       522.84
    176219       613.77
    176220       613.77
                ...    
    256954    3 190.98 
    256955       855.91
    256956       855.91
    256957    1 053.61 
    256958    1 060.27 
    256959    1 053.61 
    256960    1 060.27 
    256961       953.68
    256962       953.68
    256963    1 340.53 
    256964    1 340.53 
    256965       792.86
    256966       792.86
    256967    1 163.99 
    256968    1 163.99 
    256969       705.04
    256970       790.35
    256971       790.35
    256972       732.67
    256973       732.67
    256974    1 412.33 
    256975    1 412.33 
    256976       707.08
    256977       707.08
    256978        47.11
    256979        47.11
    256980       982.82
    256981       982.82
    256982       297.96
    256983       297.96
    Name: Monthly Debt, Length: 80793, dtype: object




```python
#APPLY the Same technique to No Default DF
no_default["Monthly Debt"] = no_default["Monthly Debt"].map(lambda x:x.replace("$"," ").replace(",", " "))
no_default["Monthly Debt"][0:10]
```

    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      





    0        584.03
    1     1 106.04 
    2     1 321.85 
    3        751.92
    4        355.18
    5        561.52
    6        386.36
    7        741.79
    8        582.84
    9     1 573.32 
    Name: Monthly Debt, dtype: object




```python
#Now to remove the space
no_default["Monthly Debt"] = no_default["Monthly Debt"].map(lambda x:x.replace(" ",""))
no_default["Monthly Debt"][0:10]
```

    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      





    0     584.03
    1    1106.04
    2    1321.85
    3     751.92
    4     355.18
    5     561.52
    6     386.36
    7     741.79
    8     582.84
    9    1573.32
    Name: Monthly Debt, dtype: object




```python
# Removing space from default
default["Monthly Debt"] = default["Monthly Debt"].map(lambda x:x.replace(" ",""))
default["Monthly Debt"][0:13]
```

    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      





    176191     542.29
    176192     542.29
    176193     597.50
    176194     596.85
    176195     596.85
    176196     597.50
    176197     662.16
    176198     662.16
    176199     745.70
    176200     745.70
    176201    1083.91
    176202    1083.91
    176203     738.08
    Name: Monthly Debt, dtype: object



### Monthly Debt column still a "string" object


```python
# Convert string to float
no_default["Monthly Debt"].dtypes

```




    dtype('O')




```python
no_default["Monthly Debt"].convert_objects(convert_numeric=True)[0:5]


```

    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:1: FutureWarning: convert_objects is deprecated.  To re-infer data dtypes for object columns, use Series.infer_objects()
    For all other conversions use the data-type specific converters pd.to_datetime, pd.to_timedelta and pd.to_numeric.
      """Entry point for launching an IPython kernel.





    0     584.03
    1    1106.04
    2    1321.85
    3     751.92
    4     355.18
    Name: Monthly Debt, dtype: float64




```python
#convert default string to float now
default["Monthly Debt"].dtypes
```




    dtype('O')




```python
#AGAIN must convert "objects" to to numeric so we can divide
default["Monthly Debt"].convert_objects(convert_numeric=True).dtypes

```

    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:2: FutureWarning: convert_objects is deprecated.  To re-infer data dtypes for object columns, use Series.infer_objects()
    For all other conversions use the data-type specific converters pd.to_datetime, pd.to_timedelta and pd.to_numeric.
      





    dtype('float64')




```python
#monthly debt/Income must make sure we don't divide by zero
default["Monthly Income"][(default["Monthly Income"] == 0 )].count()
```




    1




```python
# to ensure no ZeroDivisionError lets make each value in monthly income = $1
default["Monthly Income"].replace(0, 1, inplace = True)

```

    /anaconda2/lib/python2.7/site-packages/pandas/core/generic.py:5886: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      self._update_inplace(new_data)



```python
default["Monthly Income"][(default["Monthly Income"] == 0 )].count()
```




    0




```python
#Finally! Lets create the Feature!
column_x = default["Monthly Debt"].convert_objects(convert_numeric = True)
column_y = default["Monthly Income"]

default["Monthly Debt/Equity"] = map(lambda x, y: x/y, column_x, column_y)
default["Monthly Debt/Equity"][0:10]

```

    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:2: FutureWarning: convert_objects is deprecated.  To re-infer data dtypes for object columns, use Series.infer_objects()
    For all other conversions use the data-type specific converters pd.to_datetime, pd.to_timedelta and pd.to_numeric.
      
    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      """





    176191    0.132199
    176192    0.112198
    176193    0.123621
    176194    0.135001
    176195    0.135001
    176196    0.134998
    176197    0.225001
    176198    0.225001
    176199    0.154283
    176200    0.222001
    Name: Monthly Debt/Equity, dtype: float64




```python
#now lets make this on the no_default Column. 
no_default["Monthly Income"][(no_default["Monthly Income"] == 0 )].count()

```




    0




```python
# No_Default "Monthly Debt/Equity feature"
column_x = no_default["Monthly Debt"].convert_objects(convert_numeric = True)
column_y = no_default["Monthly Income"]

no_default["Monthly Debt/Equity"] = map(lambda x, y: x/y, column_x, column_y)
no_default["Monthly Debt/Equity"][0:10]

```

    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:2: FutureWarning: convert_objects is deprecated.  To re-infer data dtypes for object columns, use Series.infer_objects()
    For all other conversions use the data-type specific converters pd.to_datetime, pd.to_timedelta and pd.to_numeric.
      
    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      """





    0    0.208000
    1    0.314000
    2    0.176000
    3    0.236999
    4    0.085201
    5    0.160998
    6    0.066233
    7    0.158998
    8    0.108000
    9    0.123000
    Name: Monthly Debt/Equity, dtype: float64




```python
default.columns
```




    Index([u'Loan ID', u'Customer ID', u'Loan Status', u'Current Loan Amount',
           u'Term', u'Credit Score', u'Years in current job', u'Home Ownership',
           u'Annual Income', u'Purpose', u'Monthly Debt',
           u'Years of Credit History', u'Months since last delinquent',
           u'Number of Open Accounts', u'Number of Credit Problems',
           u'Current Credit Balance', u'Maximum Open Credit', u'Bankruptcies',
           u'Tax Liens', u'Monthly Income', u'Monthly Debt/Equity'],
          dtype='object')




```python
default.iloc[0:10,[10,19,20]]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Monthly Debt</th>
      <th>Monthly Income</th>
      <th>Monthly Debt/Equity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>176191</th>
      <td>542.29</td>
      <td>4102.083333</td>
      <td>0.132199</td>
    </tr>
    <tr>
      <th>176192</th>
      <td>542.29</td>
      <td>4833.333333</td>
      <td>0.112198</td>
    </tr>
    <tr>
      <th>176193</th>
      <td>597.50</td>
      <td>4833.333333</td>
      <td>0.123621</td>
    </tr>
    <tr>
      <th>176194</th>
      <td>596.85</td>
      <td>4421.083333</td>
      <td>0.135001</td>
    </tr>
    <tr>
      <th>176195</th>
      <td>596.85</td>
      <td>4421.083333</td>
      <td>0.135001</td>
    </tr>
    <tr>
      <th>176196</th>
      <td>597.50</td>
      <td>4426.000000</td>
      <td>0.134998</td>
    </tr>
    <tr>
      <th>176197</th>
      <td>662.16</td>
      <td>2942.916667</td>
      <td>0.225001</td>
    </tr>
    <tr>
      <th>176198</th>
      <td>662.16</td>
      <td>2942.916667</td>
      <td>0.225001</td>
    </tr>
    <tr>
      <th>176199</th>
      <td>745.70</td>
      <td>4833.333333</td>
      <td>0.154283</td>
    </tr>
    <tr>
      <th>176200</th>
      <td>745.70</td>
      <td>3359.000000</td>
      <td>0.222001</td>
    </tr>
  </tbody>
</table>
</div>




```python
no_default.iloc[0:10,[10,19,20]]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Monthly Debt</th>
      <th>Monthly Income</th>
      <th>Monthly Debt/Equity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>584.03</td>
      <td>2807.833333</td>
      <td>0.208000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1106.04</td>
      <td>3522.416667</td>
      <td>0.314000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1321.85</td>
      <td>7510.500000</td>
      <td>0.176000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>751.92</td>
      <td>3172.666667</td>
      <td>0.236999</td>
    </tr>
    <tr>
      <th>4</th>
      <td>355.18</td>
      <td>4168.750000</td>
      <td>0.085201</td>
    </tr>
    <tr>
      <th>5</th>
      <td>561.52</td>
      <td>3487.750000</td>
      <td>0.160998</td>
    </tr>
    <tr>
      <th>6</th>
      <td>386.36</td>
      <td>5833.333333</td>
      <td>0.066233</td>
    </tr>
    <tr>
      <th>7</th>
      <td>741.79</td>
      <td>4665.416667</td>
      <td>0.158998</td>
    </tr>
    <tr>
      <th>8</th>
      <td>582.84</td>
      <td>5396.666667</td>
      <td>0.108000</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1573.32</td>
      <td>12791.250000</td>
      <td>0.123000</td>
    </tr>
  </tbody>
</table>
</div>



### Feature 3
-Current Credit Balance/Annual Income


```python
print(default["Current Credit Balance"].dtypes)
default["Annual Income"][(default["Annual Income"] <= 1)]

```

    int64





    186271    0.0
    Name: Annual Income, dtype: float64




```python
default["Annual Income"].replace(0, 1, inplace = True)

default["Annual Income"].isnull().sum()
```




    0




```python
#Both integers so lets just create it. 
column_a = default["Current Credit Balance"]
column_b = default["Annual Income"]

default["Total Debt/Annual Income"] = map(lambda x, y: x/y, column_a, column_b)

default["Total Debt/Annual Income"][0:10]
```

    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      """





    176191    0.286907
    176192    0.243500
    176193    0.117534
    176194    0.128362
    176195    0.128362
    176196    0.128351
    176197    0.483987
    176198    0.483987
    176199    0.491603
    176200    0.707378
    Name: Total Debt/Annual Income, dtype: float64




```python
# Check no default
no_default["Annual Income"].isnull().sum()
```




    0




```python
# no zeros.  Same process as before 
column_c = no_default["Current Credit Balance"]
column_d = no_default["Annual Income"]

no_default["Total Debt/Annual Income"] = map(lambda x, y: x/y, column_c, column_d)

no_default["Total Debt/Annual Income"][0:10]
```

    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      """





    0    0.200629
    1    0.148146
    2    0.232641
    3    0.591747
    4    0.347646
    5    0.054691
    6    0.171000
    7    0.195159
    8    0.137122
    9    0.212580
    Name: Total Debt/Annual Income, dtype: float64



### Create Graphs (jose)


```python
default.columns
```




    Index([u'Loan ID', u'Customer ID', u'Loan Status', u'Current Loan Amount',
           u'Term', u'Credit Score', u'Years in current job', u'Home Ownership',
           u'Annual Income', u'Purpose', u'Monthly Debt',
           u'Years of Credit History', u'Months since last delinquent',
           u'Number of Open Accounts', u'Number of Credit Problems',
           u'Current Credit Balance', u'Maximum Open Credit', u'Bankruptcies',
           u'Tax Liens', u'Monthly Income', u'Monthly Debt/Equity',
           u'Total Debt/Annual Income'],
          dtype='object')




```python
default.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 80793 entries, 176191 to 256983
    Data columns (total 22 columns):
    Loan ID                         80793 non-null object
    Customer ID                     80793 non-null object
    Loan Status                     80793 non-null object
    Current Loan Amount             80793 non-null int64
    Term                            80793 non-null object
    Credit Score                    80793 non-null float64
    Years in current job            80793 non-null object
    Home Ownership                  80793 non-null object
    Annual Income                   80793 non-null float64
    Purpose                         80793 non-null object
    Monthly Debt                    80793 non-null object
    Years of Credit History         80793 non-null float64
    Months since last delinquent    80793 non-null float64
    Number of Open Accounts         80793 non-null int64
    Number of Credit Problems       80793 non-null int64
    Current Credit Balance          80793 non-null int64
    Maximum Open Credit             80793 non-null object
    Bankruptcies                    80793 non-null float64
    Tax Liens                       80793 non-null float64
    Monthly Income                  80793 non-null float64
    Monthly Debt/Equity             80793 non-null float64
    Total Debt/Annual Income        80793 non-null float64
    dtypes: float64(9), int64(4), object(9)
    memory usage: 14.2+ MB


# DUPLICATES
### Default and then we will move to the No_Default DF.
 - find contionous columns and remove dups by loan id 
 - find categorical columns and remove dups by loan id
 - We should be able to run algorithims at that point


```python
#Observe we take the the MAX of the current loan amount within the Loan ID to ensure we have the proper credit reality conveyed. 
```


```python
default.columns
```




    Index([u'Loan ID', u'Customer ID', u'Loan Status', u'Current Loan Amount',
           u'Term', u'Credit Score', u'Years in current job', u'Home Ownership',
           u'Annual Income', u'Purpose', u'Monthly Debt',
           u'Years of Credit History', u'Months since last delinquent',
           u'Number of Open Accounts', u'Number of Credit Problems',
           u'Current Credit Balance', u'Maximum Open Credit', u'Bankruptcies',
           u'Tax Liens', u'Monthly Income', u'Monthly Debt/Equity',
           u'Total Debt/Annual Income'],
          dtype='object')




```python
# Get Categorical
df1 = default.select_dtypes(include = ["object"])
# Get Numberical
df2 = default.select_dtypes(exclude = ["object"])
```


```python
#nOW IN THE NUMBERICAL df you must put the loan ID to quantify the duplicates
df2["Loan ID"] = default["Loan ID"]
# df2[0:2]
```

    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      



```python
df1["Loan ID"].nunique()
```




    39509




```python
# I needed to put inplace = True
df1.drop_duplicates(subset = "Loan ID", inplace = True)
df1.shape
```

    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      





    (39509, 9)




```python
#On the numberical columns this creates a dataframe where we have the highest loan aamount being captured.  reset_index allows the "Loan ID"
# to be back part of the index. 
df2 = df2.groupby("Loan ID").max().reset_index()
df2.shape
```




    (39509, 14)



### Scaling and Get Dummies 
- Adjusting a few object columns we need to normalize


```python
# making monthly Debt numeric to scale
df1['Monthly Debt'] = pd.to_numeric(df1['Monthly Debt'], errors='raise')
df1["Monthly Debt"].dtypes

#Making "Maximum Open Credit" numeric to scale 
df1['Maximum Open Credit'] = pd.to_numeric(df1['Maximum Open Credit'], errors='coerce')
df1["Maximum Open Credit"].fillna(df1['Maximum Open Credit'].mean(), inplace = True)
df1["Maximum Open Credit"].dtypes
```

    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      
    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:6: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      





    dtype('float64')



### Complete Scaling (MinMax just to get this done)


```python
from sklearn.preprocessing import MinMaxScaler
scaler = MinMaxScaler()


df1['Maximum Open Credit'] = scaler.fit_transform(df1[['Maximum Open Credit']])
df1['Monthly Debt'] = scaler.fit_transform(df1[["Monthly Debt"]])
df2["Monthly Income"] = scaler.fit_transform(df2[["Monthly Income"]])
df2["Annual Income"] = scaler.fit_transform(df2[["Annual Income"]])
df2["Monthly Debt/Equity"] = scaler.fit_transform(df2[["Monthly Debt/Equity"]])
df2["Total Debt/Annual Income"] = scaler.fit_transform(df2[["Total Debt/Annual Income"]])
df2["Current Loan Amount"] = scaler.fit_transform(df2[["Current Loan Amount"]])
df2["Current Credit Balance"] = scaler.fit_transform(df2[["Current Credit Balance"]])


```

    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      """
    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:6: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      



```python
df1.columns
```




    Index([u'Loan ID', u'Customer ID', u'Loan Status', u'Term',
           u'Years in current job', u'Home Ownership', u'Purpose', u'Monthly Debt',
           u'Maximum Open Credit'],
          dtype='object')



### Complete Get Dummies


```python
df1["Home Ownership"].unique()
```




    array(['Rent', 'Own Home', 'Home Mortgage'], dtype=object)




```python
home_mapping = {'Rent': 0, 'Own Home': 0, 'Home Mortgage': 1}
df1['Home Ownership'] = df1['Home Ownership'].map(home_mapping)
print (df1["Home Ownership"].unique())

# We Must Drop First = True to make this work in our data frame
df1 = pd.get_dummies(df1, columns = ["Home Ownership", "Purpose", "Years in current job", "Term"], drop_first = True)
```

    [0 1]


    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      



```python
print (df1.shape)
print (" ")
print(df1.columns)


```

    (39509, 26)
     
    Index([u'Loan ID', u'Customer ID', u'Loan Status', u'Monthly Debt',
           u'Maximum Open Credit', u'Home Ownership_1', u'Purpose_Buy House',
           u'Purpose_Buy a Car', u'Purpose_Debt Consolidation',
           u'Purpose_Educational Expenses', u'Purpose_Home Improvements',
           u'Purpose_Medical Bills', u'Purpose_Other', u'Purpose_Take a Trip',
           u'Purpose_other', u'Years in current job_10+ years',
           u'Years in current job_2 years', u'Years in current job_3 years',
           u'Years in current job_4 years', u'Years in current job_5 years',
           u'Years in current job_6 years', u'Years in current job_7 years',
           u'Years in current job_8 years', u'Years in current job_9 years',
           u'Years in current job_< 1 year', u'Term_Short Term'],
          dtype='object')


### Lets Merge back together


```python
default_new = pd.merge(df2,df1, on = "Loan ID")
default_new.shape
```




    (39509, 39)



### You Must do the same process with default Data Frame now

### Addressing Duplicates


```python
# Get Categorical
df3 = no_default.select_dtypes(include = ["object"])
# Get Numberical
df4 = no_default.select_dtypes(exclude = ["object"])

#get loan ID into Numberical
df4["Loan ID"] = no_default["Loan ID"]

# I needed to put inplace = True, ADDRESSES druplicates in categorical columns
df3.drop_duplicates(subset = "Loan ID", inplace = True)
# The groupby accounts for the duplicates in numberical values. 
df4 = df4.groupby("Loan ID").max().reset_index()
print (df3.shape)
print (df4.shape)
# df4
```

    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:7: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      import sys
    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:10: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      # Remove the CWD from sys.path while we load stuff.


    (176191, 8)
    (176191, 15)


### Scaling and Get Dummies 
- Adjusting a few object columns we need to normalize- 


```python
# making monthly Debt numeric to scale

df3['Monthly Debt'] = pd.to_numeric(df3['Monthly Debt'], errors='raise')
df3["Monthly Debt"].dtypes

#Making "Maximum Open Credit" numeric to scale 
df3['Maximum Open Credit'] = pd.to_numeric(df3['Maximum Open Credit'], errors='coerce')
df3["Maximum Open Credit"].fillna(df3['Maximum Open Credit'].mean(), inplace = True)
df3["Maximum Open Credit"].dtypes

from sklearn.preprocessing import MinMaxScaler
scaler = MinMaxScaler()


df3['Maximum Open Credit'] = scaler.fit_transform(df3[['Maximum Open Credit']])
df3['Monthly Debt'] = scaler.fit_transform(df3[["Monthly Debt"]])
df4["Monthly Income"] = scaler.fit_transform(df4[["Monthly Income"]])
df4["Annual Income"] = scaler.fit_transform(df4[["Annual Income"]])
df4["Monthly Debt/Equity"] = scaler.fit_transform(df4[["Monthly Debt/Equity"]])
df4["Total Debt/Annual Income"] = scaler.fit_transform(df4[["Total Debt/Annual Income"]])
df4["Current Loan Amount"] = scaler.fit_transform(df4[["Current Loan Amount"]])
df4["Current Credit Balance"] = scaler.fit_transform(df4[["Current Credit Balance"]])

```

    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:3: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      This is separate from the ipykernel package so we can avoid doing imports until
    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:7: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      import sys
    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:15: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      from ipykernel import kernelapp as app
    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:16: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      app.launch_new_instance()


### Complete Scaling (MinMax just to get this done)


```python
df4.columns
```




    Index([u'Loan ID', u'Current Loan Amount', u'Term', u'Credit Score',
           u'Annual Income', u'Years of Credit History',
           u'Months since last delinquent', u'Number of Open Accounts',
           u'Number of Credit Problems', u'Current Credit Balance',
           u'Bankruptcies', u'Tax Liens', u'Monthly Income',
           u'Monthly Debt/Equity', u'Total Debt/Annual Income'],
          dtype='object')



### Complete Get Dummies


```python
df3["Home Ownership"].unique()
```




    array(['Home Mortgage', 'Own Home', 'Rent'], dtype=object)




```python
#Create variable that contains a dictionary where it maps a "1" to the "Home Mortgage"
home_mapping = {'Rent': 0, 'Own Home': 0, 'Home Mortgage': 1}
#Applies 0, 0, 1 mapping of the Rent, Own Home, Home Mortgage respectively to the elements of the series. 
df3['Home Ownership'] = df3['Home Ownership'].map(home_mapping)
#Gts dummies and drops the first column from the Categorical columns we have.  
df3 = pd.get_dummies(df3, columns = ["Home Ownership", "Purpose", "Years in current job",], drop_first = True)
# You actually converted the "term" column numberical in the no default column thus this requirement
df4 = pd.get_dummies(df4, columns = ["Term"], drop_first = True)

print(df3.columns)
print ("")
print(df4.columns)
```

    Index([u'Loan ID', u'Customer ID', u'Loan Status', u'Monthly Debt',
           u'Maximum Open Credit', u'Home Ownership_1', u'Purpose_Buy House',
           u'Purpose_Buy a Car', u'Purpose_Debt Consolidation',
           u'Purpose_Educational Expenses', u'Purpose_Home Improvements',
           u'Purpose_Medical Bills', u'Purpose_Other', u'Purpose_Take a Trip',
           u'Purpose_other', u'Years in current job_10+ years',
           u'Years in current job_2 years', u'Years in current job_3 years',
           u'Years in current job_4 years', u'Years in current job_5 years',
           u'Years in current job_6 years', u'Years in current job_7 years',
           u'Years in current job_8 years', u'Years in current job_9 years',
           u'Years in current job_< 1 year'],
          dtype='object')
    
    Index([u'Loan ID', u'Current Loan Amount', u'Credit Score', u'Annual Income',
           u'Years of Credit History', u'Months since last delinquent',
           u'Number of Open Accounts', u'Number of Credit Problems',
           u'Current Credit Balance', u'Bankruptcies', u'Tax Liens',
           u'Monthly Income', u'Monthly Debt/Equity', u'Total Debt/Annual Income'],
          dtype='object')


    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:4: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      after removing the cwd from sys.path.


### Merge back together


```python
no_default_new = pd.merge(df4,df3, on = "Loan ID")
no_default_new.shape
```




    (176191, 38)




```python
default_new.shape
```




    (39509, 39)



### Concat the DF on rows


```python
df_t = [no_default_new, default_new ]
loan_df = pd.concat(df_t, ignore_index = True)
loan_df.shape
```

    /anaconda2/lib/python2.7/site-packages/ipykernel_launcher.py:2: FutureWarning: Sorting because non-concatenation axis is not aligned. A future version
    of pandas will change to not sort by default.
    
    To accept the future behavior, pass 'sort=True'.
    
    To retain the current behavior and silence the warning, pass sort=False
    
      





    (215700, 39)




```python
loan_df.isnull().sum()
```




    Annual Income                          0
    Bankruptcies                           0
    Credit Score                           0
    Current Credit Balance                 0
    Current Loan Amount                    0
    Customer ID                            0
    Home Ownership_1                       0
    Loan ID                                0
    Loan Status                            0
    Maximum Open Credit                    0
    Monthly Debt                           0
    Monthly Debt/Equity                    0
    Monthly Income                         0
    Months since last delinquent           0
    Number of Credit Problems              0
    Number of Open Accounts                0
    Purpose_Buy House                      0
    Purpose_Buy a Car                      0
    Purpose_Debt Consolidation             0
    Purpose_Educational Expenses           0
    Purpose_Home Improvements              0
    Purpose_Medical Bills                  0
    Purpose_Other                          0
    Purpose_Take a Trip                    0
    Purpose_other                          0
    Tax Liens                              0
    Term_Short Term                   176191
    Total Debt/Annual Income               0
    Years in current job_10+ years         0
    Years in current job_2 years           0
    Years in current job_3 years           0
    Years in current job_4 years           0
    Years in current job_5 years           0
    Years in current job_6 years           0
    Years in current job_7 years           0
    Years in current job_8 years           0
    Years in current job_9 years           0
    Years in current job_< 1 year          0
    Years of Credit History                0
    dtype: int64




```python
loan_df["Term_Short Term"].fillna(0, inplace= True)
loan_df.isnull().sum()
```




    Annual Income                     0
    Bankruptcies                      0
    Credit Score                      0
    Current Credit Balance            0
    Current Loan Amount               0
    Customer ID                       0
    Home Ownership_1                  0
    Loan ID                           0
    Loan Status                       0
    Maximum Open Credit               0
    Monthly Debt                      0
    Monthly Debt/Equity               0
    Monthly Income                    0
    Months since last delinquent      0
    Number of Credit Problems         0
    Number of Open Accounts           0
    Purpose_Buy House                 0
    Purpose_Buy a Car                 0
    Purpose_Debt Consolidation        0
    Purpose_Educational Expenses      0
    Purpose_Home Improvements         0
    Purpose_Medical Bills             0
    Purpose_Other                     0
    Purpose_Take a Trip               0
    Purpose_other                     0
    Tax Liens                         0
    Term_Short Term                   0
    Total Debt/Annual Income          0
    Years in current job_10+ years    0
    Years in current job_2 years      0
    Years in current job_3 years      0
    Years in current job_4 years      0
    Years in current job_5 years      0
    Years in current job_6 years      0
    Years in current job_7 years      0
    Years in current job_8 years      0
    Years in current job_9 years      0
    Years in current job_< 1 year     0
    Years of Credit History           0
    dtype: int64




```python
#lets get rid of customer ID and Loan ID
drop = ["Customer ID", "Loan ID"]
loan_df.drop(drop, axis = 1, inplace = True)
loan_df.shape
```




    (215700, 37)




```python
loan_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 215700 entries, 0 to 215699
    Data columns (total 37 columns):
    Annual Income                     215700 non-null float64
    Bankruptcies                      215700 non-null float64
    Credit Score                      215700 non-null float64
    Current Credit Balance            215700 non-null float64
    Current Loan Amount               215700 non-null float64
    Home Ownership_1                  215700 non-null uint8
    Loan Status                       215700 non-null object
    Maximum Open Credit               215700 non-null float64
    Monthly Debt                      215700 non-null float64
    Monthly Debt/Equity               215700 non-null float64
    Monthly Income                    215700 non-null float64
    Months since last delinquent      215700 non-null float64
    Number of Credit Problems         215700 non-null int64
    Number of Open Accounts           215700 non-null int64
    Purpose_Buy House                 215700 non-null uint8
    Purpose_Buy a Car                 215700 non-null uint8
    Purpose_Debt Consolidation        215700 non-null uint8
    Purpose_Educational Expenses      215700 non-null uint8
    Purpose_Home Improvements         215700 non-null uint8
    Purpose_Medical Bills             215700 non-null uint8
    Purpose_Other                     215700 non-null uint8
    Purpose_Take a Trip               215700 non-null uint8
    Purpose_other                     215700 non-null uint8
    Tax Liens                         215700 non-null float64
    Term_Short Term                   215700 non-null float64
    Total Debt/Annual Income          215700 non-null float64
    Years in current job_10+ years    215700 non-null uint8
    Years in current job_2 years      215700 non-null uint8
    Years in current job_3 years      215700 non-null uint8
    Years in current job_4 years      215700 non-null uint8
    Years in current job_5 years      215700 non-null uint8
    Years in current job_6 years      215700 non-null uint8
    Years in current job_7 years      215700 non-null uint8
    Years in current job_8 years      215700 non-null uint8
    Years in current job_9 years      215700 non-null uint8
    Years in current job_< 1 year     215700 non-null uint8
    Years of Credit History           215700 non-null float64
    dtypes: float64(14), int64(2), object(1), uint8(20)
    memory usage: 32.1+ MB



```python
loan_stats = {"Fully Paid":0, "Charged Off": 1}
loan_df["Loan Status"] = loan_df["Loan Status"].map(loan_stats)
print (loan_df["Loan Status"].isnull().sum())
loan_df["Loan Status"].shape

```

    0





    (215700,)



### Modeling


```python
X = loan_df.drop('Loan Status', axis=1)
y = loan_df['Loan Status']

import sklearn 
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.40)




    
```


```python
from sklearn.neighbors import KNeighborsClassifier
knn = KNeighborsClassifier()
knn_fit = knn.fit(X_train, y_train)

from sklearn import tree
dt = tree.DecisionTreeClassifier()
dt_fit = dt.fit(X_train, y_train)

from sklearn.ensemble import RandomForestClassifier
rf = RandomForestClassifier(n_estimators=15)
rf_fit = rf.fit(X_train, y_train)

from sklearn.ensemble import AdaBoostClassifier
abc = AdaBoostClassifier(n_estimators = 100)
abc_fit = abc.fit(X_train, y_train)

# from sklearn.ensemble import GradientBoostingClassifier
# gbc = GradientBoostingClassifier(n_estimators=100, learning_rate=1.0, max_depth=1)
# gbc_fit = gbc.fit(X_train, y_train)

from sklearn.metrics import accuracy_score
from sklearn.metrics import recall_score
from sklearn.metrics import precision_score
from sklearn.metrics import f1_score

algorithms = [knn, dt, rf, abc]
alg_names = ['K-Nearest Neightbors', 'Decision Tree', 'Random Forest', 'AdaBoost']

scores = pd.DataFrame()
scores['Name'] = alg_names
scores['Accuracy'] = [accuracy_score(y_test, x.predict(X_test)) for x in algorithms] 
scores['Recall'] = [recall_score(y_test, x.predict(X_test)) for x in algorithms] 
scores['Precision'] = [precision_score(y_test, x.predict(X_test)) for x in algorithms] 
scores['F1'] = [f1_score(y_test, x.predict(X_test)) for x in algorithms] 
scores.sort_values('Accuracy', ascending=False)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Accuracy</th>
      <th>Recall</th>
      <th>Precision</th>
      <th>F1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>AdaBoost</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Random Forest</td>
      <td>0.999988</td>
      <td>0.999937</td>
      <td>1.000000</td>
      <td>0.999969</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Decision Tree</td>
      <td>0.999954</td>
      <td>0.999748</td>
      <td>1.000000</td>
      <td>0.999874</td>
    </tr>
    <tr>
      <th>0</th>
      <td>K-Nearest Neightbors</td>
      <td>0.921917</td>
      <td>0.731089</td>
      <td>0.825012</td>
      <td>0.775216</td>
    </tr>
  </tbody>
</table>
</div>




```python
# loan_stats = {"Fully Paid":0, "Charged Off": 1}
# loan_df["Loan Status"] = loan_df["Loan Status"].map(loan_stats)
# loan_df["Loan Status"].shape
```




    (215700,)




```python
# import numpy as np
# import seaborn as sns
# import matplotlib.pyplot as plt
# sns.set(style = "white", context = "talk")
# da = df[["Credit Score", "Annual Income", "Monthly Debt", "Current Credit Balance","Home Ownership","Maximum Open Credit","Years of Credit History",
#         "Loan Status"]]
# #Set up Maplotlib figure

# f, (ax1, ax2, ax3) = plt.subplots(3, 1, figsize = (7,5), sharex = True)

# x = da["Loan Status"]
# y1 = da["Annual Income"]
# sns.barplot(x = x, y=y1, palette = "rocket", ax= ax1)
# ax1.axhline(0, color = "k", clip_on = False)
# ax1.set_ylabel("Sequential of monthly debt to Annual income")

```
