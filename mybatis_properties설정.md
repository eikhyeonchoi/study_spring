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
	implementation 'org.springframework.boot:spring-boot-starter-jdbc'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	implementation 'org.mybatis.spring.boot:mybatis-spring-boot-starter:2.3.0'
	compileOnly 'org.projectlombok:lombok'
	annotationProcessor 'org.projectlombok:lombok'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
	
	// https://mvnrepository.com/artifact/com.mysql/mysql-connector-j
	implementation group: 'com.mysql', name: 'mysql-connector-j', version: '8.0.32'
	
	implementation 'javax.servlet:jstl'
    implementation "org.apache.tomcat.embed:tomcat-embed-jasper"
}
```
```
oracle일시 연결방법
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
// 매퍼 기본 폼
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  
 <mapper namespace="com.example.demo.repo.MemberRepo">
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

```
// spring mvc 빈 설정 폼
<!-- Root Context: defines shared resources visible to all other web components -->
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
	<property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
	<property name="url" value="jdbc:mysql://localhost:3306/testdb"/>
	<property name="username" value="root"/>
	<property name="password" value="admin"/>
</bean>

<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
	<property name="dataSource" ref="dataSource"></property>
	<property name="configLocation" value="classpath:/mybatis-config.xml"></property>
	<property name="mapperLocations" value="classpath:mappers/**/**.xml"></property>
</bean>

<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate" destroy-method="clearCache">
	<constructor-arg name="sqlSessionFactory" ref="sqlSessionFactory"></constructor-arg>
</bean>
```