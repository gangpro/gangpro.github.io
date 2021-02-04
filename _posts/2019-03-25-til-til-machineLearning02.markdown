---
layout: post
title: '[python] Machine Learning Tensorflow에 대해서'
subtitle: 
categories: til
tags: til
comments: true
date: 2019-03-25 00:12:17 +0900
lastmod: 2019-03-25 12:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

Tensorflow에 대해서<br />



# Tensorflow
> google이 만든 머신러닝을 위한 library(python, c 언어로 구성)<br>

## import tensorflow as tf
* tensorflow를 사용하기 위해서는 module을 import 사용해야한다.
* 일반적으로 as(얼리아싱)을 써서 tf라고 정해서 사용한다.(전세계적으로 tf를 쓴다.)

## tensorflow를 이용한 Hello World 출력해보자.   
``` 
    # tesorflow의 구성요소(3가지)
    # 1. node : 수학적인 연산을 담당, 데이터의 입출력 담당.
    # 2. Tensor : 다차원 array(matrix(2차원))    -> 데이터
    # 3. edge : 하나의 line, 한 node가 가지고 있는 tesor를 다른 node로 이동
    
    import tensorflow as tf
    
    my_node = tf.constant("Hello World")
    sess  = tf.Session()   # session, runner(node를 실행시키는 공식)   
    
    print(sess.run(my_node).decode())   # 내가 원하는 특정 node 실행방법
    # constant       # 상수
    # shape : (3,)   # 데이터 3개의 1차원,   (3,4) 2차원   (2,3,4) 3차원    
                     # () 차원이 없다. 즉 리스트가 아닌 scalar라는 의미.


    # 결과값 : Hello World
```

## 간단한 수학 연산 수행
```
    import tensorflow as tf
    
    node1 = tf.constant(10, dtype=tf.float32)   # 노드가 갖고 잇는 상수값을 만들 때 constant 사용
    node2 = tf.constant(20, dtype=tf.float32)
    
    #node3 = tf.add(node1,node2)   # 일반적인 표현
    node3 = node1 + node2          # 이렇게 표현해도 됨
    
    sess = tf.Session()
    print(sess.run(node3))                 # 결과값 : 30.0 
    print(sess.run([node1,node2,node3]))   # 결과값 :  [10.0, 20.0, 30.0] # 배열의 리스트 형태  
```

## placeholder(데이터를 받아들이는 그릇)
```
    import tensorflow as tf
    
    node1 = tf.placeholder(dtype=tf.float32)   # 그릇을 만들었기 때문에 나중에 값을 채워줘야 함.
    node2 = tf.placeholder(dtype=tf.float32)
    
    node3 = node1 + node2
    
    sess = tf.Session()
    print(sess.run(node3,feed_dict={node1 : 100, node2 : 200}))
    # 결과값 : #= 300.0 # feed_dict : 딕셔너리형태({key:value}로 데이터를 넣어야한다.
    
    # np.array, list 가능   => 각각의 위치 값에 대해 연산을 함
    print(sess.run(node3,feed_dict={node1 : [1,2,3], node2 : [10,11,12]}))
    # 결과값 :  [11. 13. 15.]
    
    # input()    
    print(sess.run(node3,feed_dict={node1 : input(), node2 : input()}))
    # 결과값 : 
            1
            1
            2.0
    # 즉 들어가야할 값에 list, dictionary, numpy array 등 값이 들어 갈 수 있다.
```

## tensors의 다차원 매트릭스
```
    import tensorflow as tf
    
    node1 = tf.constant(3, dtype=tf.float32)
    print(node1)    # 결과값 : Tensor("Const_19:0", shape=(), dtype=float32)
    
    node2 = tf.constant([1,2,3], dtype=tf.float32)
    print(node2)    # 결과값 : Tensor("Const_20:0", shape=(3,), dtype=float32)
```

## 간단한 linear regression을 이용한 mechine learning (1)
```
    #(1)
    # 간단한 linear regression을 이용한 mechine learning
    # 여기서 간단한 의미는 x축 하나, y축 하나라는 의미.
    
    import tensorflow as tf
    
    # training data set
    x = [1,2,3]   # 독립변수, 입력데이터
    y = [1,2,3]   # 종속변수, 입력데이터의 label
    
    # Weight & bias 정의
    W = tf.Variable(tf.random_normal([1], name="weight")) #몇개를 뽑을지 추출할지 []배열형태로 명시
    b = tf.Variable(tf.random_normal([1], name="bias"))
    
    # Hypothesis(가설) 정의 : 우리가 최종적으로 알아내야 하는 직선,  데이터에 가장 인접한 직선
    #                       예측모델이 만들어졌으니 predictiondl 가능
    H = W * x + b
    
    # Cost function(Loss function, 비용함수)
    # Cost function이 최소가 되는 W와 b값을 구하는 것이 목적
    cost = tf.reduce_mean(tf.square(H - y))
    
    # Cost function의 minimize
    optimizer = tf.train.GradientDescentOptimizer(learning_rate=0.01)
                        #경사를 빠르게 내려가는 알고리즘(GradientDescentOptimizer)
                                                  #learning_rate= 일반적으로 0.01 부터~ 커스텀
            
    train = optimizer.minimize(cost)   # cost값을 한단계만 줄임. 추후 for문 써야함.
    
    # 그래프를 실행시키기 위한 Session
    sess = tf.Session()
    
    # W,b 값에 Variable을 사용할 경우 반드시!! 초기화를 시켜줘야 한다.
    sess.run(tf.global_variables_initializer())
    
    for step in range(3000):
        _, w_val, b_val, cost_val = sess.run([train,W,b,cost])   
        # train을 실행시켜서 cost값을 낮추는 작업  # [] 안에 실행할 노드를 다 적는다.
        if step % 300 == 0:   # 300번 반복 될 때마다 출력한다는 if문
            print("w:{}, b:{}, cost:{}".format(w_val,b_val,cost_val))



    # 결과값 : 
    w:[1.0325751], b:[0.5345425], cost:8.078195571899414
    w:[2.007141], b:[0.9837667], cost:3.798652323894203e-05
    w:[2.0034695], b:[0.9921137], cost:8.964577318693046e-06
    w:[2.0016866], b:[0.99616736], cost:2.1174573703319766e-06
    w:[2.0008206], b:[0.99813545], cost:5.01112992878916e-07
    w:[2.0004005], b:[0.99909127], cost:1.1907983576975312e-07
    w:[2.000196], b:[0.99955595], cost:2.8471143664887677e-08
    w:[2.0000968], b:[0.9997812], cost:6.9046754980206515e-09
    w:[2.0000463], b:[0.999895], cost:1.5799438424224377e-09
    w:[2.0000246], b:[0.9999456], cost:4.3125206183880493e-10      
```  

