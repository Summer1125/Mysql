# Mysql 扩展

## 1 视图
视图是一个虚拟表，其本质是：根据SQL语句获取动态的数据集，并为其命名。用户在使用的时候，只需要使用名称就可以获得结果集，并可以将其当作表来使用。
	
	创建视图：
	-- 语法： create view 视图名 as SQL 语句
	
	create view v1 as select id,name from tb1 where id <10;
	
	删除视图：
	drop view v1;
	
	修改视图：
	-- 语法： alter view 视图名 as SQL语句
	
	alter view v1 as select name from tb1 where id >20;
	
	使用视图：
	使用视图时，将其当作表来操作即可，由于视图是虚拟表，所以无法使对其进行创建，更新和删除操作，仅做查询用。
	
	select * from v1 where id > 10;
## 2 触发器
对表进行【增、删、改】操作的前后，如果希望触发某个特定的行为，就可以使用触发器，触发器用于定制用户对表的行进行【增、删、改】前后的行为。

-- 创建触发器

	# 插入前
	CREATE TRIGGER tri_before_insert_tb1 BEFORE INSERT INTO tb1 FOR EACH ROW
	BEGIN
		...
	END
	
	#插入后
	CREATE TRIGGER tri_after_insert_tb1 AFTER INSERT ON tb1 FOR EACH ROW
	BEGIN
	    ...
	END

	# 删除前
	CREATE TRIGGER tri_before_delete_tb1 BEFORE DELETE ON tb1 FOR EACH ROW
	BEGIN
	    ...
	END
	
	# 删除后
	CREATE TRIGGER tri_after_delete_tb1 AFTER DELETE ON tb1 FOR EACH ROW
	BEGIN
	    ...
	END
	
	# 更新前
	CREATE TRIGGER tri_before_update_tb1 BEFORE UPDATE ON tb1 FOR EACH ROW
	BEGIN
	    ...
	END
	
	# 更新后
	CREATE TRIGGER tri_after_update_tb1 AFTER UPDATE ON tb1 FOR EACH ROW
	BEGIN
	    ...
	END
	
	
示例：
	
	插入前触发器
	delimiter //
	create trigger tri_before_insert_tb1 before insert on tb1 for each row
	begin
		if new.name == 'alex' then
			insert into tb2 (name) values('aa')
		end
		end//
	delimiter ;
	
	插入后触发器：
	delimiter //
	create trigger tri_after_insert_tb1 after insert on tb1 for each row
	begin
		if new.num = 666 then
			insert into tb2(name)values('aaaa'),('cccc')
		end if
	end//
	delimiter ;
	
	!!! NEW 表示新数据，OLD表示老数据

-- 删除触发器
	
	drop trigger tri_after_insert_tb1;
	
-- 使用触发器
	
	触发器无法由用户直接调用，只是因为对表的[增/删/改]操作而被动触发。
## 3 函数

