---
title: "Pythonの辞書型ソートしてみた！"
emoji: "📖"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Python入門","Python勉強","学習","メモ"]
published: True
---
# 背景
今回では、プログラミング講師をする中で、講義資料を作成するために、Python勉強をしたことをメモとして残したいので、書くことになりました。

# 辞書型とは
辞書型とは、キーと値のペアを使ってデータを保存するためのデータ型です。
リストの場合では、次のように定義します。
```Python:リストの宣言する方法
li = list() #空のリストを宣言する時
numbers = [1, 2, 3]
```
辞書型の場合では、次のように定義します。
```Python:辞書型の宣言する方法
d = dict() #空の辞書型を宣言する時
people = {"suzuki":1, "yamada":2,"mizuki":3}
```
`people`変数に保存されていることを図で表すと、次のようになります。
![people変数の中身](/images/list.png)*people変数の中身*
上の図のように、ラベルと値が保存されていると考えても大丈夫だと思いました。
また、勉強する途中で、リストと辞書型の違いは何だろうと思い、違いに関して勉強をしました。
# リストと辞書型の違い
次はリストと辞書型の違いになります。 
- **宣言する方法が違う**
上の、宣言する方法を見ると、わかるようになると思います。
- **中身の参照する方法が違う**
**リスト**では、**インデックス**を利用して、値を求めます。(インデックスとは、データの位置を意味します。)
しかし、**辞書型**では、**キー**を入れることで値を求めることができます。例えば、次のようになります。
```Python:各自の値を求める方法
people = {"suzuki":1,"yamada":2,"mizuki":3}
numbers = [1, 2, 3]

print(people["mizuki"]) #出力結果:3
print(numbers[2])       #出力結果:3
```
他にも、違いが存在すると思いますが、紹介したいものとしては、この2つであると思いまいした。
# 辞書型のソート
今回勉強しながら、紹介したいものとしては、Python3.7以降では、挿入順序を保持するようになった点です。多分、2018年6月くらいから保持するようになったということですね！
リストで、ソートをする時には``sort()``メソッドを使いましたが、辞書型でこれを使うと、エラーが発生しました。よって、他の方法で辞書型をソートしていきたいと思いました。また、ソートする時にはキーに基づくソートと値に基づくソートが存在すると思いました。

- **キーに基づくソート**
```Python:キーに基づくソート
My_dict = {'yamada':3, 'asuka':4,'nomura':1}

sorted_keys = sorted(My_dict)
sorted_dict_by_key = {k: My_dict[k] for k in sorted_keys}
print(sorted_dict_by_key) #出力結果:{'asuka': 4, 'nomura': 1, 'yamada': 3}
```
先にキーを順番でソートして、`sorted_keys`に保存します。そして、`sorted_keys`に入っている順番を揃っているキーを利用して、順番でキーに該当する値をペアで改めて`sorted_dict_by_key`に保存をします。
もし、出力結果を逆順にしたい時には、`sorted_keys = sorted(My_dict)`のところを`sorted_keys = sorted(My_dict, reverse=True)`と変えたら大丈夫だと思います。

- **値に基づくソート**
```Python:値に基づくソート
My_dict = {'yamada':3, 'asuka':4,'nomura':1}

sorted_items_by_value = sorted(My_dict.items(), key=lambda item: item[1])
sorted_dict_by_value = {k: v for k, v in sorted_items_by_value}
print(sorted_dict_by_value) #出力結果:{'nomura': 1, 'yamada': 3, 'asuka': 4}
```
`sorted_items_by_value = sorted(My_dict.items(), key=lambda item: item[1])
`を理解すれば、値に基づくソートだけではなく、キーに基づくソートもできることに気づきました。
`My_dict.items()`をすることで、`[("yamada":3), ("asuka",4),("nomura",1)]`が返されることがわかります。また、`lambda item: item[1]`であるので、値の部分を取り出して、値に基づいて並び変えて`[("nomura",1)("yamada":3), ("asuka",4)]`が`sorted_items_by_value`に格納されることがわかりました。(つまり、`lambda item: item[1]`のところを`lambda item: item[0]`で変えることで、キーに基づくソートができることに気付きました)

# 参考
今回では、次のようなブログなどから参考になり、勉強になりました。
https://note.nkmk.me/python-key-sort-sorted-max-min/
https://note.nkmk.me/python-lambda-usage/
https://qiita.com/nagataaaas/items/531b1fc5ce42a791c7df
https://laid-back-scientist.com/python-sort
https://qiita.com/n10432/items/e0315979286ea9121d57
https://blogboard.io/blog/knowledge/python-sorted-lambda/