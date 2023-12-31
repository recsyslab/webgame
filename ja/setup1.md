---
title: 環境構築（一般向け）
layout: default
---

{% include header.html %}

{% raw %}

# 環境構築（一般向け）
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
> wsl --export Ubuntu-22.04 wsl2\Ubuntu-22.04_base.tar
```
4. 下記を実行し、`Ubuntu-22.04`環境を`webgame`という名前でインポートする。
```bash
> wsl --import webgame wsl2\webgame\ wsl2\Ubuntu-22.04_base.tar
```
5. 下記を実行し、`webgame`環境が正しく構築されていることを確認する。
```bash
> wsl -l --verbose
```

#### 参考
1. [WSL 上で同一ディストリビューションの環境を複数インストール･管理する - Qiita](https://qiita.com/souyakuchan/items/9f95043cf9c4eda2e1cc)
2. [WSL2の環境に同じ種類のディストリビューションを複製する \| 株式会社ピース｜PEACE Inc.](https://www.4peace.co.jp/blog_tech/621/)

### ユーザパスワードの変更
※ここでは、パスワードを`ryukoku`とする。
```bash
> wsl -d webgame -u root
# passwd rsl
# exit
```

#### 参考
1. [WSL::Ubuntuのユーザパスワード変更 - fujii's公開ノート](https://scrapbox.io/fujii-memo/WSL::Ubuntu%E3%81%AE%E3%83%A6%E3%83%BC%E3%82%B6%E3%83%91%E3%82%B9%E3%83%AF%E3%83%BC%E3%83%89%E5%A4%89%E6%9B%B4)

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

### `make`のインストール
```bash
sudo apt install make
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

#### 日本語の文字化けへの対応
```bash
$ sudo apt install language-pack-ja
$ sudo apt install fonts-ipafont
$ sudo apt install fonts-ipaexfont
$ fc-cache -fv
```

下記コマンドでGoogle Chromeが起動すればOK。
```bash
$ google-chrome
```

#### 参考
1. [【WSL2, Debian, Ubuntu】システムテスト等でChromeを立ち上げた際にWSL2側の問題で文字化けしてしまう問題を解決する方法 - LEFログ：学習記録ノート](https://lef237.hatenablog.com/entry/2022/12/05/163550)

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
(rsl-webgame) $ pip --version
(rsl-webgame) $ pip install --upgrade pip
(rsl-webgame) $ pip --version
```

## 各種パッケージのインストール
※本チュートリアルでは、下記のパッケージすべてを使用しているというわけではありません。適宜、必要なもののみインストールして頂いて結構です。

### 基本
```bash
(rsl-webgame) $ pip install ipython
```

### DB関連
```bash
(rsl-webgame) $ pip install psycopg2-binary
```

### スクレイピング関連
```bash
(rsl-webgame) $ pip install requests
```

### Django関連
```bash
(rsl-webgame) $ pip install django
(rsl-webgame) $ pip install django-leaflet
(rsl-webgame) $ pip install djangorestframework-gis # RESTful APIに必要
(rsl-webgame) $ pip install django-filter # RESTful APIに必要
(rsl-webgame) $ pip install markdown # RESTful APIに必要
(rsl-webgame) $ pip install django-bootstrap5
(rsl-webgame) $ pip install django-allauth
(rsl-webgame) $ pip install django-cleanup
```

### インストール済みパッケージの確認
```bash
(rsl-webgame) $ pip freeze
```

## rsl-webgame仮想環境のディアクティベート
```bash
(rsl-webgame) $ deactivate
# プロンプトが元に戻ればOK
```

## Visual Studio Codeのインストール
```bash
$ cd
$ sudo apt install libsecret-1-0
$ wget 'https://code.visualstudio.com/sha/download?build=stable&os=linux-deb-x64' -O code_latest_amd64.deb
$ sudo dpkg -i code_latest_amd64.deb
$ rm -f code_latest_amd64.deb
```

## Visual Studio Codeの実行
```bash
$ code
```

### webgame環境のエクスポート
1. **Windowsマーク**を右クリックし、**ターミナル (管理者)** をクリックする。
2. ターミナルで下記を実行し、`Ubuntu-22.04`が存在することを確認する。
```bash
> wsl -l --verbose
```
3. 下記を実行し、`webgame`環境をエクスポートする。
```bash
> wsl --export webgame wsl2\webgame.tar
```

#### 参考
1. [WSLで構築した環境をエクスポートして共有する - Qiita](https://qiita.com/tkc_tsuchiya/items/abfd040aebc8d0332eab)

{% endraw %}
