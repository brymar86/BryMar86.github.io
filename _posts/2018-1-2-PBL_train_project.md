---
layout: post
title: Probability of Tower Default Model
date: 2018-01-27 04:00:00
tags: MachineLearning RandomForrest RandomForrest
author: Bryan Marty
---

```python
import pandas as pd
import numpy as np

resource = pd.read_csv("resource_type.csv")
resource.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 21076 entries, 0 to 21075
    Data columns (total 2 columns):
    id               21076 non-null int64
    resource_type    21076 non-null object
    dtypes: int64(1), object(1)
    memory usage: 329.4+ KB



```python
severity = pd.read_csv("severity_type.csv")
pbl_train = pd.read_csv("PBL_train.csv")
log = pd.read_csv("log_feature.csv")
event = pd.read_csv("event_type.csv")
```


```python
print event.info()
print resource.info()
print log.info()
print severity.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 31170 entries, 0 to 31169
    Data columns (total 2 columns):
    id            31170 non-null int64
    event_type    31170 non-null object
    dtypes: int64(1), object(1)
    memory usage: 487.1+ KB
    None
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 21076 entries, 0 to 21075
    Data columns (total 2 columns):
    id               21076 non-null int64
    resource_type    21076 non-null object
    dtypes: int64(1), object(1)
    memory usage: 329.4+ KB
    None
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 58671 entries, 0 to 58670
    Data columns (total 3 columns):
    id             58671 non-null int64
    log_feature    58671 non-null object
    volume         58671 non-null int64
    dtypes: int64(2), object(1)
    memory usage: 1.3+ MB
    None
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 18552 entries, 0 to 18551
    Data columns (total 2 columns):
    id               18552 non-null int64
    severity_type    18552 non-null object
    dtypes: int64(1), object(1)
    memory usage: 289.9+ KB
    None



```python
pbl_train.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 7381 entries, 0 to 7380
    Data columns (total 3 columns):
    id                7381 non-null int64
    location          7381 non-null object
    fault_severity    7381 non-null int64
    dtypes: int64(2), object(1)
    memory usage: 173.1+ KB



```python
event.head()
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
      <th>id</th>
      <th>event_type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>6597</td>
      <td>event_type 11</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8011</td>
      <td>event_type 15</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2597</td>
      <td>event_type 15</td>
    </tr>
    <tr>
      <th>3</th>
      <td>5022</td>
      <td>event_type 15</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5022</td>
      <td>event_type 11</td>
    </tr>
  </tbody>
</table>
</div>




```python
pbl_train.nunique()  #unique = tower + Time Stamps , Location is the number of unique Towers. 
```




    id                7381
    location           929
    fault_severity       3
    dtype: int64




```python
pbl_train.shape

```




    (7381, 3)




```python
pbl_train.head(2)
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
      <th>id</th>
      <th>location</th>
      <th>fault_severity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>14121</td>
      <td>location 118</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>9320</td>
      <td>location 91</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
event.head(2)
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
      <th>id</th>
      <th>event_type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>6597</td>
      <td>event_type 11</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8011</td>
      <td>event_type 15</td>
    </tr>
  </tbody>
</table>
</div>




```python
train_event = pd.merge(pbl_train, event)
train_event.shape
```




    (12468, 4)




```python
train_event.head()
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
      <th>id</th>
      <th>location</th>
      <th>fault_severity</th>
      <th>event_type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>14121</td>
      <td>location 118</td>
      <td>1</td>
      <td>event_type 34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>14121</td>
      <td>location 118</td>
      <td>1</td>
      <td>event_type 35</td>
    </tr>
    <tr>
      <th>2</th>
      <td>9320</td>
      <td>location 91</td>
      <td>0</td>
      <td>event_type 34</td>
    </tr>
    <tr>
      <th>3</th>
      <td>9320</td>
      <td>location 91</td>
      <td>0</td>
      <td>event_type 35</td>
    </tr>
    <tr>
      <th>4</th>
      <td>14394</td>
      <td>location 152</td>
      <td>1</td>
      <td>event_type 35</td>
    </tr>
  </tbody>
</table>
</div>




