---
title: "決定木アルゴリズムをIris-datasetに対してPythonで実行した"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Python", "Iris"]
published: False
---

# 背景
今回では、大学での課題として、機械学習ライブラリを利用して、決定木アルゴリズムをIris-datasetに対して実行結果をレポートにすることになりました！

# ライブラリ準備
```python:library.py
import japanize_matplotlib  #matplotlibで日本語を表示するためのライブラリ。
import matplotlib.pyplot as plt #データのグラフィック化（プロット）のために使用する
from sklearn.datasets import load_iris #irisデータを読み込むために使用する
from sklearn.tree import DecisionTreeClassifier #決定木モデル
from sklearn.model_selection import train_test_split  #train用データ、test用データをわけるために使用する
from sklearn.metrics import classification_report, accuracy_score #訓練したものを評価するために使用する
from sklearn import tree　#決定木モデルに関連する機能を使うために使用する
```

[](
/home/park/project/Python/study/algorism/AI/report1.py
)