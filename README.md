# GO-TIPS

**1 类型断言 type assert**
如果你想把任意类型作为函数的参数，如何做到?
func Receive(argument interface{})
必须定义函数的输入参数为空接口，然后在函数内部：
value, ok := argument.(type)
此处的type 可以是go内部任何类型，比如int,string,float32...

**1.1如何判断某结构体有没有特定函数?**
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

