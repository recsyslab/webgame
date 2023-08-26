---
title: プロジェクトの作成
layout: default
---

{% include header.html %}

{% raw %}

# 環境構築

## パッケージのアップグレード
```bash
$ sudo apt update
$ sudo apt upgrade
```

## ディレクトリの準備
```bash
$ mkdir ~/bin/
$ mkdir ~/src/
$ mkdir ~/opt/
```

## 設定ファイルの準備
```bash
$ cp ~/.profile ~/.profile-org
$ echo -e '\n\n#### #### Add below. #### ####' >> ~/.profile
$ diff ~/.profile-org ~/.profile
$ cp ~/.bashrc ~/.bashrc-org
$ echo -e '\n\n#### #### Add below. #### ####' >> ~/.bashrc
$ diff ~/.bashrc-org ~/.bashrc
$ sudo cp /etc/apt/sources.list /etc/apt/sources.list-org
$ sudo sh -c 'echo "\n\n#### #### Add below. #### ####" >> /etc/apt/sources.list'
$ diff /etc/apt/sources.list-org /etc/apt/sources.list
```

## `make`、`git`のインストール
```bash
sudo apt install make
sudo apt install git
```

## `sysv-rc-conf`のインストール
```bash
$ cd ~/src/
$ wget http://archive.ubuntu.com/ubuntu/pool/universe/s/sysv-rc-conf/sysv-rc-conf_0.99.orig.tar.gz
$ tar zxvf sysv-rc-conf_0.99.orig.tar.gz
$ cd sysv-rc-conf-0.99
$ sudo make
$ sudo make install
$ sudo apt install libcurses-ui-perl libterm-readkey-perl libcurses-perl
$ sudo sysv-rc-conf
# [h]キーでバージョンを確認できる．
$ cd ../
$ rm -f sysv-rc-conf_0.99.orig.tar.gz
```

## Google Chromeのインストール
```bash
$ cd
$ sudo sh -c 'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'
$ sudo wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
$ sudo apt update
$ sudo apt install google-chrome-stable
```

下記コマンドでGoogle Chromeが起動すればOK。
```bash
$ google-chrome
```

## PostgreSQL+PostGISのインストール
```bash
$ sudo apt install postgresql
$ sudo apt install postgis
# ...（3分程度）...
```

## PostgreSQLの動作確認とバージョンの確認
```bash
$ sudo -u postgres psql
postgres=# SELECT version();
# インストールしたバージョンが表示されればOK。
postgres=# \q
# '\'はキーボードの右下のバックスラッシュ「ろ」を押す（右上の'￥'ではない）
```

## 各種パッケージのインストール
※本チュートリアルでは、下記のパッケージすべてを使用しているというわけではありません。適宜、必要なもののみインストールして頂いて結構です。

### 基本
```bash
$ sudo apt install tree
```

### データ分析関連
```bash
$ sudo apt install libbz2-dev # pandasのインポートに必要
$ sudo apt install python3-tk # matplotlib.show()で画像を表示する際に必要
$ sudo apt install libffi-dev # scikit-learnのインポートに必要
```

### GDAL関連
```bash
$ sudo apt install build-essential # GDALのインストールに必要
$ sudo apt install libgdal-dev	# GDALのインストールに必要
$ sudo apt install python3-gdal	# GDALのインストールに必要
$ sudo apt install gdal-bin # GDALのインストールに必要
# ...（5分程度）...
```

### NLP関連
```bash
$ sudo apt install mecab libmecab-dev mecab-ipadic mecab-ipadic-utf8
```

## Pythonのバージョンの確認
```bash
$ python3 --version
```

## rsl-django仮想環境の構築
```bash
$ sudo apt install python3-venv
$ mkdir ~/venv/
$ cd ~/venv/
$ python3 -m venv rsl-django
```

## rsl-django仮想環境のアクティベート
```bash
$ source ~/venv/rsl-django/bin/activate
# プロンプトが(rsl-django) ...$となればOK
```

以降、プロンプトが`(rsl-django) $`となっている行はrsl-django仮想環境上で実行することを表します。

## pipのアップグレード
```bash
(rsl-django) $ pip --version
(rsl-django) $ pip install --upgrade pip
(rsl-django) $ pip --version
```

## 各種パッケージのインストール
※本チュートリアルでは、下記のパッケージすべてを使用しているというわけではありません。適宜、必要なもののみインストールして頂いて結構です。

### 基本
```bash
(rsl-django) $ pip install ipython
(rsl-django) $ pip install tqdm
```

### データ分析関連
```bash
(rsl-django) $ pip install numpy
(rsl-django) $ pip install scipy
(rsl-django) $ pip install matplotlib
(rsl-django) $ pip install pandas
(rsl-django) $ pip install scikit-learn
```

### DB関連
```bash
(rsl-django) $ pip install psycopg2-binary
```

### NLP関連
```bash
(rsl-django) $ pip install mecab-python3
(rsl-django) $ pip install ginza
(rsl-django) $ pip install ja-ginza
(rsl-django) $ pip install spacy
```

### スクレイピング関連
```bash
(rsl-django) $ pip install beautifulsoup4
(rsl-django) $ pip install requests
```

### Django関連
```bash
(rsl-django) $ pip install django
(rsl-django) $ pip install django-leaflet
(rsl-django) $ export CPLUS_INCLUDE_PATH=/usr/include/gdal
(rsl-django) $ export C_INCLUDE_PATH=/usr/include/gdal
(rsl-django) $ gdalinfo --version
GDAL 3.4.1, released 2021/12/27
(rsl-django) $ pip install gdal==3.4.1 # libgdal-devのバージョンに合わせる # GeoDjangoに必要
# 「ERROR: Could not build wheels for gdal, which is required to install pyproject.toml-based projects」というエラーが出るので要調査
# ...（1分程度）...
(rsl-django) $ pip install djangorestframework-gis # RESTful APIに必要
(rsl-django) $ pip install django-filter # RESTful APIに必要
(rsl-django) $ pip install markdown # RESTful APIに必要
(rsl-django) $ pip install django-bootstrap5
(rsl-django) $ pip install django-allauth
(rsl-django) $ pip install django-cleanup
```

### インストール済みパッケージの確認
```bash
(rsl-django) $ pip freeze
```

## rsl-django仮想環境のディアクティベート
```bash
(rsl-django) $ deactivate
# プロンプトが元に戻ればOK
```

{% endraw %}
