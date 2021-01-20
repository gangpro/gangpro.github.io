---
layout: post
title: '[TIL] 06. Flask 로또 앱 만들기'
subtitle: 
categories: til
tags: til flask
comments: true
date: 2019-06-06 00:12:17 +0900
lastmod: 2019-06-06 15:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

Flask 로또 앱 만들기<br />


# Flask 로또 문제

> [공식사이트 : http://flask.pocoo.org ](http://flask.pocoo.org) 





## 로또 문제! 

> 동행복권에서 로또 당첨번호 JSON으로 가져와서 처리
>
> https://dhlottery.co.kr/common.do?method=getLottoNumber&drwNo=1



- app.py 코드 작성

  ```python
  from flask import Flask, render_template, request
  import random
  import requests
  app = Flask(__name__)
  
  
  # 사용자의 로또 input을 받는다.
  # lotto_result로 받는다.
  @app.route('/lotto_check')
  def lotto_check():
      return render_template('lotto_check.html')
  
  
  @app.route('/lotto_result')
  def lotto_result():
      # lotto_check에서 보낸 lotto_round input을 받는다.
      lotto_round = request.args.get('lotto_round')
      numbers = [int(num) for num in lotto_round.split()]
      print(lotto_round)  # 10 23 29 33 37 40
  
      # 동행복권 사이트에서 1회차 로또 당첨 번호를 JSON으로 가져온다.
      url = 'https://dhlottery.co.kr/common.do?method=getLottoNumber&drwNo=1'
      response = requests.get(url)  # Response [200] : '응답을 잘 받았다'라는 의미
      print(response.json())  # {'totSellamnt': 3681782000, 'returnValue': 'success', 'drwNoDate': '2002-12-07', 'firstWinamnt': 0, 'drwtNo6': 40, 'drwtNo4': 33, 'firstPrzwnerCo': 0, 'drwtNo5': 37, 'bnusNo': 16, 'firstAccumamnt': 863604600, 'drwNo': 1, 'drwtNo2': 23, 'drwtNo3': 29, 'drwtNo1': 10}
      json = response.json()
      drwNo = json[f'drwNo']  # 제 n 회
      bnusNo = json[f'bnusNo']  # 보너스 넘버
  
      # 방법 1
      # winner = []
      # for i in range(1, 7):
      #     winner.append(json[f'drwtNo{i}'])
      # print('winner : ', winner)  # winner :  [10, 23, 29, 33, 37, 40]
  
      # 방법 2
      winner = [json[f'drwtNo{i}'] for i in range(1, 7)] # 당첨번호
  
  
  
      # 번호 당첨 여부 확인하기
      if len(numbers) != 6:
          result = '번호의 수가 6개가 아닙니다!'
      else:
          matched = len(set(winner) & set(numbers))
          if matched == 6:
              result = '1등 당첨을 축하드립니다.'
          elif matched == 5:
              if json['bnusNo'] in numbers:
                  result = '2등 당첨을 축하드립니다.'
              else:
                  result = '3등 당첨을 축하드립니다.'
          elif matched == 4:
              result = '4등 당첨을 축하드립니다.'
          elif matched == 3:
              result = '5등 당첨을 축하드립니다.'
          else:
              result = '낙첨 되었습니다.'
  
      return render_template('/lotto_result.html', winner=winner, numbers=numbers, drwNo=drwNo, bnusNo=bnusNo, result=result)
  
    
    
  
  # app.py 파일이 'python app.py'로 시작되었을 때 서버를 시작하겠다 라는 의미.
  if __name__ == '__main__':
      app.run(debug=True)
      # 서버가 실행이 되어있는동안 수정이 되면 자동으로 재시작을 하겠다 라는 의미
  ```

- ``lotto_check.html``  파일 생성 후 코드 작성

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Lotto Check</title>
  </head>
  <body>
  
      <h1>Lotto Check Page</h1>
      <form action="/lotto_result">
          원하는 번호 6자리를 입력하세요 : <input type="text" name="lotto_round" />
          <input type="submit">
      </form>
  
  </body>
  </html>
  ```

- ``lotto_result.html``  파일 생성 후 코드 작성

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Lotto Result</title>
  </head>
  <body>
  
      <h1>Lotto Result Page</h1>
      <p>사용자가 입력한 로또 번호는 : {{ numbers }}</p>
      <p>제 {{ drwNo }}회 로또 당첨 번호는 : {{ winner }}</p>
      <p>{{ result }}</p>
  
  
  </body>
  </html>
  ```

- 터미널에서  ``flask run`` 또는 ``python app.py`` 실행하면 아래와 같은 결과를 볼 수 있다.

  - lotto_check 페이지에서 원하는 번호 6개를 입력하면 lotto_result 페이지에서 결과를 볼 수 있다.

  <img width="564" alt="Screen Shot 2019-06-07 at 03 22 28" src="https://user-images.githubusercontent.com/46523571/59056658-a76e2000-88d3-11e9-81a4-8acefd9b8893.png">
  <img width="563" alt="Screen Shot 2019-06-07 at 03 22 35" src="https://user-images.githubusercontent.com/46523571/59056660-a806b680-88d3-11e9-92a4-f36bc3135ee3.png">
  <img width="561" alt="Screen Shot 2019-06-07 at 03 22 56" src="https://user-images.githubusercontent.com/46523571/59056656-a76e2000-88d3-11e9-89a1-3a8b14a2b43e.png">
  <img width="621" alt="Screen Shot 2019-06-07 at 03 23 02" src="https://user-images.githubusercontent.com/46523571/59056659-a806b680-88d3-11e9-9a12-d6de7fac4898.png">

<br />

<br />

<br />






## References

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>