---
title: 環境構築（VirtualBox）（受講者向け）
layout: default
---

{% include header.html %}

{% raw %}

# 環境構築（受講者（VirtualBox）向け）
※こちらのページは演習受講者（VirtualBox）用です。

## webgame環境の設定
※VirtualBoxの`rsl-webdb`環境を起動したうえで、以下のコマンドを実行してください。

### スタートアップシェルスクリプトの実行
```bash
$ ~/bin/startup.sh
```

### パッケージのアップグレード
```bash
$ sudo apt update
$ sudo apt upgrade
```

## rsl-webgame仮想環境の構築
```bash
$ cd
$ python3.12 -m venv ~/venv/rsl-webgame
```

## rsl-webgame仮想環境のアクティベート
```bash
$ source ~/venv/rsl-webgame/bin/activate
(rsl-webgame) $
# プロンプトが(rsl-webgame) ...$となればOK
```
以降、プロンプトが`(rsl-webgame) $`となっている行はrsl-webgame仮想環境上で実行することを表します。

## rsl-webgame仮想環境へのrsl_base仮想環境の複製
```bash
(rsl-webgame) $ pip install --upgrade pip
(rsl-webgame) $ pip install -r ~/venv/rsl_base_requirements.txt
```

## 各種パッケージのインストール
```bash
(rsl-webgame) $ pip install django==5.1
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

## Visual Studio Codeの実行
```bash
$ code
```

{% endraw %}
