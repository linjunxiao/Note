### MySQL服务的登录和退出

 1. 打开cmd

 2. 输入mysql -h主机名 -p端口号 -u用户名 -p

    登录本机时，只需执行mysql -u root -p即可

### MySQL的常见命令

 1. 查看当前所有的数据库

    --- show databases;

 2. 打开指定的库  

    --- use + 库名;

 3. 查看当前库 的所有表   

    --- show tables;

 4. 查看其他库的所有表   

    --- show tables from 库名;

    5. 创建表

    --- create table 表名(

    ​	列名 列类型，

    ​	...

    );

    6. 查看表结构

    --- desc 表名；

### MySQL的语法规范

 1. 不区分大小写，但建议关键字大写，表名、列名小写

 2. 每条命令最好用分号结尾

 3. 每条命令可根据需要进行缩进和换行

    类似于

    select

    *

    from

    这条命令等同于

    select * from

    4. 注释

    单行注释：#注释文字 

    多行注释 ：/\*注释文字\*/

### Navicat常用快捷键

1. ctrl+q           打开查询窗口
2. ctrl+/            注释sql语句
3. ctrl+shift +/  解除注释
4. ctrl+r           运行查询窗口的sql语句
5. ctrl+shift+r   只运行选中的sql语句
6. F6               打开一个mysql命令行窗口
7. ctrl+l            删除一行
8. ctrl+n           打开一个新的查询窗口
9. ctrl+w          关闭一个查询窗口

---



### 查询

#### 基础查询

1. 语法：select 查询列表 from 表名

   1. 查询单字段：select 字段名 from 表名

   2. 查询多字段：select 字段1,字段2 from 表名

   3. 查询所有字段：select * from 表名

   4. 起别名：select 字段名 as 新的名字 from 表名  (as可省略,若别名有特殊符号建议加双引号),

      为表起别名后,查询的字段不能用原来的表名去限定

   5. 若要拼接字段可以使用CONCAT函数,对NULL值进行转化可使用IFNULL函数,寻找不重复字段可以使用DISTINCT函数

2. 特点：

   ①通过select查询完的结果 ，是一个虚拟的表格，不是真实存在
   ② 要查询的东西 可以是常量值、可以是表达式、可以是字段、可以是函数

#### 条件查询

1. 语法：select 查询列表 from 表名 where 筛选条件

2. 筛选条件分类：

   1. 按条件表达式筛选

      简单条件运算符：>   <  =  !=  <>  >=  <=

   2. 按逻辑表达式筛选

      逻辑运算符：&&  ||  ！  and   or  not

   3. 模糊查询

      like:一般与通配符搭配使用，查询等于所给内容的数据,可以使用通配符

      通配符：%  任意多个字符，包含0个字符

      ​				 _   任意单个字符

      between and：查询所给区间内的数据

      in：等价于=,判断某字段的值是否属于in列表中的某一项,其中in列表的值类型必须统一或兼容。

      is null：普通的=号无法判断NULL值,所以需要用到is null,而<=>(安全等于)可以判断所有值类型,但是可读性较差

#### 排序查询

​		语法：select 查询列表 from 表 where + 筛选条件 order by (asc升序,desc降序)

---



### 常见函数

#### 单行函数

#### 字符函数

1. length:SELECT   LENGTH ('参数')       ---用于获取参数的字节个数

2. concat:SELECT   CONCAT('参数1','参数2',...)  FROM  表名       ---拼接字符串

3. upper、lower:SELECT   UPPER('参数')         ---将参数变成大写或小写

4. substr、substring:SELECT   SUBSTR('文本串','索引','长度')      ---获取子串,这里的索引从1开始

5. instr:SELECT   INSTR('文本串','子串')        ---返回子串在文本串的起始索引,若找不到则返回0

6. trim:SELECT   TRIM('文本串')          ---去掉文本串前后的空格

   SELECT    TRIM('想去掉的字符'   FROM   '文本串' )         ---去掉文本串前后的想去掉的字符

