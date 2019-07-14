---
layout: post
title:  "[파이썬]캐글 타이타닉 데이터 탐색"
subtitle:   "PYTHON"
categories: pro
tags: python
comments: true
---

```python
import numpy as np # 연산
import pandas as pd # 데이터 프레임
import matplotlib.pyplot as plt # 비쥬얼라이징
import seaborn as sns # 비쥬얼라이징

# 여기서 부터는 한방에 스타일 만들기
plt.style.use('seaborn') # 디폴트가 아니고 시본 스타일로 나온다.
sns.set(font_scale=2.5) # 그림 계속 사이즈 조정하기 귀찮아서
import missingno as msno

# ignore warnings
import warnings
warnings.filterwarnings('ignore') # 업그레이드 했으면 좋겠다 이런거 무시

# matplot 쓸 때, show를 하면 윈도우가 나타나는데 이 노트북에 바로바로 볼수 있게 끔
%matplotlib inline
```


```python
# 싸이키런이라는 라이브러리를 써야한다. 모르면 안된다. 파이썬을 활용한 머신러닝이라는 책이 있는데 꼭 봐야한다.
```


```python
df_train = pd.read_csv('../input/train.csv')
df_test = pd.read_csv('../input/test.csv')
```


```python
df_train.head() # 데이터 확인
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
      <td>7.2500</td>
      <td>NaN</td>
      <td>S</td>
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
      <td>71.2833</td>
      <td>C85</td>
      <td>C</td>
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
      <td>7.9250</td>
      <td>NaN</td>
      <td>S</td>
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
      <td>53.1000</td>
      <td>C123</td>
      <td>S</td>
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
      <td>8.0500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 데이터프레임 객체입니다.
df_train.describe()
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>891.000000</td>
      <td>891.000000</td>
      <td>891.000000</td>
      <td>714.000000</td>
      <td>891.000000</td>
      <td>891.000000</td>
      <td>891.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>446.000000</td>
      <td>0.383838</td>
      <td>2.308642</td>
      <td>29.699118</td>
      <td>0.523008</td>
      <td>0.381594</td>
      <td>32.204208</td>
    </tr>
    <tr>
      <th>std</th>
      <td>257.353842</td>
      <td>0.486592</td>
      <td>0.836071</td>
      <td>14.526497</td>
      <td>1.102743</td>
      <td>0.806057</td>
      <td>49.693429</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>0.420000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>223.500000</td>
      <td>0.000000</td>
      <td>2.000000</td>
      <td>20.125000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>7.910400</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>446.000000</td>
      <td>0.000000</td>
      <td>3.000000</td>
      <td>28.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>14.454200</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>668.500000</td>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>38.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>31.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>891.000000</td>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>80.000000</td>
      <td>8.000000</td>
      <td>6.000000</td>
      <td>512.329200</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_train.shape
```




    (891, 12)




```python
df_test.describe()
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
      <th>Pclass</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Fare</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>418.000000</td>
      <td>418.000000</td>
      <td>332.000000</td>
      <td>418.000000</td>
      <td>418.000000</td>
      <td>417.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>1100.500000</td>
      <td>2.265550</td>
      <td>30.272590</td>
      <td>0.447368</td>
      <td>0.392344</td>
      <td>35.627188</td>
    </tr>
    <tr>
      <th>std</th>
      <td>120.810458</td>
      <td>0.841838</td>
      <td>14.181209</td>
      <td>0.896760</td>
      <td>0.981429</td>
      <td>55.907576</td>
    </tr>
    <tr>
      <th>min</th>
      <td>892.000000</td>
      <td>1.000000</td>
      <td>0.170000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>996.250000</td>
      <td>1.000000</td>
      <td>21.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>7.895800</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>1100.500000</td>
      <td>3.000000</td>
      <td>27.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>14.454200</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>1204.750000</td>
      <td>3.000000</td>
      <td>39.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>31.500000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1309.000000</td>
      <td>3.000000</td>
      <td>76.000000</td>
      <td>8.000000</td>
      <td>9.000000</td>
      <td>512.329200</td>
    </tr>
  </tbody>
</table>
</div>



## 1. Data Null값 확인


```python
df_train.columns
```




    Index(['PassengerId', 'Survived', 'Pclass', 'Name', 'Sex', 'Age', 'SibSp',
           'Parch', 'Ticket', 'Fare', 'Cabin', 'Embarked'],
          dtype='object')




```python
for col in df_train.columns:
    msg = 'column : {:>11} \t Percent of NAN value : {:.2f}%'.format(col, 100*(df_train[col].isnull().sum() / df_train[col].shape[0]))
    print(msg)
