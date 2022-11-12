## 基礎構文

### Hello World

```
package main

import "fmt"

func main() {
	fmt.Println("Hello World!")
}
```

- 標準ライブラリfmtをimport
- fmtのprintlnで文字列を出力
- main関数のみが実行される

> Hello World!

```
package main

import "fmt"

func bazz() {
	fmt.Println("Hello Bazz!")
}

func main() {
	bazz()
	fmt.Println("Hello World!")
}
```
- main関数以外を呼び出す場合は、main関数内で呼び出す必要がある。

> Hello Bazz! <br>
  Hello World!

### init関数

- init関数は最初に呼び出される。
- 初期化などに用いられる
  
```
package main

import "fmt"

func init() {
    fmt.Println("init!")
}

func bazz() {
    fmt.Println("Hello Bazz!")
}

func main() {
    bazz()
    fmt.Println("Hello World!")
}
```

> init!<br>Hello Bazz!<br>Hello World!

### コメント

```
/*
func init() {
    fmt.Println("init!")
}
*/

func bazz() {
    fmt.Println("Hello Bazz!")
}

func main() {
    // bazz()
    fmt.Println("Hello World!")
}
```
- `//` または `/* */`

> Hello World!

### import

- パッケージまたはライブラリを呼び出す

- 標準パッケージについては、下記のDocumentを参照  <br>
  [https://pkg.go.dev/std](https://pkg.go.dev/std)

- `godoc hoge`でdocumentを表示

```
package main

import (
    "fmt"
    "os/user"
    "time"
)

func main() {
    fmt.Println("Hello world!", time.Now())
    fmt.Println(user.Current())
}
```

- `os/user` → `/` で一つ下の階層を指定する

> Hello World! 現在時刻<br>&{501 20 matsumotoyuudai 松本雄大 /Users/matsumotoyuudai} <nil>