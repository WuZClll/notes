[MyBatis-老杜-动力节点_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1JP4y1Z73S?p=2&vd_source=796ed40051b301bfa3a84ba357f4828c)

## 一.  MyBatis概述

### 1.  三层架构

- **表现层（UI）**：直接跟前端打交互（⼀是接收前端ajax请求，⼆是返回json数据给前端） 
- **业务逻辑层（BLL）**：⼀是处理表现层转发过来的前端请求（也就是具体业务），⼆是将从持久层获取的数据返回到表现层。
- **数据访问层（DAL）**：直接操作数据库完成CRUD，并将获得的数据返回到上⼀层（也就是业务逻辑层）。 

### 2 . 了解MyBatis

- MyBatis下载[github.com](https://github.com/mybatis/mybatis-3)

- MyBatis本质上就是**对JDBC的封装**，通过MyBatis完成CRUD。 

- MyBatis在三层架构中负责持久层的，属于**持久层框架**。

- MyBatis可以将java对象与数据库表中的一条记录相互转换

- MyBatis属于**半⾃动化ORM框架**。 

- Hibernate属于全⾃动化的ORM框架， 不需要手写SQL语句
  >eg:
  >`session.save(user)`=转化为=》`insert into t_user values`

### 3.  ORM思想

- ORM思想：对象关系映射 

- O（Object）：Java虚拟机中的**Java对象**

- R（Relational）：关系型**数据库** 

- M（Mapping）：将Java虚拟机中的Java对象**映射**到数据库表中⼀⾏记录，或是将数据库表中⼀⾏记录映射成Java虚拟机中的⼀个Java对象。 

- ORM图示
  ```mermaid
  flowchart LR
      subgraph ORM
      a[类]
      b[对象]
      c[属性]
      end
      subgraph DB
      d[数据表]
      e[数据行]
      f[字段]
      end
      a --> d
      b --> e
      c -->f
  ```

### 4.  MyBatis框架**特点**

- ⽀持定制化 SQL、存储过程、基本映射以及⾼级映射 
- 避免了⼏乎所有的 JDBC 代码中⼿动设置参数以及获取结果集 
- ⽀持XML开发，也⽀持注解式开发。【为了保证sql语句的灵活，所以mybatis⼤部分是采⽤XML⽅式开发。】
- 将接⼝和 Java 的 POJOs(Plain Ordinary Java Object，简单普通的Java对象)映射成数据库中的记录
- 体积⼩好学：两个jar包，两个XML配置⽂件。
- 完全做到sql解耦合。 
- 提供了基本映射标签。 
- 提供了⾼级映射标签。
- 提供了XML标签，⽀持动态SQL的编写。 
- ...... 

## 二. MyBatis入门

### 1. resources目录

放在这个目录当中的，一般都是资源文件，配置文件。
直接放到resources目录下的资源，等同于放到了类的根路径下。

### 2.  开发步骤

#### 2.1 打包方式 `jar`

```xml
<packaging>jar</packaging>
```

#### 2.2 引入依赖

- mybatis依赖
  ```xml
  <dependency>
   <groupId>org.mybatis</groupId>
   <artifactId>mybatis</artifactId>
   <version>3.5.10</version>
  </dependency>
  ```

- mysql驱动依赖
  ```xml
  <dependency>
   <groupId>mysql</groupId>
   <artifactId>mysql-connector-java</artifactId>
   <version>8.0.30</version>
  </dependency>
  ```

#### 2.3 编写mybatis核心配置文件：`mybatis-config.xml`

- 这个文件名不是必须叫做 `mybatis-config.xml`，**可以用其他的名字**。只是大家都采用这个名字。
- 这个文件**存放的位置也不是固定的**，可以随意，但一般情况下，会放到类的根路径下。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <!--<transactionManager type="MANAGED"/>-->
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/powernode"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="CarMapper.xml"/>
    </mappers>
</configuration>
```

#### 2.4 编写XxxxMapper.xml文件

- 在这个配置文件当中**编写SQL语句**。
- 这个文件名也不是固定的，放的位置也**不是固定的**
- 我们这里给它起个名字，叫做：CarMapper.xml
- 把它暂时放到类的根路径下

CarMapper.xml:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="fdsafdsa">
    <!--insert语句，id是这条SQL语句的唯一标识。这个id就代表了这条SQL语句。-->
    <insert id="insertCar">
        insert into t_car(id,car_num,brand,guide_price,produce_time,car_type)
        values(null,'1003','丰田霸道',30.0,'2000-10-11','燃油车');
    </insert>
</mapper>
```

#### 2.5 在mybatis-config.xml文件中指定XxxxMapper.xml文件的路径

- resource属性会自动从类的根路径下开始查找资源。

```xml
<mappers>
    <mapper resource="CarMapper.xml"/>
    <mapper resource="XxxMapper.xml"/>
    <!--...-->
</mappers>
```

### 3. 编写MyBatis程序

- 在MyBatis当中，负责执行SQL语句的那个对象叫做 `SqlSession`，它是一个Java程序和数据库之间的一次会话
- mybatsi的**核心对象**包括
  1. SqlSessionFactoryBuilder   `.build()`↓
  2. SqlSessionFactory  `.openSession()`↓
  3. SqlSession

#### 3.1 步骤一： 获取`SqlSession`对象

- SqlSessionFactoryBuilder --build()--> SqlSessionFactory --openSession()--> SqlSession

1. 先获取`SqlSessionFactoryBuilder`对象
   ```java
   SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
   ```

2. 然后通过`SqlSessionFactoryBuilder`对象的`build`方法，来获取一个`SqlSessionFactory`对象。
   ```java
   //build的参数是一个输入流，它指向mybatis-config.xml
   InputStream is = Resources.getResourceAsStream("mybatis-config.xml");//Resources.getResourceAsStream默认从类的根路径下开始查找资源
   //InputStream is = Resources.getResourceAsStream("com/mybatis.xml");
   //InputStream is = new FileInputStream("d:\\mybatis-config.xml");
   
   SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(is);
   ```

3. 最后通过`SqlSessionFactory`工厂来生产`SqlSession`对象。
   ```java
   SqlSession sqlSession = sqlSessionFactory.openSession();
   ```

#### 	3.2 步骤二： 执行sql语句

```java
int count = sqlSession.insert("insertCar"); // 返回值是影响数据库表当中的记录条数。
// 手动提交
sqlSession.commit(); // 如果使用的事务管理器是JDBC的话，底层实际上还是会执行conn.commit();
```

### 4. 从 XML 中构建 SqlSessionFactory

- 在MyBatis中一定是有一个很重要的对象，这个对象是：SqlSessionFactory对象。
- SqlSessionFactory对象的创建需要XML。

### 5. 两个主要的配置文件

- `mybatis-config.xml`，这是核心配置文件，主要配置连接数据库的信息等。（一个）
- `XxxxMapper.xml`，这个文件是专门用来编写SQL语句的配置文件。（一个表一个）

### 6. 一些小细节

- mybatis中sql语句的结尾";"可以省略

- ```java
  InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
  ```

  - 以后凡是遇到resource这个单词，大部分情况下，这种加载资源的方式就是从类的根路径下开始加载。（开始查找）
  - 优点：采用这种方式，从类路径当中加载资源，项目的移植性很强。项目从windows移植到linux，代码不需要修改，因为这个资源文件一直都在类路径当中。

- ```java
  InputStream is = new FileInputStream("d:\\mybatis-config.xml");
  ```

  - 采用这种方式也可以。
  - 缺点：可移植性太差，程序不够健壮。可能会移植到其他的操作系统当中。导致以上路径无效，还需要修改java代码中的路径。这样违背了OCP原则。

- mybatis核心配置文件的名字，不一定是：mybatis-config.xml。可以是其它名字。

- mybatis核心配置文件存放的路径，也不一定是在类的根路径下。可以放到其它位置。但为了项目的移植性，健壮性，最好将这个配置文件放到类路径下面。

- ```java
  InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("mybatis-config.xml");
  ```

  - `ClassLoader.getSystemClassLoader() `获取系统的类加载器。

  - 系统类加载器有一个方法叫做：`getResourceAsStream`,它就是从类路径当中加载资源的

  - 源代码分析发现
    ```jav
    Resources.getResourceAsStream("mybatis-config.xml")
    ```

    的源码其实就是

    ```java           ;
    ClassLoader.getSystemClassLoader().getResourceAsStream("mybatis-config.xml");

- CarMapper.xml文件的名字和CarMapper.xml文件的路径        都不是固定的。

  ​        <mapper resource="CarMapper.xml"/> resource属性：这种方式是从类路径当中加载资源。
  ​        <mapper url="file:///d:/CarMapper.xml"/> url属性：这种方式是从绝对路径当中加载资源。