MySQL中提供了许多内置函数，例如：

	CHAR_LENGTH(str)
        返回值为字符串str 的长度，长度的单位为字符。一个多字节字符算作一个单字符。
        对于一个包含五个二字节字符集, LENGTH()返回值为 10, 而CHAR_LENGTH()的返回值为5。

    CONCAT(str1,str2,...)
        字符串拼接
        如有任何一个参数为NULL ，则返回值为 NULL。
    CONCAT_WS(separator,str1,str2,...)
        字符串拼接（自定义连接符）
        CONCAT_WS()不会忽略任何空字符串。 (然而会忽略所有的 NULL）。

    CONV(N,from_base,to_base)
        进制转换
        例如：
            SELECT CONV('a',16,2); 表示将 a 由16进制转换为2进制字符串表示

    FORMAT(X,D)
        将数字X 的格式写为'#,###,###.##',以四舍五入的方式保留小数点后 D 位， 并将结果以字符串的形式返回。若  D 为 0, 则返回结果不带有小数点，或不含小数部分。
        例如：
            SELECT FORMAT(12332.1,4); 结果为： '12,332.1000'
    INSERT(str,pos,len,newstr)
        在str的指定位置插入字符串
            pos：要替换位置其实位置
            len：替换的长度
            newstr：新字符串
        特别的：
            如果pos超过原字符串长度，则返回原字符串
            如果len超过原字符串长度，则由新字符串完全替换
    INSTR(str,substr)
        返回字符串 str 中子字符串的第一个出现位置。

    LEFT(str,len)
        返回字符串str 从开始的len位置的子序列字符。

    LOWER(str)
        变小写

    UPPER(str)
        变大写

    LTRIM(str)
        返回字符串 str ，其引导空格字符被删除。
    RTRIM(str)
        返回字符串 str ，结尾空格字符被删去。
    SUBSTRING(str,pos,len)
        获取字符串子序列

    LOCATE(substr,str,pos)
        获取子序列索引位置

    REPEAT(str,count)
        返回一个由重复的字符串str 组成的字符串，字符串str的数目等于count 。
        若 count <= 0,则返回一个空字符串。
        若str 或 count 为 NULL，则返回 NULL 。
    REPLACE(str,from_str,to_str)
        返回字符串str 以及所有被字符串to_str替代的字符串from_str 。
    REVERSE(str)
        返回字符串 str ，顺序和字符顺序相反。
    RIGHT(str,len)
        从字符串str 开始，返回从后边开始len个字符组成的子序列

    SPACE(N)
        返回一个由N空格组成的字符串。

    SUBSTRING(str,pos) , SUBSTRING(str FROM pos) SUBSTRING(str,pos,len) , SUBSTRING(str FROM pos FOR len)
        不带有len 参数的格式从字符串str返回一个子字符串，起始于位置 pos。带有len参数的格式从字符串str返回一个长度同len字符相同的子字符串，起始于位置 pos。 使用 FROM的格式为标准 SQL 语法。也可能对pos使用一个负值。假若这样，则子字符串的位置起始于字符串结尾的pos 字符，而不是字符串的开头位置。在以下格式的函数中可以对pos 使用一个负值。

        mysql> SELECT SUBSTRING('Quadratically',5);
            -> 'ratically'

        mysql> SELECT SUBSTRING('foobarbar' FROM 4);
            -> 'barbar'

        mysql> SELECT SUBSTRING('Quadratically',5,6);
            -> 'ratica'

        mysql> SELECT SUBSTRING('Sakila', -3);
            -> 'ila'

        mysql> SELECT SUBSTRING('Sakila', -5, 3);
            -> 'aki'

        mysql> SELECT SUBSTRING('Sakila' FROM -4 FOR 2);
            -> 'ki'

    TRIM([{BOTH | LEADING | TRAILING} [remstr] FROM] str) TRIM(remstr FROM] str)
        返回字符串 str ， 其中所有remstr 前缀和/或后缀都已被删除。若分类符BOTH、LEADIN或TRAILING中没有一个是给定的,则假设为BOTH 。 remstr 为可选项，在未指定情况下，可删除空格。

        mysql> SELECT TRIM('  bar   ');
                -> 'bar'

        mysql> SELECT TRIM(LEADING 'x' FROM 'xxxbarxxx');
                -> 'barxxx'

        mysql> SELECT TRIM(BOTH 'x' FROM 'xxxbarxxx');
                -> 'bar'

        mysql> SELECT TRIM(TRAILING 'xyz' FROM 'barxxyz');
                -> 'barx'

	
	
-- 自定义函数
	
	delimiter //
	create function f1(
		i1 int,
		i2 int)
	returns int
	BEGIN
		declare num int;
		set num = i1 + i2;
		return(num);
	END //
	delimiter ;
	
-- 删除函数

	drop function func_name;

-- 执行函数
	
	# 获取返回值
	declear @i varchar(32);
	select UPPER('apple') into @i;
	SELECT @i;
	
	
	#在查询中使用
	select f1(11,nid),name from tb1;
	
## 4 存储过程
存储过程是一个SQL集合，主动去调用存储过程时，其中内部的SQL语句会按照逻辑执行。
1、创建存储过程
	
	-- 创建存储过程
	delimiter //
	create procedure p1()
	BGEGIN
		select * from t1;
	END//
	delimiter ;
	
	-- 执行存储过程
	call p1()
存储过程可以接收参数，参数有三种类型：  
** in     	仅用于传入参数用  
** out       仅用于返回值用  
** inout     既可以传入又可以当作返回值  

