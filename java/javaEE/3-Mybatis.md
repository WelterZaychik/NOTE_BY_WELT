[toc]

# 软件开发常用框架

### 三层结构

- 三层架构包含的三层
  - 界面层（User Interface layer）
    - 用来接收客户端的输入，调用业务逻辑层进行功能处理，返回结果给客户端。过去的servlet就是界面层的功能
  - 业务逻辑层（Business Logic Layer）
    - 用来进行整个项目的业务逻辑处理，向上为界面层提供处理结果，向下为数据访问层要数据。
  - 数据访问层（Data access layer）
    -  专门用来进行数据库的增删改查操作，向上为业务逻辑层提供数据。
- 各层之间的调用顺序是固定的，不允许跨层访问。
  - **客户端<--->界面层<--->业务逻辑层<--->数据访问层<--->数据库。**
- 使用三层结构的优点：
  1. 结构清晰、耦合度低, 各层分工明确
  2. 可维护性高，可扩展性高
  3. 有利于标准化
  4. 开发人员可以只关注整个结构中的其中某一层的功能实现
  5. 有利于各层逻辑的复用

### 常用框架SSM

- Spring：
  - Spring 框架为了解决软件开发的复杂性而创建的。Spring 使用的是基本的 JavaBean 来完成以前非常复杂的企业级开发。Spring 解决了业务对象，功能模块之间的耦合，不仅在 javase,web 中使用， 大部分 Java 应用【移动开发，桌面应用，web开发等等】都提供了很好的解决方案，是一个大佬级别的存在。
  - Spring是整合其他框架的框架，他的核心是IOC和AOP，它由20多个模块构成
- SpringMVC：
  - 他是Spring家族的一员，专门用来优化控制器(Servlet)的.提供了极简单数据提交,数据携带,页面跳转等功能。
  - 为 Spring 框架提供了构建 Web 应用程序的能力。现在可以 Spring 框架提供的 SpringMVC 模块实现 web 应用开发，在 web 项目中可以无缝使用 Spring 和 Spring MVC 框架。
- Mybatis：
  - 是持久化层【数据访问层】的一个框架，用来进行数据库访问的优化，专注于sql语句，极大的简化了JDBC的访问。
  - **MyBatis 是一个优秀的基于 java的持久层框架，内部封装了 jdbc**，开发者只需要关注**sql语句**本身，而不需要处理加载驱动、创建连接、创建 statement、关闭连接，资源等繁杂的过程。
  - MyBatis 通过 xml 或注解两种方式将要执行的各种 sql 语句配置起来，并通过 java 对象和 sql 的动态参数进行映射生成最终执行的 sql 语句，最后由 mybatis 框架执行 sql 并将结果映射为 java 对象并返回。



### 什么是框架

- ### 框架的定义

  - 框架（Framework）是整个或部分系统的可重用设计，表现为一组抽象构件及构件实例间交互的方法;另一种认为，框架是可被应用开发者定制的应用骨架、模板。
  - 简单来说，它是一个半成品软件。将所有的公共的，重复的功能解决掉,帮助程序快速高效的进行开发，它是可复用，可扩展的。

- ### 什么是Mybatis框架

  - MyBatis 本是 apache 的一个开源项目iBatis, 2010 年这个项目由 apache software foundation 迁移到了 google code，并且改名为 MyBatis  。2013 年 11 月迁移到 Github,最新版本是 MyBatis 3.5.7 ，其发布时间是 2021 年 4月 7日。

  - MyBatis完成数据访问层的优化.它专注于sql语句.简化了过去JDBC繁琐的访问机制. 

  - MyBatis框架的结构

    ![framestructure](../../../Obsidian/Current/java/JavaEE/Mybatis/ph/framestructure.png)

### 添加框架

- ### 所有添加框架基础步骤

  1. 添加依赖

  2. 配置文件

- ### 具体步骤

  1. 新建数据库表
  2. 新建Maven项目
  3. 修改目录，添加缺失的目录，修改目录属性
  4. 修改pom.xml文件，添加mybatis的依赖，添加mysql的依赖
  5. 修改pom.xml文件，添加资源文件指定
  6. 在idea中添加数据库的可视化
  7. 添加jdbc.properties属性文件（数据库的配置）
  8. 添加SqlMapConfig.xml文件，mybatis的核心配置文件
  9. 创建实体类Student，用来封装数据
  10. 添加完成学生表的增删改查的功能的StudentMapper.xml文件
  11. 创建测试类，进行功能测试

