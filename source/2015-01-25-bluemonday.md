---
id: 986
title: Golangでbluemondayを使ってhtml_safeなsanitizeをする
date:  2015-01-25
tags:
- Golang
- Go
keywords:
- Golang
- Go
---

ここ数年ブログを書くのが億劫すぎていたのでまさかの1日2連続エントリーです。2015年はブログもちゃんと書くぞ！

[GolangでBlackfridayを使ってMarkdownをHTMLに変換する](http://hiropo.co.uk/archives/985.html)

上記記事でも軽くふれたのですが、sanitizerの[microcosm-cc/bluemonday](https://github.com/microcosm-cc/bluemonday)も便利なので(名前が鬱になるけど)使ってみました。

### html

hoge.html

```html
<h1>hoge</h1>
<p>hogehoge</p>
<script>alert(1);</script>
<style>body {font-size: 100px;}</style>
<footer>&copy; funnythingz</footer>
```

### Go

demo.go

```Go
package main

import (
	"fmt"
	"github.com/microcosm-cc/bluemonday"
	"io/ioutil"
)

func check(e error) {
	if e != nil {
		panic(e)
	}
}

func main() {
	html, err := ioutil.ReadFile("hoge.html")
	check(err)

	p := bluemonday.UGCPolicy()
	fmt.Println(p.Sanitize(string([]byte(html))))
}
```

### 出力結果

```
% go run demo.go
```

で実行すると

```html
<h1>hoge</h1>
<p>hogehoge</p>


© funnythingz
```

となってhtml_safeな感じで出力されます。ちなみにbluemondayのポリシーで許可されているタグは下記になります。

詳細は[bluemondayのコード](https://github.com/microcosm-cc/bluemonday/blob/master/policies.go)をみてみてください。

> article, aside, figure, section, summary, h1~h6, hgroup, br, div, hr, p, span, wbr, abbr, acronym, cite, code, dfn, em, figcaption, mark, s, samp, strong, sub, sup, var, b, i, pre, small, strike, tt, u, rp, rt, ruby

こんな感じでblackfridayとbluemondayを組み合わせて使うといろいろできそうですね！
