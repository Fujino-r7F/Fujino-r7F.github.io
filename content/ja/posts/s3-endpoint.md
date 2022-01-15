---
title: "VPCエンドポイントの違いについて"
date: 2022-01-15T21:06:59+09:00
description:
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- AWS
- S3
series:
-
categories:
-
image:
---
## はじめに
VPCエンドポイントにはゲートウェイ型とインターフェース型の2種類が現状ありますが、いまいち違いが理解できてなかったのでまとめてみました。

## そもそもVPCエンドポイントとは
VPCと他のAWSサービスの間を通信可能にするエンドポイントのこと  
VPCとVPC外のサービスをインターネットを経由せずにプライベートな接続を行うことができる  

## ゲートウェイ型
名前の通りゲートウェイとして動作する  
ゲートウェイ型をVPCに追加すると、そのVPCのルートテーブルにゲートウェイへのルーティングが追加される 

エンドポイントのIPはグローバルIPなのでACL等でローカルIPのみの制限を加えてしまうとエンドポイントと通信できなくなってしまう  

## インタフェース型
VPC内部でENIを利用してエンドポイントが立ち上がる 
これはPrivateLinkというサービスを使っているらしい 

これだとVPC内のローカルに立ち上がるためACL等で制限しても問題ない 