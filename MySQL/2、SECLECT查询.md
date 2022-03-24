# 1.SQL概述

## 1.1 SQL分类

SQL语言在功能上主要分为如下3大类：

- **DDL（Data Definition Languages、数据定义语言）**，这些语句定义了不同的数据库、表、视图、索 引等数据库对象，还可以用来创建、删除、修改数据库和数据表的结构。 
  - 主要的语句关键字包括 CREATE 、 DROP 、 ALTER 等。
  - *CREATE DATABASE* - 创建新数据库 
  - *ALTER DATABASE* - 修改数据库 
  - *CREATE TABLE* - 创建新表 
  - *ALTER TABLE* - 变更（改变）数据库表 
  - *DROP TABLE* - 删除表 
  - *CREATE INDEX* - 创建索引（搜索键） 
  - *DROP INDEX* - 删除索引 
- **DML（Data Manipulation Language、数据操作语言）**，用于添加、删除、更新和查询数据库记 录，并检查数据完整性。 
  - 主要的语句关键字包括 INSERT INTO、 DELETE 、 UPDATE 、 SELECT 等。
  -  SELECT是SQL语言的基础，最为重要。 
- **DCL（Data Control Language、数据控制语言）**，用于定义数据库、表、字段、用户的访问权限和 安全级别。
  - 主要的语句关键字包括 GRANT 、 REVOKE 、 COMMIT 、 ROLLBACK 、 SAVEPOINT 等。

# 2. SQL的语言规范

## 2.1 基本规则

- SQL 可以写在一行或者多行。为了提高可读性，各子句分行写，必要时使用缩进
- 每条命令以 ; 或 \g 或 \G 结束 
- 关键字不能被缩写也不能分行
- 关于标点符号 
  - 必须保证所有的()、单引号、双引号是成对结束的 
  - 必须使用英文状态下的半角输入方式 
  - 字符串型和日期时间类型的数据可以使用单引号（' '）表示 
  - 列的别名，尽量使用双引号（" "），而且不建议省略as

## 2.2 SQL大小写规范

- MySQL 在 Windows 环境下是大小写不敏感的 
- MySQL 在 Linux 环境下是大小写敏感的 
  - 数据库名、表名、表的别名、变量名是严格区分大小写的 
  - 关键字、函数名、列名(或字段名)、列的别名(字段的别名) 是忽略大小写的。 
- 推荐采用统一的书写规范： 
  - 数据库名、表名、表别名、字段名、字段别名等都小写 
  - SQL 关键字、函数名、绑定变量等都大写

## 2.3 注释

单行注释：#注释文字(MySQL特有的方式) 

单行注释：-- 注释文字(--后面必须包含一个空格。) 

多行注释：/* 注释文字 */

## 2.4 数据导入

mysql> source d:\mysqldb.sql

# 3 基本的SELECT语句

## 3.0 SELECT

SELECT 1; #没有任何子句

SELECT 9/2; #没有任何子句

## 3.1 SELECT...FROM

基本语法：

- SELECT 标识选择哪些列

- FROM 标识从哪个表中选择

选择全部列：

- SELECT *
- FROM departments;

选择特定的列：

- SELECT department_id, location_id
- FROM departments;

## 3.2 列的别名

```sql
SELECT last_name AS name, commission_pct comm 
FROM employees;

SELECT last_name "Name", salary*12 "Annual Salary"
FROM employees;
```

## 3.3 去除重复行

```sql
SELECT DISTINCT department_id
FROM employees;
-- DISTINCT 关键字可以去重
```

注意：去重时仅去除一个列，如果两个的话效果不明显

## 3.4 空值参与运算

- 所有运算符或列值遇到null值，运算的结果都为null

```sql
SELECT employee_id,salary,commission_pct,
12 * salary * (1 + commission_pct) "annual_sal"
FROM employees;
-- 如果需要将null视作0，需要加(1 + IFNULL(commission_pct,0))
```

## 3.5 着重号

SQL 中的 SELECT 语法的确提供了这个功能，一般来说我们只从一个表中查询数据，通常不需要增加一个 固定的常数列，但如果我们想整合不同的数据源，用常数列作为这个表的标记，就需要查询常数。 比如说，我们想对 employees 数据表中的员工姓名进行查询，同时增加一列字段 corporation ，这个 字段固定值为“尚硅谷”，可以这样写：

SELECT '尚硅谷' , last_name FROM employees;

## 3.6 显示表结构