# Mybatis入门

### Mybatis所需依赖

在pom文件下配置所需依赖

```xml
 <!--  添加Mybatis依赖  -->
 <dependencies>
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.6</version>
    </dependency>
    <!--  添加mysql依赖  -->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.25</version>
    </dependency>
  </dependencies>
```

注意：在POM文件下，还需要配置资源插件，不然会出现Java包下的文件不拷贝到target的情况

```xml
<!-- 编写在build标签下 --> 
<build>
    <resources>
      <resource>
        <directory>src/main/java</directory>
        <includes>
          <include>**/*.xml</include>
          <include>**/*.properties</include>
        </includes>
      </resource>

      <resource>
        <directory>src/main/resources</directory>
        <includes>
          <include>**/*.xml</include>
          <include>**/*.properties</include>
        </includes>
      </resource>
    </resources>
  </build>

```



### 编写JDBC.properties

```properties
jdbc.driverClassName=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssm?useUnicode=true&characterEncoding=utf-8
jdbc.username=root
jdbc.password=159357
```



### 主配置文件SqlMapConfig.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!-- 这里的头文件基本是一致的，以后忘记了可以来这里复制，百度也行 -->
<configuration>
    <!-- 读取属性文件（jdbc。properties）
        属性：
            resources：从resources目录下找指定名称的文件加载
            url：使用绝对路径加载属性文件
    -->
    <properties resource="jdbc.properties"></properties>
    
    <!-- 设置日志底层输出的方法 -->
    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>

    <!-- 注册实体类别名 -->
    <typeAliases>
        <!-- 单个注册 -->
        <!-- <typeAlias type="xyz.current.bean.Student" alias="student">			 </typeAlias>-->
        <!--  批量注册
            别名是类名的驼峰命名法（规范）
        -->
        <package name="xyz.current.bean"/>
    </typeAliases>

    <!--  配置数据库的环境变量（数据库的连接配置）
        属性：
            default：使用下面environment标签的id属性进行指定配置
     -->
    <environments default="development">
        <!-- 公司的数据库配置 -->
        <!-- id：就是提供给environments的default属性使用  -->
        <environment id="development">
            <!-- 配置事务管理器
                type：指定事务管理的方式
                    JDBC：事务的控制交给程序员处理
                    MANAGED：由容器（例如Spring）来管理事务
             -->
            <transactionManager type="JDBC"></transactionManager>
            <!-- 配置数据源
                type：指定不同的配置方式
                    JNDI：Java命名目录接口，在服务器端进行数据库连接池的管理
                    POOLED：使用数据库连接池
                    UNPOOLED：不使用数据库连接池
             -->
            <dataSource type="POOLED">
            <!-- 配置数据库基本参数
                private String driver;
                private String url;
                private String username;
                private String password;
              -->
                <property name="driver" value="${jdbc.driverClassName}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
        <!-- 在家的数据库配置 -->
        <environment id="home">
            <transactionManager type=""></transactionManager>
            <dataSource type=""></dataSource>
        </environment>
        <!-- 上线后的数据库配置 -->
        <environment id="online">
            <transactionManager type=""></transactionManager>
            <dataSource type=""></dataSource>
        </environment>
    </environments>

    <!-- 注册mapper.xml文件
            resource：从resources目录下找指定名称的文件注册
            url：使用绝对路径注册
            class：动态代理方式注册。
    -->
    <mappers>
        <mapper resource="StudentMapper.xml"></mapper>
    </mappers>
</configuration>
```

### 创建Mapper文件

例如studentMapper.xml，用来存储增删改查的sql语句

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--
    mapper:是整个文件的打标签，用来开始和结束下xml文件
    属性：
        namespace：指定命名空间（相当于包名），用来区分不同的mapper
    .xml文件中相同的id属性
 -->
<mapper namespace="current">
    <!-- 
        晚场查询全部学生的功能
        List<Student> getAll();
            resultType：指定查询返回的结果集的类型，如果是集合，则必须是泛型类型
            parameterType：如果有参数，则通过它来指定参数的类型
     -->
    <select id="getAll" resultType="student">
        select id,name,email,age from student
    </select>

    <!-- 根据主键id查询学生信息 -->
    <select id="getById" parameterType="int" resultType="student">
        select id,name,email,age from student where id = #{id}
    </select>

    <!-- 模糊查询 -->
    <select id="getByNameLike" parameterType="string" resultType="student">
        select id,name,email,age from student where name like '%${name}%'
    </select>

    <!-- 插入数据
         private String name;
        private String email;
        private Integer age;
        values中的内容要和类中属性一一对应，最后是传入一个对象，Mybatis会将这个对象中的属性一个个放入到表中
    -->
    <insert id="insert" parameterType="student" >
        insert into student (name,email,age) values (#{name},#{email},#{age})
    </insert>

    <!-- 更新表 -->
    <update id="update" parameterType="student">
        update student set name=#{name},email=#{email},age=#{age} where id=#{id}
    </update>

    <!-- 删除表中元素 -->
    <delete id="delete" parameterType="int">
        delete from student where id=#{id}
    </delete>
</mapper>
```

