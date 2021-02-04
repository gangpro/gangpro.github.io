---
layout: post
title: '[TIL] Deep Learning'
subtitle: 
categories: til
tags: til
comments: true
date: 2019-03-26 00:12:17 +0900
lastmod: 2019-03-26 13:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

Deep Learning에 대해서<br />


# Deep Learning

## AND True Table - AND 연산이 로지스틱 학습이 가능한지 알아보자
```
    # AND 연산에 학습이 가능한지 알아보자. pdf.p.8
    # Logistic Regression
    import tensorflow as tf
    
    # 1. training data set
    x_data = [[0,0],
              [0,1],
              [1,0],
              [1,1]]
    y_data = [[0],[0],[0],[1]]   # AND Operation
    
    # 2. placeholder
    X = tf.placeholder(shape=[None,2], dtype=tf.float32)
    Y = tf.placeholder(shape=[None,1], dtype=tf.float32)
    
    # 3. Weight & bias
    W = tf.Variable(tf.random_normal([2,1]), name="weight")
    b = tf.Variable(tf.random_normal([1]), name="bias")
    
    # 4. Hypothesis
    logit = tf.matmul(X,W) + b
    H = tf.sigmoid(logit)
    
    # 5. cost function
    cost = tf.reduce_mean(tf.nn.sigmoid_cross_entropy_with_logits(logits=logit, labels=Y))
    
    # 6. train node
    train = tf.train.GradientDescentOptimizer(learning_rate=0.01).minimize(cost)
    
    # 7. sesstion & initializer
    sess = tf.Session()
    sess.run(tf.global_variables_initializer())
    
    # 8. training
    for step in range(3000):
        _, cost_val = sess.run([train,cost], feed_dict={X:x_data, Y:y_data})
        if step % 300 == 0:
            print("Cost : {}".format(cost_val))
    
    # 9. accuracy
    predict = tf.cast(H > 0.5, dtype=tf.float32)   # 0.5보다 크면 1에 가깝다. 즉 1은 True
    correct = tf.equal(predict, Y)   # 예측값(predict)과 실제값(Y) 비교
    accuracy = tf.reduce_mean(tf.cast(correct, dtype=tf.float32))
    print("정확도 : {}".format(sess.run(accuracy, feed_dict={X:x_data, Y:y_data})))
    #= 정확도 : 거의 무조건 1 나옴.
```
<img width="301" alt="Screen Shot 2019-03-29 at 12 58 30 PM" src="https://user-images.githubusercontent.com/46523571/55208668-5e8a5300-5222-11e9-9a7a-98165dfe17af.png">

## OR True Table - OR 연산이 로지스틱 학습이 가능한지 알아보자
```
    # OR 연산에 학습이 가능한지 알아보자. pdf.p.8
    # Logistic Regression
    import tensorflow as tf
    
    # 1. training data set
    x_data = [[0,0],
              [0,1],
              [1,0],
              [1,1]]
    y_data = [[0],[1],[1],[1]]   # OR Operation
    
    # 2. placeholder
    X = tf.placeholder(shape=[None,2], dtype=tf.float32)
    Y = tf.placeholder(shape=[None,1], dtype=tf.float32)
    
    # 3. Weight & bias
    W = tf.Variable(tf.random_normal([2,1]), name="weight")
    b = tf.Variable(tf.random_normal([1]), name="bias")
    
    # 4. Hypothesis
    logit = tf.matmul(X,W) + b
    H = tf.sigmoid(logit)
    
    # 5. cost function
    cost = tf.reduce_mean(tf.nn.sigmoid_cross_entropy_with_logits(logits=logit, labels=Y))
    
    # 6. train node
    train = tf.train.GradientDescentOptimizer(learning_rate=0.01).minimize(cost)
    
    # 7. sesstion & initializer
    sess = tf.Session()
    sess.run(tf.global_variables_initializer())
    
    # 8. training
    for step in range(3000):
        _, cost_val = sess.run([train,cost], feed_dict={X:x_data, Y:y_data})
        if step % 300 == 0:
            print("Cost : {}".format(cost_val))
    
    # 9. accuracy
    predict = tf.cast(H > 0.5, dtype=tf.float32)   # 0.5보다 크면 1에 가깝다. 즉 1은 True
    correct = tf.equal(predict, Y)   # 예측값(predict)과 실제값(Y) 비교
    accuracy = tf.reduce_mean(tf.cast(correct, dtype=tf.float32))
    print("정확도 : {}".format(sess.run(accuracy, feed_dict={X:x_data, Y:y_data})))
    #= 정확도 : 거의 무조건 1 나옴.
```