7. lpad,rpad:SELECT   LPAD('文本串','长度','填充字符')            ---用指定的字符实现填充指定长度

8. replace:SELECT   REPLACE('文本串','原字符串','替换后的字符串')         ---将文本串中的原字符串替换成新的字符串

#### 数学函数

1. round:SELECT    ROUND(数值,小数点后保留的位数)               ---四舍五入
2. ceil:SELECT   CEIL(数值)                    ---向上取整
3. floor:SELECT   FLOOR(数值)              ---向下取整
4. truncate:SELECT   TRUNCATE(数值,'小数点后保留的位数')              ---截断,即后面的数直接舍
5. mod:SELECT    MOD(a,b)           ---取模操作

#### 日期函数

1. now:SELECT    NOW()              ---返回当前系统日期+时间

2. curdate:SELECT    CURDATE()          ---返回当前系统日期

3. curtime:SELECT   CURTIME()        ---返回当前时间,不包含日期

4. datediff:SELECT   DATEDIFF('参数1','参数2')    FROM   表名        ---返回参数一和二相差的天数

5. year,month,day,hour,minute,second:SELECT    YEAR('传入的时间')           ---可以获取指定的部分,年、月、日、小时、分钟、秒,yearname这种函数表示显示的是英文方式

6. str_to_date:SELECT    STR_TO_DATE('字符','指定格式')   ---将字符通过指定格式转化成日期

   date_format:SELECT    DATE_FORMAT('日期','指定格式')     ---将日期转化成字符

   <img src="C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200823104313780.png" a   lt="image-20200823104313780" style="zoom:50%;" />

#### 流程控制函数

1. if:SELECT   IF('条件表达式','结果为true时的返回值','结果为false时的返回值')       ---起到if else的效果

2. case:SELECT   salary,

   CASE   +   用来判断的字段或表达式

   WHEN   常量1   THEN   要显示的值1或语句1

   WHEN   常量2   THEN   要显示的值2或语句2

   WHEN   常量3   THEN   要显示的值3或语句3

   ELSE   要显示的值n或语句n

   END

   第二种表示方法

   CASE

   WHEN   条件1   THEN   要显示的值1或语句1

   WHEN   条件2   THEN   要显示的值2或语句2

   ELSE    要显示的值n或语句n

   END

#### 分组函数

​            下列函数使用过程中均忽略NULL

1. sum:SELECT	SUM('字段')	FROM	表名

   与distinct搭配使用:SELECT   SUM(DISTINCT  +  字段名)   FROM   表名

2. avg:SELECT	AVG('字段')	FROM	表名

3. min:SELECT    MIN('字段')	FROM	表名

4. max:SELECT	MAX('字段')	FROM	表名

5. count:SELECT	COUNT('字段')	FROM    表名

   SELECT	COUNT(*)	FROM	表名        ---统计行数

---

### 分组查询

1. 语法:SELECT	分组函数,列(要求出现在group by 的后面)	FROM	表名

   ​		  WHERE	+	分组前筛选条件

   ​		  GROUP BY	分组的列表

   ​		  HAVING	+	分组后筛选条件

   ​		 ORDER BY 	子句

---

### 连接查询

1. sql92语法

   1. 内连接

      1. 等值连接

         语法：SELECT	字段1	字段2	...

         ​			FROM	表1	表2	...

         ​			WHERE	+	表1.key=表2.key

      2. 非等值连接

         与等值连接不同的是他的连接条件往往不是a=b

      3. 自连接

         自连接往往是对同一张表里的数据进行操作,所以通常会将1张表起两个别名

2. sql99语法

   1. 内连接

      语法:SELECT	查询列表

      ​		  FROM	表1

      ​		  INNER	JOIN	表2	ON	连接条件

      ​		  INNER	JOIN	表3	ON	连接条件

      内连接种类的不同连接条件与92类似

      1. 等值连接
      2. 非等值连接
      3. 自连接

   2. 外连接

      用于查询一个表中有另一个表中没有的记录

      特点：

      外连接的查询结果为主表中的所有记录

      如果从表中有和他匹配的,则显示匹配值

      若没有则显示NULL

      1. 左外连接

         LEFT	OUTER	JOIN		---LEFT JOIN 左边的是主表

      2. 右外连接

         RIGHT	OUTER	JOIN		---RIGHT JOIN 右边的是主表

      3. 全外连接

         全外连接=内连接的结果+表1有但是表2没有+表2有但是表1没有

   3. 交叉连接

      CROSS JOIN	实现笛卡尔积

