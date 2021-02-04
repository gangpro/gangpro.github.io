---
layout: post
title: '[python] 파이썬 데이터 타입 - String'
subtitle: 
categories: til
tags: til python
comments: true
date: 2019-02-01 00:12:17 +0900
lastmod: 2019-02-01 13:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

파이썬 데이터 타입 - String<br />


# Python Data Type
1. 숫자형 (number)
2. **문자열 (string)**
3. 리스트 (List) : 파이썬에서는 순수 배열이 없다. #자바에서 Arraylist와 유사한 형태가 바로 파이썬 list
4. 튜플 (tuple) : 자바에는 없는 존재
5. 딕셔너리 (dictionary) : key와 value가 쌍으로 구성이 되어 있는것.  #자바에서 해쉬맵과 같은 형태
6. 집합 (Set) : 자바에서 Set과 유사
7. 논리형 (bool) : True와 False가 있다. true와 false가 아니다. 첫글자는 무조건 대문자!
8. 날짜 (date)



# 2. 문자열 (string)
```
    a = 'Hello World'
    b = "Hello World"
    c = "Hello\nWorld"     # 여러줄에 걸친 문자열을 만들고 싶을 때 (자바에서는 이스케이프 문자(\n)를 사용) #개행문자를 이용
    print(c)
    # 결과값 : Hello
              World
    
    d = """
    Hello
    World
    여러줄에 걸친 문자열
    """                 # """ 주석 """ <- 주석처리인데 문자열에서 안에 내용을 넣어 변수에 저장할 수 있다. 
    print(d)                             # 이럴땐 주석이 아닌 또다른 문자열을 만들어 내는 방법이다.
    # 결과값 : Hello
              World
              !!!
```
      
## 문자열에 대한 "+ 연산" 부연 설명
* 일반적인 문자열의 연결은 "+" 기호를 이용한다.
```
    first = "This is a"
    middle = " sample"
    last = " Text."
    print(first + middle + last)
    # 결과값 : This is a sample Text.
```
```
    first = "This is a"
    middle = " sample"
    last = " Text."
    a = 100
    result = first + a     # 파이썬에서는 문자열과 숫자를 "+" 할 수 없다.   
                           # 자바에서는 문자+숫자는 숫자가 문자로 바뀌어 합쳐진다. 그러나 파이썬에서는 불가능
    print(result)
    # 결과값 : 역시 아래와 같이 오류 발생.
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    <ipython-input-3-76ae67548cd9> in <module>
          8 
          9 a = 100
    ---> 10 result = first + a
         11 #result = first + str(a)                  #str() 안에 들어온 것을 문자열로 바꾸는 함수
         12 print(result)
    
    TypeError: must be str, not int
```
```
    first = "This is a"
    middle = " sample"
    last = " Text."
    a = 100
    result = first + str(a)     # str() : 문자열로 변환     #str() 안에 들어온 것을 문자열로 바꾸는 함수
    print(result)
    # 결과값 : This is a100
```

## 문자열에 대한 "* 연산" 부연 설명
```
    text = "python!"
    result = text * 3     # text+text+text와 같은 의미이다.
    print(result)
    # 결과값 : python!python!python!
```

## 문자열 indexing     #배열처럼 쓸 수 있다.
```
    str = "Talk is a cheap. Show me the Code"
    print(str[0])     # 0번째 index의 값을 알아오는것.
    # 결과값 : T
    
    print(str[5])
    # 결과값 : i
    
    print(str[-1])     # 맨뒤에서 첫번째 문자를 가져온다.     #자바에서는 불가능
    # 결과값 : e
    
    print(str[-4])
    # 결과값 : C
```

## 문자열 slicing (즉 서브문자 가져오기)
```
    str = "Talk is a cheap. Show me the Code"
    result = str[0:4]     #0번째에서 3번째까지 총 4개 가져오기
                            # [idx1:idx2] 
                            # idx1 = inclusive(포함)
                            # idx2 = exclusive(불포함)
    
    result = str[5:]     #idx2가 없으면 N부터 끝까지
    result = str[:10]    #idx1가 없으면 처음부터 N까지
    result = str[:]      #둘다 없으면 처음부터 끝까지 다 가져오기
    
    print(result)  
    # 결과값 : Talk is a cheap. Show me the Code
```

