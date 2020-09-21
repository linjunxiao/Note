### 常见的配置错误问题

* 问题:<img src="C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200907160359084.png" alt="image-20200907160359084" style="zoom:67%;" />

  解决方案:

  <img src="C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200907160450481.png" alt="image-20200907160450481" style="zoom:67%;" />

  <img src="C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200907160541882.png" alt="image-20200907160541882" style="zoom:67%;" />

  <img src="C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200907160617089.png" alt="image-20200907160617089" style="zoom: 67%;" />

  之后会遇到这样的问题:

  <img src="C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200907160700474.png" alt="image-20200907160700474" style="zoom:67%;" />

  此时在这个界面配置一下jdk

  <img src="C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200907160750208.png" alt="image-20200907160750208" style="zoom:67%;" />

* 解决复制IDEA项目中出现的路径报错问题

  ![image-20200906194420378](C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200906194420378.png)

  在如图所示处修改

  最后<img src="C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200907160541882.png" alt="image-20200907160541882" style="zoom:67%;" />

  <img src="C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200907161033863.png" alt="image-20200907161033863" style="zoom:67%;" />

  发现程序可以正常运行！解决问题

### 第一个mybatis程序

![]()

#### 2.1、搭建环境

1. 新建一个普通的maven项目

2. 删除src目录

3. 导入maven依赖

   ```
    <!--导入依赖-->
       <dependencies>
           <!--mysql驱动-->
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
               <version>5.1.47</version>
           </dependency>
   
           <!--mybatis-->
           <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
           <dependency>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis</artifactId>
               <version>3.4.6</version>
           </dependency>
   
           <!--junit-->
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.12</version>
           </dependency>
       </dependencies>
   ```
   
   若IDEA中maven配置项目resource下的配置文件不能够输出到target,即出现Couldn't find ...这样的错误时,可在pom.xml中加入以下配置
   
   ```
   <build>
           <resources>
               <resource>
                   <directory>src/main/resources</directory>
                   <includes>
                       <include>**/*.properties</include>
                       <include>**/*.xml</include>
                       <include>**/*.tld</include>
                       <include>**/*.xls</include>
                       <include>**/*.xlsx</include>
                   </includes>
               </resource>
               <resource>
                   <directory>src/main/java</directory>
                   <includes>
                       <include>**/*.properties</include>
                       <include>**/*.tld</include>
                       <include>**/*.xls</include>
                       <include>**/*.xlsx</include>
                       <include>**/*.xml</include>
                   </includes>
               </resource>
           </resources>
       </build>
   ```
   
   

#### 2.2、创建模块

* 编写mybatis的核心配置文件

  ```
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE configuration
          PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-config.dtd">
  <!--configration核心配置文件-->
  <configuration>
      <environments default="development">
          <environment id="development">
              <transactionManager type="JDBC"/>
              <dataSource type="POOLED">
                  <property name="driver" value="com.mysql.jdbc.Driver"/>
                  <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
                  <property name="username" value="root"/>
                  <property name="password" value="ljx3687521;"/>
              </dataSource>
          </environment>
      </environments>
      <!--每一个Mapper.xml都需要在mybatis核心配置文件中注册-->
      <mappers>
          <mapper resource="com/xiao/dao/UserMapper.xml"/>
      </mappers>
  </configuration>
  ```

* 编写mybatis的工具类

  ```
  public class MybatisUtils {
      private static SqlSessionFactory sqlSessionFactory;
      static {
          try{
              //第一步，获取SqlSessionFactory对象
              String resource = "mybatis-config.xml";
              InputStream inputStream = Resources.getResourceAsStream(resource);
              sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
          }catch (IOException e){
              e.printStackTrace();
          }
      }
      //SqlSession完全包含了面向数据库执行sql命令所需要的所有方法
      public static SqlSession getSqlSession(){
          return sqlSessionFactory.openSession();
          
      }
  }
  
  ```

#### 2.3、编写代码