<img width="293" alt="Screen Shot 2019-03-29 at 12 59 10 PM" src="https://user-images.githubusercontent.com/46523571/55208686-7661d700-5222-11e9-9981-f7d54e15c5a1.png">

## XOR True Table - XOR 연산이 로지스틱 학습이 가능한지 알아보자
```
    # XOR 연산에 학습이 가능한지 알아보자. pdf.p.8
    # Logistic Regression
    import tensorflow as tf
    
    # 1. training data set
    x_data = [[0,0],
              [0,1],
              [1,0],
              [1,1]]
    y_data = [[0],[1],[1],[0]]   # XOR Operation
    
    # 2. placeholder
    X = tf.placeholder(shape=[None,2], dtype=tf.float32)
    Y = tf.placeholder(shape=[None,1], dtype=tf.float32)
    
    # 3. Weight & bias
    W = tf.Variable(tf.random_normal([2,1]), name="weight")
    b = tf.Variable(tf.random_normal([1]), name="bias")
    
    # 4. Hypothesis
    logit = tf.matmul(X,W) + b
    H = tf.sigmoid(logit)
    
    # 5. cost function
    cost = tf.reduce_mean(tf.nn.sigmoid_cross_entropy_with_logits(logits=logit, labels=Y))
    
    # 6. train node
    train = tf.train.GradientDescentOptimizer(learning_rate=0.01).minimize(cost)
    
    # 7. sesstion & initializer
    sess = tf.Session()
    sess.run(tf.global_variables_initializer())
    
    # 8. training
    for step in range(3000):
        _, cost_val = sess.run([train,cost], feed_dict={X:x_data, Y:y_data})
        if step % 300 == 0:
            print("Cost : {}".format(cost_val))
    
    # 9. accuracy
    predict = tf.cast(H > 0.5, dtype=tf.float32)   # 0.5보다 크면 1에 가깝다. 즉 1은 True
    correct = tf.equal(predict, Y)   # 예측값(predict)과 실제값(Y) 비교
    accuracy = tf.reduce_mean(tf.cast(correct, dtype=tf.float32))
    print("정확도 : {}".format(sess.run(accuracy, feed_dict={X:x_data, Y:y_data})))
    #= 정확도 : 0.25~0.75 정도 나옴.
```
<img width="293" alt="Screen Shot 2019-03-29 at 12 59 42 PM" src="https://user-images.githubusercontent.com/46523571/55208701-8974a700-5222-11e9-8648-7bb4fa746e75.png">

## Multiple layer를 이용한 XOR문제 해결방법(layer 2개 사용)
```
    ### Multiple layer를 이용한 XOR문제 해결   # pdf p.21
    import tensorflow as tf
    
    # 1. training data set
    x_data = [[0,0],
              [0,1],
              [1,0],
              [1,1]]
    y_data = [[0],[1],[1],[0]]
    
    # 2. placeholder
    X = tf.placeholder(shape=[None,2], dtype=tf.float32)
    Y = tf.placeholder(shape=[None,1], dtype=tf.float32)
    
    #<기존>
    # 3. Weight & bias
    # W = tf.Variable(tf.random_normal([2,1]), name="weight")
    # b = tf.Variable(tf.random_normal([1]), name="bias")
    
    # 3. Weight & bias - layer 2개 방법
    W1 = tf.Variable(tf.random_normal([2,2]), name="weight1")
    b1 = tf.Variable(tf.random_normal([2]), name="bias1")
    layer1 = tf.sigmoid(tf.matmul(X,W1) + b1)   # layer1은 두번째 로지스틱의 입력값으로 들어간다.
    
    W2 = tf.Variable(tf.random_normal([2,1]), name="weight2")
    b2 = tf.Variable(tf.random_normal([1]), name="bias2")
    
    # 4. Hypothesis
    logit = tf.matmul(layer1,W2) + b2
    H = tf.sigmoid(logit)
    # 이렇게 레이어를 계속 연결하는게 뉴럴 네트워크, 신경망이 된다.
    
    # 5. cost function
    cost = tf.reduce_mean(tf.nn.sigmoid_cross_entropy_with_logits(logits=logit, labels=Y))
    
    # 6. train node
    train = tf.train.GradientDescentOptimizer(learning_rate=0.1).minimize(cost)
    
    # 7. sesstion & initializer
    sess = tf.Session()
    sess.run(tf.global_variables_initializer())
    
    # 8. training
    for step in range(3000):
        _, cost_val = sess.run([train,cost], feed_dict={X:x_data, Y:y_data})
        if step % 300 == 0:
            print("Cost : {}".format(cost_val))
    
    # 9. accuracy
    predict = tf.cast(H > 0.5, dtype=tf.float32)   # 0.5보다 크면 1에 가깝다. 즉 1은 True
    correct = tf.equal(predict, Y)   # 예측값(predict)과 실제값(Y) 비교
    accuracy = tf.reduce_mean(tf.cast(correct, dtype=tf.float32))
    print("정확도 : {}".format(sess.run(accuracy, feed_dict={X:x_data, Y:y_data})))
    #= 정확도 : 0.5~1 정도 나옴.
```