```sql
DESCRIBE employees;
或
DESC employees;
```

![image-20220319121109391](D:\编程学习\笔记\MySQL\image-20220319121109391.png)

# 4 使用WHERE过滤

基本语法：

```sql
SELECT 字段1,字段2
FROM 表名
WHERE 过滤条件
-- WHERE子句紧随 FROM子句
SELECT employee_id, last_name, job_id, department_id
FROM employees
WHERE department_id = 90 ;
```

# 5 运算符

## 5.1 算术运算符

![image-20220319124557647](D:\编程学习\笔记\MySQL\image-20220319124557647.png)

加减法：

- 一个整数类型的值对整数进行加法和减法操作，结果还是一个整数；
- 一个整数类型的值对浮点数进行加法和减法操作，结果是一个浮点数； 
- 加法和减法的优先级相同，进行先加后减操作与进行先减后加操作的结果是一样的； 
- 在Java中，+的左右两边如果有字符串，那么表示字符串的拼接。
- 但是在MySQL中+只表示数 值相加。如果遇到非数值类型，先尝试转成数值，如果转失败，就按0计算。（补充：MySQL 中字符串拼接要使用字符串函数CONCAT()实现）

乘除法：

- 一个数乘以整数1和除以整数1后仍得原数； 
- 一个数乘以浮点数1和除以浮点数1后变成浮点数，数值与原数相等； 
- 一个数除以整数后，不管是否能除尽，结果都为一个浮点数； 
- 一个数除以另一个数，除不尽时，结果为一个浮点数，并保留到小数点后4位； 
- 乘法和除法的优先级相同，进行先乘后除操作与先除后乘操作，得出的结果相同。 在数学运算中，0不能用作除数，在MySQL中，一个数除以0为NULL。

## 5.2 比较运算符

![image-20220319124824244](D:\编程学习\笔记\MySQL\image-20220319124824244.png)

- 如果等号两边的值、字符串或表达式都为字符串，则MySQL会按照字符串进行比较，其比较的 是每个字符串中字符的ANSI编码是否相等。 
- 如果等号两边的值都是整数，则MySQL会按照整数来比较两个值的大小。 
- 如果等号两边的值一个是整数，另一个是字符串，则MySQL会将字符串转化为数字进行比较。 
- 如果等号两边的值、字符串或表达式中有一个为NULL，则比较结果为NULL。



![image-20220319141927529](D:\编程学习\笔记\MySQL\image-20220319141927529.png)



![image-20220319142412236](D:\编程学习\笔记\MySQL\image-20220319142412236.png)



![image-20220319142435084](D:\编程学习\笔记\MySQL\image-20220319142435084.png)

![image-20220319142624873](D:\编程学习\笔记\MySQL\image-20220319142624873.png)

### 5.3.1 **拓展：使用正则表达式查询**

![image-20220319144201738](D:\编程学习\笔记\MySQL\image-20220319144201738.png)

![image-20220319144223574](D:\编程学习\笔记\MySQL\image-20220319144223574.png)

![image-20220319144230054](D:\编程学习\笔记\MySQL\image-20220319144230054.png)

## 5.3 逻辑运算符

![image-20220319142657161](D:\编程学习\笔记\MySQL\image-20220319142657161.png)



## 5.4 位运算符

![image-20220319142718472](D:\编程学习\笔记\MySQL\image-20220319142718472.png)



# 6 排序与分页

## 6.1 排序规则

- 使用 ORDER BY 子句排序
  - ASC（ascend）: 升序 
  - DESC（descend）:降序
- ORDER BY 子句在SELECT语句的结尾。

## 6.2 单列排序

```sql
SELECT last_name, job_id, department_id, hire_date
FROM employees
ORDER BY hire_date ;
-- 默认为升序排序
SELECT last_name, job_id, department_id, hire_date
FROM employees
ORDER BY hire_date DESC ;
```

- 注：列的别名只能在ORDER BY 中使用，不能使用在WHERE

## 6.3 多列排序

```sql
SELECT last_name, department_id, salary
FROM employees
ORDER BY department_id, salary DESC;
-- 默认为第一个，若相同则比较第二个
```

## 6.4 分页

- MySQL中使用 LIMIT 实现分页

- ```sql
  LIMIT [位置偏移量,] 行数
  
  --前10条记录：
  SELECT * FROM 表名 LIMIT 0,10;
  或者
  SELECT * FROM 表名 LIMIT 10;
  --第11至20条记录：
  SELECT * FROM 表名 LIMIT 10,10;
  --第21至30条记录：
  SELECT * FROM 表名 LIMIT 20,10;
  ```

