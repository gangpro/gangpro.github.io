---
layout: post
title: '[TIL] Deep Learning CNN 이미지 처리'
subtitle: 
categories: til
tags: til
comments: true
date: 2019-03-26 00:12:17 +0900
lastmod: 2019-03-26 14:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

Deep Learning CNN 이미지 처리<br />



# Deep Learning CNN

## gpu_env 가상환경 설치 후 확인 
    import tensorflow as tf
    tf.test.is_gpu_available()
    
# CNN(Convolution Neural Networks) 이미지 처리
## Simple CNN 이미지 처리 - Convolution + Relu 처리(1)(보통 처리)
    # pdf.p.82
    # 보통 처리 방법
    import tensorflow as tf
    import numpy as np
    
    # 4차원(1,3,3,1) 표현 방식
    # image = np.array([
    #                      [
    #                          [
    #                              [1],
    #                              [2],
    #                              [3],
    #                          ],
    #                          [
    #                              [4],
    #                              [5],
    #                              [6],
    #                          ],
    #                          [
    #                              [7],
    #                              [8],
    #                              [9],
    #                          ],
    #                      ]
    #                 ], dtype=np.float32)
    
    # image의 shape => (1,3,3,1) => (이미지개수, 가로,세로,색상)
    image = np.array([[[[1],[2],[3]],
                       [[4],[5],[6]],
                       [[7],[8],[9]]]], dtype=np.float32)
    
    # filter 1개의 shape => (2,2,1,1) => (가로,세로,색상,필터개수)
    # W = np.array([[[[1]],[[1]]],[[[1]],[[1]]]], dtype=np.float32)
    
    # filter 3개의 shape => (2,2,1,3) => (가로,세로,색상,필터개수)
    W = np.array([[[[1,10,-1]],
                   [[1,10,-1]]],
                  [[[1,10,-1]],
                   [[1,10,-1]]]], dtype=np.float32)
    
    # 이미지와 filter를 이용해서 convolution을 생성
    # strides=[1,1,1,1]   =>   stride size를 4차원으로 맞춰줘야 하니 [pading,size,size,pading]
    # padding="VALID"   => 원본이미지와 결과이미지 사이즈 다르게
    # padding="SAME"    => 원본이미지와 결과이미지 사이즈 같게
    # <보통 처리>
    # convolution 처리
    conv2d = tf.nn.conv2d(image,W,strides=[1,1,1,1],padding="VALID")
    # convolution처리한 값을 ReLu처리
    conv2d = tf.nn.relu(conv2d)
    
    sess = tf.Session()
    sess.run(conv2d)

<img width="518" alt="Screen Shot 2019-03-29 at 1 04 54 PM" src="https://user-images.githubusercontent.com/46523571/55208875-45ce6d00-5223-11e9-9050-882332baa94c.png">

## Simple CNN 이미지 처리 - Convolution + Relu 처리(2)(깔끔한 처리)
    # pdf.p.82
    # 깔끔한 처리 방법
    import tensorflow as tf
    import numpy as np
    
    # 4차원(1,3,3,1) 표현 방식
    # image의 shape => (1,3,3,1) => (이미지개수, 가로,세로,색상)
    image = np.array([[[[1],[2],[3]],
                       [[4],[5],[6]],
                       [[7],[8],[9]]]], dtype=np.float32)
    
    # filter 3개의 shape => (2,2,1,3) => (가로,세로,색상,필터개수)
    # W = np.array([[[[1,10,-1]],
    #                [[1,10,-1]]],
    #               [[[1,10,-1]],
    #                [[1,10,-1]]]], dtype=np.float32)
    
    
    
    # <깔끔한 처리>
    # 랜덤으로 만들어진 필터 개수를 쓰겠다라는 의미
    # kernel_size= 필터의 가로,세로
    # padding="VALID"   => 원본이미지와 결과이미지 사이즈 다르게
    # padding="SAME"    => 원본이미지와 결과이미지 사이즈 같게
    conv2d = tf.layers.conv2d(inputs=image, filters=32, kernel_size=[2,2], padding="VALID",
                              strides=1, activation=tf.nn.relu)
    sess = tf.Session()
    sess.run(conv2d)

