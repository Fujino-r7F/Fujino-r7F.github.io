---
title: "MicrosoftSQLServerでハイフンを含むテーブルを指定したクエリでエラーになるときの対処法"
date: 2021-09-27T21:02:55+09:00
description:
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- SQL
series:
-
categories:
-
image:
---
テーブル名を[]で囲む
```
DROP TABLE [test-table];
```