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

-- 	执行函数
	
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
		
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
		