```python
train_event_resource = pd.merge(train_event, resource)
train_event_resource.head()
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
      <th>id</th>
      <th>location</th>
      <th>fault_severity</th>
      <th>event_type</th>
      <th>resource_type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>14121</td>
      <td>location 118</td>
      <td>1</td>
      <td>event_type 34</td>
      <td>resource_type 2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>14121</td>
      <td>location 118</td>
      <td>1</td>
      <td>event_type 35</td>
      <td>resource_type 2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>9320</td>
      <td>location 91</td>
      <td>0</td>
      <td>event_type 34</td>
      <td>resource_type 2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>9320</td>
      <td>location 91</td>
      <td>0</td>
      <td>event_type 35</td>
      <td>resource_type 2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>14394</td>
      <td>location 152</td>
      <td>1</td>
      <td>event_type 35</td>
      <td>resource_type 2</td>
    </tr>
  </tbody>
</table>
</div>




```python
train_event_resource.shape
```




    (15543, 5)




```python
train_event_resource_log = pd.merge(train_event_resource, log)
train_event_resource_log.shape
```




    (61839, 7)




```python
car = pd.merge(train_event_resource_log, severity)
car.shape
```




    (61839, 8)




```python
car.head()  #CAR stands for customer analystical record= CAR 
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
      <th>id</th>
      <th>location</th>
      <th>fault_severity</th>
      <th>event_type</th>
      <th>resource_type</th>
      <th>log_feature</th>
      <th>volume</th>
      <th>severity_type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>14121</td>
      <td>location 118</td>
      <td>1</td>
      <td>event_type 34</td>
      <td>resource_type 2</td>
      <td>feature 312</td>
      <td>19</td>
      <td>severity_type 2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>14121</td>
      <td>location 118</td>
      <td>1</td>
      <td>event_type 34</td>
      <td>resource_type 2</td>
      <td>feature 232</td>
      <td>19</td>
      <td>severity_type 2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>14121</td>
      <td>location 118</td>
      <td>1</td>
      <td>event_type 35</td>
      <td>resource_type 2</td>
      <td>feature 312</td>
      <td>19</td>
      <td>severity_type 2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>14121</td>
      <td>location 118</td>
      <td>1</td>
      <td>event_type 35</td>
      <td>resource_type 2</td>
      <td>feature 232</td>
      <td>19</td>
      <td>severity_type 2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9320</td>
      <td>location 91</td>
      <td>0</td>
      <td>event_type 34</td>
      <td>resource_type 2</td>
      <td>feature 315</td>
      <td>200</td>
      <td>severity_type 2</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Time to Remove Text. 
car.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 61839 entries, 0 to 61838
    Data columns (total 8 columns):
    id                61839 non-null int64
    location          61839 non-null object
    fault_severity    61839 non-null int64
    event_type        61839 non-null object
    resource_type     61839 non-null object
    log_feature       61839 non-null object
    volume            61839 non-null int64
    severity_type     61839 non-null object
    dtypes: int64(3), object(5)
    memory usage: 4.2+ MB



