### 第2章 Spring AOP详解

#### 一、Spring IOC注解注入属性

##### 1．注解的作用

创建Bean，注入到容器，为Bean注入属性

##### 2．注解的分类

* 创建类的注解
  
@Component：任何类如果需要被spring容器创建和管理使用
@Service:业务层的类使用
@Controller：控制器类使用
@Repository:Dao数据访问层的类使用（MyBatis使用的是代理，不需要写Dao接口的实现类，所以该注解在SSM整合中用不上）

* 注入属性的注解

@Autowired：安装类型进行注入，如果Spring容器中创建并管理了某个Bean，要注入的时候，到容器中查找是否有该类型的Bean,如果有就注入，没有就报异常。
@Resource:安装名字注入，到spring的容器中查找是否有id为某个名字的Bean，有就注入。

* @Qualifier:可以在属性注入的时候显示的指定名字

##### 3．使用步骤

１）在pom.xml添加依赖
２）使用注解创建Bean
需要说明的是，如果采用注解没有用value属性作为指定bean的名字，那么会默认会用类名，同时把首字母小写获得的字符串作为value的属性值。
３）使用注解为Bean注入属性
４）在Spring的配置文件配置
５）测试

##### 4．采用XML配置通过构造方法注入属性

#### 二、Spring AOP相关概念

#### 三、Spring AOP解决方案

#### 四、Spring AOP实现方式

#### 五、IOC（DI）课程小结