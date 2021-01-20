---
layout: post
title: '[TIL] 03. Flask 활용한 fake naver'
subtitle: 
categories: til
tags: til flask
comments: true
date: 2019-06-06 00:12:17 +0900
lastmod: 2019-06-06 12:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

Flask 활용한 fake naver<br />


# Flask Fake Naver

> [공식사이트 : http://flask.pocoo.org ](http://flask.pocoo.org) 


## Fake Naver

- app.py 코드 작성

  ```python
  from flask import Flask, render_template
  app = Flask(__name__)
  
  
  # Fake naver
  @app.route('/naver')
  def naver():
      return render_template('naver.html')  
  
    
    
    
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
      <title>NAVER</title>
  </head>
  <body>
  
      <form action="https://search.naver.com/search.naver">
          <input type="text" name="query" />
          <input type="submit" />
      </form>
  
  </body>
  </html>
  ```

- 터미널에서  ``flask run`` 또는 ``python app.py`` 실행하면 아래와 같은 결과를 볼 수 있다.

  - flask 서버에서 가방을 검색하면 네이버 검색창에서 검색한 것 처럼 결과가 나온다.

  <img width="320" alt="Screen Shot 2019-06-07 at 03 03 42" src="https://user-images.githubusercontent.com/46523571/59055507-ee0e4b00-88d0-11e9-95c2-c554e27c01c3.png">
  <img width="551" alt="Screen Shot 2019-06-07 at 03 03 49" src="https://user-images.githubusercontent.com/46523571/59055508-eea6e180-88d0-11e9-84a4-6ea31a21651e.png">

<br />

<br />

<br />




## References

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>