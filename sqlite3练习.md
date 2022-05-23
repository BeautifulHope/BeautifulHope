## 1、新建数据库 database
### 方法一：
>基本语法： `sqlite3 DatabaseName.db`

通常情况下，数据库名称在 RDBMS 内应该是唯一的。

### 方法二：
>基本语法： `sqlite>.open test.db`

打开已存在数据库也是用 .open 命令，以上命令如果 test.db 存在则直接会打开，不存在就创建它。

## 2、新建表 Table
### 1.
>基本语法：
```
CREATE TABLE database_name.table_name(
   column1 datatype  PRIMARY KEY(one or more columns),
   column2 datatype,
   column3 datatype,
   .....
   columnN datatype,
);
```
`CREATE TABLE `是告诉数据库系统创建一个新表的关键字。`CREATE TABLE` 语句后跟着表的唯一的名称或标识。您也可以选择指定带有 `table_name` 的 `database_name`。

***
### eg:

创建了一个 COMPANY 表，ID 作为主键，NOT NULL 的约束表示在表中创建纪录时这些字段不能为 NULL：
```
sqlite> CREATE TABLE COMPANY(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT    NOT NULL,
   AGE            INT     NOT NULL,
   ADDRESS        CHAR(50),
   SALARY         REAL
);
```
***
### 2. 查找当前db所有tables
使用 SQLIte 命令中的 `.tables` 命令来验证表是否已成功创建，该命令用于列出附加数据库中的所有表。

### 3. 删除table
>基本用法：`DROP TABLE database_name.table_name;`

***
### eg:
```
sqlite>.tables

COMPANY       test.COMPANY
```
这意味着 COMPANY 表已存在数据库中，接下来让我们把它从数据库中删除，如下：
```
sqlite>DROP TABLE COMPANY;
sqlite>
```
***

## 3、增删改查
### 1、增
INSERT INTO 语句有两种基本语法，如下所示：
>基本语法 1、
```
INSERT INTO TABLE_NAME [(column1, column2, column3,...columnN)]  
VALUES (value1, value2, value3,...valueN);
```
column1, column2,...columnN 是要插入数据的表中的列的名称。

>基本语法 2、

如果要为表中的所有列添加值，您也可以不需要在 SQLite 查询中指定列名称。但要确保值的顺序与列在表中的顺序一致。SQLite 的 INSERT INTO 语法如下
```
INSERT INTO TABLE_NAME VALUES (value1,value2,value3,...valueN);
```
***
### eg:

test.DB->COMPANY.table:
```
sqlite> CREATE TABLE COMPANY(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT    NOT NULL,
   AGE            INT     NOT NULL,
   ADDRESS        CHAR(50),
   SALARY         REAL
);
```
下面第一种语句将在 COMPANY 表中创建六个记录：
```
INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (1, 'Paul', 32, 'California', 20000.00 );

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (2, 'Allen', 25, 'Texas', 15000.00 );

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (3, 'Teddy', 23, 'Norway', 20000.00 );

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (4, 'Mark', 25, 'Rich-Mond ', 65000.00 );

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (5, 'David', 27, 'Texas', 85000.00 );

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (6, 'Kim', 22, 'South-Hall', 45000.00 );
```

