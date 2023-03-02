# 스프링부트 JPA 활용
## 설정(start.spring.io)
```
gradle
spring web
lombok
h2(선택)
data jpa
thymeleaf(선택)
validation

p6spy
devtools
```

## 엔티티 설계 원칙
```
@ManyToMany 사용하지 말 것
ㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡ
case 1.
일대다 
ex) 회원(일)/주문(다) 테이블
: 회원은 여러 주문을 할 수 있고, 주문은 하나의 회원에만 속함

회원(일) 테이블
@OneToMany(mappedBy = "member")
private List<Order> orders = new ArrayList<>();

주문(다) 테이블
@ManyToOne
@JoinColumn(name = "member_id");
private Member member;

-> 회원은 주문을 여러 번 할 수 있으니 List
-> 한 주문은 한 회원에게만 속하니 단일 객체
-> 다대일도 마찬가지임
-> 딩연히 "다" 쪽에 FK가 들어가야함
ㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡ
case 2.
일대일
ex) 주문(일)/배달(일) 테이블
: 한 주문은 한 배달에 속함, 반대도 마찬가지
: 단 FK가 어디에 속할지는 정해야함 -> 주문에 속하도록 설계

주문(일) 테이블
@OneToOne
@JoinColumn(name = "delivery_id")
private Delivery delivery;

배달(일) 테이블
@OneToOne(mappedBy = "delivery")
private Order order;

-> 공식?
-> FK가 있는 테이블엔("다" 쪽 또는 FK설계한 테이블)
-> @JoinColumn이 들어가야함
-> FK 가 없는 테이블엔("일" 쪽 또는 FK 참조 테이블)
-> @ManyToOne, @OneToOne 속성으로 mappedBy를 넣어 참조 값을 지정해줘야함
ㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡ
case 3.
한 테이블내에서 계층 구조(일대다)
ex) 카테고리 계층구조
: Category 테이블 내에서 계층구조로 설계했을 때

@ManyToOne
@JoinColumn(name = "parent_id")
private Category parent;

@OneToMany(mappedBy = "parent")
private List<Category> child = new ArrayList<>();

: 하나의 카테고리에는 여러 자식 카테고리가 있을  수 있음 ex) 옷 -> 아우터, 신발, 팬츠 등
: 자식 카테고리는 하나의 부모 카테고리를 가짐 ex) 아우터의 부모는 옷, 신발의 부모도 옷
```

## 설계 주의점
```
1. setter 사용 자제 
: 비즈니스로직을 그냥 클래스 안에 넣어둬라
: ex) addStock, removeStock 등
: + 객체를 만들거나 바꿀 떄에도 
: createXxx, changeXxx 이런식으로 사용하는 것이 좋다

2. 값 타입은 변경 불가능하게 설계
: @Setter를 제거하고 생성자에서 모두 값을 초기화함->기본생성자가 없음
: JPA 구현 라이브러리가 객체를 생성할 때 리플렉션 같은 기술을 사용하기 때문에 기본생성자가 필요(protected로 만들 것)
: 즉, Embedded 타입이나 Entity는 기본생성자를 필요로 한다

3. 모든 연관관계는 지연로딩으로 설정
: 즉시로딩은 예측이 어렵고, JPQL 실행할 때 N+1 문제가 자주 발생함
: @XToOne 은 기본이 즉시로딩이라 직접 지연로딩으로 설정해야함(fetch = FetchType.LAZY)
: 한번에 가져오려면 fetch join을 사용

4. 컬렉션은 필드에서 초기화할 것
: null 문제에서 안전함
: hibernate는 엔티티를 영속화 할 때, 컬랙션을 감싸서 하이버네이트가 제공하는 내장 컬렉션으로 변경함
: ex)  List<OrderItem> orderItems = new ArrayList<>();

5. cascade
: @XToX에는 cascade옵션을 붙일 수 있다
: ex) Order - Delivery 일대일 매칭인데
: 원래는 order가 insert되고 delivery를 따로 insert를 해주는건데
: cascade를 사용하면 order가 insert될 때 delivery도 자동으로 insert를 해줌
: @OneToOne(mappedBy = "order", cascade = CascadeType.ALL)  
: private Devlivery delivery; // Order Class
: cascade는 이처럼 Delivery는 Order만이 관리?하기 때문에 사용한 것
: 이리 저리 묶여있다면 사용하면 안되는 것

6. Update시 ?
: 병합은 사용하지말고 변경감지를 사용해서 영속성 엔티티로 로직을 처리하자
: 아래 "변경 감지와 병합" 내용 확인

7. 컨트롤러에서 어설프게 엔티티를 생성하지 마세요.

8. 컨트롤러에서는 서비스쪽으로 식별자만 넘기고 서비스에서 jpa 영속성 엔티티를 얻어서 로직을 작성하는 것이 좋다
```

## 중요
```
변경 감지와 병합

준영속 엔티티?
: 영속성 컨텍스트가 더는 관리하지 않는 엔티티를 말한다.
: 수정을 시도하려는 객체(Pk가 설정된 객체)

준영속 엔티티는 수정하는 2가지 방법
1. 변경 감지 기능 사용(dirty check)
ex)
@Transactional
public void updateItem(Long itemId, Book param) {
    Item findItem = itemRepository.findOne(itemId);
    findItem.setPrice(param.getPrice());
    findItem.setName(param.getName());
    findItem.setStockQuantity(param.getStockQuantity());
}

2. 병합사용
@Transactional
void update(Item itemParam) { //itemParam: 파리미터로 넘어온 준영속 상태의 엔티티
    Item mergeItem = em.merge(item);
}
: itemParam은 새로 만든 준영속 엔티티임
: merge의 동작방식은 파라미터로 받은 준영속 엔티티 itemParam이 PK가 설정되어 있다면
: EntityManage에서 Pk를 가지고 em.find(Item.class, pk)해서 영속성 엔티티를 찾는다
: 그 영속성 엔티티의 값을 준영속 엔티티인 itemParam의 모든 값을 밀어넣는다
: merge의 동작방식은 아래 예제랑 완전히 동일하게 동작함
@Transactional
public Item updateItem(Long itemId, Book param) {
    Item findItem = itemRepository.findOne(itemId);
    findItem.setPrice(param.getPrice());
    findItem.setName(param.getName());
    findItem.setStockQuantity(param.getStockQuantity());
    return findItem;
}

병합 동작 방식
1. 준영속 엔티티의 식별자 값으로 영속 엔티티를 조회한다.
2. 영속 엔티티의 값을 준영속 엔티티의 값으로 모두 교체한다(병합)
3. 트랜잭션 커밋 시점에 변경 감지 기능이 동작해서 데이터베이스에 UPDATE SQL이 실행

: 주의점 -> 모든 속성이 파라미터로 받아온 값으로 완전히 변경됨
:           만약 파라미터의 name값이 null이면 null로 업데이트 되버림 
:           다 바꿔버리기 떄문에 선택할 수 있는 개념이 아님
:           선택적으로 필요한 부분만 update할 수가 없고 준영속엔티티의 모든 값을 덮어씌워야함

결론 -> update를 하는경우 병합사용을 사용하지 말고, 변경 감지를 사용해야한다!
```