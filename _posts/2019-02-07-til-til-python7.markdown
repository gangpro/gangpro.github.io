---
layout: post
title: '[python] 파이썬 Matplotlib'
subtitle: 
categories: til
tags: til python
comments: true
date: 2019-02-07 00:12:17 +0900
lastmod: 2019-02-07 11:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

파이썬 Matplotlib에 대해서 <br />

# Matplotlib
> 결과값을 수치가 아닌 그래프로 나타내는 함수

<br>

## %matplotlib inline과 %matplotlib nbagg
    # numpy나 pandas를 이용해서 데이터 분석을 한 후
    # 해당 결과를 수치로 보는것보다는 그래프로 보는게 좋다.
    
    # magic function(※jupyter notebook에서만 사용가능한 명령어)
    # matplotlib 뒤에 어떤 형태로 사용할 건지 지정할 수 있다.
    # matplotlib + 명령어 다음에 주석을 쓰면 오류 발생하니 참고하자.

    %matplotlib inline
    # inline : 그래프를 만든 후 output 영역에 그림을 넣어준다.
    # graphe에 대한 추가적인 조작을 할 수 없다. ex) 프린트한것 처럼 조작이 불가능

    %matplotlib nbagg
    # nbagg : 그래프를 만든 후 interactive(상호작용)하게 그래프를 조작할 수 있는 창이 뜬다.
    # 이후 만들어진 그래프를 조작 후 내가 설정한 값으로 그래프를 저장할 수 있다.
    # Stop interaction 버튼 누르면 내가 정한 설정값으로 그래프가 저장된다.

## numpy를 이용한 line graphe 작성
    %matplotlib inline
    
    import numpy as np
    import matplotlib
    import matplotlib.pyplot as plt
    
    # matplotlib의 기본 설정
    plt.rcParams["axes.grid"] = True         # 옵션 : 그래프 내 격자 표현 여부
    plt.rcParams["figure.figsize"] = (8,4)   # 옵션 : 그래프 크기 표현   # 튜플형태 정수(가로,세로)
    
    # plotting할 데이터
    x_data = range(50)
    y_data = np.random.randint(-100,100,50).cumsum() 
    # .cumsum() 난수 발생 후 각자의 난수를 더한 후 다음 값으로 지정
    
    plt.title("가격변동 추이")   # 옵션 : 그래프의 제목
    plt.xlabel("기간(일))")     # 옵션 : x축 의미
    plt.ylabel("가격")          # 옵션 : y축 의미
    
    plt.plot(x_data,y_data,"r")   # (x축, y축, 색옵션)
                                  # "r" red, "g" green, "b" blue, "y" yellow, "w" white
    display(plt.show())
    # 한글이 깨진다... 해결방법은 아래와 같다.
<img width="665" alt="Screen Shot 2019-03-26 at 12 26 59 AM" src="https://user-images.githubusercontent.com/46523571/54932300-ebe95100-4f5d-11e9-8c4d-f4eb022a3c32.png">

## 맥에서 사용가능한 시스템의 TTF 폰트 목록 확인하기
    import matplotlib.font_manager as font_manager
    
    font_list = font_manager.findSystemFonts(fontpaths=None, fontext='ttf')
    
    # 전체 개수
    print(len(font_list)) 
    
    # 처음 10개만 출력
    font_list[:10] 
<img width="635" alt="Screen Shot 2019-03-26 at 12 28 24 AM" src="https://user-images.githubusercontent.com/46523571/54932391-15a27800-4f5e-11e9-8b68-8a0fde1c508f.png">

## 맥에서 사용 가능한 ttf 폰트 목록
* 사용가능한 폰트 목록 중 Apple글꼴(이름에 'Apple'이 포함된) 폰트 목록만 추출
###
    import matplotlib
    import matplotlib.font_manager
    
    [f.name for f in matplotlib.font_manager.fontManager.ttflist if 'Apple' in f.name]
<img width="207" alt="Screen Shot 2019-03-26 at 12 29 46 AM" src="https://user-images.githubusercontent.com/46523571/54932521-484c7080-4f5e-11e9-9728-7e2b0e46e275.png">

## 맥에서 사용 가능한 ttf 폰트 경로 찾기
* 사용가능한 폰트 목록 중 Apple글꼴(이름에 'Apple'이 포함된) 폰트 경로 추출 
###
    import matplotlib
    import matplotlib.font_manager
    
    
    [(f.name, f.fname) for f in matplotlib.font_manager.fontManager.ttflist if 'Apple' in f.name]
<img width="838" alt="Screen Shot 2019-03-26 at 12 30 30 AM" src="https://user-images.githubusercontent.com/46523571/54932584-65813f00-4f5e-11e9-8915-50161f9759db.png">