使用第二种语法在 COMPANY 表中创建一个记录，如下所示：
```
INSERT INTO COMPANY VALUES (7, 'James', 24, 'Houston', 10000.00 );
```
***
### 2、删
### 3、改
 UPDATE 查询用于修改表中已有的记录。可以使用带有 WHERE 子句的 UPDATE 查询来更新选定行，否则所有的行都会被更新。
 >基本语法：
 ```
UPDATE table_name
SET column1 = value1, column2 = value2...., columnN = valueN
WHERE [condition];
```
>注意：*当不带`WHERE`时会对所有数据生效！！！*
***
eg:
```
sqlite> SELECT * FROM COMPANY;
1|ALLEN|25|NANSHAN|
2|Allen|25|Texas|15000.0
3|Teddy|23|Norway|20000.0
4|Mark|25|Rich-Mond |65000.0
5|David|27|Texas|85000.0
6|kim|37|Texas|99999.0
```
使用`sqlite> UPDATE COMPANY SET SALARY='99999' WHERE NAME='kim';`
再查询`sqlite>SELECT * FROM COMPANY;`
输出：
```
sqlite> SELECT * FROM COMPANY;
1|ALLEN|25|NANSHAN|
2|Allen|25|Texas|15000.0
3|Teddy|23|Norway|20000.0
4|Mark|25|Rich-Mond |65000.0
5|David|27|Texas|85000.0
6|kim|37|Texas|99999.0
```
***
使用 AND 或 OR 运算符来结合 N 个数量的条件;
使用`sqlite> UPDATE COMPANY SET SALARY='99999' AND AGE=33;`
### 4、查
## 4、Other
### 1. 退出： `sqlite>.quit`
### 2. 附加数据库
>基本语法：`ATTACH DATABASE file_name AS database_name;`

如果数据库尚未被创建，上面的命令将创建一个数据库，如果数据库已存在，则把数据库文件名称与逻辑数据库 'Alias-Name' 绑定在一起。
*****
### eg:
如果想附加一个现有的数据库 testDB.db，则 ATTACH DATABASE 语句将如下所示：

`sqlite> ATTACH DATABASE 'testDB.db' as 'TEST';`

使用 SQLite .database 命令来显示附加的数据库。

`sqlite> .database`
seq | name            | file
--- | --------------- | ----------------------
0  |  main            | /home/sqlite/testDB.db
2  |  test            | /home/sqlite/testDB.db

数据库名称 main 和 temp 被保留用于主数据库和存储临时表及其他临时数据对象的数据库。
****


### 3.查看表的结构
“查看整个db中的所有table结构”或者“指定某个表的结构”
#### 1.查看表的结构
`select * from sqlite_master where type="table";`
> 对，就是这样写死的，一个字母都不用改！

输出：
```
sqlite> SELECT * FROM SQLITE_MASTER WHERE TYPE="table";
```
```
table|TEST1|TEST1|2|CREATE TABLE TEST1(
ID INT PRIMARY KEY NOT NULL,
NAME TEXT NOT NULL,
AGE INT NOT NULL,
ADDRESS CHAR(50)
)
table|DEPARTMENT|DEPARTMENT|4|CREATE TABLE DEPARTMENT(
ID INT PRIMARY KEY NOT NULL,
DEPT CHAR(50) NOT NULL,
EMP_ID INT NOT NULL
)
table|COMPANY|COMPANY|6|CREATE TABLE COMPANY(
ID INT PRIMARY KEY NOT NULL,
NAME TEXT NOT NULL,
AGE INT NOT NULL,
ADDRESS CHAR(50),
SALARY REAL
)
sqlite> SELECT * FROM sqlite_master WHERE TYPE="table" AND NAME="DEPARTMENT";
table|DEPARTMENT|DEPARTMENT|4|CREATE TABLE DEPARTMENT(
ID INT PRIMARY KEY NOT NULL,
DEPT CHAR(50) NOT NULL,
EMP_ID INT NOT NULL
)
```
#### 2.查看具体一张表的表结构
`SELECT * FROM sqlite_master WHERE TYPE="table" AND NAME="target_table_name";`
> 对，就是这样写死的，除了NAME，改为目标table的名称，其他一个字母都不用改！
```
sqlite> SELECT * FROM sqlite_master WHERE TYPE="table" AND NAME="DEPARTMENT";
```

```
table|DEPARTMENT|DEPARTMENT|4|CREATE TABLE DEPARTMENT(
ID INT PRIMARY KEY NOT NULL,
DEPT CHAR(50) NOT NULL,
EMP_ID INT NOT NULL
)
```