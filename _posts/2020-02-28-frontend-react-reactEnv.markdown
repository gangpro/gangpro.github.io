---
layout: post
title: '[react] 리엑트 환경구축'
subtitle: 
categories: frontend
tags: react
comments: true
date: 2020-02-28 10:00:17 +0900
lastmod: 2020-02-28 11:00:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

리엑트(react) 환경구축<br />

## 1. 환경 구축
---
### Node.js 설치하기

nvm 은 여러 종류의 Node.js 버전을 설치 할 수 있게 해주는 버전입니다. 나중에 새 버전이 나왔을 때 업데이트 하기도 쉽고, 터미널을 통해 어떤 버전을 사용 할지 설정 할 수도 있어서 편리합니다.

#### NVM 설치

```bash
sudo curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.1/install.sh | bash
nvm install --lts
```

#### NVM 설치 확인

```bash
$ nvm ls

-bash: nvm: command not found
```

당황하지 않고 ``vi ~/.bash_profile`` 실행 후 아래 코드 추가

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" # This loads nvm
```

재시작

```bash
source ~/.bash_profile
```

#### NVM 설치 확인

```bash
$ nvm ls
```

```bash
->       system
node -> stable (-> N/A) (default)
iojs -> N/A (default)
```

#### Node 설치

6.10.1 버전 설치

```bash
nvm install 6.10.1
```
```bash
➜  ~ nvm install 6.10.1
Downloading and installing node v6.10.1...
Downloading https://nodejs.org/dist/v6.10.1/node-v6.10.1-darwin-x64.tar.xz...
######################################################################## 100.0%
Computing checksum with shasum -a 256
Checksums matched!
Now using node v6.10.1 (npm v3.10.10)
Creating default alias: default -> 6.10.1 (-> v6.10.1)
➜  ~ 
```

#### Node 설치 확인

```bash
nvm ls
```

```bash
➜  ~ nvm ls
->      v6.10.1
         system
