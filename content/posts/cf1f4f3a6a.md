+++
title = """XGBoostを使って機械学習を始めてみよう"""
date = 2025-03-12T08:25:26.000Z
expiryDate = 2025-05-09T11:06:19.115Z
tags = ["vtj","devops"]
+++
XGBoostとは？
==========

XGBoost（eXtreme Gradient Boosting）は、勾配ブースティング（Gradient Boosting）に基づいた機械学習アルゴリズムの一つであり、高速かつ高精度な予測を実現する手法です。

機械学習の分野では、複数のシンプルなモデル（それぞれの精度は低いが、組み合わせることで改善できるモデル）を統合して、個々のモデル単体よりも高い予測精度を得る「アンサンブル学習」が重要な役割を果たします。アンサンブル学習には、バギング（Bagging）、ブースティング（Boosting）、スタッキング（Stacking）といった手法があります。その中でも、ブースティングは、モデルを逐次的に学習させ、前のモデルの誤差を修正することで予測精度を向上させる手法です。

XGBoostが採用する「勾配ブースティング」は、ブースティングの一種で、決定木（Decision Tree）をベースとしたアンサンブル学習手法です。複数のシンプルな決定木を組み合わせ、前のモデルの誤差を補正しながら学習することで、単純な決定木よりも高い精度を達成し、非線形な関係を学習することができます。また、XGBoostはこの手法をさらに改良し、高速化や正則化の導入によって、より実用的なモデルを構築できるようにしていると言われています。

XGBoostと他のアルゴリズムとの比較
--------------------

XGBoostは、ランダムフォレストや通常の決定木と比べて、以下のような利点を持ちます。

アルゴリズム

特徴

過学習の抑制

並列計算

欠損値処理

決定木

単純なルールベースのモデル

なし

なし

なし

ランダムフォレスト

複数の決定木を組み合わせて安定した予測を行う

あり

あり

なし

XGBoost

勾配ブースティングを最適化し、高速かつ高精度

あり（正則化あり）

あり

あり

XGBoostは、ランダムフォレストのように複数の決定木を組み合わせるアンサンブル学習の手法を採用しつつ、勾配ブースティングによって学習の精度を向上させています。さらに、並列計算や欠損値処理の自動化といった最適化が施されているため、実用上のパフォーマンスが高いのが特徴です。

このように、XGBoostは他の決定木ベースのアルゴリズムと比べて多くのメリットを持っており、多くの機械学習コンペティションや実務で広く利用されています。

XGBoostのインストール
==============

XGBoostはPython環境で簡単にインストールできます。パッケージの管理には、`requirements.txt` を使用しています。

`requirements.txt` ファイルは、以下の内容を記述して作成します。

xgboost
scikit-learn

その後、以下のコマンドを実行してインストールします。

pip install \-r requirements.txt

この方法を使用することで、他の依存パッケージとともに一括管理でき、環境の再現性が向上します。

### 動作確認

インストールが正しく行われたか確認するために、`main.py`を作成して次のコードを記述して実行します。

import xgboost as xgb
print(xgb.\_\_version\_\_)

エラーが発生せず、XGBoostのバージョンが表示されれば、正常にインストールされています。

$ python main.py
2.1.4

以上で、XGBoostのインストールが完了しました。次のセクションでは、XGBoostの基本的な使い方について解説します。

XGBoostの基本的な使い方
===============

このセクションでは、XGBoostを使用した基本的な機械学習モデルの作成方法を解説します。具体的には、データの準備、前処理、モデルの作成、訓練、評価の手順を紹介します。

データの準備
------

XGBoostを用いる前に、データセットを準備する必要があります。ここでは、`scikit-learn` の `datasets` モジュールを使ってサンプルデータを取得します。

**scikit-learnとは？**

`scikit-learn` は、Pythonで機械学習を行うためのライブラリで、多くの機械学習アルゴリズムやデータセットの処理ツールを提供しています。データの前処理、モデルの学習・評価、ハイパーパラメータの調整など、幅広い用途に対応しています。

from sklearn.datasets import load\_breast\_cancer
from sklearn.model\_selection import train\_test\_split
from xgboost import XGBClassifier

\# データのロード
data = load\_breast\_cancer()
X, y = data.data, data.target

\# 訓練データとテストデータに分割
X\_train, X\_test, y\_train, y\_test = train\_test\_split(X, y, test\_size=0.2, random\_state=42)