### 测试类测试

```java
package xyz.current;

import jdk.internal.util.xml.impl.Input;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.After;
import org.junit.Before;
import org.junit.Test;
import xyz.current.bean.Student;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

public class myTest {
    SqlSession sqlSession;
    //这里的before和after是对测试类的优化，分别表示在执行测试前和测试完成后所做的代码
    @Before
    public void before() throws IOException {
        //使用文件流读取核心配置文件sqlMapConfig.xml
        InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
        //创建sqlSessionFactory
        SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
        //取出SqlSession对象
        sqlSession = factory.openSession();
    }

    @After
    public void after(){
        //关闭sqlSession
        sqlSession.close();
    }

    @Test
    public void testA() throws IOException {
        List<Student> list = sqlSession.selectList("current.getAll");
        list.forEach(student -> System.out.println(student));
    }

    @Test
    public void testB() {
        //完成查询操作
        Student student = sqlSession.selectOne("current.getById",2);
        System.out.println(student);
    }

    @Test
    public void testC() {
        //完成查询操作
        List<Student> list = sqlSession.selectList("current.getByNameLike", "张");
        list.forEach(student -> System.out.println(student));
    }

    @Test
    public void testD() {
        int idea = sqlSession.insert("current.insert", new Student("idea", "1010101@168.com", 20));
        //切记切记，在完成任何增删改的时候要提交事务
        System.out.println(idea);
        sqlSession.commit();
    }

    @Test
    public void testE(){
        int i = sqlSession.update("current.update", new Student(1, "丘桑", "666@163.com", 20));
        System.out.println("已修改"+i+"条");
        sqlSession.commit();
    }

    @Test
    public void testF(){
        int i = sqlSession.delete("current.delete",7);
        System.out.println("已删除"+i+"条");
        sqlSession.commit();
    }
}

```



# SqlMapConfig.xml文件的优化

在上面其实可以看到了这里单独拿出来讲讲

### SqlMapConfig.xml的头标签

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
```



### 文件各个组件标签顺序（这个是不能乱的）

properties --> settings --> typeAliases --> typeHandlers --> objectFactory --> objectWrapperFactory --> reflectorFactory --> plugins --> environments --> databaseIdProvider --> mappers

### 添加日志打印输出

文件加入日志配置，可以在控制台输出执行的 sql 语句和参数.

```xml
<!--    设置查看mybatis生成的sql语句的日志配置-->
    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>

```



### 为实体类起别名

- 由于XXXmapper.xml文件中入参和返回值都要使用实体类的对象,而我们在使用的时候必须书写实体类的完全限定类名（就是还要带上包名的那种）,这样比较麻烦,我们可以通过起别名的方式来简化此操作.起别名的方式有两种.

### 单个实体类起别名

```xml
<typeAliases>
	<typeAlias type="com.bjpowernode.pojo.Users" alias="users"></typeAlias>
	<!-- 下面这个写法也可以，我比较喜欢下面的 -->
	<typeAlias type="xyz.current.bean.Student" alias="student"/>
</typeAliases>
```

- type：实体类的完全限定名称
- alias：为实体类起的别名，以后在所有注册的Mapper文件中使用实体类类型的地方，写此别名即可

### 批量注册

- 可以通过`<package>`来批量的为实体类起别名,只要指定实体类所在的包的名称即可.MyBatis框架会自动为每个实体类起别名为类型的全小写或类名的大小写混合.推荐使用类名的全小写.

```xml
<typeAliases>
    <package name="com.bjpowernode.pojo"/>