default -> 6.10.1 (-> v6.10.1)
node -> stable (-> v6.10.1) (default)
stable -> 6.10 (-> v6.10.1) (default)
iojs -> N/A (default)
lts/* -> lts/dubnium (-> N/A)
lts/argon -> v4.9.1 (-> N/A)
lts/boron -> v6.17.1 (-> N/A)
lts/carbon -> v8.16.0 (-> N/A)
lts/dubnium -> v10.16.2 (-> N/A)
```

```bash
➜  ~ node -v 
v6.10.1
➜  ~ 
```

#### 다른 버전 노드 설치 및 버전 변경

```bash
# 7.7.4 버전 설치
$ nvm install 7.7.4

# 설치 확인
$ nvm ls
        v6.10.1
->       v7.7.4
         system
default -> 6.10.1 (-> v6.10.1)
node -> stable (-> v7.7.4) (default)
stable -> 7.7 (-> v7.7.4) (default)
iojs -> N/A (default)
lts/* -> lts/boron (-> v6.10.1)
lts/argon -> v4.8.1 (-> N/A)
lts/boron -> v6.10.1

$ node -v 
v7.7.4

# 노드 버전 변경
$ nvm use 6.10.1

```



### Yarn 설치하기

Yarn 설치는 [Yarn Installation](https://yarnpkg.com/en/docs/install) 페이지에서 여러분의 운영체제에 맞는 방식에 따라 설치하시면 됩니다.

Homebrew

```bash
brew install yarn
```

```bash
Updating Homebrew...
^C==> Installing dependencies for yarn: node
==> Installing yarn dependency: node

==> Downloading https://homebrew.bintray.com/bottles/node-12.4.0.mojave.bottle.1.tar.gz
==> Downloading from https://akamai.bintray.com/f1/f1172f2ee2e6f3bce711e9213ee31c7ff9d87cb0d
######################################################################## 100.0%
==> Pouring node-12.4.0.mojave.bottle.1.tar.gz
==> Caveats
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d
==> Summary
🍺  /usr/local/Cellar/node/12.4.0: 4,505 files, 52.1MB
==> Installing yarn
==> Downloading https://yarnpkg.com/downloads/1.17.0/yarn-v1.17.0.tar.gz
==> Downloading from https://github-production-release-asset-2e65be.s3.amazonaws.com/4997064
######################################################################## 100.0%
🍺  /usr/local/Cellar/yarn/1.17.0: 14 files, 5MB, built in 19 seconds
==> `brew cleanup` has not been run in 30 days, running now...
Removing: /Users/kang/Library/Caches/Homebrew/Cask/ngrok--latest.zip... (15.8MB)
Removing: /Users/kang/Library/Logs/Homebrew/postgresql... (1.2KB)
Removing: /Users/kang/Library/Logs/Homebrew/heroku... (114B)
Removing: /Users/kang/Library/Logs/Homebrew/gdbm... (64B)
Removing: /Users/kang/Library/Logs/Homebrew/python... (3 files, 132.6KB)
Removing: /Users/kang/Library/Logs/Homebrew/icu4c... (64B)
Removing: /Users/kang/Library/Logs/Homebrew/readline... (64B)
Removing: /Users/kang/Library/Logs/Homebrew/sqlite... (64B)
Removing: /Users/kang/Library/Logs/Homebrew/xz... (64B)
Removing: /Users/kang/Library/Logs/Homebrew/heroku-node... (119B)
Removing: /Users/kang/Library/Logs/Homebrew/openssl... (64B)
==> Caveats
==> node
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d
➜  ~ 

```

#### script 설치

```bash
curl -o- -L https://yarnpkg.com/install.sh | bash
```

```bash
➜  ~ curl -o- -L https://yarnpkg.com/install.sh | bash
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  7148    0  7148    0     0  23577      0 --:--:-- --:--:-- --:--:-- 23590
Installing Yarn!
> Downloading tarball...

[1/2]: https://yarnpkg.com/latest.tar.gz --> /var/folders/p5/2vjmwbqn0k75g002f3v66vrw0000gn/T/yarn.tar.gz.XXXXXXXXXX.Pu6WC8wm
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    93  100    93    0     0    787      0 --:--:-- --:--:-- --:--:--   788
100   609    0   609    0     0   1092      0 --:--:-- --:--:-- --:--:--  594k
100 1211k  100 1211k    0     0   435k      0  0:00:02  0:00:02 --:--:--  888k

[2/2]: https://yarnpkg.com/latest.tar.gz.asc --> /var/folders/p5/2vjmwbqn0k75g002f3v66vrw0000gn/T/yarn.tar.gz.XXXXXXXXXX.Pu6WC8wm.asc
100    97  100    97    0     0    904      0 --:--:-- --:--:-- --:--:--   904
100   613    0   613    0     0   1144      0 --:--:-- --:--:-- --:--:--  1144
100   832  100   832    0     0    984      0 --:--:-- --:--:-- --:--:--  812k
> WARNING: GPG is not installed, integrity can not be verified!
> Extracting to ~/.yarn...
> Adding to $PATH...
> We've added the following to your /Users/kang/.zshrc
> If this isn't the profile of your current shell then please add the following to your correct profile:
   
export PATH="$HOME/.yarn/bin:$HOME/.config/yarn/global/node_modules/.bin:$PATH"

> Successfully installed Yarn 1.17.3! Please open another terminal where the `yarn` command will now be available.
➜  ~ 

```

#### 패스 설정

```bash
.profile, .bash_profile, .bashrc, .zshrc 등…

$ export PATH="$PATH:/opt/yarn-[version]/bin"
```

#### yarn 버전 확인

```bash
yarn --version
1.17.3
```



### VSCode 설치하기

다운로드는 [Visual Studio Code](https://code.visualstudio.com/) 에서 하실 수 있습니다.



### Create-React-App 설치 및 사용

#### 설치

```bash
yarn global add create-react-app
```

```bash
➜  ~ yarn global add create-react-app
yarn global v1.17.3
[1/4] 🔍  Resolving packages...
[2/4] 🚚  Fetching packages...
[3/4] 🔗  Linking dependencies...
[4/4] 🔨  Building fresh packages...

success Installed "create-react-app@3.1.0" with binaries:
      - create-react-app
✨  Done in 6.10s.
➜  ~ 
```

#### 사용

```bash
create-react-app hello-react
```

```bash
➜  ~ create-react-app hello-react 

Creating a new React app in /Users/kang/hello-react.

Installing packages. This might take a couple of minutes.
Installing react, react-dom, and react-scripts...

yarn add v1.17.3
[1/4] 🔍  Resolving packages...
[2/4] 🚚  Fetching packages...
[3/4] 🔗  Linking dependencies...
warning "react-scripts > @typescript-eslint/eslint-plugin@1.13.0" has incorrect peer dependency "eslint@^5.0.0".
warning "react-scripts > @typescript-eslint/parser@1.13.0" has incorrect peer dependency "eslint@^5.0.0".
warning "react-scripts > @typescript-eslint/eslint-plugin > tsutils@3.17.1" has unmet peer dependency "typescript@>=2.8.0 || >= 3.2.0-dev || >= 3.3.0-dev || >= 3.4.0-dev || >= 3.5.0-dev || >= 3.6.0-dev || >= 3.6.0-beta || >= 3.7.0-dev || >= 3.7.0-beta".
[4/4] 🔨  Building fresh packages...
success Saved lockfile.
success Saved 22 new dependencies.
info Direct dependencies
├─ react-dom@16.9.0
├─ react-scripts@3.1.0
└─ react@16.9.0
info All dependencies
├─ @babel/plugin-proposal-class-properties@7.5.5
├─ @babel/plugin-proposal-decorators@7.4.4
├─ @babel/plugin-transform-flow-strip-types@7.4.4
├─ @babel/plugin-transform-runtime@7.5.5
├─ babel-plugin-macros@2.6.1
├─ babel-plugin-named-asset-import@0.3.3
├─ babel-preset-react-app@9.0.1
├─ confusing-browser-globals@1.0.8
├─ core-js@3.1.4
├─ eslint-config-react-app@5.0.0
├─ fork-ts-checker-webpack-plugin@1.5.0
├─ gzip-size@5.1.1
├─ is-root@2.1.0
├─ open@6.4.0
├─ promise@8.0.3
├─ react-app-polyfill@1.0.2
├─ react-dev-utils@9.0.2
├─ react-dom@16.9.0
├─ react-error-overlay@6.0.0
├─ react-scripts@3.1.0
├─ react@16.9.0
└─ scheduler@0.15.0
✨  Done in 67.38s.

Initialized a git repository.

Success! Created hello-react at /Users/kang/hello-react
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

  cd hello-react
  yarn start

Happy hacking!
➜  ~ 
```



### React 실행

```bash
cd hello-react
yarn start
```

```bash
Compiled successfully!

You can now view hello-react in the browser.

  Local:            http://localhost:3000/
  On Your Network:  http://192.168.123.30:3000/

Note that the development build is not optimized.
To create a production build, use yarn build.
```



### 구동 화면

<img width="1680" alt="Screen Shot 2019-08-13 at 20 06 26" src="https://user-images.githubusercontent.com/46523571/62936918-f9688000-be05-11e9-8483-c06499800bf6.png">


<br/>
<br/>
<br/>

## 참고
---
VELOPERT 개발자님의 글을 보고 공부한 내용 기록입니다. 감사의 말씀드립니다. <br/>

VELOPERT 개발자님 : [https://velopert.com/3612](https://velopert.com/3612) <br/>
NVM Quick Start : [https://gist.github.com/falsy/8aa42ae311a9adb50e2ca7d8702c9af1](https://gist.github.com/falsy/8aa42ae311a9adb50e2ca7d8702c9af1) <br/>
yarn : [https://yarnpkg.com/en/docs/install#mac-stable](https://yarnpkg.com/en/docs/install#mac-stable)

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>



## 환경
---
> macOS Mojave 10.14.5, nvm 6.10.1, yarn 1.17.3, node 8.16.0, Visual Studio Code 1.36.1.