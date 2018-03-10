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
 ---
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
# ！！！注意：Mysql命令均以分号；结尾！！！
# 基本操作
    show databases;        显示所有数据

    use db1;               切换db1数据库下

    show tables;           显示表

    select * from tb1；    显示表tb1里面的数据
## 创建用户
    默认用户：root

    select user,host from user;   查看用户和host

* 创建用户\
    create user '用户名'@'IP地址' identified by '密码'；\
    create user '用户名'@'192.168.1.%' identified by '密码'；\
    create user '用户名'@'%' identified by '密码'；        #任意Ip地址 \




    
    