## Simple CNN 이미지 처리 - MAX pooling 처리 방법
    import tensorflow as tf
    import numpy as np
    
    # images => (1,2,2,1)
    images = np.array([[[[4],[3]],
                       [[2],[1]]]], dtype=np.float32)
    
    # ksize = 커널 사이즈
    # strides= 스트라이드
    pool = tf.nn.max_pool(images, ksize=[1,2,2,1], strides=[1,1,1,1], padding="SAME")
    sess.run(pool)

<img width="351" alt="Screen Shot 2019-03-31 at 1 57 01 AM" src="https://user-images.githubusercontent.com/46523571/55279139-76fc8980-5358-11e9-9451-5346db6068e1.png">

## MNIST 이미지 1장으로 image 처리(1)
    from tensorflow.examples.tutorials.mnist import input_data
    import tensorflow as tf
    import matplotlib
    import matplotlib.pyplot as plt
    import numpy as np
    
    # 1. Data Loading
    mnist = input_data.read_data_sets("./data/mnist", one_hot=True)
    
    # 2. 처음 1장의 이미지만 가지고 실습
    img = mnist.train.images[0]   # 1차 배열
    # 2번 처리 확인차 실행
    plt.imshow(img.reshape(28,28), cmap="Greys")
    plt.show

<img width="547" alt="Screen Shot 2019-03-31 at 1 58 48 AM" src="https://user-images.githubusercontent.com/46523571/55279147-8bd91d00-5358-11e9-9bee-0bc992c641eb.png">

## MNIST 이미지 1장으로 Convolution 처리(2)
    from tensorflow.examples.tutorials.mnist import input_data
    import tensorflow as tf
    import matplotlib
    import matplotlib.pyplot as plt
    import numpy as np
    
    # 1. Data Loading
    mnist = input_data.read_data_sets("./data/mnist", one_hot=True)
    
    # 2. 처음 1장의 이미지만 가지고 실습
    img = mnist.train.images[0]   # 1차 배열
    # 2번 처리 확인차 실행
    # plt.imshow(img.reshape(28,28), cmap="Greys")
    # plt.show
    
    # 3. Convolution
    # 원본데이터의 형태부터 변경해줘야 한다.
    # shape=[-1,28,28,1]   =>  정해진이미지 1개, 가로 28, 세로 28, 색상 1 흑백
    # 1차원을 4차원으로 변경
    img = tf.reshape(img, shape=[-1,28,28,1])
    
    # 필터를 정의해줘야한다.(필터크기 : 3x3, 필터개수 : 5)
    # [3,3,1,5] => 3행, 3열, 1색상(흑백), 5장
    W = tf.Variable(tf.random_normal([3,3,1,5]))
    
    
    # padding이 "SAME"으로 잡혔지만 strides가 2이기 때문에 결과 이미지가 반으로 줄어서 나온다.
    conv2d = tf.nn.conv2d(img,W, strides=[1,2,2,1], padding="SAME")
    
    sess = tf.Session()
    sess.run(tf.global_variables_initializer())
    result = sess.run(conv2d)
    print(result.shape)   #= (1, 14, 14, 5)
    # 기존이미지 28x28에서 결과이미지가 1장의 이미지,14x14로 줄어들고, 필터가 5개
    
    
    # 결과 이미지를 출력해보기 위해서 데이터 처리를 약간 진행
    # 결과 이미지의 axes를 변경해서(5,14,14,1)으로 변경
    result = np.swapaxes(result,0,3)   # result에서 0번째와 3번째를 바꾸겠다
    print(result.shape)   #= (5, 14, 14, 1)   5장의 이미지, 가로 14, 세로14, 필터 1개라는 의미
    
    
    # fig는 한개의 틀이다. 그런데 1행 5열짜리로 서브플롯하겠다라는 의미.
    # plot는 한개만 그릴 수 있는데 subplot을 써서 구간을 나눌 수 있다.
    fig, axes = plt.subplots(1,5)   # plt.subplots(행1,열5)
    # fig는 틀, axes는 fig의 배열 인덱스 값이 들어감
    for idx, t_img in enumerate(result):
            # 4차원(5,14,14,1)에서 (5)를 가져오고 3차원을(14,14,1) 가져온다는 말
            # 그래서 (5)는 idx에, (14,14,1)은 t_img에 넣는 다는 말
            # 2차원 이미지로 보기 위해서 reshape(14,14)으로 처리
        axes[idx].imshow(t_img.reshape(14,14), cmap="Greys")
    
    plt.show()

