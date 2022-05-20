
>感谢W3C：https://www.w3school.com.cn/sql/sql_orderby.asp
## 基础阶段
student.sql

|  id   | name  |age|
|  ----  | ----  |----|
| 0  | 张三 |24|
| 1  | 李四 |23|
| 2  | 王五 |23|
| 3  | 赵六 |23|
| 4  | 张三 |20|
| 5  | 李四 |23|

## 1.select
select 列名 from 表名

`select * from Student`       显示Student所有信息
## 2.distinct
`select distinct 列名 from 表名`  显示表中唯一的列名(去除重复)

## 3.where 
`select * from Student where name='张三'`   条件选择where里还能添加 `between 、=、！=、>、<、like 、and、or`等

eg：`select * from Student where age>20`

选择年龄大于20岁的同学

## 4.and和or 把两个或者多个条件连接起来
eg1: `select * from Student where name='张三' and age=20`

eg2: `select * from Student where name='张三' or age=20`
## 5.order by
用来排序的，可以根据字母排序也可根据数字排序,有升序和降序两种 默认为升序，降序加上`DESC`

升序：`select * from Student order by age`根据年龄来从小到大排序

降序：`select * from Student order by age DESC`根据年龄来从大到小排序


>eg：`SELECT name, age FROM Student ORDER BY name DESC, age ASC`
name逆序，age顺序

**DESC**降序，**ASC**顺序

## 6.insert into插入语句。
persion.sql
|  num   | name  |age|
|  ----  | ----  |----|
| 0  | 张三 |24|

语法：
***
### 在指定的列中插入数据: 

>`INSERT INTO 表名称 VALUES (值1, 值2,....)`

eg:
`insert into student values(1,'李五'，25)`
|  num   | name  |age|
|  ----  | ----  |----|
| 0  | 张三 |24|
| 1  | 李五 |25|


### 插入新的列：
>`INSERT INTO table_name (列1, 列2,...) VALUES (值1, 值2,....)`

eg：
`insert into student (name,age) values(赵六,11)`
|  num   | name  |age|
|  ----  | ----  |----|
| 0  | 张三 |24|
| 1  | 李五 |25|
|  | 赵六 |11|

***

`insert into student(name,age) values ('李四',21)`

向student这张表中插入name为李四，age为21的值

## 7.update

update用于更新数据

|  num   | name  |age|
|  ----  | ----  |----|
| 0  | 张三 |24|
| 1  | 李五 |25|
|  | 赵六 |11|

语法：
>`UPDATE 表名称 SET 列名称 = 新值 WHERE 列名称 = 某值`

***
### 更新某一行中的一个列
`update student set num = 3 where name = '赵六'`

|  num   | name  |age|
|  ----  | ----  |----|
| 0  | 张三 |24|
| 1  | 李五 |25|
| 3  | 赵六 |11|

### 更新某一行中的若干列
`update student set num = 2,age = 15 where name = '赵六'`

|  num   | name  |age|
|  ----  | ----  |----|
| 0  | 张三 |24|
| 1  | 李五 |25|
| 2  | 赵六 |15|
***

`update Student set age=22 where name='张三'`
更新name为张三的age为22
### 8.delete
语法
>`DELETE FROM 表名称 WHERE 列名称 = 值`
***
删除表中的行

`delete from Student where name='李四'`

删除所有行
可以在不删除表的情况下删除所有的行。这意味着表的结构、属性和索引都是完整的
`delete from student` or `delete * from student`

|  num   | name  |age|
|  ----  | ----  |----|
***

## 9.创建数据库
`create database Student`

## 10.创建数据库中的表
`create table table_name(id int,name varchar（255）,age int )`

|  id   | name  |age|
|  ----  | ----  |----|
***
### 比较高级的用法：
创建一个叫vebdors的数据表，如果不存在的话

`CREATE TABLE IF NOT EXISTS vendors (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255)
);
`
>参考11.约束

|  id   | name  |
|  ----  | ----  |

***
## 11.约束
创建表的时候还应该对表添加一些约束，例如：主键，是否为空之类的

`create table Student(id int not null auto-creament,name varchar(255) not null, age int ,primary key(id))`
***
#### 1.`PRIMARY KEY`:设置为主键

    **设置主键的作用**：
    作为一条数据的唯一标识，像每个人的身份证一样。

    1）一般加在无意义的字段上，如 i; 

    2）标主键字段的要求：值不重复且值具有唯一性。主键不能为空; 

    3）可以设置“单字段主键”和“多字段主键（复合主键）”，用多个字段确定唯一性; 

    4）primary书写时可省略

#设置单字段主键

`create TABLE if not EXISTS student( id int PRIMARY keyname varcahr(20));`

#设置多字段段主键 

