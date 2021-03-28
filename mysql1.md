# MySQL入门系列(1) - 快速了解MySQL

## 了解SQL

一、数据库的好处
1、可以持久化数据到本地
2、结构化查询

二、数据库的常见概念
1、DB：数据库，存储数据的容器
2、DBMS：数据库管理系统，又称为数据库软件或数据库产品，用于创建或管理DB
3、SQL：结构化查询语言，用于和数据库通信的语言，不是某个数据库软件特有的，而是几乎所有的主流数据库软件通用的语言

三、数据库存储数据的特点
1、数据存放到表中，然后表再放到库中
2、一个库中可以有多张表，每张表具有唯一的表名用来标识自己
3、表中有一个或多个列，列又称为“字段”，相当于java中“属性”
4、表中的每一行数据，相当于java中“对象”

5、主键是唯一标识表中每行的一组列（注意：良好的习惯是总是定义主键，以便于以后的数据操作和管理）

四、常见的数据库管理系统
MySQL、oracle、db2、SQLServer

## MySQL数据类型和字段约束

|      数据类型      |   说明   |
| :----------------: | :------: |
|      int, bit      |   整数   |
|      decimal       |   小数   |
|    varchar,char    |  字符串  |
| date,time,datetime | 日期时间 |
|        enum        | 枚举类型 |

|  约束参数   |   说明   |
| :---------: | :------: |
| primary key | 主键约束 |
|  not null   | 非空约束 |
|   unique    | 唯一约束 |
|   default   | 默认约束 |

## MySQL数据库的安装

### MySQL数据库服务端软件的安装

```bash
sudo apt-get install mysql-server
```

### MySQL数据库客户端软件的安装

```bash
sudo apt-get install mysql-client
```

### MySQL数据库服务端启动

```bash
# 查看MySQL服务状态
sudo service mysql status
# 停止MySQL服务
sudo service mysql stop
# 启动MySQL服务
sudo service mysql start
# 重启MySQL服务
sudo service mysql restart
```

## MySQL数据库的配置文件

路径：`/etc/mysql/mysql.conf.d/mysqld.cnf`

配置项介绍：

* port表示端口号
* bind-address表示服务器绑定的ip
* datadir表示数据库保存路径
* log_error表示错误日志，默认为`/var/log/mysql/error.log`

## MySQL终端指令操作

### MySQL登入登出客户端操作

```bash
# 登录mysql root是用户名
mysql -uroot -p

# 显示当前时间
select now();

# 退出连接
exit/quit/ctrl+d
```

### MySQL数据库操作

```bash
# 查看所有数据库
show databases;

# 创建数据库
create database 数据库名 charset=utf8;

# 使用数据库
use 数据库名

# 查看当前使用的数据库
select database();

# 删除数据库
drop database 数据库名;
```

### MySQL表操作

```bash
# 查看当前库中所有表
show tables;

# 创建表
create table 表名(字段名 数据类型 可选的约束条件,...);

# 修改表字段类型
alter table 表名 modify 列名 类型 约束;

# 删除表
drop table 表名;

# 查看表结构
desc 表名;
```

### MySQL-CRUD增删改查操作

```bash
# 查询数据
## 查询所有列
select * from 表名;
## 查询指定列
select 列1,列2,... from 表名;

# 插入数据
## 全列插入:值的顺序必须和字段顺序完全一致
insert into 表名 values(...);
## 部分列插入:值得顺序与给出的列的顺序对应
insert into 表名(列1,...) values(值1,...);
## 全列多行插入
insert into 表名 values(...),(...),(...);
## 部分列多行插入
insert into 表名(列1,...) values(值1,...),(值1,...),(值1,...);

# 修改数据
update 表名 set 列1=值1,列2=值2,... where 条件;
## 例：
update students set age=18,gender='女' where id = 6;

# 删除数据
delete from 表名 where 条件;
## 例：
delete from students where id=5;
```

## MySQL数据库备份和恢复

### MySQL数据-备份导出

```bash
mysqldump -u用户名 -p密码 数据库名字 表名字 > data.sql
```

### MySQL数据-恢复导入

```bash
cd 到数据文件路径下
mysql -u用户名 -p密码
use 数据库
source data.sql
```

## MySQL的语法规范

1. 不区分大小写，但建议关键字大写，表名、列名小写
2. 每条命令最好用分号结尾
3. 每条命令根据需要，可以进行缩进或换行
4. 注释
   - 单行注释：`#注释文字`
   - 单行注释：`--注释文字`
   - 多行注释：`/*注释文字*/`

## Python交互MySQL数据库

### 安装pymysql第三方包

```bash
# 安装pymysql
sudo pip3 install pymysql

# 查看安装情况
pip3 show pymysql

# 卸载pymysql
sudo pip3 uninstall pymysql
```

### pymysql的使用

```python
# 导包
import pymysql

# 创建和mysql服务端的连接对象
connc = pymysql.connect(参数列表)

# 获取游标对象
cursor = connc.cursor()

# 编写sql语句
sql = 'select * from students;'

# 执行sql
row_count = cursor.execute(sql)

# 获取查询结果集
result = cursor.fetchall()

# 将增加和修改操作提交到数据库
connc.commit()

# 回滚数据
connc.rollback()

# 关闭游标对象
cursor.close()

# 关闭连接
connc.close()
```