<img width="575" alt="Screen Shot 2019-03-31 at 1 59 25 AM" src="https://user-images.githubusercontent.com/46523571/55279158-a27f7400-5358-11e9-932c-da72092cbfd3.png">

## MNIST 이미지 1장으로 Convolution -> Relu -> MAX pooling 처리(3)
    from tensorflow.examples.tutorials.mnist import input_data
    import tensorflow as tf
    import matplotlib
    import matplotlib.pyplot as plt
    import numpy as np
    
    # 1. Data Loading
    mnist = input_data.read_data_sets("./data/mnist", one_hot=True)
    
    # 2. 처음 1장의 이미지만 가지고 실습
    img = mnist.train.images[0]   # 1차 배열
    # 2번 처리 확인차 실행
    # plt.imshow(img.reshape(28,28), cmap="Greys")
    # plt.show
    
    # 3. Convolution
    # 원본데이터의 형태부터 변경해줘야 한다.
    # shape=[-1,28,28,1]   =>  정해진이미지 1개, 가로 28, 세로 28, 색상 1 흑백
    # 1차원을 4차원으로 변경
    img = tf.reshape(img, shape=[-1,28,28,1])
    
    # 필터를 정의해줘야한다.(필터크기 : 3x3, 필터개수 : 5)
    # [3,3,1,5] => 3행, 3열, 1색상(흑백), 5장
    W = tf.Variable(tf.random_normal([3,3,1,5]))
    
    
    # padding이 "SAME"으로 잡혔지만 strides가 2이기 때문에 결과 이미지가 반으로 줄어서 나온다.
    conv2d = tf.nn.conv2d(img,W, strides=[1,2,2,1], padding="SAME")
    
    # tf.nn.relu
    conv2d = tf.nn.relu(conv2d)
    
    # MAX pooling(sub sampling)
    #(이미지,커널사이즈[패딩,2,2,패딩],strides=2칸씩 -> [1,2,2,1],
    # padding="SAME" 페딩처리를 안해주면 나중에 데이터가 작아져서 데이터 유실이 일어날 수 있다.)
    pool = tf.nn.max_pool(conv2d,ksize=[1,2,2,1], strides=[1,2,2,1], padding="SAME")   
    
    
    sess = tf.Session()
    sess.run(tf.global_variables_initializer())
    
    result = sess.run(pool)
    print(result.shape)   #= (1, 7, 7, 5)
    # 기존이미지 28x28에서 결과이미지가 1장의 이미지,7x7로 줄어들고, 필터가 5개
    
    
    # 결과 이미지를 출력해보기 위해서 데이터 처리를 약간 진행
    # 결과 이미지의 axes를 변경해서(5,7,7,1)으로 변경
    result = np.swapaxes(result,0,3)   # result에서 0번째와 3번째를 바꾸겠다
    print(result.shape)   #= (5, 7, 7, 1)   5장의 이미지, 가로 7, 세로 7, 필터 1개라는 의미
    
    
    # fig는 한개의 틀이다. 그런데 1행 5열짜리로 서브플롯하겠다라는 의미.
    # plot는 한개만 그릴 수 있는데 subplot을 써서 구간을 나눌 수 있다.
    fig, axes = plt.subplots(1,5)   # plt.subplots(행1,열5)
    # fig는 틀, axes는 fig의 배열 인덱스 값이 들어감
    for idx, t_img in enumerate(result):
            # 4차원(5,7,7,1)에서 (5)를 가져오고 3차원을(7,7,1) 가져온다는 말
            # 그래서 (5)는 idx에, (7,7,1)은 t_img에 넣는 다는 말
            # 2차원 이미지로 보기 위해서 reshape(7,7)으로 처리
        axes[idx].imshow(t_img.reshape(7,7), cmap="Greys")
    
    plt.show()

<img width="553" alt="Screen Shot 2019-03-31 at 2 00 00 AM" src="https://user-images.githubusercontent.com/46523571/55279163-b6c37100-5358-11e9-9909-42c7c8c75f47.png">

