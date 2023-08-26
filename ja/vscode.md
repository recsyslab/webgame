---
title: Visual Studio Codeのインストール
layout: default
---

{% include header.html %}

{% raw %}

# Visual Studio Codeのインストール
```bash
$ cd
$ sudo apt --fix-broken install
$ sudo apt install libsecret-1-0
$ wget 'https://code.visualstudio.com/sha/download?build=stable&os=linux-deb-x64' -O code_latest_amd64.deb
$ sudo dpkg -i code_latest_amd64.deb
$ rm -f code_latest_amd64.deb
```

# Visual Studio Codeの実行
```bash
$ code
```

{% endraw %}