有参数的存储过程：
	
	-- 创建存储过程
	delimiter //
	create procedure p1(
		in i1 int,
		in i2 int,
		inout i3 int,
		out r1 int
	)
	BEGIN
		declear temp1 int;
		declear temp2 int default 0;
		
		set temp1 = 1;
		set r1 = i1 + i2 + temp1 + temp2;
		set i3 = i3 + 100;
	END//
	delimiter ;
	
	-- 执行存储过程
	set @t1 = 4;
	set @t2 = 0;
	call p1(1,2,@t1,@t2);
	select @t1,@t2;
	
结果集：

	delimiter //
    create procedure p1()
    begin
        select * from v1;
    end //
    delimiter ;
   
结果集 + out值:

		delimiter //
        create procedure p2(
            in n1 int,
            inout n3 int,
            out n2 int,
        )
        begin
            declare temp1 int ;
            declare temp2 int default 0;

            select * from v1;
            set n2 = n1 + 100;
            set n3 = n3 + n1 + 100;
        end //
        delimiter ;
        
        为什么有结果集了又有out伪造的返回值？
        
        out可以作为判断SQL语句是否执行成功的标示，比如如果成功返回1，不成功返回0。
        
 事务：
       
	delimiter \\
	create PROCEDURE p1(
	    OUT p_return_code tinyint
	)
	BEGIN 
	  DECLARE exit handler for sqlexception 
	  BEGIN 
	    -- ERROR 
	    set p_return_code = 1; 
	    rollback; 
	  END; 
	 
	  DECLARE exit handler for sqlwarning 
	  BEGIN 
	    -- WARNING 
	    set p_return_code = 2; 
	    rollback; 
	  END; 
	 
	  START TRANSACTION; 
	    DELETE from tb1;
	    insert into tb2(name)values('seven');
	  COMMIT; 
	 
	  -- SUCCESS 
	  set p_return_code = 0; 
	 
	  END\\
	delimiter ;
游标：

	delimiter //
	create procedure p3()
	begin 
	    declare ssid int; -- 自定义变量1  
	    declare ssname varchar(50); -- 自定义变量2  
	    DECLARE done INT DEFAULT FALSE;
	
	
	    DECLARE my_cursor CURSOR FOR select sid,sname from student;
	    DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;
	    
	    open my_cursor;
	        xxoo: LOOP
	            fetch my_cursor into ssid,ssname;
	            if done then 
	                leave xxoo;
	            END IF;
	            insert into teacher(tname) values(ssname);
	        end loop xxoo;
	    close my_cursor;
	end  //
	delimter ;
	
动态执行SQL：

	delimiter \\
	CREATE PROCEDURE p4 (
	    in nid int
	)
	BEGIN
	    PREPARE prod FROM 'select * from student where sid > ?';
	    EXECUTE prod USING @nid;
	    DEALLOCATE prepare prod; 
	END\\
	delimiter ;	
	
删除存储过程：
	
	drop procedure proc_name;
  	
执行存储过程：

	-- 无参数
	call pro_name()
	
	-- 有参数，全部in
	call proc_name(1,2)
	
	--有参数，有in，out，inout
	set @t1;
	set @t2;
	call proc_name(1,2,@t1,@t2)
	
	
pycharm 执行存储过程：

	import pymysql
	
	conn = pymysql.connect(host='127.0.0.1',port=3306,user='root',
								passwd='123',database='db1')
	cursor = conn.cursor(cursor=pycharm.cursors.DictCursor)
	
	#执行存储过程
	cursor.callproc('p1',args=(1,22,3,4))
	# 获取执行完存储的参数
	
	cursor.execute("select @_p1_0,@_p1_1,@_p1_2,@_p1_3")
	
	result = cursor.fetchall()
	
	conn.commit()
	cursor.close()
	conn.close()
	
	print(result)
	
	
	
	

