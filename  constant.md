## 定数

- `const` で定義する

```
import "fmt

const Pi = 3.14

const (
    username = "testUser",
    password = "testPass"
)

func main() {
    fmt.Println(Pi, username, password)
}
```

> 3.14<br>testUser<br>testPass

- 定義後に書き換えることはできない

```
const Pi = 3.14

func main() {
    Pi = 3   // error
}
```

- `const` は、`型宣言` が必要ない
- 定義した時点では、コンパイラに解釈はされるが実行はされない
  <br>→ 関数などで実行された時に初めてその定数に行った処理が実行される

```
import "fmt

var i int = 9223372036854775807 + 1    // この時点で足し算が実行されるのでoverflowのエラーになる

const big = 9223372036854775807 + 1    // この時点では足し算は実行されない

func main() {
    fmt.Println(big-1)   // ここで定数に対しての処理が実行される

    // → 定数 + 1 & -1 なのでoverflowせずに 9223372036854775807 が表示される
}
