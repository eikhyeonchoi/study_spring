```
spring.datasource.url=jdbc:mysql://localhost:3306/testdb
spring.datasource.username=root
spring.datasource.password=admin
spring.datasource.driver-class-name:com.mysql.cj.jdbc.Driver

spring.mvc.view.prefix=/WEB-INF/jsp/ # 상위경로 - src/main/webapp/ 
spring.mvc.view.suffix=.jsp

mybatis.mapper-locations=classpath:mybatis/mapper/**/**.xml
mybatis.type-aliases-package=com.example.demo.entity # 패키지 잘 확인
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
    // mysql
	implementation group: 'com.mysql', name: 'mysql-connector-j', version: '8.0.32'
	
	// https://mvnrepository.com/artifact/javax.servlet/jstl
    // spring boot 에서 jsp 실행하려면,,,
	implementation group: 'javax.servlet', name: 'jstl', version: '1.2'
	implementation group: 'org.apache.tomcat.embed', name: 'tomcat-embed-jasper', version: '9.0.60'
}
```
```
oracle일시 연결방법
https://itprogramming119.tistory.com/entry/Spring-Boot-Mybatis-Oracle-%EC%97%B0%EB%8F%99%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95
```

