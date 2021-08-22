# TDD란?

TDD(Test Driven Development)는 테스트가 주도하는 개발이라는 뜻으로 개발을 하는데 있어서 테스트가 주가 되어 개발한다는 의미가 된다.
쉽게 설명하면 선 테스트 코드 작성, 후 구현이며 총 3가지 주요 절차로 이루어져 있다.

### TDD의 3가지 절차

1. 실패: 실패하는 테스트 케이스를 먼저 만든다.(처음부터 모든 실패 테스트 케이스가 아닌 지금 가장 먼저 구현할 기능 하나씩 테스트 케이스 작성)
2. 성공: 실패하는 테스트 케이스를 통과시키기 위하여, 코드를 작성하여 테스트를 통과시키는 것.
3. 리팩토링: 구현한 코드에 중복되는 코드가 있거나, 개선시킬 방법이 있다면 리팩토링 진행후 테스트 케이스가 성공하는지 확인.

위 3가지 절차가 끝나면 다음 기능 구현을 위해 새로운 실패하는 테스트 케이스를 작성한다.

### 기존 개발 프로세스 vs TDD 개발 프로세스

기존의 개발 프로세스
```
설계  ---> 개발(코드작성) ---> 테스트(코드작성)
 ^                               |
 └───────────────────────────────┘
            설계 수정
```

TDD 개발 프로세스
```
설계  ---> 테스트(코드작성) ---> 개발(코드작성) 
 ^              |
 └──────────────┘
     설계 수정
```

### TDD의 장점

TDD를 진행하면 테스트 케이스를 작성할때 주로 ```작은 단위```로 만들기 때문에, 코드를 작성 할 때 코드가 너무 방대해지지 않고, ```코드의 모듈화```가 자연스럽게 잘 이루어지면서 개발이 진행된다.
TDD를 하면 자연스레 ```테스트 커버리지가 높아질 수 밖에 없다```. 테스트를 먼저 작성을 하고 구현하기 때문이다. 테스트 커버리지가 높아지면 결국 ```리팩토링도 쉬워```지고 ```유지보수도 쉬워```진다.
결국 ```프로젝트의 퀄리티를 높이기에 좋은 환경이 구성``` 되며 ```협업 시에도 많은 도움```이 된다.
또한 ```버그에 낭비하는 시간도 최소```한으로 할 수 있고 ```구현 기능이 요구사항을 충족하는지 쉽게 확인``` 할 수 있다.

### F.I.R.S.T
1. Fast
   - 단위 테스트는 가능한 빠르게 실행되어야 한다. 실행함에 있어 너무 느려 테스트 실행을 꺼리게 된다면 잘못된 단위테스트이다.
   - ```@SpringBootTest```를 사용하면 해당 어플리케이션의 모든 빈을 IoC Container에 등록하고 테스트를 진행하기 때문에 테스트가 느려질 수 밖에 없다. 그래서 단위 테스트의 경우 ```Mockito```나 ```@DataJpaTest```를 사용해서 테스트 하는 것이 더 좋다.
  
2. Independent
   - 단위테스트는 ```객체의 상태, 메소드, 이전 테스트 상태, 다른 메소드의 결과```등에 의존해서는 안된다. 따라서 단위테스트는 어떤 순서로 실행하더라도 성공해야 한다.
  
3. Repeatable
   - 단위테스트는 반복 가능해야한다. DB에 의존하게 되는 현상과 같이 여러번 실행하는 경우 실패하면 안 된다.

4. Self-validating
   - 단위테스트는 자체검증이 가능해야한다. 테스트를 개발자가 직접 수동으로 확인할 필요 없이, ```Assert```문 등에 의해 성공 여부가 결과로 나타나야한다.
  
5. Timely
   - 단위테스트를 통과하는 제품코드가 작성되기 바로전에 단위테스트를 작성해야한다. TDD를 하고 있다면 적용이 되지만 그렇지 않을 수도 있다.

### JUnit5
Junit5는 전 버전과 다르게 서로 다른 3개의 서브 프로젝트의 여러 모듈로 구성된다.

