---
title: "S3のバケットポリシーの設定項目についてまとめてみた"
date: 2022-01-15T21:48:29+09:00
description:
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- S3
series:
-
categories:
-
image:
---

## はじめに
S3のバケットポリシーを設定する際のパラメータについてまとめてみました


## バケットポリシーの設定項目
#### Version 
設定ファイルの文法のバージョン  
2012-10-17なのでもう10年近く変わってないが、どこかで変わったりするのだろうか  

#### Statement 
この中に設定を配列形式で書いていく それぞれのstatementは{}で囲む 

#### Sid 
Sid(statement id)はポリシードキュメントに与える任意の識別子のこと 
各statementに割り当てることができ、そのポリシードキュメント内でユニークでなければならない 

#### Effect 
Allow(ホワイトリスト)かDeny(ブラックリスト)を選択  

#### Principal
リソースへのアクセスを許可、または拒否する相手を指定する  
次のいずれかを指定できる 
* AWSアカウント及びルートユーザ 
* IAMユーザ 
* フェデレーティッドユーザー（ウェブ ID または SAML フェデレーションを使用）
* IAM ロール 
* ロールを引き受けるセッション 
* AWS のサービス 
* 匿名ユーザー  

#### Action 
許可または拒否するアクションを指定する 

#### Resource 
statementをどのリソースに設定するか指定する 

#### Condition 
ポリシーを適用する条件を設定できる 
バケットポリシーに複数のコンディションやstatementを設定した際の挙動は、下記のクラメソさんの記事がとても参考になりました。 
https://dev.classmethod.jp/articles/learn-aws-policy-documents-with-examples/


上記踏まえて特定のIPからと特定のvpceから接続可能にするポリシードキュメントを作ってみた  
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "test1",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::test",
                "arn:aws:s3:::test/*"
            ],
            "Condition": {
                "IpAddress": {
                    "aws:SourceIp": "XXX.XXX.XXX.XXX/32"
                }
            }
        },
        {
            "Sid": "test2",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::test",
                "arn:aws:s3:::test/*"
            ],
            "Condition": {
                "StringEquals": {
                    "aws:SourceVpce": "vpce-XXXXXXXXX"
                }
            }
        }
    ]
}
```