## 폰트를 지정하는 두가지 방법
* matplotlib에서 폰트를 지정하여 출력하는 하는 대표적인 방법은 크기 두 가지이다.
  * 텍스트 출력시 폰트 특성을 지정: FontProperties 속성으로 폰트 속성을 지정
  * 전역으로 지정: rcParams 을 사용하여 전역으로 설정값을 지정

## 텍스트 출력시 폰트 특성을 지정
* 텍스트를 출력하는 다음 함수들을 사용할 때, fontproperties 인자에 폰트를 지정할 수 있다.
* matplotlib.pyplot
  - title()
  - xlabel()
  - ylabel()
  - legend()
  - text()
  - matplotlib.axes

* set_title()
  - 예를 들어, 다음과 같이 텍스트와 폰트를 지정하여 출력할 수 있다.
  - plt.text(0, 0, "Sample Text", fontproperties=fontprop)

## 그래프 내 한글처리 방법(1) - font_prop
    %matplotlib nbagg
    
    import numpy as np
    import matplotlib
    import matplotlib.pyplot as plt
    import matplotlib.font_manager as fm
    
    # matplotlib의 기본 설정
    plt.rcParams["axes.grid"] = True         # 옵션 : 그래프 내 격자 표현 여부
    plt.rcParams["figure.figsize"] = (8,4)   # 옵션 : 그래프 크기 표현   # 튜플형태 정수(가로,세로)
    
    # 사용할 font의 properties를 생성
    font_path = "/Users/kang/Library/Fonts/조선일보명조.ttf"
    font_prop = fm.FontProperties(fname=font_path, size=14)
    
    # plotting할 데이터
    x_data = range(50)
    y_data = np.random.randint(-100,100,50).cumsum() 
    # .cumsum() 난수 발생 후 각자의 난수를 더한 후 다음 값으로 지정
    
    plt.title("가격변동 추이", fontproperties=font_prop)   # 옵션 : 그래프의 제목
    plt.xlabel("기간(일))", fontproperties=font_prop)     # 옵션 : x축 의미
    plt.ylabel("가격", fontproperties=font_prop)          # 옵션 : y축 의미
    
    plt.plot(x_data,y_data,"r")   # (x축, y축, 색옵션)
                                  # "r" red, "g" green, "b" blue, "y" yellow, "w" white
    plt.show()
<img width="1045" alt="Screen Shot 2019-03-26 at 12 33 30 AM" src="https://user-images.githubusercontent.com/46523571/54932784-ca3c9980-4f5e-11e9-8169-035f94246f82.png">

## 그래프 내 한글처리 방법(2) - font_family
    %matplotlib inline
    
    import numpy as np
    import matplotlib
    import matplotlib.pyplot as plt
    import matplotlib.font_manager as fm
    
    # matplotlib의 기본설정들
    plt.rcParams["axes.grid"] = False            # 옵션 : 격자 설정
    plt.rcParams["figure.figsize"] = (8,4)
    plt.rcParams["axes.unicode_minus"] = False   # '-'글자로 인한 문제발생 제거
    plt.rcParams["font.size"] = 16               # 옵션 : 폰트 크기
    plt.rcParams["xtick.labelsize"] = 10.        # 옵션 : x축 틱 사이즈
    plt.rcParams["ytick.labelsize"] = 10.        # 옵션 : y축 틱 사이즈
    plt.rcParams["axes.labelsize"]               # 옵션 : 레이블 크기 설정
    
    # 전역으로 사용할 font를 설정
    font_path = "/Users/kang/Library/Fonts/조선일보명조.ttf"
    font_family = fm.FontProperties(fname=font_path).get_name()
    plt.rcParams["font.family"] = font_family
    
    # plotting할 데이터
    x_data = range(50)
    y_data = np.random.randint(-100,100,50).cumsum() 
    # .cumsum() 난수 발생 후 각자의 난수를 더한 후 다음 값으로 지정
    
    plt.title("가격변동 추이")   # 옵션 : 그래프의 제목
    plt.xlabel("기간(일))")     # 옵션 : x축 의미
    plt.ylabel("가격")          # 옵션 : y축 의미
    
    plt.plot(x_data,y_data,"r")   # (x축, y축, 색옵션)
                                  # "r" red, "g" green, "b" blue, "y" yellow, "w" white
    plt.show()
<img width="651" alt="Screen Shot 2019-03-26 at 12 34 38 AM" src="https://user-images.githubusercontent.com/46523571/54932871-f35d2a00-4f5e-11e9-81b7-ff83fc2dcd5d.png">

