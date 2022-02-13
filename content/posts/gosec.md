---
title: gosecについて調べた事メモ
date: "2022-01-03T18:24:05+09:00"
description: gosecについて簡単にメモ
publishDate: "2022-01-03T18:24:05+09:00"
---

# gosec について調べた事メモ

仕事で [gosec](https://github.com/securego/gosec) を使う機会ができたので、調べた事を簡単にメモします。

<br>

## gosec とは

<br>
ソースコードを解析してセキュアではないコードを検出してくれるツールです。

Github のリポジトリには以下のように記載してあります。

```
Inspects source code for security problems by scanning the Go AST.
```

> Go AST を解析して、ソースコードのセキュリティ上問題がある部分を調査します。

<br>

## AST とは

<br>

Wiki によると

```
抽象構文木（英: abstract syntax tree、AST）は、通常の構文木（具象構文木あるいは解析木とも言う）から、言語の意味に関係ない情報を取り除き、意味に関係ある情報のみを取り出した（抽象した）木構造の木である。
```

参照:
https://ja.wikipedia.org/wiki/%E6%8A%BD%E8%B1%A1%E6%A7%8B%E6%96%87%E6%9C%A8
<br>
<br>

少し難しい表現に感じますが、要するにプログラムからコメントや意味を持たない空白など不要ものを取り除き、プログラムの文法構造をツリー構造で表現したものみたいです。

<br>

## どんなパターンを検出してくれるのか

<br>
クレデンシャルのハードコーディングやエラーの未チェックの箇所などを検出してくれます。

[こちら](https://github.com/securego/gosec#available-rules)に検出パターンの一覧が記載されています。

<br>

## インストール

<br>

```shell
go install github.com/securego/gosec/v2/cmd/gosec@latest
```

<br>

## 実行方法

```shell
gosec ./...
```

<br>
検査項目を絞る事も可能

```shell
検査対象を限定する
gosec -include=G101 ./...

検査対象を除外する
gosec -exclude=G101 ./...
```

<br>

## 実行例

<br>

### クレデンシャルのハードコーディング

#### ソース

```go
package main

import (
	"fmt"
)

func main() {
	AWS_CREDENTIAL = "KJH*(#(178" // dummyのクレデンシャル
	fmt.Println(AWS_CREDENTIAL)
}
```

#### gosec 実行

```shell
gosec ./...
[gosec] 2022/01/03 18:50:48 Including rules: default
[gosec] 2022/01/03 18:50:48 Excluding rules: default
[gosec] 2022/01/03 18:50:48 Import directory: /Users/maakun12/Documents/gosec_practice
[gosec] 2022/01/03 18:50:49 Checking package: main
[gosec] 2022/01/03 18:50:49 Checking file: /Users/maakun12/Documents/gosec_practice/main.go
Results:
[/Users/maakun12/Documents/gosec_practice/main.go:8] - G101 (CWE-798): Potential hardcoded credentials (Confidence: LOW, Severity: HIGH)
    7: func main() {
  > 8: 	AWS_CREDENTIAL = "KJH*(#(178"
    9: 	fmt.Println(AWS_CREDENTIAL)

Summary:
  Gosec  : dev
  Files  : 1
  Lines  : 10
  Nosec  : 0
  Issues : 1
```

<br>

### エラーの未チェック

```go
package main

func test() error {
	return nil
}
func main() {
	test()
}
```

#### gosec 実行

```shell
gosec ./...
[gosec] 2022/01/03 19:00:17 Including rules: default
[gosec] 2022/01/03 19:00:17 Excluding rules: default
[gosec] 2022/01/03 19:00:17 Import directory: /Users/maakun12/Documents/gosec_practice
[gosec] 2022/01/03 19:00:17 Checking package: main
[gosec] 2022/01/03 19:00:17 Checking file: /Users/maakun12/Documents/gosec_practice/main.go
Results:


[/Users/maakun12/Documents/gosec_practice/main.go:7] - G104 (CWE-703): Errors unhandled. (Confidence: HIGH, Severity: LOW)
    6: func main() {
  > 7: 	test()
    8: }

Summary:
  Gosec  : dev
  Files  : 1
  Lines  : 8
  Nosec  : 0
  Issues : 1
```

<br>
<br>

## まとめ

<br>
上記のように簡単にソースコードを解析する事ができました。

<br>

github action で実行して、レビュー前に非セキュアなコードを検知する事も可能なので非常に使い勝手が良さそうです。
