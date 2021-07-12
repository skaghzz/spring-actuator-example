# spring-actuator-example

1. [spring actuator](#1spring-actuator란-무엇인가)
2. [사용법](#2사용법)
3. [예제](#3예제)
4. [결론](#4결론)
5. [참고](#5참고)

## 1.spring actuator란 무엇인가
[공식문서](https://docs.spring.io/spring-boot/docs/current/reference/html/actuator.html)에는 다음과 같이 기재되어있다.
> ## Spring Boot Actuator: Production-ready Features   
> Spring Boot includes a number of additional features to help you monitor and manage your application when you push it to production.   
> You can choose to manage and monitor your application by using HTTP endpoints or with JMX.   
> Auditing, health, and metrics gathering can also be automatically applied to your application.

- 스프링 부트에는 운영 중인 애플리케이션을 HTTP나 JMX를 이용해서 모니터링하고 관리할 수 있는 기능을 제공하는데, 이것이 spring actuator이다.
- 시스템을 운영하다 보면 기능들이 잘 돌아가는지 지속적으로 모니터링하거나 서버를 재기동하거나 로그 정보를 변경하거나 등등 범애플리케이션 관점에서 처리해야 할 일이 많이 있을 때가 있다.
- 그러한 기능을 외부에서 쉽게 조작하게 만들기 위한 프로젝트가 spring actuator다.

## 2.사용법
- dependency를 추가한다.
```XML
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
</dependencies>
```

- endpoints는 모니터링이나 조작을 할 수 있도록 하는 연결점이다.
- 기본 endpoints를 제공되고 사용자가 필요에 따라 추가할 수도 있다.
- endpoints는 사용 여부(enabled/disabled)를 결정할 수 있고 노출 여부(exposed)를 결정할 수 있다.
- spring actuator에서 기본으로 제공하는 endpoints는 [여기](https://docs.spring.io/spring-boot/docs/current/reference/html/actuator.html#actuator.endpoints)에 있다.
- 몇 개의 endpoint를 보면 아래와 같다.
  - health : 애플리케이션 상태
  - metrics : 메모리, 스레드 상태 등
  - logger : logger 정보를 보고, logging level 변경도 가능하다.
  - startup/shutdown : 애플리케이션을 재기동에 사용하면 좋을 것 같다.
- endpoint별 용도와 사용방법은 모두 상이하니 [공식문서](https://docs.spring.io/spring-boot/docs/current/reference/html/actuator.html)를 참고합시다.

### 2.1 endpoint enabling
- shutdown을 제외한 모든 endpoint는 default가 enable이다.
- 다음과 같이 enable 속성을 변경할 수 있다.
```PROPERTIES
# application.properties
management.endpoints.web.exposure.include=health,info 
management.endpoint.health.show-details=always
```

### 2.2 endpoint exposing : 
- enable/disable은 endpoint를 내부적으로 제거해서 사용하지 않는 것이다. 어떤 endpoint를 사용할 수 있는지는 expose를 이용해서 조작이 가능다.
- exposing의 default는 [여기](https://docs.spring.io/spring-boot/docs/current/reference/html/actuator.html#actuator.endpoints.exposing)서 확인이 가능하다.
- default상태에서는 Web을 통해서는 health만 노출되어있다.

## 3.예제
```json
// 사용가능한 endpoint 목록을 확인할 수 있다.
// http://localhost:8080/actuator
{"_links":{"self":{"href":"http://localhost:8080/actuator","templated":false},"health-path":{"href":"http://localhost:8080/actuator/health/{*path}","templated":true},"health":{"href":"http://localhost:8080/actuator/health","templated":false},"info":{"href":"http://localhost:8080/actuator/info","templated":false}}}
```

```json
// http://localhost:8080/actuator/health
{"status":"UP","components":{"diskSpace":{"status":"UP","details":{"total":499963174912,"free":288819257344,"threshold":10485760,"exists":true}},"ping":{"status":"UP"}}}
```

## 4.결론
- 오픈소스 모니터링 툴과 연동하면 시스템 운영에 유용하게 이용 가능하다
- MSA 구조에서 eureka와 연동하면 유용하게 이용 가능할 것 같다.
- actuator는 시스템을 조작할 수 있는 권한이 있음에 따라 접근제어를 철저하게 관리해야 한다. spring security와 함께 많이 이용한다.
- 개발적 관점이 아닌 운영적 관점에서 기능을 알아보았다. spring은 운영에도 유용한 기능을 많이 제공한다.
- actuator관련 설정은 application 설정 파일에서 대부분 설정 가능하다.
- spring boot admin에서 spring actuator를 이용해서 애플리케이션을 모니터링하고 관리할 수 있는데 7/12 기준 spring boot 2.5.0.M1 버전 이상부터 지원을 하지 않는다.
  - [codecentric/spring-boot-admin](https://github.com/codecentric/spring-boot-admin)

## 5.참고
- https://docs.spring.io/spring-boot/docs/current/reference/html/actuator.html