文件格式 : properties(优先), yml(yaml)
        内容： key=value
        yml   key: value
文件名称：默认application
环境对象： Environment : 表示抽象的所有key和value。 方法getProperty(key)

多文件： 自定义独立的配置文件， 使用spring.config.import=文件路径。
        导入多个文件。

多环境 : 开发环境，测试环境，上线， 特性， bug等等
        名称：application-profile.properties(yml) , 可以有多个环境文件
        创建环境文件： 使用
         spring:
           config:
             activate:
               on-profile: 环境名称

         在application文件中激活某个环境
         spring:
           profiles:
             active: 环境名称

 读取数据：@Value("${key}") ,使用 Environment.getPropery("key")   获取的是单个值（String，int，long ,boolean)


 绑定Bean： 用于多个属性。
 注解： @ConfigurationProperties
       位置：1）在类的上面，需要有源代码
            2）方法的上面，使用第三方对象。配合@Bean注解
 数据来源 application文件（properties， 或 yml）
         指定数据来源@PropertySource(value = "xxx.properties")

 注意：1）类中有无参数构造方法
      2）属性有setXXX方法
      3）static属性无效
      4）使用bean的构造方法也能创建对象，无需set方法

 @ConfigurationProperties使用需要配合其他注解：
 1）@Configuration
 2）@EnableConfigurationProperties
 3）@ConfigurationPropertiesScan


配置文件application 名称 和 位置都是可以改变的(但是不推荐)。
application配置文件的位置：
1）项目的根目录下
2）项目根目录的/config目录
3）resources/config
4）resources目录

