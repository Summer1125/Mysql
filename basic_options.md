# Mysql:
    Mysql是用于管理文件的一个软件，基于socket编写，包括服务端和客户端两部分。
    现在一般使用关系型数据库管理系统（RNBMS）来存储和管理大量数据，所谓关系型数据库就是
    建立在关系模型基础上的数据库，借助于集合代数等数学概念和方法处理数据库中的数据。
    RNBMS:Relationship Database Management System
    特点：
        ** 数据以表格的形式出现
        ** 每行为各种记录名称
        ** 每列为该记录名称的数据域
        ** 许多行和列组成一张表
        ** 若干表组成database
    术语：

    -- 数据库: 数据库是一些关联表的集合。

    -- 数据表: 表是数据的矩阵。在一个数据库中的表看起来像一个简单的电子表格。

    -- 列: 一列(数据元素) 包含了相同的数据, 例如邮政编码的数据。

    -- 行：一行（=元组，或记录）是一组相关的数据，例如一条用户订阅的数据。

    -- 冗余：存储两倍数据，冗余降低了性能，但提高了数据的安全性。

    -- 主键：主键是唯一的。一个数据表中只能包含一个主键。你可以使用主键来查询数据。

    -- 外键：外键用于关联两个表。

    -- 复合键：复合键（组合键）将多个列作为一个索引键，一般用于复合索引。

    -- 索引：使用索引可快速访问数据库表中的特定信息。索引是对数据库表中一列或多列的值进行排序的一种结构。类似于书籍的目录。

    -- 参照完整性: 参照的完整性要求关系中不允许引用不存在的实体。与实体完整性是关系模型必须满足的完整性约束条件，目的是保证数据的一致性。

# 1.安装
## 1.1 windows下安装Mysql
    下载可执行文件，一路点点点
    压缩包
        放置在任意目录下
        初始化（需要新建一个data文件夹）
            服务端：E:\mu_mysql\mysql-5.7.18-winx64\mysql-5.7.18-winx64\bin\mysqld --initialize-insecure
            启动服务端：E:\mu_mysql\mysql-5.7.18-winx64\mysql-5.7.18-winx64\bin\mysqld
            客户端连接：E:\mu_mysql\mysql-5.7.18-winx64\mysql-5.7.18-winx64\bin\mysql
            发送指令：
                show databases;
                create database db1;

    配置环境变量：
        E:\mu_mysql\mysql-5.7.18-winx64\mysql-5.7.18-winx64\bin\ 添加到path
    windows服务：管理员身份运行cmd 输入：H:\mu_mysql\mysql-5.7.18-winx64\mysql-5.7.18-winx64\bin\mysqld --install
    再去Ctrl+Alt+delete启动服务
    net start MySQL
    net stop MySQL
## 1.2 Mac 安装Mysql
    参考：https://www.jianshu.com/p/fd3aae701db9
# _！！！注意：Mysql命令均以分号；结尾！！！_
# 2.基本操作
    show databases;        显示所有数据

    use db1;               切换db1数据库下

    show tables;           显示表

    select * from tb1；    显示表tb1里面的数据
## 用户操作（创建，删除）
    默认用户：root

    select user,host from user;   查看用户和host

* 创建用户：\
        create user '用户名'@'IP地址' identified by '密码'；\
        create user '用户名'@'192.168.1.%' identified by '密码'；\
        create user '用户名'@'%' identified by '密码'；        #任意Ip地址 \
* 删除用户：
    drop user '用户名'@'IP地址'；
* 修改用户：
    rename user '用户名'@'IP地址' to '新用户名'@'IP地址'；
* 修改密码：
    set password for '用户名'@'IP地址' = Password('新密码')；
* 给用户授权：
    grant select,insert,update on db1.* to 'summer'@'%';   在数据库db1下给用户summer授查改写权限
    grant all privileges on db1.t1 to 'summer'@'%';         在数据库db1下的表t1给用户summer授所有权限
* 移除用户的权限：
    revoke all privilege on db1.t1 from 'summer'@'198.168.1.1';
-- 权限（了解）
all privileges  除grant外的所有权限
            select          仅查权限
            select,insert   查和插入权限
            ...
            usage                   无访问权限
            alter                   使用alter table
            alter routine           使用alter procedure和drop procedure
            create                  使用create table
            create routine          使用create procedure
            create temporary tables 使用create temporary tables
            create user             使用create user、drop user、rename user和revoke  all privileges
            create view             使用create view
            delete                  使用delete
            drop                    使用drop table
            execute                 使用call和存储过程
            file                    使用select into outfile 和 load data infile
            grant option            使用grant 和 revoke
            index                   使用index
            insert                  使用insert
            lock tables             使用lock table
            process                 使用show full processlist
            select                  使用select
            show databases          使用show databases
            show view               使用show view
            update                  使用update
            reload                  使用flush
            shutdown                使用mysqladmin shutdown(关闭MySQL)
            super                   􏱂􏰈使用change master、kill、logs、purge、master和set global。还允许mysqladmin􏵗􏵘􏲊􏲋调试登陆
            replication client      服务器位置的访问
            replication slave       由复制从属使用
## 数据库基本操作
### 操作文件夹:
    create database db1;                      创建一个文件夹db1

    create database db1 default carset utf8;   创建一个文件夹db1

    show databases;                            显示所有文件夹

    drop database db1;                         删除db1

### 操作文件（表）
    show tables;
* 新建一个表：
    create tabelt1(
        列名 类型 not null auto_increment primary key,
        列名 类型 null,
        列名 类型 not null,
        列名 类型)engine = innodb default charset=utf8;

*innodb表示支持事物原子性操作，操作失败则回滚到之前的状态*
*auto_increment 表示自增*
*primary key 表示主键，约束，不能重复且不能为空，是为了加速查找*

---
    举例：
    create table t1(
        id int auto_increment primary key,
        name char(10),
        age int
        )engine=innodb default charset=utf8;
* 删除表：
    drop table t1;
* 清空表：
    delete from t1;    新插入数据的话自增的id会继续接着自增

    truncate table t1; 新插入数据的话自增的id会从1开始！！！
* 修改表：
-- 添加列：alter table 表名 modify column 列名 类型；
-- 删除列：alter table 表名 drop column 列名；
-- 修改列：
    alter table 表名 modify column 列名 类型；
    alter table 表名 change 原列名 新列名 类型；

-- 添加主键：
    alter table 表名 add primary key(列名)；
-- 删除主键：
    alter table 表名 drop primary key;
    alter table 表名 modify 列名 int,drop primary key;
### 操作数据：
* 插入数据：
    insert into t1(id,name) values(1,'chenjie');        如果id是自增主键的话可以省略
* 删除数据：
    delete from t1 where id < 6;

* 修改数据：
    update t1 set age=19;
    update t1 set age=18 where age=17;
    update t1 set age=18 where id<6;

* 查看数据：
    select * from t1;






    
    
