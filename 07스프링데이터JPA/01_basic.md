# basic
```
interface를 만들고 JpaRepository를 상속받으면
spring data jpa가 interface 구현클래스를 생성해줌(Proxy 기술)
@Repository 어노테이션을 생략가능



@EnableJpaRepositories(basePackages = "jpabook.jpashop.repository")
Boot 사용시 생략가능


org.springframework.data.repository.Repository 를 구현한 클래스는 스캔 대상(JpaRepository<>)
spring data jpa가 interface 구현체(Proxy -> class com.sun.proxy.$ProxyXXX)를 만들어서 injection
public interface TeamRepo extends JpaRepository<Team, Long> {}
@Repository annotation 생략가능



JpaRepository
```
