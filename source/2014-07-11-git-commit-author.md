---
id: 973
title: GitコミットログのAuthorの変更
date:  2014-07-11
comments: true
tags:
- Git
---

違うメールアドレスと名前で間違ってコミットしてそのままGitHubにpushしたら違うメールアドレスでちゃったやばいどうしよーとかっていうことやっちゃいがちですよね。
僕はたまにやっちゃって悲しい想いをします。

そんな時のための修正コマンドです。直前のコミットに関してはこれでいけます。

```
% git commit --amend --author='Author name <name@hoge.hoge>'
```
