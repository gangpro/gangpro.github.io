---
layout: post
title: '[env] macOS에 파이썬 환경구축'
subtitle: 
categories: env
tags: env python
comments: true
date: 2019-03-23 10:00:17 +0900
lastmod: 2019-03-23 10:00:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

macOS에 파이썬 개발환경 구축하기<br/>

## 파이썬 개발환경 구축
---

### 파이썬 설치

터미널 실행

```bash
$ cd ~
# 최상위 폴더로 이동
```

### pip 최신버전 업데이트

```bash
$ pip install --upgrade pip

Collecting pip
  Downloading https://files.pythonhosted.org/packages/d7/41/34dd96bd33958e52cb4da2f1bf0818e396514fd4f4725a79199564cd0c20/pip-19.0.2-py2.py3-none-any.whl (1.4MB)
    100% |████████████████████████████████| 1.4MB 11.6MB/s
Installing collected packages: pip
  Found existing installation: pip 18.1
    Uninstalling pip-18.1:
      Successfully uninstalled pip-18.1
Successfully installed pip-19.0.2
```

현재 개발환경(base)은 python 3.7버전인데 <br>
나중에 사용할 tensorflow는 python 버전 3.6까지만 지원(2019.02 기준). <br>
따라서 python 3.6 버전을 가지고 가상환경을 하나 생성.<br>
여기서 'data_env'는 내가 원하는 가상환경 이름이므로 사용자가 원하는 이름으로 정할 수 있다.

```bash
$ conda create -n data_env python=3.6

Solving environment: done

==> WARNING: A newer version of conda exists. <==
  current version: 4.5.12
  latest version: 4.6.3

Please update conda by running

    $ conda update -n base -c defaults conda


  ​      
  ​      

*## Package Plan ##

  environment location: /anaconda3/envs/data_env

  added / updated specs:
    - python=3.6


​      

The following packages will be downloaded:

    package                    |            build
    ---------------------------|-----------------
    ca-certificates-2019.1.23  |                0         126 KB
    python-3.6.8               |       haf84260_0        20.5 MB
    certifi-2018.11.29         |           py36_0         146 KB
    setuptools-40.7.3          |           py36_0         619 KB
    pip-19.0.1                 |           py36_0         1.9 MB
    libedit-3.1.20181209       |       hb402a30_0         159 KB
    wheel-0.32.3               |           py36_0          35 KB
    ------------------------------------------------------------
                                            Total:        23.5 MB

The following NEW packages will be INSTALLED:

    ca-certificates: 2019.1.23-0
    certifi:         2018.11.29-py36_0
    libcxx:          4.0.1-hcfea43d_1
    libcxxabi:       4.0.1-hcfea43d_1
    libedit:         3.1.20181209-hb402a30_0
    libffi:          3.2.1-h475c297_4
    ncurses:         6.1-h0a44026_1
    openssl:         1.1.1a-h1de35cc_0
    pip:             19.0.1-py36_0
    python:          3.6.8-haf84260_0
    readline:        7.0-h1de35cc_5
    setuptools:      40.7.3-py36_0
    sqlite:          3.26.0-ha441bb4_0
    tk:              8.6.8-ha441bb4_0
    wheel:           0.32.3-py36_0
    xz:              5.2.4-h1de35cc_4
    zlib:            1.2.11-h1de35cc_3

Proceed ([y]/n)? y
  ↳ 설치를 원할 시 y 버튼을 클릭하고 엔터!

Downloading and Extracting Packages
ca-certificates-2019 | 126 KB    | ##################################### | 100%
python-3.6.8         | 20.5 MB   | ##################################### | 100%
certifi-2018.11.29   | 146 KB    | ##################################### | 100%
setuptools-40.7.3    | 619 KB    | ##################################### | 100%
pip-19.0.1           | 1.9 MB    | ##################################### | 100%
libedit-3.1.20181209 | 159 KB    | ##################################### | 100%
wheel-0.32.3         | 35 KB     | ##################################### | 100%
Preparing transaction: done
Verifying transaction: done
Executing transaction: done
#
# To activate this environment, use
#
#     $ conda activate data_env
#
# To deactivate an active environment, use
#
#     $ conda deactivate
```