## 5 事务
事务用于将某些操作的多个SQL作为原子性操作，一旦某一个出现错误，既可以回滚到原来的状态，从而保证数据库的完整性。


	#支持事务的存储过程
	
	delimiter //
	create PROCEDURE p1(
		OUT p_return_code tinyint
	)
	BEGIN
		DECLEAR exit handler for sqlexception
		BEGIN
			-- ERROR
			set p_return_code = 1;
			rollback;
		END;
		
		DECLARE exit handler for sqlwarning 
	  
	   BEGIN 
	     -- WARNING 
	     set p_return_code = 2; 
	     rollback; 
	   END; 
	 
	   START TRANSACTION; 
	     DELETE from tb1;
	     insert into tb2(name)values('seven');
	   COMMIT; 
	 
	   -- SUCCESS 
	   set p_return_code = 0; 
	 
	  END//
	delimiter ;	
	
调用
	
	set @i= 0；
	call p1(@i);
	select @i;
## 索引
    
索引是数据库中专门用于帮助用户快速查询数据的一种数据结构。类似于字典中的目录，查找字典内容时可以根据目录查找到数据的存放位置，然后可以直接获取。
    
    MySQL中常见的索引有：
        ** 普通索引  
        ** 唯一索引  
        ** 主键索引  
        ** 组合索引  
    
### 1、普通索引
    普通索引只有一个功能：加速查找

    create table in(
            nid int not null auto_increment primary key,
            name varchar(32) not null,
            email varchar(64) not null.
            extra text,
            index ix_name (name)
        )

创建索引：

    create index index_name on table_name(column_name);

删除索引：

    drop index_name on table_name;

查看索引：

    show index from table_name;
 
 !!!注意，对于创建索引时如果是BLOB和TEXT类型，必须指定length

    create index ix_extra on in1 (extra(32));

### 2、唯一索引

唯一索引有两个功能：加速查询，约束唯一（可含null）

创建表 + 唯一索引
    
    create table in1(
        nid int not null auto_increment primary key,
        name varchar(32) not null,
        email varchar(64) not null,
        extra text,
        unique ix_name(name)
    )
 

 创建唯一索引

    create unique index 索引名 on 表名(列名)

删除唯一索引

    drop unique index 索引名 on 表名

### 3、主键索引

主键索引有两个功能：加速查找，唯一约束（不可以null）

创建表 + 主键索引

    create table in1(
        nid int not null auto_increment primary key,
        name varchar(32) not null,
        email varchar(64) not null,
        extra text,
        index ix_name(name)
    )


    or 

    create table in1(
        nid int not null auto_increment,
        name varchar(32) not null,
        email varchar(64) not null,
        extra text,
        primary key(nid),
        index ix_name(name)
    )


创建主键

    alter table 表名 add primary key(列名)；

删除主键

    alter table 表名 drop primary key;
    alter table 表名 modify 列名 int,drop primary key;

### 4、组合索引
组合索引是将n个列组合成一个索引
其应用场景：频繁的同时使用n列来进行查询，如where n1='alisa' and n2=666

创建表：

    create table in3(
        nid int not null auto_increment primary key,
        name varchar(32) not null,
        email varchar(64) not null,
        extra text
    )

创建组合索引：

    create index ix_name_email on in3(name,email);


    按照这样创建组合索引之后，查询：
    ** name and email  --使用索引
    ** name            --使用索引
    **email            --不使用索引

    ！！！注意，对于同时搜索n个条件时，组合索引的性能好于单一索引合并


## 索引补充，会用到

### 1、索引是表的目录，在查找内容之前可以先在目录中查找索引位置，以此快速定位查询数据。索引则会保存在额外的文件中。

### 2、索引的种类
    
    普通索引：仅加速查询
    唯一索引：加速查询 + 列值唯一（可以有null）
    主键索引：加速查询 + 列值唯一 +　表中只有一个（不可以有null）
    组合索引：多列值组成一个索引，专门用于组合搜索，其效率大于索引合并
    全文索引：对文本的内容进行分词，进行搜索 

索引合并，使用多个单列索引组合搜索
覆盖索引，select的数据列只用从索引中就能够取得，不必读取数据行，换句话说查询列要被所建的索引覆盖

#### 3、一些命令

    - 查看表结构
    desc 表名
 
    - 查看生成表的SQL
        show create table 表名
     
    - 查看索引
        show index from  表名
     
    - 查看执行时间
        set profiling = 1;
        SQL...
        show profiles;

