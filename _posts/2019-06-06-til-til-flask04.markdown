---
layout: post
title: '[flask] 플라스크 - 04. Flask Send & Receive'
subtitle: 
categories: til
tags: til flask
comments: true
date: 2019-06-06 00:12:17 +0900
lastmod: 2019-06-06 13:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

Flask Send & Receive<br />

# Flask Send & Receive
> [공식사이트 : http://flask.pocoo.org ](http://flask.pocoo.org) 



## Send & Receive

- app.py 코드 작성

  ```python
  from flask import Flask, render_template, request
  import random
  
  
  # ex 사용자 로그인 페이지 같은 곳, 값을 넣어서 보내는 곳
  @app.route('/send')
  def send():
      return render_template('send.html')
  
  
  # send에서 작성한 것을 receive로 받는다.
  @app.route('/receive')
  def receive():
      username = request.args.get('username')
      message = request.args.get('message')
      return render_template('receive.html', username=username, message=message)  
  
    
    
    
  # app.py 파일이 'python app.py'로 시작되었을 때 서버를 시작하겠다 라는 의미.
  if __name__ == '__main__':
      app.run(debug=True)
      # 서버가 실행이 되어있는동안 수정이 되면 자동으로 재시작을 하겠다 라는 의미
  ```

- ``send.html``  파일 생성 후 코드 작성

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>SEND</title>
  </head>
  <body>
  
      <h1>Send page</h1>
      <form action="/receive">
          사용자 이름 : <input type="text" name="username" />
          보낸 메세지 : <input type="text" name="message" />
          <input type="submit" />
      </form>
  
  </body>
  </html>
  ```

- ``receive.html``  파일 생성 후 코드 작성

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>RECEIVE</title>
  </head>
  <body>
  
      <h1>Receive page</h1>
      <p>{{ username }}님이 보낸 메세지입니다.</p>
      <p>{{ message }}</p>
  
  
  </body>
  </html>
  ```

- 터미널에서  ``flask run`` 또는 ``python app.py`` 실행하면 아래와 같은 결과를 볼 수 있다.

  - send 페이지에서 값을 입력하면 receive 페이지에 결과값이 나타난다.

  <img width="648" alt="Screen Shot 2019-06-07 at 03 08 56" src="https://user-images.githubusercontent.com/46523571/59055786-a6d48a00-88d1-11e9-9450-7ddb68a9d924.png">
  <img width="621" alt="Screen Shot 2019-06-07 at 03 09 01" src="https://user-images.githubusercontent.com/46523571/59055788-a6d48a00-88d1-11e9-98db-654640738d92.png">

<br />

<br />

<br />






## References

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>