### 현재 시스템 내 가상 환경 정보

```bash
$ conda info --envs

# conda environments:
#
base                  *  /anaconda3
data_env                 /anaconda3/envs/data_env
```

### 사용할 가상환경으로 전환
```bash
$ source activate data_env
```

### nb_conda 설치
주피터 노트북에서 가상환경을 선택할 수 있도록 nb_conda를 설치

```bash
$ conda install -c anaconda nb_conda

Solving environment: done

==> WARNING: A newer version of conda exists. <==
  current version: 4.5.12
  latest version: 4.6.3

Please update conda by running

    $ conda update -n base -c defaults conda

## Package Plan ##

  environment location: /anaconda3/envs/data_env

  added / updated specs:
    - nb_conda

The following packages will be downloaded:

    package                    |            build
    ---------------------------|-----------------
    ptyprocess-0.6.0           |           py36_0          23 KB  anaconda
    testpath-0.4.2             |           py36_0          91 KB  anaconda
    prompt_toolkit-2.0.8       |             py_0         222 KB  anaconda
    ipython_genutils-0.2.0     |   py36h241746c_0          39 KB  anaconda
    appnope-0.1.0              |   py36hf537a9a_0           8 KB  anaconda
    decorator-4.3.2            |           py36_0          17 KB  anaconda
    jedi-0.13.2                |           py36_0         228 KB  anaconda
    jupyter_core-4.4.0         |           py36_0          63 KB  anaconda
    terminado-0.8.1            |           py36_1          21 KB  anaconda
    ipython-7.2.0              |   py36h39e3cac_0         1.0 MB  anaconda
    pandoc-2.2.3.2             |                0        13.8 MB  anaconda
    nbconvert-5.3.1            |           py36_0         405 KB  anaconda
    wcwidth-0.1.7              |   py36h8c6ec74_0          25 KB  anaconda
    certifi-2018.11.29         |           py36_0         146 KB  anaconda
    notebook-5.7.4             |           py36_0         7.3 MB  anaconda
    pandocfilters-1.4.2        |           py36_1          13 KB  anaconda
    ipykernel-5.1.0            |   py36h39e3cac_0         156 KB  anaconda
    entrypoints-0.3            |           py36_0          12 KB  anaconda
    jsonschema-2.6.0           |   py36hb385e00_0          62 KB  anaconda
    parso-0.3.2                |           py36_0         119 KB  anaconda
    jupyter_client-5.2.4       |           py36_0         127 KB  anaconda
    ca-certificates-2019.1.23  |                0         126 KB  anaconda
    six-1.12.0                 |           py36_0          22 KB  anaconda
    jinja2-2.10                |           py36_0         184 KB  anaconda
    prometheus_client-0.5.0    |           py36_0          67 KB  anaconda
    mistune-0.8.4              |   py36h1de35cc_0          54 KB  anaconda
    pyzmq-17.1.2               |   py36h0a44026_2         433 KB  anaconda
    libsodium-1.0.16           |       h3efe00b_0         324 KB  anaconda
    pexpect-4.6.0              |           py36_0          77 KB  anaconda
    nb_conda-2.2.1             |           py36_0          33 KB  anaconda
    send2trash-1.5.0           |           py36_0          16 KB  anaconda
    openssl-1.1.1              |       h1de35cc_0         4.6 MB  anaconda
    zeromq-4.3.1               |       h0a44026_3         565 KB  anaconda
    tornado-5.1.1              |   py36h1de35cc_0         662 KB  anaconda
    nb_conda_kernels-2.2.0     |           py36_0          32 KB  anaconda
    backcall-0.1.0             |           py36_0          19 KB  anaconda
    python-dateutil-2.7.5      |           py36_0         275 KB  anaconda
    pygments-2.3.1             |           py36_0         1.4 MB  anaconda
    nbformat-4.4.0             |   py36h827af21_0         138 KB  anaconda
    markupsafe-1.1.0           |   py36h1de35cc_0          25 KB  anaconda
    pickleshare-0.7.5          |           py36_0          12 KB  anaconda
    webencodings-0.5.1         |           py36_1          19 KB  anaconda
    bleach-3.1.0               |           py36_0         224 KB  anaconda
    traitlets-4.3.2            |   py36h65bd3ce_0         131 KB  anaconda
    ------------------------------------------------------------
                                            Total:        33.1 MB

The following NEW packages will be INSTALLED:

    appnope:           0.1.0-py36hf537a9a_0  anaconda
    backcall:          0.1.0-py36_0          anaconda
    bleach:            3.1.0-py36_0          anaconda
    decorator:         4.3.2-py36_0          anaconda
    entrypoints:       0.3-py36_0            anaconda
    ipykernel:         5.1.0-py36h39e3cac_0  anaconda
    ipython:           7.2.0-py36h39e3cac_0  anaconda
    ipython_genutils:  0.2.0-py36h241746c_0  anaconda
    jedi:              0.13.2-py36_0         anaconda
    jinja2:            2.10-py36_0           anaconda
    jsonschema:        2.6.0-py36hb385e00_0  anaconda
    jupyter_client:    5.2.4-py36_0          anaconda
    jupyter_core:      4.4.0-py36_0          anaconda
    libsodium:         1.0.16-h3efe00b_0     anaconda
    markupsafe:        1.1.0-py36h1de35cc_0  anaconda
    mistune:           0.8.4-py36h1de35cc_0  anaconda
    nb_conda:          2.2.1-py36_0          anaconda
    nb_conda_kernels:  2.2.0-py36_0          anaconda
    nbconvert:         5.3.1-py36_0          anaconda
    nbformat:          4.4.0-py36h827af21_0  anaconda
    notebook:          5.7.4-py36_0          anaconda
    pandoc:            2.2.3.2-0             anaconda
    pandocfilters:     1.4.2-py36_1          anaconda
    parso:             0.3.2-py36_0          anaconda
    pexpect:           4.6.0-py36_0          anaconda
    pickleshare:       0.7.5-py36_0          anaconda
    prometheus_client: 0.5.0-py36_0          anaconda
    prompt_toolkit:    2.0.8-py_0            anaconda
    ptyprocess:        0.6.0-py36_0          anaconda
    pygments:          2.3.1-py36_0          anaconda
    python-dateutil:   2.7.5-py36_0          anaconda
    pyzmq:             17.1.2-py36h0a44026_2 anaconda
    send2trash:        1.5.0-py36_0          anaconda
    six:               1.12.0-py36_0         anaconda
    terminado:         0.8.1-py36_1          anaconda
    testpath:          0.4.2-py36_0          anaconda
    tornado:           5.1.1-py36h1de35cc_0  anaconda
    traitlets:         4.3.2-py36h65bd3ce_0  anaconda
    wcwidth:           0.1.7-py36h8c6ec74_0  anaconda
    webencodings:      0.5.1-py36_1          anaconda
    zeromq:            4.3.1-h0a44026_3      anaconda

The following packages will be UPDATED:

    ca-certificates:   2019.1.23-0                    --> 2019.1.23-0       anaconda
    certifi:           2018.11.29-py36_0              --> 2018.11.29-py36_0 anaconda
    openssl:           1.1.1a-h1de35cc_0              --> 1.1.1-h1de35cc_0  anaconda

Proceed ([y]/n)? y
  ↳ 설치를 원할 시 y 버튼을 클릭하고 엔터!

Downloading and Extracting Packages
ptyprocess-0.6.0     | 23 KB     | ##################################### | 100%
testpath-0.4.2       | 91 KB     | ##################################### | 100%
prompt_toolkit-2.0.8 | 222 KB    | ##################################### | 100%
ipython_genutils-0.2 | 39 KB     | ##################################### | 100%
appnope-0.1.0        | 8 KB      | ##################################### | 100%
decorator-4.3.2      | 17 KB     | ##################################### | 100%
jedi-0.13.2          | 228 KB    | ##################################### | 100%
jupyter_core-4.4.0   | 63 KB     | ##################################### | 100%
terminado-0.8.1      | 21 KB     | ##################################### | 100%
ipython-7.2.0        | 1.0 MB    | ##################################### | 100%
pandoc-2.2.3.2       | 13.8 MB   | ##################################### | 100%
nbconvert-5.3.1      | 405 KB    | ##################################### | 100%
wcwidth-0.1.7        | 25 KB     | ##################################### | 100%
certifi-2018.11.29   | 146 KB    | ##################################### | 100%
notebook-5.7.4       | 7.3 MB    | ##################################### | 100%
pandocfilters-1.4.2  | 13 KB     | ##################################### | 100%
ipykernel-5.1.0      | 156 KB    | ##################################### | 100%
entrypoints-0.3      | 12 KB     | ##################################### | 100%
jsonschema-2.6.0     | 62 KB     | ##################################### | 100%
parso-0.3.2          | 119 KB    | ##################################### | 100%
jupyter_client-5.2.4 | 127 KB    | ##################################### | 100%
ca-certificates-2019 | 126 KB    | ##################################### | 100%
six-1.12.0           | 22 KB     | ##################################### | 100%
jinja2-2.10          | 184 KB    | ##################################### | 100%
prometheus_client-0. | 67 KB     | ##################################### | 100%
mistune-0.8.4        | 54 KB     | ##################################### | 100%
pyzmq-17.1.2         | 433 KB    | ##################################### | 100%
libsodium-1.0.16     | 324 KB    | ##################################### | 100%
pexpect-4.6.0        | 77 KB     | ##################################### | 100%
nb_conda-2.2.1       | 33 KB     | ##################################### | 100%
send2trash-1.5.0     | 16 KB     | ##################################### | 100%
openssl-1.1.1        | 4.6 MB    | ##################################### | 100%
zeromq-4.3.1         | 565 KB    | ##################################### | 100%
tornado-5.1.1        | 662 KB    | ##################################### | 100%
nb_conda_kernels-2.2 | 32 KB     | ##################################### | 100%
backcall-0.1.0       | 19 KB     | ##################################### | 100%
python-dateutil-2.7. | 275 KB    | ##################################### | 100%
pygments-2.3.1       | 1.4 MB    | ##################################### | 100%
nbformat-4.4.0       | 138 KB    | ##################################### | 100%
markupsafe-1.1.0     | 25 KB     | ##################################### | 100%
pickleshare-0.7.5    | 12 KB     | ##################################### | 100%
webencodings-0.5.1   | 19 KB     | ##################################### | 100%
bleach-3.1.0         | 224 KB    | ##################################### | 100%
traitlets-4.3.2      | 131 KB    | ##################################### | 100%
Preparing transaction: done
Verifying transaction: done
Executing transaction: / + /anaconda3/envs/data_env/bin/python -m nb_conda_kernels.install --enable
Enabling nb_conda_kernels...
Status: enabled

\ + /anaconda3/envs/data_env/bin/jupyter-nbextension enable nb_conda --py --sys-prefix
Enabling notebook extension nb_conda/main...
      - Validating: OK
Enabling tree extension nb_conda/tree...
      - Validating: OK
+ /anaconda3/envs/data_env/bin/jupyter-serverextension enable nb_conda --py --sys-prefix
Enabling: nb_conda
- Writing config: /anaconda3/envs/data_env/etc/jupyter
    - Validating...
      nb_conda 2.2.1 OK

done
```

