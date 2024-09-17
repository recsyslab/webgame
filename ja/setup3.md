---
title: 環境構築（VirtualBox）（受講者向け）
layout: default
---

{% include header.html %}

{% raw %}

# 環境構築（受講者（VirtualBox）向け）
※こちらのページは演習受講者（VirtualBox）用です。

## webgame環境の設定
※VirtualBoxの`rsl-webgame`環境を起動したうえで、以下のコマンドを実行してください。

### パッケージのアップグレード
```bash
$ sudo apt update
$ sudo apt upgrade
```

## rsl-webgame仮想環境のアクティベート
```bash
$ source ~/venv/rsl-webgame/bin/activate
# プロンプトが(rsl-webgame) ...$となればOK
```

以降、プロンプトが`(rsl-webgame) $`となっている行はrsl-webgame仮想環境上で実行することを表します。

### インストール済みパッケージの確認
```bash
(rsl-webgame) $ pip freeze
```

### パッケージのインストール
```bash
(rsl-webgame) $ sudo apt install tree
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
