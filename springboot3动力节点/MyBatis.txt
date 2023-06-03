MyBatis注解开发
1.加入mybatis的starter ， mysql驱动（8.0.32）
2.创建实体类 XXXPO , XXXEntity , XXXDomain
3.创建Mapper接口， 在接口中定义方法， 在方法的上面使用合适的注解
 @Select：查询 ，使用@Results和@Result做结果映射。
 @Insert：新增
 @Update：更新
 @Delete：删除

4.在启动上面，加入@MapperScan
  @MapperScan(basePackages = "com.bjpowernode.mybatis.mapper")

5.application.properties
  1）定义数据库连接
  2）mybatis设置
        日志
        驼峰命名支持

=========================================================================
ResultMap:结果映射，将查询结果中的列和实体Bean的属性对应关系。
通过在xml文件使用<resultMap>标签定义映射关系，
    在其他的<select resultMap="xxx">

注解中的映射
1）@Results和@Result
2）@ResultMap

@ResultMap使用方式：
第一种：先通过@Results定义列的映射关系， @ResultMap(value="@Result的id")
第二种：在xml中定义<resultMap id="xxx"> ,在代码中使用@ResultMap(value="xml的id")


==============================================================================
提供Sql语句， 使用提供者
@SelectProvider
@InsertProvider
@UpdateProvider
@DeleteProvider

提供者类： 定义一个普通类， 定义静态方法， 方法的返回值是要执行的sql语句

==============================================================================
关系： @One 一对一， @Many 一对多

==============================================================================
MybatisAutoConfiguration ： 自动配置类

1.SqlSessionFactory , 能够获取SqlSession
2.SqlSessionTemplate ， 用于执行sql语句，mybatis和spring整合时，使用的模板对象
3.MapperFactoryBean ：创建dao接口的代理对象

===============================================================================
连接池，连接的大小控制：
 connections = ((cpu 核心数 * 2) + 磁盘数量) 近似值。 默认 10

=================================================================================
事务： spring的事务管理
采用声明式事务，注解方式@Transactionl , 在public方法的上面加入注解
控制事务： 1.传播行为，2.隔离级别，3.只读，4.超时时间

回滚规则：
 1.业务方法发生RuntimeException 和 Error 回滚事务
 2.使用@Transactional的rollbackFor控制事务的回滚类型