## 간단한 linear regression을 이용한 mechine learning (2)
```
    #(2)
    # 간단한 linear regression을 이용한 mechine learning
    # 여기서 간단한 의미는 x축 하나, y축 하나라는 의미.
    
    import tensorflow as tf
    
    # training data set
    x_data = [1,2,3]   # 독립변수, 입력데이터
    y_data = [3,5,7]   # 종속변수, 입력데이터의 label
    
    # placeholder
    x = tf.placeholder(dtype=tf.float32)
    y = tf.placeholder(dtype=tf.float32)
    
    # Weight & bias 정의
    W = tf.Variable(tf.random_normal([1], name="weight")) #몇개를 뽑을지 추출할지 []배열형태로 명시
    b = tf.Variable(tf.random_normal([1], name="bias"))
    
    # Hypothesis(가설) 정의 : 우리가 최종적으로 알아내야 하는 직선,  데이터에 가장 인접한 직선
    #                       예측모델이 만들어졌으니 predictiondl 가능
    H = W * x + b
    
    # Cost function(Loss function, 비용함수)
    # Cost function이 최소가 되는 W와 b값을 구하는 것이 목적
    cost = tf.reduce_mean(tf.square(H - y))
    
    # Cost function의 minimize
    optimizer = tf.train.GradientDescentOptimizer(learning_rate=0.01)
                        #경사를 빠르게 내려가는 알고리즘(GradientDescentOptimizer)
                                                  #learning_rate= ?
            
    train = optimizer.minimize(cost)   # cost값을 한단계만 줄임. 추후 for문 써야함.
    
    # 그래프를 실행시키기 위한 Session
    sess = tf.Session()
    
    # W,b 값에 Variable을 사용할 경우 반드시!! 초기화를 시켜줘야 한다.
    sess.run(tf.global_variables_initializer())
    
    for step in range(3000):
        _, w_val, b_val, cost_val = sess.run([train,W,b,cost],
                                             feed_dict={x:x_data, y:y_data})   
        # train을 실행시켜서 cost값을 낮추는 작업  # [] 안에 실행할 노드를 다 적는다.
        if step % 300 == 0:   # 300번 반복 될 때마다 출력한다는 if문
            print("w:{}, b:{}, cost:{}".format(w_val,b_val,cost_val))
            
    # 학습 끝! 
    # Prediction
    sess.run(H,feed_dict={x:10})   #H 가설, feed_dict=  x값에 10이 들어가면 값이 얼마니?  



    # 결과값 : 
    w:[1.3896229], b:[-0.06446765], cost:6.904306888580322
    w:[2.142508], b:[0.676046], cost:0.015127833932638168
    w:[2.0692244], b:[0.8426369], cost:0.003569564549252391
    w:[2.0336263], b:[0.92355925], cost:0.0008422805112786591
    w:[2.0163343], b:[0.96286863], cost:0.00019874447025358677
    w:[2.0079346], b:[0.98196286], cost:4.68974030809477e-05
    w:[2.0038548], b:[0.99123764], cost:1.1067789273511153e-05
    w:[2.0018737], b:[0.9957416], cost:2.614180857563042e-06
    w:[2.0009117], b:[0.9979287], cost:6.187672170199221e-07
    w:[2.000444], b:[0.99899125], cost:1.4664580305634445e-07                                     
    
    array([21.001682], dtype=float32)
```

## 기본적인 Linear regression에 대한 예제
* training data set(준비 데이터)
* placeholder(입력파라미터)
* Weight & bias(기울기 & 절편)
* Hypothesis(가설)
* cost function
* train node(학습)
* Session, 초기화 과정
* 실제 학습을 진행하는 과정
* 직선 그리기
* Prediction(예측)
```
    %matplotlib inline
    # 기본적인 linear regression에 대한 예제
    
    import tensorflow as tf
    import matplotlib
    import matplotlib.pyplot as plt
    import numpy as np
    
    # training data set
    np.random.seed(12345)
    x_data = np.arange(0,20,1)
    y_data = np.array([ t*2 + np.random.normal(2,2) for t in x_data ])   # x값에 해당하는 label
    
    # 일단 눈으로 먼저 확인을 해보자.
    plt.scatter(x_data,y_data)
    #plt.show()
    
    # placeholder(입력 파라미터)   # 일반적으로 입력값 대문자 사용
    X = tf.placeholder(dtype=tf.float32)   # (shape과 data type) 지금은 1차원이니 dtype만 표시
    Y = tf.placeholder(dtype=tf.float32)
    
    # Weight & bias
    W = tf.Variable(tf.random_normal([1]), name="weight")
        # W의 값이 변하니 상수가 아닌 변수로 값을 정해야한다. 
        # tf.random_normal([])   -> 초기값을 난수로 줘야함.
    b = tf.Variable(tf.random_normal([1]), name="bias")
    
    # Hypothesis
    H = W * X + b
    
    # cost function
    cost = tf.reduce_mean(tf.square(H-Y))
    
    # train node(학습)
    train = tf.train.GradientDescentOptimizer(learning_rate=0.001).minimize(cost)
    
    # Session, 초기화 과정
    sess = tf.Session()
    sess.run(tf.global_variables_initializer())
    
    # 실제 학습을 진행하는 과정
    for step in range(3000):   # 에폭 : training data를 갖고 학습시키는 것   #= 3000에폭
        _, cost_val = sess.run([train,cost], feed_dict={X:x_data,Y:y_data})
        if step % 300 == 0:   # 300번 반복 될 때마다 출력한다는 if문
            print("cost:{}".format(cost_val)) 
    
    # 결과값 : 
    cost:31.155223846435547
    cost:6.124107837677002
    cost:5.5054192543029785
    cost:5.056721210479736
    cost:4.731313705444336
    cost:4.4953131675720215
    cost:4.324159145355225
    cost:4.200027942657471
    cost:4.110005855560303
    cost:4.044719219207764
        
    # -> 아무리 learning_rate=를 0.001에서 0.0001, 0.00001 해도 0에 가까워지지 않는다.
    # 이런 경우 학습이 제대로 이루어지지 않은 상태이다. 
    # 입력데이터에 대한 처리가 이루어져야 정상적으로 학습이 진행 될 수 있다. 
    # 만약 학습이 정상적으로 이루어졌으면 W,b값이 결정되게 된다.



    # 직선 그리기
    x_line = np.arange(0,20,1)
    y_line = np.array([ sess.run(W) * t + sess.run(b) for t in x_line ])
    plt.plot(x_line,y_line,"r")
    plt.show()
```
<img width="468" alt="Screen Shot 2019-03-26 at 1 22 53 AM" src="https://user-images.githubusercontent.com/46523571/54936382-b34d7580-4f65-11e9-9846-6094fcb9c2bb.png">

