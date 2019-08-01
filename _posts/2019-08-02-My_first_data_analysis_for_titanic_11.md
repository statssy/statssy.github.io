---
layout: post
title:  "[파이썬]캐글 타이타닉 데이터 탐색 #11(Feature Engineering)"
subtitle:   "PYTHON"
categories: pro
tags: python
comments: true
---


# 11. Feature engineering - Change string to categorical and Pearson coefficient


```python
df_train.Initial.unique()
```




    array([2, 3, 1, 0, 4])




```python
df_train['Initial'] = df_train['Initial'].map({'Master' : 0, 'Miss' : 1, 'Mr' : 2, 'Mrs' :3, 'Other' : 4 })
df_test['Initial'] = df_test['Initial'].map({'Master' : 0, 'Miss' : 1, 'Mr' : 2, 'Mrs' :3, 'Other' : 4 })
```


```python
df_train['Embarked'] = df_train['Embarked'].map({'C' : 0, 'Q' : 1, 'S' : 2})
df_test['Embarked'] = df_test['Embarked'].map({'C' : 0, 'Q' : 1, 'S' : 2})
```


```python
# Null Data가 없다.
df_train.Embarked.isnull().any()
```




    False




```python
df_train['Sex'].unique()
```




    array(['male', 'female'], dtype=object)




```python
df_train['Sex'] = df_train['Sex'].map({'female': 0, 'male' : 1})
df_test['Sex'] = df_test['Sex'].map({'female': 0, 'male' : 1})
```

![Pearson’s Coefficient](https://businessjargons.com/wp-content/uploads/2016/04/Karl-Pearson-final.jpg)


```python
heatmap_data = df_train[['Survived', 'Pclass', 'Sex', 'Embarked', 'FamilySize', 'Initial', 'Age_cat']]
```


```python
colormap = plt.cm.viridis
plt.figure(figsize=(12, 10))
plt.title('Pearson Correalation of Features', y=1.05, size=15)
sns.heatmap(heatmap_data.astype(float).corr(), linewidths=0.1, vmax=1.0,
           square=True, cmap=colormap, linecolor='white', annot=True, annot_kws = {'size': 16})
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f2f0a079a58>




![png](/assets/img/post_img/My_first_data_analysis_for_titanic_files/My_first_data_analysis_for_titanic_97_1.png)

