## Goのパッケージ管理

### パッケージとは
> Go コンパイラにおける処理単位。具体的にはひとつの物理ディレクトリ内のファイル群をひとつのパッケージとして処理する。

- import宣言でパッケージを指定する

```
import "path/to/package"
```

- `go get` または `go mod tidy` コマンドを使ってあらかじめ外部パッケージをダウンロードしておくことで参照可能となる。

### GOPATHモード

> 標準ライブラリを除く全てのパッケージのコード管理とビルドを環境変数 `GOPATH` で指定されたディレクトリ下で行う。パッケージの管理はリポジトリの最新リビジョンのみが対象となる。

- import された package の探索先は `$GOPATH/src`
- package は `$GOPATH/src` 配下に存在しなければ探索できない

→ `GOPATH/src` 配下でしか開発できない

参考: [https://zenn.dev/tennashi/articles/3b87a8d924bc9c43573e](https://zenn.dev/tennashi/articles/3b87a8d924bc9c43573e)

### モジュール対応モード (module-aware mode)

> 標準ライブラリを除く全てのパッケージを**モジュール**として管理する。コード管理とビルドは**任意のディレクトリ**で可能で，モジュールはリポジトリのバージョンタグまたはリビジョン毎に管理される。

- `go.mod` をプロジェクトルートに配置することで、モジュール対応モードで動作する。
  
→ 自分の好きなところにプロジェクトを作成できる。

- `$GOPATH/pkg/mod` 配下に依存パッケージがダウンロードされる。

- `go build` または、`go test` の際に自動で依存パッケージがダウンロードされる。

- `go.sum` が生成される。

> `go.sum` ファイルにはインポートするモジュールの SHA-256 チェックサム値が格納されている。

参考: [https://zenn.dev/spiegel/articles/20210223-go-module-aware-mode](https://zenn.dev/spiegel/articles/20210223-go-module-aware-mode)


### モジュールとは

> モジュール対応モードでは，標準ライブラリを除くパッケージを「**モジュール（module）**」として管理する。パッケージが単一のディレクトリを指すのに対し，モジュールは `go.mod` ファイルのあるディレクトリ以下の（go.mod を含まない）全てのパッケージがモジュールの配下となる。


### Modules を使う流れ

> 直接 `go.mod` ファイルを操作する機会は少なく、他の `go サブコマンド` を実行したときに、自動的に処理が行われることが多い。


1. 新規プロジェクトディレクトリを用意
2. `go mod init` で、初期化する。
3. `go build` などのビルドコマンドで、依存モジュールを自動インストールする。
4. `go list -m all` で、現在の依存モジュールを表示する。
5.  `go get` で、依存モジュールの追加やバージョンアップを行う。
6.  `go mod tidy` で、使われていない依存モジュールを削除する。

- `go.mod` の初期化

```
$ mkdir go-mod-test
$ cd go-mod-test
$ go mod init example.com/go-mod-test
```

- `go.mod` ファイルが作成される。
　
```
$ cat go.mod
module example.com/go-mod-test

go 1.12
```

- `go build` などのビルドコマンドで、依存モジュールを自動インストールする。

```
package main

import (
    "github.com/aws/aws-lambda-go/events"
    "github.com/aws/aws-lambda-go/lambda"
)

func handler(request events.APIGatewayProxyRequest) (events.APIGatewayProxyResponse, error) {
    return events.APIGatewayProxyResponse{}, nil
}

func main() {
    lambda.Start(handler)
}
```
- 依存しているモジュールが自動でインストールされる

```
$ go build
go: finding github.com/aws/aws-lambda-go/lambda latest
go: finding github.com/aws/aws-lambda-go/events latest
```

> `go.mod`が更新される

```
$ cat go.mod
module example.com/go-mod-test

go 1.12

require github.com/aws/aws-lambda-go v1.13.2
```

### go mod tidy によるモジュール情報の更新

> Go バージョン 1.15 までは `go build` や `go test` といったコマンドの実行時に `go.mod` および `go.sum` ファイルが更新されていたが， 1.16 からはこれができなくなった。

- ソース・コード上で新しい外部パッケージを import しても `go.mod` と `go.sum` に記述がないと下記のようなエラーが出る。
  
``` $ go test ./...
main.go:9:2: no required module provides package github.com/goark/cov19jpn/chart; to add it:
    go get github.com/goark/cov19jpn/chart
```

```
$ go test ./...
go: github.com/goark/cov19jpn@v0.3.0: missing go.sum entry; to add it:
    go mod download github.com/goark/cov19jpn
```

- `go mod tidy` を実行し `go.mod` を更新する必要がある。

```
go mod tidy 
```