```python
#these nexts lines of code Strip parts of string from the elements within a features
#lstrip stands for Left strip.  everything on the Left you will strip.
car["location"] = car["location"].map(lambda x: x.lstrip("location"))
car.head()
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
      <th>id</th>
      <th>location</th>
      <th>fault_severity</th>
      <th>event_type</th>
      <th>resource_type</th>
      <th>log_feature</th>
      <th>volume</th>
      <th>severity_type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>14121</td>
      <td>118</td>
      <td>1</td>
      <td>event_type 34</td>
      <td>resource_type 2</td>
      <td>feature 312</td>
      <td>19</td>
      <td>severity_type 2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>14121</td>
      <td>118</td>
      <td>1</td>
      <td>event_type 34</td>
      <td>resource_type 2</td>
      <td>feature 232</td>
      <td>19</td>
      <td>severity_type 2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>14121</td>
      <td>118</td>
      <td>1</td>
      <td>event_type 35</td>
      <td>resource_type 2</td>
      <td>feature 312</td>
      <td>19</td>
      <td>severity_type 2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>14121</td>
      <td>118</td>
      <td>1</td>
      <td>event_type 35</td>
      <td>resource_type 2</td>
      <td>feature 232</td>
      <td>19</td>
      <td>severity_type 2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9320</td>
      <td>91</td>
      <td>0</td>
      <td>event_type 34</td>
      <td>resource_type 2</td>
      <td>feature 315</td>
      <td>200</td>
      <td>severity_type 2</td>
    </tr>
  </tbody>
</table>
</div>




```python
car["event_type"] = car["event_type"].map(lambda x: x.lstrip("event_type"))
car.head()
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
      <th>id</th>
      <th>location</th>
      <th>fault_severity</th>
      <th>event_type</th>
      <th>resource_type</th>
      <th>log_feature</th>
      <th>volume</th>
      <th>severity_type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>14121</td>
      <td>118</td>
      <td>1</td>
      <td>34</td>
      <td>resource_type 2</td>
      <td>feature 312</td>
      <td>19</td>
      <td>severity_type 2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>14121</td>
      <td>118</td>
      <td>1</td>
      <td>34</td>
      <td>resource_type 2</td>
      <td>feature 232</td>
      <td>19</td>
      <td>severity_type 2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>14121</td>
      <td>118</td>
      <td>1</td>
      <td>35</td>
      <td>resource_type 2</td>
      <td>feature 312</td>
      <td>19</td>
      <td>severity_type 2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>14121</td>
      <td>118</td>
      <td>1</td>
      <td>35</td>
      <td>resource_type 2</td>
      <td>feature 232</td>
      <td>19</td>
      <td>severity_type 2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9320</td>
      <td>91</td>
      <td>0</td>
      <td>34</td>
      <td>resource_type 2</td>
      <td>feature 315</td>
      <td>200</td>
      <td>severity_type 2</td>
    </tr>
  </tbody>
</table>
</div>




```python
car["resource_type"] = car["resource_type"].map(lambda x: x.lstrip("resource_type"))
car.head()
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
      <th>id</th>
      <th>location</th>
      <th>fault_severity</th>
      <th>event_type</th>
      <th>resource_type</th>
      <th>log_feature</th>
      <th>volume</th>
      <th>severity_type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>14121</td>
      <td>118</td>
      <td>1</td>
      <td>34</td>
      <td>2</td>
      <td>feature 312</td>
      <td>19</td>
      <td>severity_type 2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>14121</td>
      <td>118</td>
      <td>1</td>
      <td>34</td>
      <td>2</td>
      <td>feature 232</td>
      <td>19</td>
      <td>severity_type 2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>14121</td>
      <td>118</td>
      <td>1</td>
      <td>35</td>
      <td>2</td>
      <td>feature 312</td>
      <td>19</td>
      <td>severity_type 2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>14121</td>
      <td>118</td>
      <td>1</td>
      <td>35</td>
      <td>2</td>
      <td>feature 232</td>
      <td>19</td>
      <td>severity_type 2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9320</td>
      <td>91</td>
      <td>0</td>
      <td>34</td>
      <td>2</td>
      <td>feature 315</td>
      <td>200</td>
      <td>severity_type 2</td>
    </tr>
  </tbody>
</table>
</div>




```python
car["log_feature"] = car["log_feature"].map(lambda x: x.lstrip("feature"))
car.head()
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
      <th>id</th>
      <th>location</th>
      <th>fault_severity</th>
      <th>event_type</th>
      <th>resource_type</th>
      <th>log_feature</th>
      <th>volume</th>
      <th>severity_type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>14121</td>
      <td>118</td>
      <td>1</td>
      <td>34</td>
      <td>2</td>
      <td>312</td>
      <td>19</td>
      <td>severity_type 2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>14121</td>
      <td>118</td>
      <td>1</td>
      <td>34</td>
      <td>2</td>
      <td>232</td>
      <td>19</td>
      <td>severity_type 2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>14121</td>
      <td>118</td>
      <td>1</td>
      <td>35</td>
      <td>2</td>
      <td>312</td>
      <td>19</td>
      <td>severity_type 2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>14121</td>
      <td>118</td>
      <td>1</td>
      <td>35</td>
      <td>2</td>
      <td>232</td>
      <td>19</td>
      <td>severity_type 2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9320</td>
      <td>91</td>
      <td>0</td>
      <td>34</td>
      <td>2</td>
      <td>315</td>
      <td>200</td>
      <td>severity_type 2</td>
    </tr>
  </tbody>
