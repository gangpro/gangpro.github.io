---
layout: post
title: '[python] 파이썬 데이터 타입 - date'
subtitle: 
categories: til
tags: til python
comments: true
date: 2019-02-01 00:12:17 +0900
lastmod: 2019-02-01 19:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

파이썬 데이터 타입 - date<br />


# Python Data Type
1. 숫자형 (number)
2. 문자열 (string)
3. 리스트 (List) : 파이썬에서는 순수 배열이 없다. #자바에서 Arraylist와 유사한 형태가 바로 파이썬 list
4. 튜플 (tuple) : 자바에는 없는 존재
5. 딕셔너리 (dictionary) : key와 value가 쌍으로 구성이 되어 있는것.  #자바에서 해쉬맵과 같은 형태
6. 집합 (Set) : 자바에서 Set과 유사
7. 논리형 (bool) : True와 False가 있다. true와 false가 아니다. 첫글자는 무조건 대문자!
8. **날짜 (date)** 



# 8. 날짜 (date)
```
    from datetime import date, datetime
    # date, datetime 객체 import 하기

    # 오늘 날짜 구하기
    today = date.today()
    print(type(today))     #결과값 : <class 'datetime.date'>
    print(today)           #결과값 : 2019-02-13

    #.format함수를 사용하여 원하는 방식으로 출력할 수 있다.
    print("연도 : {}, 월 : {}, 일 : {}".format(today.year, today.month, today.day))   
    # 결과값 : 연도 : 2019, 월 : 2, 일 : 13
    
    print("{}년 {}월 {}일".format(today.year, today.month, today.day))
    #결과값 : 2019년 2월 13일

    # 오늘 날짜(연월일)와 시간(시분초)을 구해보자.
    today = datetime.today()
    print(today)        #결과값 : 2019-02-13 15:42:58.351525
    print(today.hour)   #결과값 : 15
    print(today.minute) #결과값 : 42
    print(today.second) #결과값 : 58
    print("{}시".format(today.hour))     #결과값 : 15시
    print("{}분".format(today.minute))   #결과값 : 42분
    print("{}초".format(today.second))   #결과값 : 58초
```
    
## 날짜 연산( date, datetime, timedelta, relativedelta)
```
    from datetime import date, datetime, timedelta
    # days, hours, minutes, seconds, weeks 사용 가능 O
    # timedelta로는 years, months는 사용 불가능 X
    
    from dateutil.relativedelta import relativedelta
    # relativedelta를 써야 years, months는 사용 가능 O


    today = date.today()
    days = timedelta(days=-1)   # 날짜를 기준으로 하루 전   #간격 데이터
    print(today)           # 결과값 : 2019-02-13 오늘
    print(today + days)    # 결과값 : 2019-02-12 어제
    
    days = timedelta(days=1)   # 날짜를 기준으로 하루 후
    print(today)           # 결과값 : 2019-02-13 오늘
    print(today + days)    # 결과값 : 2019-02-14 내일
    
    days = timedelta(days=-7)   # 날짜를 기준으로 일주일 전
    print(today)           # 결과값 : 2019-02-13 오늘
    print(today + days)    # 결과값 : 2019-02-06 일주일 전
    
    today = datetime.today()
    days = timedelta(hours=-7)  # 시간을 기준으로 7시간 전
    print(today)           # 결과값 : 2019-02-13 16:07:47.580668
    print(today + days)    # 결과값 : 2019-02-13 09:07:47.580668
    
    today = datetime.today()
    days = timedelta(hours=7)  # 시간을 기준으로 7시간 후
    print(today)           # 결과값 : 2019-02-13 16:07:47.580668
    print(today + days)    # 결과값 : 2019-02-13 23:07:47.580668
    
    # 연(years)과 월(months)을 사용할 시에는 
    # relativedelta 사용
    months = relativedelta(months=-2)   # 2달 전
    print(today)             #결과값 : 2019-02-13 16:15:02.987422
    print(today + months)    #결과값 : 2018-12-13 16:14:18.287608
    
    # ※ 만약 현재 날짜가 3월 31일인데 1달 전 날짜를 구하면 -> 2월 31일이 존재하지 않기 때문에 2월 28일이 된다.
```

### 날짜 연산 formatting
```
    from datetime import datetime
    
    today = datetime.today()
    
    print("현재 시간은 : {}".format(today))
    # 결과값 : 현재 시간은 : 2019-02-13 16:22:31.226415

    print("현재 시간은 : {}".format(today.strftime("%Y년 %m월 %d일")))
    # 결과값 : 현재 시간은 : 2019년 02월 13일
    
    print("현재 시간은 : {}".format(today.strftime("%y년 %m월 %d일")))    # %Y : year, %y : year, %m : month, %d: day
    # 결과값 : 현재 시간은 : 19년 02월 13일
        
    print("현재 시간은 : {}".format(today.strftime("%Y년 %m월 %d일 %H시 %M분 %S초"))) # %H : hour, %M : minute, %S : second
    # 결과값 : 현재 시간은 : 2019년 02월 13일 16시 22분 31초     
```





## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>