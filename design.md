# Design 설계 주의 점 

## setter 사용 자제 
```
: 비즈니스로직을 그냥 클래스 안에 넣어둬라
: ex) addStock, removeStock 등
: + 객체를 만들거나 바꿀 떄에도 
: createXxx, changeXxx 이런식으로 사용하는 것이 좋다
```

## 연관 관계가 있다면 지연로딩으로 설정해야함
```
: EAGER XXXXX, LAZY OOOOOO
: @XToOne 은 기본이 즉시로딩이라 직접 지연로딩으로 설정해야함
: (fetch = FetchType.LAZY)
```

## 컬렉션은 필드에서 초기화할 것
```
: null 문제에서 안전함
: hibernate는 엔티티를 영속화 할 때, 컬랙션을 감싸서 하이버네이트가 제공하는 내장 컬렉션으로 변경함
```

## cascade
```
: @XToX에는 cascade옵션을 붙일 수 있다
: ex) Order - Delivery 일대일 매칭인데
: 원래는 order가 insert되고 delivery를 따로 insert를 해주는건데
: cascade를 사용하면 order가 insert될 때 delivery도 자동으로 insert를 해줌
: @OneToOne(mappedBy = "order", cascade = CascadeType.ALL)  
: private Devlivery delivery; // Order Class
: cascade는 이처럼 Delivery는 Order만이 관리?하기 때문에 사용한 것
: 이리 저리 묶여있다면 사용하면 안되는 것
```

## Update시
```
: 병합은 사용하지말고 변경감지를 사용해서 영속성 엔티티로 로직을 처리하자
: "변경 감지와 병합" 내용 확인
```

## 컨트롤러에서 어설프게 엔티티를 생성하지 마세요.
```
: 컨트롤러에서는 서비스쪽으로 "식별자만" 넘기고 서비스에서 jpa 영속성 엔티티를 얻어서 로직을 작성하는 것이 좋다
```

## API 설계시 엔티티는 직접 리턴하지마라 
```
: DTO에 담아서 JSON으로 리턴해야함, 연관관계 때문에 무한루프에 빠질 수 있음
```

## 연관관계 설정시 일단 단방향으로만 설계하라
```
: 필요하면 양방향 연관관계를 설정할 것
: 양방향연관 관계를 설정했다면 양쪽 다 객체를 설정해주는게 맞음 
: 아래 예제를 볼것
```

## 양방향 연관관계를 설정했을 경우엔 연관관계 편의 메서드를 생성할것
```
: 만약 멤버 - 팀의 경우
: 편의 메서드를 작성안하면?
: Team team = new Team();
: em.persist(team);
: Member member = new Member();
: member.setTeam(team);
: team.getMembers().add(member);
: 이런식으로 따로 따로 해줘야함 
: 그것보단 편의 메서드를 작성하자
: Member -> setTeam 또는 changeTeam
: {
:     this.team = team;
:     team.getMembers().add(this);
: }
: 물론 team.addMember(member); 이렇게해서 할 수도 있는데
: Team -> addMember
: {
:     member.setTeam(this);
:     getMembers().add(member);
: }
```

## 상속관계를 매핑할때
```
: @DiscriminatorColumn(name=“DTYPE”) 
: @DiscriminatorValue(“XXX”)
: 꼭 넣어주자
```

## 값 타입을 바꿀 경우(embeded type을 사용하는 경우)
```
ex)
class Member {
    @Embedded
    private Address address;
}
Member mebmer = em.find(Member.class, 1L);
member.setAddress(new Adderss("", "", ""));
-> primitive type이 아닌 이상
-> 이렇게 통으로 바꿔야함
-> equals, hashcode 오버라이딩 주의할 것!
```

## 묵시적 조인을 쓰면 망함
```
★★★ 
명시적 조인을 사용할 것
비추천 = select t.members from Team t
추천 = select m from Team t join t.members m 
비추천 = select m.team from Member m
추천 - select t from Member m join m.team t
해결책은 페치조인
★★★
```

## 벌크연산 사용후 영속성 컨텍스트를 초기화할 것
```
```

## API 설계시 엔티티를 파라미터로 받지도말고 리턴해서도 안된다
```
★★★
DTO를 만들어야하는데 inner class로 만들어도 됨
Request, Response 모두 다 엔티티를 사용하면 안되고 DTO 사용해야함
이렇게하면 엔티티 클래스들을 깔끔하게 할 수 있음 @NotEmpty, @JsonIgnore등을 안 쓸 수 있음

주의1 
엔티티를 직접 노출할 때는 양방향 연관관계가 걸린 곳은 꼭! 한곳을 @JsonIgnore 처리 해야 한다. 안그러면 양쪽을 서로 호출하면서 무한 루프가 걸린다. 
stackoverflow error

주의2 
지연로딩 오류는 엔티티를 직접 반환하는 경우 LAZY로 설정되어있으면 프록시기 때문에 오류가 발생하는거임
지연 로딩(LAZY)을 피하기 위해 즉시 로딩(EARGR)으로 설정하면 안된다! 즉시 로딩 때문에
연관관계가 필요 없는 경우에도 데이터를 항상 조회해서 성능 문제가 발생할 수 있다. 즉시 로딩으로
설정하면 성능 튜닝이 매우 어려워 진다.

위 두개의 주의 모두 엔티티를 직접 반환했을 때 발생하는 문제임
그냥 DTO 쓰면됨
★★★
```

## command query를 분리하자
```
public Member updateMember(Long id, String name) {
    Member member = memberRepository.findOne(id);
    member.setName(name);
    return Member;
}
-> command(update), query(return member);
-> 이건 command, query가 합쳐진 것
-> 분리하려면 메서드 리턴타입을 void로 하고 return을 앖앤다
```

## 엔티티 DTO변환  vs  DTO로 직접 조회
```
★★★
1. 우선 엔티티를 DTO로 변환하는 방법을 선택한다.
2. 필요하면 페치 조인으로 성능을 최적화 한다. 대부분의 성능 이슈가 해결된다.
3. 그래도 안되면 DTO로 직접 조회하는 방법을 사용한다(DTO 직접 조회시 fetch join 불가 -> join을 잘할 것)
4. 최후의 방법은 JPA가 제공하는 네이티브 SQL이나 스프링 JDBC Template을 사용해서 SQL을 직접 사용한다
★★★
```