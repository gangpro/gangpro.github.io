---
layout: post
title: '[TIL] Node & React를 활용하여 Docker에 서버 구축'
subtitle: 
categories: til
tags: til node react docker
comments: true
date: 2021-01-04 00:12:17 +0900
lastmod: 2021-01-07 20:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

Node & React를 활용하여 Docker에 서버 구축<br />



## 프로젝트 폴더 및 개발환경구축

```basic
➜  workspace mkdir project-a && cd project-a && npm init -y
Wrote to /Users/kang/workspace/project-a/package.json:

{
  "name": "project-a",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

## React 앱 생성

```basic

➜  project-a npx create-react-app client

Creating a new React app in /Users/kang/Documents/workspace/project-a/client.

Installing packages. This might take a couple of minutes.
Installing react, react-dom, and react-scripts with cra-template...

yarn add v1.17.3
[1/4] 🔍  Resolving packages...
[2/4] 🚚  Fetching packages...
[3/4] 🔗  Linking dependencies...
warning "react-scripts > @typescript-eslint/eslint-plugin > tsutils@3.17.1" has unmet peer dependency "typescript@>=2.8.0 || >= 3.2.0-dev || >= 3.3.0-dev || >= 3.4.0-dev || >= 3.5.0-dev || >= 3.6.0-dev || >= 3.6.0-beta || >= 3.7.0-dev || >= 3.7.0-beta".
[4/4] 🔨  Building fresh packages...
success Saved lockfile.
warning Your current version of Yarn is out of date. The latest version is "1.22.5", while you're on "1.17.3".
info To upgrade, run the following command:
$ curl --compressed -o- -L https://yarnpkg.com/install.sh | bash
success Saved 6 new dependencies.
info Direct dependencies
├─ cra-template@1.1.1
├─ react-dom@17.0.1
├─ react-scripts@4.0.1
└─ react@17.0.1
info All dependencies
├─ cra-template@1.1.1
├─ react-dev-utils@11.0.1
├─ react-dom@17.0.1
├─ react-scripts@4.0.1
├─ react@17.0.1
└─ scheduler@0.20.1
✨  Done in 11.17s.

Initialized a git repository.

Installing template dependencies using yarnpkg...
yarn add v1.17.3
[1/4] 🔍  Resolving packages...
[2/4] 🚚  Fetching packages...
[3/4] 🔗  Linking dependencies...
warning "react-scripts > @typescript-eslint/eslint-plugin > tsutils@3.17.1" has unmet peer dependency "typescript@>=2.8.0 || >= 3.2.0-dev || >= 3.3.0-dev || >= 3.4.0-dev || >= 3.5.0-dev || >= 3.6.0-dev || >= 3.6.0-beta || >= 3.7.0-dev || >= 3.7.0-beta".
warning " > @testing-library/user-event@12.6.0" has unmet peer dependency "@testing-library/dom@>=7.21.4".
[4/4] 🔨  Building fresh packages...
success Saved lockfile.
success Saved 17 new dependencies.
info Direct dependencies
├─ @testing-library/jest-dom@5.11.8
├─ @testing-library/react@11.2.2
├─ @testing-library/user-event@12.6.0
├─ react-dom@17.0.1
├─ react@17.0.1
└─ web-vitals@0.2.4
info All dependencies
├─ @testing-library/dom@7.29.0
├─ @testing-library/jest-dom@5.11.8
├─ @testing-library/react@11.2.2
├─ @testing-library/user-event@12.6.0
├─ @types/aria-query@4.2.0
├─ @types/jest@26.0.19
├─ @types/testing-library__jest-dom@5.9.5
├─ css.escape@1.5.1
├─ css@3.0.0
├─ dom-accessibility-api@0.5.4
├─ lz-string@1.4.4
├─ min-indent@1.0.1
├─ react-dom@17.0.1
├─ react@17.0.1
├─ redent@3.0.0
├─ strip-indent@3.0.0
└─ web-vitals@0.2.4
✨  Done in 6.54s.
Removing template package using yarnpkg...

yarn remove v1.17.3
[1/2] 🗑  Removing module cra-template...
[2/2] 🔨  Regenerating lockfile and installing missing dependencies...
warning " > @testing-library/user-event@12.6.0" has unmet peer dependency "@testing-library/dom@>=7.21.4".
warning "react-scripts > @typescript-eslint/eslint-plugin > tsutils@3.17.1" has unmet peer dependency "typescript@>=2.8.0 || >= 3.2.0-dev || >= 3.3.0-dev || >= 3.4.0-dev || >= 3.5.0-dev || >= 3.6.0-dev || >= 3.6.0-beta || >= 3.7.0-dev || >= 3.7.0-beta".
success Uninstalled packages.
✨  Done in 5.01s.

