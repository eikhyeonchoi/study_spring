# JAVA Grammer
```
WAR는 서버에 톰캣(WAS)를 별도로 설치하고 거기에 build된 파일을 넣을 때, jsp를 써야할 때
JAR는 내장 톰캣으로 바로 돌리는 경우 // webapp 경로도 사용하지 않음
```

```
private HashMap<Long, String> store = new HashMap<>();
store.values().stream().filter(member -> member.getName().equals(name)).findAny();
: .values() -> collection return
: .stream() -> stream return
: .filter() -> stream return
: .findAny() -> Optionnal return
: .map() -> js map이랑 비슷
```

```
Optionnal(java 8)
NullpointerException 문제를 해결할 수 있는 문법
.of(T value) -> 객체를 Optionnal로 감싸기
.ofNullable(T value) -> of와 동일하고, null 가능
.isPresent() -> Optionnal이 null인지 아닌지 판단가능 (if 대체가능)
.ifPresent(Consumer<? super T> consumer) -> 값이 있는 경우 작성된 기능을 실행 없을 경우 실행X
.get() -> 값이 존재하면 해당 값을 리턴 없을 경우 예외발생
.orElseGet(Supplier<? extends T> other) -> 값이 있는 경우 값 전달 없으면 Other 처리된 부분을 전달
.orElse(T other) -> Optional의 값이 있으면 값을 전달 아닌 경우 처리된 케이스 전달
```

```
Java 8의 Function은 1개의 인자(Type T)를 받고 1개의 객체(Type R)를 리턴하는 함수형 인터페이스
public interface Function<T, R> {
    R apply(T t);
}

ex1)
Function<Integer, Integer> func1 = x -> x * x;
Integer result = func1.apply(10);
System.out.println(result);
```