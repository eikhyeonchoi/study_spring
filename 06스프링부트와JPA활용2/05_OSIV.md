# Open Session In View
```
spring.jpa.open-in-view : true 기본값

ON
트랜잭션 시작점(@Transactional)에 db connection을 가져온다
반환은 언제?
API 응답이 끝날 때 까지 영속성 컨텍스트와 db connection을 유지함
그래서 LAZY로딩이 가능했던 것
기본적으로 영속성 컨텍스트는 db connection을 유지함
오랫동안 db connection 리소스를 사용하기 때문에 실시간이라면 리소스가 부족할 수 있다

OFF
트랜잭션을 종료할 때 영속성 컨텍스트를 닫고, 데이터베이스 커넥션도 반환한다. 따라서 커넥션 리소스를 낭비하지 않는다.
모든 지연로딩을 트랜잭션 안에서 처리해야 한다. 따라서 지금까지 작성한 많은 지연 로딩 코드를 트랜잭션 안으로 넣어야 하는 단점이 있다.

고객 서비스의 실시간 API는 OSIV를 끄고, ADMIN 처럼 커넥션을 많이 사용하지 않는 곳에서는 OSIV를 켠다
```