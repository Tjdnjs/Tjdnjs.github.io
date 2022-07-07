---
layout : single
title : "STAGE3. Cookie & Session (2)"
categories: Security
tag: [dreamhack, webhacking, wargame]
sidebar:
    nav: "docs"
---
# 드림핵 웹 [함께실습] Cookie
## 웹 서비스 분석
### 엔드포인트 : /
index 페이지를 구성하는 코드로, 해당 페이지에서는 요청에 포함된 쿠키를 통해 이용자를 식별하며, 쿠키에 전재하는 username이 admin일 경우 FLAG를 출력한다
```python
@app.route('/') # /페이지 라우팅
def index():
    username = request.cookies.get('username', None) # 이용자가 전송한 쿠키의 username 입력값을 가져옴
    if username: # username 입력값이 존재하는 경우
        # "admin"인 경우 FLAG 출력, 아닌 경우 "you are not admin" 출력
        return render_template('index.html', text=f'Hello {username}, {"flag is " + FLAG if username == "admin" else "you are not admin"}') 
    return render_template('index.html')
```

### 엔드포인트 : /login
GET 메소드는 username과 password를 입력할 수 있는 로그인 페이지를 제공하며, POST 메소드는 이용자가 입력한 username과 password 입력 값을 users 변수값과 비교한다.
\* login 페이지 코드 <br><br>
```python
@app.route('/login', methods=['GET', 'POST']) # login 페이지 라우팅, GET/POST 메소드로 접근 가능
def login():
    if request.method == 'GET': # GET 메소드로 요청 시
        return render_template('login.html') # login.html 페이지 출력
    elif request.method == 'POST': # POST 메소드로 요청 시
        username = request.form.get('username') # 이용자가 전송한 username 입력값을 가져옴
        password = request.form.get('password') # 이용자가 전송한 password 입력값을 가져옴
        try:
            pw = users[username] # users 변수에서 이용자가 전송한 username이 존재하는지 확인
        except: 
            # 존재하지 않는 username인 경우 경고 출력
            return '<script>alert("not found user");history.go(-1);</script>' 
        if pw == password: # password 체크
            # index 페이지로 이동하는 응답 생성
            resp = make_response(redirect(url_for('index')) ) 
            # username 쿠키 설정
            resp.set_cookie('username', username) 
            return resp 
        # password가 동일하지 않은 경우
        return '<script>alert("wrong password");history.go(-1);</script>' 
```

\* users 변수 선언<br>
손님 계정의 비밀번호는 guest, 관리자 계정의 비밀번호는 파일에서 읽어온 FLAG임을 알 수 있다
```python
try:
    # flag.txt 파일로부터 FLAG 데이터를 가져옴.
    FLAG = open('./flag.txt', 'r').read() 
except:
    FLAG = '[**FLAG**]'
users = {
    'guest': 'guest',
    'admin': FLAG # FLAG 데이터를 패스워드로 선언
}
```

## 취약점 분석 & 익스플로잇 (Exploit : 취약점 공격)
users 변수 선언부를 살벼포면, 이용자의 계정을 나타내는 username 변수가 요청에 포함된 쿠키에 의해 결정되기에 이용자가 임의로 조작할 수 있다는 문제가 발생한다.
<br><br>
이러한 코드에서는 개발자도구의 Application에서 username을 admin으로 조작(쿠키변조)하면, FLAG를 획득할 수 있다. 이러한 문제를 해결하기 위해서 세션의 사용이 필요하다.

---

# 드림핵 웹 [혼자실습] session-basic
사실 위의 문제와 같은 문제라고 생각하여 코드에 명시된 아이디와 비밀번호로 로그인을 해보았으나, 여기에서는 session id의 값이 랜덤으로 암호화되어 표시되기에, 위 문제처럼 단순히 value를 변경하여 해결할 수는 없었다. 아래의 코드를 통해,세션 아이디가 32비트의 랜덤값으로 나타난다는 것과 username이 session_storage에서 가져오는 값임을 알 수 있었다.

```python
if pw == password:
    resp = make_response(redirect(url_for('index')) )
    session_id = os.urandom(32).hex()
    session_storage[session_id] = username
    resp.set_cookie('sessionid', session_id)
    return resp 
```
아래 코드를 살펴보면, 하위 페이지 중 /admin에 접속하면 session_storage를 보여주는 것을 알 수 있다. 
```python
@app.route('/admin')
def admin():
    # what is it? Does this page tell you session? 
    # It is weird... TODO: the developer should add a routine for checking privilege 
    return session_storage
```
/admin으로 접속해보면, guest와 admin의 세션 값을 알려주는 것을 확인할 수 있으며, guest로 로그인했던 정보에서 value를 admin의 세션값을 해당 값으로 바꾸어 페이지를 새로고침하면 FLAG를 얻을 수 있다. 파이썬 코드를 이해할 수만 있다면 굉장히 쉬웠던 문제 ,,