### 가상환경 등록
파이썬 3.6버전을 설치했기 때문에 수동으로 ipykernel에 우리의 가상환경을 등록해줘야 한다.

```bash
$ python -m ipykernel install --user --name data_env

Installed kernelspec data_env in /Users/kang/Library/Jupyter/kernels/data_env
  ↳ data_env : 내가 원하는 가상환경 이름이므로 사용자가 원하는 이름으로 정할 수 있다.
```

<br />
<br />
<br />

## jupyter notebook 첫 실행
---

```bash
$ (data_env)jupyter notebook

[I 10:19:22.618 NotebookApp] [nb_conda_kernels] enabled, 1 kernels found
[I 10:19:22.635 NotebookApp] Writing notebook server cookie secret to /Users/kang/Library/Jupyter/runtime/notebook_cookie_secret
[I 10:19:22.879 NotebookApp] [nb_conda] enabled
[I 10:19:22.879 NotebookApp] Serving notebooks from local directory: /Users/kang
[I 10:19:22.879 NotebookApp] The Jupyter Notebook is running at:
[I 10:19:22.879 NotebookApp] http://localhost:8888/?token=13c5f69c269d21bbd6b859c7f9a90eff199278b039b6cbc0
[I 10:19:22.879 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 10:19:22.885 NotebookApp]

    To access the notebook, open this file in a browser:
        file:///Users/kang/Library/Jupyter/runtime/nbserver-3767-open.html
    Or copy and paste one of these URLs:
        http://localhost:8888/?token=13c5f69c269d21bbd6b859c7f9a90eff199278b039b6cbc0
^C[I 10:19:33.215 NotebookApp] interrupted
Serving notebooks from local directory: /Users/kang
0 active kernels
The Jupyter Notebook is running at:
http://localhost:8888/?token=13c5f69c269d21bbd6b859c7f9a90eff199278b039b6cbc0


↳ 주피터 노트북 실행
↳ control + 엔터 빵! 하면 아래와 같음

Shutdown this notebook server (y/[n])? y
  ↳ jupyter notebook 종료를 원하면 y
  
[C 10:19:37.667 NotebookApp] Shutdown confirmed
[I 10:19:37.668 NotebookApp] Shutting down 0 kernels
```

