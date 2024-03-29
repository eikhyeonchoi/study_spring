# Annotations
```
컴포넌트 스캔과 자동 의존관계 설정

@ComponentScan
: 컴포넌트 스캔은 이름 그대로 @Component 애노테이션이 붙은 클래스를 스캔해서 스프링 빈으로 등록
: 스프링 빈의 기본이름은 클래스명을 사용하되 맨 앞글자만 소문자로 바꾼다
: 직접 지정할 수 있다 @Component("abc")
: @Autowried를 사용해 DI를 완성한다
: 스프링부트로 시작한다면 @SpringBootApplication에 @ComponentScan이 붙어있다
: 스캔 범위를 직접 지정할 수 있으며, 기본은 이 어노테이션이 붙은 패키지이다 -> 보통 최상단에 두는 것이 관례

@Controller - Controller, class 에서 사용
@Service - Service, class 에서 사용
@Repository - Repository, class 에서 사용
: 스프링컨테이너에 객체를 만들어 넣는다
: 컴포넌트 스캔 방법임 -> 사실 다 @Component임 scope은 패키지임 메인패키지 하위만 스캔

@Autowried - Controller, constructor 에서 사용
: 스프링컨테이너에 있는 객체를 알아서 연결시켜줌 
: Controller - Service - Repository를 연결시켜주는 역할
: Controller는 Service가 필요하고 Service는 Repository가 필요함
: 이 annotation이 Dependancy Injection(=DI) 역할을 하는 듯
: 당연하겠지만, Spring Bean으로 등록된 객체만 Autowried가 동작한다

@Qulifier("name")
: 동일한 타입의 스프링 빈이 여러개 있을 경우 이름을 지정해서 주입할 수 있게 해줌
: 생성자 파라미터에도 붙여줘야하고 주입 되는 빈에도 적어줘야함
: @Primary보다 우선순위가 높다

@Primary
: 동일한 타입의 스프링 빈이 여러 있다면 이 어노테이션이 붙은 빈이 우선순위가 높다
: 주입되는 빈에만 적어줘도 된다 
```

```
자바 코드로 직접 스프링 빈 등록하기

@Configuration - class에서 사용
: 스프링에 bean객체를 직접 넣고 싶을 때 사용
: 해당 어노테이션이 붙은 class를 설정 정보로 사용함
: 이 어노테이션이 스프링 컨테이너의 싱글톤을 유지시켜주는 어노테이션이다
: class에 붙여 사용하게 되는데, 스프링이 이 어노테이션이 붙은 클래스를 상속받아서
: CGLIB 기술을 사용해 상속받은 가짜 클래스를 스프링 컨테이너로 등록해서 싱글톤을 유지시켜준다
: @Bean과 같이 사용하며 @Bean 어노테이션이 붙은 메서드를 모두 호출해서 컨테이너 등록함(key: 메서드명, value: 객체)


@Bean - 메서드에 사용
: 스프링컨테이너에 객체를 직접 넣고 싶을 때 사용한다
: 해당 어노테이션이 붙은 메서드를 컨테이너가 호출해 객체를 반환받아 저장해서 관리함(key: 메서드명, value: 객체)
: 스프링 빈이라고 칭함
: 이 어노테이션이 붙은 메서드마다 메타정보를 생성한다
: 스프링 컨테이너는 이 메타정보를 기반으로 스프링 빈을 생성함
: @Configuration과 같이 사용한다

@PostConstruct, @PreDestroy
: 빈 생명주기와 관련된 어노테이션
: @PostConstruct: 의존성 주입이 시작되고 호출(사용 전 호출)
: @PreDestroy: 빈이 소멸되기 전 호출

※ 스프링 빈 생명주기
컨테이너 생성 => 빈 등록 -> 의존성 주입 -> @PostConstruct -> 사용 -> @PreDestroy -> 소멸
```

```
:: 파라미터로 받을 수 있는 항목들 :: 
https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-ann-arguments
@RequestParam
@RequestPart
@PathVariable
@RequestBody
HttpServletRequest request,
HttpServletResponse response,
HttpMethod httpMethod,
Locale locale,
@RequestHeader MultiValueMap<String, String> headerMap,
@RequestHeader("host") String host,
@CookieValue(value = "myCookie", required = false) String cookie
```

```
:: 리턴타입 :: 
https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-annreturn-types
```