```
    # Prediction(예측)
    sess.run(H,feed_dict={X:15})
    # 결과값 : array([32.628284], dtype=float32)
```

## ozone데이터를 이용한 linear regression 예제
```
    %matplotlib inline
    
    import numpy as np
    import pandas as pd
    import matplotlib
    import matplotlib.pyplot as plt
    from scipy import stats
    
    # Data Loading
    data = pd.read_csv("./data/ozone/ozone.csv", sep=",")
    df = data.dropna(how="any", inplace=False)
    
    # 독립변수와 종속변수를 추출
    x = df["Temp"]   # 입력 파라미터
    y = df["Ozone"]   # label
    
    result = stats.linregress(x,y)
    w = result[0]
    b = result[1]
    
    # LinregressResult(slope=2.439109905529362, intercept=-147.64607238059494, rvalue=0.6985414096486389, pvalue=1.552677229392932e-17, stderr=0.23931937849409174)
    # slope=2.439109905529362   # 기울기
    # intercept=-147.64607238059494   # 절편
    # rvaluse= 0.6985414096486389   # 상관계수   # 0.7 이상은 강환 관계라 본다.
    # pvalue=1.552677229392932e-17
    # stderr=0.23931937849409174
    
    plt.scatter(x,y)   # 흙뿌리기
    plt.plot(x,w*x+b,"r")   # 선
    plt.show()
```

<img width="486" alt="Screen Shot 2019-03-29 at 11 33 02 AM" src="https://user-images.githubusercontent.com/46523571/55205505-7d82e800-5216-11e9-8759-0e1b33f832d4.png">

## 다중선형회귀분석 Multivariable(multiple) linear regression - 성적예측문제
```
    # 다중선형회귀분석 Multivariable(multiple) linear regression
    # 입력파라미터가 많다는 뜻. x1,x2,x3=입력파라미터, y= label
    
    import tensorflow as tf
    
    # training data set
    x_data = [[73,80,75],
             [93,88,93],
             [89,91,90],
             [96,98,100],
             [73,66,70]]
    y_data = [[152],     # 주의할점은 y label의 값은 하나지만.
             [185],      # x값이 매트릭스 형태이니 y도 매트릭스 형태로 표시해야한다
             [180],
             [196],
             [142]]
    
    
    # placeholder
    # X = tf.placeholder(shape=[5,3], dtype=tf.float32)   # [행5,열3] 이렇게 해야하지만
    # [None,3] 여기서 None는 없다라는 의미가 아닌 don't care 라는 의미. 행의 개수는 고려하지 않고 열만 3으로 표시
    X = tf.placeholder(shape=[None,3], dtype=tf.float32)
    Y = tf.placeholder(shape=[None,1], dtype=tf.float32)
    
    # weight & bias
    W = tf.Variable(tf.random_normal([3,1]), name="weight")
    b = tf.Variable(tf.random_normal([1]), name="bias")
    
    # Hypothesis
    # H = W * x + b (기존)
    H = tf.matmul(X,W) + b   # matmul 행렬곱
    
    
    ## 그 다음부터는 1차원과 동일과정으로 진행
                
    # cost function
    cost = tf.reduce_mean(tf.square(H-Y))
    
    # train node(학습)
    train = tf.train.GradientDescentOptimizer(learning_rate=0.00001).minimize(cost)
    #learning_rate = W와 b와의 관계 0.1 크게크게
    #learning_rate = W와 b와의 관계 0.0001 좁게좁게
    
    # Session, 초기화 과정
    sess = tf.Session()
    sess.run(tf.global_variables_initializer())
    
    # 실제 학습을 진행하는 과정
    for step in range(30000):   # 에폭 : training data를 갖고 학습시키는 것   #= 3000에폭
        _, cost_val = sess.run([train,cost], feed_dict={X:x_data,Y:y_data})
        if step % 1000 == 0:   # 300번 반복 될 때마다 출력한다는 if문
            print("cost:{}".format(cost_val)) 
    
    # Prediction(예측)
    sess.run(H,feed_dict={X:[[60, 70, 50]]})
    
    # (참고) 결과값에 NaN이 뜨는 이유는 최저값에서 계속 왔다갔다 한다는 의미이다.
```

<img width="387" alt="Screen Shot 2019-03-29 at 11 34 08 AM" src="https://user-images.githubusercontent.com/46523571/55205528-9f7c6a80-5216-11e9-8e0c-a4cd4ddae151.png">

