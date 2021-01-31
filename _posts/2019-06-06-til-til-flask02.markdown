---
layout: post
title: '[flask] 플라스크 - 02. Flask 기초'
subtitle: 
categories: til
tags: til flask
comments: true
date: 2019-06-06 00:12:17 +0900
lastmod: 2019-06-06 11:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

Flask 기초<br />

# Flask Basic

> [공식사이트 : http://flask.pocoo.org ](http://flask.pocoo.org) 





## [1] 첫 화면 띄우기

* app.py 파일 생성 후 코드 작성

  ```python
  from flask import Flask     # Flask import
  app = Flask(__name__)       # app 초기화 과정
  
  
  @app.route("/")  # Decorator  # route 디렉토리로 들어오면 hello world를 출력시킬거란 의미
  def hello():
      return "Hello World!"  
  
    
    
    
  # app.py 파일이 'python app.py'로 시작되었을 때 서버를 시작하겠다 라는 의미.
  if __name__ == '__main__':
      app.run(debug=True)
      # 서버가 실행이 되어있는동안 수정이 되면 자동으로 재시작을 하겠다 라는 의미
  ```

* 터미널에서  ``flask run`` 또는 ``python app.py`` 실행하면 아래와 같은 결과를 볼 수 있다.

  <img width="288" alt="Screen Shot 2019-06-06 at 22 56 07" src="https://user-images.githubusercontent.com/46523571/59038651-54ce3d00-88ae-11e9-94ce-1613af13a75c.png">

<br />

<br />

<br />

## [2] 경로 변경해서 화면 띄우기

- app.py 코드 작성

  ```python
  from flask import Flask     # Flask import
  app = Flask(__name__)       # app 초기화 과정
  
  
  @app.route('/hello')
  def hello_new():
      return "Hello? Hello! New World!"  
  
    
    
    
  # app.py 파일이 'python app.py'로 시작되었을 때 서버를 시작하겠다 라는 의미.
  if __name__ == '__main__':
      app.run(debug=True)
      # 서버가 실행이 되어있는동안 수정이 되면 자동으로 재시작을 하겠다 라는 의미
  ```

- 터미널에서  ``flask run`` 또는 ``python app.py`` 실행하면 아래와 같은 결과를 볼 수 있다.

  <img width="315" alt="Screen Shot 2019-06-06 at 23 01 55" src="https://user-images.githubusercontent.com/46523571/59039082-1dac5b80-88af-11e9-87b5-dd21ce05f19c.png">

<br />

<br />

<br />

## [3] 변수 받기!

- app.py 코드 작성

  ```python
  from flask import Flask     # Flask import
  app = Flask(__name__)       # app 초기화 과정
  
  
  @app.route('/greeting/<string:name>')
  def greeting(name):     # name이라는 변수를 받겠다라는 의미
      return f'반갑습니다! {name}님!'  
  
    
    
    
  # app.py 파일이 'python app.py'로 시작되었을 때 서버를 시작하겠다 라는 의미.
  if __name__ == '__main__':
      app.run(debug=True)
      # 서버가 실행이 되어있는동안 수정이 되면 자동으로 재시작을 하겠다 라는 의미
  ```

- 터미널에서  ``flask run`` 또는 ``python app.py`` 실행하면 아래와 같은 결과를 볼 수 있다.

  <img width="406" alt="Screen Shot 2019-06-06 at 23 06 59" src="https://user-images.githubusercontent.com/46523571/59039453-d6729a80-88af-11e9-9f54-7993dd2d9678.png">

<br />

<br />

<br />

## [4] 계산 가능!

- app.py 코드 작성

  ```python
  from flask import Flask     # Flask import
  app = Flask(__name__)       # app 초기화 과정
  
  
  @app.route('/cube/<int:num>')
  def cube(num):
      result = num ** 3  # 3제곱이라는 의미(num*num*num 같다.)
      return str(result)  # return은 string 값을 가져오기 때문에 위와 return result만 하면 타입 오류가 발생한다. 그래서 str(return)을 해야한다.
    
  
    
    
    
  # app.py 파일이 'python app.py'로 시작되었을 때 서버를 시작하겠다 라는 의미.
  if __name__ == '__main__':
      app.run(debug=True)
      # 서버가 실행이 되어있는동안 수정이 되면 자동으로 재시작을 하겠다 라는 의미
  ```

- 터미널에서  ``flask run`` 또는 ``python app.py`` 실행하면 아래와 같은 결과를 볼 수 있다.

  <img width="326" alt="Screen Shot 2019-06-06 at 23 09 22" src="https://user-images.githubusercontent.com/46523571/59039626-23567100-88b0-11e9-83ea-4c05e26a18d8.png">

<br />

<br />

<br />

## [5] 랜덤 처리!

- app.py 코드 작성

  ```python
  from flask import Flask     # Flask import
  import random
  
  app = Flask(__name__)       # app 초기화 과정
  
  
  @app.route('/lunch/<int:person>')
  def lunch(person):
      # menu 라는 리스트를 만들고
      # 사람 수 만큼 랜덤 아이템을 뽑아서 반환
      menu = ['짜장면', '짬뽕', '짬짜면', '삼선고추짬뽕', '복짬면', '탕수육']
      order = random.sample(menu, person)
      
      return f'{order} 주문할게요!'  
  
    
    
    
  # app.py 파일이 'python app.py'로 시작되었을 때 서버를 시작하겠다 라는 의미.
  if __name__ == '__main__':
      app.run(debug=True)
      # 서버가 실행이 되어있는동안 수정이 되면 자동으로 재시작을 하겠다 라는 의미
  ```

- 터미널에서  ``flask run`` 또는 ``python app.py`` 실행하면 아래와 같은 결과를 볼 수 있다.

  <img width="327" alt="Screen Shot 2019-06-06 at 23 11 18" src="https://user-images.githubusercontent.com/46523571/59039779-687aa300-88b0-11e9-86ee-6035680bc429.png">







<br />

<br />

<br />

## [6] 리턴 값 꾸미기

- app.py 코드 작성

  ```python
  from flask import Flask     # Flask import
  app = Flask(__name__)       # app 초기화 과정
  
  
  @app.route('/html')
  def html():
      return '''
      <h1>Happy Hacking!</h1>
      <p>즐겁게 코딩합시다 :)</p>
      '''  
  
    
    
    
  # app.py 파일이 'python app.py'로 시작되었을 때 서버를 시작하겠다 라는 의미.
  if __name__ == '__main__':
      app.run(debug=True)
      # 서버가 실행이 되어있는동안 수정이 되면 자동으로 재시작을 하겠다 라는 의미
  ```

- 터미널에서  ``flask run`` 또는 ``python app.py`` 실행하면 아래와 같은 결과를 볼 수 있다.

  <img width="328" alt="Screen Shot 2019-06-06 at 23 12 24" src="https://user-images.githubusercontent.com/46523571/59039875-995ad800-88b0-11e9-8c2a-d82b734d6d58.png">

<br />

<br />

<br />

## [7] 파일 불러 리턴하기

- app.py 코드 작성

  ```python
  from flask import Flask, render_template
  app = Flask(__name__)
  
  
  @app.route('/html_file')
  def html_file():
      return render_template('html_file.html')  # import render_template 해야 사용 가능  
  
    
    
    
  # app.py 파일이 'python app.py'로 시작되었을 때 서버를 시작하겠다 라는 의미.
  if __name__ == '__main__':
      app.run(debug=True)
      # 서버가 실행이 되어있는동안 수정이 되면 자동으로 재시작을 하겠다 라는 의미
  ```

- ``html_file.html``  파일 생성 후 코드 작성

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>HTML</title>
  </head>
  <body>
  
      <h1>Happy Hacking!</h1>
      <p>즐겁게 코딩합시다 :D</p>
      <p>야호 신난다!!</p>
  
  </body>
  </html>
  ```

- 터미널에서  ``flask run`` 또는 ``python app.py`` 실행하면 아래와 같은 결과를 볼 수 있다.

  <img width="339" alt="Screen Shot 2019-06-06 at 23 15 31" src="https://user-images.githubusercontent.com/46523571/59040118-040c1380-88b1-11e9-9ed1-b0d528c796fd.png">

<br />

<br />

<br />

## [8]  변수 받아서 파일 실행

- app.py 코드 작성

  ```python
  from flask import Flask, render_template
  app = Flask(__name__)
  
  
  @app.route('/hi/<string:name>')
  def hi(name):
      return render_template('/hi.html', name=name)  
  
    
    
    
  # app.py 파일이 'python app.py'로 시작되었을 때 서버를 시작하겠다 라는 의미.
  if __name__ == '__main__':
      app.run(debug=True)
      # 서버가 실행이 되어있는동안 수정이 되면 자동으로 재시작을 하겠다 라는 의미
  ```

- ``hi.html``  파일 생성 후 코드 작성

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>HI</title>
  </head>
  <body>
  
      <h1>HI page</h1>
      <p>{{ name }}님 안녕하세요.</p>
  
  </body>
  </html>
  ```

  

- 터미널에서  ``flask run`` 또는 ``python app.py`` 실행하면 아래와 같은 결과를 볼 수 있다.

  <img width="382" alt="Screen Shot 2019-06-07 at 02 57 52" src="https://user-images.githubusercontent.com/46523571/59055118-19446a80-88d0-11e9-837f-5918a5284ec4.png">

<br />

<br />

<br />

## [9] 변수 받아서 계산 후 파일 실행

- app.py 코드 작성

  ```python
  from flask import Flask, render_template
  app = Flask(__name__)
  
  
  @app.route('/cube_new/<int:number>')
  def cube_new(number):
      # 계산
      result = number ** 3
      return render_template('/cube_new.html', result=result, number=number)  
  
    
    
    
  # app.py 파일이 'python app.py'로 시작되었을 때 서버를 시작하겠다 라는 의미.
  if __name__ == '__main__':
      app.run(debug=True)
      # 서버가 실행이 되어있는동안 수정이 되면 자동으로 재시작을 하겠다 라는 의미
  ```

- ``cube_new.html``  파일 생성 후 코드 작성

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>cube_new</title>
  </head>
  <body>
  
      <h1>Cube Page</h1>
      <p>{{ number }}의 3제곱은 {{ result }} 입니다.</p>
  
  </body>
  </html>
  ```

- 터미널에서  ``flask run`` 또는 ``python app.py`` 실행하면 아래와 같은 결과를 볼 수 있다.

  <img width="378" alt="Screen Shot 2019-06-07 at 03 01 18" src="https://user-images.githubusercontent.com/46523571/59055332-8c4de100-88d0-11e9-9885-402e759531b1.png">

<br />

<br />

<br />


## References

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>