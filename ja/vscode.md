---
title: Visual Studio Codeの設定
layout: default
---

{% include header.html %}

{% raw %}

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


{% endraw %}
