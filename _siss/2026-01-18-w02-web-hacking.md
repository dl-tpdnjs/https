---
title: "[Week2] Web Hacking Study - Cookie & Session"
date: 2026-01-18
domain: web_hacking
week: 2
part: 1
layout: single
toc: true
---

## 웹 해킹 스터디 2주차 🖥️

*Dreamhack Web Hacking 커리큘럼의 Cookie & Session을 공부하고 정리한 내용입니다.*

### Background: Cookie & Session
#### 들어가며 🫧
웹 서버는 수많은 클라이언트를 구별하고 요청에 따라 다른 결과를 반환해주기 위해 HTTP 프로토콜에 포함된 메소드와 URL을 사용한다.<br>
이외에도 헤더를 통해 소통을 하기도 하는데, 헤더에는 클라이언트의 인증 정보 또한 포함될 수 있다.<br>
클라이언트의 인증 정보는 **Cookie&Session**에 포함된다.

<hr>

#### 쿠키 🍪
클라이언트의 IP 주소와 User-Agent는 **고유정보가 아니다.** 또한, 아래의 HTTP 프로토콜의 특징으로 인해 웹 서버는 클라이언트를 기억할 수 없다.

>**HTTP 프로토콜의 특징**
>- **Connectionless**: 하나의 요청에 하나의 응답을 한 후 연결을 종료함
>    - 새 요청이 있을 때마다 항상 새로운 연결을 맺는다.
>- **Stateless**: 통신이 끝난 후 상태 정보를 저장하지 않는다.
>   - 이전 연결에서 사용한 데이터를 다른 연결에서 요구할 수 없다.

이를 보완하여 HTTP에서도 상태를 유지할 수 있도록 쿠키라는 개념이 등장했다. 쿠키는 **Key/Value**로 이루어진 단위로 다음과 같이 사용된다.<br>
1. 웹서버가 클라이언트에게 쿠키를 발급한다.
2. 클라이언트는 웹서버에 요청을 보낼 시 쿠키를 함께 전송한다.
3. 웹서버는 클라이언트에 요청에 포함된 쿠키를 확인하여 클라이언트를 구분한다.

**용도**<br>
일반적으로 클라이언트의 정보 기록과 상태 정보를 표현하는 용도로 사용한다.

**정보 기록**<br>
클라이언트 별로 선택 정보를 저장할 때 쿠키를 사용하였으나, 이는 서버와 통신시 리소스 낭비가 발생할 수 있다. 
- 따라서, 최근에는 Modern Storage APIs를 통한 데이터 저장을 권장한다.

**상태 정보**<br>
웹서버가 클라이언트의 로그인 상태와 이용자 구별 시 식별값을 쿠키에 저장해 사용한다.

**쿠키가 없는 통신**<br>
쿠키가 없을 경우 서버는 요청을 보낸 클라이언트가 누구인지 알 수 없기 때문에, 로그인 이후 본인의 정보를 확인하는 것이 불가능하다.

**쿠키가 있는 통신**<br>
쿠키가 있을 경우 클라이언트는 자신의 쿠키를 요청에 포함하므로 모든 요청이 유기적으로 연결되어 작동할 수 있다.

**쿠키 변조**<br>
악성 클라이언트는 쿠키 정보를 **변조**하여 서버에 요청을 보낼 수 있다. 이 때 서버가 별다른 검증 없이 쿠키만으로 사용자 인증 정보를 식별한다면, 사칭 및 정보 탈취가 가능해진다.

<hr>

#### 세션 ⏱️
인증 정보를 변조하는 것을 방지하기 위해 **세션**이 등장하였다. 세션은 인증 정보를 서버에 저장하고, 해당 인증 정보에 접근할 수 있는 **키(Session ID)**를 클라이언트에게 전달한다. 이 때, 키는 유추할 수 없도록 랜덤한 문자열로 구성되어 있다.<br>
동작 방식은 다음과 같다.
1. 브라우저는 해당 키를 쿠키에 저장한다.
2. 브라우저가 서버에 HTTP 요청을 보낼 시 키를 포함한다.
3. 서버는 요청에 포함된 키에 해당하는 데이터를 가져와 인증 상태를 확인한다.

