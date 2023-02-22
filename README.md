# **스프링과 핵심 원리**

# @ComponentScan

---

```java
@Configuration
@ComponentScan(
        basePackages = "hello.core.member",
        basePackageClasses = AutoAppConfig.class,
        excludeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = Configuration.class)
)
public class AutoAppConfig {

}
```

### basePackages

탐색할 패키지의 시작 위치를 지정. 입력한 패키지를 포함 한 하위 패키지 모두 탐색

모든 자바 클래스를 컴포넌트 스캔하면 시간이 많이 소요하기 때문에 필요한 위치부터 탐색하기 위해 사용

- 위 코드에서 basePackages = “hello.core.member”는 hello.core.member를 포함한 하위 패키지 모두를 탐색한다.
- basePackages = {“hello.core.member”, “hello.core.order”}   처럼 ,를 이용하여 여러 시작 위치 지정 가능

### basePackageClasses

지정한 클래스의 패키지를 탐색 시작 위치로 지정

- default : 시작위치는 @ComponentScan이 붙은 설정 정보 클래스의 패키지
- 설정 정보 클래스의 위치를 프로젝트 최상단의 두는 것을 추천
    - hello.core에 AppConfig같은 메인 설정 정보를 두고 @ComponentScan 을 붙이면  hello.core를 포함한 하위 패키지는 모두 컴포넌트 스캔의 대상이 됨

### @ComponentScan 기본 대상과 스프링 부가 기능

- @Component : 컴포넌트 스캔에 사용
- @Controller : 스프링 MVC 컨트롤러에서 사용
    - 스프링 MVC 컨트롤러로 인식
- @Service : 스프링 비즈니스 로직에서 사용
    - 특별한 처리 x, 개발자들이 핵심 비즈니스 로직을 파악하기 용이
- @Repository : 스프링 데이터 접근 계층에서 사용
    - 스프링 데이터 접근 계층으로 인식하고, 데이터 계층의 예외를 스프링 예외로 변환
- @Configuration : 스프링 설정 정보에서 사용
    - 스프링 설정 정보로 인식, 스프링 빈이 싱글톤을 유지하도록 추가 처리

**위의 어노테이션을 타고 들어가보면  @Component를 포함 하고 있음.**

```java
@Component
public @interface Service {

	/**
	 * The value may indicate a suggestion for a logical component name,
	 * to be turned into a Spring bean in case of an autodetected component.
	 * @return the suggested component name, if any (or empty String otherwise)
	 */
	@AliasFor(annotation = Component.class)
	String value() default "";

}
```