## 다중선형회귀분석 Multivariable(multiple) linear regression - 오존 예측 문제
```
    # 다중선형회귀분석 Multivariable(multiple) linear regression
    # 입력파라미터가 많다는 뜻. x1,x2,x3=입력파라미터, y= label
    
    import numpy as np
    import pandas as pd
    import matplotlib
    import matplotlib.pyplot as plt
    from scipy import stats
    import tensorflow as tf
    
    # # training data set
    # x_data = [[73,80,75],
    #          [93,88,93],
    #          [89,91,90],
    #          [96,98,100],
    #          [73,66,70]]
    # y_data = [[152],     # 주의할점은 y label의 값은 하나지만.
    #          [185],      #  x값이 매트릭스 형태이니 y도 매트릭스 형태로 표시해야한다
    #          [180],
    #          [196],
    #          [142]]
    
    # Data Loading
    data = pd.read_csv("./data/ozone/ozone.csv", sep=",")
    df = data.dropna(how="any", inplace=False)
    
    # Normalization : (현재값 - min값) / (max값 - min값)
    df["Ozone_Norm"] = (df["Ozone"] - df["Ozone"].min()) / (df["Ozone"].max() - df["Ozone"].min())  
    df["Solar.R_Norm"] = (df["Solar.R"] - df["Solar.R"].min()) / (df["Solar.R"].max() - df["Solar.R"].min())
    df["Wind_Norm"] = (df["Wind"] - df["Wind"].min()) / (df["Wind"].max() - df["Wind"].min())
    df["Temp_Norm"] = (df["Temp"] - df["Temp"].min()) / (df["Temp"].max() - df["Temp"].min())
    display(df)
    
    # 필요 자료만 slicing
    x_data = df[["Solar.R_Norm","Wind_Norm","Temp_Norm"]]
    y_data = df[["Ozone_Norm"]]
    
    # placeholder
    # X = tf.placeholder(shape=[5,3], dtype=tf.float32)   # [행5,열3] 이렇게 해야하지만
    # [None,3] 여기서 None는 없다라는 의미가 아닌 don't care 라는 의미. 행의 개수는 고려하지 않고 열만 3으로 표시
    X = tf.placeholder(shape=[None,3], dtype=tf.float32)
    Y = tf.placeholder(shape=[None,1], dtype=tf.float32)
    
    # weight & bias
    W = tf.Variable(tf.random_normal([3,1]), name="weight")
    b = tf.Variable(tf.random_normal([1]), name="bias")
    
    # Hypothesis
    # H = W * x + b (기존)
    H = tf.matmul(X,W) + b   # matmul 행렬곱
    
    
    ## 그 다음부터는 1차원과 동일과정으로 진행
                
    # cost function
    cost = tf.reduce_mean(tf.square(H-Y))
    
    # train node(학습)
    train = tf.train.GradientDescentOptimizer(learning_rate=0.001).minimize(cost)
    
    # Session, 초기화 과정
    sess = tf.Session()
    sess.run(tf.global_variables_initializer())
    
    # 실제 학습을 진행하는 과정
    for step in range(30000):   # 에폭 : training data를 갖고 학습시키는 것   #= 3000에폭
        _, cost_val = sess.run([train,cost], feed_dict={X:x_data,Y:y_data})
        if step % 1000 == 0:   # 300번 반복 될 때마다 출력한다는 if문
            print("cost:{}".format(cost_val)) 
    
    # Prediction(예측)
    sess.run(H,feed_dict={X:[[300, 13.4, 97]]})
```

## 다중선형회귀분석 Multivariable(multiple) linear regression - 오존 예측 문제 sklearn 사용
```
    import pandas as pd
    import tensorflow as tf
    import matplotlib
    import matplotlib.pyplot as plt
    from sklearn.preprocessing import MinMaxScaler
    
    # 1. Data Loading
    data = pd.read_csv("./data/ozone/ozone.csv", sep=",")
    df = data.dropna(how="any", inplace=False)
    display(df.head())
    
    # 2. training data set
    x_data = MinMaxScaler().fit_transform(df[["Solar.R","Wind","Temp"]].values)
    y_data = MinMaxScaler().fit_transform(df["Ozone"].values.reshape(-1,1))
    
    # placeholder
    # X = tf.placeholder(shape=[5,3], dtype=tf.float32)   # [행5,열3] 이렇게 해야하지만
    # [None,3] 여기서 None는 없다라는 의미가 아닌 don't care 라는 의미. 행의 개수는 고려하지 않고 열만 3으로 표시
    X = tf.placeholder(shape=[None,3], dtype=tf.float32)
    Y = tf.placeholder(shape=[None,1], dtype=tf.float32)
    
    # weight & bias
    W = tf.Variable(tf.random_normal([3,1]), name="weight")
    b = tf.Variable(tf.random_normal([1]), name="bias")
    
    # Hypothesis
    # H = W * x + b (기존)
    H = tf.matmul(X,W) + b   # matmul 행렬곱
    
    
    ## 그 다음부터는 1차원과 동일과정으로 진행
                
    # cost function
    cost = tf.reduce_mean(tf.square(H-Y))
    
    # train node(학습)
    train = tf.train.GradientDescentOptimizer(learning_rate=0.001).minimize(cost)
    
    # Session, 초기화 과정
    sess = tf.Session()
    sess.run(tf.global_variables_initializer())
    
    # 실제 학습을 진행하는 과정
    for step in range(30000):   # 에폭 : training data를 갖고 학습시키는 것   #= 3000에폭
        _, cost_val = sess.run([train,cost], feed_dict={X:x_data,Y:y_data})
        if step % 1000 == 0:   # 300번 반복 될 때마다 출력한다는 if문
            print("cost:{}".format(cost_val)) 
    
    # Prediction(예측)
    sess.run(H,feed_dict={X:[[300, 13.4, 97]]})
```

## Simple Linear Regression
* Simple Linear Regression (입력파라미터가 1개라는 걸 simple Linear Regression)
* Multiple Linear Regression (입력파라미터가 여러개라는 걸 multiple Linear Regression)
```
    import tensorflow as tf
    import matplotlib
    import matplotlib.pyplot as plt
    
    # 1. Data Loading
    # 2. training data set
    x_data = [1,2,5,8,10]
    y_data = [0,0,0,1,1]
    
    # placeholder
    X = tf.placeholder(dtype=tf.float32)
    Y = tf.placeholder(dtype=tf.float32)
    
    # weight & bias
    W = tf.Variable(tf.random_normal([1]), name="weight")
    b = tf.Variable(tf.random_normal([1]), name="bias")
    
    # Hypothesis
    H = W * X + b
    
    # cost function   # 최소제곱법   # cost 함수의 값이 최소가 되는 W,b를 찾는게 목적
    cost = tf.reduce_mean(tf.square(H-Y))
    
    # train node(학습)   # learning_rate= 일반적으로 0.01부터 시작 #커스터마이즈해야함.
    train = tf.train.GradientDescentOptimizer(learning_rate=0.01).minimize(cost)
    
    # Session & 초기화 과정
    sess = tf.Session()
    sess.run(tf.global_variables_initializer())
    
    # 실제 학습을 진행하는 과정
    for step in range(3000):
        _, cost_val = sess.run([train,cost],feed_dict={X:x_data, Y:y_data})
        if step % 300 ==0:
            print("Cost : {}".format(cost_val))
    
    # 그래프 그리기
    plt.scatter(x_data,y_data)   # 흙뿌리기
    plt.plot(x_data,sess.run(W) * x_data + sess.run(b),"r")   #선그래프
    plt.show()  
    
    # y축 0.5에 해당하는 x축 값은? 
    # 0.5 = sess.run(W) * x + sess.run(b)
    x = (0.5 - sess.run(b)) / sess.run(W)
    print(x)       #=array([5.973688], dtype=float32) 
    
    #합격하려면 5.9시간 정도는 공부를 해야 합격할 수 있다는 예측이 가능해 진다.
    
    # Prediction(예측)
```

<img width="516" alt="Screen Shot 2019-03-29 at 11 38 49 AM" src="https://user-images.githubusercontent.com/46523571/55205701-3f39f880-5217-11e9-9b7b-1fd2efeaef97.png">