<img width="292" alt="Screen Shot 2019-03-29 at 1 00 17 PM" src="https://user-images.githubusercontent.com/46523571/55208735-a8733900-5222-11e9-8ce5-c5f7267b4a28.png">

## Multiple layer를 이용한 XOR문제 해결방법(layer 여러개 방법)
```
    ### Multiple layer를 이용한 XOR문제 해결   # pdf p.21
    import tensorflow as tf
    
    # 1. training data set
    x_data = [[0,0],
              [0,1],
              [1,0],
              [1,1]]
    y_data = [[0],[1],[1],[0]]
    
    # 2. placeholder
    X = tf.placeholder(shape=[None,2], dtype=tf.float32)
    Y = tf.placeholder(shape=[None,1], dtype=tf.float32)
    
    # 3. Weight & bias - layer 여러개 방법
    # [2,2] -> x값이 들어왔을 때 로지스틱 2개를 쓴다는 의미
    # W1 = tf.Variable(tf.random_normal([2,2]), name="weight1")
    # [2,20] -> x값이 들어왔을 때 로지스틱 20개를 쓴다는 의미. 즉 여러개 돌린다는 의미.
    # 한 layer에서 2개 처리하던걸 20개 처리하니깐 학습을 많이 한다는 의미. wide 표현.
    # [2,숫자] -> 숫자에 큰수를 넣으면 그만큼 학습을 많이 한다는 의미.
    # 결론적으로 맞춰줘야 할 것은 X[None,2] -> W1[2,숫자], b1[숫자] -> W2[숫자,1], b2[1]
    W1 = tf.Variable(tf.random_normal([2,20]), name="weight1")
    b1 = tf.Variable(tf.random_normal([20]), name="bias1")
    layer1 = tf.sigmoid(tf.matmul(X,W1) + b1)  # layer1은 두번째 로지스틱의 입력값으로 들어간다.
    
    W2 = tf.Variable(tf.random_normal([20,1]), name="weight2")
    b2 = tf.Variable(tf.random_normal([1]), name="bias2")
    
    # 4. Hypothesis
    logit = tf.matmul(layer1,W2) + b2
    H = tf.sigmoid(logit)
    # 이렇게 레이어를 계속 연결하는게 뉴럴 네트워크, 신경망이 된다.
    
    # 5. cost function
    cost = tf.reduce_mean(tf.nn.sigmoid_cross_entropy_with_logits(logits=logit, labels=Y))
    
    # 6. train node
    train = tf.train.GradientDescentOptimizer(learning_rate=0.1).minimize(cost)
    
    # 7. sesstion & initializer
    sess = tf.Session()
    sess.run(tf.global_variables_initializer())
    
    # 8. training
    for step in range(10000):
        _, cost_val = sess.run([train,cost], feed_dict={X:x_data, Y:y_data})
        if step % 1000 == 0:
            print("Cost : {}".format(cost_val))
    
    # 9. accuracy
    predict = tf.cast(H > 0.5, dtype=tf.float32)   # 0.5보다 크면 1에 가깝다. 즉 1은 True
    correct = tf.equal(predict, Y)   # 예측값(predict)과 실제값(Y) 비교
    accuracy = tf.reduce_mean(tf.cast(correct, dtype=tf.float32))
    print("정확도 : {}".format(sess.run(accuracy, feed_dict={X:x_data, Y:y_data})))
    #= 정확도 : 1 정도 나옴.
```

