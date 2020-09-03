---
layout: post
title:  "[기초] Flask 파일 업로드 하기"
subtitle:   "Flask"
categories: dev
tags: flask
comments: true
---

Flask에 대해서 좀 더 파야겠다. 오늘은 플라스크로 파일 업로드 하기를 해봤다. 시작해보자.

---

### HTML 템플릿 
- 플라스크 파일 업로드 기능을 구현하기 위해서는 `upload.html`을 `templates` 디렉터리에 넣어둬야한다.
- `upload.html` 파일을 플라스크에서 template 렌더링 후 선택한 파일을 지정된 경로로 업로드(제출하면 내 로컬에 들어와있다.)
- 라인별 설명
    - 3번라인 : html form을 사용한다.
    - 4번라인 : 파일을 업로드 할때 <form>태그에서 ENCTYPE="multipart/form-data"라는 애트리뷰트를 반드시 써야 한다.
    그렇게 하지 않으면 웹 서버로 데이터를 넘길때 파일의 경로명만 전송되고 파일 내용이 전송되지 않기 때문이다.
    그리고 이때 METHOD 애트리뷰트에는 'POST' 값을 지정해야 한다.


> ```templates/upload.html```

{ % highlight html linenos % }
```html
<html>
    <body>
        <form action = "http://localhost:5000/fileUpload" method = "POST"
            enctype = "multipart/form-data">
            <input type = "file" name = "file" />
            <input type = "submit"/>
        </form>
    </body>
</html>
```
{ % endhighlight % }



### 플라스크 파이썬 코드
- 라인별 설명
    - 3번라인 : Flask 객체 app 할당
    - 7번라인 : 브라우저 주소창에 /upload 요청시에 `upload.html`을 렌더링 하는 함수
    - 12번라인 : `upload.html`에서 form submit 시에 request된 파일 데이터를 저장하는 함수


> ```upload.html```

{ % highlight python linenos % }
```python
from flask import Flask, render_template, request
from werkzeug.utils import secure_filename
app = Flask(__name__)

# 업로드 HTML 렌더링
@app.route('/upload')
def render_file():
    return render_template('upload.html')

# 파일 업로드 처리
@app.route('/fileUpload', methods = ['GET', 'POST'])
def upload_file():
    if request.method == 'POST':
        f = request.files['file']
        # 저장할 경로 + 파일명
        f.save(secure_filename(f.filename))
        return 'uploads 디렉토리 -> 파일 업로드 성공!'

if __name__ == '__main__':
    # 서버 실행
    app.run(debug = True)
```
{ % endhighlight % }


[참고 사이트](https://niceman.tistory.com/150?category=940948)