---

### 子查询

出现在其他语句内部的SELECT语句,称为子查询或者内查询,内部嵌套其他SELECT语句的查询,称为外查询或主查询

1. WHERE或HAVING后的子查询

   特点:子查询放在小括号内、子查询一般放在条件的右侧,子查询的执行在主查询之前

   1. 标量子查询(单行子查询)           ---一般跟> <  <>   = >=  <=等单行操作符搭配使用

      SELECT

      字段名

      FROM

      表名

      WHERE

      字段名 + 单行操作符 (

      ​	加一个子查询

      )

   2. 列子查询(多行子查询)                ---一般搭配着IN、ANY、SOME、ALL等多行操作符使用

      SELECT 

      字段名

      FROM

      表名

      WHERE

      字段名	IN/ALL/ANY/SOME(

      ​	加一个子查询

      )

   3. 行子查询(多列多行)

      SELECT

      字段名

      FROM

      表名

      WHERE(字段1,字段2)=(

      ​	SELECT  条件1,条件2(与上面的字段相对应)

      ​	FROM

      ​	表名

      )

2. SELECT后面的子查询 

   SELECT

   (SELECT ...)

   FROM

   表名

3. FROM后面的子查询                       ---即从新表中获取内容

   SELECT 

   字段名

   FROM(

   子查询

   )

   ...

---

### 分页查询

​		语法:SELECT	查询列表

​				  ON	连接条件

​				  WHERE	筛选条件

​				  GROUP BY  分组字段

​				  HAVING	分组后的筛选

​				  ORDER BY	排序的字段

​				  LIMIT	OFFSET,SIZE;

​				  OFFSET要显示条目的起始索引(起始索引从0开始)

​				  SIZE	要显示的条目个数

---

### 联合查询

​	用来合并多张表的查询结果,要求多条查询语句的查询列数一致,要求多条查询语句的查询的每一列的类型和顺序最好一致,union关键字默认去重,如果使用union all则可以包含重复项

​	语法:SELECT  FROM 表1

​			 UNION

​			 SELECT FROM 表2’

---

### 插入语句

​	语法:INSERT  INTO   表名(列名,...)	values(值1,...),

​	若省略列名,则默认为所有列,该方式支持插入多行,支持子查询

​	如INSERT	INTO	表名	SELECT ... FROM ...

​	语法2:INSERT	INTO	表名

​				SET	列名=值...

​	插入的值类型要与列的类型一致或者兼容

---

### 修改语句

​	语法:UPDATE	表名

​			 SET	列=新值

​			WHERE	筛选条件

​	修改多表的记录

​		sql92语法:

​			UPDATE	表1,表2

​			SET	列=值

​			WHERE	连接条件

​			AND	筛选条件

​		sql99语法:

​			UPDATE	表1

​			INNER|LEFT|RIGHT	JOIN	表2

​			ON	+	连接条件

​			SET	列=值,

​			WHERE	+	筛选条件

---

### 删除语句

​	语法:DELETE

​			  FROM	表名

​			  WHERE	+	筛选条件

​	语法2:TRUNCATE	+	表名	(删除表中所有内容)

​	多表删除的方式与多表修改类似

​	假如删除表中有自增长列,若用DELETE删除,在插入数据,自增长列的值从断点开始,而TRUNCATE删除后,在插入数据,自增长列的值从1开始,DELETE删除有返回值,TRUNCATE没有,TRUNCATE删除不能回滚,DELETE删除可以回滚	

---

### 库的管理

1. 库的创建

   语法:CREATE	DATABASE	[IF NOT EXISTS]	库名;	(若无则创建,若没[]中语句,则直接创建)