</table>
</div>




```python
car["severity_type"] = car["severity_type"].map(lambda x: x.lstrip("severity_type"))
car.head()
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
      <th>id</th>
      <th>location</th>
      <th>fault_severity</th>
      <th>event_type</th>
      <th>resource_type</th>
      <th>log_feature</th>
      <th>volume</th>
      <th>severity_type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>14121</td>
      <td>118</td>
      <td>1</td>
      <td>34</td>
      <td>2</td>
      <td>312</td>
      <td>19</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>14121</td>
      <td>118</td>
      <td>1</td>
      <td>34</td>
      <td>2</td>
      <td>232</td>
      <td>19</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>14121</td>
      <td>118</td>
      <td>1</td>
      <td>35</td>
      <td>2</td>
      <td>312</td>
      <td>19</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>14121</td>
      <td>118</td>
      <td>1</td>
      <td>35</td>
      <td>2</td>
      <td>232</td>
      <td>19</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9320</td>
      <td>91</td>
      <td>0</td>
      <td>34</td>
      <td>2</td>
      <td>315</td>
      <td>200</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
#know that you have numbers but they are just recognized as strings 
#we need to know if we have normals 
#What is the Y axis here class?? help.  

import matplotlib.pyplot as plt

fig = plt.figure()

ax1 = fig.add_subplot(121)
ax1.hist(car.event_type.astype(int))  #x axis is event type

##

ax2 = fig.add_subplot(122)
ax2.hist(car.log_feature.astype(int)) #x axis is the log_feature


```




    (array([  943.,  8021.,  4607.,  3411.,  4342., 10778.,  8391.,  7665.,
            11298.,  2383.]),
     array([  1. ,  39.3,  77.6, 115.9, 154.2, 192.5, 230.8, 269.1, 307.4,
            345.7, 384. ]),
     <a list of 10 Patch objects>)




```python
#converting Categorical using get dummies

car = pd.get_dummies(car, drop_first = True)
car.head()
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
      <th>id</th>
      <th>fault_severity</th>
      <th>volume</th>
      <th>location_ 10</th>
      <th>location_ 100</th>
      <th>location_ 1000</th>
      <th>location_ 1002</th>
      <th>location_ 1005</th>
      <th>location_ 1006</th>
      <th>location_ 1007</th>
      <th>...</th>
      <th>log_feature_ 94</th>
      <th>log_feature_ 95</th>
      <th>log_feature_ 96</th>
      <th>log_feature_ 97</th>
      <th>log_feature_ 98</th>
      <th>log_feature_ 99</th>
      <th>severity_type_ 2</th>
      <th>severity_type_ 3</th>
      <th>severity_type_ 4</th>
      <th>severity_type_ 5</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>14121</td>
      <td>1</td>
      <td>19</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>14121</td>
      <td>1</td>
      <td>19</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>14121</td>
      <td>1</td>
      <td>19</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>14121</td>
      <td>1</td>
      <td>19</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9320</td>
      <td>0</td>
      <td>200</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 1322 columns</p>
</div>




```python
car.shape
```




    (61839, 1322)




```python
#When you do a Groupby on a feature within a DataFrame it will make that feature an index. Be alert
CAR = car.groupby(["id"]).sum()
CAR.head(4)
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
      <th>fault_severity</th>
      <th>volume</th>
      <th>location_ 10</th>
      <th>location_ 100</th>
      <th>location_ 1000</th>
      <th>location_ 1002</th>
      <th>location_ 1005</th>
      <th>location_ 1006</th>
      <th>location_ 1007</th>
      <th>location_ 1008</th>
      <th>...</th>
      <th>log_feature_ 94</th>
      <th>log_feature_ 95</th>
      <th>log_feature_ 96</th>
      <th>log_feature_ 97</th>
      <th>log_feature_ 98</th>
      <th>log_feature_ 99</th>
      <th>severity_type_ 2</th>
      <th>severity_type_ 3</th>
      <th>severity_type_ 4</th>
      <th>severity_type_ 5</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>12</td>
      <td>20</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0</td>
      <td>34</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2</td>
      <td>2</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>0</td>
      <td>32</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>12.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>4 rows × 1321 columns</p>
</div>