`create TABLE if not EXISTS timez( id int auto_increment, atime year, card char(18), primary key(id,card)`

注意点：当设置复合主键时，如设置2个字段为主键，可以允许2个字段中的某一个值可以与其他值重复，但是不可以2个值均重复，插入值进行验证：

`insert into timez values(“1”,‘2000’,‘11111111111111111’);`

`insert into timez values(“1”,‘2000’,‘11111111111111112’); `#均可添加值成功


#### `AUTO_INCREMENT`:
    1）通过设置主键进行自增长，默认从1开始，每次+1

    2）一个表中只能有1个自增长字段，而且自增长的字段一定配合主键使用，也就是说“被标识为自增长的字段，一定是主键，但是主键不一定是自增长的”自增长只对整数类、整数列有效，对字符串无意义

>Test:

1.创建数据表：
```
create TABLE if not EXISTS timea(
id int PRIMARY key auto_increment, #将id设置为主键，并且为自增长
atime year
)engine=innodb charset = utf8;
```

|  id   | year  |
|  ----  | ----  |

2.插值验证：
```
#向数据库插入值，验证auto_increment
insert into timea values(1,‘2000’); #向timea表中自己给定自增长列的值为1
```
|  id   | year  |
|  ----  | ----  |
|  1  | 2000  |
```
insert into timea（atime ） values(‘2001’); #直接插入atime的值，id自动填充值为2
```
|  id   | year  |
|  ----  | ----  |
|  1  | 2000  |
|  2  | 2001  |
```
insert into timea values(null,‘2002’); #用null也可以，id自动填充3
```
|  id   | year  |
|  ----  | ----  |
|  1  | 2000  |
|  2  | 2001  |
|  3  | 2002  |

```
insert into timea values(default,‘2003’); #用default也可以，id自动填充4
```
|  id   | year  |
|  ----  | ----  |
|  1  | 2000  |
|  2  | 2001  |
|  3  | 2002  |
|  4  | 2003  |

3.修改自增长开始的值:
```
#已创建的表，修改自增长开始的值
alter table timea auto_increment=9; #重新设定了自增长的值，则再次插入数据时，id的值为9，依次插入时+1
insert into timea values(default,‘2009’); #用default也可以，id自动填充4
```
|  id   | year  |
|  ----  | ----  |
|  1  | 2000  |
|  2  | 2001  |
|  3  | 2002  |
|  4  | 2003  |
|  9  | 2009  |

4.开始创建表时，自定义 自增长开始的值`auto_increment=`
```
create TABLE if not EXISTS timea(
id int PRIMARY key auto_increment, #将id设置为主键，并且为自增长
atime year
)engine=innodb auto_increment=8 charset = utf8; #创建表时手动设定了自增长的值为8，则插入数据时，id的值不从1开始，而是8，依次插入时+1

insert into timea values(default,‘3001’); #用default也可以，id自动填充8
```
|  id   | year  |
|  ----  | ----  |
|  8  | 3001  |
***
## 12.删除数据库或表
`droptable Student`

`drop database Student`

清空表中的数据 

`truncate table Student`

没有删除表，只是清空了数据而已

`Alter table Studentadd birth date`

Alter 是用来改变表或数据库的关键字

## 进阶阶段

## 1.top">.top
top用来规定返回的记录的数目
`select top 1 * from Student`  返回Student表的第一条数据

`select top 50 percent * from Student` 返回Student表50%的数据 
## 2.like      
`like`用来指定模式匹配
`select * from Student where name like '张%'`  返回Student表里名字以张开头的数据

    这里介绍一下通配符 % :代表一个或者多个字符
    _ 代表一个字符
    [abc] abc中任一字符(这里类似java的regex)
    [^abc] 或者 [！abc] 不在abc中的任意字符（1个）
    类似正则表达式
## 3.in       
允许在where里规定多个值select * from Student where name in （'张三','李四'）
## 4.between...and
操作符选取了一个范围select * from Student where age between 15 and 30 选取15到30之前包含15的(mysql)不同数据库对这个包含的含义不同
## 5.Alias
用于表的别名

`select name as n , age as a from Student `
## 6.join...on
连接2个或者多个表连接两个表,

需要注意，其中一个表中必须有另外一个表的主键,根据这个主键来连接。
`SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo FROM Persons INNER JOIN Orders ON Persons.Id_P = Orders.Id_P ORDER BY Persons.LastNameinner join = join` 

选取两张表中共同的部分left join 选择左边表中所有部分right join 选择右边表中所有部分full join 选择两张表中所有部分
## 7.Union
合并两张表
前提：两张表有相同数量的列，列的数据类型也必须相似 
## 8.select into
从一个表里选择数据插入到另外一个表里

`select * into Student_backup from Student`