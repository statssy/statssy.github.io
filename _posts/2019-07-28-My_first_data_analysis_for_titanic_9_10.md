---
layout: post
title:  "[파이썬]캐글 타이타닉 데이터 탐색 #9, #10(Feature Engineering)"
subtitle:   "Analysis"
categories: data
tags: anal
comments: true
---

### 캐글 타이타닉 데이터 탐색 #9, #10(Feature Engineering)
#9, #10 동영상에서는 Fill Null in Age와 Fill NUll in Embarked and Categorize Age를 한다.

참고 : [You Han Lee 유튜브](https://www.youtube.com/watch?v=qVknmB5OElE)



```python
df_train['Age'].isnull().sum()
```




    177




```python
df_train['Initial']= df_train['Name'].str.extract('([A-Za-z]+)\.') # 점 앞에꺼를 가져오겠다.
df_test['Initial']= df_test['Name'].str.extract('([A-Za-z]+)\.') # 점 앞에꺼를 가져오겠다.
```


```python
df_train.head()
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
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
      <th>FamilySize</th>
      <th>initial</th>
      <th>Initial</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>1.981001</td>
      <td>NaN</td>
      <td>S</td>
      <td>2</td>
      <td>Mr</td>
      <td>Mr</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>4.266662</td>
      <td>C85</td>
      <td>C</td>
      <td>2</td>
      <td>Mrs</td>
      <td>Mrs</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>2.070022</td>
      <td>NaN</td>
      <td>S</td>
      <td>1</td>
      <td>Miss</td>
      <td>Miss</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>113803</td>
      <td>3.972177</td>
      <td>C123</td>
      <td>S</td>
      <td>2</td>
      <td>Mrs</td>
      <td>Mrs</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>0</td>
      <td>3</td>
      <td>Allen, Mr. William Henry</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>373450</td>
      <td>2.085672</td>
      <td>NaN</td>
      <td>S</td>
      <td>1</td>
      <td>Mr</td>
      <td>Mr</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.crosstab(df_train['Initial'], df_train['Sex']).T.style.background_gradient(cmap='summer_r')
```




<style  type="text/css" >
    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col0 {
            background-color:  #ffff66;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col1 {
            background-color:  #ffff66;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col2 {
            background-color:  #008066;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col3 {
            background-color:  #ffff66;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col4 {
            background-color:  #ffff66;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col5 {
            background-color:  #ffff66;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col6 {
            background-color:  #008066;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col7 {
            background-color:  #ffff66;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col8 {
            background-color:  #ffff66;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col9 {
            background-color:  #008066;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col10 {
            background-color:  #008066;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col11 {
            background-color:  #008066;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col12 {
            background-color:  #ffff66;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col13 {
            background-color:  #008066;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col14 {
            background-color:  #008066;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col15 {
            background-color:  #ffff66;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col16 {
            background-color:  #ffff66;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col0 {
            background-color:  #008066;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col1 {
            background-color:  #008066;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col2 {
            background-color:  #ffff66;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col3 {
            background-color:  #008066;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col4 {
            background-color:  #008066;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col5 {
            background-color:  #008066;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col6 {
            background-color:  #ffff66;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col7 {
            background-color:  #008066;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col8 {
            background-color:  #008066;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col9 {
            background-color:  #ffff66;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col10 {
            background-color:  #ffff66;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col11 {
            background-color:  #ffff66;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col12 {
            background-color:  #008066;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col13 {
            background-color:  #ffff66;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col14 {
            background-color:  #ffff66;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col15 {
            background-color:  #008066;
        }    #T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col16 {
            background-color:  #008066;
        }</style>  