- 分页显式公式：（当前页数-1）*每页条数，每页条数

- ```sql
  SELECT * FROM table
  LIMIT(PageNo - 1)*PageSize,PageSize;
  注意：LIMIT 子句必须放在整个SELECT语句的最后！
  ```

- 约束返回结果的数量可以 减少数据表的网络传输量 ，也可以 提升查询效率 。如果我们知道返回结果只有 1 条，就可以使用 LIMIT 1 ，告诉 SELECT 语句只需要返回一条记录即可。这样的好处就是 SELECT 不需 要扫描完整的表，只需要检索到一条符合条件的记录即可返回。

## 6.5 WHERE ... ORDER BY ... LIMIT声明顺序

1. WHERE
2. ORDER BY
3. LIMIT



# 7 多表查询

- 多表查询，也称为关联查询，指两个或更多个表一起完成查询操作。 
- 前提条件：这些一起查询的表之间是有关系的（一对一、一对多），它们之间一定是有关联字段，这个 关联字段可能建立了外键，也可能没有建立外键。比如：员工表和部门表，这两个表依靠“部门编号”进 行关联。

![image-20220319190144525](D:\编程学习\笔记\MySQL\image-20220319190144525.png)

## 7.1 笛卡尔积

笛卡尔乘积是一个数学运算。假设我有两个集合 X 和 Y，那么 X 和 Y 的笛卡尔积就是 X 和 Y 的所有可能 组合，也就是第一个对象来自于 X，第二个对象来自于 Y 的所有可能。组合的个数即为两个集合中元素 个数的乘积数。

![image-20220319221316098](D:\编程学习\笔记\MySQL\image-20220319221316098.png)

- SQL92中，笛卡尔积也称为 交叉连接 ，英文是 CROSS JOIN 。在 SQL99 中也是使用 CROSS JOIN表示交 叉连接。它的作用就是可以把任意表进行连接，即使这两张表不相关。在MySQL中如下情况会出现笛卡 尔积：

```sql
#查询员工姓名和所在部门名称
SELECT last_name,department_name FROM employees,departments;
SELECT last_name,department_name FROM employees CROSS JOIN departments;
SELECT last_name,department_name FROM employees INNER JOIN departments;
SELECT last_name,department_name FROM employees JOIN departments;
```

- 笛卡尔积的错误会在下面条件下产生：
  - **省略多个表的连接条件（或关联条件）** 
  - 连接条件（或关联条件）无效 
  - 所有表中的所有行互相连接

- 为了避免笛卡尔积， 可以在 WHERE 加入有效的连接条件。
- 加入连接条件后，查询语法：

```SQL
SELECT table1.column, table2.column
FROM table1, table2
WHERE table1.column1 = table2.column2; #连接条件
```

- 在 WHERE子句中写入连接条件。

## 7.2 正确写法

- 正确写法：

- ```SQL
  #案例：查询员工的姓名及其部门名称
  SELECT last_name, department_name
  FROM employees, departments
  WHERE employees.department_id = departments.department_id;
  ```

建议：在多表查询时，每个字段前都指明其所在的表

​			在定义了表的别名后，在WHERE和SELECT中必须使用别名

```sql
SELECT e.employee_id,e.first_name,d.department_name,d.department_id,l.location_id
FROM employees e,departments d,locations l
WHERE e.department_id = d.department_id
AND d.location_id = l.location_id;
-- 多表查询时，需要有变量相互连接
```



## 7.3 多表查询的分类

### 7.3.1 等值连接&非等值连接

等值连接：（之前的是等值）

![image-20220320162747841](D:\编程学习\笔记\MySQL\image-20220320162747841.png)

非等值连接：

![image-20220320163859937](D:\编程学习\笔记\MySQL\image-20220320163859937.png)

```sql
SELECT e.last_name,e.salary,j.grade_level
FROM employees e,job_grades j
WHERE e.salary BETWEEN j.lowest_sal AND j.highest_sal;
```

### 7.3.2 自连接&非自连接

自连接：在同一张表的操作

![image-20220320164917302](D:\编程学习\笔记\MySQL\image-20220320164917302.png)

```sql
SELECT emp.first_name,emp.employee_id,mgr.first_name,mgr.employee_id
FROM employees emp,employees mgr
WHERE emp.manager_id = mgr.employee_id;
```

