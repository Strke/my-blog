## 基本select语句

### SELECT基本语句

```sql
SELECT employees_id, salary
from  employees;
```

##### 起别名

三种方式，空格、AS、使用引号

* 别名只能在ORDER BY 中使用，不能再WHERE中使用

```sql
SELECT employee_id emp_id, last_name AS lname, name_id "nm_id"
FROM employees;
```

##### 去除重复行

```sql
SELECT DISTINCT department_id
from  employees;
```

##### 空值

空值在数据库中用null表示，空值参与运算，结果一定为空值

使用IFNULL来进行替换空值从而避免空值运算

```sql
SELECT 	salary*(1 + IFNULL(commission_pct, 0)) * 12
FROM employees;
```

##### 着重号``

避免由于表名和关键字相同

```SQL
SELECT * FROM `order`;
```

##### 显示表结构

```sql
DESCRIBE employees;
DESC employees;
```

### 过滤数据

```SQL
SELECT 字段1, 字段2
FROM employees
WHERE department_id = 90;
```



## 运算符

### 比较运算符

##### 等于 =

当用来判断是否为NULL时，结果永远为null

##### 安全等于<=>

可以用来对NULL做判断

##### LEAST() \ GREATEST

```SQL
SELECT LEAST(first_name, last_name)
FROM employees;
```

##### BETWEEN

BETWEEN包含边界

```SQL
SELECT employee_id, last_name
FROM employees
WHERE salary BETWEEN 6000 AND 8000
```

##### IN / NOT IN

```SQL
SELECT last_name, salary, department_id
FROM employees
WHERE department_id NOT IN (10, 20, 30)
```

##### 模糊查询 

%：表示不确定个数的字符

_：表示一个不确定的字符

\：转义字符

```SQL
SELECT last_name
FROM employees
WHERE last_name LIKE '%a%';
```

##### REGEXP运算符匹配字符串

如果匹配上了，返回1，否则返回0，如果匹配对象和匹配条件其中一个为null，则返回null。

```SQL
SELECT 'shkstart' REGEXP '^s' , 'shkstart' REGEXP 't$', 'shkstart' REGEXP 'hk'
```



## 排序与分页

### 1、排序

使用ORDER BY对查询的数据进行排序，默认升序

升序：ASC

降序：DESC

```SQL
SELECT employee_id, last_name, salary
FROM employees
WHERE department_id IN (50, 60, 70)
ORDER BY salary DESC;
```

SQL语句执行顺序

* 先FROM 和WHERE
* 再SELECT
* 最后ORDER BY

所以SELECT中起别名在WHERE中没用，在ORDER BY 中有用

##### 二级排序

按照department_id降序排序，salary升序排序

```SQL
SELECT employee_id, salary, department_id
FROM employees
ORDER BY department_id, salary ASC;
```

### 2、分页

格式

```SQL
LIMIT 位置偏移量 条目数
LIMIT 10 # 省略了0，表示第一页，一页10个条目
LIMIT 2 OFFSET 31 # 8.0新特性，表示显示显示2条记录，偏移量为31
```

每页显示20条记录，此时显示第一页

```sql
SELECT employee_id, last_name
FROM employees
LIMIT 0, 20
```

每页显示20条记录，此时显示第二页

```SQL
SELECT employee_id, last_name
FROM employees
LIMIT 20, 20;
```



