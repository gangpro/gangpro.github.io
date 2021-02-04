---
layout: post
title: '[python] 파이썬 클래스에 대해서'
subtitle: 
categories: til
tags: til python
comments: true
date: 2019-02-04 00:12:17 +0900
lastmod: 2019-02-04 12:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

파이썬 클래스에 대해서 <br />

# Python Class
> 과거부터 프로그램이 진행이 되었다.<br>
> 기계서(0,1) -> 어셈블리언어 -> 포트란, 코블 -> C언어(절차적 언어의 대표격인 프로그래밍 언어)<br>
> 절차적 언어(프로그램을 만드는 방식(세분화))(프로시졀방식)<br>
> 기능을 기준으로 큰 문제를 작은 문제로 세분화

> 예) 은행프로그램 작성<br>
-외환업무, -대출업무, -예금업무, -보험업무...<br>
> 각각의 업무를 다시 세분화<br>
-예금업무 -> 입금업무, 출금업무, 무통장입금업무...

> 더이상 세분화 시킬 수 없을 때까지 기능을 세분화시키고<br>
> 이렇게 세분화된 기능을 실제 코드로 구현 -> 구현된 코드를 하나로 합친다.

> 절차적 언어 프로그램 장점 :<br>
(단위코드를 만들어서 합치면 되니깐 구현이 쉽다. 이는 즉 구현을 빨리 만들 수 있다. 이는 즉 비용 절감 효과)<br>
> 프로그래밍 설계가 쉽다.<br>
> 빨리 만들 수 있다.<br>
> 비용 절감<br>

> 절차적 언어 프로그램 단점 : <br>
> 프로그램을 수정하는게 어렵다. 유지보수하는게 어렵다.<br>

> 1990년대에 프로그래밍의 패러다임이 바뀌게 된다. 유지보수의 최적화된 프로그램을 만들자.<br>
> 그래서 객체지향언어 프로그래밍이 등장.(유지보수에 강점)<br>
> 객체지향프로그램 방식<br>
> 현실세계에서 해결해야하는 문제를 프로그램적으로 modeling하는 프로그래밍 기법이 객체지향방식<br>

> 객체지향 방식은 프로그램을 기능으로 세분화하지 않는다.<br>
> 내가 해결해야하는 문제를 구성하고 있는 구성요소를 파악하고(개체),<br>
> 이 구성요소들 간의 상호관계(정보전달관계)를 파악한다.<br>
> 프로그램 설계가 어렵다. 하지만 일단 잘 만들면 프로그램의 유지보수에 강점이 있다.<br>
> 현실세계의 구성요소를 파악하고 프로그램으로 옮기는 작업을 해야한다.<br>
> 프로그램에서 필요한 것들만 추린다. (abstraction)<br>
> 개체를 프로그램으로 표현하기 위한 객체 모델링의 수단이 class<br>
> class의 또 다른 의미는 새로운 data type을 만드는 것이다.<br>
> ADT( Abstract Data Type, 추상 데이터 타입)<br>
> class가 가지는 기능 중 하나는 캡슐화(encapsulation)<br>
> 관련이 있는 특성(field)과 기능(method)을 class라는 하나의 단위로 묶는 역할<br>

## python의 module
* python의 변수, 함수, 클래스를 모아 놓은 파일, 다른 python program에서 불러다 사용할 수 있는 파일
* import : module을 사용할 수 있도록 만들어 주는 keyword
* import 모듈이름(파일명)  -> 파일은 홈화면에서 text 새로 만들어서 쓰면 된다.
* import 모듈이름 as 별명
* from 모듈이름 import 함수, 변수, 클래스
```
    import module1 as m1     # module1(모듈)을 as를 이용해 m1으로 축약해서 쓸 수 있다.
    
    print(module1.PI)
    # 결과값 : 3.1415926535
    print(m1.PI)
    # 결과값 : 3.1415926535
```

```
    from module1 import my_sum    # 해당 module1에서 my_sum(해당 모듈안에 함수)을 사용한다는 의미
                                  # 여기서 module1을 라이브러리라 칭한다.
    
    print(my_sum(10, 20))
    # 결과값 : 30
```

## module을 불러올 수 있는 폴더들이 있다.
```
    import sys
    sys.path     # list
    sys.path.append("/Users/kang/Documents/Python/python_lib")      # list에 내가 원하는 요소를 붙인다.
    sys.path     # list
    # 환경변수 PYTHONPATH를 생성해서 여기에 폴더를 지정해도 module을 사용할 수 있는 폴더가 된다.
    # 결과값 :
    ['/anaconda3/envs/data_env/lib/python36.zip',
     '/anaconda3/envs/data_env/lib/python3.6',
     '/anaconda3/envs/data_env/lib/python3.6/lib-dynload',
     '',
     '/anaconda3/envs/data_env/lib/python3.6/site-packages',
     '/anaconda3/envs/data_env/lib/python3.6/site-packages/IPython/extensions',
     '/Users/kang/.ipython',
     '/Users/kang/Documents/Python/python_lib',
     '/Users/kang/Documents/Python/python_lib']
```
        
## python module을 관리하기 위한 방안
* 가장 일반적으로 쉽게 생각할 수 있는 방안
* 관련 있는 module끼리 폴더구조로 저장
* package : 관련있는 module을 묶어놓은 논리적인 집합
* package는 결국 폴더로 표현
* import 할 때 module이 package안에 포함되어 있으면
* package이름을 명시해야 한다.
```
    # 이렇게 써도 되지만 아래와 같이 from import를 일반적으로 많이 쓴다.
    
    import myPackage.inner.myModule
    print(myPackage.inner.myModule.my_variable)
    
    import myPackage.inner.myModule as haha     # as를 써서 줄일 수 있다.
    print(haha.my_variable)
```
    
## 가장 정형화된 형태
```
    from myPackage.inner import myModule     #myPackage.inner로부터 myModule 임포트해
    print(myModule.my_variable)
```

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>