データの前処理
-------

XGBoostは欠損値を自動的に処理できますが、データのスケール調整や特徴量エンジニアリングを行うことで、より高い精度を得られる場合があります。

以下のように、データの標準化を行うことが一般的です（ただし、決定木ベースのモデルではスケーリングは必須ではありません）。

from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
X\_train = scaler.fit\_transform(X\_train)
X\_test = scaler.transform(X\_test)

データの標準化とは、データの数値の大きさ（スケール）を揃えることです。例えば、ある特徴量が0〜1の範囲にあるのに対し、別の特徴量が1,000〜10,000の範囲にあると、学習において影響の大きい特徴量と小さい特徴量が生じてしまいます。標準化を行うことで、すべての特徴量が同じ基準で扱われ、学習が安定しやすくなります。

XGBoostのような決定木ベースのアルゴリズムでは、データのスケールに左右されにくいため、標準化は必須ではありません。しかし、線形回帰やニューラルネットワークなどのモデルでは、特徴量のスケールが異なると学習がうまくいかないことがあります。そのため、これらのモデルとXGBoostを組み合わせる場合や、データの分布を均一にしたい場合には、標準化を行うことで学習の安定性が向上します。

XGBoostモデルの作成
-------------

XGBoostには、分類用の `XGBClassifier` と回帰用の `XGBRegressor` が用意されています。ここでは、分類タスクの例として `XGBClassifier` を使用します。

\# モデルの定義
model = XGBClassifier(
    n\_estimators=100,
    learning\_rate=0.1,
    max\_depth=3,
    random\_state=42
)

モデルの訓練
------

作成したモデルを訓練データで学習させます。

\# モデルの学習
model.fit(X\_train, y\_train)

モデルの評価
------

学習済みのモデルをテストデータで評価し、精度を確認します。

from sklearn.metrics import accuracy\_score

\# 予測
y\_pred = model.predict(X\_test)

\# 精度評価
accuracy = accuracy\_score(y\_test, y\_pred)
print(f"モデルの精度: {accuracy:.4f}")

交差検証を用いた評価
----------

より信頼性の高い評価を行うために、交差検証を活用することもできます。

import numpy as np
from sklearn.model\_selection import cross\_val\_score

\# 交差検証を実行
cv\_scores = cross\_val\_score(model, X, y, cv=5, scoring='accuracy')
print(f"交差検証の平均精度: {np.mean(cv\_scores):.4f}")

このように、XGBoostは比較的シンプルなコードで利用でき、高い精度のモデルを構築することが可能です。次のステップでは、ハイパーパラメータの調整を行い、さらなる精度向上を目指します。

コード全体

from sklearn.datasets import load\_breast\_cancer
from sklearn.model\_selection import train\_test\_split
from xgboost import XGBClassifier
import numpy as np
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import accuracy\_score
from sklearn.model\_selection import cross\_val\_score

\# データのロード
data = load\_breast\_cancer()
X, y = data.data, data.target

\# 訓練データとテストデータに分割
X\_train, X\_test, y\_train, y\_test = train\_test\_split(X, y, test\_size=0.2, random\_state=42)

scaler = StandardScaler()
X\_train = scaler.fit\_transform(X\_train)
X\_test = scaler.transform(X\_test)

model = XGBClassifier(
    n\_estimators=100,
    learning\_rate=0.1,
    max\_depth=3,
    random\_state=42
)

\# モデルの学習
model.fit(X\_train, y\_train)

\# 予測
y\_pred = model.predict(X\_test)

\# 精度評価
accuracy = accuracy\_score(y\_test, y\_pred)
print(f"モデルの精度: {accuracy:.4f}")

\# 交差検証を実行
cv\_scores = cross\_val\_score(model, X, y, cv=5, scoring='accuracy')
print(f"交差検証の平均精度: {np.mean(cv\_scores):.4f}")

実行結果

モデルの精度: 0.9561
交差検証の平均精度: 0.9666

ハイパーパラメータのチューニング
----------------

XGBoostの真価を発揮するためには、適切なハイパーパラメータの調整が不可欠です。最適な設定を行うことで、モデルの精度を向上させ、より汎化性能の高い学習が可能になります。