## 로지스틱 회귀(Logistic Regression)
```
    %matplotlib inline
    import numpy as np
    import matplotlib
    import matplotlib.pyplot as plt
    import mglearn   # Sample Data를 가져오기 위한 utility module
    from sklearn.linear_model import LogisticRegression  #sklearn 파이썬이 제공하는 수학적 통계 모듈
    import warnings   # warning 제어를 목적으로 사용
    warnings.filterwarnings(action="ignore")   # warning을 표시 없애기
    
    # x : x parameter 2개
    # y : label ( 0 or 1의 로지스틱 데이터)
    x,y = mglearn.datasets.make_forge()   #make_forge()는 의미없는 더미 데이터 만들기
    
    #x값 좌표에 y값이 0이면 동그라미
    #x값 좌표에 y값이 1이면 세모
    mglearn.discrete_scatter(x[:,0], x[:,1], y)
    
    # 학습
    model = LogisticRegression()
    model.fit(x,y)
    clf = model.fit(x,y)
    
    # clf 모델에 대해서 그림
    mglearn.plots.plot_2d_separator(clf,x,fill=False, eps=0.5)
```
    
<img width="448" alt="Screen Shot 2019-03-29 at 11 39 20 AM" src="https://user-images.githubusercontent.com/46523571/55205715-4f51d800-5217-11e9-8b91-eebb1f151a90.png">

## Tensorflow를 이용한 logistiic regression
```
    # Tensorflow를 이용한 logistiic regression
    
    import tensorflow as tf
    
    # Data Loading
    # 파일이나 network를 통해서 데이터를 로딩한 후 전처리 과정
    
    # Training Data Set   # 2차원 매트릭스 형태
    x_data = [[10,0],
             [8,1],
             [3,3],
             [2,3],
             [5,1],
             [2,0],
             [1,0]]
    y_data = [[1],[1],[1],[1],[0],[0],[0]]
    
    # 행의 데이터는 상관 없지만 열의 데이터는 2개야 라는 의미
    # placeholder
    X = tf.placeholder(shape=[None,2], dtype=tf.float32)
    Y = tf.placeholder(shape=[None,1], dtype=tf.float32)
    
    # Weight & bias
    # 행렬곱에 따라 위의 X의 열 값이 W에 행이 온다.   # Y의 열 값이 W에 열이 온다
    W = tf.Variable(tf.random_normal([2,1]), name="weight")
    b = tf.Variable(tf.random_normal([1]), name="bias")
    
    # Hypothesis(가설정의)
    logit = tf.matmul(X,W) + b
    H = tf.sigmoid(logit)
    
    # Cost Function
    # 수학식 : cost = -tf.reduce_mean(y * tf.log(H) + (1-Y) * tf.log(1-H))
    # nn = nuaral network    
    # logits=logit이라고 불리는 tf.matmul(X,W) + b 값이 들어가야함
    # labels=Y   <-Y측 label에 해당하는 값이 들어가야함
    cost = tf.reduce_mean(tf.nn.sigmoid_cross_entropy_with_logits(logits=logit, labels=Y))
    
    # train node
    train = tf.train.GradientDescentOptimizer(learning_rate=0.01).minimize(cost)
    
    # session & 초기화 작업
    sess = tf.Session()
    sess.run(tf.global_variables_initializer())
    
    # 학습
    for step in range(3000):
        _, cost_val = sess.run([train,cost], feed_dict={X:x_data,Y:y_data})
        if step % 300 == 0:
            print("cost : {}".format(cost_val))
    
    # 우리가 만든 모델이 얼마나 정확한지를 측정
    # accuracy(정확도)
    # cast 정수를 실수로, 실수를 정수로 바꾸는 함수
    # H 값에 대해 0.5보다 크면 True, 작으면 False 즉 실수로 바꾸라고 했으니 True ->1, False ->0 바꿔줌
    predict = tf.cast(H > 0.5, dtype=tf.float32)
    correct = tf.equal(predict, Y)   #예측(predict)값과 실제 Y(placehold)값 비교
    accuracy = tf.reduce_mean(tf.cast(correct,dtype=tf.float32))
    
    # 학습데이터를 갖고 정확도 측정하기
    print("정확도 : {}".format(sess.run(accuracy,feed_dict={X:x_data,Y:y_data})))
    
    # prediction(예측)
    print("예측값 : {}".format(sess.run(predict,feed_dict={X:[[7,1]]})))  # 7시간에 1년  합격
    print("예측값 : {}".format(sess.run(predict,feed_dict={X:[[6,1]]})))  # 6시간에 1년  합격
    print("예측값 : {}".format(sess.run(predict,feed_dict={X:[[5,1]]})))  # 5시간에 1년  합격
    print("예측값 : {}".format(sess.run(predict,feed_dict={X:[[4,1]]})))  # 4시간에 1년  합격
    print("예측값 : {}".format(sess.run(predict,feed_dict={X:[[3,1]]})))  # 3시간에 1년  탈락
```
    
<img width="292" alt="Screen Shot 2019-03-29 at 11 39 55 AM" src="https://user-images.githubusercontent.com/46523571/55205740-6395d500-5217-11e9-82c8-618dadbbdb9c.png">

