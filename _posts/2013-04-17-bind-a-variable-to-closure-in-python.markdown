---
layout: post
status: publish
published: true
title: pythonのクロージャに変数を束縛する方法
author: ReSTARTR
author_login: admin
author_email: yoshida.masaki+blog@gmail.com
wordpress_id: 1378
wordpress_url: http://blog.restartr.com/?p=1378
date: '2013-04-17 22:19:14 +0900'
date_gmt: '2013-04-17 13:19:14 +0900'
categories:
- python
tags:
- python
- lambda
- closure
- functools
comments: []
---
ハマったので。

```python
a = 2
double = lambda x: x*a
double(4)  # 8 (=4*2)
double(10) # 20 (=10*3)
a = 3
double(4)  # 12 # WTF?
double(10) # 30
```

doubleというクロージャ内の変数aを、クロージャ宣言時のaで束縛したいのです。

対応は２つ。

1. lambdaのデフォルト引数で束縛する
2. functools.partialで束縛する

### 1. lambdaのデフォルト引数で束縛する

参考：[closures - Python lambda's binding to local values - Stack Overflow](http://stackoverflow.com/questions/10452770/python-lambdas-binding-to-local-values)

```python
a = 2
double = lambda x, y=a: x*y
double(4)  # 8 (=4*2)
double(10) # 20 (=10*3)
a = 3
double(4)  # 12 (=4*2)
double(10) # 30 (=10*2)
```

### 2. functools.partialで束縛する

やってることは1と同じなのですが、一応動くよねということで。

```python
from functools import partial
a = 2
double = partial(lambda x, y=None: x*y, y=a)
double(4) # 8 (=8*2)
double(10) # 30 (=10*2)
a = 3
double(4)  # 12 (=4*2)
double(10) # 30 (=10*2)
```

### そもそも変数上書きしなければ良いんじゃない？

普段は変数の上書きは基本的にやりません。なので変数の束縛とかあまり意識してませんでした。

今回、プロジェクトオイラーを解くにあたって、素数ジェネレータをつくろうとした結果、ハマったのでした。

```python
from itertools import ifilter, count
def gen_primes():
    it = count(2)  # [2, 3, 4, ...]
    while True:
        v = it.next()
        yield v
        it = ifilter(lambda x, y=v: x % y > 0, it)
        # 当初は以下のようにしていた
        # これだと次のループ時のifilter内でvの値が変わってしまう
        # it = ifilter(lambda x: x % v > 0, it)
for v in gen_primes():
    print v
    if v > 100:
        break
```

### 余談1: functools.partialの使いどころ
ちょくちょく[functools.partial](http://docs.python.jp/2.7/library/functools.html#functools.partial)使ってましたが、そんなの使わなくてもlambdaで事足りますね。今更気づきました...

```python
mul = lambda a, b: a * b
mul(3,2)  # 6
# lambda
double = lambda a, b=2: mul(a,b)
double(3)  # 6
# functools.partial
import functools
double = functools.partial(mul, 2)
double(3)  # 6
```

こうなると、functools.partialの使いどころが難しいですね。
戻り値がpartialオブジェクトなので、あとで引数とかが参照できることくらいでしょうか...

```python
>>> f = functools.partial(lambda a, b=0:a+b, b=0)
>>> f
<functools.partial object at 0x1092b2ec0>
>>> f.args
()
>>> f.keywords
{'b': 0}
>>> f.func
<function <lambda> at 0x1092eab18>
>>> f.args = (1,)  # 引数を後から上書きはできない
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: readonly attribute
```