Created git commit.

Success! Created client at /Users/kang/Documents/workspace/project-a/client
Inside that directory, you can run several commands:

  yarn start
    Starts the development server.

  yarn build
    Bundles the app into static files for production.

  yarn test
    Starts the test runner.

  yarn eject
    Removes this tool and copies build dependencies, configuration files
    and scripts into the app directory. If you do this, you can’t go back!

We suggest that you begin by typing:

  cd client
  yarn start

Happy hacking!
➜  project-a
```

## Node 생성

index.js 파일 생성

```basic
➜  project-a touch server.js
➜  project-a vi server.js
```

```basic
#! /usr/bin/env node

const http = require("http");

// 포트 환경 변수
const PORT = process.env.PORT || 5000;

// 노드 서버 생성
const SERVER = http.createServer();

// 선택한 서버에서 포트 시작
SERVER.listen(PORT);

SERVER.on("listening", () => {
    console.log("[Server]::LISTEN:%s", PORT);
});

// 서버 연결 및 에러 체크 콜백 함수
SERVER.on("error", error => {
    throw new Error(`[Server]::ERROR:${error.message}`);
});
```

노드와 리액트 동시 실행할 수 있도록 package.json에 스크립트 추가

package.json 파일 수정

```basic
➜  project-a npm i -D concurrently
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN project-a@1.0.0 No description
npm WARN project-a@1.0.0 No repository field.

+ concurrently@5.3.0
added 57 packages from 50 contributors and audited 57 packages in 5.366s

4 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities

➜  project-a
```

```basic
➜  project-a vi package.json
```

```basic
{
  "name": "project-a",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "server": "node index.js",
    "client": "npm start --prefix client",
    "dev": "concurrently \"npm run server\" \"npm run client\""
  },
  "keywords": [],
  "author": "gangpro",
  "license": "ISC",
  "devDependencies": {
    "concurrently": "^5.3.0"
  }
}
```

개발 서버 구동 확인

```basic
$ npm run dev

➜  project-a npm run dev

> project-a@1.0.0 dev /Users/kang/Documents/workspace/project-a
> concurrently "npm run server" "npm run client"

