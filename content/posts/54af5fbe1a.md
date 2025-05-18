+++
title = """GitHub ActionsでGitHub Scriptを使ってみよう"""
date = 2025-03-26T05:03:47.000Z
expiryDate = 2025-05-21T19:05:54.372Z
tags = ["vtj","devops"]
+++
はじめに
====

GitHub Actionsを活用してCI/CDや各種自動化を行っていると、「特定の条件で処理を分岐したい」「IssueやPull Request（PR）に対して自動でコメントを投稿したい」といった、ちょっとした処理を柔軟に実行したくなる場面があります。

こうした細かなニーズに対応するために便利なのが GitHub Script です。GitHub Scriptを使えば、JavaScriptで簡潔にロジックを書き、GitHub APIを通じてさまざまな操作を実行できます。ワークフローの中に直接スクリプトを埋め込めるため、外部スクリプトの管理や複雑な設定も不要です。

本記事では、GitHub Scriptの基本的な仕組みから、具体的なユースケース、活用時のベストプラクティスまでを丁寧に解説します。「GitHub Actionsの中で、もう少し自由に処理を書きたい」と感じたことのある方は、ぜひ参考にしてみてください。

GitHub Scriptとは
===============

GitHub Scriptは、GitHubが公式に提供しているActionsのひとつで、JavaScriptを使ってGitHub APIやワークフローに手軽にアクセスできるのが特徴です。 actions/github-scriptという名前で公開されており、ワークフロー内で直接JavaScriptコードを記述して実行できます。

たとえば、「特定の条件でIssueにコメントしたい」「Pull Requestに応じてラベルを変更したい」といった処理を、非常に短いコードで柔軟に実現できます。通常であれば、curlや外部スクリプトを使って実装するような処理も、1つのステップで完結します。

ライブラリのインストールや外部ファイルの準備は不要で、実行環境にはあらかじめGitHubのAPIクライアント（Octokit）やワークフローの文脈情報（contextオブジェクト）が組み込まれているため、最小限の記述で自動化が可能です。

このように、GitHub Scriptは「ちょっとした自動化」を素早く、シンプルに実装したい場面で非常に有効なツールです。

GitHub Scriptを使わずに実現しようとする場合
============================

GitHub ActionsでPull Requestにラベルを付けたり、Issueにコメントを残したりする処理は、curl や bash を使ってGitHub APIを直接操作する方法でも実現可能です。ただし、この方法には思わぬ手間が発生します。

たとえば、APIを呼び出すための認証トークンやヘッダーの設定、レスポンスの解析に使う jq、条件分岐やループ処理を書く bash など、複数のツールを組み合わせる必要があります。また、GitHubのイベント情報をシェルスクリプトで扱うには、環境変数の参照やJSONの加工が必要になり、実装が煩雑になりがちです。

こうした「ちょっとした処理」こそ、GitHub Scriptが得意とする分野です。次のセクションでは、その基本的な使い方を紹介します。

GitHub Scriptの基本的な使い方
=====================

GitHub Scriptを使うには、GitHubが公式に提供している [`actions/github-script`](https://github.com/actions/github-script) を利用します。このActionを使うことで、JavaScript（Node.js）を記述し、GitHub APIやワークフローの情報に直接アクセスできます。

通常、GitHub APIを扱うには `curl` や `jq` などのツールを組み合わせる必要がありますが、GitHub Scriptを使えば、より簡潔に柔軟な処理を記述できます。

基本構成
----

\- name: GitHub Scriptを使った処理
  uses: actions/github-script@v7
  with:
    script: |
      // JavaScriptコード

スクリプト内では、以下の2つのオブジェクトがあらかじめ利用可能です。

*   `github`: GitHub REST APIクライアント（Octokitベース）
*   `context`: 実行中のイベントやリポジトリ、Pull Request番号などの情報を含むオブジェクト

Pull Requestにコメントを投稿する
======================

以下は、Pull Request が作成されたときに、自動でコメントを投稿する例です。

name: PR Comment

on:
  pull\_request:
    types: \[opened\]

jobs:
  comment:
    runs-on: ubuntu-latest
    steps:
      \- name: Send Comment
        uses: actions/github-script@v7
        with:
          github-token: ${{ secrets.GITHUB\_TOKEN }}
          script: |
            await github.rest.issues.createComment({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue\_number: context.issue.number,
              body: 'このコメントは、GitHub Scriptを使って送信されました'
            });

このように、GitHub Scriptでは少ないコードでAPIを操作でき、ワークフロー内に自然に組み込めるのが特徴です。

では、実際に動かしてみましょう。GitHub上で[新しいリポジトリ](https://docs.github.com/ja/repositories/creating-and-managing-repositories/creating-a-new-repository)を作成します。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/v/virtualtech/20250326/20250326140349.png)

「Actions」タブを選択し、Simple workflowの「Configure」をクリックします。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/v/virtualtech/20250326/20250326140352.png)

画面が表示されたら、ファイル名をわかりやすい名前に変更します。今回は、`sample.yml`としています。次に、先ほど紹介したコードをコピー&ペーストして貼り付けます。最後に、「Commit changes...」を押して、変更をコミットします。コミット名を編集できるウィンドウが表示された場合は、変更せずにそのまま進めます。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/v/virtualtech/20250326/20250326140356.png)

コミットができたら「Actions」のセクションに戻り、先ほど追加したワークフローが認識されていることを確認します。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/v/virtualtech/20250326/20250326140359.png)

今回はプルリクエストが作成された時を条件に実行されるようにしているので、プルリクエストを作成してみます。

on:
  pull\_request:
    types: \[opened\]

GitHubの画面上からテキストファイルを追加します。`Add file > Create new file`を選択して、`hello.txt`ファイルを追加します。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/v/virtualtech/20250326/20250326140403.png)

コミットメッセージを入力する画面が表示されたら、今回は`new branch`の方を選択してからコミットします。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/v/virtualtech/20250326/20250326140407.png)

プルリクエストの画面が表示されますが、そのまま作成します。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/v/virtualtech/20250326/20250326140411.png)

プルリクエストを作成したらGitHub Actionsが実行されます。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/v/virtualtech/20250326/20250326140415.png)

実行が終わるとコメントにメッセージが追加されます。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/v/virtualtech/20250326/20250326140419.png)

おわりに
====

GitHub Scriptは、GitHub Actionsにおけるちょっとした処理に対して、シンプルかつ柔軟に対応できるツールの1つです。JavaScriptで記述でき、githubオブジェクトやcontextオブジェクトを通じてGitHub APIへ直感的にアクセスできるため、複雑な外部スクリプトやツールチェインを用意せずとも、目的の処理を実装できます。ぜひ、活用してみてください。

[[source]](https://devops-blog.virtualtech.jp/entry/20250326/1742965427)
