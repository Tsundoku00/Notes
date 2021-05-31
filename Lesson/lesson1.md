显示包含强调的项目,git提交一次就行了
[Toc]
### 如何创建一个maven java 项目

准备book和booktype表

1. 新建maven java项目
2. 写依赖 mysql org.batics resources
   
``` java
<dependencies>
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.32</version>
    </dependency>
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.2.8</version>
    </dependency>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
  </dependencies>

  <build>
    <resources>
      <resource>
        <directory>src/main/java</directory>
        <includes>
          <include>**/*.xml</include>
        </includes>
      </resource>
      <resource>
        <directory>src/main/resources</directory>
        <includes>
          <include>**/*.*</include>
        </includes>
      </resource>
    </resources>
  </build>
  ```
  3. mybatis-config.xml

``` java
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="dev">
        <environment id="dev">
            <transactionManager type="jdbc"> </transactionManager>
            <dataSource type ="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"></property>
                <property name="url" value="jdbc:mysql://127.0.0.1:3306/exam?useUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"></property>
                <property name="password" value="1234"></property>
            </dataSource>
        </environment>
    </environments>
    <mappers>
<mapper resource="com/zfz/dao/BookMapper.xml"></mapper>
    </mappers>
</configuration>
 ```
 4. Mapper.xml

``` java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    <mapper namespace="com.zfz.dao.BookDao">
    <select id="listAll" resultType="com.zfz.pojo.Book">
        select * from book
    </select>
    <select id="findBookByBooID" resultType="com.zfz.pojo.Book" parameterType="int">
        select * from book where bookid=#{bookid}
    </select>
    <delete id="delBookByBookID" parameterType="int">
        delete from book where bookid=#{bookid}
    </delete>
    <insert id="addBook" parameterType="com.zfz.pojo.Book">
        insert into book values (null,#{bookname},#{price},#{pubtime},#{author},#{typeid})
    </insert>
    <update id="updateBook" parameterType="com.zfz.pojo.Book">
        update book set bookname=#{bookname},price=#{price},pubtime=#{pubtime},author=#{author},typeid=#{typeid}
where bookid=#{bookid}
    </update>
</mapper>
 ```
 5.utils中的MybatisUtil

 ``` java
 package com.zfz.utils;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.Reader;

public class MybatisUtil {
    private static SqlSessionFactory build;
    private static Reader reader =null;
    //静态方法只执行一次
    static {
        try {
             reader = Resources.getResourceAsReader("mybatis-config.xml");
        } catch (IOException e) {
            e.printStackTrace();
        }
         build = new SqlSessionFactoryBuilder().build(reader);
    }
    public static SqlSession getSqlSession(){
        if (build==null){
            build = new SqlSessionFactoryBuilder().build(reader);
        }
            return build.openSession(true);
    }
    public static void close(){
        build.openSession().close();
    }

}
 ```
6. 测试

``` java
package com.zfz;

import static org.junit.Assert.assertTrue;

import com.zfz.dao.BookDao;
import com.zfz.pojo.Book;
import com.zfz.utils.MybatisUtil;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.Reader;
import java.util.Date;
import java.util.List;

/**
 * Unit test for simple App.
 */
public class AppTest {
    @Test
    public void listAll() throws Exception {
        Reader reader = Resources.getResourceAsReader("mybatis-config.xml");
        SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader);
        SqlSession sqlSession = factory.openSession();
        BookDao mapper = sqlSession.getMapper(BookDao.class);
        List<Book> books = mapper.listAll();
        for (Book book : books) {
            System.out.println(book);
        }
    }
    @Test
    public void delBook(){
        SqlSession sqlSession = MybatisUtil.getSqlSession();
        BookDao mapper = sqlSession.getMapper(BookDao.class);
        mapper.delBookByBookID(49);
    }
    @Test
    public void addBook(){
        SqlSession sqlSession = MybatisUtil.getSqlSession();
        BookDao mapper = sqlSession.getMapper(BookDao.class);
        Book book = new Book();
        book.setBookname("三体");
        book.setPrice(76.0);
        book.setAuthor("刘慈欣");
        book.setTypeid(1);
        book.setPubtime(new Date());
        mapper.addBook(book);
    }
    @Test
    public void updateBook(){
        SqlSession sqlSession = MybatisUtil.getSqlSession();
        BookDao mapper = sqlSession.getMapper(BookDao.class);
        Book book = new Book();
        book.setBookname("球状闪电");
        book.setPrice(76.0);
        book.setAuthor("刘慈欣");
        book.setTypeid(1);
        book.setPubtime(new Date());
        book.setBookid(48);
        mapper.updateBook(book);
    }
}
 ```
 初步完成,但还是存在问题,暂且当作测试


