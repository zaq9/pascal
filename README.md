# pascal
Pascal's triangle

#はじめに
[パスカルの三角形とフラクタル](https://qiita.com/YuukiToriyama/items/d5a3a4635640a860f0a7) を見て、絵文字こんな使い方もあるんだとちょっと感動し、Colaboratory（Python）上で試してみました。

[ブラウザ（colab）上で動かしてみる場合はこちら](https://github.com/zaq9/pascal/blob/master/pascal.ipynb)

![image.png](https://qiita-image-store.s3.amazonaws.com/0/351020/28fd503b-6804-ea43-e90a-04a71dc8a866.png)



##パスカルの三角形とは

>>パスカルの三角形（パスカルのさんかくけい、英: Pascal's triangle）は、二項展開における係数を三角形状に並べたものである ( [wikipedia](https://ja.wikipedia.org/wiki/%E3%83%91%E3%82%B9%E3%82%AB%E3%83%AB%E3%81%AE%E4%B8%89%E8%A7%92%E5%BD%A2) ）

>>この三角形の作り方は単純なルールに基づいている。まず最上段に 1 を配置する。それより下の行はその位置の右上の数と左上の数の和を配置する。例えば、5段目の左から2番目には、左上の 1 と右上の 3 の合計である 4 が入る。このようにして数を並べると、上から n 段目、左から k 番目の数は、二項係数 

>> ![image.png](https://qiita-image-store.s3.amazonaws.com/0/351020/56cb1534-59c8-6ade-9426-0f0c359f2953.png)

>>に等しい。これは、パスカルによって示された以下の式に基づいている。
負でない整数 n ≥ k に対して 

>> ![image.png](https://qiita-image-store.s3.amazonaws.com/0/351020/2e91192e-3725-93cd-8932-ff7d49aa4314.png)
>> が成り立つ

##作成したソースコード

再帰を使うとパスカルの三角形の漸化式はスムーズに。

```python
def c(n,k): 
  return 1 if(k<=0 or n<=k) else c(n-1, k-1) + c(n-1, k)
```

まずはリスト形式で１０段分表示。

```python
for i in range(10):
  print([c(i,j) for j in range(i+1)])

'''OUTPUT:
[1]
[1, 1]
[1, 2, 1]
[1, 3, 3, 1]
[1, 4, 6, 4, 1]
[1, 5, 10, 10, 5, 1]
[1, 6, 15, 20, 15, 6, 1]
[1, 7, 21, 35, 35, 21, 7, 1]
[1, 8, 28, 56, 70, 56, 28, 8, 1]
[1, 9, 36, 84, 126, 126, 84, 36, 9, 1]
'''
```

少しだけアレンジし、剰余利用＋文字列でリスト展開表示

```python

def c(n,k): 
  return 1 if(k<=0 or n<=k) else ( c(n-1, k-1) + c(n-1, k) ) %4

for i in range(10):
  print(' '.join([ str( c(i,j)) for j in range(i+1) ] ))


'''OUTPUT:
1
1 1
1 2 1
1 3 3 1
1 0 2 0 1
1 1 2 2 1 1
1 2 3 0 3 2 1
1 3 1 3 3 1 3 1
1 0 0 0 2 0 0 0 1
1 1 0 0 2 2 0 0 1 1
'''
```

絵文字を使ったアレンジ（２０段まで）

```python


e = ['🍈','🍎','🍇','🍊']
for i in range(20):
  print(' '.join([ str(e[c(i,j)%4]) for j in range(i+1)]))

```

##メモ化
同じ計算が繰り返される再帰計算の場合、「メモ化」を行うと超高速化できます。
方法は一行追加するのみです。

参考:[１行追加するだけの超お手軽な高速化その１　メモ化](https://qiita.com/alchemist/items/c75174c41b0bcd31ecc6)


```python

from functools import lru_cache

@lru_cache(maxsize=1000)
def c(n,k): 
  return 1 if(k<=0 or n<=k) else ( c(n-1, k-1) + c(n-1, k) ) %4


e = ['🍈','🍎','🍇','🍊']
for i in range(50):
  print(' '.join([ str(e[c(i,j)%4]) for j in range(i+1)]))
```

![image.png](https://qiita-image-store.s3.amazonaws.com/0/351020/4a0ebb0a-7cc4-344a-a8ae-eb75eb7ce298.png)