### 4、使用索引和不使用索引

    由于索引是专门用于加速搜索而生，所以加上索引之后，查询效率会快到飞起来。
 
    # 有索引
    mysql> select * from tb1 where name = 'wupeiqi-888';
    +-----+-------------+---------------------+----------------------------------+---------------------+
    | nid | name        | email               | radom                            | ctime               |
    +-----+-------------+---------------------+----------------------------------+---------------------+
    | 889 | wupeiqi-888 | wupeiqi888@live.com | 5312269e76a16a90b8a8301d5314204b | 2016-08-03 09:33:35 |
    +-----+-------------+---------------------+----------------------------------+---------------------+
    1 row in set (0.00 sec)
     
    # 无索引
    mysql> select * from tb1 where email = 'wupeiqi888@live.com';
    +-----+-------------+---------------------+----------------------------------+---------------------+
    | nid | name        | email               | radom                            | ctime               |
    +-----+-------------+---------------------+----------------------------------+---------------------+
    | 889 | wupeiqi-888 | wupeiqi888@live.com | 5312269e76a16a90b8a8301d5314204b | 2016-08-03 09:33:35 |
    +-----+-------------+---------------------+----------------------------------+---------------------+
    1 row in set (1.23 sec)

### 5、正确使用索引

数据库表中添加索引后确实会让查询速度起飞，但前提必须是正确的使用索引来查询，如果以错误的方式使用，则即使建立索引也会不奏效。
即使建立索引，索引也不会生效：

    - like '%xx'
        select * from tb1 where name like '%cn';
    - 使用函数
        select * from tb1 where reverse(name) = 'wupeiqi';
    - or
        select * from tb1 where nid = 1 or email = 'seven@live.com';
        特别的：当or条件中有未建立索引的列才失效，以下会走索引
                select * from tb1 where nid = 1 or name = 'seven';
                select * from tb1 where nid = 1 or email = 'seven@live.com' and name = 'alex'
    - 类型不一致
        如果列是字符串类型，传入条件是必须用引号引起来，不然...
        select * from tb1 where name = 999;
    - !=
        select * from tb1 where name != 'alex'
        特别的：如果是主键，则还是会走索引
            select * from tb1 where nid != 123
    - >
        select * from tb1 where name > 'alex'
        特别的：如果是主键或索引是整数类型，则还是会走索引
            select * from tb1 where nid > 123
            select * from tb1 where num > 123
    - order by
        select email from tb1 order by name desc;
        当根据索引排序时候，选择的映射如果不是索引，则不走索引
        特别的：如果对主键排序，则还是走索引：
            select * from tb1 order by nid desc;
     
    - 组合索引最左前缀
        如果组合索引为：(name,email)
        name and email       -- 使用索引
        name                 -- 使用索引
        email                -- 不使用索引


### 6、其他注意事项

    - 避免使用select *
    - count(1)或count(列) 代替 count(*)
    - 创建表时尽量时 char 代替 varchar
    - 表的字段顺序固定长度的字段优先
    - 组合索引代替多个单列索引（经常使用多个条件查询时）
    - 尽量使用短索引
    - 使用连接（JOIN）来代替子查询(Sub-Queries)
    - 连表时注意条件类型需一致
    - 索引散列值（重复少）不适合建索引，例：性别不适合

### 7、limit分页

无论是否有索引，limit分页是一个值得关注的问题

    每页显示10条：
    当前 118 120， 125

    倒序：
                大      小
     7 6  6 5  54  43  32
    98     
    下一页：

        select 
            * 
        from 
            tb1 
        where 
            nid < (select nid from (select nid from tb1 where nid < 当前页最小值 order by nid desc limit 每页数据 *【页码-当前页】) A order by A.nid asc limit 1)  
        order by 
            nid desc 
        limit 10;



        select 
            * 
        from 
            tb1 
        where 
            nid < (select nid from (select nid from tb1 where nid < 970  order by nid desc limit 40) A order by A.nid asc limit 1)  
        order by 
            nid desc 
        limit 10;


    上一页：

        select 
            * 
        from 
            tb1 
        where 
            nid < (select nid from (select nid from tb1 where nid > 当前页最大值 order by nid asc limit 每页数据 *【当前页-页码】) A order by A.nid asc limit 1)  
        order by 
            nid desc 
        limit 10;


        select 
            * 
        from 
            tb1 
        where 
            nid < (select nid from (select nid from tb1 where nid > 980 order by nid asc limit 20) A order by A.nid desc limit 1)  
        order by 
            nid desc 
        limit 10;


