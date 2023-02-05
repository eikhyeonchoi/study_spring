```
mybatis transaction 처리 : https://codevang.tistory.com/264
```

```
spring.datasource.url=jdbc:mysql://localhost:3306/testdb
spring.datasource.username=root
spring.datasource.password=admin
spring.datasource.driver-class-name:com.mysql.cj.jdbc.Driver

# 상위경로 - src/main/webapp/ 
spring.mvc.view.prefix=/WEB-INF/jsp/
spring.mvc.view.suffix=.jsp

# 패키지 잘 확인
mybatis.mapper-locations=classpath:mybatis/mapper/**/**.xml
mybatis.type-aliases-package=com.example.demo.entity 
```

```
dependencies {
	// spring boot에서 jsp 사용 시 추가해야함
	implementation 'javax.servlet:jstl'
    implementation "org.apache.tomcat.embed:tomcat-embed-jasper"
}
```
```
oracle일시 연결방법
https://po9357.github.io/spring/2019-05-12-MyBatis_Oracle/
https://itprogramming119.tistory.com/entry/Spring-Boot-Mybatis-Oracle-%EC%97%B0%EB%8F%99%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95
```

```
// jsp 기본 폼
<%@ page language="java" contentType="text/html; charset=utf-8"
         pageEncoding="utf-8" isELIgnored="false"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<html>
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
        <title>Spring Boot Application with JSP</title>
    </head>
    <body>
    </body>
</html>
```
```
// root-context.xml 기본 폼
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:mybatis-spring="http://mybatis.org/schema/mybatis-spring"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:jdbc="http://www.springframework.org/schema/jdbc"
	xmlns:task="http://www.springframework.org/schema/task"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="http://www.springframework.org/schema/jdbc http://www.springframework.org/schema/jdbc/spring-jdbc-4.3.xsd
		http://mybatis.org/schema/mybatis-spring http://mybatis.org/schema/mybatis-spring-1.2.xsd
		http://www.springframework.org/schema/task http://www.springframework.org/schema/task/spring-task-4.3.xsd
		http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd
		http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.3.xsd">
	
	<!-- Root Context: defines shared resources visible to all other web components -->
	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
		<property name="url" value="jdbc:mysql://localhost:3306/dbname"/>
		<property name="username" value=""/>
		<property name="password" value=""/>
	</bean>

	<!-- oracle database 인 경우, repository는 pom.xml에 추가해야함 -->
	<repositories>
		<repository>
			<id>oracle</id>
			<name>Oracle JDBC Repository</name>
			<url>http://repo.spring.io/plugins-release/</url>
		</repository>
	</repositories>
	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource" lazy-init="false">
		<property name="driverClassName" value="oracle.jdbc.driver.OracleDriver" />
		<property name="url" value="jdbc:oracle:thin:@localhost:1521:xe" />
		<property name="username" value="" />
		<property name="password" value=""/>
	</bean>   

	
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource"></property>
		<property name="configLocation" value="classpath:/mybatis-config.xml"></property>
		<property name="mapperLocations" value="classpath:mapper/**/*.xml"></property>
	</bean>
	
	<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate" destroy-method="clearCache">
		<constructor-arg name="sqlSessionFactory" ref="sqlSessionFactory"></constructor-arg>
	</bean>
	

	<!-- root-context.xml -> open with -> spring config editor 로 열어야 이거 가능함 -->
	<mybatis-spring:scan base-package="com.exam.demo.repo"/>

	<bean id="transactionManager"
		class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource" />
	</bean>
	<tx:annotation-driven transaction-manager="transactionManager" />	
</beans>
```

```
// mybatis-config 기본 폼
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd" >
<configuration>
<typeAliases>
		<typeAlias type="com.exam.demo.entity.Member" alias="Member"/>
	</typeAliases>
</configuration>
```

```
// 매퍼 기본 폼
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  
 <mapper namespace="repo or mapper 패키지 경로">
 	<resultMap  id="memberRm" type="Member">
 		<id property="id" column="id"/>
 		<result property="name" column="name"/>
 		<result property="createdAt" column="created_at"/>
 	</resultMap>
 	
    <select id="findAll" resultMap="memberRm">
        select * from member
    </select>
</mapper>
```