<table id="T_65536b82_b11c_11e9_8f4e_7f5be285089c" > 
<thead>    <tr> 
        <th class="index_name level0" >Initial</th> 
        <th class="col_heading level0 col0" >Capt</th> 
        <th class="col_heading level0 col1" >Col</th> 
        <th class="col_heading level0 col2" >Countess</th> 
        <th class="col_heading level0 col3" >Don</th> 
        <th class="col_heading level0 col4" >Dr</th> 
        <th class="col_heading level0 col5" >Jonkheer</th> 
        <th class="col_heading level0 col6" >Lady</th> 
        <th class="col_heading level0 col7" >Major</th> 
        <th class="col_heading level0 col8" >Master</th> 
        <th class="col_heading level0 col9" >Miss</th> 
        <th class="col_heading level0 col10" >Mlle</th> 
        <th class="col_heading level0 col11" >Mme</th> 
        <th class="col_heading level0 col12" >Mr</th> 
        <th class="col_heading level0 col13" >Mrs</th> 
        <th class="col_heading level0 col14" >Ms</th> 
        <th class="col_heading level0 col15" >Rev</th> 
        <th class="col_heading level0 col16" >Sir</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Sex</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_65536b82_b11c_11e9_8f4e_7f5be285089clevel0_row0" class="row_heading level0 row0" >female</th> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col0" class="data row0 col0" >0</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col1" class="data row0 col1" >0</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col2" class="data row0 col2" >1</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col3" class="data row0 col3" >0</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col4" class="data row0 col4" >1</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col5" class="data row0 col5" >0</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col6" class="data row0 col6" >1</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col7" class="data row0 col7" >0</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col8" class="data row0 col8" >0</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col9" class="data row0 col9" >182</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col10" class="data row0 col10" >2</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col11" class="data row0 col11" >1</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col12" class="data row0 col12" >0</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col13" class="data row0 col13" >125</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col14" class="data row0 col14" >1</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col15" class="data row0 col15" >0</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow0_col16" class="data row0 col16" >0</td> 
    </tr>    <tr> 
        <th id="T_65536b82_b11c_11e9_8f4e_7f5be285089clevel0_row1" class="row_heading level0 row1" >male</th> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col0" class="data row1 col0" >1</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col1" class="data row1 col1" >2</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col2" class="data row1 col2" >0</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col3" class="data row1 col3" >1</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col4" class="data row1 col4" >6</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col5" class="data row1 col5" >1</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col6" class="data row1 col6" >0</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col7" class="data row1 col7" >2</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col8" class="data row1 col8" >40</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col9" class="data row1 col9" >0</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col10" class="data row1 col10" >0</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col11" class="data row1 col11" >0</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col12" class="data row1 col12" >517</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col13" class="data row1 col13" >0</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col14" class="data row1 col14" >0</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col15" class="data row1 col15" >6</td> 
        <td id="T_65536b82_b11c_11e9_8f4e_7f5be285089crow1_col16" class="data row1 col16" >1</td> 
    </tr></tbody> 
</table> 




```python
df_train['Initial'].replace(['Mile', 'Mme', 'Ms', 'Dr', 'Major','Lady', 'Countess', 'Jonkheer', 'Col', 'Rev', 'Capt', 'Sir', 'Don', 'Dona'],
                           ['Miss', 'Miss', 'Miss', 'Mr', 'Mr', 'Mrs', 'Mrs', 'Other', 'Other', 'Other', 'Mr', 'Mr', 'Mr', 'Mr'], inplace=True)
df_test['Initial'].replace(['Mile', 'Mme', 'Ms', 'Dr', 'Major','Lady', 'Countess', 'Jonkheer', 'Col', 'Rev', 'Capt', 'Sir', 'Don', 'Dona'],
                           ['Miss', 'Miss', 'Miss', 'Mr', 'Mr', 'Mrs', 'Mrs', 'Other', 'Other', 'Other', 'Mr', 'Mr', 'Mr', 'Mr'], inplace=True)
```