2. 更改库的字符集

   语法:ALTER	DATABASE	BOOKS	CHARACTER	SET	...

3. 库的删除

   DROP	DATABASE	[IF EXISTS]	BOOKS

---

### 表的管理

1. 表的创建

   CREATE	TABLE	表名(列名	列的类型【(长度)	约束】)

2. 表的修改

   1. 修改列名

      ALTER	TABLE	表名	CHANGE	COLUMN	列名	新列名+类型

   2. 修改列的类型或约束

      ALTER	TABLE	表名	MODIFY	COLUMN	列名	类型	约束

   3. 添加新列

      ALTER	TABLE	表名	ADD	COLUMN	列名	类型	约束

   4. 删除列

      ALTER	TABLE	表名	DROP	COLUMN	列名

   5. 修改表名

      ALTER	TABLE	表名	RENAME	TO	新名

3. 表的删除

   DROP	TABLE	表名

4. 表的复制

   1. 复制表的结构

      CREATE	TABLE	表名	LIKE	表名

   2. 复制表的结构+数据

      CREATE	TABLE	表名	SELECT	*	FROM	表名

   3. 复制部分字段

      CREATE	TABLE	表名	SELECT	字段	FROM	表名	WHERE	0

---

### 常见数据类型

​	整形、浮点型 、字符型、日期型

1. 整形

   若要设置无符号,在类型后面加上UNSIGNED即可

   若要实现填充,在类型后面加上ZEROFILL,例如INT(7)	ZEROFILL	则所有数据若不满7位在前面补0

   <img src="C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200901165247799.png" alt="image-20200901165247799"  />	

2. 小数

   FLOAT(M,D),M代表整数部位+小数部位,D代表小数部位,若超过范围则插入临界值,M和D可省略,如果是DECIMAL,M默认为10,D默认为0

   ![image-20200901170000732](C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200901170000732.png)

3. 字符型

   char空间耗费较多,效率较高,varchar空间耗费较少,效率较低

   ![image-20200901170832384](C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200901170832384.png)

   较长的字符串一般用test来保存

   

4. 日期型

   ![image-20200901171345943](C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200901171345943.png)

   date只保存日期

   time只保存时间

   year只保存年

   datetime保存日期+时间	不受时区影响

   timestamp保存日期+时间	受时区影响

---

### 常见约束

​	一种限制,用于限制表中的数据,为了保证表中的数据的准确和可靠性

六大约束:

​	NOT	NULL:非空约束,用于保证该字段的值不能为空,比如姓名,学号等

​	DEFAULT:默认,用于保证该字段有默认值

​	PRIMARY	KEY:主键,用于保证该字段值有唯一性,并且非空

​	UNIQUE:用于保证字段值有唯一性,可以为空

​	CHECK:检查约束

​	FOREIGN	KEY:外键,用于限制两个表的关系,用于保证该字段的值必须来自于主表的关联列的值,要求在从表设置外键关系,从表的外键列的类型和主表的关联列的类型要求一致或兼容,名称无要求,主表的关联列必须是一个key(一般是主键或唯一),插入数据时,插入数据时,先插入主表,再插入从表,删除数据时,先删除从表,再删除主表

---

### 创建表时添加约束

1. 创建表时添加列级约束

   直接在字段名和类型后面追加约束类型即可

   只支持默认、非空、主键、唯一

2. 创建表时添加表级约束

   CONSTRAINT	约束名	约束类型(字段名)

   不起名则直接使用约束类型(字段名)

---

### 主键和唯一的对比

主键:唯一且非空,至多有1个,允许 组合(如INT(a,b),这里以(a,b)为主键)

唯一:唯一但不一定非空,可以有多个,允许组合

---

### 修改表时添加约束

1. 添加非空元素

   ALTER	TABLE	表名	MODIFY	COLUMN	列名	类型	NOT	NULL

2. 添加默认约束

   ALTER	TABLE	表名	MODIFY	COLUMN	列名	类型	DEFAULT	值