```JUnit5 = JUnit Platform + JUnit Jupiter + JUnit Vintage```

- JUnit Platform
    - 테스트를 실행해주는 런처와 TestEngine API 제공한다.
- JUnit Jupiter
    - TestEngine API 구현체로 JUnit5 제공
- JUnit Vintage
    - TestEngine API 구현체로 JUnit3, JUnit4 제공
    
참고) JUnit4와 다르게 클래스와 메소드에 public을 붙을 필요가 없다.(Java Reflection을 사용하기 때문에 private나, default도 접근하고 실행 가능하다.)

### JUnit5 Annotation
- @Test
   - 기본적인 테스트를 나타냄
- @BeforeAll
   - 모든 테스트들이 실행되기 전에 딱 한번 먼저 실행되는 테스트. 
   - 반드시 static 메소드를 사용해야하며, private으로 만들 수 없고 return 타입이 있으면 안된다.
- @AfterAll
   - 모든 테스드들이 실행된 이후에 딱 한번 실행되는 테스트.
   - 반드시 static 메소드를 사용해야하며, private으로 만들 수 없고 return 타입이 있으면 안된다.
- @BeforeEach
   - 각각 테스트를 실행하기 이전에 실행되는 테스트.
   - static일 필요 없다.
- @AfterEach
   - 각각 테스트를 실행한 이후에 실행되는 테스트.
   - static일 필요 없다.
- @Disabled
   - 만들어놓은 테스트를 무시할때 사용.
- @DisplayName("원하는 이름")
   - 테스트 실행 시 해당 테스트의 이름을 원하는 이름으로 교체할 때 사용.

- 이외의 내용은 [JUnit5 Annotation](https://junit.org/junit5/docs/current/user-guide/#writing-tests-annotations) 참조

### Mockito
Mockito는 Mock(진짜 객체 처럼 동작하지만 프로그래머가 직접 컨트롤 할 수 있는 객체)을 지원하는 프레임워크이다.
Mock 객체를 만들고 관리하고 검증 할 수 있는 방법을 제공한다. 주고 특정 단위만 테스트 하고 싶을 경우 주로 사용한다.

### Mockito Annotation
- mock() / @Mock : Mock 만들기
  - Answer/MockSettings를 통해 동작하는 방법을 지정할 수 있다.
  - when() / given()는 mock이 어떻게 행동할지 지정한다.
  - 만약 제공된 Answers가 필요로 하는 것과 들어맞지 않는 경우 Answer interface를 확장하여 직접 작성할 수 있다.
- spy() / @Spy : partial mocking, 실제 메소드가 호출되지만 verify와 stub가 가능하다.
  - @InjectMock: @Spy 또는 @Mock Annotation이 달린 spies/mocks fields를 자동으로 주입한다.
  - verify(): 주어진 argument로 메소드가 호출되었는지 확인한다. 가변적인 argument에 대한 매칭을 사용할 수 있다.

### Mock vs MockBean
@Mock은 가짜 객체를 만들겠다는 의미이고, @MockBean은 가짜 빈을 만들겠다는 의미이다.

@WebMvcTest를 할 경우 service를 @Mock으로 만들면 Controller는 로드가 되지만 협력 객체인 service는 로드가 되지 않는다.
그래서 contoller에 실제로 요청을 보내도 service가 bean container에 생성되어 있지 않기 때문에 ```java.lang.IllegalStateException: Failed to load ApplicationContext```이 발생한다. 

### ref
 - [TDD의 소개](https://velog.io/@velopert/TDD%EC%9D%98-%EC%86%8C%EA%B0%9C)
 - [TDD(Test-driven Development) 소개](https://m.blog.naver.com/suresofttech/221569611618)
 - [JUnit5 공식 문서](https://junit.org/junit5/docs/current/user-guide/)
 - [Mockito 공식 문서](https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html)
 - [Spring boot test, junit5, mockito 사용에 대한 정리](https://wan-blog.tistory.com/71)
 - [Writing Your F.I.R.S.T Unit Tests](https://dzone.com/articles/writing-your-first-unit-tests)