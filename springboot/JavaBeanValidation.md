### Jakarta Bean Validation
Jakarta Bean Validation은 아래와 같은 기능을 하는 자바 사향이다.
 - 어노테이션을 통해 객제 모델에 제약을 표현할 수 있다.
 - 확장 가능한 방식으로 사용자 지정 제약 조건을 작성할 수 있다.
 - 개체 및 개체 그래프의 유효성을 검사하는 API를 제공한다.
 - 매개 변수의 유효성을 검사하고 메서드 및 생성자의 값을 반환하는 API를 제공한다.
 - 일련의 위반을 리포트한다.
 - Java SE에서 실행되며 Jakarta EE 8에 통합되어 있다.

### Bean Validation 2.0(JSR 380)
실제로 프로그래밍을 하다보면 어떤 객체의 값이 비었는지, 공백인지, 날짜가 이전인지 등의 유효성 검사를 빈번하게 수행하게 되고
이로 인해 유효성 검사 로직들이 여러 곳에 흩어지게 되는 경우도 있다. Java Bean Validation을 이용하면 검사 대상 클래스에
어노테이션 기반 제약조건을 선언하여 간결하게 유효성 검사를 할 수 있다.
Bean Validation은 자바에서 유효성 검사를 하는 방법에 대한 명세다. 즉, 실제로 동작하는 코드가 아니라는 것이다.
때문에 실제로 동작하려면 이 명세를 구현한 구현체가 있어야하는데 대표적인게 Hibernate Validator이다.
그리고 Spring Boot 2.0은 Bean Validation 2.0을 사용하고 있다.

#### Built-in Constraint Definitions
Bean Validation에는 대부분의 경우에 사용가능한 제약조건 어노테이션들이 내장(Built-in) 되어있다.
```javax.validation.constraints``` 패키지를 확인해보면 총 22개의 내장 제약조건이 있는것을 확인할 수 있다.

- @AssertFalse
- @AssertTrue
- @DecimalMax
- @DecimalMin
- @Digits
- @Email
- @Future
- @FutureOrPresent
- @Max
- @Min
- @Negative
- @NegativeOrZero
- @NotBlank
- @NotEmpty
- @NotNull
- @Null
- @Past
- @PastOrPresent
- @Pattern
- @Positive
- @PositiveOrZero
- @Size

이외의 자세한 내용은 [공식 홈페이지](https://beanvalidation.org/2.0/spec/#builtinconstraints) 의 ```8. Built-in Constraint definitions```
섹션을 참고하면 좋다.

### ref
 - [Jakarta Bean Validation](https://beanvalidation.org/)
 - [Bean Validation specification](https://beanvalidation.org/2.0-jsr380/spec/)
 - [Java Bean Validation 사용하기](https://do-study.tistory.com/99)
 - [Java Bean Validation Basics](https://www.baeldung.com/javax-validation)