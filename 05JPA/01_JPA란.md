# JPA 기본

## SQL 중심적인 개발의 문제점
```
객체를 관계형DB에 저장
SQL 중심적인 개발
반복이 잦다

객체지향과 관계형DB는 페러다임이 다름
설계방식이 달라 서로 호환?이 쉽지않음
다른 대안은 없음 

ex 1 ) 
class Member {
    Order order;
}
이렇게 domain이 있는데, Mybatis, JDBC등을 사용하는 경우엔
Member member = memberDAO.getMember(mebmerId);
member.getOrder(); // 이게 가능한지 눈으로 보지 않는 이상 알 수 없음
쿼리문을 봐야 알 수 있음 Order를 조인해서 꺼내는지 안꺼내는지

ex 2 )
Mybatis, JDBC등을 사용하는 경우엔
Member member1 = memberDAO.getMember(mebmerId);
Member member2 = memberDAO.getMember(mebmerId);
member1 != member // true
```

## JPA
```
애플리케이션과 JDBC 사이에서 동작함
표준명세 -인터페이스의 모음
JPA는 인터페이스고 이를 구현한게 Hibernate임

기본 사용법 예제
public static void main(String[] args) {
    // 엔티티 매니저 팩토리는 하나만 생성해서 애플리케이션 전체에 서 공유
    EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

    // 엔티티 매니저는 쓰레드간에 공유X (사용하고 버려야 한다)
    em = emf.createEntityManager();

    // JPA의 모든 데이터 변경은 트랜잭션 안에서 실행
    EntityTransaction tx = em.getTransaction();
    tx.begin();

    try {
        // 삽입
        // Member member = new Member();
        // member.setId(1L);
        // member.setName("helloA");

        // 찾기(one)
        // Member member = em.find(Member.class, 1L);

        // 찾기(all or condition)
        // List<Member> memberList = em.createQuery("select m from Member", Member.class).getResultList();

        // 삭제
        // Member member = em.find(Member.class, 1L);
        // em.remove(member);

        // 수정
        // Member member = em.find(Member.class, 1L);
        // member.setName("aaa");
        // jpa 꺼낸 객체는 영속성을 유지해 변화를 감지할 수 있다
        // = 영속성 컨텍스트
        tx.commit();
    } catch (Exception e) {
        tx.rollback();
    } finally {
        em.close();
        emf.close();
    }
}
```

## JPQL
```
 JPA는 SQL을 추상화한 JPQL이라는 객체 지향 쿼리 언어 제공
 JPQL은 엔티티 객체를 대상으로 쿼리
```