### 8、执行计划

explain + 查询SQL - 用于显示SQL执行信息参数，根据参考信息可以进行SQL优化

    mysql> explain select * from tb2;
    +----+-------------+-------+------+---------------+------+---------+------+------+-------+
    | id | select_type | table | type | possible_keys | key  | key_len | ref  | rows | Extra |
    +----+-------------+-------+------+---------------+------+---------+------+------+-------+
    |  1 | SIMPLE      | tb2   | ALL  | NULL          | NULL | NULL    | NULL |    2 | NULL  |
    +----+-------------+-------+------+---------------+------+---------+------+------+-------+
    1 row in set (0.00 sec)


    详细：
    id
        查询顺序标识
            如：mysql> explain select * from (select nid,name from tb1 where nid < 10) as B;
            +----+-------------+------------+-------+---------------+---------+---------+------+------+-------------+
            | id | select_type | table      | type  | possible_keys | key     | key_len | ref  | rows | Extra       |
            +----+-------------+------------+-------+---------------+---------+---------+------+------+-------------+
            |  1 | PRIMARY     | <derived2> | ALL   | NULL          | NULL    | NULL    | NULL |    9 | NULL        |
            |  2 | DERIVED     | tb1        | range | PRIMARY       | PRIMARY | 8       | NULL |    9 | Using where |
            +----+-------------+------------+-------+---------------+---------+---------+------+------+-------------+
        特别的：如果使用union连接气值可能为null


    select_type
        查询类型
            SIMPLE          简单查询
            PRIMARY         最外层查询
            SUBQUERY        映射为子查询
            DERIVED         子查询
            UNION           联合
            UNION RESULT    使用联合的结果
            ...
    table
        正在访问的表名


    type
        查询时的访问方式，性能：all < index < range < index_merge < ref_or_null < ref < eq_ref < system/const
            ALL             全表扫描，对于数据表从头到尾找一遍
                            select * from tb1;
                            特别的：如果有limit限制，则找到之后就不在继续向下扫描
                                   select * from tb1 where email = 'seven@live.com'
                                   select * from tb1 where email = 'seven@live.com' limit 1;
                                   虽然上述两个语句都会进行全表扫描，第二句使用了limit，则找到一个后就不再继续扫描。

            INDEX           全索引扫描，对索引从头到尾找一遍
                            select nid from tb1;

            RANGE          对索引列进行范围查找
                            select *  from tb1 where name < 'alex';
                            PS:
                                between and
                                in
                                >   >=  <   <=  操作
                                注意：!= 和 > 符号


            INDEX_MERGE     合并索引，使用多个单列索引搜索
                            select *  from tb1 where name = 'alex' or nid in (11,22,33);

            REF             根据索引查找一个或多个值
                            select *  from tb1 where name = 'seven';

            EQ_REF          连接时使用primary key 或 unique类型
                            select tb2.nid,tb1.name from tb2 left join tb1 on tb2.nid = tb1.nid;



            CONST           常量
                            表最多有一个匹配行,因为仅有一行,在这行的列值可被优化器剩余部分认为是常数,const表很快,因为它们只读取一次。
                            select nid from tb1 where nid = 2 ;

            SYSTEM          系统
                            表仅有一行(=系统表)。这是const联接类型的一个特例。
                            select * from (select nid from tb1 where nid = 1) as A;
    possible_keys
        可能使用的索引

    key
        真实使用的

    key_len
        MySQL中使用索引字节长度

    rows
        mysql估计为了找到所需的行而要读取的行数 ------ 只是预估值

    extra
        该列包含MySQL解决查询的详细信息
        “Using index”
            此值表示mysql将使用覆盖索引，以避免访问表。不要把覆盖索引和index访问类型弄混了。
        “Using where”
            这意味着mysql服务器将在存储引擎检索行后再进行过滤，许多where条件里涉及索引中的列，当（并且如果）它读取索引时，就能被存储引擎检验，因此不是所有带where子句的查询都会显示“Using where”。有时“Using where”的出现就是一个暗示：查询可受益于不同的索引。
        “Using temporary”
            这意味着mysql在对查询结果排序时会使用一个临时表。
        “Using filesort”
            这意味着mysql会对结果使用一个外部索引排序，而不是按索引次序从表里读取行。mysql有两种文件排序算法，这两种排序方式都可以在内存或者磁盘上完成，explain不会告诉你mysql将使用哪一种文件排序，也不会告诉你排序会在内存里还是磁盘上完成。
        “Range checked for each record(index map: N)”
            这个意味着没有好用的索引，新的索引将在联接的每一行上重新估算，N是显示在possible_keys列中索引的位图，并且是冗余的。


