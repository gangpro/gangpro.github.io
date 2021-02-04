---
layout: post
title: '[TIL] Machine Learning 통계 목적에 따른 분류'
subtitle: 
categories: til
tags: til
comments: true
date: 2019-03-25 00:12:17 +0900
lastmod: 2019-03-25 11:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

통계 목적에 따른 분류<br />

# Statistical Hypothesis Testing
> 통계 목적에 따른 분류

<br>

## 통계의 목적에 따른 분류
* 기술통계와 추리통계
```
    # 통계의 목적에 따른 분류
    # 1. 기술통계(Descriptive statistics)
    #    - 우리에게 주어진 데이터를 요약, 설명, 분석하는 통계기법을 의미
    #    - pandas(EDA - 탐색적 데이터 분석)
    #    - 평균(대표값), 분산(데이터의 흩어져있는 정보인 분포)
    # 2. 추리통계(Inferential statistics)
    #    - 수집한 데이터를 기반으로 어떠한 사실을 예측하고 검정하는 통계기법을 의미
    #    - 통계적 가설 검증 : 표본으로부터 얻은 어떠한 사실을 근간으로 가설이 맞는지를 통계적으로 검정하는 방법
    #    - 데이터에 대한 제일 먼저 할일은 가설을 정의하는 것.
    #     -- 귀무가설(H0)(null hypothesis) : 관계가 없다. 영향력이 없다. 관련이 없다. 등
    #                                   채택되기 원하는 가설이 아니다. reject가 되는것을 기대하는 가설
    #     -- 대립가설(H1)(alternative hypothesis) : 관계가 있다. 영향력이 있다. 관련이 있다. 등
    #                                   통계에 근거해서 채택되기를 원하는 가설. 내가 주장하고 싶은것.
```

## Q. 통계적 가설 검정 문제
```
    # 통계적 가설 검정 예시
    # 우리나라 남자의 평균 수명 75, 표준편차는 10
    # 의학기술의 발달로 수명이 더 높아졌을 것이라는 의견
    # 최근 사망한 남성 30명의 수명을 조사 => 평균 79
    # 유의수준 0.1에서(90%의 신뢰수준)에서 가설을 검정
```

```
    # 내가 푼것
    H0 = "평균 수명은 변화가 없다."   # 귀무가설 : 내가 주장하고 싶은 것의 역
    H1 = "평균 수명이 더 증가했다."   # 대립가설 : 내가 주장하고 싶은 것
    
    #CV = (79 - 75) / (10 / 30 ** 0.5)
    CV = (10 / 30 ** 0.5)
    CV = CV * 1.28
    CV = CV + 75
    print(CV)
    
    if CV > 75:
        print("대립가설")
    else:
        print("귀무가설")
        
    # 결과값 : 
            77.3369495786887
            대립가설
```

```
    # 강사님이 푼것
    import numpy as np
    
    H0 = "평균 수명은 변화가 없다."   # 귀무가설 : 내가 주장하고 싶은 것의 역
    H1 = "평균 수명이 더 증가했다."   # 대립가설 : 내가 주장하고 싶은 것
    
    last_mean = 75   #지난 평균
    last_std = 10   #표준편차
    num_of_sample = 30   # 샘플 수
    mean_of_sample = 79   # 샘플의 평균 수명
    z = 1.28   #alpha = 0.1    # 유의수준 10%(alpha = 0.1 이면 z값 = 1.28)
    
    # z = (CV - mean) / (std / squr(사람수))
    CV = z * (last_std / np.sqrt(num_of_sample)) + last_mean
    print(CV)
    
    if mean_of_sample > CV:
        print(H1)
    else:
        print(H0)

    # 결과값 : 
            77.3369495786887
            평균 수명이 더 증가했다.
```

