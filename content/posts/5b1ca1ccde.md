+++
title = """helmとhelm templateについての気づき 第二回 yqを使ったマニフェストの分割"""
date = 2025-04-28T01:36:20.000Z
expiryDate = 2025-07-07T09:08:20.822Z
tags = ["vtj","devops"]
+++
さていよいよ本題に入るのですが、この作成されたマニフェストを見てみると、複数のYAML形式で書かれたマニフェストが合体しているようなファイルになっています。

$ helm template mywordpress ./wordpress -f ./values.yaml > mywp-manifests.yaml 

ものすごく長いファイルになっており、可読性は良くないですよね。 幸いにもこの方法で生成されたマニフェストは`---`区切りでそれぞれ記述されていますので、この辺りで切り離せば複数のファイルに分割できそうです。 早速ChatGPTやGoogle Geminiにその方法を聞いてみました。 おおよそどちらも同じような回答だったので、以下はChatGPTを使った場合で説明します。

まず、ChatGPTは二つの方法を提示しました。 一つはcsplitを使う方法。Windowsの場合はWSLなどで環境を整える必要はありますが、LinuxやmacOSでは標準で使えるコマンドが用意されています。 もう一つはyqというツールを使う方法があるようです。jqはJsonに使うのに対し、yqはYAMLに使うみたいなものです。

yqは使ったことがなかったので、yqを使って試そうとしました。 ところがうまくいきません。

$ pipx install yq
$ yq eval-all 'splitDoc' mywp-manifests.yaml  \\
  | awk 'BEGIN{n=0;}{if(/^---/){n++} print > sprintf("manifest\_%03d.yaml", n)}'
yq: error: argument files: can't open 'splitDoc': \[Errno 2\] No such file or directory: 'splitDoc'

そこで再度ChatGPTに聞いてみると、これを実行するにはyqのバージョン4が必要であることがわかりました。Pythonのyqモジュールはバージョン3の実装のものが提供されるらしく、そちらが必要だと。

*   [https://github.com/mikefarah/yq/](https://github.com/mikefarah/yq/)

macOSを使っているので、次のようにバージョン3のyqをアンインストールして、バージョン4のyqをインストールしなおしました。

$ pipx install yq
$ brew install yq

今度はコマンドが通ったのですが何かがおかしいです。一つのファイルしかつくられないではありませんか。

$ yq eval 'splitDoc' mywp-manifests.yaml  \\
  | awk 'BEGIN{n=0;}{if(/^---/){n++} print > sprintf("manifest\_%03d.yaml", n)}'

「なんで？」と聞いたところ次のような回答をもらいました。

なるほど、詳しく説明してくれてありがとう！macOSで yq eval 'splitDoc' を使っても1つのファイルにしかならないということですね。
実は、ここでちょっとした 理解のズレ がありました 🙇‍♂️
yq eval 'splitDoc' は 標準出力上で各ドキュメントを --- で区切って表示するだけ で、複数ファイルに自動で分割するわけではありません。

スクリプトを使う方法と、ワンライナーで済ます方法が提示されたため、今回はワンライナーの方を実行して、ことなきを得ました。

$ yq eval '... comments=""' mywp-manifests.yaml  | \\
awk '/^---/ {n++} {print > sprintf("manifest\_%03d.yaml", n)}'

長くなったので、次回に続きます。

第3回に続く

[devops-blog.virtualtech.jp](https://devops-blog.virtualtech.jp/entry/20250430/1745979859)

[[source]](https://devops-blog.virtualtech.jp/entry/20250428/1745804180)