### 7.3.3 内连接&外连接

内连接：以上

```sql
SELECT last_name, department_name
FROM employees, departments
WHERE employees.department_id = departments.department_id;
-- 只是把满足条件的行显示出来
```

外连接：合并具有同一列的两个以上的表的行，结果包含出了匹配的行以外，还包含左表或右表不匹配的行

- 两个表在连接过程中除了返回满足连接条件的行以外还返回左（或右）表中不满足条件的 行 ，这种连接称为左（或右） 外连接。没有匹配的行时, 结果表中相应的列为空(NULL)。

外连接的分类：

- 左外连接：查出左表中不满足的项
  - 如果是左外连接，则连接条件中左边的表也称为 主表 ，右边的表称为 从表 。 
- 右外连接：查出右表中不满足的项
  - 如果是右外连接，则连接条件中右边的表也称为 主表 ，左边的表称为 从表 。
- 满外连接

## 7.4 SQL99实现多表查询

sql99使用JOIN...ON语法实现

### 7.4.1 实现内连接：

```sql
SELECT 字段列表
FROM A表 INNER JOIN B表
ON 关联条件
WHERE 等其他子句;

SELECT last_name,department_name,city
from employees e 
JOIN departments d
ON e.department_id = d.department_id
JOIN locations l
ON d.location_id = l.location_id;
```

### 7.4.2 实现外连接

- 左外连接：(LEFT OUTER JOIN)
- 语法：

```sql
#实现查询结果是A
SELECT 字段列表
FROM A表 LEFT JOIN B表
ON 关联条件
WHERE 等其他子句;
```

- 举例:

```sql
SELECT last_name,department_name
FROM employees e
LEFT OUTER JOIN departments d
ON e.department_id  = d.department_id;
```



- 右外连接(RIGHT OUTER JOIN)
- 语法：

```sql
#实现查询结果是A
SELECT 字段列表
FROM A表 RIGHT JOIN B表
ON 关联条件
WHERE 等其他子句;
```

- 举例：

```sql
SELECT e.last_name, e.department_id, d.department_name
FROM employees e
RIGHT OUTER JOIN departments d
ON (e.department_id = d.department_id) ;
```



-  满外连接(FULL OUTER JOIN)
  - 满外连接的结果 = 左右表匹配的数据 + 左表没有匹配到的数据 + 右表没有匹配到的数据。 
  - SQL99是支持满外连接的。使用FULL JOIN 或 FULL OUTER JOIN来实现。 
  - 需要注意的是，MySQL不支持FULL JOIN，但是可以用 LEFT JOIN UNION RIGHT join代替。



### 7种SQL JOINS的实现

![image-20220320220105200](D:\编程学习\笔记\MySQL\image-20220320220105200.png)



```sql
#中图：内连接 A∩B
SELECT employee_id,last_name,department_name
FROM employees e JOIN departments d
ON e.`department_id` = d.`department_id`;

#左上图：左外连接
SELECT employee_id,last_name,department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`;

#右上图：右外连接
SELECT employee_id,last_name,department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`;

#左中图：A - A∩B
SELECT employee_id,last_name,department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE d.`department_id` IS NULL

#右中图：B-A∩B
SELECT employee_id,last_name,department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE e.`department_id` IS NULL

#左下图
SELECT employee_id,last_name,department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE d.`department_id` IS NULL
UNION ALL #没有去重操作，效率高
SELECT employee_id,last_name,department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`;

#右下图
#左中图 + 右中图 A ∪B- A∩B 或者 (A - A∩B) ∪ （B - A∩B）
SELECT employee_id,last_name,department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE d.`department_id` IS NULL
UNION ALL
SELECT employee_id,last_name,department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE e.`department_id` IS NULL

```



### 7.4.3 自然连接

SQL99 在 SQL92 的基础上提供了一些特殊语法，比如 NATURAL JOIN 用来表示自然连接。我们可以把 自然连接理解为 SQL92 中的等值连接。它会帮你自动查询两张连接表中 所有相同的字段 ，然后进行 等值 连接 。

```sql
SELECT employee_id,last_name,department_name
FROM employees e NATURAL JOIN departments d
```



### 7.4.4 USING连接

当我们进行连接的时候，SQL99还支持使用 USING 指定数据表里的 同名字段 进行等值连接。但是只能配 合JOIN一起使用。比如：

```sql
SELECT employee_id,last_name,department_name
FROM employees e JOIN departments d
USING (department_id);
```

























