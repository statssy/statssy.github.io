---
layout: post
title:  "[Redis] docker-compose를 활용하여 Redis 컨테이너 사용하기"
subtitle:   "Docker"
categories: dev
tags: docker
comments: true
---

Redis란 무엇인지, docker로 빠르게 사용할수 있는지 내가 활용할수 있는지 확인겸 정리

---

### Redis란
- cache 용도로 사용되는 NoSql DB
- 인메모리 DB 이기 떄문에 컴퓨터 메모리에 보관
  
- 특징
    - List, Set, Sorted Set, Hash 등과 같은 Collection을 지원
    - persistence를 지원하여 서버가 꺼지더라도 다시 데이터를 불러들일 수 있음
        - (TIP) docker-compose를 이용하여 데이터가 영속성(컨테이너 지웠다 다시 새로 깔아도 데이터 남아있기)있게 유지할수 있게 만드는게 중요
  
- 레디스를 사용하는 경우
    - 여러 서버에서 데이터를 공유할 떄
    - 인증 토큰 저장 용(String : key-value)
    - Ranking 서버 (Sorted Set)
    - 캐싱용도
    - 좌표 저장
---

### 1. Redis 도커 이미지 가져오기
```docker pull redis:latest```
  
### 2. docker-compose.yml 작성
```
# 파일 규격 버전
version: "3.1"

# 실행하려는 컨테이너들 정의
services:  
  # 서비스명
  redis_container:
    # 사용할 이미지
    image: redis:latest
    # 컨테이너명
    container_name: redis_hd_test
    # 접근 포트 설정(컨테이너 외부:컨테이너 내부)
    ports:
      - 6379:6379
    # 스토리지 마운트(볼륨) 설정
    volumes:
      - ./redis/data:/data
      - ./redis/conf/redis.conf:/usr/local/conf/redis.conf
    # 컨테이너에 docker label을 이용해서 메타데이터 추가
    labels:
      - "name=redis"
      - "mode=standalone"
    # 컨테이너 종료시 재시작 여부 설정
    restart: always
    command: redis-server /usr/local/conf/redis.conf
```
  
### 3. docker-compose로 컨테이너 생성 및 실행
```docker-compose up -d```
  
### 4. redis-cli로 데이터 입력, 조회 확인
- 대화형 모드인 redis-cli는 타이핑으로 할수 있게 편집기능이 있음.
```docker exec -it redis_hd_test redis-cli```
- 데이터 쓰고 읽기
```
SET helloworld 15000 
GET helloworld
```
  
### 5. docker-compose로 컨테이너 삭제하고 다시 실행해서 영속성 확인(데이터확인)
```
docker-compose down
docker-compose up -d
docker exec -it redis_hd_test redis-cli
GET helloworld
```

---
(1) [참고1](https://redis.io/docs/ui/cli/)  
(2) [참고2](https://soyoung-new-challenge.tistory.com/m/117)  
(3) [참고3](https://bskyvision.com/entry/docker-docker-compose%EB%A1%9C-Redis-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88-%EC%8B%A4%ED%96%89%ED%95%98%EA%B8%B0)  
(4) [참고4](https://jungwoong.tistory.com/87)