## 数据库
数据 ：能够输入计算机并能被计算机程序识别和处理的信息集合。

数据库（Database）：是按照数据结构来组织、存储和管理数据的仓库。数据库是在数据库管理系统管理和控制之下，存放在存储介质上的数据集合。

数据库管理系统：数据库管理系统是一种操纵和管理数据库的大型软件，用于建立、使用和维护数据库，简称DBMS。它对数据库进行统一的管理 和控制，以保证数据库的安全性和完整性。一般支持数据库定义功能，数据库操纵功能，数据库运行控制功能，数据通信功能，支持存取海量数据等

数据库系统（DBS）：它和数据库不是一个概念，数据库系统比数据库大的多，由数据库、数据库管理系统、应用开发工具构成。常见的数据库系统：甲骨文的Oracle、IBM的DB2、微软的SQLServer、开源的MySQL等。

##MySql数据库的安装
并不是所有的系统自来就有mysql，如果你想使用mysql数据库需要进行安装。下面以ubuntu为例进行安装，如果是windows下可以去Oracle的mysql官方页面下载自己喜欢的版本安装。最近的版本已经是5.6版本。
[mysql官网](http://www.mysql.com/%20mysql%E5%AE%98%E7%BD%91)

```
1． Sudo apt-get update

2． Sudo apt-get install mysql-server
```
安装过程会自动弹出配置界面

安装mysql的服务端后，可以对数据库进行后端操作。还可以安装个客户端用来访问数据库

```
sudo apt-get install mysql-client
```


安装完成后通过mysql –u root -p 然后输入密码可以进入数据库了。

如果想用python对mysql数据库进行操作不像sqlite那么简单，还需要安装python的MySQLdb 进行支持，实际这是一个接口程序，没安装前导入模块失败。

MySQLdb可以通过源码安装或者apt-get直接安装

```
sudo apt-get install python-mysqldb
```


安装以后可以在python中导入MySQLdb模块，如果导入成功基本就没有什么问题了。安装过程会有一些自动的路径搜索，如果你的操作系统中不同目录下安装了很多不同版本那么可能只有一个能用，需要你进行配置。当然安装方法还有很多，只要最终能够达到目的就可以了。

### 常用命令
help : 查看命令帮助

quit 或 exit： 退出mysql操作

show databases ： 显示数据库文件列表

use [database]：打开数据库

show tables ：显示数据库中所有表名

desc ``` : 查看表的结构

在sql中关键字是不区分大小写的，操作内容区分大小写。

### 语句主要可以分为以下三类
DDL语句（数据定义）
这些语句定义了不同的数据段、数据库、表、列、索引等数据库对象。常用的关键字包括create、drop、alter等

DML语句（数据操纵）
用于添加、删除、更新和查询数据记录，并检查数据完整性。常用关键字包括insert、delete、update、和select的等。

DCL语句（数据控制）

用于控制不同数据段直接的许可和访问级别的语句。这些语句定义了数据库、表、字段、用户的访问权限和安全级别。主要的语句关键字有grant、revoke等。

### sql数据定义
创建一个数据库

create database [databasename]

创建后MySql在linux下默认的数据文件存储目录为/var/lib/mysql

删除一个数据库

drop database [databasename]

创建一个表：

create table tabname(col1 type1 [not null] [primary key],col2 type2 [not null],..)

这个命令中tabname为表名后面col为字段名称，type1为字段数据类型，后面是约束条件。

```
runoob_tbl(
   runoob_id INT NOT NULL AUTO_INCREMENT,
   runoob_title VARCHAR(100) NOT NULL,
   runoob_author VARCHAR(40) NOT NULL,
   submission_date DATE,
   PRIMARY KEY ( runoob_id )
);
```
如果你不想字段为NULL可以设置字段的属性为NOT NULL，在操作数据库时如果输入该字段的数据为NULL，就会报错。

AUTO_INCREMENT定义列为自增的属性，一般用于主键，数值会自动加1。

PRIMARY KEY关键字用于定义列为主键。可以使用多列来定义主键，列间以逗号分隔。
删除一个表：

drop table [table_name]

修改数据表字段类型

alter table [tablename] modify [field] [new_type] [first | after]

增加表字段

alter table [tablename] add column ([field] [type], ...) [first | after]

删除表字段

alter table [tablename] drop column [field];

字段改名

alter table [tablename] change <old_field> <new_field> [type] [first | after]

修改默认值

ALTER TABLE [tablename] ALTER [field] SET DEFAULT 1000;

ALTER TABLE [tablename] ALTER [field] DROP DEFAULT;

更改表名

alter table [tablename] rename [to] [new_tablename]

### sql数据操作
插入记录

insert into [table_name] (field1,field2....) values (value1, value2,…);

如果不指定类型那么默认需要按照表的字段顺序来写值

更新记录

update [table_name] set [f1=value1], [f2=value2] … where [expression];

删除记录

delete from [table_name] where [expression]

查询记录

select * from [table_name] where [expression]

“*”代表所有记录，也可以用都好分割的方式选定字段来代替。

条件查询

select * from [table_name] where [expression]

在条件中可以使用个= > < >= <= != 等比较运算符，还可以使用and，or等逻辑运算符。

like也可以进行字符串比较, '%' 代表多个字符,   '_'代表一个字符。
### Mysql的python数据库接口
mysqldb

#### 建立连接
要与数据库进行通信, 必须先和数据库建立连接。连接对象处理命令如何送往服务器, 以及如 何从服务器接收数据等基础功能。连接成功(或一个连接池)后你就能够向数据库服务器发送请求, 得到响应。
####常用方法
connect()              创建数据库连接
close()                关闭数据库连接
commit()               提交当前事务
rollback()             取消当前事务
cursor()               使用这个连接创建并返回一个游标或类游标的对象

connect()调用：注意不是所有的接口程序都是严格按照规范实现的。MySQLdb 就使用了 db 参 数而不是规范推荐的 database 参数来表示要访问的数据库。
一旦执行了close()方法，再试图使用连接对象的方法将会导致异常。对不支持事务的数据库或者虽然支持事务，但设置了自动提交(auto- commit)的数据库系统来说，commit()方法什么也不做。如果你确实需要，可以实现一个自定义方法来关闭自动提交行为。由于 DB-API 要求必须实现此方法，对那些没有事务概念的数据库来说，这个方法只需要有一条pass语句就可以了。类似 commit()，rollback()方法仅对支持事务的数据库有意义。执行完 rollback()，数据库将恢复到提交事务前的状态。根据 PEP249，在提交 commit()之前关闭数据库连接将会自动调用rollback()方法。对不支持游标的数据库来说,  cursor()方法仍然会返回一个尽量模仿游标对象的对象。这些是最低要求。特定数据库接口程序的开发者可以任意为他们的接口程序添加额外的属性，只要 他们愿意。DB-API 规范建议但不强制接口程序的开发者为所有数据库接口模块编写异常类。如果没有提供异常类，则假定该连接对象会引发一致的模块级异常。一旦你完成了数据库连 接，并且关闭了游标对象，你应该执行 commit() 提交你的操作，然后关闭这个连接。
### 游标对象操作
当你建立连接之后，就可以与数据库进行交互。我们提交一个游标允许用户执行数据库命令和得到查询结果。一个 Python DB-API 游标对象总是扮演游标的角色, 无论数据库是否真正支持游标。从这一点讲, 数据库接口程序必须实现游标对象。只有这样，才能保证无论使用何种后端数据库你的代码都不需要做任何改变。创建游标对象之后，你就可以执行查询或其它命令 (或者多个查询和多个命令), 也可以从结果集中取出一条或多条记录。
#### 游标属性和方法
arraysize 使用 fechmany()方法一次取出多少条记录, 默认值为 1
connectionn 创建此游标对象的连接(可选)
description 返回游标活动状态(一个包含七个元素的元组): (name, type_code,display_size, internal_ size, precision, scale, null_ok); 只有 name 和 type_code 是必须提供的
lastrowid 返回最后更新行的 id (可选), 如果数据库不支持行 id, 默认返回 None)
rowcount 最后一次 execute() 操作返回或影响的行数
callproc(func[,args]) 调用一个存储过程
**close() 关闭游标对象**
**execute(op[,args]) 执行一个数据库查询或命令**
executemany(op,args) 类似 execute() 和 map() 的结合, 为给定的每一个参数准备并执行一个数据库查询/命令
**fetchone() 得到结果集的下一行**
**fetchmany([size=cursor.arraysize]) 得到结果集的下几行 (几 = size)**
**fetchall() 返回结果集中剩下的所有行**
__iter__() 创建一个迭代对象 (可选; 参阅 next())
messages 游标执行后数据库返回的信息列表 (元组集合) (可选)
next() 使用迭代对象得到结果集的下一行(可选; 类似 fetchone(), 参阅 __iter__())
nextset() 移到下一个结果集 (如果支持的话)
rownumber 当前结果集中游标的索引 (以行为单位, 从 0 开始) (可选)
setinput- sizes(sizes) 设置输入最大值 (必须有, 但具体实现是可选的)
setoutput- size(size[,col]) 设置大列的缓冲区大写(必须有, 但具体实现是可选的)

在这众多游标对象中最重要的属性是 execute() 和 fetch()方法。所有对数据库服务器的请求都由它们来完成。对 fetchmany()方法来说, 设置一个合理的 arraysize属性会很有用。当然，在不需要时关掉游标对象也是个好主意

#### python使用mysql数据库的一些基本操作

```
#! /usr/bin/python
#coding=utf-8
import MySQLdb
#打开数据库连接
db = MySQLdb.connect('localhost','root','5560442','testdb')
#创建游标，用来通过sql语句操作具体数据
cursor = db.cursor()
#通过游标对象执行sql语句
cursor.execute("select age,sex from emp where name = 'gt'")
data = cursor.fetchone()
print data
#---------------
#创建表
#cursor.execute("drop table if exists newtab")
#sql = "create table newtab(id int not null,name varchar(20),age int,income float)"
#cursor.execute(sql)
#---------------
sql = "select * from newtab"
cursor.execute(sql)
id1 = cursor.fetchall()[0][0]#这个返回的是一个查询出来的结果组成元组
print id1
name = cursor.description[1][0]#返回一个由（每个属性的七个元素组成的元组）的元组，元组里面有元组
print name
for i in cursor.description:
    print i[0],
sql1 = "update newtab set name = 'gt' where id = '"+str(id1)+"'"
try:
    cursor.execute(sql1)
    db.commit()
except:
    db.rollback()#回滚到执行前的状态
try:
    cursor.execute("insert into qq values('"+id1+"','"+name+"')")
    db.commit()
except:
    db.rollback()
db.close()

```

#### 题目 ： 完成一个学生成绩管理系统

实验要求 ：

使用python和mysql数据库完成该实验

基本功能应该包含学生信息和分数的增删改查操作

自己创建数据库和表

程序有简单的命令提示，代码具有一定的封装性和可扩建型

#### 实现代码

```
#! /usr/bin/python
#coding=utf-8
import MySQLdb
def insert(db):
    cursor = db.cursor()
    name = raw_input("姓名：")
    age = raw_input("年龄：")
    mark = raw_input("成绩：")
    try:
        sql = "insert into soure values('%s','%s','%s')"%(name,age,mark)
        cursor.execute(sql)
        db.commit()
        print "插入成功！"
    except:
        print "插入失败！"
        db.rollback()
def select(db):
    cursor = db.cursor()
    sql = "select * from soure "
    cursor.execute(sql)
    for i in cursor.description:
        print i[0],
    print ""
    for row in cursor.fetchall():
        print row[0],row[1],row[2]
def selectone(db,name):
    cursor = db.cursor()
    sql = "select * from soure where name='%s'"%name
    cursor.execute(sql)
    for i in cursor.description:
        print i[0],
    print ""
    for row in cursor.fetchall():
        print row[0],row[1],row[2]
def delete(db):
    cursor = db.cursor()
    sql = "select * from soure "
    cursor.execute(sql)
    for i in cursor.description:
        print i[0],
    print ""
    for row in cursor.fetchall():
        print row[0],row[1],row[2]
    name = raw_input("输入要删除学生姓名：")
    sql = "delete from soure where name='%s'"%name
    try:
        cursor.execute(sql)
        db.commit()
        print "删除成功！"
    except:
        print "删除失败！"
        db.rollback()
def update(db):
    cursor = db.cursor()
    sql = "select * from soure "
    cursor.execute(sql)
    for i in cursor.description:
        print i[0],
    print ""
    for row in cursor.fetchall():
        print row[0],row[1],row[2]
    name = raw_input("输入修改的学生姓名：")
    mark = raw_input("输入修改的成绩：")
    sql = "update soure set mark = '%s' where name = '%s'"%(mark,name)
    try:
        cursor.execute(sql)
        db.commit()
        print "修改成功！"
    except:
        print "修改失败！"
        bd.rollback()
def updateone(db,name):
    cursor = db.cursor()
    sql = "select * from soure where name='%s'"%name
    cursor.execute(sql)
    for i in cursor.description:
        print i[0],
    print ""
    for row in cursor.fetchall():
        print row[0],row[1],row[2]
    age = raw_input("要修改的年龄")
    sql1 = "update soure set age = '%s' where name = '%s'"%(age,name)
    try:
        cursor.execute(sql1)
        db.commit()
        print "修改成功！"
    except:
        print "修改失败！"
        db.rollback()
def updatepw(db,name):
    cursor = db.cursor()
    pas = raw_input("输入旧密码：")
    sql1 = "select password from users where password = '%s'"%(pas)
    cursor.execute(sql1)
    data = cursor.fetchone()
    if data != None:
        new_pas = raw_input("新密码：")
        new_pas1 = raw_input("确认新密码：")
        if new_pas == new_pas1:
            sql = "update users set password = '%s' where username = '%s'"%(new_pas,name)
            try:
                cursor.execute(sql)
                db.commit()
                print "修改密码成功,请重新登陆！"
                return main()
            except:
                print "失败！"
                db.rollback()
def register(db,n,pw):
    cursor = db.cursor()
    sql2 = "select username from users where username = '%s'"%n
    cursor.execute(sql2)
    data = cursor.fetchone()
    if data == None:
        sql = "insert into users(username,password) values('%s','%s')"%(n,pw)
        sql1 = "insert into soure(name) values('%s')"%(n)
        try:
            cursor.execute(sql)
            cursor.execute(sql1)
            db.commit()
            print "注册成功！"
        except:
            print "register error"
            db.rollback()
    else:
        print "用户名已存在！"
        return main()
def main():
    db = MySQLdb.connect('localhost','root','5560442','student')
    while True:
        print "------menu------"
        print "----register----"
        print "----loginon-----"
        print "----quit--------"
        order = raw_input(">>>")
        if order == "register":
            username = raw_input("请输入用户名：")
            password = raw_input("请输入密码：")
            register(db,username,password)
        elif order == "loginon":
                loginname = raw_input("请输入登陆名：")
                loginpass = raw_input("请输入密码：")
                cursor = db.cursor()
                sql = "select username,password,biaozhi from users where username='%s' and password='%s'"%(loginname,loginpass)
                try:
                    cursor.execute(sql)
                    data = cursor.fetchone()[2]
                    while True:
                        if data == 1:
                            print "--manager menu--"
                            print "----1.select----"
                            print "----2.insert----"
                            print "----3.update----"
                            print "----4.delete----"
                            print "----5.quit------"
                            operate = raw_input("输入操作对应的数字：")
                            if operate == '1':
                                select(db)
                            if operate == '2':
                                insert(db)
                            if operate == '3':
                                update(db)
                            if operate == '4':
                                delete(db)
                            if operate == '5':
                                break
                        elif data == 0:
                            print "---user menu---"
                            print "---1.select----"
                            print "---2.update----"
                            print "---3.quit------"
                            operate = raw_input("输入操作对应的数字：")
                            if operate == '1':
                                selectone(db,loginname)
                            if operate == '2':
                                print "---1.update information---"
                                print "---2.update password---"
                                op = raw_input("输入操作：")
                                if op == '1':
                                    updateone(db,loginname)
                                if op == '2':
                                    updatepw(db,loginname)
                            if operate == '3':
                                break
                        else:
                            break
                except:
                    print "登陆失败！"
                    db.rollback()
        elif order == 'quit':
            db.close()
            return
        else:
            continue
if __name__ == '__main__':
    main()
```
