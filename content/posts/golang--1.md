

+++

date=2023-11-07T13:48:08-04:00
description="Esmeralda"
featured_image="/images/esmeralda.jpg"
tags=[]
title="golang---1"
categories="golang"

+++

## 风格

1、行注释

2、一行不超过80个字符，使用逗号换行



## 变量和数据类型

### 1、定义变量

​	1、var age int = 18

​	2、var age int    //默认给0

​	3、var age = "18"  //自动判断类型

​	4、age := "boy"  //省略var

​	5、一次性声明

​		var(

​			n9 = 500

​			n10 = "fcv"	

​		)

### 2、字符类型

用byte来存储字符类型

var x byte = 'a'

其底层按照Unicode码存储，ASCII码是Unicode码的前几位

要显示字符，要格式化输出

fmt.printf("%c", x)