```python
#we have to make the id a Colunm again b/c we need a common column to merge data frames. 

CAR.reset_index(inplace = True)
CAR.head(3)
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
      <th>id</th>
      <th>fault_severity</th>
      <th>volume</th>
      <th>location_ 10</th>
      <th>location_ 100</th>
      <th>location_ 1000</th>
      <th>location_ 1002</th>
      <th>location_ 1005</th>
      <th>location_ 1006</th>
      <th>location_ 1007</th>
      <th>...</th>
      <th>log_feature_ 94</th>
      <th>log_feature_ 95</th>
      <th>log_feature_ 96</th>
      <th>log_feature_ 97</th>
      <th>log_feature_ 98</th>
      <th>log_feature_ 99</th>
      <th>severity_type_ 2</th>
      <th>severity_type_ 3</th>
      <th>severity_type_ 4</th>
      <th>severity_type_ 5</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>12</td>
      <td>20</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5</td>
      <td>0</td>
      <td>34</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6</td>
      <td>2</td>
      <td>2</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>3 rows × 1322 columns</p>
</div>




```python
#THIS IS AWESOME!!! I just now Merged the ID  and Fault Severity.  

#RECALL we had to do all of this because we had sooooo many 1, 12 in the idea column it didn't make sense in the X.  we had to get Fault Severity so we have 0,1,2.

car_data = pd.merge(CAR, pbl_train[["id", "fault_severity"]])

car_data.shape
```




    (4964, 1322)




```python
#car_data = CAR.drop(["fault_severity"], axis = 1, inplace = True)  we did this earlier as the Fault Severity was 4, 6 `8
#car_data
```


```python
car_data.head()
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
      <th>id</th>
      <th>fault_severity</th>
      <th>volume</th>
      <th>location_ 10</th>
      <th>location_ 100</th>
      <th>location_ 1000</th>
      <th>location_ 1002</th>
      <th>location_ 1005</th>
      <th>location_ 1006</th>
      <th>location_ 1007</th>
      <th>...</th>
      <th>log_feature_ 94</th>
      <th>log_feature_ 95</th>
      <th>log_feature_ 96</th>
      <th>log_feature_ 97</th>
      <th>log_feature_ 98</th>
      <th>log_feature_ 99</th>
      <th>severity_type_ 2</th>
      <th>severity_type_ 3</th>
      <th>severity_type_ 4</th>
      <th>severity_type_ 5</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5</td>
      <td>0</td>
      <td>34</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8</td>
      <td>0</td>
      <td>32</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>12.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>13</td>
      <td>0</td>
      <td>4</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>20</td>
      <td>0</td>
      <td>18</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>18.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>0</td>
      <td>12</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>8.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 1322 columns</p>
</div>




```python
car_data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 4964 entries, 0 to 4963
    Columns: 1322 entries, id to severity_type_ 5
    dtypes: float64(1319), int64(3)
    memory usage: 50.1 MB



```python
car_data.shape
```




    (4964, 1322)




```python

#I had to do this because i used this code and dropped "fault_severity" completely
# x = car_data.drop("fault_severity", axis = 1, inplace = True)  I should not have used inplace = True
# car_data = pd.merge(CAR, pbl_train[["fault_severity"]], left_index = True, right_index = True)
# car_data.head(2)
```


```python
car_data.shape
```




    (4964, 1322)




```python
x = car_data.drop("fault_severity", axis = 1)

y = car_data["fault_severity"]
  
```


```python
#Train Test Split

from sklearn.cross_validation import train_test_split
x_train, x_test, y_train, y_test = train_test_split(x, y, test_size = .30, random_state = 22)


```

    /anaconda2/lib/python2.7/site-packages/sklearn/cross_validation.py:41: DeprecationWarning: This module was deprecated in version 0.18 in favor of the model_selection module into which all the refactored classes and functions are moved. Also note that the interface of the new CV iterators are different from that of this module. This module will be removed in 0.20.
      "This module will be removed in 0.20.", DeprecationWarning)



```python
# confirms  1321 features being used as features. 
print y_train.shape
print ""
print x_train.shape
```

    (3474,)
    
    (3474, 1321)



```python
from sklearn.linear_model import LogisticRegression
LogReg = LogisticRegression()
LogReg.fit(x_train, y_train)

