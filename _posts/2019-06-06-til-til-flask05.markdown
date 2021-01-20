---
layout: post
title: '[TIL] 05. Flask 로그인 페이지 만들기'
subtitle: 
categories: til
tags: til flask
comments: true
date: 2019-06-06 00:12:17 +0900
lastmod: 2019-06-06 14:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

Flask 로그인 페이지 만들기<br />

# Flask 로그인 페이지 만들기

> [공식사이트 : http://flask.pocoo.org ](http://flask.pocoo.org) 





## Login Page 만들기

- app.py 코드 작성

  ```python
  from flask import Flask, render_template, request
  import random
  
  
  # 사용자의 uersname과 password를 input으로 받는다.
  # form action을 통해 login_check로 redirect한다.
  @app.route('/login')
  def login():
      return render_template('login.html')
  
  
  # 사용자의 입력이 admin / admin123 이 맞는지 확인한다.
  # 맞으면 '환영합니다.' 아니면 '관리자가 아닙니다'
  # 라고 출력한다.
  @app.route('/login_check')
  def login_check():
      username = request.args.get('username')
      password = request.args.get('password')
  
      if username == 'admin' and password == 'admin123':
          message = '환영합니다.'
      else:
          message = '관리자가 아닙니다.'
  
      return render_template('login_check.html', message=message)  
    
  
    
    
    
  # app.py 파일이 'python app.py'로 시작되었을 때 서버를 시작하겠다 라는 의미.
  if __name__ == '__main__':
      app.run(debug=True)
      # 서버가 실행이 되어있는동안 수정이 되면 자동으로 재시작을 하겠다 라는 의미
  ```

- ``login.html``  파일 생성 후 코드 작성

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Login</title>
  </head>
  <body>
  
      <h1>Login page</h1>
      <form action="/login_check">
          ID <input type="text" name="username" />
          PW <input type="password" name="password" />
          제출 <input type="submit" />
      </form>
  
  </body>
  </html>
  ```

  

- ``login_check.html``  파일 생성 후 코드 작성

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Login check</title>
  </head>
  <body>
  
      <h1>Login check page</h1>
      <p>{{ message }}</p>
  
  </body>
  </html>
  ```

  

- 터미널에서  ``flask run`` 또는 ``python app.py`` 실행하면 아래와 같은 결과를 볼 수 있다.

  - login 페이지에서 설정한 ID와 PW를 입력하면 login 성공 또는 실패 화면이 뜬다.

  <img width="561" alt="Screen Shot 2019-06-07 at 03 13 01" src="https://user-images.githubusercontent.com/46523571/59056061-485bdb80-88d2-11e9-88e0-c45188ae58df.png">

  <img width="633" alt="Screen Shot 2019-06-07 at 03 13 11" src="https://user-images.githubusercontent.com/46523571/59056099-5dd10580-88d2-11e9-85e0-6d2527d61988.png">

  <img width="634" alt="Screen Shot 2019-06-07 at 03 13 29" src="https://user-images.githubusercontent.com/46523571/59056062-485bdb80-88d2-11e9-9e95-a60a06bfaae5.png">

<br />

<br />

<br />






## References

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>