## MNIST 이미지 1장으로 Convolution -> Relu -> MAX pooling -> CNN 처리(4)
    # pdf.p.91
    from tensorflow.examples.tutorials.mnist import input_data
    import tensorflow as tf
    
    # tensorflow graph 및 변수 초기화를 위해 처리 함수 사용
    tf.reset_default_graph()
    
    # 1. Data Loading
    mnist = input_data.read_data_sets("./data/mnist", one_hot=True)
    
    # 2. placeholder
    X = tf.placeholder(shape=[None,784], dtype=tf.float32)
    Y = tf.placeholder(shape=[None,10], dtype=tf.float32)
    
    # 2-1. dropout(overfitting을 방지하기 위해)
    keep_rate = tf.placeholder(dtype=tf.float32)   # dropout 비율
    
    
    # 3. convolution layer
    # 3-1. convolution layer 1   (CONV(reshape-fillter-convolution)/ReLU/pooling)
    
    # reshape
    # 입력데이터의 형태(None, 784)를 convolution 할 수 있도록 4차 배열로 reshape
    X_img = tf.reshape(X, shape=[-1,28,28,1])   #=shape=(?, 28, 28, 1)
    
    # fillter를 생성(필터 초기값 랜덤, 그리고 넣을 값은 내가 정해줘야함)
    #[3,3,1,32][가로,세로,색상,필터개수]   #stddev=0.01 너무 이상한 난수가 들어오지 못하게 표준편차 0.01
    W1 = tf.Variable(tf.random_normal([3,3,1,32], stddev=0.01))
    
    # convolution
    # strides=[패딩,1,1,패딩]=1칸씩 이동
    # padding="SAME"   = 원본과 출력 동일하게 만들기
    # activation map으로 처리됨
    L1 = tf.nn.conv2d(X_img, W1, strides=[1,1,1,1], padding="SAME")   #= (?, 28, 28, 32)
    
    # ReLU
    L1 = tf.nn.relu(L1)
    
    # MAX pooling
    # ksize=[패딩,2,2,패딩]   = 4칸씩가져와 큰거를 가져오겠다. # strides=[패딩,2,2,패딩]
    L1 = tf.nn.max_pool(L1, ksize=[1,2,2,1], strides=[1,2,2,1], padding="SAME")   #= (?, 14, 14, 32)
    
    
    
    # 3-2. convolution layer 2   (convolution layer1에서 숫자만 바꿔서 써도 상관없음. 2는 다른 방식 적용해보기)
    # Fillter, Convolution, ReLU 작업 한번에 처리
    # inputs=첫번째처리한결과값, fillters=랜덤, kernel_size=필터의 사이즈, strides=[몇칸씩], activation=ReLU사용
    L2 = tf.layers.conv2d(inputs=L1, filters=64, kernel_size=[3,3], padding="SAME", strides=1, activation=tf.nn.relu)
    #= (?, 14, 14, 64)
    
    # MAX pooling
    L2 = tf.layers.max_pooling2d(inputs=L2, pool_size=[2,2], padding="SAME", strides=2)
    #= (?, 7, 7, 64)
    # 참고로 필터가 64개이면 엄청난 데이터이기 때문에 dropout을 쓸 수 있지만
    # 일단 여기서 안쓰고 CNN에서 쓰는 걸로 한다.
    #### End of Convolution
    
    
    # 4. FC(Neural Network) - 최종 처리된 L2의 데이터를 사용하겠다 라는 의미.
    # L2는 4차원(?, 7, 7, 64) -> 2차원(?, 3136)
    L2 = tf.reshape(L2, shape=[-1,7*7*64])    #= (?, 3136)
    
    # 5. Weight & bias, ReLU
    # Nuaral Network에서 layer 처리
    W2 = tf.get_variable("weight2", shape=[7*7*64,256], initializer=tf.contrib.layers.xavier_initializer())
    b2 = tf.Variable(tf.random_normal([256]), name="bias2")
    _layer1 = tf.nn.relu(tf.matmul(L2,W2) + b2)
    layer1 = tf.nn.dropout(_layer1, keep_prob=keep_rate)
    
    W3 = tf.get_variable("weight3", shape=[256,256], initializer=tf.contrib.layers.xavier_initializer())
    b3 = tf.Variable(tf.random_normal([256]), name="bias3")
    _layer2 = tf.nn.relu(tf.matmul(layer1,W3) + b3)
    layer2 = tf.nn.dropout(_layer2, keep_prob=keep_rate)
    
    W4 = tf.get_variable("weight4", shape=[256,10], initializer=tf.contrib.layers.xavier_initializer())
    b4 = tf.Variable(tf.random_normal([10]), name="bias4")
    
    # Hypothesis
    H = tf.matmul(layer2,W4) + b4
    
    # cost function
    cost = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits_v2(logits=H, labels=Y))
    
    # train node(기존)
    # train = tf.train.GradientDescentOptimizer()
    
    # train node(새로운 방식(더 좋음))
    # AdamOptimizer의 경우는 값을 작게 줘야 더 좋게 나온다. 그래서 0.001
    train = tf.train.AdamOptimizer(learning_rate=0.001).minimize(cost)
    
    # session & initializer
    sess = tf.Session()
    sess.run(tf.global_variables_initializer())
    
    # training
    # 데이터가 클때는 배치 처리를 해야한다.
    num_of_epoch = 10 # 전체 트레이닝 데이터(55,000개 학습)  # 55,000 * 10 = 550,000
    batch_size = 100   # 데이터를 100개씩 불러오기
    
    for step in range(num_of_epoch):
        num_of_iter = int(mnist.train.num_examples / batch_size)   # mnist.train.num_examples = 55,000
        
        for i in range(num_of_iter):
            batch_x, batch_y = mnist.train.next_batch(batch_size)   #mnist.train.next_batch(batch_size) 55,000개에서 100개씩 읽어와
            _, cost_val = sess.run([train,cost], feed_dict={X:batch_x, Y:batch_y, keep_rate:0.5})
            
        print("cost : {}".format(cost_val))
        
    # accuracy(정확도)
    predict = tf.argmax(H,1)   #arg_max 0~9 중 가장 큰 값 선택
    correct = tf.equal(predict, tf.argmax(Y,1))
    accuracy = tf.reduce_sum(tf.cast(correct, dtype=tf.float32))   # tf.cast(correct, dtype=tf.float32) 맞으면 1 틀리면 0
    
    result_sum = 0;
    ### accuracy 측정을 위한 배치처리
    num_of_iter = int(mnist.test.num_examples / batch_size)   # mnist.train.num_examples = 55,000
    
    for i in range(num_of_iter):
        batch_x, batch_y = mnist.test.next_batch(batch_size)   #mnist.train.next_batch(batch_size) 55,000개에서 100개씩 읽어와
        correct_num = sess.run(accuracy, feed_dict={X:batch_x, Y:batch_y, keep_rate:1})
        # 100개 끊어서 맞춘개수를 더한 값이 correct_num
        
        result_sum += correct_num
        # 10000개 데이터로 테스트를 하니깐 10000개 맞추면 정확도 100%
    
    print("정확도 : {}".format(result_sum/10000))
    #= 정확도가 0.9909 나왔으니 99% 나옴.
    # 앙상블 
    # 100%를 만드는 방법은 이렇게 처리한 방법을 10번 한 다음 적절하게 처리 처리하면 99%보다 조금 더 높아진다.

