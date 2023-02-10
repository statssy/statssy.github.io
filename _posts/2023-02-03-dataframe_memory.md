---
layout: post
title:  "[Pandas] 데이터프레임 메모리 사용 최적화 정리"
subtitle:   "Python"
categories: dev
tags: python
comments: true
published: true
---

데이터프레임에서 메모리 사용을 최저고하 하는 방법 정리

---
  
## 1. DataFrame에 대한 내부 수정
### 1.1 표준할당(Standard Assignment) 
- DataFrame의 새 복사본을 만들고 원본도 그대로 둔다.

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({"col1" : [1, 4, np.NaN],
"col2" : ["A", "B", "C"],
"col3" : [1.3, 2.9, 5.6]})

# 표준할당
dfFillna0 = df.fillna(0)
dfFillna0
```
  
### 1.2 내부할당(inplace Assigmnet Operations)
- 원래 DataFrame 자체를 수정
  
```python
# 내부 할당 작업
df.fillna(0, inplace = True)
df
```
  
## 2. CSV에서 필수 열(column)만 읽기
### 2.1 모든 열 로딩하기
  
```python
peopleData.info(memory_usage = "deep") # 메모리 사용량
%timeit pd.read_csv("D:/People_data.csv") # csv파일 로드 런타임
```
  
### 2.2 필요한 열만 로딩하기
- 관심있는 컬럼만 로드하면 메모리 사용률 3배나 감소했음

```python
colList = ["Name", "Birth Date", "State", "Year", "Link"]
data = pd.read_csv("D:/People_data.csv", usecols=colList)
data.info(memory_usage = "deep")
```

- 로딩시간도 2배 가깝게 감소

```python
%timeit pd.read_csv("D:/People_data.csv", usecols=colList)
```
  

## 3. 정수 열의 데이터 유형 변경
  
- 기본적으로 Pandas는 항상가장 높은 메모리 데이터 유형을 열에 할당
- astype()메소드를 사용하여 데이터 유형을 변경할수 있음
  
```python
print("데이터 유형 변환 전 메모리 사용량:", peopleData["Zip Code"].memory_usage())
peopleData["Zip Code"] = peopleData["Zip Code"].astype(np.int32)
print("데이터 유형 변환 후 메모리 사용량:", peopleData["Zip Code"].memory_usage())
```
  
## 4. 범주형 데이터를 나타내는 열의 데이터 유형 변경
- ```dtype```해보면 object 문자열 유형으로 나올경우 category로 바꿔주면 메모리 사용률이 감소
  
```python
print("데이터 유형 변환 전 메모리 사용량:", peopleData["Prefix"].memory_usage())
peopleData["Prefix"] = peopleData["Prefix"].astype("category")
print("데이터 유형 변환 후 메모리 사용량:", peopleData["Prefix"].memory_usage())
```
  
## 5. NaN 값으로 열의 데이터 유형 변경
- astype() 메서드를 사용하여 희소 열의 데이터 유형을 ```Sparse[str]/Sparse[float]/Sparse[int]``` 데이터 유형으로 변경
  
```python
data["missing값을 가지는 열이름"] = data["missing값을 가지는 열이름"].astype("Sparse[float32]")
```
  
## 6. CSV를 읽는 동안 열 데이터 유형 지정
- 앞에 #3~#5는 사후 팁이었고, CSV 읽을때 바로 제어할수 있게
  
```python
colList = ["Name", "Birth Date", "State", "Year", "Link"]
data = pd.read_csv("D:/People_data.csv", usecols=colList,
dtype = {"Year":np.int16, "State":"category"})
data.info(memory_usage = "deep")
```
  
## 7. 청크로 CSV 데이터 읽기
- pandas 읽을때 행 수가 너무많아서 메모리 로드 할수 없을때는 chunksize 인수로 읽으면됨
  
```python
for chunk in pd.read_csv("D:/People_data.csv", chunksize=5000):
## process chunk
pass
```
  
---
[참고](https://zzinnam.tistory.com/entry/%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%B2%98%EB%A6%AC-%EC%8B%9C-%EC%95%8C%EC%95%84%EC%95%BC-%ED%95%A0-7%EA%B0%80%EC%A7%80-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EC%B5%9C%EC%A0%81%ED%99%94-%EA%B8%B0%EC%88%A0)