```python
df_train.groupby('Initial').mean()
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
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Fare</th>
      <th>FamilySize</th>
    </tr>
    <tr>
      <th>Initial</th>
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
      <th>Master</th>
      <td>414.975000</td>
      <td>0.575000</td>
      <td>2.625000</td>
      <td>4.574167</td>
      <td>2.300000</td>
      <td>1.375000</td>
      <td>3.340710</td>
      <td>4.675000</td>
    </tr>
    <tr>
      <th>Miss</th>
      <td>408.864130</td>
      <td>0.701087</td>
      <td>2.298913</td>
      <td>21.831081</td>
      <td>0.706522</td>
      <td>0.543478</td>
      <td>3.113425</td>
      <td>2.250000</td>
    </tr>
    <tr>
      <th>Mlle</th>
      <td>676.500000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>24.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>4.070251</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Mr</th>
      <td>455.880907</td>
      <td>0.162571</td>
      <td>2.381853</td>
      <td>32.739609</td>
      <td>0.293006</td>
      <td>0.151229</td>
      <td>2.651507</td>
      <td>1.444234</td>
    </tr>
    <tr>
      <th>Mrs</th>
      <td>456.393701</td>
      <td>0.795276</td>
      <td>1.984252</td>
      <td>35.981818</td>
      <td>0.692913</td>
      <td>0.818898</td>
      <td>3.443751</td>
      <td>2.511811</td>
    </tr>
    <tr>
      <th>Other</th>
      <td>564.444444</td>
      <td>0.111111</td>
      <td>1.666667</td>
      <td>45.888889</td>
      <td>0.111111</td>
      <td>0.111111</td>
      <td>2.641605</td>
      <td>1.222222</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_train.groupby('Initial')['Survived'].mean().plot.bar()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7fde203fc5f8>




![png](/assets/img/post_img/My_first_data_analysis_for_titanic_files/My_first_data_analysis_for_titanic_65_1.png)



```python
df_all = pd.concat([df_train, df_test])
```


```python
df_all.head()
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
      <th>Age</th>
      <th>Cabin</th>
      <th>Embarked</th>
      <th>FamilySize</th>
      <th>Fare</th>
      <th>Initial</th>
      <th>Name</th>
      <th>Parch</th>
      <th>PassengerId</th>
      <th>Pclass</th>
      <th>Sex</th>
      <th>SibSp</th>
      <th>Survived</th>
      <th>Ticket</th>
      <th>initial</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>22.0</td>
      <td>NaN</td>
      <td>S</td>
      <td>2.0</td>
      <td>1.981001</td>
      <td>Mr</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>0</td>
      <td>1</td>
      <td>3</td>
      <td>male</td>
      <td>1</td>
      <td>0.0</td>
      <td>A/5 21171</td>
      <td>Mr</td>
    </tr>
    <tr>
      <th>1</th>
      <td>38.0</td>
      <td>C85</td>
      <td>C</td>
      <td>2.0</td>
      <td>4.266662</td>
      <td>Mrs</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>female</td>
      <td>1</td>
      <td>1.0</td>
      <td>PC 17599</td>
      <td>Mrs</td>
    </tr>
    <tr>
      <th>2</th>
      <td>26.0</td>
      <td>NaN</td>
      <td>S</td>
      <td>1.0</td>
      <td>2.070022</td>
      <td>Miss</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>0</td>
      <td>3</td>
      <td>3</td>
      <td>female</td>
      <td>0</td>
      <td>1.0</td>
      <td>STON/O2. 3101282</td>
      <td>Miss</td>
    </tr>
    <tr>
      <th>3</th>
      <td>35.0</td>
      <td>C123</td>
      <td>S</td>
      <td>2.0</td>
      <td>3.972177</td>
      <td>Mrs</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>0</td>
      <td>4</td>
      <td>1</td>
      <td>female</td>
      <td>1</td>
      <td>1.0</td>
      <td>113803</td>
      <td>Mrs</td>
    </tr>
    <tr>
      <th>4</th>
      <td>35.0</td>
      <td>NaN</td>
      <td>S</td>
      <td>1.0</td>
      <td>2.085672</td>
      <td>Mr</td>
      <td>Allen, Mr. William Henry</td>
      <td>0</td>
      <td>5</td>
      <td>3</td>
      <td>male</td>
      <td>0</td>
      <td>0.0</td>
      <td>373450</td>
      <td>Mr</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_all.groupby('Initial').mean()
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
      <th>Age</th>
      <th>FamilySize</th>
      <th>Fare</th>
      <th>Parch</th>
      <th>PassengerId</th>
      <th>Pclass</th>
      <th>SibSp</th>
      <th>Survived</th>
    </tr>
    <tr>
      <th>Initial</th>
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
      <th>Master</th>
      <td>5.482642</td>
      <td>4.675000</td>
      <td>15.442677</td>
      <td>1.377049</td>
      <td>658.852459</td>
      <td>2.655738</td>
      <td>2.049180</td>
      <td>0.575000</td>
    </tr>
    <tr>
      <th>Miss</th>
      <td>21.814104</td>
      <td>2.250000</td>
      <td>14.096861</td>
      <td>0.498099</td>
      <td>616.539924</td>
      <td>2.342205</td>
      <td>0.657795</td>
      <td>0.701087</td>
    </tr>
    <tr>
      <th>Mlle</th>
      <td>24.000000</td>
      <td>1.000000</td>
      <td>4.070251</td>
      <td>0.000000</td>
      <td>676.500000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Mr</th>
      <td>32.556397</td>
      <td>1.444234</td>
      <td>10.003941</td>
      <td>0.159533</td>
      <td>658.831388</td>
      <td>2.359274</td>
      <td>0.286641</td>
      <td>0.162571</td>
    </tr>
    <tr>
      <th>Mrs</th>
      <td>37.034884</td>
      <td>2.511811</td>
      <td>23.896996</td>
      <td>0.824121</td>
      <td>685.673367</td>
      <td>1.929648</td>
      <td>0.658291</td>
      <td>0.795276</td>
    </tr>
    <tr>
      <th>Other</th>
      <td>44.923077</td>
      <td>1.222222</td>
      <td>24.523034</td>
      <td>0.153846</td>
      <td>714.923077</td>
      <td>1.615385</td>
      <td>0.230769</td>
      <td>0.111111</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 두번째(1)로우의 값을 가져와 달라 (location을 쓰는것)