<img width="300" alt="Screen Shot 2019-03-29 at 1 01 09 PM" src="https://user-images.githubusercontent.com/46523571/55208761-bd4fcc80-5222-11e9-9764-8e96df0e6aeb.png">

## Multiple layer를 이용한 XOR문제 해결방법(layer 여러개 + depth여러개 방법)
```
    ### Multiple layer를 이용한 XOR문제 해결   # pdf p.21
    import tensorflow as tf
    
    # 1. training data set
    x_data = [[0,0],
              [0,1],
              [1,0],
              [1,1]]
    y_data = [[0],[1],[1],[0]]
    
    # 2. placeholder
    X = tf.placeholder(shape=[None,2], dtype=tf.float32)
    Y = tf.placeholder(shape=[None,1], dtype=tf.float32)
    
    # 3. Weight & bias - layer 여러개 + deep 여러개 방법
    # 결론적으로 맞춰줘야 할 것은
    # X[None,2] -> W1[2,숫자], b1[숫자] -> W2[숫자,숫자2], b2[숫자2] -> W3[숫자2,1], b3[숫자1]
    # 이렇게 deep게 나누는 것을 딥 러닝이라고 한다.
    # 로지스틱 수를 늘리고 뎁스를 늘리면 좋아지긴 하지만 너무 깊게 하면 안된다.
    W1 = tf.Variable(tf.random_normal([2,256]), name="weight1")
    b1 = tf.Variable(tf.random_normal([256]), name="bias1")
    layer1 = tf.sigmoid(tf.matmul(X,W1) + b1)  # layer1은 두번째 로지스틱의 입력값으로 들어간다.
    
    W2 = tf.Variable(tf.random_normal([256,512]), name="weight2")
    b2 = tf.Variable(tf.random_normal([512]), name="bias2")
    layer2 = tf.sigmoid(tf.matmul(layer1,W2) + b2)  # layer1은 두번째 로지스틱의 입력값으로 들어간다.
    
    W3 = tf.Variable(tf.random_normal([512,1]), name="weight3")
    b3 = tf.Variable(tf.random_normal([1]), name="bias3")
    #= 뎁스 3단계.
    # 학습이 너무 잘되면 오버피팅이 발생하여 커스텀 잘해야한다.
    
    # 4. Hypothesis
    logit = tf.matmul(layer2,W3) + b3
    H = tf.sigmoid(logit)
    # 이렇게 레이어를 계속 연결하는게 뉴럴 네트워크, 신경망이 된다.
    
    # 5. cost function
    cost = tf.reduce_mean(tf.nn.sigmoid_cross_entropy_with_logits(logits=logit, labels=Y))
    
    # 6. train node
    train = tf.train.GradientDescentOptimizer(learning_rate=0.1).minimize(cost)
    
    # 7. sesstion & initializer
    sess = tf.Session()
    sess.run(tf.global_variables_initializer())
    
    # 8. training
    for step in range(3000):
        _, cost_val = sess.run([train,cost], feed_dict={X:x_data, Y:y_data})
        if step % 300 == 0:
            print("Cost : {}".format(cost_val))
    
    # 9. accuracy
    predict = tf.cast(H > 0.5, dtype=tf.float32)   # 0.5보다 크면 1에 가깝다. 즉 1은 True
    correct = tf.equal(predict, Y)   # 예측값(predict)과 실제값(Y) 비교
    accuracy = tf.reduce_mean(tf.cast(correct, dtype=tf.float32))
    print("정확도 : {}".format(sess.run(accuracy, feed_dict={X:x_data, Y:y_data})))
    #= 정확도 : 1 정도 나옴.
```

<img width="354" alt="Screen Shot 2019-03-29 at 1 01 41 PM" src="https://user-images.githubusercontent.com/46523571/55208780-d2c4f680-5222-11e9-9a1c-36c70f645518.png">

