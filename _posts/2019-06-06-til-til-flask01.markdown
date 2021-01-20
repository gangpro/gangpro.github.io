---
layout: post
title: '[TIL] 01. Flask 설치 및 개발환경구축'
subtitle: 
categories: til
tags: til flask
comments: true
date: 2019-06-06 00:12:17 +0900
lastmod: 2019-06-06 10:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

Flask 설치 및 개발환경구축<br />


# Flask 설치 및 서버 실행 방법
> [공식사이트 : http://flask.pocoo.org ](http://flask.pocoo.org) 

## [1] Flask 설치

* 터미널에서 ``pip install flask`` 실행

  ```
  (venv) ➜  Flask git:(master) pip install Flask
  
  Collecting Flask
    Using cached https://files.pythonhosted.org/packages/9a/74/670ae9737d14114753b8c8fdf2e8bd212a05d3b361ab15b44937dfd40985/Flask-1.0.3-py2.py3-none-any.whl
  Collecting itsdangerous>=0.24 (from Flask)
    Using cached https://files.pythonhosted.org/packages/76/ae/44b03b253d6fade317f32c24d100b3b35c2239807046a4c953c7b89fa49e/itsdangerous-1.1.0-py2.py3-none-any.whl
  Collecting click>=5.1 (from Flask)
    Using cached https://files.pythonhosted.org/packages/fa/37/45185cb5abbc30d7257104c434fe0b07e5a195a6847506c074527aa599ec/Click-7.0-py2.py3-none-any.whl
  Collecting Werkzeug>=0.14 (from Flask)
    Using cached https://files.pythonhosted.org/packages/9f/57/92a497e38161ce40606c27a86759c6b92dd34fcdb33f64171ec559257c02/Werkzeug-0.15.4-py2.py3-none-any.whl
  Collecting Jinja2>=2.10 (from Flask)
    Using cached https://files.pythonhosted.org/packages/1d/e7/fd8b501e7a6dfe492a433deb7b9d833d39ca74916fa8bc63dd1a4947a671/Jinja2-2.10.1-py2.py3-none-any.whl
  Collecting MarkupSafe>=0.23 (from Jinja2>=2.10->Flask)
    Downloading https://files.pythonhosted.org/packages/ce/c6/f000f1af136ef74e4a95e33785921c73595c5390403f102e9b231b065b7a/MarkupSafe-1.1.1-cp37-cp37m-macosx_10_6_intel.whl
  Installing collected packages: itsdangerous, click, Werkzeug, MarkupSafe, Jinja2, Flask
  Successfully installed Flask-1.0.3 Jinja2-2.10.1 MarkupSafe-1.1.1 Werkzeug-0.15.4 click-7.0 itsdangerous-1.1.0
  You are using pip version 10.0.1, however version 19.1.1 is available.
  You should consider upgrading via the 'pip install --upgrade pip' command.
  
  (venv) ➜  Flask git:(master) 
  
  ```

<br />
<br />
<br />

## [2] Flask 서버 실행 방법

* (1) 모듈로 실행 : flask run

  터미널 창에서 ``flask run`` 실행하면 아래와 같이 서버 실행을 확인 할 수 있다.

  ```
  ➜  Flask git:(master) ✗ flask run 
  
   * Environment: production
     WARNING: This is a development server. Do not use it in a production deployment.
     Use a production WSGI server instead.
   * Debug mode: off
   * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
  127.0.0.1 - - [06/Jun/2019 22:44:52] "GET / HTTP/1.1" 200 -
  127.0.0.1 - - [06/Jun/2019 22:44:52] "GET /robots.txt HTTP/1.1" 404 -
  
  
  control + c : 종료
  ```

  

* (2) 직접 실행(아래와 같이 코드 작성해야 함) : python app.py

  아래와 같이 app.py 에 코드를 작성한 후에 터미널에서 ``python app.py`` 실행하면 아래와 같이 서버 실행을 확인 할 수 있다.

  ```python
  # python name.py로 설정할 수 있도록 설정 방법
  # app.py 파일이 'python app.py'로 시작되었을 때 서버를 시작하겠다 라는 의미.
  if __name__ == '__main__':
      app.run(debug=True)
      # 서버가 실행이 되어있는동안 수정이 되면 자동으로 재시작을 하겠다 라는 의미
  ```

  ```
  ➜  Flask git:(master) ✗ python app.py
  
   * Serving Flask app "app" (lazy loading)
   * Environment: production
     WARNING: This is a development server. Do not use it in a production deployment.
     Use a production WSGI server instead.
   * Debug mode: on
   * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
   * Restarting with stat
   * Debugger is active!
   * Debugger PIN: 170-447-309
  127.0.0.1 - - [06/Jun/2019 22:39:46] "GET / HTTP/1.1" 200 -
  127.0.0.1 - - [06/Jun/2019 22:39:47] "GET /favicon.ico HTTP/1.1" 404 -
  
  
  control + c : 종료
  ```




## References

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>