y_pred_log = LogReg.predict(x_test)
y_pred_log
```




    array([0, 0, 0, ..., 0, 0, 0])




```python
from sklearn.metrics import r2_score, mean_squared_error
from scipy import stats
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score

```


```python
print (accuracy_score(y_test, y_pred_log))
print ("")
print (mean_squared_error(y_test, y_pred_log))

```

    0.9624161073825503
    
    0.06375838926174497



```python
# I thought r2 and accuracy were supposed yield the same answer?
r2_score(y_test, y_pred_log)
```




    -0.034805430261205306




```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.neighbors import KNeighborsClassifier
```


```python
k_range = range(1,100)
scores = []
for k in k_range:
    knn = KNeighborsClassifier(n_neighbors = k)
    knn.fit(x_train, y_train)
    y_pred = knn.predict(x_test)
    scores.append(accuracy_score(y_test, y_pred))
    
import matplotlib.pyplot as plt
%matplotlib inline

plt.plot(k_range, scores)
plt.xlabel("Value of K for Knn")
plt.ylabel("Testing Accuracy")


    
```




    Text(0,0.5,'Testing Accuracy')




![png](output_42_1.png)



```python
K_N = KNeighborsClassifier(n_neighbors = 20)

```


```python
K_N.fit(x_train, y_train)

y_pred_knn = K_N.predict(x_test)
y_pred_knn
```




    array([0, 0, 0, ..., 0, 0, 0])




```python
accuracy_score(y_test, y_pred)
```




    0.9630872483221476




```python
#What does Number of estimators in RandomForestClassifier provide to us?
RandomForrest = RandomForestClassifier()
RandomForrest.fit(x_train, y_train)
y_pred_random = RandomForrest.predict(x_test)

y_pred_random
```




    array([0, 0, 1, ..., 0, 0, 0])




```python
test = y_test.values
count = 0 
```


```python
for i in range(len(y_pred_random)):
    if y_pred[i] == test[i]:
        count = count + 1
        
```


```python
count
```




    1435




```python
len(y_pred_random)
```




    1490




```python
#Gaahhhh!! 
float(count)/len(y_pred_random)
```




    0.9630872483221476




```python
accuracy_score(y_test, y_pred_random)
```




    0.963758389261745




```python
from sklearn.tree import DecisionTreeClassifier
d = DecisionTreeClassifier(min_samples_split = 10)

d.fit(x_train, y_train)

y_pred_decision = d.predict(x_test)

y_pred_decision
```




    array([0, 0, 1, ..., 0, 0, 0])




```python
#samples_split = 50
accuracy_score(y_test, y_pred_decision)
```




    0.9563758389261745




```python
#samples_split = 10
accuracy_score(y_test, y_pred_decision)
```




    0.9563758389261745




```python
#experiement with the number of boosters 
from sklearn.ensemble import GradientBoostingClassifier
gbc = GradientBoostingClassifier() 
```


```python
gbc.fit(x_train, y_train)
y_pred_gradient = gbc.predict(x_test)

y_pred_gradient
```




    array([0, 0, 1, ..., 0, 0, 0])




```python
accuracy_score(y_test, y_pred)
```




    0.9644295302013423




```python
#with 200
from sklearn.ensemble import GradientBoostingClassifier
gbc = GradientBoostingClassifier(n_estimators = 200)

gbc.fit(x_train, y_train)
y_pred_gradient = gbc.predict(x_test)

print (precision_score(y_test, y_pred_gradient))
print (recall_score(y_test, y_pred{gradient}))
```




    0.9630872483221476




```python
#with 50

from sklearn.ensemble import GradientBoostingClassifier
gbc = GradientBoostingClassifier(n_estimators = 50)

gbc.fit(x_train, y_train)
y_pred = gbc.predict(x_test)

accuracy_score(y_test, y_pred)
```




    0.9630872483221476




```python
#lets now adjust the tree holding n_estimators at 100. adjusting max_features and Leaf nodes prun data.
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score

from sklearn.ensemble import GradientBoostingClassifier
gbc = GradientBoostingClassifier(n_estimators = 50, max_leaf_nodes = 60)