3. 添加主键

   ALTER	TABLE	表名	MODIFY	COLUMN	列名	类型	PRIMARY	KEY

​           ....

---

### 修改表时删除约束

​	ALTER TABLE	表名	DROP	约束	(字段名)

---

### 标识列(自增长列)

标识列必须是一个key,一个表中至多有一个标识列,标识列的类型只能是数值型,标识列可通过

SET	AUTO_INCREMENT=?设置步长,也可通过手动插入值设置起始值

语法:在约束后面加上	AUTO_INCREMENT

---

### 事务

![image-20200902064147911](C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200902064147911.png)

事务的ACID属性

1. 原子性

   原子性指事务是一个不可分割的工作单位,要么都发生,要么都不发生

2. 一致性

   事务必须使数据库从一个一致性状态变成另一个一致性状态

3. 隔离性

   一个事务的执行不能被其他事务所干扰,并发执行的各个事务之间不能相互干扰

4. 持久性

   一个事务一旦被提交,就是对数据库永久的改变

事务的创建

1. 隐式事务

   事务没有明显的开始和结束标记,如update,insert,delete语句

2. 显式事务

   事务具有明显的开始和结束的标记,前提是必须先设置自动提交功能为禁用

   步骤1:开启事务

   SET	AUTOCOMMIT=0

   START	TRANSACTION

   步骤2:编写事务中的sql语句

   步骤3:结束事务

   commit	提交事务

   rollback	回滚事务,可与保存点配合使用,语法为先设置保存点:SAVEPOINT	名称,然后ROLLBACK	TO	名称

### 事务的并发问题以及事务隔离级别

![image-20200902072054658](C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200902072054658.png)

![image-20200902071941153](C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200902071941153.png)

查看当前的隔离级别:SELECT	@@tx_isolation

设置当前MYSQL的隔离级别:SET	TRANSACTION	ISOLATION	LEVEL	+	事务隔离级别

设置数据库系统的全局的隔离级别:SET	GLOBAL	TRANSACTION	ISOLATION	LEVEL	+	事务隔离级别

---

### 视图

视图只保存SQL逻辑,不保存查询结果,一般当多个地方用到同样的查询结果且查询语句较复杂时会用到视图,可以简化复杂的SQL操作,保护数据,提高了安全性

1. 视图的创建

   语法:

   CREATE	VIEW	视图名

   AS

   查询语句

2. 视图的修改

   1. CREATE	OR	REPLACE	VIEW	视图名

      AS

      查询语句

   2. ALTER    VIEW      视图名

      AS

      查询语句

3. 视图的查看和删除

   删除:DROP	VIEW	视图名

   查看:SHOW	CREATE	VIEW	视图名

4. 视图的更新

   ![image-20200902080929506](C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200902080929506.png)

   

---

### 变量

1. 系统变量

   1. 全局变量

      查看所有全局变量:SHOW GLOBAL	VARIABLES

      查看部分全局变量:SHOW GLOBAL	VARIABLES	LIKE

      查看指定全局变量:SELECT	@@GLOBAL.变量名

      为全局变量赋值:SET	@@GLOBAL.变量名=值

   2. 会话变量

      作用域:仅针对于当前会话(连接)有效

      语法与全局变量类似,只不过把GLOBAL关键字改成SESSION

2. 自定义变量

   使用步骤:声明	赋值	使用

   1. 用户变量

      作用域:针对于当前会话(连接)有效,同会话变量的作用域

      1. 声明并初始化

         SET	@用户变量名=值

         SET	@用户变量名:=值

         SELECT	@用户变量名:=值

      2. 赋值

         SET	@用户变量名=值

         SET	@用户变量名:=值

         SELECT	@用户变量名:=值

         SELECT	字段	INTO	变量名	FROM	表

      3. 查看用户变量的值

         SELECT	@用户变量名

   2. 局部变量

      作用域:仅在它定义的begin	end中有效

      1. 声明

         DECLARE	变量名	类型

         DECLARE	变量名	类型	DEFAULT	值

      2. 赋值

         SET	局部变量名=值

         SET	局部变量名:=值

         SELECT	@局部变量名:=值

         SELECT	字段	INTO	变量名	FROM	表

      3. 使用

         SELECT	局部变量名