<hr>

#### 쿠키 및 세션 실습

**쿠키 적용법**<br>
쿠키는 클라이언트에 저장되기 때문에 저장된 쿠키를 **조회/수정/추가**가 가능하다. 쿠키의 만료는 클라이언트(브라우저)에서 관리되며, 쿠키 설정 시 만료 시간을 지정할 수 있다.

**쿠키 설정**<br>
서버에서는 HTTP 응답 중 헤더에 Set-Cookie라는 쿠키 설정 해더를 추가하면 설정이 가능하다.
```
HTTP/1.1 200 OK
Server: Apache/2.4.29 (Ubuntu)
Set-Cookie: name=test;
Set-Cookie: age=30; Expires=Fri, 30 Sep 2022 14:54:50 GMT;
...
```

클라이언트에서는 자바스크립트를 사용하여 쿠키를 설정한다.
```
document.cookie = "name=test;"
document.cookie = "age=30; Expires=Fri, 30 Sep 2022 14:54:50 GMT;"
```

**쿠키 열람**<br>
크롬의 개발자 도구를 사용하면 쿠키 열람이 가능하다.
<div>
    <img src="/assets/images/siss/w2/w2whprac1.png" alt="쿠키열람">
</div>
<div>
    <img src="/assets/images/siss/w2/w2whprac2.png" alt="쿠키열람">
</div>
<br>

**연습: 드림핵 세션**<br>
*세션 실습의 경우 네이버로 대신 하였습니다.*
<div>
    <img src="/assets/images/siss/w2/w2whprac3.png" alt="세션열람">
</div>
<br>

네이버의 세션 값은 NID_SES에 저장되어 있다. 삭제 후 새로고침 시 로그아웃 된다.
<div>
    <img src="/assets/images/siss/w2/w2whprac4.png" alt="세션열람">
</div>
<br>

그러나, NID_SES만 복원하였을 때 로그인이 되지는 않다. 부수적으로 함께 관리하는 2가지의 요소가 더 있기 때문이다.

<div>
    <img src="/assets/images/siss/w2/w2whprac5.png" alt="세션열람">
</div>
<br>

따라서, 공격자가 이용자의 쿠키를 훔칠 수 있다면 인증 상태 또한 훔치는 것이 가능하다. 이를 **세션 하이재킹**이라고 한다.

#### 퀴즈 풀이
<div>
    <img src="/assets/images/siss/w2/w2whprac6.png" alt="세션열람">
</div>
- 드라이버는 이번 강의에서 다루지 않았으며, 세션은 클라이언트의 인증 정보를 관리할 때 사용하는 값이다.
<div>
    <img src="/assets/images/siss/w2/w2whprac7.png" alt="세션열람">
</div>
- 쿠키는 클라이언트에 저장되지만, 세션은 키값은 클라이언트에 인증 정보는 서버에 저장된다.
<div>
    <img src="/assets/images/siss/w2/w2whprac8.png" alt="세션열람">
</div>
- Scriptless는 다루지 않았으며, Connectionless는 한 번 통신한 후, 연결 상태를 유지하지 않는 특성을 가리키는 용어이다.

<hr>

### Exercise: Cookie
#### 들어가며
본 문제의 서버는 파이썬 Flask 프레임워크를 통해 구현되었다. 문제의 목표는 관리자 권한을 획득하여 FLAG를 획득하는 것이다.