```
@GetMapping
@PostMapping
@PutMapping
@DeleteMapping
@RequestMapping
: http 메소드 매핑


@RequestParam
: 요청파라미터 받기
: value - 받을 파라미터 명
: required - 필수 여부
: default - 기본값


@RestController - 클래스에 사용(@Controller, @ResponseBody 합친 것)
: class에 붙이면 클래스 전체가 전체가 api화(?)된다
: viewResolver 대신 HttpMessageConverter를 통해 문자면 그냥 문자를 반환함, 객체라면 json으로 반환함
: @Controller와는 다르게 @RestController는 리턴값에 자동으로 @ResponseBody가 붙게되어 별도 어노테이션을 명시해주지 않아도 HTTP 응답데이터(body)에 자바 객체가 매핑되어 전달 된다.
: @Controller - 반환 값이 String 이면 뷰 이름으로 인식된다. 그래서 뷰를 찾고 뷰가 랜더링 된다.
: @RestController - 반환 값으로 뷰를 찾는 것이 아니라, HTTP 메시지 바디에 바로 입력한다.


@PathVariable - path에 매핑된 파라미터 받기


@RequestBody 
: Http request body를 통쨰로 받겠다는 의미
: request body에 담긴 값을 자바객체로 conversion해줌
: HTTP요청의 body가 그대로 전달되어 HttpMessageConverter를 사용함
: 쉽게말해 request body -> 자바객체
: 대신 자바객체에는 빈생성자가 있어야한다
: serialize = Object -> byte stream / Deserialize = byte stream -> Object
: serialize, deserialize할 때 생성자 없는 파라미터를 이용함


@ResponseBody - 클래스, 메서드에 사용
: Http response body를 통째로 내보내겠다는 의미


@SpringBootTest - 스프링 컨테이너와 테스트를 함께 실행
@Transactional - 테스트 완료 후 DB rollback을 한다(테스트 반복을 위해) 테스트 case에 붙었을때만 rollback
: 테스트 할 때 사용할 anno(@Transactional은 다른곳에서도 사용한다)
```

