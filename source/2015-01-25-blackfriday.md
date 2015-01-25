---
id: 985
title: GolangでBlackfridayを使ってMarkdownをHTMLに変換する
date:  2015-01-25
tags:
- Golang
- Go
keywords:
- Golang
- Go
---

Rails大好きfunnythingzです。TypeScriptよりもRails書いてます。

Railsに慣れすぎてしまうと他のことができなくなってやばいと思い2015年はGolangに力を入れていこうと思っています。Go界隈の流れの早さについていけていません。

それはさておきGoでMarkdownをHTMLにしたいなと思いGitHubをいろいろ探していたら[russross/blackfriday](https://github.com/russross/blackfriday)というものを見つけました。名前が中二感ハンパなくてカッコイイです。sanitizerの[microcosm-cc/bluemonday](https://github.com/microcosm-cc/bluemonday)というものもあります。名前カッコイイわー。

とりあえず使い方は簡単なのでコード書いておきます。

### markdown

hoge.md

```Markdown
# hoge

hogehoge

- hoge
- hoge

## hoge
```

### Go

demo.go

```Go
package main

import (
	"fmt"
	"github.com/russross/blackfriday"
	"io/ioutil"
)

func check(e error) {
	if e != nil {
		panic(e)
	}
}

func main() {
	md, err := ioutil.ReadFile("hoge.md")
	check(err)
	output := blackfriday.MarkdownCommon([]byte(md))
	fmt.Println(string(output))
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

<ul>
<li>hoge</li>
<li>hoge</li>
</ul>

<h2>hoge</h2>
```

となります。簡単！