## 문자열 안에 연산자
```
    str = "Talk is a cheap. Show me the Code"
    
    print("cheap" in str)     # str 안에 해당 문자열("cheap")이 있니 없니? 결과값 있으면 True가 나온다.     
    # 결과값 : True
    
    print("Cheap" in str)     # str 안에 해당 문자열("Cheap")이 있니 없니? 결과값 없으면 False가 나온다.
    # 결과값 : False
    
    print("Cheap" not in str)     # str 안에 해당 문자열("Cheap")이 없다면 결과값은 True 있다면 False가 나온다.
    # 결과값 : True   
```

## 문자열 formatting (%d, %f, %s)
```
    amount = 10                             # %f : 실수
    str = "나는 사과 %d개가 있어요." %amount     # %d : 정수     # %뒤에 변수나 함수를 쓰면 인자값이 들어간다. 숫자만
    str = "나는 사과 %s개가 있어요." %"다섯"     # %s : 문자열     # %뒤에 변수나 함수를 쓰면 인자값이 들어간다. 문자만
    
    # 길어졌을 때 다음줄과 이어서 쓰려면 역슬래쉬(\)를 쓴다.
    # 인자값 2개 있을시 %뒤에 괄호를 쓰고 콤마를 쓴다. (N,M)
    str = "나는 사과 %d개, 참외%d개 있어요."\
    %(3, 5)                                         
    
    # 만약 %문자를 그대로 출력하려면 %% 2개를 써서 사용.
    str = "공장 가동률은 %d%%입니다." %95
    
    # %숫자d의 숫자는 자리의 개수
    # 숫자가 양수면 오른쪽 정렬, 음수면 왼쪽 정렬
    str = "나는 사과 %10d개가 있어요." %10   #총 10자리를 확보 후 우측 정렬
    str = "나는 사과 %-10d개가 있어요." %10   #총 10자리를 확보 후 좌측 정렬
    
    str = "원주율은 %f입니다." %3.1415926535     # 3.141593
    str = "원주율은 %.3f입니다." %3.1415926535     # 소수점 셋째자리까지 # 넷째자리에서 반올림처리 후 3.142
    print(str)         
    # 결과값 : 원주율은 3.142입니다. 
```

## 문자열 함수 type()
```
    str = "hello"
    print(type(str))
    # 결과값 : <class 'str'>  
    # str의 데이터 타입을 출력 #클래스 str의 인스턴스(객체로서)야 : type()
```

## 문자열 함수 dir()   
``` 
    str = "hello"
    print(dir(str))

    # 내장함수 중 dir()
    # 해당 인스턴스(개체)가 갖고 있는 필드와 메소드를 사용할 수 있는지 알려준다.
    # 객체가 가지고 있는 property와 #method를 리스트로 리턴
    # 결과값 : ['__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'capitalize', 'casefold', 'center', 'count', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'format_map', 'index', 'isalnum', 'isalpha', 'isdecimal', 'isdigit', 'isidentifier', 'islower', 'isnumeric', 'isprintable', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'maketrans', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']
```

## 문자열 함수 .count     .find     .index
```
    str = "cocacola"
    result = len(str)           # 인자로 들어오는 것의 개수를 count
    result = str.count("c")     # 인자로 들어온 문자열이 몇개 있는지를 count (= str 안에 "c"의 문자 개수를 count)
    result = str.find("c")      # 인자로 들어온 문자열이 맨 처음 나타나는 위치(index)를 리턴    # 0이 나온다 이유는 숫자는 0부터 시작하니
    result = str.find("o")      # 인자로 들어온 문자열이 맨 처음 나타나는 위치(index)를 리턴    # 1이 나온다 이유는 숫자는 0부터 시작하니
    result = str.find("ca22")   # 인자로 들어온 문자열이 맨 처음 나타나는 위치(index)를 리턴    # 찾지 못하면 -1 return
    #result = str.index("ca44") # find()와 동일한데 찾지 못하면 에러 발생
```

