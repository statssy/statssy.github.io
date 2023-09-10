---
layout: post
title:  "[FastAPI] '실전! FastAPI 입문' 인프런 간단 정리"
subtitle:   "API"
categories: dev
tags: api
comments: true
---

FastAPI 입문 인프런 강의 듣고 간단하게 정리. 워낙 리팩토링를 많이해서 글로 정리하긴 힘들꺼같아서, 제목 및 키워드만 정리.  
pydantic v2로 넘어가면서 안되는 것도 좀 많았음. v1 기준으로 해야함.

---

### 섹션 1. FastAPI 알아보기
- FastAPI 장점
    - 직관적인 디자인 : path, query, body, response, dependency
    - Type Hint : Pydantic, Data validation
    - 비동기처리
    - 자동 API 문서 생성 : OpenAPI, SwaggerUI
- FastAPI 세팅
    - ```uvicorn main:app --reload``` 개발 할때 코드 변경시 reload
    - ```127.0.0.1:8000/docs``` 처럼 docs 들어가면 docs를 볼수있음
- GET API - 전체조회
    - ```@app.get("/todos")``` 처럼 전체 조회
- GET API - 단일조회
    - ```@app.get("/todos/{todos_id}")``` 처럼 단일 조회
- POST API - 생성
    - request에 필요한 스키마 BaseModel로 상속받아 만들기
        ```python
        class CreateToDoRequest(BaseModel):
            id: int
            contents: str
            is_done: bool
        ```
    - ```@app.post("/todos")```로 post 사용
  
- PATCH API - 수정
    - ```@app.patch("/todos")```로 patch 사용
    - fastAPI의 Body 이용. ```is_done: bool = Body(..., embed=True)```
    - 위처럼 Body를 이용하면 post의 body처럼 is_done을 보낼 수 있게됨.
- DELETE API - 삭제
    - ```@app.delete("/todos/{todo_id}")```로 delete 사용
- HTTP Status Code
    - 2xx
    - 4xx
    - 5xx
- Error 처리
    - ```@app.get("/todos", status_code=200)``` 성공하면 이렇게 나오게함
    - ```raise HTTPException(status_code=404, detail="ToDo Not Found")``` 실패하면 이렇게 404 처럼 사용
  
### 섹션 2. 데이터베이스
- sqlalchemy 소개
    - sqlalchemy : ORM, Query, Transaction, Connection Pooling 기능 사용 가능한 python 라이브러리
    - ORM : 관계형 데이터베이스를 객체지향 프로그래밍에 대응하여 사용하는 프로그래밍 기술
        - 하나의 테이블 = 하나의 클래스, 하나의 행 = 하나의 객체)
        - 데이터 베이스가 (id|1, username|AB) 라면 python은 User(id=1, username='AB')
- 아래 내용은 강의 실습 따라서 하면 될듯. session 관련, 리팩토링성 내용이 많음
    - 전체/단일GET, POST, PATCH, DELETE
  
### 섹션 3. 테스트코드
- Pytest
- Pytest Mocking
- PyTest Fixture
- 아래 내용도 강의 실습 따라하면 될듯
    - 테스트 코드 - GET, POST, PATCH, DELETE
  
### 섹션 4. 리팩터링
- FastAPI Router
- Dependency Injection - 의존성 주입
- 래포지토리 패턴
  
### 섹션 5. 기능 고도화
- User 모델링 실습
- Lazy Loading / Eager loading
- JWT
- Caching
- OTP