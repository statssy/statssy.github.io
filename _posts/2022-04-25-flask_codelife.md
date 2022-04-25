---
layout: post
title:  "[생활코딩] Flask web framework 내 정리"
subtitle:   "Flask"
categories: dev
tags: flask
comments: true
published: true
---

Flask 공부할겸 정리했다. 생활코딩이 세상에서 가장 쉽고 재밌는거 같다.

---

[생활코딩-Flask web framework](hhttps://www.youtube.com/watch?v=eAifC2_4VQk) 보면서 정리
<br>

### 헷갈렸던거만 정리
- html은 따로 정리하기(꼭)
- debug = True 하면 저장할때마다 내 웹도 변경됨
- get방식
    - method에 아무것도 안쓰면 이거로(디폴트), 데이터를 가져올땐 get을 쓴다.
    - (구글에 머 검색하면 주소창에 내가 검색한게 나오는데 이건 get방식. 데이터변경할땐 post)
    - app.route는 기본 get방식인데 내가 method를 post 형식으로 하니까 받아올때 에러뜸
- post방식 : 데이터를 변경할때는 post 쓴다. Network에 Payload 들어가면 은밀하게 숨겨서 보낸다.
- flask는 요청한게 GET인지 POST인지 구분할수 있다.
- html에서 form action 쓰면 사용자가 입력한 정보를 서버로 전송하는 역할(예시에서 asdsfasfsdf 막 이렇게 예시 보여줬었음)
- redirect(url) 처럼하면 그 url로 보내버린다.
- 삭제하는 내용 만들때
    - 삭제 할땐, url 필요없다. (플러그인에는 미리 방문하거나 이런경우가 있어서 지워져버릴수도있다.) = get 방식 필요없다.
    - post방식으로만 하면 된다.



### 코드 (내가 따라하면서 써서 유튜브랑 다를지도모름)
```python
from flask import Flask, request, redirect
import random

app = Flask(__name__)


nextId = 4
topics = [
    {'id': 1, 'title': 'html', 'body': 'html is ...'},
    {'id': 2, 'title': 'css', 'body': 'css is ...'},
    {'id': 3, 'title': 'javascript', 'body': 'javascript is ...'}
]

def template(contents, content, id=None):
    contextUI = ''
    if id != None:
        contextUI = f'''
            <li><a href="/update/{id}/">update</a></li>
            <li><form action="/delete/{id}/" method="POST"><input type="submit" value="delete"></form></li>
        '''

    return f'''<!doctype html>
    <html>
        <body>
            <h1><a href="/">WEB</a></h1>
            <ol>
                {contents}
            </ol>
            {content}
            <ul>
                <li><a href="/create/">create</a></li>
                {contextUI}
            </ul>
        </body>
    </html>
    '''
def getContents():
    liTags = ''
    for topic in topics:
        liTags = liTags + f'<li><a href="/read/{topic["id"]}/">{topic["title"]}</a></li>'
    return liTags

@app.route('/')
def index():
    return template(getContents(), '<h2>Welcome</h2>Hello, WEB')

@app.route('/read/<int:id>/')
def read(id):
    title = ''
    body = ''
    for topic in topics:
        if id == topic['id']:
            title = topic['title']
            body = topic['body']
            break
    print(title, body)
    
    return template(getContents(), f'<h2>{title}</h2>{body}', id)

@app.route('/create/', methods=['GET', 'POST'])
def create():
    if request.method == 'GET':          
        content = '''
            <form action="/create/" method="POST">
                <p><input type="text" name="title" placeholder="title"></p>
                <p><textarea name="body" placeholder="body"></textarea></p>
                <p><input type="submit" value="create"></p>
            </form>
            '''
        return template(getContents(), content)
    elif request.method == 'POST':
        global nextId
        title = request.form['title']
        body = request.form['body']
        newTopic = {'id': nextId, 'title': title, 'body': body}
        topics.append(newTopic)
        url = '/read/'+str(nextId)+'/'
        nextId = nextId + 1
        return redirect(url)
    
    
@app.route('/update/<int:id>/', methods=['GET', 'POST'])
def update(id):
    if request.method == 'GET':
        title = ''
        body = ''
        for topic in topics:
            if id == topic['id']:
                title = topic['title']
                body = topic['body']
                break       
        content = f'''
            <form action="/update/{id}/" method="POST">
                <p><input type="text" name="title" placeholder="title" value="{title}"></p>
                <p><textarea name="body" placeholder="body">{body}</textarea></p>
                <p><input type="submit" value="update"></p>
            </form>
            '''
        return template(getContents(), content)
    elif request.method == 'POST':
        global nextId
        title = request.form['title']
        body = request.form['body']
        for topic in topics:
            if id == topic['id']:
                topic['title'] = title
                topic['body'] = body
                break
        url = '/read/'+str(id)+'/'
        return redirect(url)
      
@app.route('/delete/<int:id>/', methods=['POST'])
def delete(id):
    for topic in topics:
        if id == topic['id']:
            topics.remove(topic)
            break
    return redirect('/')

app.run(debug=True)
```