# {:>11} 이건 오른쪽 정렬이고 11은 몇 자리 공간을 둘껀지
# {}이게 그다음 포맷에 들어갈 자리에다가 포맷
# isnull의 개수/전체shape(행)갯수=obs갯수
```

    column : PassengerId 	 Percent of NAN value : 0.00%
    column :    Survived 	 Percent of NAN value : 0.00%
    column :      Pclass 	 Percent of NAN value : 0.00%
    column :        Name 	 Percent of NAN value : 0.00%
    column :         Sex 	 Percent of NAN value : 0.00%
    column :         Age 	 Percent of NAN value : 19.87%
    column :       SibSp 	 Percent of NAN value : 0.00%
    column :       Parch 	 Percent of NAN value : 0.00%
    column :      Ticket 	 Percent of NAN value : 0.00%
    column :        Fare 	 Percent of NAN value : 0.00%
    column :       Cabin 	 Percent of NAN value : 77.10%
    column :    Embarked 	 Percent of NAN value : 0.22%



```python
for col in df_test.columns:
    msg = 'column : {:>11} \t Percent of NAN value : {:.2f}%'.format(col, 100*(df_test[col].isnull().sum() / df_test[col].shape[0]))
    print(msg)
```

    column : PassengerId 	 Percent of NAN value : 0.00%
    column :      Pclass 	 Percent of NAN value : 0.00%
    column :        Name 	 Percent of NAN value : 0.00%
    column :         Sex 	 Percent of NAN value : 0.00%
    column :         Age 	 Percent of NAN value : 20.57%
    column :       SibSp 	 Percent of NAN value : 0.00%
    column :       Parch 	 Percent of NAN value : 0.00%
    column :      Ticket 	 Percent of NAN value : 0.00%
    column :        Fare 	 Percent of NAN value : 0.24%
    column :       Cabin 	 Percent of NAN value : 78.23%
    column :    Embarked 	 Percent of NAN value : 0.00%



```python
msno.matrix(df=df_train.iloc[:, :], figsize=(8, 8), color=(0.8, 0.5, 0.2))
# 미싱노 라이브러리 사용
# [:,:] 다 가져오겠다. 1로 2로 막 바꿔보셈
# iloc 인덱싱하는거
# null 데이터의 분포, 위치를 보고싶을때,
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7fe1d1f741d0>



![png](/_images/My_first_data_analysis_for_titanic_files/My_first_data_analysis_for_titanic_11_1.png)



```python
msno.bar(df=df_test.iloc[:, :], figsize=(8, 8), color=(0.8, 0.5, 0.2))
# 몇 퍼센트의 Null 데이터가 있는지 확인
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7fe1d2f50e10>




![png](/_images/My_first_data_analysis_for_titanic_files/My_first_data_analysis_for_titanic_12_1.png)



```python
# 타겟레이블이 얼마만큼 밸런스되어 있는가? 어떤 분포인지?
```


```python
f, ax = plt.subplots(1, 2, figsize=(18,8)) # 도화지를 꺼내는 과정 row = 1, column = 2 하나의 행에 두개의 그림, 사이즈는 가로 18 x 세로 8

df_train['Survived'].value_counts().plot.pie(explode=[0,0.1], autopct='%1.1f%%', ax=ax[0], shadow=True) 
# explode 째는거, 퍼센트, ax[0] - 어디도화지에 그릴것인가, shawdow - 그림자
ax[0].set_title('Pie plot - Survived')
ax[0].set_ylabel('')
sns.countplot('Survived', data=df_train, ax=ax[1]) # count수 세기
ax[1].set_title('Count plot -Survived')
plt.show()
```


![png](/_images/My_first_data_analysis_for_titanic_files/My_first_data_analysis_for_titanic_14_0.png)


## 2.1 Pclass


```python
df_train[['Pclass', 'Survived']].groupby(['Pclass'], as_index=True).count()
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
      <th>Survived</th>
    </tr>
    <tr>
      <th>Pclass</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>216</td>
    </tr>
    <tr>
      <th>2</th>
      <td>184</td>
    </tr>
    <tr>
      <th>3</th>
      <td>491</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_train[['Pclass', 'Survived']].groupby(['Pclass'], as_index=True).mean()
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
      <th>Survived</th>
    </tr>
    <tr>
      <th>Pclass</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>0.629630</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.472826</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.242363</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.crosstab(df_train['Pclass'], df_train['Survived'], margins=True).style.background_gradient(cmap='winter')