* 实体类

  ```
  public class User {
      private int id;
      private String name;
      private String pwd;
      public User(){
  
      }
      public User(int id, String name, String pwd) {
          this.id = id;
          this.name = name;
          this.pwd = pwd;
      }
  
      public int getId() {
          return id;
      }
  
      public void setId(int id) {
          this.id = id;
      }
  
      public String getName() {
          return name;
      }
  
      public void setName(String name) {
          this.name = name;
      }
  
      public String getPwd() {
          return pwd;
      }
  
      public void setPwd(String pwd) {
          this.pwd = pwd;
      }
  
      @Override
      public String toString() {
          return "User{" +
                  "id=" + id +
                  ", name='" + name + '\'' +
                  ", pwd='" + pwd + '\'' +
                  '}';
      }
  }
  
  ```

  

* Dao接口

  ```
  public interface Userdao {
      List<User> getUserList();
  }
  
  ```

  

* 接口实现类由原来的UserDaoImpl转变成一个Mapper配置文件

  ```
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <!--namespace=绑定一个对应的Dap/mapper接口-->
  <mapper namespace="com.xiao.dao.Userdao">
  	//	在这里编写对应的sql语句
      <select id="getUserList" resultType="com.xiao.pojo.User">
      select * from mybatis.user
    </select>
  </mapper>
  ```

#### 2.4、junit测试

```
public class UserdaoTest {
    @Test
    public void test(){
        //第一步：获得sqlSession对象

        SqlSession sqlSession = MybatisUtils.getSqlSession();
        //执行sql
        Userdao userDao = sqlSession.getMapper(Userdao.class);
        List<User>userList = userDao.getUserList();
        for(User user : userList){
            System.out.println(user);
        }
        sqlSession.close();
    }
}

```

### Mybatis执行流程剖析

<img src="D:\firefoxDownload\未命名文件.png" style="zoom:50%;" />

### 增删改查

#### 1、namespace

namespace中的包名要和DAO/MAPPER接口的包名一致

#### 2、select

选择、查询语句

* id:就是对应的namespace中的方法名,相当于重写了原来的方法
* resultType:sql语句执行的返回值
* parameterType:参数类型

1. 编写接口
2. 编写对应的mapper中的sql语句
3. 测试

#### 3、insert

#### 4、update

#### 5、delete

增删改需要提交事务,即sqlsession.commit()

#### 6、map

假设我们的实体类或者数据库中的表字段或者参数过多时,我们可以考虑使用map

```
<insert id="addUser" parameterType="map">
		//#{}中的是map中的key
        insert into mybatis.user (id,name,pwd) values (#{userid},#{username},#{userpwd})
</insert>
```

Map传递参数,直接在sql中取key

对象传递参数,直接在sql中取对象的属性

只有一个基本类型参数的情况下,可以直接在sql中取到

### 配置解析

#### 1、核心配置文件

* mybatis-config.xml

* Mybatis的配置文件包含了会深深影响Mybatis行为的设置和属性信息

  ```
  configuration（配置）
  properties（属性）
  settings（设置）
  typeAliases（类型别名）
  typeHandlers（类型处理器）
  objectFactory（对象工厂）
  plugins（插件）
  environments（环境配置）
  environment（环境变量）
  transactionManager（事务管理器）
  dataSource（数据源）
  databaseIdProvider（数据库厂商标识）
  mappers（映射器）
  ```

#### 2、环境配置（environments）

MyBatis 可以配置成适应多种环境

**不过要记住：尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境。**

Mybatis默认的事务管理器就是JDBC,连接池:POOLED

#### 3、属性(properties)

可通过properties属性来实现引用配置文件

这些属性可外部配置 且动态替换,可在java属性文件中配置 ,也可通过properties元素的子元素来传递

编写配置文件

db.properties

```
driver = com.mysql.jdbc.Driver
url = jdbc:mysql://localhost:3306/mybatis?useSSL=true&useUnicode=true&characterEncoding=UTF-8
username = root
password = ljx3687521;
```

在核心配置文件中引入

```
<properties resource="db.properties">
	<property>...<property>
</properties>
```

若两文件有同一字段,外部配置文件的优先级最高