### jupyter notebook 환경설정 파일 생성
```bash
$ (data_env) jupyter notebook --generate-config

Writing default config to: /Users/kang/.jupyter/jupyter_notebook_config.py
```

### jupyter notebook 환경설정 파일 수정(메모장으로 파일 열기)
```
  Users/kang/.jupyter/jupyter_notebook_config.py
   ↳ .jupyter 파일이 보이지 않을 시 숨김파일 보기 버튼을 클릭( shift + command + , )
```

### 환경설정 파일 중간쯤? 아래와 같은 내용 수정

```
$ (기존)
## The directory to use for notebooks and kernels.
#c.NotebookApp.notebook_dir = ''

$ (수정)
## The directory to use for notebooks and kernels.
c.NotebookApp.notebook_dir = '/Users/kang/Documents/Python/python_ML'
  ↳ c.앞에 # 주석 제거
  ↳ jupyter notebook이 실행할 경로 설정
  ↳ 설정한 경로에 가서 새폴더 생성
```

### 모든 환경 설정이 끝났으므로 jupyter notebook 사용이 가능하다.
터미널 실행 - jupyter notebook 실행

```bash
$ source activate data_env
$ (data_env) jupyter notebook
[I 22:30:23.935 NotebookApp] [nb_conda_kernels] enabled, 1 kernels found
[I 22:30:24.520 NotebookApp] [nb_conda] enabled
[I 22:30:24.520 NotebookApp] Serving notebooks from local directory: /Users/kang/Documents/Python/python_ML
[I 22:30:24.521 NotebookApp] The Jupyter Notebook is running at:
[I 22:30:24.521 NotebookApp] http://localhost:8888/?token=45d9dca46413f11cfa585b82fa6ea873a9f4b85a175aa98d
[I 22:30:24.521 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 22:30:24.527 NotebookApp] 
    
    To access the notebook, open this file in a browser:
        file:///Users/kang/Library/Jupyter/runtime/nbserver-6335-open.html
    Or copy and paste one of these URLs:
        http://localhost:8888/?token=45d9dca46413f11cfa585b82fa6ea873a9f4b85a175aa98d
```


<br/>
<br/>
<br/>

## 아나콘다3 설치
---

**설명** <br>
파이썬 홈페이지에가서 파이썬을 설치해도 되지만 아나콘다를 설치하면 가상환경을 설정 할 수도 있고 다수의 유용한 파이썬 패키지를 쓸 수 있어서 바로 아나콘다를 설치한다.


**아나콘다 다운로드 사이트** <br> 
[https://www.anaconda.com/distribution/](https://www.anaconda.com/distribution/)

**아나콘다 3.7버전 다운로드** <br>
Python Anaconda 3.7버전 2018.12 ( Anaconda3-2018.12-MacOSX-x86_64.pkg )

**설치시 어려움은 없다. Yes맨이 되면 설치 완료.** <br>

**공식 영문 홈페이지** <br>
[https://docs.anaconda.com/anaconda/install/mac-os/](https://docs.anaconda.com/anaconda/install/mac-os/)




## 환경
---
> macOS Mojave 10.14.3.
> python 3.6.8, 
> Python Anaconda 3.7, 2018.12.