df_train.loc[1, :]
```




    PassengerId                                                    2
    Survived                                                       1
    Pclass                                                         1
    Name           Cumings, Mrs. John Bradley (Florence Briggs Th...
    Sex                                                       female
    Age                                                           38
    SibSp                                                          1
    Parch                                                          0
    Ticket                                                  PC 17599
    Fare                                                     4.26666
    Cabin                                                        C85
    Embarked                                                       C
    FamilySize                                                     2
    initial                                                      Mrs
    Initial                                                      Mrs
    Name: 1, dtype: object




```python
# 로케이션 이용해서 Survived가 1인것들만 가져옴
df_train.loc[df_train['Survived'] ==1 ].head()
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
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
      <th>FamilySize</th>
      <th>initial</th>
      <th>Initial</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>4.266662</td>
      <td>C85</td>
      <td>C</td>
      <td>2</td>
      <td>Mrs</td>
      <td>Mrs</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>2.070022</td>
      <td>NaN</td>
      <td>S</td>
      <td>1</td>
      <td>Miss</td>
      <td>Miss</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>113803</td>
      <td>3.972177</td>
      <td>C123</td>
      <td>S</td>
      <td>2</td>
      <td>Mrs</td>
      <td>Mrs</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>1</td>
      <td>3</td>
      <td>Johnson, Mrs. Oscar W (Elisabeth Vilhelmina Berg)</td>
      <td>female</td>
      <td>27.0</td>
      <td>0</td>
      <td>2</td>
      <td>347742</td>
      <td>2.409941</td>
      <td>NaN</td>
      <td>S</td>
      <td>3</td>
      <td>Mrs</td>
      <td>Mrs</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>1</td>
      <td>2</td>
      <td>Nasser, Mrs. Nicholas (Adele Achem)</td>
      <td>female</td>
      <td>14.0</td>
      <td>1</td>
      <td>0</td>
      <td>237736</td>
      <td>3.403555</td>
      <td>NaN</td>
      <td>C</td>
      <td>2</td>
      <td>Mrs</td>
      <td>Mrs</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_train.loc[(df_train['Age'].isnull()) & (df_train['Initial'] == 'Mr')].head()
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
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
      <th>FamilySize</th>
      <th>initial</th>
      <th>Initial</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>0</td>
      <td>3</td>
      <td>Moran, Mr. James</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>330877</td>
      <td>2.135148</td>
      <td>NaN</td>
      <td>Q</td>
      <td>1</td>
      <td>Mr</td>
      <td>Mr</td>
    </tr>
    <tr>
      <th>17</th>
      <td>18</td>
      <td>1</td>
      <td>2</td>
      <td>Williams, Mr. Charles Eugene</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>244373</td>
      <td>2.564949</td>
      <td>NaN</td>
      <td>S</td>
      <td>1</td>
      <td>Mr</td>
      <td>Mr</td>
    </tr>
    <tr>
      <th>26</th>
      <td>27</td>
      <td>0</td>
      <td>3</td>
      <td>Emir, Mr. Farred Chehab</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>2631</td>
      <td>1.977547</td>
      <td>NaN</td>
      <td>C</td>
      <td>1</td>
      <td>Mr</td>
      <td>Mr</td>
    </tr>
    <tr>
      <th>29</th>
      <td>30</td>
      <td>0</td>
      <td>3</td>
      <td>Todoroff, Mr. Lalio</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>349216</td>
      <td>2.066331</td>
      <td>NaN</td>
      <td>S</td>
      <td>1</td>
      <td>Mr</td>
      <td>Mr</td>
    </tr>
    <tr>
      <th>36</th>
      <td>37</td>
      <td>1</td>
      <td>3</td>
      <td>Mamee, Mr. Hanna</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>2677</td>
      <td>1.978128</td>
      <td>NaN</td>
      <td>C</td>
      <td>1</td>
      <td>Mr</td>
      <td>Mr</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 조건을 만족하는거를 치환