## 실습예제 1 : 타이타닉
```
    # Titanic 분석(Tensorflow)
    import tensorflow as tf
    import numpy as np
    import pandas as pd
    from sklearn.preprocessing import MinMaxScaler
    
    # 1. Data Loading
    data = pd.read_csv("./data/titanic/titanic_data.csv", sep=",")
    data_x = data[["Sex","Age","Pclass","Fare"]]
    data_y = data["Survived"]
    
    
    # 2. 데이터 전처리 과정(더미 처리, 정규화(normalization))을 거쳐야 한다.
    # 더미 처리
    Pclass_dummies = pd.get_dummies(data_x["Pclass"], prefix="Pclass")
    # .get_dummies = 특정 열에서 더미를 만들때 사용하는 함수
    # prefix= 열의 값을 세분화된 열로 나누어준다.
    
    # data_x에 Pclass_dummiess를 붙인다.
    data_x = data_x.join(Pclass_dummies)
    
    # Pclass를 세분화된 열로 바꿨으니 기존 Pclass를 삭제한다.
    # axis=1 열에 있는 "Pclass"를 지우라는 말 # inplace=True 원본을 지우라는 말
    data_x.drop("Pclass", axis=1, inplace=True)
    
    # 성별도 더미 세분화 작업을 진행한다.
    Sex_dummies = pd.get_dummies(data_x["Sex"], prefix="Sex")
    data_x = data_x.join(Sex_dummies)
    data_x.drop("Sex", axis=1, inplace=True)
    
    # data_y는 0과 1 죽었냐 살았냐라고 나뉘어 있으니 따로 더미 처리를 안한다.
    
    
    # 3. training data set
    # 데이터 프레임 안에 있는 것을 2차원 numpy array로 가져온다.
    # MinMaxScaler() = 최소값 최대값 구하는 등등
    # fit_transform = 정규화 작업
    x_data = MinMaxScaler().fit_transform(data_x.values) 
    # data_y.values 1차원 배열 상태이니
    # reshape(-1,1) 2차원 매트릭스 상태로 변경
    y_data = data_y.values.reshape(-1,1)
    
    # 4. placeholder
    # display(x_data.shape)   #=(891, 7)  #x_data의 형태를 확인 후 아래와 같이 placeholder에 넣는다.
    X = tf.placeholder(shape=[None,7], dtype=tf.float32)
    Y = tf.placeholder(shape=[None,1], dtype=tf.float32)
    
    # 5. Weight & bias
    W = tf.Variable(tf.random_normal([7,1]), name="weight")
    b = tf.Variable(tf.random_normal([1]), name="bias")
    
    # 6. Hypothesis
    logit = tf.matmul(X,W) + b
    H = tf.sigmoid(logit)
    
    # 7. cost function
    cost = tf.reduce_mean(tf.nn.sigmoid_cross_entropy_with_logits(logits=logit, labels=Y))
    
    # 8. train
    train = tf.train.GradientDescentOptimizer(learning_rate=0.01).minimize(cost)
    
    # 9. session & initializer
    sess = tf.Session()
    sess.run(tf.global_variables_initializer())
    
    # 10. training
    for step in range(10000):
        _, cost_val = sess.run([train,cost], feed_dict={X:x_data, Y:y_data})
        if step %1000 == 0:
            print("Cost : {}".format(cost_val))
    
    # 11. accuracy(정확도) 측정
    predict = tf.cast(H > 0.5, dtype=tf.float32)
    correct = tf.equal(predict, Y)   #입력값과 실제값 비교
    # True&False -> 0&1로 바꾼 후 평균
    accuracy = tf.reduce_mean(tf.cast(correct, dtype=tf.float32))
    
    print("-" * 50)
    print("정확도(%) : {}".format(100 * sess.run(accuracy, feed_dict={X:x_data, Y:y_data})))
    print("※ 참고 : 학습한 데이터를 갖고 예측을 한거라 정확도가 엄청 높게 나온다.")
```

<img width="610" alt="Screen Shot 2019-03-29 at 11 41 19 AM" src="https://user-images.githubusercontent.com/46523571/55205783-95a73700-5217-11e9-97ba-1a40d7bae937.png">

## 실습예제 2 : 대학원 입학
* admit(0 불합격, 1 합격)
* gre 입학 시험 점수
* gpa 학부 성적
* rank 등급(1 > 2 > 3)
* 주어진 데이터의 70%를 학습용으로 사용하고 나머지 30%를 검증용으로 사용.
```
    # admission 분석(Tensorflow)
    import tensorflow as tf
    import numpy as np
    import pandas as pd
    from sklearn.preprocessing import MinMaxScaler
    
    # 1. Data Loading
    data = pd.read_csv("./data/admission/admission.csv", sep=",")
    # data70p = data[:280]
    # data30p = data.iloc[280:400]
    data70p = data[0:int(data.shape[0]*0.7)]
    data30p = data[int(data.shape[0]*0.7):]
    data_x_train = data70p[["gre","gpa","rank"]]
    data_y_train = data70p["admit"]
    data_x_test = data30p[["gre","gpa","rank"]]
    data_y_test = data30p["admit"]
    
    
    # 2. 데이터 전처리 과정
    rank_dummies = pd.get_dummies(data_x_train["rank"], prefix="rank")
    data_x_train = data_x_train.join(rank_dummies)
    data_x_train.drop("rank", axis=1, inplace=True)
    
    rank_dummies = pd.get_dummies(data_x_test["rank"], prefix="rank")
    data_x_test = data_x_test.join(rank_dummies)
    data_x_test.drop("rank", axis=1, inplace=True)
    
    # 3. training data set
    x_data_train = MinMaxScaler().fit_transform(data_x_train.values) 
    x_data_test = MinMaxScaler().fit_transform(data_x_test.values) 
    y_data_train = data_y_train.values.reshape(-1,1)
    y_data_test = data_y_test.values.reshape(-1,1)
    
    
    # 4. placeholder
    #display(x_data.shape)   #=(280, 6)
    X = tf.placeholder(shape=[None,6], dtype=tf.float32)
    Y = tf.placeholder(shape=[None,1], dtype=tf.float32)
    X
    # 5. Weight & bias
    W = tf.Variable(tf.random_normal([6,1]), name="weight")
    b = tf.Variable(tf.random_normal([1]), name="bias")
    
    # 6. Hypothesis
    logit = tf.matmul(X,W) + b
    H = tf.sigmoid(logit)
    
    # 7. cost function
    cost = tf.reduce_mean(tf.nn.sigmoid_cross_entropy_with_logits(logits=logit, labels=Y))
    
    # 8. train
    train = tf.train.GradientDescentOptimizer(learning_rate=0.01).minimize(cost)
    
    # 9. session & initializer
    sess = tf.Session()
    sess.run(tf.global_variables_initializer())
    
    # 10. training
    for step in range(1000):
        _, cost_val = sess.run([train,cost], feed_dict={X:x_data_train, Y:y_data_train})
        if step %100 == 0:
            print("Cost : {}".format(cost_val))
    
    # 11. accuracy(정확도) 측정
    predict = tf.cast(H > 0.5, dtype=tf.float32)
    correct = tf.equal(predict, Y)
    accuracy = tf.reduce_mean(tf.cast(correct, dtype=tf.float32))
    
    print("-" * 50)
    print("나머지 데이터로 테스트한 정확도(%) : {}"
          .format(100 * sess.run(accuracy, feed_dict={X:x_data_test, Y:y_data_test})))
    
    
    # result = sess.run(predict, feed_dict={X:scaler.fit_transform([[500,3.7,1,0,0,0]])})
    
    
    # if result == 0:
    #     print("불합격입니다.")
    # else:
    #     print("축하합니다. 합격입니다.")
        
        
    result = sess.run(H, feed_dict={X:scaler.fit_transform([[500,3.7,1,0,0,0]])})
    if result > 0.5:
        print("축하합니다. 합격입니다. {}".format(result))
    else:
        print("불합격입니다. {}".format(result))
```