可在其中增加一些属性配置

#### 4、类型别名（TypeAliases）

类型别名可为 Java 类型设置一个缩写名字。 它仅用于 XML 配置，意在降低冗余的全限定类名书写

```
<!--给实体类起别名-->
    <typeAliases>
        <typeAlias type="com.xiao.pojo.User" alias="User"></typeAlias>
    </typeAliases>
```

也可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean，比如：

```
<package name="com.xiao.pojo"/>
```

那么这个java类型的别名就是为这个类的类名,首字母小写,若有注解,则别名为注解名

在实体类比较少的情况下使用第一种,若实体类比较多则用第二种

#### 5、设置（settings）

#### 6、其他配置

#### 7、映射器(mappers)

```
<!-- 使用相对于类路径的资源引用,每一个Mapper.xml文件都需要在核心配置文件中注册 -->
<mappers>
  <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
  <mapper resource="org/mybatis/builder/BlogMapper.xml"/>
  <mapper resource="org/mybatis/builder/PostMapper.xml"/>
</mappers>
```

```
<!-- 使用映射器接口实现类的完全限定类名 -->
<mappers>
  <mapper class="org.mybatis.builder.AuthorMapper"/>
  <mapper class="org.mybatis.builder.BlogMapper"/>
  <mapper class="org.mybatis.builder.PostMapper"/>
</mappers>
```

使用映射器接口实现类的注意点

* 接口和Mapper配置文件必须同名
* 接口和Mapper配置文件必须在同一包下

```
<!-- 将包内的映射器接口实现全部注册为映射器 -->
<mappers>
  <package name="org.mybatis.builder"/>
</mappers>
```

该方法的注意点与方式二一致

#### 8、生命周期和作用域

不同作用域和生命周期类别是至关重要的，因为错误的使用会导致非常严重的并发问题。

SqlSessionFactoryBuilder:一旦创建了 SqlSessionFactory，就不再需要它了。 因此 SqlSessionFactoryBuilder 实例的最佳作用域是方法作用域（也就是局部方法变量）。

SqlSessionFactory:可以认为是数据库连接池

SqlSessionFactory 一旦被创建就应该在应用的运行期间一直存在，没有任何理由丢弃它或重新创建另一个实例。 使用 SqlSessionFactory 的最佳实践是在应用运行期间不要重复创建多次，多次重建 SqlSessionFactory 被视为一种代码“坏习惯”。因此 SqlSessionFactory 的最佳作用域是**应用作用域**。 有很多方法可以做到，最简单的就是使用单例模式或者静态单例模式。

SqlSession:可以认为是连接到连接池的一个请求

每个线程都应该有它自己的 SqlSession 实例。SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的最佳的作用域是**请求或方法作用域**。 绝对不能将 SqlSession 实例的引用放在一个类的静态域，甚至一个类的实例变量也不行。 也绝不能将 SqlSession 实例的引用放在任何类型的托管作用域中，比如 Servlet 框架中的 HttpSession。用完之后就要关闭

### 解决属性名和字段名不一致的问题

当实体类和字段不一致时

```
private int id;
    private String name;
    private String password;
```

![image-20200907192111774](C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200907192111774.png)

会出现这种情况

解决方法:使用resultMap

```
 <resultMap id="UserMap" type="User">
        <!--column对应的是数据库中的字段,property对应的是类中的属性-->
        <result column="pwd" property="password"></result>
        <result column="id" property="id"></result>
        <result column="name" property="name"></result>
 </resultMap>
  <select id="getUserById" parameterType="int" resultMap="UserMap">
        select * from mybatis.user where id = #{id}
  </select>
```

* `resultMap` 元素是 MyBatis 中最重要最强大的元素

* ResultMap 的设计思想是，对简单的语句做到零配置，对于复杂一点的语句，只需要描述语句之间的关系就行了。

### 日志

#### 1、日志工厂

如果一个数据库操作,出现了异常,我们需要排错,就需要用到日志

Mybatis 通过使用内置的日志工厂提供日志功能。内置日志工厂将会把日志工作委托给下面的实现之一：

