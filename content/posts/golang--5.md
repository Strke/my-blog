+++

date=2023-11-15T20:23:08-04:00
description="Esmeralda"
featured_image="/images/esmeralda.jpg"
tags=[]
title="golang---5"
categories="golang"

+++

### 映射

* map内部无序
* map初始化

```go
//       [key]value
var a map[int]string
//声明不会分配空间，必须用make初始化来分配空间
a = make(map[int]string, 10)//存放10个键值对，不传size大小默认分配一个

b := make(map[int]string)

c := map[int]string{
    3425:"dff",
}
```

* map操作

```go
//增加
map["key"] = value
//删除
delete(map, "key")
//查找
value, bool = map[key]
//遍历
for k,v := range b{    
}
//二重map
a := make(map[string]map[int]string)
a["11"] = make(map[int]string)
a["11"][1] = "xx"

a["22"] = make(map[int]string)
a["22"][1] = "tt"

```

### 面向对象

golang基于结构体实现OOP特性

* 定义

```go
type Teacher struct {
	Name string
    Age int
    School string
}
//初始化
func main(){
    //第一种赋值方式
    var x Teacher
    x.Name = "xx"
    //第二种赋值方式
    var ma Teacher = Teacher{"s", 23, "df"}
    //第三种赋值方式
    var t *Teacher = new(Teacher)
    *(t).Name = "fd"
}
```

* 结构体间转化

```go
package main
type Student struct{
    Age int
}
type stu Student

type Person struct{
    Age int
}
func main(){
    var s Student = Student{10}
    var t Person = Person{10}
    s = Student(t);
	
    //type起别名后也要强转
    var ss stu = stu{10}
    s = Student(ss)
}
```

* 结构体的方法
  * 方法名首字母大写，可以被其他包访问
  * 如果一个类型实现了String()方法，则fmt.Println默认调用这个变量的String()进行输出


```go
//声明
type A struct {
    Num int
}
type Person struct{
    Age int
}
//test和A结构体绑定，结构体对象传入为值传递
func (a A) test(){
    fmt.Println(a.Num)
}
//将方法改为地址传递
func (p *Person) test1(){
    (*p).Name = "xx"
}

func main(){
    var a A
    a.test()
}
```

* 函数和方法的区别

主要看需不需要绑定指定类型

对于函数来说，参数类型是什么就要传入什么

对于方法来说，指针和值类型可以混用

```go
type Student struct{
    Name  string
    Age int
}
//方法
func(s Student) test01{
}
//函数
func method01(s Student){
}
```

* 跨包创建结构体实例

如果结构体首字母大写的话，在其他包下可以访问

如果结构体首字母小写的话，使用工厂模式也可以访问

```go
//student.go 中
package model
type student struct {
	Name string
    Age int
}
func NewStudent(n string, a int) *student{
    return &student{n, a}
}
//main.go中
package main
import (
	"fmt"
    "model"
)
func main(){
    s := model.NewStudent("xx", 22)
    fmt.Println(*s)
}
```

#### 封装

person.go

```go
package model
import "fmt"
type person struct {
	Name string
	age int
}

//定义工厂模式的函数，相当于构造函数
func NewPerson(name string) *person{
	return &person{
		Name : name,
	}
}

//定义set和get方法，对age进行封装
func (p *person) SetAge(age int){
	if age > 0 && age < 150{
		p.age = age 
	}
	else{
		fmt.Println("error")
	}
}

func (p *person) GetAge(){
	return p.age
}
```

main.go

```go
package main
import(
	"fmt"
	"demo12/model"
)

func main(){
	p := model.NewPerson("xx")
	p.SetAge(12)
	
}
```



#### 继承

```go
package main
import "fmt"

type Animal struct {
	Age int
	Weight float32
}

func (an *Animal) Shot(){
	fmt.Println("sssss")
}

func (an *Animal) Show(){
	fmt.Println("ddddd")
}

type Cat struct{
	//将匿名结构体嵌入
	Animal 
}

func (c *Cat) Scratch(){
	fmt.Println("qqq")
}
func main(){
	//创建cat实例
	cat := &Cat{}
	cat.Animal.Age = 3 //======>cat.Age = 3
	cat.Animal.Weight = 3.5 //=>cat.Weight = 3.5
	cat.Animal.Shot() //=======>cat.Shot()
	cat.Animal.Show() //=======>cat.Show()
	cat.Scratch()
}
```

注意点：

* 匿名结构体字段访问可以简化

cat.Age会先去找结构体中是否有Age字段，有的话直接使用， 没有的话去找嵌入的结构体类型中的Age（赋值也是如此）

* 虽然有多重继承，但是不建议使用



