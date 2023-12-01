+++

date=2023-11-14T21:26:08-04:00
description="Esmeralda"
featured_image="/images/esmeralda.jpg"
tags=[]
title="golang---4"
categories="golang"

+++



### 数组

```go
//定义：
var a [5]int
//遍历(普通for循环)
for i := 0; i < len(a); i ++{}
//遍历（键值循环）
for key, val := range a{}

//初始化
//第一种
var a[3]int = [3]int{1, 2, 3}
//第二种
var a = [3]int{1, 2, 3}
//第三种
var a = [...]int{1, 2, 3, 4}
//第四种
var a = [...]int{2:66, 0:22, 1:66, 3:54}
```

* 长度属于类型的一部分
* Go数组是值类型，拷贝或者传参时默认值传递

```go
//默认初始化为全0
var arr [2][3]int16
//初始化操作
var arr1 [2][3]int = [2][3]int{{1, 2, 3},{4, 5, 6}}
//遍历
//方式1：普通for循环
for i := 0; i < len(arr); i ++{
    for j:= 0; j < len(arr[i]); i ++{
        
    }
}
//方式2：键值循环
for key, value := range arr{
    for k,v := range value{
	}
}
```



### 切片

```go
//定义切片方式1
var intarr [6]int = [6]int{1, 2, 3, 4, 5, 6}
//索引：从1开始，到3结束（不包含3）
var slice []int = intarr[1:3]

//定义切片方式2：, 参数表示，1、切片类型，2、切片长度，3、切片容量
slice := make([]int, 4, 20)

//定义切片方式3：指定具体数组
slice2 := []int{1, 2, 3}


//切片遍历
//方式1：普通for循环
for i := 0; i < len(slice); i ++{
    
}
//方式2：键值循环
for key, value := range slice{
}

```

切片底层由三部分组成：一个指向底层数组的指针，一个切片的长度，一个切片的容量

* 切片的自动增长

```go
var intarr [6]int = [6]int{1, 2, 3, 4, 5, 6}
var slice []int = inarr[1:4]
slice = append(slice, 9, 3)
```

* 自动增长底层原理

1、底层追加元素时对数组扩容，创建一个新数组，复制元素到新数组中，并在新数组中进行追加

2、slice在append后指向新数组的内存

3、底层的新数组需要通过切片简介维护操作



* 切片的复制

```go
var a[]int = []int{1, 2, 3, 4}
var b[]int = make([]int, 10)
copy(b, a)
```