## 문자열 함수 .join
```
    a = "-"
    b = "abcdef"
    result = a.join(b)
    print(result)
    # 결과값 : a-b-c-d-e-f
    # 각각의 문자열 사이사이에 내가 원하는 특수기호를 넣고 싶을 때 사용한다.
```

## 문자열 함수 .upper     .lower    .strip
```
    str = "     Hello      World      "
    print(str.upper())     #문자열 대문자로 변환
    # 결과값 :      HELLO      WORLD   # 공백발생      
    

    
    print(str.lower())     #문자열 소문자로 변환
    # 결과값 :      hello      world   # 공백발생
    
    print(str.strip())     #문자열의 양쪽 공백 제거
    # 결과값 : Hello      World   # 양쪽 공백 제거
```

## 문자열 함수 .replace    .split
* 문자열은 기본적으로 불변의(immutable) 특성이 있다.
* 함수를 적용해도 원본 문자열은 변하지 않는다.

```
    str = "Show me the Code"
    result = str.replace("Code", "Money")
    print(result)     # 원래 문자열 Code가 바뀐게 아니라 결과값만 변한 것이다라고 생각하기.
    # 결과값 : Show me the Money

    print(str)
    # 결과값 : Show me the Code
    
    str = "Show me the Code"
    result = str.split() #list로 결과 return     # 아무값도 안 넣으면 defualt 공백을 기준으로 나눈다. 
    print(result)
    # 결과값 : ['Show', 'me', 'the', 'Code']
    
    str = "a::b::c::d::e"
    result = str.split("::") #list로 결과 return     # "::" 값을 넣으면 :: 값을 기준으로 나뉜다.
    print(result)
    # 결과값 : ['a', 'b', 'c', 'd', 'e']
```

## 문자열 함수 .format
* python에서 일반적으로 사용되는 문자열
* 아래와 같이 해도 되지만 formatting method를 쓰자.
```
    str = "나는 사과 %d개 있어요!!" %5
    print(str)
    # 결과값 : 나는 사과 5개 있어요!!
```

## 문자열 함수. formatting method .format(인자값)
```
    str = "나는 사과 {0}개 있어요!!".format(3)     #  중괄호 안에 인덱스를 넣어도 안 넣어도 됨 ex) {} or {0}
    str = "나는 사과 {}개 있어요!!".format(3)
    print(str)
    # 결과값 : 나는 사과 3개 있어요!!
        
    str = "나는 사과 {0}개, 바나나 {1}개 있어요!!".format(5, 3)    #중괄호 안에 인덱스를 넣어도 안 넣어도 됨.
    str = "나는 사과 {}개, 바나나 {}개 있어요!!".format(5, 3)
    print(str)
    # 결과값 : 나는 사과 5개, 바나나 3개 있어요!!
        
    str = "나는 사과 {num1}개, 바나나 {num2}개 있어요!!".format(num1=5, num2=3) # 변수값 num1, num2을 넣을 수도 있다.
    str = "나는 사과 {num1}개, 바나나 {num2}개 있어요!!".format(num2=3, num1=5) # 변수값 num1, num2을 넣을 수도 있다. 
    print(str)                                                           # 변수값을 지정했기 때문에 순서 상관 없다.
    # 결과값 : 나는 사과 5개, 바나나 3개 있어요!!                                    
    
    str = "원주율은 {0}입니다.".format(3.141592) 
    str = "원주율은 {0:0.3f}입니다.".format(3.141592) #앞의 0는 순번, 0.3f는 소수점 세자리까지
    print(str)
    # 결과값 : 원주율은 3.142입니다.
```

## 천단위마다 "," 출력
```
    num = 1234567890
    str = "{0}".format(num)   
    print(str)
    # 결과값 : 1234567890 모두 출력
    
    num = 1234567890    
    str = "{0:,}".format(num)
    print(str)
    # 결과값 : 1,234,567,890
```

## 백분율 처리(반올림 처리 가능)
```
    num = 0.87356
    str = "{0:.0%}".format(num)
    print(str)
    # 결과값 : 87%
    
    num = 0.87356
    str = "{0:.2%}".format(num)
    print(str)
    # 결과값 : 87.36%
```





## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>