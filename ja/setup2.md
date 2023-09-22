---
title: 環境構築（受講者向け）
layout: default
---

{% include header.html %}

{% raw %}

# 環境構築（受講者向け）
※こちらのページは演習受講者用です。演習で配布される`webgame_base.tar`を使用してください。

## webgame環境の構築

### 授業用のwebgame環境のインポート
```bash
> wsl --import webgame .\wsl2\webgame\ .\wsl2\webgame_base.tar
> wsl -l --verbose
```

### ユーザパスワードの変更
※ここでは、ユーザ名を`rsl`、パスワードを`ryukoku`とする。
```bash
> wsl -d webgame -u root
# passwd rsl
# exit
```

#### 参考
1. [WSL::Ubuntuのユーザパスワード変更 - fujii's公開ノート](https://scrapbox.io/fujii-memo/WSL::Ubuntu%E3%81%AE%E3%83%A6%E3%83%BC%E3%82%B6%E3%83%91%E3%82%B9%E3%83%AF%E3%83%BC%E3%83%89%E5%A4%89%E6%9B%B4)

### webgame環境の起動
下記コマンドで`webgame`環境を起動する。
※ここでは、ユーザ名を`rsl`、パスワードを`ryukoku`とする。
```bash
> wsl -d webgame -u rsl
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

## rsl-webgame仮想環境のディアクティベート
```bash
(rsl-webgame) $ deactivate
# プロンプトが元に戻ればOK
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