*   learning\_rate（学習率）: 低くすると安定した学習、高くすると高速だが過学習のリスク。
*   max\_depth（木の深さ）: 深いと複雑なモデルになりやすく、過学習に注意。
*   n\_estimators（ブースティング回数）: 増やすことで精度が向上するが、過学習しやすくなる。
*   subsample（サンプリング率）: 1.0未満にすることで過学習を抑え、汎化性能を向上。
*   colsample\_bytree（特徴量のサブサンプリング）: 特徴量の一部を使用することで、モデルのロバスト性を高める。

これらのパラメーターも上手く活用して最適なモデル作成にチャレンジしてみてください。

XGBoostをもっと使いこなすために
-------------------

XGBoostをより効果的に活用するためのテクニックとして、特徴量の重要度の可視化、SHAPを用いたモデル解釈、GPUを活用した高速化について説明します。

### 特徴量の重要度の可視化

XGBoostでは、学習済みモデルを用いて各特徴量の重要度を可視化できます。これにより、どの特徴量がモデルの予測に大きく寄与しているかを理解し、特徴選択やモデル改善の参考にすることが可能です。

#### 特徴量の重要度の取得と可視化

Pythonでは、以下のコードを使用して特徴量の重要度を可視化できます。ただし、ターミナル上で実行したとしても可視化されないため、ColaboratoryやJupyter Notebookなどグラフィカルな表示に対応したツールを活用する必要があります。

import xgboost as xgb
import matplotlib.pyplot as plt

\# XGBoostモデルの作成
model = xgb.XGBClassifier()
model.fit(X\_train, y\_train)

\# 重要度のプロット
xgb.plot\_importance(model)
plt.savefig('feature\_importance.png')
plt.show()

`plot_importance` 関数を使うと、各特徴量の重要度が棒グラフで表示されます。重要度の低い特徴量を削減することで、モデルの解釈性を向上させ、計算コストを削減できる可能性があります。

今回は、`plt.savefig()`を使って画像ファイルとして出力していますが、Jupyter Notebookなどのツールを使うと画面上にそのまま表示することもできます。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/v/virtualtech/20250312/20250312172527.png)

* * *

### SHAPを使った解釈可能性の向上

特徴量の重要度だけでは、各特徴がどのように予測結果に影響を与えているかは明確ではありません。そこで、SHAP（SHapley Additive exPlanations）を用いることで、各特徴量の影響をより詳細に分析できます。

#### SHAPのインストールと基本的な使い方

SHAPライブラリを使用するには、まずインストールが必要です。

pip install shap

次に、SHAPを使ってXGBoostモデルの説明を行う方法を示します。

import shap

\# SHAP値の計算
explainer = shap.Explainer(model)
shap\_values = explainer(X\_train)

\# 要約プロットの表示
shap.summary\_plot(shap\_values, X\_train)
plt.savefig('shap\_summary\_plot.png')
plt.show()

![](https://cdn-ak.f.st-hatena.com/images/fotolife/v/virtualtech/20250312/20250312172531.png)

`summary_plot` を使うことで、各特徴量がどの程度の影響を持つか、またその影響がポジティブかネガティブかを直感的に理解できます。

* * *

### GPUを活用した高速化

XGBoostは、CPUだけでなくGPUを使用して計算を高速化することが可能です。特に大規模データセットを扱う際には、GPUを活用することでモデルの学習時間を大幅に短縮できます。

#### GPUを有効にする方法

XGBoostでGPUを使用するには、`tree_method` を `gpu_hist` に設定します。

model = xgb.XGBClassifier(tree\_method='gpu\_hist')
model.fit(X\_train, y\_train)

これにより、決定木の構築がGPU上で行われ、学習速度が向上します。

また、`nvidia-smi` コマンドを使ってGPUの使用状況を確認しながら、計算負荷を最適化することも可能です。

nvidia-smi

これらの手法を活用することで、XGBoostモデルの性能を向上させるだけでなく、より深くモデルの振る舞いを理解することにつながります。

おわりに
====

機械学習というものを初めて知って触っていた頃からすると随分と便利なライブラリなどが増えてきて、気軽に触れやすくなりました。

奥が深い機械学習の世界の本の入り口に過ぎない話ですが、ぜひ興味を持つきっかけにしていただいて、より深い世界に入っていってもらえると嬉しいです。

[[source]](https://devops-blog.virtualtech.jp/entry/20250312/1741767926)
