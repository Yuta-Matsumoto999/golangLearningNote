## 開発環境構築(Mac)

#### 環境情報

||情報|
|--|--|
|PC|Macbook Air M2 16GB|
|OS|macOS ventura 13.0|
|Homebrew|3.6.10|
|go|go1.19.3 darwin/arm64|
|editor|visual studio code|

### 1. Goをインストールする
- homebrewを使う

    `brew install go`

- version確認

    `go version`

    > go version go1.19.3 darwin/arm64

### 2. GOPATHの設定
> 環境変数であるGOPATH変数を必ず設定する必要がある。

> Goのインストールディレクトリと同じにはできない。

> GoのソースコードやGoの実行可能ファイル、並びにコンパイル済みのパッケージファイルを保存する為に使用する。

```
mkdir /Users/matsumotoyuudai/go
echo "export GOPATH=/Users/matsumotoyuudai/go" >> ~/.zshrc
echo 'export PATH="$GOPATH/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

- go開発を行うディレクトリを決める (→ /User/matsumotoyuudai/go)
- bashrcやzshrcにGOPATHを設定する
- ${GOPATH}/binにPATHを通しておく
  
参考: [https://github.com/astaxie/build-web-application-with-golang/blob/master/ja/01.2.md](https://github.com/astaxie/build-web-application-with-golang/blob/master/ja/01.2.md)

### 3. Goの拡張機能を追加

   - GO
   - Go Outliner
   - Go Test Exlorer
   - Go AutoTest

### 4. go.modを作成

- プロジェクトルートに `go.mod` ファイルを作成することでモジュール対応モードで動作する。

> モジュール対応モードでは、`go.mod` でパッケージの管理を行うことができる。

- `go mod init` で、初期化する。

```
$ mkdir go-mod-test
$ cd go-mod-test
$ go mod init example.com/go-mod-test
```

> go: creating new go.mod: module example.com/go-mod-test


### 4. 開発支援ツール

  - Goが標準で提供している機能は`go help`で確認可能
  - [https://pkg.go.dev/golang.org/x/tools/cmd](https://pkg.go.dev/golang.org/x/tools/cmd) から公式に開発されている有用なコマンドラインツールをインストールできる。

  #### godocインストール

  > Godoc は、Go プログラムのドキュメントを抽出して生成します。これは Web サーバーとして実行され、ドキュメントを Web ページとして表示します。

  - `go get golang.org/x/tools/cmd/godoc`でgodocをインストール