<img width="552" alt="Screen Shot 2019-03-31 at 2 00 34 AM" src="https://user-images.githubusercontent.com/46523571/55279170-cb076e00-5358-11e9-9200-bc2d44d5b8f7.png">

## ensemble(앙상블)
    # Kaggle MNIST 문제를 작성 중 예측값을 더 높이는 방법
    
    # 우리의 MNIST의 해결 Mechanism
    # 하나의 model을 생성
    # CNN( Convolution layer, Neural Network)
    # train data를 이용해서 해당 모델을 학습
    # 최종적으로 우리가 얻어내는 것은 Hypothesis(가설), 예측모델을 만드는 것!
    # 이런 Hypothesis를 이용해서 예측값을 도출할 수 있다. 학습 끝.
    
    # test data(입력 feature)가 들어간다.
    # Hypothesis가 도출된다.
    # 이렇게 나온 결과 H에 argmax를 취하면 가장 큰 값의 index를 얻어낼 수 있고
    # 이 값을 입력 labe의 argmax와 비교해서 만약 같으면 예측이 잘 되었다. 만약 틀리면 예측모델이 이상한거다.
    
    # ensemble(앙상블)
    # 앙상블 기법(위와 같은 모델을 여러사람한테 물어봐서 결과값을 취합하는 것과 같은것.)
    # model이 여러개다. ex) 10개
    # 10개 model을 각각 학습시킨다.
    # 각 model에 입력 parameter(이미지 픽셀)를 넣어서 예측값을 알아낸다.
    # model 1 => [0.12, 0.02, 0.33, ..., 0.21]
    # model 2 => [0.13, 0.22, 0.13, ..., 0.11]
    # ...
    # model 10 => [0.22, 0.12, 0.26, ..., 0.25]
    # 각 열을 sum => [0.47, 0.24, 0.72, ..., 0.57] 이렇게 합친 후 argmax처리 한 후 label과 비교해서 예측