- SLF4J
- Apache Commons Logging
- Log4j 2
- Log4j            【掌握】
- JDK logging
- COMMONS_LOGGING
- STDOUT_LOGGING           【掌握】
- NO_LOGGING

在mybatis的核心配置文件中配置日志

STDOUT_LOGGING:标准日志输出

```
 <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
 </settings>
```

输出结果为:

```
Opening JDBC Connection
Created connection 278934944.
Setting autocommit to false on JDBC Connection [com.mysql.jdbc.JDBC4Connection@10a035a0]
==>  Preparing: select * from mybatis.user 
==> Parameters: 
<==    Columns: id, name, pwd
<==        Row: 2, 关羽, 123456
<==        Row: 3, 刘备, 123456
<==        Row: 4, 阿骁, 123456
<==      Total: 3
User{id=2, name='关羽', pwd='123456'}
User{id=3, name='刘备', pwd='123456'}
User{id=4, name='阿骁', pwd='123456'}
Resetting autocommit to true on JDBC Connection [com.mysql.jdbc.JDBC4Connection@10a035a0]
Closing JDBC Connection [com.mysql.jdbc.JDBC4Connection@10a035a0]
Returned connection 278934944 to pool.
```

#### 2、log4J

Log4j是[Apache](https://baike.baidu.com/item/Apache/8512995)的一个开源项目，通过使用Log4j，我们可以控制日志信息输送的目的地是[控制台](https://baike.baidu.com/item/控制台/2438626)、文件、[GUI](https://baike.baidu.com/item/GUI)组件，甚至是套接口服务器、[NT](https://baike.baidu.com/item/NT/3443842)的事件记录器、[UNIX](https://baike.baidu.com/item/UNIX) [Syslog](https://baike.baidu.com/item/Syslog)[守护进程](https://baike.baidu.com/item/守护进程/966835)等；我们也可以控制每一条日志的输出格式；通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程。

* 导包

  ```
  <!-- https://mvnrepository.com/artifact/log4j/log4j -->
          <dependency>
              <groupId>log4j</groupId>
              <artifactId>log4j</artifactId>
              <version>1.2.17</version>
          </dependency>
  ```

* 配置log4j.properties

  ```
  log4j.rootLogger=DEBUG,console,file
  
  # 控制台(console)
  log4j.appender.console=org.apache.log4j.ConsoleAppender
  log4j.appender.console.Threshold=DEBUG
  log4j.appender.console.Target=System.out
  log4j.appender.console.layout=org.apache.log4j.PatternLayout
  log4j.appender.console.layout.ConversionPattern=[%c]-%m%n
  #文件输出的相关设置
  log4j.appender.file  = org.apache.log4j.RollingFileAppender
  log4j.appender.file.File = ./log/xiao.log
  log4j.appender.file.MaxFileSize = 10mb
  log4j.appender.file.Threshold = DEBUG
  log4j.appender.file.layout = org.apache.log4j.PatternLayout
  log4j.appender.file.layout.ConversionPattern = [%p][%d{yy-MM-dd}][%c]%m%n
  #日志输出级别
  log4j.logger.org.mybatis = DEBUG
  log4j.logger.java.sql = DEBUG
  log4j.logger.java.sql.Statement = DEBUG
  log4j.logger.java.sql.ResultSet = DEBUG
  log4j.logger.java.sql.PreparedStatement = DEBUG
  ```

* 配置log4j为日志的实现

  ```
  <settings>
          <setting name="logImpl" value="LOG4J"/>
  </settings>
  ```

* log4j的使用

  在要使用log4j的类中导包 import org.apache.log4j.Logger

  日志对象,参数为当前类的class

  ```
   static Logger logger = Logger.getLogger(UserdaoTest.class)
  ```

  日志级别

  ```
  logger.info("info:进入了testLog4j");
  logger.debug("debug:进入了testLog4j");
  logger.error("error:进入了testLog4j");
  ```

### 分页

减少数据的处理量

sql语句

```
SELECT * from 表 LIMIT startIndex,pageSize
```

RowBounds:通过java代码层面实现分页

```
SqlSession sqlSession = MybatisUtils.getSqlSession();
        RowBounds rowBounds = new RowBounds(1,2);
        List<User> userList = sqlSession.selectList("com.xiao.dao.Userdao.getUserByRowBounds",null,rowBounds);
        for (User user : userList) {
            System.out.println(user);
        }
```

结果:

<img src="C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200908085839156.png" alt="image-20200908085839156" style="zoom: 67%;" />

### 使用注解开发

#### 面向接口编程

![image-20200908090822933](C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200908090822933.png)

![image-20200908090836756](C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200908090836756.png)

![image-20200908090844189](C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200908090844189.png)

#### 使用注解开发

1. 注解在接口上实现
2. 需要在核心配置文件上绑定接口

本质:反射机制实现

底层:动态代理

对于像 BlogMapper 这样的映射器类来说，还有另一种方法来完成语句映射。 它们映射的语句可以不用 XML 来配置，而可以使用 Java 注解来配置。比如，上面的 XML 示例可以被替换成如下的配置：

```
 @Select("select * from user")
    List<User> getUserList();
```

使用注解来映射简单语句会使代码显得更加简洁，但对于稍微复杂一点的语句，Java 注解不仅力不从心，还会让你本就复杂的 SQL 语句更加混乱不堪。 因此，如果你需要做一些很复杂的操作，最好用 XML 来映射语句。

选择何种方式来配置映射，以及认为是否应该要统一映射语句定义的形式，完全取决于你和你的团队。 换句话说，永远不要拘泥于一种方式，你可以很轻松的在基于注解和 XML 的语句映射方式间自由移植和切换。

#### CRUD

可以通过

```
sqlSessionFactory.openSession(true)
```

实现自动提交事务

**关于@Param()注解**

* 基本类型的参数或String类型需要加上
* 引用类型不需要加上
* 如果只有一个基本类型可以忽略,但是建议加上
* 我们在SQL中引用的就是@Param()中设定的属性名

```
 User getUserById(@Param("id") int id);
```

#### Lombok插件

1. 安装Lombok插件

2. 在项目中导入jar包

3. ```
   @Getter and @Setter
   @FieldNameConstants
   @ToString
   @EqualsAndHashCode
   @AllArgsConstructor, @RequiredArgsConstructor and @NoArgsConstructor
   @Log, @Log4j, @Log4j2, @Slf4j, @XSlf4j, @CommonsLog, @JBossLog, @Flogger, @CustomLog
   @Data
   @Builder
   @SuperBuilder
   @Singular
   @Delegate
   @Value
   @Accessors
   @Wither
   @With
   @SneakyThrows
   @val
   @var
   experimental @var
   @UtilityClass
   Lombok config system
   Code inspections
   Refactoring actions (lombok and delombok)
   ```

   ```
   @Data:无参构造、get、set、tostring、hashcode,equals
   @AllArgsConstructor:有参
   @NoArgsConstructor:无参
   ```

### 多对一处理

按照查询嵌套处理

```
<!--
    思路:
    1.查询所有的学生信息
    2.根据查询出来的学生的tid寻找对应的老师   子查询
    -->
    <select id="getStudent" resultMap="StudentTeacher">
        select * from student;
    </select>
    <resultMap id="StudentTeacher" type="Student">
        <result property="id" column="id"></result>
        <result property="name" column="name"></result>
        <association property="teacher" column="tid" javaType="Teacher" select="getTeacher"></association>
    </resultMap>
    <select id="getTeacher" resultType="Teacher">
        select * from teacher where id = #{id}
    </select>
```

按照结果嵌套处理

```
<!--按照结果嵌套处理-->
    <select id="getStudent2" resultMap="StudentTeacher2">
        select s.id sid,s.name sname,t.name tname,t.id tid
        from student s,teacher t
        where s.tid=t.id
    </select>
    <resultMap id="StudentTeacher2" type="Student">
        <result property="id" column="sid"></result>
        <result property="name" column="sname"></result>
        <association property="teacher" javaType="Teacher">
            <result property="name" column="tname"></result>
            <result property="id" column="tid"></result>
        </association>
    </resultMap>
```

### 一对多处理

按照查询嵌套处理

```
<!--按查询嵌套-->
    <select id="getTeacher2" resultMap="TeacherStudent2">
        select * from mybatis.teacher where id = #{tid}
    </select>
    <resultMap id="TeacherStudent2" type="Teacher">
        <result property="id" column="id"></result>
        <result property="name" column="name"></result>
        <collection property="students" column="id" ofType="Student" select="getStudentByTeacherId" javaType="ArrayList"></collection>
    </resultMap>
    <select id="getStudentByTeacherId" resultType="Student">
        select * from mybatis.student where  tid = #{id}
    </select>
```

按照结果嵌套处理

```

    <!--按结果嵌套-->
    <select id="getTeacher" resultMap="TeacherStudent">
        select s.id sid,s.name sname,t.name tname,t.id tid
        from mybatis.student s,mybatis.teacher t
        where s.tid = t.id and t.id = #{tid}
    </select>
    <resultMap id="TeacherStudent" type="Teacher">
        <result property="id" column="tid"></result>
        <result property="name" column="tname"></result>
        <collection property="students" ofType="Student">
            <result property="id" column="sid"></result>
            <result property="name" column="sname"></result>
            <result property="tid" column="tid"></result>
        </collection>
    </resultMap>

```

关联-association(多对一)

集合-collection(一对多)

javaType & ofType

1. JavaType用来指定实体类中属性的类型
2. ofType用来指定映射到List或者集合中的pojo类型,泛型中的约束类型

### 动态SQL

**动态SQL就是指根据不同的条件生成不同的SQL语句**

#### 动态SQL常用的四个标签

1. if

   ```
   <select id="queryBlogIf" resultType="Blog">
           select * from mybatis.blog where 1=1
           <if test="title != null">
               and title = #{title}
           </if>
           <if test="author != null">
               and author = #{author}
           </if>
       </select>
   ```

   

2. choose (when, otherwise)

3. trim (where, set)

   *where* 元素只会在子元素返回任何内容的情况下才插入 “WHERE” 子句。而且，若子句的开头为 “AND” 或 “OR”，*where* 元素也会将它们去除。这样即可避免出现where and这种错误语法。

   ```
   <select id="queryBlogIf" resultType="Blog">
           select * from mybatis.blog
           <where>
               <if test="title != null">
                   and title = #{title}
               </if>
               <if test="author != null">
                   and author = #{author}
               </if>
           </where>
   </select>
   ```

   set关键字

   这个例子中，*set* 元素会动态地在行首插入 SET 关键字，并会删掉额外的逗号（这些逗号是在使用条件语句给列赋值时引入的）。

   ```
   <update id="updateBlog" parameterType="map">
           update mybatis.blog
           <set>
               <if test="title != null">
                   title = #{title},
               </if>
               <if test="author != null">
                   author = #{author},
               </if>
           </set>
           where id = #{id}
       </update>
   ```

   **所谓的动态SQL,本质还是SQL语句,只是我们可以在SQL层面,去执行一个逻辑代码**

4. SQL片段

   有时候,我们可以将一些功能的部分抽取出来,方便复用

   1. 使用SQL标签抽取公共的部分

      ```
      <sql id="if-title-author">
              <if test="title != null">
                  title = #{title}
              </if>
              <if test="author != null">
                  and author = #{author}
              </if>
      </sql>
      <select id="queryBlogIf" resultType="Blog">
              select * from mybatis.blog
              <where>
                 <include refid="if-title-author"></include>
              </where>
      </select>
      ```

   2. 在要用的时候使用include标签即可

5. foreach

   ```
    <select id="queryBlogForeach" parameterType="map" resultType="Blog">
           select * from mybatis.blog
           <where>
               <foreach collection="ids" item="id" open="and (" close=")" 				                 separator="or"> 
               	id = #{id}
               </foreach>
           </where>
    </select>
   ```

### 缓存

#### 简介

一次查询的结果,给他暂存在一个可以直接取到的地方,我们再次查询相同数据的时候,直接走缓存,就不用走数据库了

1. 什么是缓存

   * 存在内存中的临时数据
   * 将用户经常查询的数据放在缓存中,用户去查询就不用从磁盘上查询,从而提高查询效率,解决高并发系统的性能问题

2. 为什么使用缓存

   减少和数据库的交互次数,减少系统开销,提高系统效率

3. 什么样的数据能够使用缓存

   经常查询并且不经常改变的数据

#### mybatis缓存

MyBatis 内置了一个强大的事务性查询缓存机制，它可以非常方便地配置和定制。 

MyBatis系统中默认定义了两级缓存:一级缓存和二级缓存

* 默认情况下只有一级缓存开启(SqlSession级别的缓存,也称为本地缓存)
* 二级缓存需要手动开启和配置,是基于namespace级别的缓存。
* 为了提高拓展性,mybatis定义了缓存接口Cache,我们可以通过实现Cache接口来自定义二级缓存

#### 一级缓存

一级缓存也叫本地缓存:

* 与数据库同一次会话期间查询到的数据会放在本地缓存中
* 以后如果需要获取相同的数据,直接从缓存中拿,没必要再去查询数据库

缓存失效的情况:

1. 查询不同的东西
2. 增删改操作,可能会改变原来的数据,所以必定会刷新缓存
3. 查询不同的Mapper.xml
4. 手动清理缓存

一级缓存是默认开启的,只在一次SqlSession中有效,也就是开启连接到关闭连接这一个区间段

#### 二级缓存

* 二级缓存也叫全局缓存,一级缓存作用域太低了,所以诞生了二级缓存
* 基于namespace级别的缓存,一个名称空间,对应一个二级缓存
* 工作机制
  * 一个会话查询一条数据,这个数据就会被放在当前会话的一级缓存中
  * 如果当前会话关闭了,这个会话对应的一级缓存就没了,但是我们想要的是,会话关闭了,一级缓存中的数据被保存到二级缓存中
  * 新的会话查询信息,就可以从二级缓存中获取内容

默认情况下，只启用了本地的会话缓存，它仅仅对一个会话中的数据进行缓存。 要启用全局的二级缓存，只需要在你的 SQL 映射文件中添加一行：

```
<cache/>
```

基本上就是这样。这个简单语句的效果如下:

- 映射语句文件中的所有 select 语句的结果将会被缓存。
- 映射语句文件中的所有 insert、update 和 delete 语句会刷新缓存。
- 缓存会使用最近最少使用算法（LRU, Least Recently Used）算法来清除不需要的缓存。
- 缓存不会定时进行刷新（也就是说，没有刷新间隔）。
- 缓存会保存列表或对象（无论查询方法返回哪种）的 1024 个引用。
- 缓存会被视为读/写缓存，这意味着获取到的对象并不是共享的，可以安全地被调用者修改，而不干扰其他调用者或线程所做的潜在修改。

步骤:

1. 开启全局缓存

   ```
    <!--显示的开启全局缓存-->
   <setting name="cacheEnabled" value="true"/>
   ```

2. 在要使用二级缓存的Mapper中开启

   ```
    <!--在当前Mapper.xml中使用二级缓存-->
       <cache></cache>
   ```

3. 也可以自定义参数

   ```
   <cache
     eviction="FIFO"
     flushInterval="60000"
     size="512"
     readOnly="true"/>
   ```

使用二级缓存时,我们需要将实体类序列化,否则就会报错

```
Caused by: java.io.NotSerializableException: com.xiao.pojo.User
```

```
public class User implements Serializable{

}
```

小结：

* 只要开启了二级缓存,在同一个Mapper下就有效
* 所有的数据都会先放在一级缓存中
* 只有当会话提交或者关闭的时候,才会提交到二级缓存中

#### 缓存原理

![](D:\firefoxDownload\未命名文件(1).png)