<img width="559" alt="Screen Shot 2019-03-29 at 11 42 10 AM" src="https://user-images.githubusercontent.com/46523571/55205810-b3749c00-5217-11e9-96e3-6b379d8be5ac.png">

## 중간정리
```
    # < Machine Learning >
    # - Supervised Learning (지도학습)
    #   - training data set에 label이 있다.(Y의 결과값이 있는 것). 입력값에 의해서 결과값이 나온다.
    #     1. Simple Linear Regression
    #      : X쪽 독립변수(입력 파라미터) 한개, Y쪽 레이블 한개)
    #      : Hypothesis (H = W * X + b), cost Function
    
    #     2. Multiple Linear Regression(2차원 매트릭스 형태)
    #      : X쪽 독립변수(입력 파라미터) 두개 이상, Y쪽 레이블 한개)
    #      : Hypothesis (H = XW + b)(XW 행렬곱 처리)
    
    #     3. Logistic Regression(매트릭스 형태로 가정)
    #      : data에 대한 label이 0 혹은 1, True 혹은 False의 형태
    #      : 실제로 현실에서 많이 이용되고 있는 학습모델
    #      : 정확도(accuracy) 측정 가능.
    #      : 다른말로 binary classification이라고도 한다.
    #      : Hypothesis =>  sigmoid(XW + b)   #결과값을 바인딩하기 위해 sigmoid 사용
    
    # - UnSupervised Learning (비지도학습)
    #   - training data set에 label이 없다.
```


## Multinomial Classification 실습 p.87
```
    import tensorflow as tf
    
    # 1. data loading
    # 2. training data set
    x_data = [[10,7,8,5],
              [8,8,9,4],
              [7,8,2,3],
              [6,3,9,3],
              [7,5,7,4],
              [3,5,6,2],
              [2,4,3,1]
            ]
    y_data = [[1,0,0],    # 범주가 3개이니 A를 [1,0,0], B를 [0,1,0], C를[0,0,1]로 간주한다.
              [1,0,0],
              [0,1,0],
              [0,1,0],
              [0,1,0],
              [0,0,1],
              [0,0,1]
             ]
    # placeholder
    X = tf.placeholder(shape=[None,4], dtype=tf.float32)
    Y = tf.placeholder(shape=[None,3], dtype=tf.float32)
    
    # Weight & bias
    W = tf.Variable(tf.random_normal([4,3]), name="weight")
    b = tf.Variable(tf.random_normal([3]), name="bias")
    
    # Hypothesis
    logit = tf.matmul(X,W) + b
    H = tf.nn.softmax(logit)
    
    # cost function
    cost = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits_v2(logits=logit, labels=Y))
    
    # train
    train = tf.train.GradientDescentOptimizer(learning_rate=0.01).minimize(cost)
    
    # session & initializer
    sess = tf.Session()
    sess.run(tf.global_variables_initializer())
    
    # training
    for step in range(3000):
        _, cost_val = sess.run([train,cost], feed_dict={X:x_data,Y:y_data})
        if step % 300 == 0:
            print("cost : {}".format(cost_val))
    
    # accuracy
    # H => [[0.3, 0.6, 0.1] ABC 위치에 각 숫자인 확률값(%)이 나온다.
    # 이 중에 어떤 값이 가장 크냐?라는 걸 알고 싶다. 
    # argmax() 가장 큰값이 몇번째에 있냐?를 알려주는 함수 # 1은 axis=1을 의미, 즉 열방향
    predict = tf.argmax(H,1)   # 학습에 의한 예측값
    correct = tf.equal(predict, tf.argmax(Y,1))   #이게 맞으면 예측이 잘된것, 틀리면 예측이 잘못된것
    accuracy = tf.reduce_mean(tf.cast(correct, dtype=tf.float32))
    
    print("정확도 : {}".format(sess.run(accuracy, feed_dict={X:x_data, Y:y_data})))
    
    # 예측
    # [[0.3 0.6 0.1]] -> [[0]] -> A
    result = sess.run(predict, feed_dict={X:[[5,3,9,4]]})
    if result[0] == 0:
        print("A")
    elif result[0] == 1:
        print("B")
    else:
        print("C")
```

<img width="298" alt="Screen Shot 2019-03-29 at 11 44 50 AM" src="https://user-images.githubusercontent.com/46523571/55205914-12d2ac00-5218-11e9-84a6-39058584dc56.png">

## 실습예제 : BMI p.102
```
    import tensorflow as tf
    import numpy as np
    import pandas as pd
    from sklearn.preprocessing import MinMaxScaler
    import warnings   # warning 제어를 목적으로 사용
    warnings.filterwarnings(action="ignore")   # warning을 표시 없애기
    
    # 1. data loading
    data = pd.read_csv("./data/bmi/bmi.csv", sep=",", skiprows=3)
    
    # 있을지 모르는 결치값을 없애기 위해 dropna를 쓴다.
    data = data.dropna(how="any")
    
    # training data, testing data
    num_of_data = int(data.shape[0]*0.7)
    
    # 학습용(70%), 검증용(30%) 나누기
    data_train = data.loc[:num_of_data,:]
    data_test = data.loc[num_of_data:,:]
    
    df_train_x = data_train[["height","weight"]]   # 데이터파일에서 해당 열만 추출
    df_train_y = data_train["label"]     # 데이터파일에서 해당 열만 추출
    df_test_x = data_test[["height","weight"]]   # 데이터파일에서 해당 열만 추출
    df_test_y = data_test["label"]     # 데이터파일에서 해당 열만 추출
    # print(type(df_train_x))   # <class 'pandas.core.frame.DataFrame'>
    # print(df_train_x.shape)   # (14001, 2)
    # print(type(df_train_y))   # <class 'pandas.core.series.Series'>
    # print(df_train_y.shape)   # (14001,)
    
    sess = tf.Session()
    # 2-1. training data set
    scaler = MinMaxScaler()
    # train_x_data = MinMaxScaler().fit_transform(df_train_x.values)
    train_x_data = scaler.fit_transform(df_train_x.values)   # fit_transform() 괄호안에 있는걸 가지고 처리  
    train_y_data = tf.one_hot(df_train_y,3).eval(session=sess)
    #train_y_data = sess.run(tf.one_hot(df_train_y,3))
    
    # 2-2. testing data set
    test_x_data = scaler.transform(df_test_x.values)   # transform() 괄호가 아닌 자기가 가지고 있는걸로 처리
    test_y_data = tf.one_hot(df_test_y,3).eval(session=sess)
    
    # 3. placeholder
    X = tf.placeholder(shape=[None,2], dtype=tf.float32)
    Y = tf.placeholder(shape=[None,3], dtype=tf.float32)
    
    # 4. Weight & bias
    W = tf.Variable(tf.random_normal([2,3]), name="weight")
    b = tf.Variable(tf.random_normal([3]), name="bias")
    
    # 5. Hypothesis
    logit = tf.matmul(X,W) + b
    H = tf.nn.softmax(logit)
    
    # 6. cost function
    cost = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits_v2(logits=logit, labels=Y))
    
    # 7. train
    train = tf.train.GradientDescentOptimizer(learning_rate=0.1).minimize(cost)
    
    # 8. initializer
    sess.run(tf.global_variables_initializer())
    
    # training
    for step in range(3000):
        _, cost_val = sess.run([train,cost], feed_dict={X:train_x_data,Y:train_y_data})
        if step % 300 == 0:
            print("cost : {}".format(cost_val))
    
    # accuracy
    predict = tf.argmax(H,1)   # 학습에 의한 예측값
    correct = tf.equal(predict, tf.argmax(Y,1))   #이게 맞으면 예측이 잘된것, 틀리면 예측이 잘못된것
    accuracy = tf.reduce_mean(tf.cast(correct, dtype=tf.float32))
    
    print("정확도(%) : {}".format(100 * sess.run(accuracy, feed_dict={X:test_x_data, Y:test_y_data})))
    print("예측도 : {}".format(sess.run(H, feed_dict={X:scaler.transform([[188,75]])})))
```