</typeAliases>
```

### 注意

- 这俩起别名方式都要放在`<typeAliases>`标签中，并且改标签顺序得对
- 这俩方式一般直接选择批量注册



# 注册XXXMapper.xml文件

总的来说，有四种方式

### 使用resource注册

```xml
<mapper resource="com/bjpowernode/mapper/UsersMapper.xml"></mapper>
```

注意: UserMapper.xml是带后缀的,分隔符使用"/"，不是"."

### 使用class注册

- 这种注册方式是用在动态代理中，在后面的笔记中在说明动态代理

  ```xml
  <mapper class="com.bjpowernode.mapper.UsersMapper"></mapper>
  ```

- 注意：class的值是接口的完全限定名称。

### 使用url注册

```xml
<mapper url="file:///E:/UserMapper.xml"></mapper>
```

指定绝对路径注册,注意file后面是双杠.

###  使用`<package>`注册

```xml
<package name="com.bjpowernode.mapper"/>
```

# 动态代理

- 在三层架构中，业务逻辑层要通过接口访问数据访问层的功能，动态代理可以实习那

### 动态代理的实现规范

1. Mapper接口与Mapper.xml文件在同一个目录下
2. Mapper接口的完全限定名与Mapper.xml文件中的namespace的值相同。
3. Mapper接口方法名称与Mapper.xml中的标签的statement 的ID完全相同。
4. Mapper接口方法的输入参数类型与Mapper.xml的每个sql的parameterType的类    型相同
5. Mapper接口方法的输出参数与Mapper.xml的每个sql的resultType的类型相同。
6. Mapper文件中的namespace的值是接口的完全限定名称.
7. 在SqlMapConfig.xml文件中注册时,使用class属性=接口的完全限定名。



### 开发步骤

1. 添加项目依赖
2. 简历属性文件jdbc.properties
3. 新建环境配置文件SqlMapConfig.xml
   - 这里给实体类起别名
   - 注册Mapper文件的时候选择class="接口的完全限定名称"
4. 新建数据库可视化窗口
5. 新建实体类
6. 新建接口
7. 新建与接口类同名的xml文件（类似XXXMapper.xml），完成数据库操作
8. 测试类测试



### #{}和${}

- **#{}是对非字符串拼接的参数的占位符**，如果入参是简单的数据类型（即基本数据类型，包装类，String），#{}里可以任意写。但是如果入参的是对象类型，则#{}里必须是独享的成员变量的名称，并且严格区分大小写。它的底层使用的是PreparedStatement对象，是安全的数据库访问，#{}可以有效防止Sql注入

- **$ {}主要是针对字符串拼接替换**，如果入参是基本数据类型，$ {}里可以随便写（这个是分版本的，如果是3.5.1及以下的版本则必须填写value）。如果入参是对象类型，则$ {}里必须是对象的成员变量名称。${}还可以替换列名和表名存在sql注入的风险，尽量少用

  ```xml
  <!--
      //模糊用户名和地址查询
      //如果参数超过一个,则parameterType不写
      List<Users> getByNameOrAddress(
              @Param("columnName")  ===>为了在sql语句中使用的名称
              String columnName,
              @Param("columnValue")   ===>为了在sql语句中使用的名称
              String columnValue);
      -->
      <select id="getByNameOrAddress" resultType="users">
          select id,username,birthday,sex,address
          from users
          where ${columnName} like concat('%',#{columnValue},'%')  ==>此处使用的是@Param注解里的名称
      </select>
  ```



### 返回主键值

- 在插入语句结束之后，返回自增的主键值到入参的users对象的id属性中

  ```xml
   <insert id="insert" parameterType="users" >
  	    <selectKey  keyProperty="id" resultType="int" order="AFTER">
  	        select last_insert_id()
  	    </selectKey>
          insert into users (username, birthday, sex, address) values(#{userName},#{birthday},#{sex},#{address})
    </insert>
  ```

- 关于`<selectKey>`标签的参数详解

  - keyProperty: users对象的哪个属性来接返回的主键值
  - resultType:返回的主键的类型
  - order:在插入语句执行前,还是执行后返回主键的值
    - AFTER就是在sql语句之后执行
    - BEFORE就是在sql语句之前执行

- UUID

  - 这个是一个全球唯一随机字符串，由36个字母数字中划线组成

    ```java
    UUID uuid = UUID.randomUUID();
    System.out.println(uuid.toString().replace("-","").substring(20));
    ```




# 动态sql

可以定义代码片段，可以进行逻辑判断，可以进行循环处理（批量处理），使条件判断更为简单。

- `<sql>`：用来定义代码片段，可以将所有的列名，或复杂的条件定义为代码片段，供使用是调用

- `<include>`：用来引用`<sql>`定义的代码片段。

  ```xml
   <!-- 定义代码片段 -->
      <sql id="allColumns">
          id,username,birthday,sex,address
      </sql>
      
   <!-- 使用include标签来获取之前定义的片段 -->  
      <select id="getAll" resultType="users">
          select <include refid="allColumns"></include> from users
      </select>
  ```

- `<if>`：进行条件判断，下面的判断都是靠它

  - 如果遇到idea提示大于小于符号错误，可以不用管或者使用其他的符号代替大于小于号
  - `&gt;` ：大于【注意冒号不能少】
  - `&lt;`：小于【注意冒号不能少】

- `<where>`：进行多条件拼接，在查询删除，更新中使用

  ```xml
  <!-- 多条件查询 -->
      <select id="getByCondition" parameterType="users" resultType="users">
          select <include refid="allColumns"></include> from users
          <where>
              <if test="userName != null and userName != ''">
                   and username like concat('%',#{userName},'%')
              </if>
              <if test="birthday != null">
                  and birthday = #{birthday}
              </if>
              <if test="sex != null and sex != ''">
                  and sex = #{sex}
              </if>
              <if test="address != null and address != ''">
                  and address like concat('%',#{address},'%')
              </if>
          </where>
      </select>
  ```

- `<set>`：有选择的更新处理，至少更新一列。如果啥都不更新会直接报错，但是一般情况下前端页面就可以直接判断

  ```xml
  <!-- 有选择的更新 -->
      <update id="updateBySet" parameterType="users">
          update users
          <set>
              <if test="userName != null and userName != ''">
                  username=#{userName},
              </if>
              <if test="birthday != null">
                   birthday = #{birthday},
              </if>
              <if test="sex != null and sex != ''">
                  sex = #{sex},
              </if>
              <if test="address != null and address != ''">
                  address = #{address},
              </if>
          </set>
          where id = #{id}
      </update>
  ```

- `<foreach>`：用来进行循环遍历，完成循环条件查询，批量删除，批量增加，批量更新等等

  - 参数详解

    - collection：用来指定入参的类型，如果是List集合就写list，如果是Map集合就写map，如果是数组就写array
    - item：每次循环遍历出来的值或者对象
    - separator：多个值或对象或语句之间的分隔符
    - open：整个foreach语句外前面的字符串，这里可以写"("
    - close：整个foreach语句外后面的字符串，这里可以写")"
    - index：在list和数组中，index是元素的序号下标，在map中，index是元素的key，该参数可选

  - 例子：

    - 查询多个id

      ```xml
      <!-- 查询多个指定id的用户信息 -->
          <select id="getByIds" resultType="users">
              select <include refid="allColumns"></include> from users
              where id in
                  <foreach collection="array" item="id" separator="," open="(" close=")"  >
                      #{id}
                  </foreach>
          </select>
      ```

    - 批量插入

      ```xml
      <!-- 批量插入 -->
          <insert id="insertBatch" >
              insert into users (username,birthday,sex,address) values 
                  <foreach collection="list" item="users" separator=",">
                      (#{users.userName},#{users.birthday},#{users.sex},#{users.address})
                  </foreach>
          </insert>
      ```

    - 批量删除

      ```xml
      <!-- 批量删除 -->
          <delete id="deleteBatch" >
              delete from users where id in 
                  <foreach collection="array" item="id" separator="," open="(" close=")">
                      #{id}
                  </foreach>
          </delete>
      ```

    - 批量更新

      批量更新和上面就不一样的，本质上批量更新是执行多条更新语句，所以要在jdbc文件中，添加一些配置来确保允许同时执行多条sql语句

      **在jdbc.properties属性文件中的url中添加&allowMultiQueries=true，允许多行操作**

      ```xml
      <!-- 批量更新 -->
          <update id="updateBatch" >
              <foreach collection="list" item="u" separator=";">
                      update users
                      <set>
                          <if test="u.userName != null and u.userName != ''">
                              username=#{u.userName},
                          </if>
                          <if test="u.birthday != null">
                              birthday = #{u.birthday},
                          </if>
                          <if test="u.sex != null and u.sex != ''">
                              sex = #{u.sex},
                          </if>
                          <if test="u.address != null and u.address != ''">
                              address = #{u.address},
                          </if>
                      </set>
                      where id = #{u.id}
              </foreach>
          </update>
      ```

      

# 参数的使用

- 如果入参是多个，可以通过指定参数位置进行传参， 是实体包含不住的条件。实体类只能封装住成员变量的条件，如果某个成员变量要有区间范围内的判断,或者有两个值进行处理,则实体类包不住。

### 直接指定参数位置

- 可以不使用对象的属性名进行参数值绑定，使用下标值。 mybatis-3.3 版本和之前的版本使用#{0},#{1}方式， 从 mybatis3.4 开始使用#{arg0}方式。

  UsersMapper.java接口中：

  ```java
  List<Users> getByBirthday(Date begin, Date end);
  ```

  UserMapper.xml文件中

  ```xml
  <select id="getByBirthday" resultType="users">
     select * from users where birthday between #{arg0} and #{arg1}
  </select>
  ```

### @param指定参数位置

- 可以不使用对象的属性名进行参数值绑定，使用下标值。 mybatis-3.3 版本和之前的版本使用#{0},#{1}方式， 从 mybatis3.4 开始使用#{arg0}方式。

  UsersMapper.java接口中：

  ```java
  List<Users> getByNameOrAddress(
              @Param("columnName")
              String columnName,
              @Param("columnValue")
              String columnValue
  
      );
  ```

  UserMapper.xml文件中

  ```xml
  <!-- 模糊用户名和地址查询 -->
      <select id="getByNameOrAddress" resultType="users">
          select <include refid="allColumns"></include> from users where ${columnName} like concat('%',#{columnValue},'%')
      </select>
  ```

### 入参是map

- 入参是map,是因为当传递的数据有多个,不适合使用指定下标或指定名称的方式来进行传参,又加上参数不一定与对象的成员变量一致,考虑使用map集合来进行传递.map使用的是键值对的方式.当在sql语句中使用的时候#{键名},${键名},用的是键的名称.

  UsersMapper.java接口中：

  ```java
  //入参是map
  List<Users> getByMap(Map<String,Date> map);
  ```

  UserMapper.xml文件中：

  ```xml
  <select id="getByMap" parameterType="map" resultType="users">
      select <include refid="columns"></include>
      from users
      where birthday between #{zarbegin} and #{zarend}
  </select>
  ```

  测试类中：

  ```java
  @Test
  public void testGetByMap()throws Exception{
      Date begin = new SimpleDateFormat("yyyy-MM-dd").parse("1996-01-01");
      Date end = new SimpleDateFormat("yyyy-MM-dd").parse("1998-12-31");
      Map<String,Date> map = new HashMap<>();
      //放入map集合中的数据是键值对
      map.put("zarbegin",begin);=
      map.put("zarend",end);
      List<Users> list = mapper.getByMap(map);
      list.forEach(u-> System.out.println(u));
  }
  ```

### 返回值是map

- 返回值是map的适用场景,如果的数据不能使用对象来进行封装,可能查询的数据来自多张表中的某些列,这种情况下,使用map,但是map的返回方式破坏了对象的封装,返回来的数据是一个一个单独的数据, 之间不相关.**map使用表中的列名或别名做为键名进行返回数据.**

- ### map封装返回一行

  UsersMapper.java接口中：

  ```java
  //返回值是一个值,是map类型,根据主键查用户对象
  Map<String,Object> getReturnMapOne(int id);
  ```

  UserMapper.xml文件中：

  ```xml
  <select id="getReturnMapOne" resultType="map" parameterType="int">
      select id myid,username myusername,sex mysex,address myaddress,birthday mybirthday
      from users
      where id=#{id}
  </select>
  ```

  测试类中：

  ```java
  @Test
  public void testGetReturnMapOne(){
      Map<String,Object> map = mapper.getReturnMapOne(7);
      System.out.println(map);
      System.out.println(map.get("username"));
  }
  
  ```

- ### map封装返回多行

  UsersMapper.java接口中：

  ```java
  /使用map封装返回多个map的集合--->List<Map<String,Object>>
  List<Map<String,Object>> getReturnMap();
  ```

  UsersMapper.xml文件中：

  ```xml
  <select id="getReturnMap" resultType="map" >
      select <include refid="columns"></include>
      from users
  </select>
  ```

  测试类中：

  ```java
  @Test
  public void testGetReturnMap(){
      List<Map<String,Object>> list = mapper.getReturnMap();
      list.forEach(map-> System.out.println(map));
  }
  ```

### 列名与类中成员变量名称不一致

- 解决方案一：
  - 使用列的别名，别名与类中的成员变量名一样，即可完成注入。
- 解决方案二：
  - 使用`<resultMap>`标签进行映射。

```xml
<resultMap type="TeacherEntity" id="teacherResultMap">  
    <id property="teacherID" column="TEACHER_ID" />  
    <result property="teacherName" column="TEACHER_NAME" />  
    <result property="teacherSex" column="TEACHER_SEX" />  
    <result property="teacherBirthday" column="TEACHER_BIRTHDAY"/>  
    <result property="workDate" column="WORK_DATE"/>  
    <result property="professional" column="PROFESSIONAL"/>  
</resultMap>  

<select id="getTeacher" parameterType="String"  resultMap="teacherResultMap">  
    SELECT *  
      FROM TEACHER_TBL TT  
     WHERE TT.TEACHER_ID = #{teacherID}  
</select> 
```

- 属性说明：
  - id属性 ，resultMap标签的标识。
  - type属性 ，返回值的全限定类名，或类型别名。
  - autoMapping属性 ，值范围true（默认值）|false, 设置是否启动自动映射功能，自动映射功能就是自动查找与字段名小写同名的属性名，并调用setter方法。而设置为false后，则需要在`resultMap`内明确注明映射关系才会调用对应的setter方法。

# 表之间的关联关系

### 四种关系

tips：关联关系是有方向的

- 一对多关联：一个菜单可以查看多个财评，多个菜品只有这一个菜单容纳，站在菜单方，就是一对多关联
- 多对一关联：一个菜单可以存多个菜品的信息，多个菜品信息只有这一个菜单来容纳，站在菜品方，这就是多对一关系
- 一对一关联：一个菜单中只有这一个菜品，这个菜品只存在在这一个菜单中，这就是一对一关联
- 多对多关联：世界上有许多菜单，每一个菜单中可以是任意一个菜品，任意一菜品信息可以存在任意一个菜单上。



### 一对多

- 在一堆过关联关系中，一方中有多方的集合，所以要使用`<collection>`标签来映射多方的属性。

  ```xml
   <mapper namespace="com.bjpowernode.mapper.CustomerMapper">
     <!--
       //根据客户的id查询客户所有信息并同时查询该客户名下的所有订单
      Customer getById(Integer id)
      实体类:
      //customer表中的三个列
      private Integer id;
      private String name;
      private Integer age;
      //该客户名下的所有订单的集合
      private List<Orders> ordersList;
     -->
      <resultMap id="customermap" type="customer">
          <!--主键绑定-->
          <id property="id" column="cid"></id>
          <!--非主键绑定-->
          <result property="name" column="name"></result>
          <result property="age" column="age"></result>
  
          <!--多出来的一咕噜绑定ordersList
          Orders实体类:
          private Integer id;
          private String orderNumber;
          private Double orderPrice;
          -->
          <collection property="ordersList" ofType="orders">
              <!--主键绑定-->
              <id property="id" column="oid"></id>
              <!--非主键绑定-->
              <result property="orderNumber" column="orderNumber"></result>
              <result property="orderPrice" column="orderPrice"></result>
          </collection>
      </resultMap>
      <select id="getById" parameterType="int" resultMap="customermap">
          select c.id cid,name,age,o.id oid,orderNumber,orderPrice,customer_id
          from customer c left  join orders o on c.id = o.customer_id
          where c.id=#{id}
      </select>
  </mapper>
  ```

  

### 多对一

- 在多对一关联联系中，多方中持有乙方的对象，需要使用标签`<association>`标签来映射一方的属性。

  ```xml
   <mapper namespace="com.bjpowernode.mapper.OrdersMapper">
      <!--
        //根据主键查询订单,并同时查询下此订单的客户信息
      Orders getById(Integer id);
      -->
  
      <!--
        手工绑定数据
        实体类
          private Integer id;
          private String orderNumber;
          private Double orderPrice;
  
          //关联下此订单的客户信息,多方持有一方的对象
          private Customer customer;
      -->
      <resultMap id="ordersmap" type="orders">
          <!--主键绑定-->
          <id property="id" column="oid"></id>
          <!--非主键绑定-->
          <result property="orderNumber" column="orderNumber"></result>
          <result property="orderPrice" column="orderPrice"></result>
          <!--多出来的一咕噜绑定
              private Integer id;
              private String name;
              private Integer age;
  
              //该客户名下的所有订单的集合,一方持有多方的集合
              private List<Orders> ordersList; //不用管
          -->
          <association property="customer" javaType="customer">
              <id property="id" column="cid"></id>
              <result property="name" column="name"></result>
              <result property="age" column="age"></result>
  
          </association>
      </resultMap>
      <select id="getById" parameterType="int" resultMap="ordersmap">
          select o.id oid,orderNumber,orderPrice,customer_id,c.id cid,name,age
          from orders o inner join customer c on o.customer_id = c.id
          where o.id=#{id}
      </select>
    </mapper>
  ```

### 其余

- 其余的在上面的基础上进行修改就行

### 总结

- 无论是什么关联关系，如果某方持有另一方的集合，则使用`<collection>`标签来完成映射，如果是某方持有另一方的单个对象，则使用`<association>`标签来完成映射

# 事务

### 事务的特性

- 事务即是多个操作同时完成，或者同时失败称为输出处理
- 驶入具有一致性，持久性，原子性，隔离性。

### Mybatis事务

- Mybatis 框架是对 JDBC 的封装，所以 Mybatis 框架的事务控制方式有两种，一种是容器进行事务管理的，一种是程序员手工决定事务的提交与回滚。

- MyBatis 支持两种事务管理器类型：**JDBC** **与 MANAGED**。

  在SqlMapConfig.xml文件中可以设置这两种事务管理器

  ```xml
   <transactionManager type="JDBC"></transactionManager>  ===>程序员自己控制处理的提交和回滚
  ```

- JDBC

  - 使用 JDBC 的事务管理机制。即，通过 Connection 的 commit()方法提交，通过 rollback()方法回滚。但默认情况下，MyBatis 将自动提交功能关闭了，改为了手动提交。

  - 在获得SqlSession的时候,如果openSession()是无参或者是false,则必须手工提交事务，如果openSession(true),则为自动提交事务，在执行完增删改后无须commit(),事务自动提交

    ```java
    session = factory.openSession(true);
    ```

    

# Mybatis缓存

### Mybatis缓存概述

- 将用户经常查询的数据放在缓存（内存）中，用户去查询数据就不用从磁盘上(关系型数据库数据文件)查询，从缓存中查询，从而**提高查询效率**，解决了高并发系统的性能问题。mybatis提供查询缓存，用于减轻数据库压力，提高数据库性能
- 缓存机制的执行
  - 在进行数据库访问时，首先去访问缓存，如果缓存中有要访问的数据，则直接返回客户端，如果没有则去访问数据库，在库中得到数据后，先在缓存放一份，再返回客户端。
- **mybaits提供一级缓存和二级缓存。默认开启一级缓存。**

### 一级缓存

- 第一次发起查询用户id为1的用户信息，先去找缓存中是否有id为1的用户信息，如果没有，从数据库查询用户信息。得到用户信息，将用户信息存储到一级缓存中。如果sqlSession去执行commit操作（执行插入、更新、删除），则清空SqlSession中的一级缓存，这样做的目的为了让缓存中存储的是最新的信息，避免脏读。
- 第二次发起查询用户id为1的用户信息，先去找缓存中是否有id为1的用户信息，缓存中有，直接从缓存中获取用户信息。如果没有重复第一次查询操作。
- 简单来说：一级缓存的作用域是sqlSession。同一个sqlSession共享一级缓存的数据.

### 二级缓存

- 二级缓存使用的是mapper的作用域，不同的sqlSession只要访问的同一个mapper.xml文件，则共享二级缓存作用域。

- mybaits的二级缓存是mapper范围级别，除了在SqlMapConfig.xml设置二级缓存的总开关，还要在具体的mapper.xml中开启二级缓存，并且要让实体类实现serializable接口。

- 具体步骤

  1. 在核心配置文件SqlMapConfig.xml中加入设置

     ```xml
     <settings>
     	<setting name="logImpl" value="STDOUT_LOGGING"/>
     	<!-- 开启二级缓存 -->
     	<setting name="cacheEnabled" value="true"/>
     </settings>
     ```

  2. 在XXXMapper.xml文件中开启二级缓存，使用`<cache></cache>`，在mapper标签在加入该标签即可

  3. 实体类必须实现Java.io.serializable接口，保证实体可序列化


# ORM

- **ORM(Object Relational Mapping):**对象关系映射

- 编写程序的时候，以面向对象的方式处理数据，保存数据的时候，却以关系型数据库的方式存储。
- 持久化的操作：将对象保存到关系型数据库中 ,将关系型数据库中的数据读取出来以对象的形式封装
- Mybatis是非常优秀的ORM框架