#### 웹서비스 분서
**엔드포인트: /**<br>
```
@app.route('/') # / 페이지 라우팅 
def index():
    username = request.cookies.get('username', None) # 이용자가 전송한 쿠키의 username 입력값을 가져옴
    if username: # username 입력값이 존재하는 경우
        return render_template('index.html', text=f'Hello {username}, {"flag is " + FLAG if username == "admin" else "you are not admin"}') # "admin"인 경우 FLAG 출력, 아닌 경우 "you are not admin" 출력
    return render_template('index.html')
```
위의 인덱스 페이지에서는 쿠키에 존재하는 username이 `admin`일 경우 FLAG를 출력한다.<br>
위의 엔드포인트에 취약점이 존재하는데, 클라이언트의 요청에 포함된 쿠키를 신뢰하기 때문에 쿠키에 타 계정 정보를 삽입하면 하이재킹이 가능하다.

**엔드포인트: /login**<br>
```
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
            return '<script>alert("not found user");history.go(-1);</script>' # 존재하지 않는 username인 경우 경고 출력
        if pw == password: # password 체크
            resp = make_response(redirect(url_for('index')) ) # index 페이지로 이동하는 응답 생성
            resp.set_cookie('username', username) # username 쿠키 설정
            return resp 
        return '<script>alert("wrong password");history.go(-1);</script>' # password가 동일하지 않은 경우
```
login 페이지는 GET과 POST 메소드로 접근 가능함을 확인할 수 있으며, 각각의 기능은 다음과 같다.

> GET: `username`과 `password`를 입력할 수 있는 로그인 페이지 제공
> POST: `username`과 `paasword`의 입력값을 `users` 변숫값과 비교

#### Wargame: Cookie

문제를 해결하기 위해선 Guest 계정으로 로그인 해야한다.
```
users = {
    'guest': 'guest',
    'admin': FLAG
}
```
위 내용을 보았을 때, guest 계정의 아이디와 비번 모두 `guest`임을 알 수 있다.

guest 계정으로 로그인 시, 아래와 같은 화면이 뜬다.
<div>
    <img src="/assets/images/siss/w2/w2whprac9.png" alt="워게임">
</div>

위의 소스코드 분석에서 찾아낸 취약점을 사용하기 위해 개발자 도구의 Application에 접근하여 username을 `guest -> admin`로 변경해주었다.

<div>
    <img src="/assets/images/siss/w2/w2whprac10.png" alt="워게임">
</div>

플래그를 획득하였다!

<div>
    <img src="/assets/images/siss/w2/w2whprac11.png" alt="워게임">
</div>

<hr>

### Exercise: Cookie & Session
#### 웹서비스 분석
**엔드포인트: /**<br>
```
@app.route('/') # / 페이지 라우팅 
def index():
    session_id = request.cookies.get('sessionid', None) # 쿠키에서 sessionid 조회
    try:
        username = session_storage[session_id] # session_storage에서 해당 sessionid를 통해 username 조회
    except KeyError:
        return render_template('index.html')

    return render_template('index.html', text=f'Hello {username}, {"flag is " + FLAG if username == "admin" else "you are not admin"}')
```
해당 웹서비스는 쿠키를 통해 sessionid의 값을 획득한 후, 해당 sessionid의 username을 session_storage에서 조회한다는 것을 파악할 수 있다.

**엔드포인트: /admin**<br>
```
@app.route('/admin')
def admin():
    # developer's note: review below commented code and uncomment it (TODO)

    #session_id = request.cookies.get('sessionid', None)
    #username = session_storage[session_id] session_storage에 저장된 username을 불러옴
    #if username != 'admin': # username이 admin인지 확인
    #    return render_template('index.html')
      
    return session_storage
@app.route('/admin')
def admin():
    # developer's note: review below commented code and uncomment it (TODO)

    #session_id = request.cookies.get('sessionid', None)
    #username = session_storage[session_id] session_storage에 저장된 username을 불러옴
    #if username != 'admin': # username이 admin인지 확인
    #    return render_template('index.html')
      
    return session_storage

if __name__ == '__main__':
    import os
    # create admin sessionid and save it to our storage
    # and also you cannot reveal admin's sesseionid by brute forcing!!! haha
    session_storage[os.urandom(32).hex()] = 'admin' # username이 admin인 Session ID를 무작위로 생성
    print(session_storage)
    app.run(host='0.0.0.0', port=8000)
```
위 코드는 관리자 페이지를 구성하고 있다. admin 세션 생성 시에는 무작위 값을 생성하여 곧바로 session_storage에 저장하는 것을 확인할 수 있다. 따라서, session_storage 값에 접근할 수 있다면 곧바로 session ID를 획득할 수 있다.<br>
또한, 위 코드에서 취약점이 존재한다. 본래 session_storage는 username이 admin인 경우에만 접근할 수 있었으나, 해당 부분이 주석처리 되어 있어 인증을 거치지 않고도 조회가 가능하다.

#### Wargame: Session Basic
우선, 관리자 페이지에 접근하기 위해 `/admin`으로 URL을 수정해준다.
<div>
    <img src="/assets/images/siss/w2/w2whprac12.png" alt="워게임">
</div>

그러면 위와 같이 admin 계정에 대한 Session ID를 획득할 수 있다. 이제 guest 계정으로 로그인 한 뒤, 개발자 도구의 Application에서 sessionid를 위의 값으로 수정해주면 FLAG 조회가 가능하다.

<div>
    <img src="/assets/images/siss/w2/w2whprac13.png" alt="워게임">
</div>
<div>
    <img src="/assets/images/siss/w2/w2whprac14.png" alt="워게임">
</div>

### Mitigation: Same Origin Policy
#### 들어가며
SOP(동일 출처 정책): 다른 페이지에서 자바스크립트를 사용하여 사용자의 특정 페이지에 접근하는 것을 방지하기 위한 보안 매커니즘

#### Same Origin Policy (SOP)
브라우저는 이용자가 웹서비스에 접속할 때 해당 웹서비스에서 사용하는 쿠키를 HTTP 요청에 포함시켜 전달한다. 그러나, 브라우저는 웹 리소스를 통해 간접적으로 **타사이트**에 접근할 때도 쿠키를 함께 전송하는 특징을 가지고 있다.

**Same Origin Policy의 오리진(Origin) 구분 방법**<br>
**Origin**: 프로토콜, 포트, 호스트로 구성되며, 3가지의 구성요소가 **모두** 일치해야 동일한 오리진이라고 판단한다.

**Same Origin Policy 실습**<br>
<div>
    <img src="/assets/images/siss/w2/w2whprac15.png" alt="SOP실습">
</div>

#### Same Origin Policy 제한 완화
브라우저는 SOP의 제한에서 벗어나 외부 출처에 대한 접근을 허용해주는 경우가 존재한다.
> 1. <img>, <script>, <style> 태그
> 2. 교차 출처 리소스 공유 (Cross Origin Resource Sharing, CORS)

**Cross Origin Resource Sharing (CORS)** <br>
HTTP 헤더에 기반하여 Cross Origin 사이 리소스를 공유하는 방법이다. 발신 측에서 CORS 헤더를 설정하여 요청하면, 수신 측에서 정해진 규칙에 맞게 데이터를 가져갈 수 있도록 설정한다.
<br>
**CORS preflight**: 발신측이 웹 리소스를 요청해도 되는지 질의하는 과정이며, `OPTIONAL` 메소드를 가진 HTTP 요청을 전달한다.

**JSON with Padding (JSONP)**<br>
`<script>` 태그는 SOP에 구애받지 않는다는 특성을 활용하여 해당 태그를 활용하여 Cross Origin의 데이터를 불러오는 방법이다. 그러나, CORS가 생기기 전 주로 사용하던 방식으로 최근에는 잘 사용하지 않는다.

#### Lab: Same Origin Policy (SOP)
<div>
    <img src="/assets/images/siss/w2/w2whprac16.png" alt="SOP실습">
    <img src="/assets/images/siss/w2/w2whprac17.png" alt="SOP실습">
    <img src="/assets/images/siss/w2/w2whprac18.png" alt="SOP실습">
</div>

#### 퀴즈
*개념 문제들로 구성되어 있어, 설명은 생략하였습니다.*
<div>
    <img src="/assets/images/siss/w2/w2whprac19.png" alt="SOP퀴즈">
    <img src="/assets/images/siss/w2/w2whprac20.png" alt="SOP퀴즈">
    <img src="/assets/images/siss/w2/w2whprac21.png" alt="SOP퀴즈">
    <img src="/assets/images/siss/w2/w2whprac22.png" alt="SOP퀴즈">
</div>