+++
title = """作業タスクをSQLiteで管理する"""
date = 2025-05-14T01:56:20.000Z
expiryDate = 2025-06-16T16:08:15.035Z
tags = ["vtj","devops"]
+++
今回も小ネタです。

日々の作業タスクの管理って難しいですよね。 インターネットでタスク管理をするためのツールを探せば色々なサービスが提供されていて便利そうだなと思って最初のうちは使うわけですが、気がついたらそれを使うのが億劫になったり使っていたのを忘れてしまったりめんどくさくなったりで、気がついたら使うのをやめてしまう。 結局、その繰り返しになったりします。

ではもう少し簡単なものをということで探しては試すのですが、直感的でないと習慣に根付かず、あっという間に使わなくなってしまったりと大体、いつもそんな感じです。

運用方針
----

結局のところ、難しすぎたり面倒すぎると気がついたときには使わなくなっているくらい普段の私はものぐさなので、とにかく最低限の機能だけを使ってタスクの管理をする方針にします。 SQLはこの業界に入っていればどこかしらで使うので、慣れておくと便利というメリットはいくらかあると思います。

ざっくりとした使い方
----------

まずはSQLiteのインストールです。OSによって色々な方法があるので、何らかの方法でインストールしてください。 macOSでHomebrewを使っている場合は次でインストールできます。 ちなみに現在、sqliteでもsqlite3でもバージョン3のSQLiteがインストールされます。

% brew install sqlite

次に、わかりやすい名前でデータベースファイルを作成します。

% sqlite3 todo.sqlite3

タスクを管理するためのテーブルをわかりやすい名前で作成します。`.mode line`はテーブルの表示の仕方を設定するオプションのうちの一つです。`.headers on`と`.mode list`が他のSQLサーバーにおける出力に似ています。

sqlite> CREATE TABLE todo(id integer, date text, summary text, priority integer); 
sqlite> .headers on
sqlite> .mode list

タスクを追加します。 インストール済みのSQLite 3は日本語も問題なく使えるようなので、サマリーは日本語で書いておきます。

タスクの終了可否みたいな列もあるとよりそれっぽいですが、列が増えれば増えると私は面倒くさがるような気がするので、終わったら消すくらいの感じで使おうと思います（UPDATEで終了みたいな文字を冒頭に追加して、その日の定時にDELETEする方針でもいいかもしれませんね）。

sqlite> INSERT INTO todo VALUES(1, 20250512, "次の定例に向けた調査", 1); 
sqlite> INSERT INTO todo VALUES(2, 20250512, "定例に向けた資料作成", 1);
sqlite> INSERT INTO todo VALUES(3, 20250512, "blogネタ考える", 2);

表示してみましょう。いい感じです。

sqlite> SELECT \* FROM todo;
id|date|summary|priority
1|20250512|次の定例に向けた調査|1
2|20250512|定例に向けた資料作成|1
3|20250512|blogネタ考える|2

タスク更新をしてみます。プライオリティをあげてみましょう。

sqlite> UPDATE todo SET priority = 1 WHERE id = 3;

表示してみましょう。

sqlite> SELECT \* FROM todo;
id|date|summary|priority
1|20250512|次の定例に向けた調査|1
2|20250512|定例に向けた資料作成|1
3|20250512|blogネタ考える|1

タスク更新をしてみます。ブログネタをもう少し具体的にしましょう。

sqlite> UPDATE todo SET summary = "blogネタ K8s関連" WHERE id = 3;

表示してみましょう。

sqlite> SELECT \* FROM todo;
id|date|summary|priority
1|20250512|次の定例に向けた調査|1
2|20250512|定例に向けた資料作成|1
3|20250512|blogネタ K8s関連|1

タスクを削除してみましょう。

sqlite> DELETE FROM todo WHERE id = 3; 

表示してみましょう。

sqlite> SELECT \* FROM todo;
id|date|summary|priority
1|20250512|次の定例に向けた調査|1
2|20250512|定例に向けた資料作成|1

SQLiteクエリーから抜けるには、次を実行します。

sqlite> .quit

応用
--

かっこでSQL文を囲って実行すれば、ワンライナーでSQliteのシェルに入らずに操作できます。以下例はSELECT区を使っていますが、INSERTやDELETEなども同じ方法で操作できます。

% sqlite3 todo.sqlite3 "SELECT \* FROM todo"

この方法だとテーブルのヘッダーが表示されないため、ヘッダーがどうしても必要ならば次のような内容の`.sqliterc`ファイルを作ると便利です。`.headers on`の代わりに`.mode`を指定するのもありなのかもしれません。色々な設定については[Command Line Shell For SQLite changing\_output\_formats](https://sqlite.org/cli.html#changing_output_formats)などを確認してください。

% cat ~/.sqliterc
.mode column
.headers on

次のように実行すると、ワンライナーで結果を出せます。

% sqlite3 todo.sqlite3 "SELECT \* FROM todo"
id|date|summary|priority
1|20250512|次の定例に向けた調査|1
2|20250512|定例に向けた資料作成|1

macOSではzshを使っているので、次のような設定を`.zshrc`に書いて`todo`コマンドでタスク一覧の表示、`todo-edit`でSQL Shellに入ってタスクの管理みたいな設定をしたらより使いやすくなりました。

alias todo="sqlite3 ~/todo.sqlite3 'SELECT \* FROM todo'"
alias todo-edit="sqlite3 ~/todo.sqlite3"

実際使ってみるとこんな感じです。

% todo
-- Loading resources from /Users/ytooyama/.sqliterc
id|date|summary|priority
1|20250512|YugabyteDBの障害時の復旧について|1
2|20250512|blogネタ推敲|2

% todo-edit
-- Loading resources from /Users/ytooyama/.sqliterc
SQLite version 3.43.2 2023-10-10 13:08:14
Enter ".help" for usage hints.
sqlite> SELECT \* FROM todo;
1|20250512|次の定例に向けた調査|1
2|20250512|定例に向けた資料作成|1

\--

ここ最近はSQLをよく触っているので、おそらく独自の何かを導入して独自の形式のファイルを生成してタスクを管理するようなCLIツールをインストールするよりかはラクではないかと思います。 色々工夫はしたものの、3日後とかに使わなくなっている可能性は大ですが。

今回は最小限のリソースで動かすためにSQLiteを使いましたが、最近検証に使っているNoSQL系を使うのもありかもしれませんね。それこそYugabyteDBのようなSQLと互換性のあるものを使えば、応用が効きますし。

SQLite形式でデータを取っておけば、MySQLだとかPostgreSQLにデータを持っていくのもラクです。 データベースサーバーを提供できれば、タスクの共有とかもできるかなとか？ 「いやDB直でタスク投入するんじゃなくて、フロントエンドを作れよw」というご意見については、ごもっともな話です。

[[source]](https://devops-blog.virtualtech.jp/entry/20250514/1747187780)