# margins 토탈이 있냐 없냐. # style 이거는 그냥 색깔
```




<style  type="text/css" >
    #T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4row0_col0 {
            background-color:  #0000ff;
        }    #T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4row0_col1 {
            background-color:  #0031e6;
        }    #T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4row0_col2 {
            background-color:  #000bfa;
        }    #T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4row1_col0 {
            background-color:  #0009fa;
        }    #T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4row1_col1 {
            background-color:  #0000ff;
        }    #T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4row1_col2 {
            background-color:  #0000ff;
        }    #T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4row2_col0 {
            background-color:  #009fb0;
        }    #T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4row2_col1 {
            background-color:  #0020ef;
        }    #T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4row2_col2 {
            background-color:  #006fc8;
        }    #T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4row3_col0 {
            background-color:  #00ff80;
        }    #T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4row3_col1 {
            background-color:  #00ff80;
        }    #T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4row3_col2 {
            background-color:  #00ff80;
        }</style>  
<table id="T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4" > 
<thead>    <tr> 
        <th class="index_name level0" >Survived</th> 
        <th class="col_heading level0 col0" >0</th> 
        <th class="col_heading level0 col1" >1</th> 
        <th class="col_heading level0 col2" >All</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Pclass</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4level0_row0" class="row_heading level0 row0" >1</th> 
        <td id="T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4row0_col0" class="data row0 col0" >80</td> 
        <td id="T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4row0_col1" class="data row0 col1" >136</td> 
        <td id="T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4row0_col2" class="data row0 col2" >216</td> 
    </tr>    <tr> 
        <th id="T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4level0_row1" class="row_heading level0 row1" >2</th> 
        <td id="T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4row1_col0" class="data row1 col0" >97</td> 
        <td id="T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4row1_col1" class="data row1 col1" >87</td> 
        <td id="T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4row1_col2" class="data row1 col2" >184</td> 
    </tr>    <tr> 
        <th id="T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4level0_row2" class="row_heading level0 row2" >3</th> 
        <td id="T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4row2_col0" class="data row2 col0" >372</td> 
        <td id="T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4row2_col1" class="data row2 col1" >119</td> 
        <td id="T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4row2_col2" class="data row2 col2" >491</td> 
    </tr>    <tr> 
        <th id="T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4level0_row3" class="row_heading level0 row3" >All</th> 
        <td id="T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4row3_col0" class="data row3 col0" >549</td> 
        <td id="T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4row3_col1" class="data row3 col1" >342</td> 
        <td id="T_9af5a086_a620_11e9_a8fc_bbaaed3dfee4row3_col2" class="data row3 col2" >891</td> 
    </tr></tbody> 
</table> 




```python
df_train['Survived'].unique()
```




    array([0, 1])




```python
df_train[['Pclass', 'Survived']].groupby(['Pclass'], as_index=True).mean().sort_values(by='Survived', ascending=False).plot.bar()
# 아래에 왜 index를 트루로 해야하는지 알수있다.
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7fe1bff6ea58>




![png](/_images/My_first_data_analysis_for_titanic_files/My_first_data_analysis_for_titanic_20_1.png)



```python
df_train[['Pclass', 'Survived']].groupby(['Pclass'], as_index=False).mean().sort_values(by='Survived', ascending=False).plot.bar()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7fe1c423cba8>




![png](/_images/My_first_data_analysis_for_titanic_files/My_first_data_analysis_for_titanic_21_1.png)



```python
y_position = 1.02
f, ax = plt.subplots(1, 2, figsize=(18,8))
df_train['Pclass'].value_counts().plot.bar(color=['#CD7F32', '#FFDF00', '#D3D3D3'], ax=ax[0])
ax[0].set_title('Number of passengers By Pclass', y=y_position)
ax[0].set_ylabel('Count')
sns.countplot('Pclass', hue='Survived', data=df_train, ax=ax[1]) # hue - 색으로 구분해서 나타나게 해준다.
ax[1].set_title('Pclass: Survived vs Dead', y=y_position)
plt.show()
```


![png](/_images/My_first_data_analysis_for_titanic_files/My_first_data_analysis_for_titanic_22_0.png)