## Q. 카이제곱 검정 문제
```
    # 내가 푼것(1)
    import numpy as np
    import pandas as pd
    
    H0 = "흡연량과 음주량 사이에는 연관성이 없다."
    H1 = "흡연량과 음주량 사이에는 연관성이 있다."
    
    data = {"1갑 이상" : [23,31,13,67],
               "1갑 이하" : [21,48,23,92],
               "안피움" : [63,159,119,341],
               "계" : [107,238,155,500]}
    
    df = pd.DataFrame(data,
                     columns=["1갑 이상","1갑 이하","안피움","계"],
                     index=["반병 이상","반병 이하","못마심","계"])
    display(df)
    
    aa = df.loc["반병 이상","1갑 이상"]
    ba = df.loc["반병 이하","1갑 이상"]
    ca = df.loc["못마심","1갑 이상"]
    da = df.loc["계","1갑 이상"] 
    
    ab = df.loc["반병 이상","1갑 이하"]
    bb = df.loc["반병 이하","1갑 이하"]
    cb = df.loc["못마심","1갑 이하"]
    db = df.loc["계","1갑 이하"] 
    
    ac = df.loc["반병 이상","안피움"]
    bc = df.loc["반병 이하","안피움"]
    cc = df.loc["못마심","안피움"]
    dc = df.loc["계","안피움"] 
    
    ad = df.loc["반병 이상","계"]
    bd = df.loc["반병 이하","계"]
    cd = df.loc["못마심","계"]
    dd = df.loc["계","계"] 
    
    expected_result_aa = (da / dd * ad / dd) * dd
    observed_value_aa = aa - expected_result_aa
    _observed_value_aa = observed_value_aa ** 2
    chi_aa1 = (((observed_value_aa - expected_result_aa) ** 2) / expected_result_aa)
    _chi_aa1 = chi_aa1 ** 2
    
    print("기대값 :", expected_result_aa)
    print("관측값 :", observed_value_aa)
    print("관측값-기대값의 제곱 :", _observed_value_aa)
    print("카이스퀘어 :", _chi_aa1)
```
<img width="384" alt="Screen Shot 2019-03-26 at 12 44 31 AM" src="https://user-images.githubusercontent.com/46523571/54933614-54d1c880-4f60-11e9-8657-168439144519.png">

```
    # 내가 푼것(2)
    import numpy as np
    import pandas as pd
    
    H0 = "흡연량과 음주량 사이에는 연관성이 없다."
    H1 = "흡연량과 음주량 사이에는 연관성이 있다."
    
    arr = np.array([[23,21,63,107],
                    [31,48,159,238],
                    [13,23,119,155],
                    [67,92,341,500]])
    
    arr_d1 = 67
    arr_d2 = 92
    arr_d3 = 341
    arr_a1 = 107
    arr_a2 = 238
    arr_a3 = 155
    arr_sum = 500
    
    expected_result = (arr[3,0] / arr_sum * arr[0,3] / arr_sum) * arr_sum
    observed_value = arr[0,0] - expected_result
    observed_value_sq = observed_value ** 2
    chi = observed_value_sq / expected_result
    
    print("값 :", arr[0,0])
    print("기대값 :", expected_result)
    print("관측값-기대값 :", observed_value)
    print("제곱 :", observed_value_sq)
    print("카이스퀘어 :", chi)
    
    # 결과값 : 
            값 : 23
            기대값 : 14.338000000000001
            관측값-기대값 : 8.661999999999999
            제곱 : 75.03024399999998
            카이스퀘어 : 5.232964430185519
```

```
    # 강사님이 푼거
    import numpy as np
    from scipy import stats
    
    arr = np.array([[23,21,63],
                    [31,48,159],
                    [13,23,119],
                    [67,92,341]])
    
    chi2, pvalue, free, _table = stats.chi2_contingency(arr)
    print("카이제곱값 : {}".format(chi2))
    print("pvalue : {}".format(pvalue))
    print("자유도 : {}".format(free))
    
    if pvalue < 0.05:
        print("대립가설 accept")
    else:
        print("귀무가설 accept")
    print(arr)
    
    # 결과값 : 
            카이제곱값 : 12.826630603041854
            pvalue : 0.045873182714106564
            자유도 : 6
            대립가설 accept
            [[ 23  21  63]
             [ 31  48 159]
             [ 13  23 119]
             [ 67  92 341]]
```

## 통계란
* 기술통계와 추리통계
```
    # matplotlib을 이용한 graph 그리기
    # data간의 관계, 방향성등을 눈으로 파악하기 위해서 사용
    
    # [통계]
    # 통계는 목적에 따라서 크게 두가지 분류로 구분
    # 1. 가지고 있는 데이터를 분석해서 해당 데이터를 요약
    #    - 특정한 사실을 이끌어내는 통계 기법
    #    - 기술통계 : EDA(탐색적 데이터 분석)
    
    # 2. 수집한 데이터를 기반으로 모집단의 특성을 유추
    #    - 독렵변수에 따른 종속변수의 변화를 예측하기 위해서 사용하는 통계기법
    #    - 추리(추론)통계
    #      - 통계적 가설 검정 : 표본에 대한 내용을 근거로 모집단의 특성을 유추하는 것
    #        가설 : 귀무가설(기각이 목표), 대립가설(채택이 목표)
    
    #    또 다른 목적으로 통계적 가설 검정을 이용한다.
    #    머신러닝을 위해 여러가지 입력 parameter들이 들어간다.
    #    특정 parameter는 머신러닝을 위해 필요하며, 어떤 parameter는 그닥 필요가 없다.
    #    parameter간의 관계성을 고려해서 입력 parameter를 설정해주는 작업이 필요하다.
    #    parameter간의 관련성을 검증하기 위해 통계적 가설검증이 사용된다.
    
    #    음주량과 흡연량의 관계를 통계적으로 가설검증 기법으로 알아본다.
    #    범주형으로 되어 있고 밀도 값이 데이터로 사용되는 이런 검증에 카이제곱검증이 이용된다.
```