```
JPA

@Entity - domain, class에 사용
: jpa가 관리하는 객체로 설정


@Id - domain 인스턴스 필드에 설정
: PK 설정
: 기본 키를 매핑하려면
: @GeneratedValue와 같이 사용해주면 된다
: 언에 따라 위의 세 가지 전략을 자동으로 지정한다.


@Column(name = "column name") - domain 인스턴스 필드에 설정
: column 매칭
: nullalbe, unique, length 등 옵션 확인


@Table(name = "table name") - 클래스에 사용
: 테이블 이름 지정


@ManyToMany
@OneToMany
@ManyToOne - LAZY 해야함
@OneToOne -LAZY 해야함
: 엔티티 관계 설정
: fetch(EAGER, LAZY)
: 즉시, 지연로딩을 설정할 수 있으며
: 지연로딩의 경우 프록시 객체를 사용하기 때문에 딱 사용할 때 영속성 컨텍스트에 요청한 후 
: DB에서 가져오고 실객체 Entity에 값을 설정한 후 프록시객체에 레퍼런스 값(실객체 참조값)을 설정한다
: 그 후 실객체 메서드를 호출하는 것 -> 때문에 지연로딩이라고 한다
: @XToOne은 default가 EAGER이기 때문에 LAZY로 사용할 것
:
: cascade(ALL, PERSIST, REMOVE)
: 영속성을 전이하는 어노테이션
ex)
class Parent {
    @OneToMany(mappedBy = "parent", cascade = CascadeType.ALL)
    private List<Child> childList = new ArrayList<>();
}
: child는 다른 테이블과 관계없을 때(오직 Parent와 관계가 있을 때)만 사용해야하며
: parent.getChildList().add(new Child()); // 하면? child 추가됨
: em.remove(parent); // parent와 연관된 모든 child도 같이 삭제

: orphanRemovel(Boolean)
: 부모 엔티티와 연관관계가 끊어진 자식 엔티티를 자동으로 식제
: parent.getChildList().remove(0); // 이것만 해도 child 1개가 삭제됨
: 
: mappedBy
: 양방향 매핑을 구현할 때 "일"쪽에 쓰는 어노테이션이며
: "다" 쪽에 어느 필드와 매핑되는지 명시해야 한다
: class Parent를 참고할 것


@JoinColumn(name = "컬럼명")
: FK 매칭
: 주로 @ManyToOne과 같이 쓰임
: 중요!
: name 옵션은 
: 대상 테이블의 PK를 적는줄 알았지만 FK이름을 지정하는거임
: 원래 referencedColumnName를 사용해서 대상 테이블의 FK를 지정하는 것인데
: 지정하지 않으면 대상엔티티의 PK를 JPA가 알아서 외래키로 걸어줌
: 즉 이 어노테이션은 "외래키" 컬럼을 만드는 어노테이션이다


@Embeddable - 클래스에 사용
@Embedded - 필드에 사용
@AttributeOverride - 컬럼이름이 겹쳤을때 사용
: 일일히 컬럼을 나열하는게 아닌
: 공통적인 필드를 하나의 객체로 관리할 수 있게끔 해주는 문법
: String zipcode, address1, address2 
: 이렇게 필드 3개를 생성하느니
: @Embeddable class Address를 만들어서 사용하는게 좋음
: 다른곳에서 Address를 사용할땐
: @Embedded Address address 이렇게 사용
:
: 만약 2개 써서 컬럼이름 겹치면?
: @AttributeOverride(name= "zipcode", column = @Column(name = "a_zipcode"))
: @AttributeOverride(name= "address1", column = @Column(name = "a_address1"))
: @AttributeOverride(namess2 이렇게 사용


@Enumerated(EnumType.STRING)
: enum 타입을 사용할 때...
: 꼭 EnumType을 String으로 설정해야줘야함


@PersistenceContext - 권장
EntityManager em;
: jpa의 entity manager를 주입받을 수 있다
: @Autowired보다 thread safe 하다 
em.persist(Object) - insert
em.find(Class, id) - 단건조회PK
em.createQuery(query, Class) - 쿼리
em.merge(Object) - update


@Transactional
: 읽기일땐 readOnly = true
: @Transactional은 클래스나 메서드에 붙여줄 경우, 해당 범위 내 메서드가 트랜잭션이 되도록 보장해줌


@Inheritance(strategy = InheritanceType.옵션)
: 엔티티 상속을 구현하는 어노테이션이며
: 부모엔티티에 선언한다
: JOINED: 조인 전략(자식테이블에 PK,FK가 한 컬럼으로 들어감)
: SINGLE_TABLE: 단일 테이블 전략(한 테이블에 다 들어감)
: 간단하다면 그냥 json으로 밀어넣는것도 방법


@DiscriminatorColumn(name="DTYPE") 
@DiscriminatorValue("XXX")
: 엔티티 상속을 구현하는 어노테이션이며
: @DiscriminatorColumn은 부모 클래스에 넣는다
: 해당 어노테이션을 넣으면 부모 테이블 컬럼이 name이름으로 추가되서 구분지을 수 있게 해준다
: @DiscriminatorValue은 자식 클래스에 넣는다
: 부모테이블에 구분컬럼의 어떤 값이 들어갈지 정할 수 있다


@MappedSuperclass
: 공통 매핑 정보가 필요할 때 사용(create_dt, update_dt 등)
```

```
spring data JPA

@Query(value = "", nativeQuery = boolean)
: 쿼리를 명시할 때(interface에)
: value - 쿼리내용
: nativeQuery - 네이티브쿼리인지?
@EntityGraph(attributePaths = {"team"})
: @Query 사용시 페치조인 추가
@Param
: @Query 사용시 파라미터 바인딩할 때(파라미터)


@Modifying(clearAutomatically = true)
: 벌크 수정 및 수정 후 영속성 컨텍스트 초기화


@EnableJpaAuditing
: Auditing할 때 스프링 부트 설정클래스에 명시해야함
@CreatedDate
: 생성일
@LastModifiedBy
: 수정일
```

```
Querydsl

@QueryProjection - 클래스->생성자에 사용
: DTO까지 Qtype으로 관리함
```

```
spring mvc

@ServletComponentScan - 서블릿 어플리케이션에 명세
: 서블릿 자동 등록


@WebServlet(name = "helloServlet", urlPatterns = "/hello") - 서블릿 애노테이션
: name: 서블릿 이름
: urlPatterns: URL 매핑

@ControllerAdvice
@RestControllerAdvice
: 대상으로 지정한 여러 컨트롤러에 @ExceptionHandler , @InitBinder 기능을 부여해주는 역할을 함
: @ControllerAdvice 에 대상을 지정하지 않으면 모든 컨트롤러에 적용된다.

@ExceptionHandler
```