## (복습) MNIST Multinomial Classification
```
    # MNIST Multinomial Classification 머신러닝에서 로지스틱 1개를 가지고 mnist 처리
    from tensorflow.examples.tutorials.mnist import input_data
    import tensorflow as tf
    
    # 1. Data Loading
    mnist = input_data.read_data_sets("./data/mnist", one_hot=True)
    
    # MNIST(개수, 가로, 세로, 색상) 4차원 형태
    
    # 2. placeholder
    # X쪽 데이터(입력데이터)는 이미지 데이터이고
    # 원래 이미지 데이터는 3차원 데이터인데 MNIST는 흑백이기 때문에
    # 2차원 형태의 이미지 데이터이고 처리를 쉽게하기 위해 이미지 자체의 데이터를 1차원으로 표현
    # 288*28 이미지인데 1차원으로 표현 28*28=784 열이 된다.
    X = tf.placeholder(shape=[None,784], dtype=tf.float32)
    # Y측 label이 one_hot 인코딩 때문에 0~10까지 숫자인 10을 열에 [None,10]으로 써야한다.
    Y = tf.placeholder(shape=[None,10], dtype=tf.float32)
    
    # < 기존 >
    # # 3. Weight & bias
    # W = tf.Variable(tf.random_normal([784,10]), name="weight")
    # b = tf.Variable(tf.random_normal([10], name="bias"))
    # # 4. Hypothesis
    # logit = tf.matmul(X,W) + b
    # H = tf.nn.softmax(logit)
    
    # < 새로운 > 
    # 3. Weight & bias
    W1 = tf.Variable(tf.random_normal([784,256]), name="weight1")
    b1 = tf.Variable(tf.random_normal([256], name="bias1"))
    layer1 = tf.sigmoid(tf.matmul(X,W1) + b1)
    
    W2 = tf.Variable(tf.random_normal([256,512]), name="weight2")
    b2 = tf.Variable(tf.random_normal([512], name="bias2"))
    layer2 = tf.sigmoid(tf.matmul(layer1,W2) + b2)
    
    W3 = tf.Variable(tf.random_normal([512,10]), name="weight3")
    b3 = tf.Variable(tf.random_normal([10], name="bias3"))
    
    # 4. Hypothesis
    logit = tf.matmul(layer2,W3) + b3
    H = tf.nn.softmax(logit)   # 확률값으로 결과를 얻기 위해
    
    # 5. cost function
    cost = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits_v2(logits=logit, labels=Y))
    
    # 6. train node
    train = tf.train.GradientDescentOptimizer(learning_rate=0.01).minimize(cost)
    
    # 7. Session & initializer
    sess = tf.Session()
    sess.run(tf.global_variables_initializer())
    
    # 8. training
    # 기존의 방식은 feed_dict값이 너무 크기 때문에 반복하기엔 학습이 어렵다. 그래서 배치 처리해야한다.
    # 그래서 방대한 양의 데이터를 처리할 때는 mnist에 있는 배치처리 함수를 쓴다. epoch
    # 1epoch은 전체 학습 데이터를 한번 학습했다라는 의미
    train_epoch = 30   # 30번 돌리겠다는 의미
    batch_size = 100  # 한번에 읽어들일 데이터의 크기  # 데이터가 방대하니 100행씩 잘라와서 처리하겠다는 의미
    
    # 기존방식은 데이터양이 많기 때문에 아래 배치 방법으로 사용해야한다.
    # for step in range(30):
    #     _, cost_val = sess.run([train,cost], feed_dict={X:mnist.train.images,Y:mnist.train.labels})
    #     if step % 3 == 0:
    #         print("cost : {}".format(cost_val))
    
    # 55000개를 한번에 가져오는게 아니라 나눠서 가져와야한다.
    for step in range(train_epoch):   # 30번 반복학습하겠다 라는 의미
        # num_of_iter 반복 횟수   # mnist.train.num_examples = 55000개  # batch_size = 100
        num_of_iter = int(mnist.train.num_examples / batch_size)
        cost_val = 0
        for i in range(num_of_iter):
            batch_x, batch_y = mnist.train.next_batch(batch_size)
            _, cost_val = sess.run([train,cost], feed_dict={X:batch_x, Y:batch_y})   
        if step % 3 == 0:
            print("Cost : {}".format(cost_val))
    
    # accuracy
    predict = tf.argmax(H,1)
    correct = tf.equal(predict, tf.argmax(Y,1))
    accuracy = tf.reduce_mean(tf.cast(correct, dtype=tf.float32))
    
    # 정확도 출력
    print("정확도 : {}".format(sess.run(accuracy, feed_dict={X:mnist.test.images, Y:mnist.test.labels})))
    
    #정확도가 90% 정도가 나온다. 그 이상가기 힘들어서 deep learning 기술을 사용하면 99%까지 올릴 수 있다.
```