df_train.loc[(df_train['Age'].isnull()) & (df_train['Initial'] == 'Mr'), 'Age'] = 33
df_train.loc[(df_train['Age'].isnull()) & (df_train['Initial'] == 'Mrs'), 'Age'] = 37
df_train.loc[(df_train['Age'].isnull()) & (df_train['Initial'] == 'Master'), 'Age'] = 5
df_train.loc[(df_train['Age'].isnull()) & (df_train['Initial'] == 'Miss'), 'Age'] = 22
df_train.loc[(df_train['Age'].isnull()) & (df_train['Initial'] == 'Other'), 'Age'] = 45

df_test.loc[(df_test['Age'].isnull()) & (df_test['Initial'] == 'Mr'), 'Age'] = 33
df_test.loc[(df_test['Age'].isnull()) & (df_test['Initial'] == 'Mrs'), 'Age'] = 37
df_test.loc[(df_test['Age'].isnull()) & (df_test['Initial'] == 'Master'), 'Age'] = 5
df_test.loc[(df_test['Age'].isnull()) & (df_test['Initial'] == 'Miss'), 'Age'] = 22
df_test.loc[(df_test['Age'].isnull()) & (df_test['Initial'] == 'Other'), 'Age'] = 45

```


```python
df_train['Age'].isnull().sum()
```




    0




```python
df_test['Age'].isnull().sum()
```




    0




```python
df_train['Embarked'].isnull().sum()
```




    2




```python
df_train.shape
```




    (891, 15)




```python
df_train['Embarked'].fillna('S', inplace=True)
```


```python
df_train['Embarked'].isnull().sum()
```




    0




```python
df_train['Age_cat'] = 0
```


```python
df_train.head()
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
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
      <th>FamilySize</th>
      <th>initial</th>
      <th>Initial</th>
      <th>Age_cat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>1.981001</td>
      <td>NaN</td>
      <td>S</td>
      <td>2</td>
      <td>Mr</td>
      <td>Mr</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>4.266662</td>
      <td>C85</td>
      <td>C</td>
      <td>2</td>
      <td>Mrs</td>
      <td>Mrs</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>2.070022</td>
      <td>NaN</td>
      <td>S</td>
      <td>1</td>
      <td>Miss</td>
      <td>Miss</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>113803</td>
      <td>3.972177</td>
      <td>C123</td>
      <td>S</td>
      <td>2</td>
      <td>Mrs</td>
      <td>Mrs</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>0</td>
      <td>3</td>
      <td>Allen, Mr. William Henry</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>373450</td>
      <td>2.085672</td>
      <td>NaN</td>
      <td>S</td>
      <td>1</td>
      <td>Mr</td>
      <td>Mr</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 하드 코딩을 하는데 너무 어렵다.
