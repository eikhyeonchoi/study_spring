# Cautions
```
querydsl cannot find symbol
Help > Find Action > Reload All Gradle Projects
```

```
@GeneratedValue(strategy = GenerationType.IDENTITY)
strategy 지정하지 않으면
모든 테이블 PK가 공유됨?? -> 같이올라감
A table -> 1,2,3
B table -> 4,5,6 ...
```

```
page 객체 -> dto로 변환
Page<Member> page = memberRepository.findByAge(10, pageRequest);
Page<MemberDto> dtoPage = page.map(m -> new MemberDto());
```

```
만약 일대다 관계에서 쿼리문을 수행하는 경우엔
fetch 조인으로 하면 안되고 batchsize를 이용해야 정상 결과를 얻을 수 있다
```

```
builder 작성시 null인거 예외처리 해야함
```