## 독립표본 t검정(2Sample t-test)
* stats.ttest_ind(x,y)
```
    # 독립표본 t검정 연습문제
    
    # 두 집단의 평균을 이용해서 두 집단이 서로 차이가 있는지를 판단하는 검정 방법
    # 그룹1과 그룹2의 평균키를 조사해서 그 차이가 있는지 혹은 차이를 무시할 정도인지를 판별
    
    # 귀무가설 : 두 그룹간의 평균키의 차이가 없다.
    # 대립가설 : 두 그룹간의 평균키의 차이는 의미가 있다.
    
    import numpy as np
    from scipy import stats
    
    np.random.seed(1)
    group1 = [170 + np.random.normal(2,1) for _ in range(10)]   # 사용하지 않는 변수는 _를 쓴다
    group2 = [174 + np.random.normal(0,3) for _ in range(10)]
    print("Group1의 평균 : {}".format(np.mean(group1)))
    print("Group2의 평균 : {}".format(np.mean(group2)))
    
    print(stats.ttest_ind(group1,group2))
    # 결과값 : Ttest_indResult(statistic=-1.4774069798043346, pvalue=0.15685018595907008)
    # pvalue가 0.05보다 크니 귀무가설을 선택한다.
    # pvalue가 0.05보다 작으면 대립가설을 채택한다.
    
    #_, pvalue => 값이 두개니깐 첫번째꺼는 안쓰니깐 _, 두번째꺼는 pvalue 변수로 지정
    _, pvalue = stats.ttest_ind(group1,group2)
    print(pvalue)
    if pvalue < 0.05:
        print("대립가설 선택 : 평균키의 차이는 의미가 있다.")
    else:
        print("귀무가설 선택 : 평균키의 차이는 의미가 없다.")
    
    # 결과값 : 
            Group1의 평균 : 171.9028591091939
            Group2의 평균 : 173.4912348536539
            Ttest_indResult(statistic=-1.4774069798043346, pvalue=0.15685018595907008)
            0.15685018595907008
            귀무가설 선택 : 평균키의 차이는 의미가 없다.    
```
    
## 대응표본 t검정(pared t-test)
* 어느 한 독립표본에 대해서 전과 후를 비교
* stats.ttest_rel(x,y)
```
    # 대응표본 t검정 연습문제
    
    # 다이어트약의 복용전과 복용후의 값을 통계적으로 검증하여 약의 효과를 알아보기 위해서 사용.
    # 귀무가설 : 복용전후의 체중의 차이가 없다.
    # 대립가설 : 복용전후의 체중의 차이가 있다.
    
    import numpy as np
    from scipy import stats
    
    before = [60 + np.random.normal(0,5) for _ in range(20)]
    after = [w - np.random.normal(2,1) for w in before]
    _, pvalue = stats.ttest_rel(before,after)
    print(pvalue)
    if pvalue < 0.05:
        print("대립가설이 채택 : 다이어트 약이 효과가 있다.")
    else:
        print("귀무가설이 채택 : 다이어트 약이 효과가 없다.")
    
    # 결과값 : 
            1.1068298590940042e-09
            대립가설이 채택 : 다이어트 약이 효과가 있다.
```

## 분산분석(ANOVA)
* 2개의 데이터를 비교할 때는 대응표본 t검증을 사용
* 3개의 이상일 땐 ANOVA를 사용
* stats.f_oneway(a,b,c,d.,)
```
    # ANOVA 검정
    # 비교대상이 3개 이상일 경우 t-test 대신 사용
    
    from scipy import stats
    
    #교육 훈련 데이터
    a = [67,45,98,67,34,22]
    b = [56,48,80,37,32,62]
    c = [47,47,58,37,84,12]
    d = [77,65,38,87,24,32]
    
    _, pvalue = stats.f_oneway(a,b,c,d)
    print(pvalue)
    if pvalue < 0.05:
        print("대립가설이 채택 : 교육 효과가 있다.")
    else:
        print("귀무가설이 채택 : 교육 효과가 없다.")

    # 결과값 : 
            0.9447776342385614
            귀무가설이 채택 : 교육 효과가 없다.
```

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>