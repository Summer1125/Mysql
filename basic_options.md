# Mysql:
    Mysql是用于管理文件的一个软件，基于socket编写，包括服务端和客户端两部分。
    现在一般使用关系型数据库管理系统（RNBMS）来存储和管理大量数据，所谓关系型数据库就是
    建立在关系模型基础上的数据库，借助于集合代数等数学概念和方法处理数据库中的数据。
    RNBMS:Relationship Database Management System
_特点_：  
* 数据以表格的形式出现  
* 每行为各种记录名称  
* 每列为该记录名称的数据域  
* 许多行和列组成一张表  
* 若干表组成database    
_术语_：

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

    创建用户:  
        create user '用户名'@'IP地址' identified by '密码'；  
        
        create user '用户名'@'192.168.1.%' identified by '密码'；  
        
        create user '用户名'@'%' identified by '密码'；  #任意Ip地址   
    删除用户：  
    drop user '用户名'@'IP地址'；
    
    修改用户：  
        rename user '用户名'@'IP地址' to '新用户名'@'IP地址'；
    修改密码：  
    
        set password for '用户名'@'IP地址' = Password('新密码')；
    
    给用户授权：
    
        grant select,insert,update on db1.* to 'summer'@'%';   在数据库db1下给用户summer授查改写权限
    
        grant all privileges on db1.t1 to 'summer'@'%';         在数据库db1下的表t1给用户summer授所有权限
    移除用户的权限：
    
        revoke all privilege on db1.t1 from 'summer'@'198.168.1.1';
### 权限（了解）
            all privileges  除grant外的所有权限  
            select 仅查权限  
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
    新建一个表:  
 
    create tabelt1(
        列名 类型 not null auto_increment primary key,
        列名 类型 null,
        列名 类型 not null,
        列名 类型)engine = innodb default charset=utf8;
    * innodb表示支持事物原子性操作，操作失败则回滚到之前的状态
    * auto_increment 表示自增
    * primary key 表示主键，约束，不能重复且不能为空，是为了加速查找

---
    举例：
    create table t1(
        id int auto_increment primary key,
        name char(10),
        age int
        )engine=innodb default charset=utf8;  
    删除表:  
        drop table t1;
     
     清空表:  
   
        delete from t1;    新插入数据的话自增的id会继续接着自增

        truncate table t1; 新插入数据的话自增的id会从1开始！！！
    
    修改表:  
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
    插入数据：
    insert into t1(id,name) values(1,'chenjie');     如果id是自增主键的话可以省略
    
    删除数据：
    delete from t1 where id < 6;

    修改数据：
    update t1 set age=19;
    update t1 set age=18 where age=17;
    update t1 set age=18 where id<6;

    查看数据：
    select * from t1;
    select id,name from t1;
    select id,name where id > 10;
    
---
### 自增：
    如果表中的某一列设置了自增，在插入数据时无需对此列进行操作，默认自增，且每张表中只能有一个自增列。
    create table t1(
        id int aotu_increment primary key,
        name char(10),
        age int)engine=innodb default charset=utf8;
    
    注意：
    1.自增列这必须是索引（含主键）
    2.可以对自增列设置步长和起始值
        show session variable like 'auto_inc%';
        set session auto_increment_increment=2;
        set session auto_increment_offset=10;
        
        show global variable like 'aotu_inc%';
        set global auto_increment_increment=2;
        set global auto_increment_offset =10;
### 主键：
    主键，一种特殊的唯一索引，不允许有空值，如果某个主键使用单列，则它的值必须唯一，如果是多列，那么多列组合起来的值必须唯一。
    create table t1(
        id int auto_increment primary key,
        name char(10));
    或：
    create table t1(
        id int not null,
        num int not null,
        primary key(id,num));
### 外键
    外键，一个特殊的索引，只能是制定的内容，表示表与表之间的关系。
    create table coloe(
        cid int not null auto_increment primary key,
        name char(16) not null)engine=innodb default charset=utf8;
    
    create table fruit(
        id int primary key,
        smt varchar(10) null,
        color_id int null,
        constraint fk_fruit_color foreign key(color_id) references color(cid)
        )engine=innodb default charset=utf8;
