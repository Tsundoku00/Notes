### 遇到的bug及解决方法

``` java
org.apache.ibatis.exceptions.PersistenceException: 
### Error building SqlSession.
### The error may exist in com/zfz/dao/BookMapper.xml
### The error occurred while processing mapper_resultMap[bookMap]_association[bookType]
### Cause: org.apache.ibatis.builder.BuilderException: Error parsing SQL Mapper Configuration. Cause: org.apache.ibatis.builder.BuilderException: Error parsing Mapper XML. Cause: org.apache.ibatis.builder.BuilderException: Error resolving class. Cause: org.apache.ibatis.type.TypeException: Could not resolve type alias 'bookMap'.  Cause: java.lang.ClassNotFoundException: Cannot find class: bookMap

	at org.apache.ibatis.exceptions.ExceptionFactory.wrapException(ExceptionFactory.java:26)
	at org.apache.ibatis.session.SqlSessionFactoryBuilder.build(SqlSessionFactoryBuilder.java:54)
	at org.apache.ibatis.session.SqlSessionFactoryBuilder.build(SqlSessionFactoryBuilder.java:38)
	at com.zfz.AppTest.listAll(AppTest.java:25)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.junit.runners.model.FrameworkMethod$1.runReflectiveCall(FrameworkMethod.java:47)
	at org.junit.internal.runners.model.ReflectiveCallable.run(ReflectiveCallable.java:12)
	at org.junit.runners.model.FrameworkMethod.invokeExplosively(FrameworkMethod.java:44)
	at org.junit.internal.runners.statements.InvokeMethod.evaluate(InvokeMethod.java:17)
	at org.junit.runners.ParentRunner.runLeaf(ParentRunner.java:271)
	at org.junit.runners.BlockJUnit4ClassRunner.runChild(BlockJUnit4ClassRunner.java:70)
	at org.junit.runners.BlockJUnit4ClassRunner.runChild(BlockJUnit4ClassRunner.java:50)
	at org.junit.runners.ParentRunner$3.run(ParentRunner.java:238)
	at org.junit.runners.ParentRunner$1.schedule(ParentRunner.java:63)
	at org.junit.runners.ParentRunner.runChildren(ParentRunner.java:236)
	at org.junit.runners.ParentRunner.access$000(ParentRunner.java:53)
	at org.junit.runners.ParentRunner$2.evaluate(ParentRunner.java:229)
	at org.junit.runners.ParentRunner.run(ParentRunner.java:309)
	at org.junit.runner.JUnitCore.run(JUnitCore.java:160)
	at com.intellij.junit4.JUnit4IdeaTestRunner.startRunnerWithArgs(JUnit4IdeaTestRunner.java:69)
	at com.intellij.rt.junit.IdeaTestRunner$Repeater.startRunnerWithArgs(IdeaTestRunner.java:33)
	at com.intellij.rt.junit.JUnitStarter.prepareStreamsAndStart(JUnitStarter.java:220)
	at com.intellij.rt.junit.JUnitStarter.main(JUnitStarter.java:53)
Caused by: org.apache.ibatis.builder.BuilderException: Error parsing SQL Mapper Configuration. Cause: org.apache.ibatis.builder.BuilderException: Error parsing Mapper XML. Cause: org.apache.ibatis.builder.BuilderException: Error resolving class. Cause: org.apache.ibatis.type.TypeException: Could not resolve type alias 'bookMap'.  Cause: java.lang.ClassNotFoundException: Cannot find class: bookMap
	at org.apache.ibatis.builder.xml.XMLConfigBuilder.parseConfiguration(XMLConfigBuilder.java:109)
	at org.apache.ibatis.builder.xml.XMLConfigBuilder.parse(XMLConfigBuilder.java:92)
	at org.apache.ibatis.session.SqlSessionFactoryBuilder.build(SqlSessionFactoryBuilder.java:52)
	... 24 more
Caused by: org.apache.ibatis.builder.BuilderException: Error parsing Mapper XML. Cause: org.apache.ibatis.builder.BuilderException: Error resolving class. Cause: org.apache.ibatis.type.TypeException: Could not resolve type alias 'bookMap'.  Cause: java.lang.ClassNotFoundException: Cannot find class: bookMap
	at org.apache.ibatis.builder.xml.XMLMapperBuilder.configurationElement(XMLMapperBuilder.java:120)
	at org.apache.ibatis.builder.xml.XMLMapperBuilder.parse(XMLMapperBuilder.java:92)
	at org.apache.ibatis.builder.xml.XMLConfigBuilder.mapperElement(XMLConfigBuilder.java:322)
	at org.apache.ibatis.builder.xml.XMLConfigBuilder.parseConfiguration(XMLConfigBuilder.java:107)
	... 26 more
Caused by: org.apache.ibatis.builder.BuilderException: Error resolving class. Cause: org.apache.ibatis.type.TypeException: Could not resolve type alias 'bookMap'.  Cause: java.lang.ClassNotFoundException: Cannot find class: bookMap
	at org.apache.ibatis.builder.BaseBuilder.resolveClass(BaseBuilder.java:103)
	at org.apache.ibatis.builder.xml.XMLStatementBuilder.parseStatementNode(XMLStatementBuilder.java:72)
	at org.apache.ibatis.builder.xml.XMLMapperBuilder.buildStatementFromContext(XMLMapperBuilder.java:135)
	at org.apache.ibatis.builder.xml.XMLMapperBuilder.buildStatementFromContext(XMLMapperBuilder.java:128)
	at org.apache.ibatis.builder.xml.XMLMapperBuilder.configurationElement(XMLMapperBuilder.java:118)
	... 29 more
Caused by: org.apache.ibatis.type.TypeException: Could not resolve type alias 'bookMap'.  Cause: java.lang.ClassNotFoundException: Cannot find class: bookMap
	at org.apache.ibatis.type.TypeAliasRegistry.resolveAlias(TypeAliasRegistry.java:117)
	at org.apache.ibatis.builder.BaseBuilder.resolveAlias(BaseBuilder.java:130)
	at org.apache.ibatis.builder.BaseBuilder.resolveClass(BaseBuilder.java:101)
	... 33 more
Caused by: java.lang.ClassNotFoundException: Cannot find class: bookMap
	at org.apache.ibatis.io.ClassLoaderWrapper.classForName(ClassLoaderWrapper.java:190)
	at org.apache.ibatis.io.ClassLoaderWrapper.classForName(ClassLoaderWrapper.java:89)
	at org.apache.ibatis.io.Resources.classForName(Resources.java:256)
	at org.apache.ibatis.type.TypeAliasRegistry.resolveAlias(TypeAliasRegistry.java:113)
	... 35 more


Process finished with exit code -1
 ```
 ### 解决方法

``` java
  Mapper.xml文件中的
     <select id="listAll" resultType="bookMap" >
        select * from book b left join booktype bt on b.typeid=bt.typeid
    </select>
    resultType改为resultMap
  ```