---

### 存储过程和函数

​	类似于JAVA中的方法,存储过程可以有0个返回,也可以有多个返回,适合做批量插入、批量更新,而函数有且仅有1个返回,适合做处理数据过后最后的结果

1. 存储过程

   含义:一组预先编译好的SQL语句集合,可理解成批处理语句

   优点:提高代码重用性,简化操作,减少了编译次数并且减少了和数据库服务器的连接次数,提高了效率

   1. 创建语法:

      CREATE	PROCEDURE	存储过程名(参数列表)

      BEGIN

      ​	存储过程体(合法有效的SQL语句)

      END

      参数列表包含三部分,参数模式、参数名、参数类型

      ```
      1、需要设置新的结束标记
      delimiter 新的结束标记
      示例：
      delimiter $
      
      CREATE PROCEDURE 存储过程名(IN|OUT|INOUT 参数名  参数类型,...)
      BEGIN
      	sql语句1;
      	sql语句2;
      
      END $
      
      2、存储过程体中可以有多条sql语句，如果仅仅一条sql语句，则可以省略begin end
      
      3、参数前面的符号的意思
      in:该参数只能作为输入 （该参数不能做返回值）
      out：该参数只能作为输出（该参数只能做返回值）
      inout：既能做输入又能做输出
      ```

   2. 调用语法:

      CALL	存储过程名(实参列表);

   3. 删除语法:

      DROP	PROCEDURE	存储名称

   4. 查看语法

      SHOW	CREATE	PROCEDURE	存储名;

2. 函数

   1. 创建语法:

      CREATE	FUNCTION	函数名(参数列表)	RETURNS	返回类型

      BEGIN

      ​	函数体

      END

      参数列表包含参数名和参数类型,必须要有RETURN 语句

   2. 调用语法

      SELECT	函数名(参数列表)

   3. 删除语法

      DROP	FUNCTION	函数名

   4. 查看语法

      SHOW	CREATE	FUNCTION	函数名

---

### 分支结构

一、if函数
	语法：if(条件，值1，值2)
	特点：可以用在任何位置

二、case语句

语法：

	情况一：类似于switch
	case 表达式
	when 值1 then 结果1或语句1(如果是语句，需要加分号) 
	when 值2 then 结果2或语句2(如果是语句，需要加分号)
	...
	else 结果n或语句n(如果是语句，需要加分号)
	end 【case】（如果是放在begin end中需要加上case，如果放在select后面不需要）
	
	情况二：类似于多重if
	case 
	when 条件1 then 结果1或语句1(如果是语句，需要加分号) 
	when 条件2 then 结果2或语句2(如果是语句，需要加分号)
	...
	else 结果n或语句n(如果是语句，需要加分号)
	end 【case】（如果是放在begin end中需要加上case，如果放在select后面不需要）


特点：
	可以用在任何位置

三、if elseif语句

语法：

	if 情况1 then 语句1;
	elseif 情况2 then 语句2;
	...
	else 语句n;
	end if;

特点：
	只能用在begin end中！！！！！！！！！！！！！！！

三者比较：
			应用场合
	if函数		简单双分支
	case结构	等值判断 的多分支
	if结构		区间判断 的多分支

---

### 循环结构

分类:while、loop、repeat

循环控制:iterate类似于continue,leave类似于break

语法：


	【标签：】WHILE 循环条件  DO
		循环体
	END WHILE 【标签】;
	【标签：】LOOP
		循环体
	END LOOP 【标签】;
	【标签：】REPEAT 
		循环体
		UNTIL	结束循环的条件
	END REPEAT 【标签】;

特点：

	只能放在BEGIN END里面
	
	如果要搭配leave跳转语句，需要使用标签，否则可以不用标签
	
	leave类似于java中的break语句，跳出所在循环！！！



