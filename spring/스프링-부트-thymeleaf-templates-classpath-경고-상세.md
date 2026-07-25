# Spring Boot Thymeleaf `classpath:/templates/` 경고 상세

## 코드

```text
src/main/resources/
├── static/
│   ├── hello-static.html
│   └── index.html
└── templates/
    ├── hello.html
    └── hello-template.html
```

```java
@Controller
public class HelloController {
    @GetMapping("/hello")
    public String hello() {
        return "hello";
    }
}
```

`return "hello"`는 기본 설정에서 `classpath:/templates/hello.html`을 찾는다. `static/`은 가공 없이 제공하는 파일용이고, Thymeleaf 문법을 렌더링할 HTML은 `templates/`에 둔다.

## 오류 코드

```text
WARN ... DefaultTemplateResolverConfiguration :
Cannot find template location: classpath:/templates/
(please add some templates, check your Thymeleaf configuration,
or set spring.thymeleaf.check-template-location=false)
```

## trouble

원본 프로젝트에는 `src/main/resources/templates/`와 템플릿 파일이 존재했고 `src/main/resources`도 Resources Root로 인식됐다. 그러나 `HelloSpringApplication.main()` 실행 시 JVM classpath가 `out/production/classes`를 사용했고, Thymeleaf가 그 실행용 classpath에서 `templates/` 위치를 찾지 못해 경고를 출력했다.

`Tomcat started on port 8080`과 `Started HelloSpringApplication`이 뒤이어 출력됐다면 서버 시작은 성공한 것이다. 프로그램이 계속 실행되는 것은 내장 Tomcat이 HTTP 요청을 기다리기 때문이며, 종료하려면 IntelliJ의 Stop 버튼 또는 터미널의 `Control + C`를 사용한다.

## 원인

`classpath:/templates/`는 macOS 절대 경로가 아니라 JVM이 실제 실행에 사용하는 리소스 경로다. Gradle 실행에서는 보통 `build/resources/main/templates/`가, IntelliJ 자체 빌드에서는 보통 `out/production/classes/templates/`가 classpath에 포함된다.

이번 로그의 `out/production/classes`는 IntelliJ 빌드 결과로 실행됐음을 보여준다. 이 출력 폴더에 리소스가 아직 복사되지 않았거나 오래된 출력이 남아 있으면, 원본 `templates/` 폴더가 정상이어도 경고가 발생할 수 있다. Rebuild가 해결한 이유는 출력 결과를 다시 만들면서 리소스를 복사했기 때문이다.

## 해결 방법

1. `src/main/resources`가 Resources Root인지와 `application.properties`의 `spring.thymeleaf.prefix`가 `classpath:/templates/`인지 확인한다. 기본값을 사용한다면 prefix 설정은 생략한다.
2. IntelliJ의 **Settings → Build, Execution, Deployment → Build Tools → Gradle**에서 **Build and run using**과 **Run tests using**을 Gradle로 설정한 뒤 **Reload All Gradle Projects**를 실행한다.
3. `./gradlew processResources` 또는 Rebuild를 실행하고 `build/resources/main/templates/` 또는 현재 실행 출력 폴더에 템플릿이 복사됐는지 확인한 뒤 애플리케이션을 다시 실행한다.

## 확인 방법

```bash
# IntelliJ 자체 빌더를 사용할 때 확인
find out/production/classes/templates -maxdepth 1 -type f

# Gradle 리소스 처리 결과 확인
./gradlew processResources
find build/resources/main/templates -maxdepth 1 -type f
```

템플릿 파일이 실제 실행 classpath에 포함된 상태에서 다시 실행해 WARN이 사라지는지 확인한다. `spring.thymeleaf.check-template-location=false`는 Thymeleaf를 전혀 사용하지 않는 프로젝트에서 경고 검사를 끌 때만 사용하고, 템플릿을 사용할 때는 원인을 먼저 해결한다.

## 해결 완료

Rebuild 또는 Gradle 리소스 처리 후 실행 classpath에 `templates/`가 포함되면 Thymeleaf 경고가 사라지고 `return "hello"`가 템플릿을 정상적으로 찾는다.

## 정리

- Gradle과 Maven은 의존성 다운로드, 컴파일, 리소스 복사, 테스트, 실행을 담당하는 빌드 도구다. `build.gradle`이 있으면 Gradle 프로젝트다.
- Build는 변경분을 준비하고, Rebuild는 기존 출력물을 지운 뒤 전체를 다시 준비하며, Run은 준비된 결과로 `main()`을 실행한다.
- Spring Boot 프로젝트는 IntelliJ·터미널·CI 환경이 같은 Gradle 설정을 사용하도록 빌드와 실행을 Gradle에 위임하면 리소스 classpath 차이를 줄일 수 있다.
