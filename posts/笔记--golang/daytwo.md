### 字符串

* 没有特殊字符

  使用双引号

* 有特殊字符

  可以用反引号



### 类型转换

golang中需要手动转换类型，没有自动转换

##### 基本数据类型转string

```go
var n1 int = 10
var s1 string = fmt.Sprintf("%d", n1)

var s2 string = strconv.FormatInt(int64(n1), 10) //没有s1简单，其中需要强制转换
```



##### string类型转其他类型

```go
var b bool 
b, _ = strconv.ParseBool(s1)
num, _ = strconv.ParseInt(n1, in, 64)
```



### 循环

##### for range键值循环

```go
var str string = "hello golang"

for i, value := range str{
    fmt.Printf(i, value)
}
```



### 函数

* golang函数不支持重载

* golang函数支持可变参数

```GO
func test(args...int){}   //可以传入任意多个int类型的数据a
```



* Go中，函数也是一种数据类型，可以被复制给变量
* 也可以把函数当形参
* type命名

```go
func test(num int){}

func  test02(num1 int, num2 float32, testFunc func(int)){}

func main(){
    a := test
    a(10)   //等价于test(10)
    test02(10, 2.14, a)
}

type myFunc func(int)
```



### 包

解决同名函数问题

main.go

```go
package main  //1、package进行包的声明，建议包和所在文件夹相同
//2、main包是程序的入口包，一般main函数在main包下

//3、包名从$GOPATH/src/后开始计算，GOPATH定义在系统变量中

//4、一次性导入多个包
import(
	"fmt"
    "xxx"
)
func main(){
    GetConn()
}
```

dbutils.go

```go
package dbutils
import "fmt"

func GetConn(){}  //5、首字母大写，可以被其他包访问
```





