---
title: 環境構築
layout: default
---

{% include header.html %}

{% raw %}

# 環境構築
※本チュートリアルでは、WSL2環境に既にUbuntu-22.04環境がインストールされていることを前提としています。

## webgame環境の構築

### 既存のUbuntu環境の複製
1. **Windowsマーク**を右クリックし、**ターミナル (管理者)** をクリックする。
2. ターミナルで下記を実行し、`Ubuntu-22.04`が存在することを確認する。
```bash
> wsl -l --verbose
```
3. 下記を実行し、`Ubuntu-22.04`環境をエクスポートする。
```bash
> mkdir wsl2\
> wsl --export Ubuntu-22.04 wsl2\Ubuntu-22.04.tar
```
4. 下記を実行し、`Ubuntu-22.04`環境を`webgame`という名前でインポートする。
```bash
> wsl --import webgame wsl2\webgame\ wsl2\Ubuntu-22.04.tar
```
5. 下記を実行し、`webgame`環境が正しく構築されていることを確認する。
```bash
> wsl -l --verbose
```

### ユーザパスワードの変更
※ここでは、パスワードを`ryukoku`とする。
```bash
> wsl -d webgame -u root
# passwd rsl
# exit
```

### webgame環境の起動
下記コマンドで`webgame`環境を起動する。【ユーザ名】には元の`Ubuntu-22.04`環境で作成したユーザ名を入れること。
```bash
> wsl -d webgame -u 【ユーザ名】
```

### webgame環境の解除
`webgame`環境が不要になれば、下記コマンドで`webgame`環境を解除する。
```bash
> wsl --unregister webgame
> wsl --list --verbose
```

## webgame環境の設定
※`webgame`環境を起動したうえで、以下のコマンドを実行してください。

### パッケージのアップグレード
```bash
$ sudo apt update
$ sudo apt upgrade
```

### ディレクトリの準備（不要？）
```bash
$ mkdir ~/bin/
$ mkdir ~/src/
$ mkdir ~/opt/
```

### 設定ファイルの準備（不要？）
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

### `make`、`git`のインストール
```bash
sudo apt install make
sudo apt install git（不要？）
```

### `sysv-rc-conf`のインストール
```bash
$ mkdir ~/src/
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

### Google Chromeのインストール
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

### PostgreSQL+PostGISのインストール
```bash
$ sudo apt install postgresql
$ sudo apt install postgis
```

### PostgreSQLの動作確認とバージョンの確認
```bash
$ sudo -u postgres psql
postgres=# SELECT version();
# インストールしたバージョンが表示されればOK。
postgres=# \q
# '\'はキーボードの右下のバックスラッシュ「ろ」を押す（右上の'￥'ではない）
```

### PostgreSQLのパスワードの設定
```bash
$ sudo -u postgres psql
```

下記コマンドで、PostgreSQLに`postgres`ユーザとしてログインするためのパスワードを指定する。
※ここでは、パスワードを`ryukoku`とする。
```pgsql
postgres=# \password
postgres=# \q
```

※ここでは、PostgreSQLのバージョンが`14.x`であることを前提とする。異なるバージョンの場合、`14`の部分は自身のバージョンに合わせること。
```bash
$ sudo less /etc/postgresql/14/main/pg_hba.conf
$ sudo nano /etc/postgresql/14/main/pg_hba.conf
```
`pg_hba.conf`の下記3箇所について`peer`を`md5`に書き換える。

`pg_hba.conf`
```
...（略）...
# Database administrative login by Unix domain socket
local all postgres md5
...（略）...
# "local" is for Unix domain socket connections only
local all all md5
...（略）...
# Allow replication connections from localhost, by a user with the
# replication privilege.
local replication all md5
...（略）...
```

```bash
$ sudo service postgresql restart
$ sudo -u postgres psql
# 設定したパスワードを入力してログインできることを確認する。
```

## 各種パッケージのインストール
※本チュートリアルでは、下記のパッケージすべてを使用しているというわけではありません。適宜、必要なもののみインストールして頂いて結構です。

### 基本
```bash
$ sudo apt install tree
```

### データ分析関連（不要？）
```bash
$ sudo apt install libbz2-dev # pandasのインポートに必要
$ sudo apt install python3-tk # matplotlib.show()で画像を表示する際に必要
$ sudo apt install libffi-dev # scikit-learnのインポートに必要
```

### GDAL関連（不要？）
```bash
$ sudo apt install build-essential # GDALのインストールに必要
$ sudo apt install libgdal-dev	# GDALのインストールに必要
$ sudo apt install python3-gdal	# GDALのインストールに必要
$ sudo apt install gdal-bin # GDALのインストールに必要
# ...（5分程度）...
```

### NLP関連（不要？）
```bash
$ sudo apt install mecab libmecab-dev mecab-ipadic mecab-ipadic-utf8
```

## Pythonのバージョンの確認
```bash
$ python3 --version
```

## rsl-webgame仮想環境の構築
```bash
$ sudo apt install python3-venv
$ mkdir ~/venv/
$ cd ~/venv/
$ python3 -m venv rsl-webgame
```

## rsl-webgame仮想環境のアクティベート
```bash
$ source ~/venv/rsl-webgame/bin/activate
# プロンプトが(rsl-webgame) ...$となればOK
```

以降、プロンプトが`(rsl-webgame) $`となっている行はrsl-webgame仮想環境上で実行することを表します。

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
