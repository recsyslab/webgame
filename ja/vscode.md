---
title: Visual Studio Codeの設定
layout: default
---

{% include header.html %}

{% raw %}

# Visual Studio Codeの設定

## 初期設定

1. Visual Studio Codeを起動する。
```bash
(rsl-webgame) $ code
```
   1. **File > Open Folder**メニューを開く。
      1. `/home/rsl/webgame/`を選択し、**Open**ボタンをクリックする。
      2. 「Do you trust the authors of the files in this folder?」というメッセージが表示されたら、**Yes, I trust the authors**ボタンをクリックする。
   2. 左メニューから**Extensions**アイコンをクリックする。
      1. `Python`を選択し、`Install`ボタンをクリックする。
   3. **View > Command Palette**メニューを開く。
      1. `Python: select interpreter`をクリックし、`Python 3.**.** ('rsl-webgame')`を選択する。
   4. 左メニューから**Run and Debug**アイコンをクリックする。
      1. **create a launch.json file**をクリックする。
      2. `Python`を選択する。
      3. `Django`を選択する。
   5. 上メニューから**Start Debugging**ボタン（再生ボタン）をクリックする。
   6. ブラウザで`http://localhost:8000/`にアクセスし、「The install worked successfully! Congratulations!」と表示されればOK。

## データベースの設定
1. 左メニューから**Run and Debug**アイコンをクリックする。
   1. **Open 'launch.json'**アイコン（歯車アイコン）をクリックする。
   2. `launch.json`に下記のように`DB_USER`と`DB_PASSWORD`に関する設定を追加する。

リスト1: `lauch.json`
```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Django",
            "type": "python",
            "request": "launch",
            "program": "${workspaceFolder}/manage.py",
            "args": [
                "runserver"
            ],
            "django": true,
            "justMyCode": true,
            "env": {
                "DB_USER": "rsl",
                "DB_PASSWORD": "【パスワード】",
            }
        }
    ]
}
```


#### 参考
1. [Python and Django tutorial in Visual Studio Code](https://code.visualstudio.com/docs/python/tutorial-django)
2. [Django を VSCode で 開発するまでの手順 - Qiita](https://qiita.com/soh506/items/12a5df2d19f1c2c792fe)

{% endraw %}
