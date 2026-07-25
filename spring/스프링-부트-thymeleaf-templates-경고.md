# Spring Boot에서 `Cannot find template location: classpath:/templates/` 경고

## 코드

```java
@SpringBootApplication
public class HelloSpringApplication {
    public static void main(String[] args) {
        SpringApplication.run(HelloSpringApplication.class, args);
    }
}
```

Thymeleaf 템플릿은 `src/main/resources/templates/hello.html`에 두고, 컨트롤러에서는 뷰 이름만 반환합니다.

```java
return "hello";
```

## 오류 코드

```text
WARN ... Cannot find template location: classpath:/templates/
```

## 문제 상황

원본 `src/main/resources/templates/` 폴더와 HTML 파일은 존재했지만, `main()` 실행 시 Thymeleaf가 템플릿 경로를 찾지 못한다는 경고가 출력됐다. 서버는 `Tomcat started`와 `Started HelloSpringApplication` 로그까지 출력하며 정상 실행됐다.

## 원인

`classpath:/templates/`는 원본 소스 폴더가 아니라 실행 중인 JVM의 리소스 경로다. 실행 classpath가 IntelliJ의 `out/production/classes`를 사용했고, 그 출력 폴더에 `templates/` 리소스가 반영되지 않아 Thymeleaf의 시작 시점 검사에서 경고가 발생한 것으로 판단했다.

## 해결 방법

1. `src/main/resources`가 Resources Root이고 `spring.thymeleaf.prefix`에 오타가 없는지 확인한다.
2. IntelliJ의 **Build and run using**을 Gradle로 설정하고 **Reload All Gradle Projects**를 실행한다.
3. `./gradlew processResources` 또는 Rebuild 후 `build/resources/main/templates/`에 템플릿 파일이 복사됐는지 확인하고 다시 실행한다.

## 해결 완료

Rebuild 또는 Gradle 리소스 처리 후 실행 출력 classpath에 `templates/`가 포함되면 경고가 사라지고 Thymeleaf 뷰를 정상적으로 찾는다.

## 정리

- `static/`은 가공 없이 제공하는 정적 파일, `templates/`는 Thymeleaf가 렌더링하는 뷰 파일용이다.
- WARN은 서버 시작 실패가 아니다. Spring Boot 애플리케이션은 Tomcat이 요청을 기다리므로 `main()` 실행 후 계속 실행된다.
- `spring.thymeleaf.check-template-location=false`는 템플릿을 사용하지 않는 경우에만 경고를 끄는 선택지이며, 템플릿을 사용할 때는 리소스 포함 문제를 먼저 해결한다.