### 9、慢日志查询

a、配置MySQL自动记录慢日志

    slow_query_log = OFF                            是否开启慢日志记录
    long_query_time = 2                              时间限制，超过此时间，则记录
    slow_query_log_file = /usr/slow.log        日志文件
    log_queries_not_using_indexes = OFF     为使用索引的搜索是否记录
    注：查看当前配置信息：
    　　     show variables like '%query%'
         修改当前配置：
    　　　　set global 变量名 = 值


b、查看MySQL慢日志

    mysqldumpslow -s at -a  /usr/local/var/mysql/MacBook-Pro-3-slow.log


    """
    --verbose    版本
    --debug      调试
    --help       帮助
     
    -v           版本
    -d           调试模式
    -s ORDER     排序方式
                 what to sort by (al, at, ar, c, l, r, t), 'at' is default
                  al: average lock time
                  ar: average rows sent
                  at: average query time
                   c: count
                   l: lock time
                   r: rows sent
                   t: query time
    -r           反转顺序，默认文件倒序拍。reverse the sort order (largest last instead of first)
    -t NUM       显示前N条just show the top n queries
    -a           不要将SQL中数字转换成N，字符串转换成S。don't abstract all numbers to N and strings to 'S'
    -n NUM       abstract numbers with at least n digits within names
    -g PATTERN   正则匹配；grep: only consider stmts that include this string
    -h HOSTNAME  mysql机器名或者IP；hostname of db server for *-slow.log filename (can be wildcard),
                 default is '*', i.e. match all
    -i NAME      name of server instance (if using mysql.server startup script)
    -l           总时间中不减去锁定时间；don't subtract lock time from total time
    """

## 其他

### 1、条件语句
    
    delimiter \\
    CREATE PROCEDURE proc_if ()
    BEGIN
        
        declare i int default 0;
        if i = 1 THEN
            SELECT 1;
        ELSEIF i = 2 THEN
            SELECT 2;
        ELSE
            SELECT 7;
        END IF;

    END\\
    delimiter ;

### 2、循环语句

while循环：
    delimiter \\
    CREATE PROCEDURE proc_while ()
    BEGIN

        DECLARE num INT ;
        SET num = 0 ;
        WHILE num < 10 DO
            SELECT
                num ;
            SET num = num + 1 ;
        END WHILE ;

    END\\
    delimiter ;

repeat循环：
    delimiter \\
    CREATE PROCEDURE proc_repeat ()
    BEGIN

        DECLARE i INT ;
        SET i = 0 ;
        repeat
            select i;
            set i = i + 1;
            until i >= 5
        end repeat;

    END\\
    delimiter ;

loop:
    BEGIN
        
        declare i int default 0;
        loop_label: loop
            
            set i=i+1;
            if i<8 then
                iterate loop_label;
            end if;
            if i>=10 then
                leave loop_label;
            end if;
            select i;
        end loop loop_label;

    END

### 3、动态执行SQL语句

    delimiter \\
    DROP PROCEDURE IF EXISTS proc_sql \\
    CREATE PROCEDURE proc_sql ()
    BEGIN
        declare p1 int;
        set p1 = 11;
        set @p1 = p1;

        PREPARE prod FROM 'select * from tb2 where nid > ?';
        EXECUTE prod USING @p1;
        DEALLOCATE prepare prod; 

    END\\
    delimiter ;
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
		
