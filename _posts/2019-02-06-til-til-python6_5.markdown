---
layout: post
title: '[python] 파이썬 Pandas with OpenAPI'
subtitle: 
categories: til
tags: til python
comments: true
date: 2019-02-06 00:12:17 +0900
lastmod: 2019-02-06 15:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

파이썬 Pandas with OpenAPI에 대해서 <br />

# Pandas with OpenAPI
> 영화진흥위원회 open API 활용<br>
> http://www.kobis.or.kr/kobisopenapi/homepg/apiservice/searchServiceInfo.do

<br>
<br>

## open API 활용
    # open API를 활용해서 JSON data를 받은 다음
    # 해당 data를 처리해서 DataFrame으로 생성해보기.
    
    import numpy as np
    import pandas as pd
    import urllib     # 외부 url 호출
    import json
    
    day = "20190209"
    movie_url = "http://www.kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchDailyBoxOfficeList.json?key=430156241533f1d058c603178cc3ca0e&targetDt={}".format(day)
    
    movie_page = urllib.request.urlopen(movie_url)
    json_page = json.loads(movie_page.read())
    json_page
    
    # json_page : Dictionary
    # 박스오피스 순위 : index
    # 제목, 당일 매출액 : column
    # ~~~list 생성 for문 돌아서... 로직코드가 들어가야 한다.
<img width="668" alt="Screen Shot 2019-03-25 at 5 57 20 PM" src="https://user-images.githubusercontent.com/46523571/54906824-74003400-4f27-11e9-91c0-f4f644125a81.png">









## References

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>