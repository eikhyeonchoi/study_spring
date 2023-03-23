# TDD IMPORT and H2 Connection
```
import static org.junit.jupiter.api.Assertions.*;
import static org.assertj.core.api.Assertions.*;
```

```
// h2 연결

1. jdbc:h2:~/<dbname> (최소 한번)

2. ~/jpashop.mv.db 파일 생성 확인

3. 이후 부터는 jdbc:h2:tcp://localhost/~/<dbname> 
```