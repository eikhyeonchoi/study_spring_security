# Summary

## 1. User Request

## 2. DelegatingFilterProxy
```
Servlet Filter는 spring bean을 사용할 수 없기 때문에
springSecurityFilterChain bean을 찾아 요청을 위임한다
```

## 3. FilterChainProxy
```
보안의 시작점
DelegatingFilterProxy으로 부터 요청을 위임받는다
spring security 초기화시 생성되는 filter를 관리,제어
request를 filter 순서대로 호출하여 전달 - 필터가 N개임
마지막 filter까지 인증 및 인가 예외가 발생하지 않으면 통과
```

## 4. Authentication
```
FilterChainProxy를 다 통과하면 유저는 자신이 누구인지를 증명해야함
유저 정보를 가지고 Authentication을 만든 후 AuthenticationManager를 통해 인증/인가 절차를 거침
```

## 5. SecurityContext
```
AuthenticaitonManager를 통과했다면 SecurityContext에 Authentication 객체를 저장함
ThreadLocal에 저장되어 아무 곳에서나 참조가능 -> ThreadSafe
```

## 6. SecurityContextPersistenceFilter
```
FilterChainProxy에 2번 째에 위치한 Filter이며
인증 전이면 SecurityContextHolder에 SecurityContext를 생성해 저장한다(이때 Authentication = null 이다)
그다음 Filter를 거치면서 Authenticaiton이 만들어지면 SecutityContext에 저장하고
session에 SecurityContext를 저장한다

인증 후라면 session에서 SecurityContext를 load하고 SecurityContextHolder에 저장
```