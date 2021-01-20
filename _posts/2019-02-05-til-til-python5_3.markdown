---
layout: post
title: '[python] 파이썬 Numpy with Matplotlib'
subtitle: 
categories: til
tags: til python
comments: true
date: 2019-02-05 00:12:17 +0900
lastmod: 2019-02-05 13:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

파이썬 Numpy with Matplotlib에 대해서 <br />


# Numpy with Matplotlib
> 결과값을 수치가 아닌 그래프로 나타내는 함수 

# Matplotlib
> Matplotlib Library<br>
> plotting을 할 수 있는 python liblrary가 많이 있다. 그중 matplotlib <br>


## **Matplotlib Library**<br>

* 터미널 실행


        $ KANGs-MacBook-Pro ~ $ cd ~
          ↳ cd ~ : 최상위 폴더로 이동

* 가상환경 접속


        $ KANGs-MacBook-Pro ~ $ source activate data_env

* Matplotlib 설치


        $ KANGs-MacBook-Pro ~ $ conda install matplotlib
        Solving environment: done
        
        
        ==> WARNING: A newer version of conda exists. <==
          current version: 4.5.12
          latest version: 4.6.4
        
        Please update conda by running
        
            $ conda update -n base -c defaults conda
        
        
        
        ## Package Plan ##
        
          environment location: /anaconda3/envs/data_env
        
          added / updated specs: 
            - matplotlib
        
        
        The following packages will be downloaded:
        
            package                    |            build
            ---------------------------|-----------------
            libpng-1.6.36              |       ha441bb4_0         296 KB
            cycler-0.10.0              |   py36hfc81398_0          13 KB
            matplotlib-3.0.2           |   py36h54f8f79_0         6.5 MB
            kiwisolver-1.0.1           |   py36h0a44026_0          56 KB
            pyparsing-2.3.1            |           py36_0         105 KB
            ------------------------------------------------------------
                                                   Total:         7.0 MB
        
        The following NEW packages will be INSTALLED:
        
            cycler:     0.10.0-py36hfc81398_0
            freetype:   2.9.1-hb4e5f40_0     
            kiwisolver: 1.0.1-py36h0a44026_0 
            libpng:     1.6.36-ha441bb4_0    
            matplotlib: 3.0.2-py36h54f8f79_0 
            pyparsing:  2.3.1-py36_0         
        
        
        Proceed ([y]/n)? y
         ↳ 설치를 원할 시 y 버튼을 클릭하고 엔터!
        
        
        Downloading and Extracting Packages
        libpng-1.6.36        | 296 KB    | ##################################### | 100% 
        cycler-0.10.0        | 13 KB     | ##################################### | 100% 
        matplotlib-3.0.2     | 6.5 MB    | ##################################### | 100% 
        kiwisolver-1.0.1     | 56 KB     | ##################################### | 100% 
        pyparsing-2.3.1      | 105 KB    | ##################################### | 100% 
        Preparing transaction: done
        Verifying transaction: done
        Executing transaction: done
        
        (data_env) KANGs-MacBook-Pro:~ kang$ 

 
* 이제 jupyter notebook에서 Matplotlib import 사용 가능. 



## numpy array를 만드는 또 다른 방법 ( linspace ) (선형적인 공간)
* np.linspace(start,stop,num)
* start부터 시작해서 stop까지 num개를 균일한 간격으로 데이터를 생성해서 numpy array를 만드는 함수
###
    import numpy as np
    import matplotlib.pyplot as plt
    
    # arr = np.linspace(0,10,11)   # 결과값 : [ 0.,  1.,  2.,  3.,  4.,  5.,  6.,  7.,  8.,  9., 10.])
    # print(arr)
    
    arr = np.linspace(0,10,6)    # 결과값 : [ 0.,  2.,  4.,  6.,  8., 10.]
    print(arr)
    
    plt.plot(arr, "o")   # arr 값을 가진 "o" 표현으로 그림을 그려줌.
    plt.show() # 아래 이미지 참고
    