<img width="498" alt="Screen Shot 2019-03-29 at 11 45 41 AM" src="https://user-images.githubusercontent.com/46523571/55205954-33026b00-5218-11e9-8029-ef1f254105ff.png">

## MINIST - Multinomail Classfication
```
    # MINIST - Multinomail Classfication
    # 입력데이터 - 이미지에 대한 pixel data가 들어온다.
    # 원래 이미지 데이터는 3차원 데이터인데 흑백이고, 
    # 2차원 데이터를 1차원으로 변환해서 입력을 받는다.
    # 약 5.5천개의 이미지를 입력으로 받는다.
    # 입력데이터(x parameter)의 shape -> (55000,784)
    # y측 label의 shape -> (55000,10)
    
    from tensorflow.examples.tutorials.mnist import input_data
    import tensorflow as tf
    import matplotlib
    import matplotlib.pyplot as plt
    import numpy as np
    
    # 1. data loading
    mnist = input_data.read_data_sets("./data/mnist/", one_hot=True)
    # one_hot=True 처리가 알아서 됨
    
    
    # 위치에 있는 값 보기
    # mnist.train.labels[0]   #[0]의 데이터는 아래와 같다.
    # #=array([0., 0., 0., 0., 0., 0., 0., 1., 0., 0.])  one_hot 처리
    # #=       0   1   2   3   4   5   6   7   8   9     실제 테이터
    # sess = tf.Session()
    # sess.run(tf.argmax(mnist.train.labels[0].reshape(-1,10),1))   #= array([7])
    
    # 이미지 샘플 보기
    ## mnist.train.images[0] # 현재 784개 1차원 배열을 2차원 배열로 바꾸기
    # plt.imshow(mnist.train.images[0].reshape(28,28), cmap="Greys", interpolation="nearest")
    ## 28x28크기의 2차원 배열
    # plt.show()
    
    # 2. placeholder
    X = tf.placeholder(shape=[None,784], dtype=tf.float32)   # 784 = 28 * 28
    Y = tf.placeholder(shape=[None,10], dtype=tf.float32)   # 10 = 
    
    # 3. Weight & bias
    W = tf.Variable(tf.random_normal([784,10]), name="weight")
    b = tf.Variable(tf.random_normal([10]), name="bias")
    
    # 4. Hypothesis
    logit = tf.matmul(X,W) + b
    H = tf.nn.softmax(logit)
    
    # 5. cost function
    cost = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits_v2(logits=logit, labels=Y))
    
    # 6. train node
    train = tf.train.GradientDescentOptimizer(learning_rate=0.1).minimize(cost)
    
    # 7. Session & initializer
    sess = tf.Session()
    sess.run(tf.global_variables_initializer())
    
    # 8. training
    # 기존의 방식은 feed_dict값이 너무 크기 때문에 반복하기엔 학습이 어렵다. 그래서 배치 처리해야한다.
    # 그래서 방대한 양의 데이터를 처리할 때는 mnist에 있는 배치처리 함수를 쓴다. epoch
    # 1epoch은 전체 학습 데이터를 한번 학습했다라는 의미
    train_epoch = 1   # 30번 돌리겠다는 의미
    batch_size = 100  # 한번에 읽어들일 데이터의 크기  # 데이터가 방대하니 100행씩 잘라와서 처리하겠다는 의미
    
    # 기존방식은 데이터양이 많기 때문에 아래 배치 방법으로 사용해야한다.
    for step in range(30):
        _, cost_val = sess.run([train,cost], feed_dict={X:mnist.train.images,Y:mnist.train.labels})
        if step % 3 == 0:
            print("cost : {}".format(cost_val))
    
    # 55000개를 한번에 가져오는게 아니라 나눠서 가져와야한다.
    # for step in range(train_epoch):   # 30번 반복학습하겠다 라는 의미
    #     # num_of_iter 반복 횟수   # mnist.train.num_examples = 55000개  # batch_size = 100
    #     num_of_iter = int(mnist.train.num_examples / batch_size)
    #     cost_val = 0
    #     for i in range(num_of_iter):
    #         batch_x, batch_y = mnist.train.next_batch(batch_size)
    #         _, cost_val = sess.run([train,cost], feed_dict={X:batch_x, Y:batch_y})   
    #     if step % 3 == 0:
    #         print("Cost : {}".format(cost_val))
    
    # accuracy
    predict = tf.argmax(H,1)
    correct = tf.equal(predict, tf.argmax(Y,1))
    accuracy = tf.reduce_mean(tf.cast(correct, dtype=tf.float32))
    
    # 정확도 출력
    print("정확도 : {}".format(sess.run(accuracy, feed_dict={X:mnist.test.images, Y:mnist.test.labels})))
    
    #정확도가 90% 정도가 나온다. 그 이상가기 힘들어서 deep learning 기술을 사용하면 99%까지 올릴 수 있다.
```

## tensorflow 버전 확인.
```
    import tensorflow as tf
    print(tf.__version__)
```

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>