## 앙상블 간단한 예제
    import tensorflow as tf
    
    # python class 안에 정의 되어있는 함수의 첫번째 키워드에 self를 넣어야한다. (참고: 꼭 그런건 아님..)
    
    # 클래스
    class Student:
        # 생성자 
        def __init__(self,name,kor,eng,math):  # (self)현재 자기 자신의 인스턴스를 가리키는 말   
                                               # parameter name 객체 클래스 정의
            self.name = name
            self.kor = kor
            self.eng = eng
            self.math = math
    
        # 함수
        def calc_avg(self):
            return (self.kor + self.eng + self.math) / 3
    
    # 객체생성
    stu1 = Student("홍길동",10,20,30)   # name: "홍길동", kor:10, eng:20, math:30   # 데이터 캡슐레이션
    # 각각 함수 호출
    print(stu1.calc_avg())            # 결과값 : 20.0
    # 객체생성
    stu2 = Student("김길동",30,50,80)
    # 각각 함수 호출
    print(stu2.calc_avg())            # 결과값 : 53.333333333333336

## 앙상블 조금 더 상세히 체크
    import tensorflow as tf
    import pandas as pd
    
    # 여러개의 모델을 만들어서 관리를 해야한다.
    # 각각의 모델을 class의 instance로 만들어서 관리하자.
    # 우리가 만들 model의 데이터와 기능을 생각해서 class를 design을 해야한다.
    
    class CNNModel:
        # 사용하는 데이터를 field(변수로 선언)
        # 사용하는 기능은 method(함수)로 표현
        # - 데이터를 로딩하는 기능
        # - tensorflow graph를 그리는 기능(모델을 구축)
        # - 학습하는 기능(trainning)
        # - 정확도 측정(accuracy)
        # - 예측 작업(prediction)
        
        # def = 생성자 정의, 호출 //  __init__ 정해져있음.
        def __init__(self.name,sess):
            self.name = name   # self.name = instance 변수 // name = 지역 변수
            self.sess = sess
            
        def build_model(self):    #()괄호안에 인자가 들어올게 있으면 쓰고 없으면 안써도 된다.
            # placeholder부터 코드 작성, 그래프를 그리는 모든 코드를 다. 
            # placeholder, convolution layer, FC(NN), W&b, H, cost, train, predict, correct, accuracy, sess 전까지
            self.train = tf.train.AdamOptimizer(learning_rate=0.001).minimize(cost)
            
        def exec_train(self,x_data,y_data):   # 학습을 시키는 기능
            self.sess.run([self.train,cost], feed_dict={X:x_data, Y:y_data})
            
    # 1. data loading
    #    pandas를 이용해서 데이터를 읽는다.
    data_train = pd.read_csv("./data/digit_recognize/train.csv")
    data_test = pd.read_csv("./data/digit_recognize/test.csv")
    x_data = xxx
    y_data = yyy
    # 2. 모델 객체를 생성(10개)
    sess = tf.Session()
    model1 = CNNModel("model1",sess)   # 이처럼 모델을 무식한 방법으로 해도 되지만,
    model1 = CNNModel("model2",sess)   # 메들을 리스트에 넣어 작업하는게 좋다.
    model1 = CNNModel("model3",sess)
    model1.build_model()   # model1의 graph를 생성
    model1.exec_train(x_data)    # 학습
        
        
        





## References

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>