<img width="492" alt="Screen Shot 2019-03-24 at 8 57 04 PM" src="https://user-images.githubusercontent.com/46523571/54878997-6a1ef800-4e77-11e9-9f92-73bd69267746.png">

    
## random하게 numpy array를 생성하는 방법(정규분포, 균등분포, 표준정규분포)
    import numpy as np
    import matplotlib.pyplot as plt
###

    # (일반) 정규분포에서 난수값을 추출해서 array를 생성하는 방법
    # 추출된 난수값들이 정규분포를 띈다.(정규그래프는 종모양을 띈다.)
    mean = 10   #평균
    std = 2    #표준편차
    arr = np.random.normal(mean,std,(1000,))   # 1000개 난수
    plt.hist(arr,bins=100)   #히스토그램 - 빈도를 나타냄(구간마다 특정값이 몇개가 있나) #bins 구간
    plt.show()
    print("{0:^50}".format("<정규분포>"))
<img width="479" alt="Screen Shot 2019-03-24 at 8 59 04 PM" src="https://user-images.githubusercontent.com/46523571/54879038-eaddf400-4e77-11e9-9892-ba97140d7a38.png">


###
    # 균등분포1에서 난수값을 추출해서 array를 생성하는 방법(각 구간에서 거의 유사하게 뽑을때)
    # [0,1) 0이상 1미만의 구간에서 균등하게 정수로 난수 발생
    # [ 이상, ] 이하, ( 초과, ) 미만   수학적 표시
    # arr = np.random.rand(10000,10)   #2차원형태(인자형태)
    arr = np.random.rand(10000)   #1차원형태로 10000개 난수 추출하는 형태
    plt.hist(arr,bins=100)
    plt.show()
    print("{0:^50}".format("<균등분포1(정수)>"))
<img width="486" alt="Screen Shot 2019-03-24 at 8 59 11 PM" src="https://user-images.githubusercontent.com/46523571/54879047-fb8e6a00-4e77-11e9-9dcb-041a7ec25811.png">

    
###
    # 표준정규분포에서 난수값을 추출해서 array를 생성(평균이 0 표준편차가 1이 표준정규분포이다.)
    arr = np.random.randn(10000)   #1차원형태로 10000개 난수 추출하는 형태
    plt.hist(arr,bins=100)
    plt.show()
    print("{0:^50}".format("<표준정규분포>"))
<img width="488" alt="Screen Shot 2019-03-24 at 8 59 17 PM" src="https://user-images.githubusercontent.com/46523571/54879062-0943ef80-4e78-11e9-96c5-4edfb9a07b06.png">


###
    # 주어진 범위에서 균등분포로 정수형 난수를 추출해서 
    # numpy array로 생성하는 함수가 있다.
    arr = np.random.randint(0,100,(50,))   #0부터 99까지 50개만 뽑아
    plt.hist(arr,bins=10)
    plt.show()
    print("{0:^50}".format("<균등분포(주어진 범위)>"))
<img width="486" alt="Screen Shot 2019-03-24 at 8 59 21 PM" src="https://user-images.githubusercontent.com/46523571/54879065-1365ee00-4e78-11e9-892b-c0a683ad069c.png">


###
    # 균등분포2에서 난수값을 추출해서 array를 생성하는 방법(각 구간에서 거의 유사하게 뽑을때)
    # [0,1) 0이상 1미만의 구간에서 균등하게 실수로 난수 발생
    # [ 이상, ] 이하, ( 초과, ) 미만   수학적 표시
    # arr = np.random.rand((10000,10))   #2차원형태(쉐입형태)
    arr = np.random.random((10000,))   #1차원형태로 10000개 난수 추출하는 형태
    plt.hist(arr,bins=100)
    plt.show()
    print("{0:^50}".format("<균등분포2(실수)>"))
<img width="508" alt="Screen Shot 2019-03-24 at 8 59 26 PM" src="https://user-images.githubusercontent.com/46523571/54879068-1a8cfc00-4e78-11e9-8396-6698626c5123.png">




## References

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>