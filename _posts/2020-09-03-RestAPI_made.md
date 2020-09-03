---
layout: post
title:  "[기초] Flask RestFul API 서버 만들기"
subtitle:   "Flask"
categories: dev
tags: flask
comments: true
---

진짜 Flask가 Restful이 먼지 API가 먼지 하나도 모르겠었지만, 여전히 잘 모르겠지만.. 일단 부딫혀보자.  
지인의 지인이 추천해준 사이트를 토대로 이 새벽공기를 마시며 내 나름 정리해본다.  

---


## Flask RestFul API 서버 만들기

### 나만의 정리
- Flask-Restful : 파이썬 Flask 프레임워크를 확장해서 만든 REST API 작성을 위한 경량화 된 프레임워크
- 코드 설명 :
    - Flask 인스턴스를 만들고
    - 기존에 있는 할일 정의, 예외 처리(abort이용)
    - parser 만들고
    - Todo에는 기존에 있는 할일을 get(READ), delete, put(UPDATE) 하게끔
    - TodoList는 Post(Create) 할수 있게 함
    - URL Router에 맵핑(참고로 필요한 url들을 만들고 이 url에 맞는 페이지들을 연결하는 것을 라우팅이라고한다.)
    - 서버 실행
- Postman 프로그램으로 새로운 할일을 Post하고 Get해보고 Delete 해봤더니 성공


### 코드 
```python
from flask import Flask
from flask_restful import reqparse, abort ,Api, Resource

# Flask 인스턴스 생성
app = Flask(__name__)
api = Api(app)

# 할일 정의
TODOS = {
    'todo1': {'task': 'Listen Music'},
    'todo2': {'task': 'Go to Gym'},
    'todo3': {'task': 'Study!'}
}

# 예외 처리 (상태코드 설명 : https://ko.wikipedia.org/wiki/HTTP_%EC%83%81%ED%83%9C_%EC%BD%94%EB%93%9C)
def abort_if_todo_doesnt_exist(todo_id):
    if todo_id not in TODOS:
        abort(404, message="Todo {} doesn't exist".format(todo_id)) # abort : 강제 프로그램 종료

parser = reqparse.RequestParser()
parser.add_argument('task')

# 할일(Todos)
# Get, Delete, Put 정의
class Todo(Resource):
    def get(self, todo_id):
        abort_if_todo_doesnt_exist(todo_id)
        return TODOS[todo_id]

    def delete(self, todo_id):
        abort_if_todo_doesnt_exist(todo_id)
        del TODOS[todo_id]
        return '', 204

    def put(self, todo_id):
        args = parser.parse_args()
        task = {'task': args['task']}
        TODOS[todo_id] = task
        return task, 201

# 할일 리스트(Todos)
# Get, POST 정의
class TodoList(Resource):
    def get(self):
        return TODOS

    def post(self):
        args = parser.parse_args()
        todo_id = 'todo%d' % (len(TODOS)+1)
        TODOS[todo_id] = {'task': args['task']}
        return TODOS[todo_id], 201

##
## URL Router에 맵핑한다.(Rest URL 정의)
##
api.add_resource(TodoList, '/todos/')
api.add_resource(Todo, '/todos/<string:todo_id>')

# 서버 실행
if __name__ == '__main__':
    app.run(debug=True)
```


[참고 사이트](https://niceman.tistory.com/101)