gbc.fit(x_train, y_train)
y_pred_gradient = gbc.predict(x_test)

print (accuracy_score(y_test, y_pred_gradient,))
print (precision_score(y_test, y_pred_gradient, average=None))
print (recall_score(y_test, y_pred_gradient, average=None))



```

    0.9644295302013423
    [0.96947083 0.53333333 0.        ]
    [0.99651325 0.18604651 0.        ]



```python
#maintain 100 for estimators.

#I need to re-evaluate how i've put the features together and try and think of some creative ways to capture the key realities of the relevant attributes
#along with perhaps get rid of some uncessary noise in my feature selection to improve this algorithim 
```


```python
x_test.id.shape
```




    (1490,)




```python
y_pred_gradient
```




    array([0, 0, 1, ..., 0, 0, 0])




```python
# 
y_pred_proba_gbc = gbc.predict_proba(x_test)

y_pred_proba_rf = RandomForrest.predict_proba(x_test)
y_pred_proba_gbc[:,0]
```




    array([0.97670793, 0.99409982, 0.33645086, ..., 0.96517846, 0.99409982,
           0.9864808 ])




```python
# result_gradient = pd.DataFrame({"id": x_test.id, "Predicted fault_severity": y_pred_gradient, "prediction_probability_0": y_pred_gradient[:,0], "prediction_probability_1": y_pred_gradient[:,1],
# "prediction_probability_2": y_pred_gradient[:,2]})
result_gradient = pd.DataFrame({"id": x_test.id, "predicted fault_severity": y_pred_gradient,
                               "prediction_probability_0": y_pred_proba_gbc[:,0],
                                "prediction_probability_1": y_pred_proba_gbc[:,1],
                                "prediction_probability_2": y_pred_proba_gbc[:,2],
})
# result_gradient = pd.DataFrame({'car': ['Murcielago','Mustang'], 'rating':['awesome','cool']})
result_gradient=result_gradient.reset_index(drop=True)
result_gradient.head()
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
      <th>id</th>
      <th>predicted fault_severity</th>
      <th>prediction_probability_0</th>
      <th>prediction_probability_1</th>
      <th>prediction_probability_2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>14465</td>
      <td>0</td>
      <td>0.976708</td>
      <td>0.018620</td>
      <td>0.004672</td>
    </tr>
    <tr>
      <th>1</th>
      <td>18179</td>
      <td>0</td>
      <td>0.994100</td>
      <td>0.004047</td>
      <td>0.001854</td>
    </tr>
    <tr>
      <th>2</th>
      <td>234</td>
      <td>1</td>
      <td>0.336451</td>
      <td>0.647795</td>
      <td>0.015754</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2437</td>
      <td>0</td>
      <td>0.994100</td>
      <td>0.004047</td>
      <td>0.001854</td>
    </tr>
    <tr>
      <th>4</th>
      <td>16007</td>
      <td>0</td>
      <td>0.961651</td>
      <td>0.033727</td>
      <td>0.004622</td>
    </tr>
  </tbody>
</table>
</div>




```python
result_gradient.sort_values("prediction_probability_2", ascending = False).head()

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
      <th>id</th>
      <th>predicted fault_severity</th>
      <th>prediction_probability_0</th>
      <th>prediction_probability_1</th>
      <th>prediction_probability_2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>242</th>
      <td>18117</td>
      <td>0</td>
      <td>0.574035</td>
      <td>0.004170</td>
      <td>0.421796</td>
    </tr>
    <tr>
      <th>579</th>
      <td>4861</td>
      <td>2</td>
      <td>0.245613</td>
      <td>0.355928</td>
      <td>0.398459</td>
    </tr>
    <tr>
      <th>313</th>
      <td>18543</td>
      <td>1</td>
      <td>0.258220</td>
      <td>0.623233</td>
      <td>0.118547</td>
    </tr>
    <tr>
      <th>221</th>
      <td>623</td>
      <td>0</td>
      <td>0.918081</td>
      <td>0.003737</td>
      <td>0.078182</td>
    </tr>
    <tr>
      <th>231</th>
      <td>2058</td>
      <td>0</td>
      <td>0.810243</td>
      <td>0.112725</td>
      <td>0.077032</td>
    </tr>
  </tbody>
</table>
</div>


