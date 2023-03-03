# API 개발기본
```
@Controller + @ResponseBody = @RestController
@RequestBody: 파라미터를 json으로 받음


엔티티를 DTO로 사용하지말자(Request, Response시 모두 사용X)
API 스펙을 위한 별도의 DTO를 만들어서 사용할 것(필수)


command&query 분리
public void update(...)
만약 여기서 public Member update(...) 이렇게 하면
command : update / query : Member 가 같이 있다 -> 분리해주는게 좋음
public void update(...) 하고나서 다시 find로 member 찾기
```

# API 개발고급
```
```
