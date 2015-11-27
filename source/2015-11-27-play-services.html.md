---
id: 993
title: Android2系でインストール時にINSTALL_FAILED_DEXOPTエラー
date: 2015-11-27 23:55 JST
tags:
- Android
---

お久しぶりです。Goエンジニアやってたと思ったら今ではAndroidエンジニアやってる今日このごろです。

Android2系で[こんな感じのエラー](https://www.facebook.com/notes/facebook-engineering/under-the-hood-dalvik-patch-for-facebook-for-android/10151345597798920)に遭遇してしまい、メソッド数を減らさなくてはいけないため対処した内容をメモっておきます。

どうもgradleで `com.google.android.gms:play-services` を読み込んでいると、ほとんど使ってない余計なものがたくさんあるようで、なんと `18513` もメソッドがあるようです。

```
dependencies {
  compile 'com.google.android.gms:play-services:6.5.+'
}
```

なので、これを


```
dependencies {
  compile 'com.google.android.gms:play-services-base:6.5.87'
  compile 'com.google.android.gms:play-services-identity:6.5.87'
}
```

こんな感じに必要なものだけをcompileするようにします。これでメソッド数がかなり減らせるのと、ビルド時間もかなり短縮できます。

さらに[詳しくはこちら](http://android-developers.blogspot.de/2014/12/google-play-services-and-dex-method.html)をみてみてください。
