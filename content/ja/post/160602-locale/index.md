---
title: "MacでRやpython実行時のロケール設定"
date: 2016-06-02
mathjax: false
categories: ["comp"]
tags: ["R"," mac"]
---

RやpythonをMacで実行するとlocaleまわりのエラーが出ることがある。

Rの場合
```R
$ Rscript test.r
 起動準備中です -  警告メッセージ: 
1: Setting LC_COLLATE failed, using "C" 
2: Setting LC_TIME failed, using "C" 
3: Setting LC_MESSAGES failed, using "C" 
4: Setting LC_MONETARY failed, using "C
```

<!--more-->

pythonの場合
```python
Traceback (most recent call last):
  ...
  File "/usr/local/var/pyenv/versions/anaconda3-2.5.0/lib/python3.5/site-packages/matplotlib/__init__.py", line 1000, in _open_file_or_url
    encoding = locale.getdefaultlocale()[1]
  File "/usr/local/var/pyenv/versions/anaconda3-2.5.0/lib/python3.5/locale.py", line 559, in getdefaultlocale
    return _parse_localename(localename)
  File "/usr/local/var/pyenv/versions/anaconda3-2.5.0/lib/python3.5/locale.py", line 487, in _parse_localename
    raise ValueError('unknown locale: %s' % localename)
ValueError: unknown locale: UTF-8
```

localeの設定をすれば解消。
```
$ cat ~/.bash_profile
...
export LC_ALL=ja_JP.UTF-8
```
