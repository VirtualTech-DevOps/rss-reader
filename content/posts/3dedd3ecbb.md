+++
title = """CFnテンプレートをHCLにコンバート"""
date = Mon, 22 Jul 2024 11:54:37 +0900
tags = ["vtj","devops"]
+++
私はCloudFormationテンプレートが苦手です。CloudFormationが悪いツールだとは思いませんが、どうも慣れないというかなんというか。Terraformの方が簡単に書ける印象です。書くのが苦手ならば、もちろん読むのも苦手でして。たまにCFnテンプレートを渡されて、Terraformにしといてちょっと言われるのですが、よくわからないので渡されたテンプレートからCFnスタックを作成し、それをTerraformのimportでTerraform化していました。が、ちょっと検索するとcf2tfなるツールが・・・

cf2tf
-----

[github.com](https://github.com/DontShaveTheYak/cf2tf)

CFnテンプレートをTerraformのコードにコンバートしてくれるツールです。できることはそれだけですが大変便利。

試す
--

簡単なCFnテンプレートを用意しました。

\---
Resources:
  IAMUser:
    Type: AWS::IAM::User
    Properties:
      Path: /
      UserName: "test-user"

  IAMUserAccessKey:
    Type: AWS::IAM::AccessKey
    Properties:
      UserName: !Ref IAMUser

  IAMUserAccessKeySecret:
    Type: AWS::SecretsManager::Secret
    Properties:
      Name: !Sub credentials/${IAMUser}
      SecretString: !Sub "{\\"accessKeyId\\":\\"${IAMUserAccessKey}\\",\\"secretAccessKey\\":\\"${IAMUserAccessKey.SecretAccessKey}\\"}"

このコードでは、IAMユーザーを作成し、そのIAMユーザーで発行したクレデンシャルをSecrets Managerに登録します。

このコードを`iam.aml`というファイルに保存し、cf2tfに食わせてみましょう。

$ cf2tf iam.yaml

// Converting iam.yaml to Terraform!
// Existing Terraform src code found at /var/folders/ys/ql50h491259ft68p\_sf6qfjc0000gn/T/terraform\_src.

resource "aws\_iam\_user" "iam\_user" {
  path = "/"
  name = "test-user"
}

resource "aws\_iam\_access\_key" "iam\_user\_access\_key" {
  user = aws\_iam\_user.iam\_user.arn
}

resource "aws\_secretsmanager\_secret" "iam\_user\_access\_key\_secret" {
  name = "credentials/${aws\_iam\_user.iam\_user.arn}"
  force\_overwrite\_replica\_secret = "{"accessKeyId":"${aws\_iam\_access\_key.iam\_user\_access\_key.create\_date}","secretAccessKey":"${aws\_iam\_access\_key.iam\_user\_access\_key.secret}"}"
}

見事にTerraformのコードが生成されました。

デフォルトの動作では、コンバートしたHCLが標準出力されます。cf2tfに`-o`オプションで出力先のディレクトリを指定してあげると、指定のディレクトリに出力できます。

$ cf2tf iam.yaml -o test            

// Converting iam.yaml to Terraform!
// Existing Terraform src code found at /var/folders/ys/ql50h491259ft68p\_sf6qfjc0000gn/T/terraform\_src.
Saving converted terraform objects to /path/to/test

$ ls test
                                                      
resource.tf

今回は`resource.tf`しかありませんが、変数を使っていれば`variable.tf`、データソースを使用する場面があれば`data.tf`など、いい感じにファイルを分割してくれます。

`terraform validate`コマンドを実行してみると微妙にエラーが出ました。

$ terraform validate

╷
│ Error: Missing newline after argument
│ 
│   on resource.tf line 12, in resource "aws\_secretsmanager\_secret" "iam\_user\_access\_key\_secret":
│   12:   force\_overwrite\_replica\_secret = "{"accessKeyId":"${aws\_iam\_access\_key.iam\_user\_access\_key.create\_date}","secretAccessKey":"${aws\_iam\_access\_key.iam\_user\_access\_key.secret}"}"
│ 
│ An argument definition must end with a newline.
╵
╷
│ Error: Invalid character
│ 
│   on resource.tf line 12, in resource "aws\_secretsmanager\_secret" "iam\_user\_access\_key\_secret":
│   12:   force\_overwrite\_replica\_secret = "{"accessKeyId":"${aws\_iam\_access\_key.iam\_user\_access\_key.create\_date}","secretAccessKey":"${aws\_iam\_access\_key.iam\_user\_access\_key.secret}"}"
│ 
│ This character is not used within the language.
╵
╷
│ Error: Invalid character
│ 
│   on resource.tf line 12, in resource "aws\_secretsmanager\_secret" "iam\_user\_access\_key\_secret":
│   12:   force\_overwrite\_replica\_secret = "{"accessKeyId":"${aws\_iam\_access\_key.iam\_user\_access\_key.create\_date}","secretAccessKey":"${aws\_iam\_access\_key.iam\_user\_access\_key.secret}"}"
│ 
│ This character is not used within the language.
╵

Secrets Managerに設定するJSONがうまくコンバートできなかったみたいです。

diff --git a/resource.tf b/resource.tf
index ef81a3e..380c0a0 100644
--- a/resource.tf
+++ b/resource.tf
@@ -9,6 +9,8 @@ resource "aws\_iam\_access\_key" "iam\_user\_access\_key" {
 
 resource "aws\_secretsmanager\_secret" "iam\_user\_access\_key\_secret" {
   name = "credentials/${aws\_iam\_user.iam\_user.arn}"
-  force\_overwrite\_replica\_secret = "{"accessKeyId":"${aws\_iam\_access\_key.iam\_user\_access\_key.create\_date}","secretAccessKey":"${aws\_iam\_access\_key.iam\_user\_access\_key.secret}"}"
+  force\_overwrite\_replica\_secret = jsonencode({
+    accessKeyId     = aws\_iam\_access\_key.iam\_user\_access\_key.create\_date,
+    secretAccessKey = aws\_iam\_access\_key.iam\_user\_access\_key.secret
+  })
 }
-

このように修正して再度バリデートしてあげると

$ terraform validate

Success! The configuration is valid.

成功です。

$ terraform plan    

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws\_iam\_access\_key.iam\_user\_access\_key will be created
  + resource "aws\_iam\_access\_key" "iam\_user\_access\_key" {
      + create\_date                    = (known after apply)
      + encrypted\_secret               = (known after apply)
      + encrypted\_ses\_smtp\_password\_v4 = (known after apply)
      + id                             = (known after apply)
      + key\_fingerprint                = (known after apply)
      + secret                         = (sensitive value)
      + ses\_smtp\_password\_v4           = (sensitive value)
      + status                         = "Active"
      + user                           = (known after apply)
    }

  # aws\_iam\_user.iam\_user will be created
  + resource "aws\_iam\_user" "iam\_user" {
      + arn           = (known after apply)
      + force\_destroy = false
      + id            = (known after apply)
      + name          = "test-user"
      + path          = "/"
      + tags\_all      = (known after apply)
      + unique\_id     = (known after apply)
    }

  # aws\_secretsmanager\_secret.iam\_user\_access\_key\_secret will be created
  + resource "aws\_secretsmanager\_secret" "iam\_user\_access\_key\_secret" {
      + arn                            = (known after apply)
      + force\_overwrite\_replica\_secret = (sensitive value)
      + id                             = (known after apply)
      + name                           = (known after apply)
      + name\_prefix                    = (known after apply)
      + policy                         = (known after apply)
      + recovery\_window\_in\_days        = 30
      + tags\_all                       = (known after apply)
    }

Plan: 3 to add, 0 to change, 0 to destroy.

───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run "terraform apply" now.

`terraform plan`も大丈夫そうでした。

まとめ
---

CFnテンプレートをわざわざ実環境にデプロイしなくてHCL化できるのは大変便利です。ただし、完璧にコンバードができるわけではなさそうなので、`terraform validate`などと組み合わせて修正しつつ進める必要がありました。

[[source]](https://devops-blog.virtualtech.jp/entry/20240722/1721616877)
