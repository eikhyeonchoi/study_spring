# MVC 패턴 복습
## FrontController Pattern
```
프론트 컨트롤러 서블릿 하나로 클라이언트의 요청을 받음(입구 한개, 공통처리 가능)
Dispatcher Servlet이 FrontController Pattern으로 구현되어 있음

모든 요청은 먼저 Dispatcher Servlet이 받는다(WAS가 Request를 파싱해서 서블릿에 던짐)
그럼 Dispatcher Servlet은 Special Bean을 활용해(Handler Mapping, Handler Adapter 등)
핸들러를 찾아 핸들러를 호출함(스레드 활용)
```