## 数据类型
    bit[(M)]:
        二进制位（101001），m表示二进制的长度（1-64），默认m=1
        
    tinyint[(m)][usigned][zerofill]:
        小整数，该数据类型用于保存一些范围内的📄数值范围；
        有符号： -128～127
        无符号：0～255
    ！！！ mysql无布尔型，可以用tinyint（1）来表示。
    
    int[(m)][usigned][zerofill]:
        整数
        有符号：-2147483648～2147483647
        无符号：0～4294967295
        特别的，整数类型中的m仅用于显示，对存储范围无限制，例如，int(5),当插入数据2时，select 的话数据显示为00002
        
    bigint[(m)][usigned][zerofill]:
        大整数
        有符号：-9223372036854775808～9223372036854775807
        有符号：0～18446744073709551615
        
    decimal[(m[,d])][usigned][zerofill]:
        准确的小数，m是数字总个数（不算负号）,d是小数点后的个数。m最大65，d最大30。
        
        ！！！对于精确数值计算的时候需要此类型
        decimal能存储精确的原因在于其按照字符串存储
        
    float[(m,d)][usigned][zerofill]:
        单精度浮点数，非准确小数值，m是数字个数，d是小数点后的个数。
        
        ！！！数值越大，越不准确。
    double[(m,d)][usigned][zerofill]:
        双精度浮点数，m是数字总个数，d是小数点后的个数。
        ！！！数值越大，越不准确。
        
    char(m):
        char类型用于表示固定长度的字符，可包含最多255个字符，其中m表示字符串长度。
        ！！！即使数据长度小于m，也会占用m个长度
        
    varchar(m):
        varchar数据类型用于表示变长的字符串，其中m表示做允许的字符串的最大长度，m最大为255,只要实际数据长度小于给定的m值，就可以。
        
        ！！！注：虽然varchar使用起来比较灵活，但是从整个系统的性能来讲，char类型的处理速度更快，有时甚至超出varchar速度的50%，因此在设计数据库时，要综合考虑以达到最佳.
        
        ！！！在创建数据库的时候，一般定长的char往前放。
    text:
        text用于存储变长的大字符串，最多65535个。（2**16-1）
        上传文件： 
                文件存硬盘
                db存路径
    时间类型：
            DATETIME
            YYYY-MM-DD HH：MM：SS
    枚举类型    
        enum
        示例：
        create table shirts(
            name varchar(30),
            size enum('x-small','dedium','large')
            );
            
        insert into shirt (name,size)values('dress','large'),('t-shirt','x-small');
    集合类型：set
        例如：
        create table myset(
            col set('a','b','c');
        insert into myset (col) values('a,c'),('c,b'),('b,c');
---
## 数据行的操作
    增加：

    insert into 表（列名，列名，...）values（值，值，值，...）；
    insert into 表（列名，列名，...）values（值，值，值,...）,(值，值，值,...);
    insert into 表（列名，列名，...）select (列名，列名，...)from 表
    
    删除：
    delete from 表；
    delete from 表 where id=1 and name='egg';
    
    修改：
    update 表 set name='egg' where id>10;
    
    查询：
    select * from 表；
    select * from 表 where id>10;
    select id,name,gender as G from 表 where id > 10;
### 关于查询
    1.条件
        select * from 表 where id>10 and name != 'egg' and age = 18;
        
        select * from 表 where id between 5 and 20;
        
        select * from 表 where id in (11,20,30);
        selcet * from 表 where id not in(11,20,30);
        select * from 表 where id in (select in from 表2)；
    
    2. 通配符
        select * from 表 where name like 'abc%';  abc开头的所有（多个字符串）
        select * from 表 where name like 'abc_';  abc开头后面只有一个字符的所有
        
    3.限制
    
        select * from 表 limit 5;           前5行
        select * from 表 limit 4,5;         从第4行开始的后面5行
        select * from 表 limit 8 offset 4； 从第4行开始的后面5行

        分页：
            在浏览网页的时候不可能把所有的数据一次显示，这时候就要分页。

            select * from 表 limit 10；

            select * from 表 limit 1,10；
            select * from 表 limit 10,10；
            select * from 表 limit 20,10;

            select * from 表 limit 10 offset 20；

            # page = input（'请输入要查看的页码：'）
            # page = int(page)
            #(page - 1)*10  
            #select * from 表 limit 0,(page-1)*10    每页查看十行内容的数据

        
    4.排序
        select * from 表 order by id asc;    根据id从小到大排序
        select * from 表 order by age desc;  根据age从小到大排序
        select * from 表 order by id desc,age asc;根据id降序age升序排列
        
        select * from 表 order by id desc limit 10;  取最后10行

    
        
    5. 分组
        select age from 表 group by age;
        select age,id from 表 group by age,id;
        select age,id from 表 where id>10 group by age,id order if asc;
        select age,id count（*),sum(age),min(age) from 表 group by age,id;
        
        select age from 表 group by age having max(id) > 10;
        
        !!!注意，group by 必须在where 之后，order by之前
        
        函数：count,min,max,sum,avg
         
        *** 如果对队聚合数据进行二次筛选，必须使用having！！！
        
    6. 连表
![image](Mysql/picture/6G13F$Z46[N8DXV5O$`_K2G.png)
![image](Mysql/picture/Z3{{QSPPT~Z0JW%ETE74AFG.png)
        无对应关系则不进行显示
        select A.score,A.name,B.name from A,B where A.num = B.id;


        select * from userinfo,department;

        select * from userinfo,department where userinfo.part_id = department.id;

        推荐以下方式：
        select * from userinfo left join department on userinfo.part_id = department.id;
        select * from department left join userinfo on userinfo.part_id = department.id;
        #left表示userinfo左边全部显示

        select * from userinfo right join department on userinfo.part_id = department.id;
        #department右边全部显示

        select * from userinfo innder join department on userinfo.part_id = department.id;
        #将出现null的行隐藏

        平时只记left就行了，谁在前在后可以这样表示：
            select * from department left join userinfo on userinfo.part_id = department.id;
            select * from userinfo left join department on userinfo.part_id = department.id;






        
                
            
        
        
        
        
        
     
        
        
        
        
        
    
 
        
    






    