## Series 을 이용한 line plotting  
    import numpy as np
    import pandas as pd
    import matplotlib
    import matplotlib.pyplot as plt
    
    # plotting할 데이터
    s = pd.Series(np.random.randn(10),
                 index=np.arange(0,100,10))
     
    plt.title("가격변동 추이")    # 옵션 : 그래프의 제목
    plt.xlabel("기간(일))")      # 옵션 : x축 의미
    plt.ylabel("가격")           # 옵션 : y축 의미
    # Series인 경우 index를 x축으로 value를 y축으로 사용
    plt.plot(s,label="오렌지가격")
    plt.legend()                 # 옵션 : 범례 표현
    plt.show()
<img width="651" alt="Screen Shot 2019-03-26 at 12 35 46 AM" src="https://user-images.githubusercontent.com/46523571/54932963-1b4c8d80-4f5f-11e9-945e-8536902b113e.png">

## DataFrame을 이용한 line plotting
    # Series의 column 단위로 그래프를 그려준다.
    
    import numpy as np
    import pandas as pd
    import matplotlib
    import matplotlib.pyplot as plt
    
    # plotting할 데이터
    df = pd.DataFrame(np.random.randint(50,100,(10,3)),
                     columns=["이지안","박동훈","홍길동"],
                     index=np.arange(0,10,1))
    
    plt.title("가격변동 추이")   # 옵션 : 그래프의 제목
    plt.xlabel("기간(일))")     # 옵션 : x축 의미
    plt.ylabel("가격")          # 옵션 : y축 의미
    # Series인 경우 index를 x축으로 value를 y축으로 사용
    plt.plot(df)
    plt.legend()               # 옵션 : 범례 표현
    plt.show()
<img width="672" alt="Screen Shot 2019-03-26 at 12 36 40 AM" src="https://user-images.githubusercontent.com/46523571/54933020-3b7c4c80-4f5f-11e9-8f9c-f329b860c1b4.png">

## 예제(Series) - 한 사람의 국어, 영어, 수학 점수를 표현
    import numpy as np
    import pandas as pd
    import matplotlib
    import matplotlib.pyplot as plt
    
    # plotting할 데이터
    s = pd.Series(np.random.randint(50,100,(3,)),   # 50~100사이 3개
                 index=["국어","영어","수학"])
    
    # plt.plot(s)    &    s.plot() 둘다 사용가능
    #plt.plot(s)
    s.plot(kind="bar", label="각 과목 성정")      # 옵션 : 그래프 수직방향 표시
    # s.plot(kind="barh", label="각 과목 성정")   # 옵션 : 그래프 수평방향 표시
    plt.legend()
    plt.title("성적표")    # 옵션 : 그래프의 제목
    plt.xlabel("과목")    # 옵션 : x축 의미
    plt.ylabel("점수")    # 옵션 : y축 의미
    plt.show()
<img width="670" alt="Screen Shot 2019-03-26 at 12 37 32 AM" src="https://user-images.githubusercontent.com/46523571/54933099-5b137500-4f5f-11e9-8828-b0a70b6bf553.png">

## 예제(DataFrame) - 여러 사람의 국어, 영어, 수학, 과학 점수를 표현
    import numpy as np
    import pandas as pd
    import matplotlib
    import matplotlib.pyplot as plt
    
    # plotting할 데이터
    # 사람별 과목 그래프
    df1 = pd.DataFrame(np.random.randint(50,100,(3,4)),   #(3,4)(3행 = 3명, 4열 = 4과목 지칭)
                    columns=["국어","영어","수학","과학"],   # 4열
                    index=["이지안","박동훈","홍길동"])      # 3행
    # 과목별 사람 그래프
    df2 = pd.DataFrame(np.random.randint(50,100,(4,3)),   #(4,3)(4행 = 4명, 3열 = 3과목 지칭)
                    columns=["이지안","박동훈","홍길동"],    # 3열
                    index=["국어","영어","수학","과학"])     # 4행
    
    # plt.plot(df)    &    df.plot() 둘다 사용가능
    #plt.plot(df)
    df1.plot(kind="bar")
    df2.plot(kind="bar")
    plt.show()
<img width="638" alt="Screen Shot 2019-03-26 at 12 38 15 AM" src="https://user-images.githubusercontent.com/46523571/54933158-754d5300-4f5f-11e9-9d29-2fa56a8aaf92.png">

## 산점도(scatter)
* 두개의 독립변수 X, Y의 관계를 알아보기 위해 사용.
###
    %matplotlib inline
    
    import numpy as np
    import matplotlib
    import matplotlib.pyplot as plt
    
    x = np.random.normal(1,1,(100,1))
    y = np.random.normal(-2,4,(100,1))
    
    plt.scatter(x,y)
    plt.show()
<img width="499" alt="Screen Shot 2019-03-26 at 12 39 03 AM" src="https://user-images.githubusercontent.com/46523571/54933217-901fc780-4f5f-11e9-9e1a-e11ff80e9525.png">


## References

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>