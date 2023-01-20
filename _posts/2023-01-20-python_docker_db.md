---
layout: post
title:  "[Python] Docker+MySQL후, 파이썬으로 데이터 DB에 적재하기"
subtitle:   "Python"
categories: dev
tags: python
comments: true
published: true
---

Docker+MySQL후, 파이썬으로 데이터 DB에 적재해보자. 나중에 써먹을거같다.

---
  
## 도커 사용해서 MySQL 설치하고 접속하기

  
### 1. mysql 이미지 불러오기
```docker pull mysql```
  
### 2. 도커 이미지 확인
```sudo docker images```
  
### 3. 도커 컨테이너 생성
```sudo docker run -d -p 3305:3306 -e MYSQL_ROOT_PASSWORD=1234 --restart=unless-stopped -v /home/ubuntu/db:/var/lib/mysql --name test_mysql mysql```
  
### 4. MySQL Workbench 설치
- hostname, port 적기(나는 원격이라 원격 hostname ip를 적었다.)
- password : 위에서 정한 1234를 적음
- username : root로 둠
  
---

## 파이썬으로 데이터를 MYSQL DB에 적재/ 업데이트 하는 방법

  
### 5. 활용 데이터 다운
- [캐글 bike 데이터](https://www.kaggle.com/c/bike-sharing-demand/data?select=train.csv) 에서 다운
  
### 6. MySQL Workbech 설치된 곳에 TABLE 생성
```File>New Query Tab```들어가서  
  
```
CREATE TABLE `sys`.`new_table` (
  `datetime` DATETIME NULL,
  `season` INT NULL,
  `holiday` INT NULL,
  `workingday` INT NULL,
  `weather` INT NULL,
  `temp` FLOAT NULL,
  `atemp` FLOAT NULL,
  `humidity` INT NULL,
  `windspeed` FLOAT NULL,
  `casual` INT NULL,
  `registered` INT NULL,
  `count` INT NULL);
```  
이렇게 쓰고 ```Execute```해주면 Table이 생성된다.


### 7.(1) 데이터를 한번에 DB로 밀어 넣기(UPSERT로) Upsert란
- ```to_sql```로 사용하면 편리하나, 꾸준히 업데이트 해줄때나, 대용량으로 데이터를 적제할때 메모리 문제가 날수 있다.
- 따라서 UPSERT를 활용한다.
- Upsert란 중복값이 있으면 업데이트 하지않고, 중복값이 없으면 Insert한다. 즉 Unique Key의 앖이 중복되지않는다면 Insert한다.

### 7.(2) 데이터를 한번에 DB로 밀어 넣기(UPSERT로) Unique Key 할당 
- Unique Key를 할당하지않으면 upsert가 작동하지 않는다.
- 여기서는 datetime 컬럼을 Unique Key로 활용한다.
  
```
ALTER TABLE `sys`.`new_table` 
CHANGE COLUMN `datetime` `datetime` DATETIME NOT NULL ,
ADD PRIMARY KEY (`datetime`),
ADD UNIQUE INDEX `datetime_UNIQUE` (`datetime` ASC) VISIBLE;
;
```
  
### 7.(3) 데이터를 한번에 DB로 밀어 넣기(UPSERT로) 파이썬으로 데이터 넣기
- Placeholder는 동적 값이 들어갈 위치에 ```%s```를 이용해 SQL문을 만들어 놓으면 된다.
  
```python
import pandas as pd
from sqlalchemy import create_engine
import pymysql
location = 'train.csv'
bike_data = pd.read_csv(location)

host = "127.0.0.1"
user = "root"
password = "1234"
database = "sys"
port = 3305 # 도커 포트

pymysql.install_as_MySQLdb()

connection = pymysql.connect(host=host,
                             port=port,
                             user=user,
                             password=password,
                             db = database,
                             )
cursor = connection.cursor()

table_name = "new_table"

query = f"""
    INSERT INTO {table_name} (
        `datetime`,
        `season`,
        `holiday`,
        `workingday`,
        `weather`,
        `temp`,
        `atemp`, 
        `humidity`,
        `windspeed`,
        `casual`,
        `registered`,
        `count`
        )
    VALUES (%s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s)
    ON DUPLICATE KEY UPDATE
        `season`=VALUES(`season`),
        `holiday`=VALUES(`holiday`),
        `workingday`=VALUES(`workingday`),
        `weather`=VALUES(`weather`),
        `temp`=VALUES(`temp`),
        `atemp`=VALUES(`atemp`),
        `humidity`=VALUES(`humidity`),
        `windspeed`=VALUES(`windspeed`),
        `casual`=VALUES(`casual`),
        `registered`=VALUES(`registered`),
        `count`=VALUES(`count`)
    """

# 데이터를 리스트화
args  = bike_data.values.tolist()

# sql 쿼리 적용
cursor.executemany(query, args)

# sql 쿼리 실행
connection.commit()

# DB연결 종료
connection.close()
```

### 8. 데이터를 한번에 DB로 밀어 넣기(UPSERT로) 파이썬으로 데이터 넣기(간결하게 짜기)

```
import pandas as pd
from sqlalchemy import create_engine
import pymysql
location = 'train.csv'
bike_data = pd.read_csv(location)

bike_data["season"] = 6
bike_data["casual"] = 30

host = "127.0.0.1"
user = "root"
password = "1234"
database = "sys"
port = 3305

pymysql.install_as_MySQLdb()

connection = pymysql.connect(host=host,
                             port=port,
                             user=user,
                             password=password,
                             db = database,
                             )
cursor = connection.cursor()

table_name = "new_table"


def query_col_list():
    query = """
        `datetime`,
        `season`,
        `holiday`,
        `workingday`,
        `weather`,
        `temp`,
        `atemp`, 
        `humidity`,
        `windspeed`,
        `casual`,
        `registered`,
        `count`
        """
    return query

def update_value():
    query = """
        `season`=VALUES(`season`),
        `holiday`=VALUES(`holiday`),
        `workingday`=VALUES(`workingday`),
        `weather`=VALUES(`weather`),
        `temp`=VALUES(`temp`),
        `atemp`=VALUES(`atemp`),
        `humidity`=VALUES(`humidity`),
        `windspeed`=VALUES(`windspeed`),
        `casual`=VALUES(`casual`),
        `registered`=VALUES(`registered`),
        `count`=VALUES(`count`)            
    """
    return query

pymysql.install_as_MySQLdb()

connection = pymysql.connect(host=host,
                             port=port,
                             user=user,
                             password=password,
                             db = database,
                             )
cursor = connection.cursor()

table_name = "new_table"

cols = bike_data.columns # 컬럼의 개수 count
value_length = str("%s, "* len(cols))[:-2] # 마지막에 공백 콤마가 붙어서 이거 떼어줘야함

query = f"""
    INSERT INTO {table_name} (
        {query_col_list()}
        )
    VALUES ({value_length})
    ON DUPLICATE KEY UPDATE
        {update_value()}
    """

args  = bike_data.values.tolist()

cursor.executemany(query, args)

connection.commit()
connection.close()
```

---
### 참고
- 참고 : https://dev-taerin.tistory.com/13
- 참고 : https://ysyblog.tistory.com/291