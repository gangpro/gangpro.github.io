---
layout: post
title: '[TIL] 텔레그램 API를 활용하여 봇 만들기'
subtitle: 
categories: til
tags: til telegram bot
comments: true
date: 2019-06-03 00:12:17 +0900
lastmod: 2019-06-03 20:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

텔레그램 API를 활용하여 봇 만들기<br />


## [0] 텔레그램 봇
> Bots: An introduction for developers <br>
> You control your bots using HTTPS requests to our bot API(https://core.telegram.org/bots/api). <br>

<br>
<br>
<br>

## [1] 텔레그램 Bot 만들기
1. 텔레그램 웹 접속 : ```https://web.telegram.org/```
2. 회원가입 후 로그인 하기
3. search 창에서 ```botfather``` 검색 
<img width="393" alt="Screen Shot 2019-06-03 at 18 54 05" src="https://user-images.githubusercontent.com/46523571/58793435-0fe9a280-8631-11e9-93fd-2393e10afe23.png">
4. 대화창에서 ```/start``` 메시지 보내기를 누르면 Botfather가 아래와 같이 사용방법을 알려준다.
<img width="776" alt="Screen Shot 2019-06-03 at 18 55 38" src="https://user-images.githubusercontent.com/46523571/58793521-40314100-8631-11e9-9ed4-8b7daaa576ad.png">

###
        BotFather
        I can help you create and manage Telegram bots. If you're new to the Bot API, please see the manual.
        
        You can control me by sending these commands:
        
        /newbot - create a new bot
        /mybots - edit your bots [beta]
        
        Edit Bots
        /setname - change a bot's name
        /setdescription - change bot description
        /setabouttext - change bot about info
        /setuserpic - change bot profile photo
        /setcommands - change the list of commands
        /deletebot - delete a bot
        
        Bot Settings
        /token - generate authorization token
        /revoke - revoke bot access token
        /setinline - toggle inline mode
        /setinlinegeo - toggle inline location requests
        /setinlinefeedback - change inline feedback settings
        /setjoingroups - can your bot be added to groups?
        /setprivacy - toggle privacy mode in groups
        
        Games
        /mygames - edit your games [beta]
        /newgame - create a new game
        /listgames - get a list of your games
        /editgame - edit a game
        /deletegame - delete an existing game

5. 메시지 창에서 ```/newbot``` 메시지 보내기를 통해서 bot 만들기
<img width="677" alt="Screen Shot 2019-06-03 at 18 57 01" src="https://user-images.githubusercontent.com/46523571/58793650-8c7c8100-8631-11e9-8457-2aaa79ea3d03.png">

###
        # 
        사용자 : /newbot
        
        BotFather : Alright, a new bot. How are we going to call it? Please choose a name for your bot.
        
        사용자 : kangpro
        
        BotFather : Good. Now let's choose a username for your bot. It must end in `bot`. Like this, for example: TetrisBot or tetris_bot.
        
        사용자 : kangpro_bot
        
        BotFather : 
        Done! Congratulations on your new bot. 
        You will find it at t.me/kangpro_bot. 
        You can now add a description, about section and profile picture for your bot, 
        see /help for a list of commands. 
        By the way, when you've finished creating your cool bot, ping our 
        Bot Support if you want a better username for it. 
        Just make sure the bot is fully operational before you do this.
        
        Use this token to access the HTTP API:
        <----------토큰값---------->
        Keep your token secure and store it safely, 
        it can be used by anyone to control your bot.
        
        For a description of the Bot API, 
        see this page: https://core.telegram.org/bots/api

<br>
<br>
<br>

## [2] 텔레그램 Bot 활용하기 : getMe
1. getMe 매뉴얼 확인하기 : ```https://core.telegram.org/bots/api```
-getMe 사용방법 알아보기
-```getMehttps://api.telegram.org/bot<token>/METHOD_NAME<토큰값>``` 
-<토큰값> URL에 검색해 보기 : ```https://api.telegram.org/bot<토큰값>/getMe``` 

2. 파이참에서 프로젝트 생성 - telegram.py 생성 - 코딩 
###
        import requests
        
        token = '<토큰값>'
        api_url = f'https://api.telegram.org/bot{token}'
        
        response = requests.get(api_url + '/getMe').json()
        print(response)
        
        # 아래와 같이 출력되면 된다.  
        # {'ok': True, 'result': {'id': 713810722, 'is_bot': True, 'first_name': 'kangpro', 'username': 'kangpro_bot'}}

<br>
<br>
<br>

## [3] 텔레그램 Bot 활용하기 : getUpdates
1. Search에서 kangpro_bot 검색 후 ```/start``` 하기 
2. getUpdates 매뉴얼 확인하기 : ```https://core.telegram.org/bots/api```  
-getUpdateshttps://api.telegram.org/bot<토큰값>/getUpdates
3. URL에 검색해 보기 : https://api.telegram.org/bot<토큰값>/getMe 
4. 파이참 프로젝트 - telegram.py 수정 - 코딩
###  
    import requests
    
    token = '<토큰값>'
    api_url = f'https://api.telegram.org/bot{token}'
    
    # getMe를 통해 접속 체크
    # response = requests.get(api_url + '/getMe').json()
    # print(response)  
    # {'ok': True, 'result': {'id': 713810722, 'is_bot': True, 'first_name': 'kangpro', 'username': 'kangpro_bot'}}
    
    # getUpdates  
    # chat_id에게 메시지를 보낼 준비하기
    response = requests.get(api_url + '/getUpdates').json()
    chat_id = response['result'][0]['message']['from']['id']
    print(chat_id)  
    # 728488945

<br>
<br>
<br>

## [4] 텔레그램 Bot 활용하기 : sendMessage
1. sendMessage 매뉴얼 확인하기 ( https://core.telegram.org/bots/api ) 
-sendMessage에 반드시 들어가야 하는 것 체크 (chat_id(사용자 아이디)와 text)
2. URL에 검색해 보기 : ```https://api.telegram.org/bot<토큰값>/sendMessage?chat_id=728488945&text=안녕하세요. 저는 봇입니다.```
3. 파이참 프로젝트 - telegram.py 수정 - 코딩
###
    import requests
    
    token = '<토큰값>'
    api_url = f'https://api.telegram.org/bot{token}'
    
    # getMe를 통해 접속 체크
    # response = requests.get(api_url + '/getMe').json()
    # print(response)  
    # {'ok': True, 'result': {'id': 713810722, 'is_bot': True, 'first_name': 'kangpro', 'username': 'kangpro_bot'}}
    
    # getUpdates  
    # chat_id에게 메시지를 보낼 준비하기
    response = requests.get(api_url + '/getUpdates').json()
    chat_id = response['result'][0]['message']['from']['id']
    print(chat_id)  
    # 728488945
    
    # text = '안녕하세요. 또 메시지를 보냅니다 :)'  -> app.py - send_message('곧 점심이네요.')  # 매개 변수매개변수 설정했으므로 주석처리
    # requests.get(api_url + f'/sendMessage?chat_id={chat_id}&text={text}')  # 참고로 이렇게 적어도 되고 아래처럼 적어도 된다.
    requests.get(f'{api_url}/sendMessage?chat_id={chat_id}&text={text}')

<br>
<br>
<br>

## [5] 텔레그램 Bot 활용하기 : 새로운 app.py 생성
* 파이참 프로젝트 - app.py 생성 - 코딩

###
    from flask import Flask, request
    import pprint
    import requests
    from decouple import config
    app = Flask(__name__)       # app 초기화 과정
###    
    
    token = config('TOKEN')
    api_url = f'https://api.telegram.org/bot{token}'
###    
    
    # http://127.0.0.1:5000/
    @app.route('/')  # Decorator  # root 디렉토리로 들어오면 happy hacking를 출력시킬거란 의미
    def root():
        return '<h1>Happy Hacking!</h1>' \
               '<h3>root 페이지입니다.</h3>'
###    
    
    @app.route(f'/{token}', methods=['POST'])
    def telegram():    
        # [1] 방식
        # response = request.get_json()
        # # print(request.get_json())  # 사용자가 텔레그램 웹에서 메시지를 보내면 flask 서버에서 프린트 된다.  # {'update_id': 385295894, 'message': {'message_id': 27, 'from': {'id': 728488945, 'is_bot': False, 'first_name': 'River', 'last_name': 'Kang', 'username': 'kangpro404', 'language_code': 'en'}, 'chat': {'id': 728488945, 'first_name': 'River', 'last_name': 'Kang', 'username': 'kangpro404', 'type': 'private'}, 'date': 1559530336, 'text': '11시 52분'}}
        # # 파일 보기가 좋지 않으니 파일 상단에 import pprint 해서 아래와 같이 사용하면 좋다.
        # # pprint.pprint(request.get_json())
        #
        # # JSON에서 사용자의 id 추출
        # chat_id = response['message']['from']['id']
        # # print('사용자 아이디(chat_id): ', chat_id)
        #
        # # JSON에서 사용자가 입력한 text 추출
        # chat_text = response['message']['text']
        # # print('사용자가 보낸 문자(chat_text): ', chat_text)
        #
        # # 사용자가 입력한 걸 챗봇이 똑같이 출력
        # requests.get(f'{api_url}/sendMessage?chat_id={chat_id}&text={chat_text}')
###    
    
        # [2] 방식 - 사용자가 메시지를 입력했는지 안했는지 체크하고 결과를 출력하는 로직
        pprint.pprint(request.get_json())
        message = request.get_json().get('message')
        if message is not None:
            chat_id = message.get('from').get('id')
            chat_text = message.get('text')
            requests.get(f'{api_url}/sendMessage?chat_id={chat_id}&text={chat_text}')
    
            # # 추가 예시
            # if text == '점심메뉴':
            #     # 점심메뉴를 확인하는 코드
            #     text = '점심메뉴'
            # elif text == '로또':
            #     # 로또를 확인하는 코드
            #     text = '특정 로또번호'
    
        return '', 200
###    
    
    # ※ 아래 함수?는 맨 아래에 둘 것!(위의 app.route 함수를 다 실행 한 뒤에 app.run 앱을 실행하겠다기 때문에)
    # python name.py로 설정할 수 있도록 설정방법
    # app.py 파일이 'python app.py'로 시작되었을 때 서버를 시작하겠다라는 의미.
    if __name__ == '__main__':
        app.run(debug=True)
        # '서버가 실행이 되어있는동안 수정이 되면 자동으로 재시작을 하겠다'라는 의미

<br>
<br>
<br>

## [6] 텔레그램 Bot 활용하기 : webhook
1. 파이참 프로젝트 - set_webhook.py 생성 - 코딩
###  
    # <setWebhook 정의> : https://core.telegram.org/bots/api
    # Use this method to specify a url and receive incoming updates via an outgoing webhook.
    # Whenever there is an update for the bot,
    # we will send an HTTPS POST request to the specified url, containing a JSON-serialized Update.
    # In case of an unsuccessful request, we will give up after a reasonable amount of attempts.
    # Returns True on success.
    
    # If you'd like to make sure that the Webhook request comes from Telegram,
    # we recommend using a secret path in the URL,
    # e.g. https://www.example.com/<token>.
    # Since nobody else knows your bot‘s token, you can be pretty sure it’s us.
    
###    
    # <ngrok 사용법>
    # ngrok 파일을 프로젝트 폴더에 놓는다. (아래 이미지 참고)
    # app.py 서버 실행 중에 터미널 세션 추가 후
    # 터미널에서 ngrok http 5000 실행
    # http://66b14c17.ngrok.io이 새롭게 생성 된다. (주의할점 : 8시간 뒤 변경 되니 체크하고 사용해야한다.)(아래 이미지 참고)
<img width="187" alt="Screen Shot 2019-06-03 at 19 12 56" src="https://user-images.githubusercontent.com/46523571/58794654-a5863180-8633-11e9-8d82-96a901c61ee4.png">
<img width="678" alt="Screen Shot 2019-06-03 at 19 15 28" src="https://user-images.githubusercontent.com/46523571/58794798-ff86f700-8633-11e9-9984-b4e4ea6fa528.png">
    
    # set_webhook.py에 코딩 부분    
    token = config('<토큰값>')
    api_url = f'https://api.telegram.org/bot{token}'
    webhook_url = input()
    
    print(f'{api_url}/setWebhook?url={webhook_url}')
###
    # 기존 실행중인 app.py 서버 터미널, ngrok 서버 실행 터미널 외에 새로운 터미널 세션 추가
    # 터미널에서 python set_webhook.py 실행하면 아무것도 안뜸 그런 상태에서 아래 url 입력
    # ngrok 서버 IP + 토큰정보를 터미널에 입력 -> http://66b14c17.ngrok.io/<토큰값>
    # 새롭게 생성된 걸 인터넷 창에서 열어보기  https://api.telegram.org/bot<토큰값>/setWebhook?url=https://66b14c17.ngrok.io/<토큰값>
    # JSON 파일을 확인 할 수 있음(세팅 완료)

<br>
<br>
<br>

## [7] 텔레그램 Bot Token 값 숨기기?
1. 터미널 실행
2. ```touch .env``` 파일 생성
3. 파일 트리에서 .env 파일을 text 파일로 열기
4. TOKEN='<토큰값>' 값 입력
5. app.py, set_webhook.py 등 <토큰값> 사용한거 아래와 같이 코드 수정
- (상단에 추가) from decouple import config -> 터미널에서 ```pip install python-decouple``` 추가 설치 후 사용가능
- (추가) 토큰 값 수정
  - (기존) token = '713810722:AAFvBo7jbqPTnxOFTrAoZkzamWQi4JmzBh4'
  - (변경) token = config('TOKEN')
6. Git에 올라갈 파일에 나의 개인정보인 토큰값은 별도의 .env에 저장된 후 이를 제외한 모든 파일이 업로드 된다.

<br>
<br>
<br>

## [8] 텔레그램 Bot을 활용 완성?
* 사용자가 입력을 하면 똑같이 출력 한다 :) 이제 코딩을 해보자.
<img width="856" alt="Screen Shot 2019-06-03 at 19 31 56" src="https://user-images.githubusercontent.com/46523571/58795695-4d9cfa00-8636-11e9-9d96-3e2b06186e24.png">



## References

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>