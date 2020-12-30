---
title: "Python Tips"
date: 2020-12-30
categories: ["comp"]
tags: ["python"]
---

PythonのPandasで漸化式の計算を作ろうとしたら，いろいろはまって勉強になったので，メモ。

## 変数が定義されているか否かの確認

- `locals()`: ローカルスコープで宣言されているシンボル一覧(変数等)を辞書化して返す
- `globals()`: グローバルスコープで宣言されているシンボル一覧(変数等)を辞書化して返す

```python
'hoge' in locals() # 変数`hoge`がローカルスコープにあればTrue   
'hoge' in globals() # 変数`hoge`がグローバルスコープにあればTrue   
```

参考URL: [Python Tips：変数が定義されたかどうかを調べたい](https://www.lifewithpython.com/2015/05/python-check-variable-existence.html)

## Pandasで前列の情報を参照してデータ更新

### 新規列のデータを，事前に計算済みのデータに基づいて計算する場合

[`shift()`](pandas.DataFrame.shift)を使うと簡単

### 新規列のデータを前列のデータから逐次的に計算する場合

Pandasで漸化式\[x_{n+1}=x_n\]のように，ある列のデータに基づいて次の列のデータを作るのは少し面倒。

C言語のように，[DataFrame.iat](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.iat.html)で位置指定して`for`文で要素を書き換えていくのが嫌で，良い方法がないか調べたらdecoratorを使う方法が
[Stackoverflow](https://stackoverflow.com/questions/16698415/reference-previous-row-when-iterating-through-dataframe/38155257#38155257)に紹介されていた。


```python
import pandas as pd

df = pd.DataFrame({'a':[0,1,2], 'b':[0,10,20]})

new_col = 'c'

def apply_func_decorator(func):
    prev_row = {}
    def wrapper(curr_row, **kwargs):
        val = func(curr_row, prev_row)
        prev_row.update(curr_row)
        prev_row[new_col] = val
        return val
    return wrapper

@apply_func_decorator
def running_total(curr_row, prev_row):
    return curr_row['a'] + curr_row['b'] + prev_row.get('c', 0)

df[new_col] = df.apply(running_total, axis=1)

print(df)
# Output will be:
#    a   b   c
# 0  0   0   0
# 1  1  10  11
# 2  2  20  33
```

1行前の値を参照したいだけなのに，使い慣れないデコレータの勉強からすることになってしまった。。。

**参考URL**

- [Reference previous row when iterating through dataframe](https://stackoverflow.com/questions/16698415/reference-previous-row-when-iterating-through-dataframe/38155257#38155257)
- [Pythonのデコレータを理解するための12Step](https://qiita.com/_rdtr/items/d3bc1a8d4b7eb375c368)