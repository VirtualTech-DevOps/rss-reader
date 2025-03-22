+++
title = """Pleasanterのプロセスで作成したボタンのアイコンをスクリプトで上書きする方法"""
date = 2024-11-07T01:55:05.000Z
expiryDate = 2025-03-25T22:06:04.745Z
tags = ["vtj","devops"]
+++
Pleasanterのプロセス機能でボタンを作成すると、作成や更新と同じアイコンが表示されます。 誤操作や誤解を防ぐために、ボタンのアイコンを実際の処理近いものに変更したいことがありますが、標準機能ではアイコンを変更することはできません。

今回は、Materialアイコンを設定する方法を紹介します。ただし、PleasanterのバージョンによってはMaterialがサポートされていない可能性があります。

具体的にどのバージョンからとは言えないのですが、噂によるとログイン時のパスワード表示/非表示アイコンが出ていたらサポートされているみたいです。

ボタン名とアイコンの意味が一致していないですが、こんな感じで変更できます

![](https://cdn-ak.f.st-hatena.com/images/fotolife/v/virtualtech/20241107/20241107105510.png)

使用したいアイコンを決める
=============

Materialアイコンから今回使用したいアイコンを選びます。今回は`post_add`を使用します。Materialアイコンの一覧は以下のURLから確認できます。

[Material Symbols and Icons - Google Fonts](https://fonts.google.com/icons)

必要な`<span>...</span>`タグをコピーしてメモ帳やエディターに貼り付けて用意します。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/v/virtualtech/20241107/20241107105506.png)

スクリプトの作成
========

以下のスクリプトをPleasanterのスクリプトに追加して保存します。

function overrideButtonIcon(id, newClassName, newText) {
  const element \= document.querySelector(\`#${id} > span.ui-button-icon.ui-icon.ui-icon-disk\`);

  if (!element) {
    console.error(\`要素が見つかりません: #${id}\`);
    return;
  }
    
  element.className \= newClassName;
  element.innerText \= newText;

  element.style.fontSize \= "16px";
  element.style.verticalAlign \= "middle";
}
  
window.addEventListener("load", (event) \=> {
  overrideButtonIcon("Process\_1", "material-symbols-outlined", "post\_add");
});

中の処理を簡単に説明するとclass属性とinnerTextを置き換えて、フォントサイズなどを整えているだけです。

先ほどコピーした`<span>...</span>`タグの中身を`overrideButtonIcon`関数の引数に渡しています。

*   `Process_1`: ボタンのIDを`overrideButtonIcon`関数の第1引数に渡します。プロセスIDに応じて置き換えてください
*   `material-symbols-outlined`: class属性の値を`overrideButtonIcon`関数の第2引数に渡します。
*   `post_add`: innerTextの値を`overrideButtonIcon`関数の第3引数に渡します。

<span class\="material-symbols-outlined"\>
post\_add
</span>

スクリプトの変更ができたら、更新して編集画面を開いてボタンのアイコンが変更されていることが確認できたら成功です。

[[source]](https://devops-blog.virtualtech.jp/entry/20241107/1730944505)
