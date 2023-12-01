### 包

* 导入包时，要把包的所在文件路径import进来
* 一个目录下的同级文件同属于一个包
* 对包起别名后，原名字失效

```go
import(
	test "fmt"
)
//这里用test给fmt起别名，在下面的程序中也就只能使用test
```



### init函数

* 初始化函数，每个源文件都能包含init函数，在main函数之前，被GO运行框架调用
* 执行顺序：全局变量定义 > init函数 > main函数
* 多个源文件都有init函数，则按照main.go中的import顺序来执行

### 匿名函数

* 使用场景：只用一次（也可以多次使用（给全局变量），但不常用这种方式）

```go
result := func(num1 int, num2, int) int{
    return num1 + num2
}(10, 20)
```



### 闭包

```go
//getsum参数为空
func getsum() func(int) int{
    var sum int = 0
    return func(num int) int{
        sum += num
        return sum
    }
}
//闭包：返回的匿名函数 + 匿名函数以外的变量num
```

* 本质依旧是一个匿名函数，只是这个匿名函数引入外界的变量或者参数
* 特点：
  * 闭包中使用的变量会一直保存在内存中，所以闭包不可以滥用
* 什么情况下使用闭包

​	闭包可以保留上次引用的某个值，我们传入一次就可以反复使用

### defer

为了在函数执行完毕后及时释放资源

```go
func add(num1 int, num2 int) int{
    //golang中遇到defer，会将defer中的语句压入栈，然后继续执行后面的语句
    defer fmt.Println("num1=", num1)
    defer fmt.Println("num2=", num1)
    //函数执行完毕后，从栈中取出语句开始执行，按照栈的先进后出顺序
    //并且相关的值也会被拷贝入栈
    var sum int = num1 + num2
    fmt.Println("sum=", sum)
    return sum
}
//上面代码的输出为
sum = 90
num2 = 30
num1 = 60
```

* 应用defer的场景

​	例如在写数据库时，在关闭与数据库连接的代码前加个defer，可以不关注 语句顺序自动关闭

### 字符串

* len(str)  内置函数不用导包，直接用

​	golang中，汉字是utf-8字符集，一个汉字3个字节，字母一个字节

* 对字符串遍历

方式一：键值循环：for-range

```go
for i, value := range str{
    fmt.Println("index=%d, value=%c  \n") //golang你好
}
//index=0, value=g
//...
//index=5, value=g
//index=6, value=你 
//index=9, value=好
```

方式二：r:=[]rune(str)

```go
r:=rune(str)
for i:= 0; i < len(r); i ++{
    fmt.Println("%c", r[i])
}
```

* 字符串转整数n, err := strconv.Atoi("555")
* 整数转字符串str = strconv.Itoa(3424)
* 统计字符串中有几个指定的字串

```go
count := strings.Count("dfsf","d")
```

* 不区分大小写的字符串比较 

```go
strings.EqualFold("G","g")
```

* 字符串的替换

```go
strings.Replace("goalsdf","go","kdf", n)
//表示把go替换为kdf
//n表示替换几个，-1表示全部
```

* 按照指定的某个字符进行切割，将一个字符串转化为字符串数组

```go
arr := strings.Split("go-sdf-fsd","-")
```

* 大小写转换

```go
strings.ToLower()
strings.ToUpper()
```

* 去掉左右两边指定字符

```go
strings.Trim()
strings.TrimLeft()
strings.TrimRight()
strings.TrimSpace()  //去掉空格
```

### 时间

* 获取当前时间

```go
now := time.Now()  //返回一个结构体，类型为time.Time，使用具体单词获取具体信息，比如year
```

### 内置函数

不用调包的函数

* len()
* new()

```go
num := new(int)  
```





### defer + recover机制处理错误

提高程序健壮性

```go
func test(){
    defer func(){
        err := recover()
        if  err != nil{
            fmt.Println("get")
        }
    }() //defer后加上匿名函数的调用
    num1 := 10
    num2 := 0
    resulr := num1 / num2
}
```