<img width="551" alt="Screen Shot 2019-03-29 at 1 02 40 PM" src="https://user-images.githubusercontent.com/46523571/55208807-f425e280-5222-11e9-90f9-25e70d831df9.png">

## 정확도를 높이기 위한 방법
* sigmoid -> Relu 변경
* W 초기값 수정
* Overfitting을 방지하기 위해 Dropout 사용
  * (※ 학습할 땐 몇개를 끄고, 테스트할 땐 다 키고 한다.)

## ReLU(Rectified Linear Unit) & W 초기값 수정
```
    # MNIST Multinomial Classification 머신러닝에서 로지스틱 1개를 가지고 mnist 처리
    # MNIST - Neural Network(wide & deep) 
    #         => Sigmoid => ReLU로 변경                         => 91% 나옴
    #         => Xavier initialization 도입해서 초기 w값 지정   => 97% 나옴
    #         => overfitting을 피하기 위해 dropout을 도입       => 98% 나옴
    from tensorflow.examples.tutorials.mnist import input_data
    import tensorflow as tf
    
    tf.reset_default_graph()   #이전에 사용했던 것들 초기화 해줌
    
    # 1. Data Loading
    mnist = input_data.read_data_sets("./data/mnist", one_hot=True)
    
    # 2. placeholder
    # X쪽 데이터(입력데이터)는 이미지 데이터이고
    # 원래 이미지 데이터는 3차원 데이터인데 MNIST는 흑백이기 때문에
    # 2차원 형태의 이미지 데이터이고 처리를 쉽게하기 위해 이미지 자체의 데이터를 1차원으로 표현
    # 28*28 이미지인데 1차원으로 표현 28*28=784 열이 된다.
    X = tf.placeholder(shape=[None,784], dtype=tf.float32)
    # Y측 label이 one_hot 인코딩 때문에 0~10까지 숫자인 10을 열에 [None,10]으로 써야한다.
    Y = tf.placeholder(shape=[None,10], dtype=tf.float32)
    
    
    ################################################################################
    
    # # 3. Weight & bias (기존)
    # W1 = tf.Variable(tf.random_normal([784,256]), name="weight1")
    # b1 = tf.Variable(tf.random_normal([256], name="bias1"))
    # layer1 = tf.sigmoid(tf.matmul(X,W1) + b1)
    
    # W2 = tf.Variable(tf.random_normal([256,512]), name="weight2")
    # b2 = tf.Variable(tf.random_normal([512], name="bias2"))
    # layer2 = tf.sigmoid(tf.matmul(layer1,W2) + b2)
    
    # W3 = tf.Variable(tf.random_normal([512,10]), name="weight3")
    # b3 = tf.Variable(tf.random_normal([10], name="bias3"))
    
    # # 4. Hypothesis
    # logit = tf.matmul(layer2,W3) + b3
    # H = tf.nn.softmax(logit)   # 확률값으로 결과를 얻기 위해
    
    ################################################################################
    
    # sigmoid -> ReLU // W 초기값 수정 // dropout 해서 정확도 높이기.
    # 3. Weight & bias (sigmoid를 ReLu로 바꾸고 W 초기값 방식 수정)
    keep = tf.placeholder(dtype=tf.float32)   # 학습과 테스트를 따로 사용해야하기 때문에 새로운 placeholder 생성
    
    W1 = tf.get_variable("weight1", shape=[784,256], initializer=tf.contrib.layers.xavier_initializer())
    b1 = tf.Variable(tf.random_normal([256], name="bias1"))
    _layer1 = tf.nn.relu(tf.matmul(X,W1) + b1)
    layer1 = tf.nn.dropout(_layer1, keep_prob=keep)
    
    W2 = tf.get_variable("weight2", shape=[256,512], initializer=tf.contrib.layers.xavier_initializer())
    b2 = tf.Variable(tf.random_normal([512], name="bias2"))
    _layer2 = tf.nn.relu(tf.matmul(layer1,W2) + b2)
    layer2 = tf.nn.dropout(_layer2, keep_prob=keep)
    
    W3 = tf.get_variable("weight3", shape=[512,10], initializer=tf.contrib.layers.xavier_initializer())
    b3 = tf.Variable(tf.random_normal([10], name="bias3"))
    
    # 4. Hypothesis
    H = tf.matmul(layer2,W3) + b3
    
    ################################################################################
    
    # 5. cost function
    cost = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits_v2(logits=H, labels=Y))
    
    # 6. train node
    train = tf.train.GradientDescentOptimizer(learning_rate=0.15).minimize(cost)
    
    # 7. Session & initializer
    sess = tf.Session()
    sess.run(tf.global_variables_initializer())
    
    # 8. training
    # 기존의 방식은 feed_dict값이 너무 크기 때문에 반복하기엔 학습이 어렵다. 그래서 배치 처리해야한다.
    # 그래서 방대한 양의 데이터를 처리할 때는 mnist에 있는 배치처리 함수를 쓴다. epoch
    # 1epoch은 전체 학습 데이터를 한번 학습했다라는 의미
    train_epoch = 30   # 30번 돌리겠다는 의미
    batch_size = 100  # 한번에 읽어들일 데이터의 크기  # 데이터가 방대하니 100행씩 잘라와서 처리하겠다는 의미
    
    # 55000개를 한번에 가져오는게 아니라 나눠서 가져와야한다.
    for step in range(train_epoch):   # 30번 반복학습하겠다 라는 의미
        # num_of_iter 반복 횟수   # mnist.train.num_examples = 55000개  # batch_size = 100
        num_of_iter = int(mnist.train.num_examples / batch_size)
        # cost_val = 0
        for i in range(num_of_iter):
            batch_x, batch_y = mnist.train.next_batch(batch_size)
            _, cost_val = sess.run([train,cost], feed_dict={X:batch_x, Y:batch_y, keep:0.7}) #keep:0.7의미는 X와 Y 데이터의 70%사용해라는 의미   
        if step % 3 == 0:
            print("Cost : {}".format(cost_val))
    
    # accuracy
    predict = tf.argmax(H,1)
    correct = tf.equal(predict, tf.argmax(Y,1))
    accuracy = tf.reduce_mean(tf.cast(correct, dtype=tf.float32))
    
    # 정확도 출력
    # keep:1의 의미는 전체 파일 100% 사용해라는 의미
    print("정확도 : {}".format(sess.run(accuracy, feed_dict={X:mnist.test.images, Y:mnist.test.labels, keep:1})))
    
    
    # 딥러닝을 통해 정확도가 98%이상으로 향상됐다.
    # MNIST Multinomial Classification 머신러닝에서 로지스틱 1개를 가지고 mnist 처리
    # MNIST - Neural Network(wide & deep) 
    #         => Sigmoid => ReLU로 변경                         => 91% 나옴
    #         => Xavier initialization 도입해서 초기 w값 지정   => 97% 나옴
    #         => overfitting을 피하기 위해 dropout을 도입       => 98% 나옴
    from tensorflow.examples.tutorials.mnist import input_data
    import tensorflow as tf
    
    tf.reset_default_graph()   #이전에 사용했던 것들 초기화 해줌
    
    # 1. Data Loading
    mnist = input_data.read_data_sets("./data/mnist", one_hot=True)
    
    # 2. placeholder
    # X쪽 데이터(입력데이터)는 이미지 데이터이고
    # 원래 이미지 데이터는 3차원 데이터인데 MNIST는 흑백이기 때문에
    # 2차원 형태의 이미지 데이터이고 처리를 쉽게하기 위해 이미지 자체의 데이터를 1차원으로 표현
    # 28*28 이미지인데 1차원으로 표현 28*28=784 열이 된다.
    X = tf.placeholder(shape=[None,784], dtype=tf.float32)
    # Y측 label이 one_hot 인코딩 때문에 0~10까지 숫자인 10을 열에 [None,10]으로 써야한다.
    Y = tf.placeholder(shape=[None,10], dtype=tf.float32)
    
    
    ################################################################################
    
    # # 3. Weight & bias (기존)
    # W1 = tf.Variable(tf.random_normal([784,256]), name="weight1")
    # b1 = tf.Variable(tf.random_normal([256], name="bias1"))
    # layer1 = tf.sigmoid(tf.matmul(X,W1) + b1)
    
    # W2 = tf.Variable(tf.random_normal([256,512]), name="weight2")
    # b2 = tf.Variable(tf.random_normal([512], name="bias2"))
    # layer2 = tf.sigmoid(tf.matmul(layer1,W2) + b2)
    
    # W3 = tf.Variable(tf.random_normal([512,10]), name="weight3")
    # b3 = tf.Variable(tf.random_normal([10], name="bias3"))
    
    # # 4. Hypothesis
    # logit = tf.matmul(layer2,W3) + b3
    # H = tf.nn.softmax(logit)   # 확률값으로 결과를 얻기 위해
    
    ################################################################################
    
    # sigmoid -> ReLU // W 초기값 수정 // dropout 해서 정확도 높이기.
    # 3. Weight & bias (sigmoid를 ReLu로 바꾸고 W 초기값 방식 수정)
    keep = tf.placeholder(dtype=tf.float32)   # 학습과 테스트를 따로 사용해야하기 때문에 새로운 placeholder 생성
    
    W1 = tf.get_variable("weight1", shape=[784,256], initializer=tf.contrib.layers.xavier_initializer())
    b1 = tf.Variable(tf.random_normal([256], name="bias1"))
    _layer1 = tf.nn.relu(tf.matmul(X,W1) + b1)
    layer1 = tf.nn.dropout(_layer1, keep_prob=keep)
    
    W2 = tf.get_variable("weight2", shape=[256,512], initializer=tf.contrib.layers.xavier_initializer())
    b2 = tf.Variable(tf.random_normal([512], name="bias2"))
    _layer2 = tf.nn.relu(tf.matmul(layer1,W2) + b2)
    layer2 = tf.nn.dropout(_layer2, keep_prob=keep)
    
    W3 = tf.get_variable("weight3", shape=[512,10], initializer=tf.contrib.layers.xavier_initializer())
    b3 = tf.Variable(tf.random_normal([10], name="bias3"))
    
    # 4. Hypothesis
    H = tf.matmul(layer2,W3) + b3
    
    ################################################################################
    
    # 5. cost function
    cost = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits_v2(logits=H, labels=Y))
    
    # 6. train node
    train = tf.train.GradientDescentOptimizer(learning_rate=0.15).minimize(cost)
    
    # 7. Session & initializer
    sess = tf.Session()
    sess.run(tf.global_variables_initializer())
    
    # 8. training
    # 기존의 방식은 feed_dict값이 너무 크기 때문에 반복하기엔 학습이 어렵다. 그래서 배치 처리해야한다.
    # 그래서 방대한 양의 데이터를 처리할 때는 mnist에 있는 배치처리 함수를 쓴다. epoch
    # 1epoch은 전체 학습 데이터를 한번 학습했다라는 의미
    train_epoch = 30   # 30번 돌리겠다는 의미
    batch_size = 100  # 한번에 읽어들일 데이터의 크기  # 데이터가 방대하니 100행씩 잘라와서 처리하겠다는 의미
    
    # 55000개를 한번에 가져오는게 아니라 나눠서 가져와야한다.
    for step in range(train_epoch):   # 30번 반복학습하겠다 라는 의미
        # num_of_iter 반복 횟수   # mnist.train.num_examples = 55000개  # batch_size = 100
        num_of_iter = int(mnist.train.num_examples / batch_size)
        # cost_val = 0
        for i in range(num_of_iter):
            batch_x, batch_y = mnist.train.next_batch(batch_size)
            _, cost_val = sess.run([train,cost], feed_dict={X:batch_x, Y:batch_y, keep:0.7}) #keep:0.7의미는 X와 Y 데이터의 70%사용해라는 의미   
        if step % 3 == 0:
            print("Cost : {}".format(cost_val))
    
    # accuracy
    predict = tf.argmax(H,1)
    correct = tf.equal(predict, tf.argmax(Y,1))
    accuracy = tf.reduce_mean(tf.cast(correct, dtype=tf.float32))
    
    # 정확도 출력
    # keep:1의 의미는 전체 파일 100% 사용해라는 의미
    print("정확도 : {}".format(sess.run(accuracy, feed_dict={X:mnist.test.images, Y:mnist.test.labels, keep:1})))
    
    
    # 딥러닝을 통해 정확도가 98%이상으로 향상됐다.
```

<img width="551" alt="Screen Shot 2019-03-29 at 1 03 38 PM" src="https://user-images.githubusercontent.com/46523571/55208837-19b2ec00-5223-11e9-95e6-12c1c8ad3c75.png">

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>