---
layout: post
title:  "[파이썬]캐글 타이타닉 데이터 탐색 #3(성별)"
subtitle:   "Analysis"
categories: data
tags: anal
comments: true
---

### 캐글 타이타닉 데이터 탐색 #3 (성별)
3번째 타이타닉 데이터 탐색을 해보려한다.

참고 : [You Han Lee 유튜브](https://www.youtube.com/watch?v=-v42Y-r9VqE&list=PLC_wC_PMBL5MnqmgTLqDgu4tO8mrQakuF&index=3)



2.2 Sex (성별))


```python
f, ax = plt.subplots(1, 2, figsize=(18,8))
df_train[['Sex', 'Survived']].groupby(['Sex'], as_index=True).mean().plot.bar(ax=ax[0])
ax[0].set_title('Survived vs Sex')
sns.countplot('Sex', hue='Survived', data=df_train, ax=ax[1])
ax[1].set_title('Sex: Survived vs Dead')
plt.show()
```


![png](/assets/img/post_img/My_first_data_analysis_for_titanic_files/My_first_data_analysis_for_titanic_24_0.png)



```python
df_train[['Sex', 'Survived']].groupby(['Sex'], as_index=True).mean().plot.bar()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f50c6b77198>




![png](/assets/img/post_img/My_first_data_analysis_for_titanic_files/My_first_data_analysis_for_titanic_25_1.png)



```python
df_train[['Sex', 'Survived']].groupby(['Sex'], as_index=False).mean()
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
      <th>Sex</th>
      <th>Survived</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>female</td>
      <td>0.742038</td>
    </tr>
    <tr>
      <th>1</th>
      <td>male</td>
      <td>0.188908</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.crosstab(df_train['Sex'], df_train['Survived'], margins=True).style.background_gradient(cmap='summer_r')
```




<style  type="text/css" >
    #T_4ca5cf7a_ae25_11e9_8c65_1b5ba89e4749row0_col0 {
            background-color:  #ffff66;
        }    #T_4ca5cf7a_ae25_11e9_8c65_1b5ba89e4749row0_col1 {
            background-color:  #77bb66;
        }    #T_4ca5cf7a_ae25_11e9_8c65_1b5ba89e4749row0_col2 {
            background-color:  #ffff66;
        }    #T_4ca5cf7a_ae25_11e9_8c65_1b5ba89e4749row1_col0 {
            background-color:  #2c9666;
        }    #T_4ca5cf7a_ae25_11e9_8c65_1b5ba89e4749row1_col1 {
            background-color:  #ffff66;
        }    #T_4ca5cf7a_ae25_11e9_8c65_1b5ba89e4749row1_col2 {
            background-color:  #8bc566;
        }    #T_4ca5cf7a_ae25_11e9_8c65_1b5ba89e4749row2_col0 {
            background-color:  #008066;
        }    #T_4ca5cf7a_ae25_11e9_8c65_1b5ba89e4749row2_col1 {
            background-color:  #008066;
        }    #T_4ca5cf7a_ae25_11e9_8c65_1b5ba89e4749row2_col2 {
            background-color:  #008066;
        }</style>  
<table id="T_4ca5cf7a_ae25_11e9_8c65_1b5ba89e4749" > 
<thead>    <tr> 
        <th class="index_name level0" >Survived</th> 
        <th class="col_heading level0 col0" >0</th> 
        <th class="col_heading level0 col1" >1</th> 
        <th class="col_heading level0 col2" >All</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Sex</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_4ca5cf7a_ae25_11e9_8c65_1b5ba89e4749level0_row0" class="row_heading level0 row0" >female</th> 
        <td id="T_4ca5cf7a_ae25_11e9_8c65_1b5ba89e4749row0_col0" class="data row0 col0" >81</td> 
        <td id="T_4ca5cf7a_ae25_11e9_8c65_1b5ba89e4749row0_col1" class="data row0 col1" >233</td> 
        <td id="T_4ca5cf7a_ae25_11e9_8c65_1b5ba89e4749row0_col2" class="data row0 col2" >314</td> 
    </tr>    <tr> 
        <th id="T_4ca5cf7a_ae25_11e9_8c65_1b5ba89e4749level0_row1" class="row_heading level0 row1" >male</th> 
        <td id="T_4ca5cf7a_ae25_11e9_8c65_1b5ba89e4749row1_col0" class="data row1 col0" >468</td> 
        <td id="T_4ca5cf7a_ae25_11e9_8c65_1b5ba89e4749row1_col1" class="data row1 col1" >109</td> 
        <td id="T_4ca5cf7a_ae25_11e9_8c65_1b5ba89e4749row1_col2" class="data row1 col2" >577</td> 
    </tr>    <tr> 
        <th id="T_4ca5cf7a_ae25_11e9_8c65_1b5ba89e4749level0_row2" class="row_heading level0 row2" >All</th> 
        <td id="T_4ca5cf7a_ae25_11e9_8c65_1b5ba89e4749row2_col0" class="data row2 col0" >549</td> 
        <td id="T_4ca5cf7a_ae25_11e9_8c65_1b5ba89e4749row2_col1" class="data row2 col1" >342</td> 
        <td id="T_4ca5cf7a_ae25_11e9_8c65_1b5ba89e4749row2_col2" class="data row2 col2" >891</td> 
    </tr></tbody> 
</table> 



2.2 Both Sex and Pclass


```python
sns.factorplot('Pclass', 'Survived', hue='Sex', data=df_train, size=6, aspect=1.5)
```




    <seaborn.axisgrid.FacetGrid at 0x7f50c6b409e8>




![png](/assets/img/post_img/My_first_data_analysis_for_titanic_files/My_first_data_analysis_for_titanic_29_1.png)


- Lady first.
- Money brings survival?


```python
sns.factorplot(x='Sex', y='Survived', hue='Pclass', data=df_train, saturation=5,
              size=9, aspect=1)
```




    <seaborn.axisgrid.FacetGrid at 0x7f50c6ccd048>




![png](/assets/img/post_img/My_first_data_analysis_for_titanic_files/My_first_data_analysis_for_titanic_31_1.png)