[0] 
[0] > project-a@1.0.0 server /Users/kang/Documents/workspace/project-a
[0] > node index.js
[0] 
[1] 
[1] > project-a@1.0.0 client /Users/kang/Documents/workspace/project-a
[1] > npm start --prefix client
[1] 
[0] [Server]::LISTEN:5000
[1] 
[1] > client@0.1.0 start /Users/kang/Documents/workspace/project-a/client
[1] > react-scripts start
[1] 
[1] ℹ ｢wds｣: Project is running at http://192.168.25.50/
[1] ℹ ｢wds｣: webpack output is served from 
[1] ℹ ｢wds｣: Content not from webpack is served from /Users/kang/Documents/workspace/project-a/client/public
[1] ℹ ｢wds｣: 404s will fallback to /
[1] Starting the development server...
[1] 
[1] Compiled successfully!
[1] 
[1] You can now view client in the browser.
[1] 
[1]   Local:            http://localhost:3000
[1]   On Your Network:  http://192.168.25.50:3000
[1] 
[1] Note that the development build is not optimized.
[1] To create a production build, use yarn build.
[1]
```

화면 접속 확인

```jsx
[http://localhost:3000](http://localhost:3000/)
```

## 도커 클라이언트 사이드 세팅(React)

```basic
➜  project-a cd client      
➜  client git:(master) ✗ touch Dockerfile
➜  client git:(master) ✗ vi Dockerfile
```

```basic
FROM node:lts-slim
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app
EXPOSE 3000
CMD [ "npm", "start" ]
```

## 도커 서버 사이드 세팅(Node)

```basic
➜  project-a touch Dockerfile
➜  project-a vi Dockerfile 
```

```basic
FROM node:lts-slim
RUN mkdir -p /usrr/src/app
WORKDIR /usr/src/app
EXPOSE 5000

# You can change this
CMD [ "npm", "run", "dev" ]
```

## 도커 클라이언트와 서버 구분을 위한 명칭 수정 및 파일 이동

```basic
➜  project-a mkdir server

➜  project-a mv index.js ./server 
➜  project-a mv Dockerfile ./server
➜  project-a mv package.json ./server
➜  project-a mv package-lock.json ./server
➜  project-a mv node_modules ./server
```

## 도커와 함께 구동

```basic
➜  project-a touch docker-compose.yml
➜  project-a vi docker-compose.yml
```

```basic
version: "1"
services:
    frontend:
        container_name: project-a
        build:
            context: ./client
            dockerfile: Dockerfile
        image: gangpro/project_web
        ports:
            - "3000:3000"
        volumes:
            - ./client:/usr/src/app
    backend:
        container_name: awesome_server
        build:
            context: ./server
            dockerfile: Dockerfile
        image: gangpro/project_server
        ports:
            - "5000:5000"
        volumes:
            - ./server:/usr/src/app
```

마지막으로 프록시 수정

```basic
➜  project-a cd client 
➜  client git:(master) ✗ vi package.json
```

```basic
{
  "name": "client",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "@testing-library/jest-dom": "^5.11.4",
    "@testing-library/react": "^11.1.0",
    "@testing-library/user-event": "^12.1.10",
    "react": "^17.0.1",
    "react-dom": "^17.0.1",
    "react-scripts": "4.0.1",
    "web-vitals": "^0.2.4"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "eslintConfig": {
    "extends": [
      "react-app",
      "react-app/jest"
    ]
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  },
  "proxy": "http://localhost:5000"
}
```

불필요?파일 숨김 처리하여 서버에 반영

```basic
➜  project-a touch .dockerignore
➜  project-a vi .dockerignore
```

```basic
node_modules
./client/node_modules
./server/node_modules
npm-debug.log
```

Now we can run our project by running following command. This will create docker images and volumes which will run in the containers.

```basic
➜  project-a docker-compose up
```

If you want to build your images before starting your containers.

```basic
➜  project-a docker-compose up --build
```

This can be a tedious approach to work with as you must have the knowledge of docker and docker-compose. But It has some advantage:

- One setup for all development workflow.
- Docker can be used for any programming language.
- Production Deloyment can be a breeze, if you use docker in your DevOps.
- No npm package required (though replaced by docker).

## 마지막 접속 확인

```basic
➜  server npm run dev

> project-a@1.0.0 dev /Users/kang/Documents/workspace/project-a/server
> concurrently --kill-others-on-fail "yarn server" "yarn client"

yarn run v1.17.3
yarn run v1.17.3
$ nodemon server.js
[1] $ cd ../client && npm start
[0] [nodemon] 2.0.4
[0] [nodemon] to restart at any time, enter `rs`
[0] [nodemon] watching path(s): *.*
[0] [nodemon] watching extensions: js,mjs,json
[0] [nodemon] starting `node server.js`
[0] [Server]::LISTEN:5000
[1] npm WARN lifecycle The node binary used for scripts is /var/folders/p5/2vjmwbqn0k75g002f3v66vrw0000gn/T/yarn--1609690148438-0.8276043425217803/node but npm is using /Users/kang/.nvm/versions/node/v12.18.1/bin/node itself. Use the `--scripts-prepend-node-path` option to include the path for the node binary npm was executed with.
[1] 
[1] > client@0.1.0 start /Users/kang/Documents/workspace/project-a/client
[1] > react-scripts start
[1] 
[1] ℹ ｢wds｣: Project is running at http://192.168.25.50/
[1] ℹ ｢wds｣: webpack output is served from 
[1] ℹ ｢wds｣: Content not from webpack is served from /Users/kang/Documents/workspace/project-a/client/public
[1] ℹ ｢wds｣: 404s will fallback to /
[1] Starting the development server...
[1] 
[1] Compiled successfully!
[1] 
[1] You can now view client in the browser.
[1] 
[1]   Local:            http://localhost:3000
[1]   On Your Network:  http://192.168.25.50:3000
[1] 
[1] Note that the development build is not optimized.
[1] To create a production build, use yarn build.
[1]
```

## 참고, 추가하면 좋을 것

[https://scotch.io/tutorials/react-docker-with-security-in-10-minutes](https://scotch.io/tutorials/react-docker-with-security-in-10-minutes)

[https://github.com/oktadeveloper/okta-react-docker-example](https://github.com/oktadeveloper/okta-react-docker-example)

## References

- 따라하기 블로그 : [https://dev.to/numtostr/running-react-and-node-js-in-one-shot-with-concurrently-2oac](https://dev.to/numtostr/running-react-and-node-js-in-one-shot-with-concurrently-2oac)
- 소스코드 참고 블로그 : [https://github.com/boffti/cfu](https://github.com/boffti/cfu)
- 개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀드립니다.<br/>