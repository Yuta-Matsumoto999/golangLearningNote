## 変数定義

- var 変数名 型 = hoge

```
var i int = 1
var f64 float = 1.2
var s string = "test"
var t bool = true
var f bool = false
```

- 型が同じ変数は、`,`区切りでまとめることができる。

```
var t,f bool = true, false
```

- `()`でまとめる
  
```
var (
    var i int = 1
    var f64 float64 = 1.2
	var s string = "test"
	var t bool = true
	var f bool = false
)
```

- 値を入れない場合は、初期値が入る

```
import "fmt"

var (
    var i int
    var f64 float64
	var s string
	var t bool
	var f bool
    fmt.Println(i, f64, s, t, f)
)
```

> 0 0 false false

### Short variable declarations

- 関数の中では、 `var` 宣言の代わりに、短い `:=` の代入文を使い、**暗黙的な型宣言**ができる。

- **関数の外**では、キーワードではじまる宣言( `var`, `func`, など)が**必要**で、 `:=` での暗黙的な宣言は**利用できない。**

```
import "fmt"

func main() {
	xi := 1
	xf64 := 1.2
	xs := "test"
	xt, xf := true, false

	fmt.Println(xi, xf64, xs, xt, xf)
}
```

> 1 1.2 true false

#### 変数の型を書き換えたい場合

- 例) f64をfloat32に変更したい
- 関数内で型を明確に記述する

```
import "fmt"

var (
    i int
    f64 float64
	s string
	t bool
	f bool
)

func foo() {
    xi := 1
	var xf64 float32 := 1.2
	xs := "test"
	xt, xf := true, false
    fmt.Printf("%T", xf64)

	// fmt.Printf("%T", 変数) → その変数の型を出力
}

func main() {
    fmt.Println(i, f64, s, t, f)
    foo()
}
```

> 1 1.2 test true false<br>float32

- 型を明確に記述するか省略するか使い分ける

#### 下記はエラーになる

- Short variable declarationsで定義された変数に再度 `:=` をつけて値を入れる
- 型宣言されている変数に再度, `型` を宣言して値を入れる
  
```
func foo() {
    xi := 1
	xi := 1   // error
}
```

```
var (
    i int
)

func foo() {
   var i int = 1   // error
}
```

- 下記は**正常**に動作する

```
func foo() {
    xi := 1
	xi = 1
}
```

```
var (
    i int
)

func foo() {
   i = 1
}
```