df_train.loc[df_train['Age'] < 10, 'Age_cat'] = 0
df_train.loc[(10 <= df_train['Age']) & (df_train['Age'] < 20), 'Age_cat'] = 1
df_train.loc[(20 <= df_train['Age']) & (df_train['Age'] < 30), 'Age_cat'] = 2
df_train.loc[(30 <= df_train['Age']) & (df_train['Age'] < 40), 'Age_cat'] = 3
df_train.loc[(40 <= df_train['Age']) & (df_train['Age'] < 50), 'Age_cat'] = 4
df_train.loc[(50 <= df_train['Age']) & (df_train['Age'] < 60), 'Age_cat'] = 5
df_train.loc[(60 <= df_train['Age']) & (df_train['Age'] < 70), 'Age_cat'] = 6
df_train.loc[(70 <= df_train['Age']), 'Age_cat'] = 7
```


```python
df_test.loc[df_test['Age'] < 10, 'Age_cat'] = 0
df_test.loc[(10 <= df_test['Age']) & (df_test['Age'] < 20), 'Age_cat'] = 1
df_test.loc[(20 <= df_test['Age']) & (df_test['Age'] < 30), 'Age_cat'] = 2
df_test.loc[(30 <= df_test['Age']) & (df_test['Age'] < 40), 'Age_cat'] = 3
df_test.loc[(40 <= df_test['Age']) & (df_test['Age'] < 50), 'Age_cat'] = 4
df_test.loc[(50 <= df_test['Age']) & (df_test['Age'] < 60), 'Age_cat'] = 5
df_test.loc[(60 <= df_test['Age']) & (df_test['Age'] < 70), 'Age_cat'] = 6
df_test.loc[(70 <= df_test['Age']), 'Age_cat'] = 7
```


```python
df_train.head()
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
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
      <th>FamilySize</th>
      <th>initial</th>
      <th>Initial</th>
      <th>Age_cat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>1.981001</td>
      <td>NaN</td>
      <td>S</td>
      <td>2</td>
      <td>Mr</td>
      <td>Mr</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>4.266662</td>
      <td>C85</td>
      <td>C</td>
      <td>2</td>
      <td>Mrs</td>
      <td>Mrs</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>2.070022</td>
      <td>NaN</td>
      <td>S</td>
      <td>1</td>
      <td>Miss</td>
      <td>Miss</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>113803</td>
      <td>3.972177</td>
      <td>C123</td>
      <td>S</td>
      <td>2</td>
      <td>Mrs</td>
      <td>Mrs</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>0</td>
      <td>3</td>
      <td>Allen, Mr. William Henry</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>373450</td>
      <td>2.085672</td>
      <td>NaN</td>
      <td>S</td>
      <td>1</td>
      <td>Mr</td>
      <td>Mr</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>




```python
def category_age(x):
    if x < 10:
        return 0
    elif x < 20:
        return 1
    elif x < 30:
        return 2
    elif x < 40:
        return 3
    elif x < 50:
        return 4
    elif x < 60:
        return 5
    elif x < 70:
        return 6
    else:
        return 7
```


```python
df_train['Age_cat_2'] = df_train['Age'].apply(category_age)
```


```python
# 하드 코딩과 같은 결과를 내는지 봄
(df_train['Age_cat'] == df_train['Age_cat_2']).all()
```




    True




```python
df_train.drop(['Age', 'Age_cat_2'], axis=1, inplace=True)
df_test.drop(['Age'], axis=1, inplace=True)
```
