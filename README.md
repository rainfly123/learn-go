# GO-TIPS

**1 类型断言 type assert**
如果你想把任意类型作为函数的参数，如何做到?
func Receive(argument interface{})
必须定义函数的输入参数为空接口，然后在函数内部：
value, ok := argument.(type)
此处的type 可以是go内部任何类型，比如int,string,float32...

**2 如何判断某结构体有没有特定函数**
很简单 定义个接口里面实现了特定方法，再实用类型断言
```
package main
import "fmt"

type Student struct {
         Age int
}

type Read interface {
         Read()
}
 
func (xiaoming *Student) Read() {
      fmt.Println(xiaoming.Age)
}

func caller(p interface{}) {
      v, t := p.(Read)
      fmt.Println(v, t)
      v.Read()
 }
  
 func main() {
      a := Student{Age: 13}
      caller(&a)
 }
```

**3 变长参数**

```
package main

import "fmt"

func println(args ...interface{}) {
	for _, arg := range args {
		switch arg.(type) {
		case int:
			fmt.Println("int")
		case string:
			fmt.Println("string")
		case int64:
			fmt.Println("int64")
		default:
			fmt.Println("unkown")
		}
	}
}

func main() {
	println(3, "ddd")
}
```

**4 slice-append的使用**

```
package main

import "fmt"

func main() {
	var a = []int{1, 2, 3}
	var b = []int{4, 5, 6}
	fmt.Println(append(a, 4, 5))
	fmt.Println(append(a, b...))
	fmt.Println(append(a, b[:]...))
	fmt.Println(append(a[:1], a[2:]...))  删除

}
```

**5 channel**
ch := make(chan string)
close(ch)
i := <- ch // 不会panic, i读取到的值是空 "",  如果channel是bool的，那么读取到的是false
判断channel是否close
i, ok := <- ch
if ok {
    println(i)
} else {
    println("channel closed")
}
但是你也可以 range 它会自动退出 如果channel close啦
for i := range ch { // ch关闭时，for循环会自动结束
    println(i)
}
如何防止超时？
select {
    case <- time.After(time.Second*2):
        println("read channel timeout")
    case i := <- ch:
        println(i)
}
