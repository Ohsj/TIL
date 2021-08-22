# BeanPostProcessor 란?

- 빈 후처리기라고도 불린다.
- 빈(Bean)의 설정을 후처리(PostProcessing)함으로써 빈의 생명 주기와 빈 팩토리의 생명주기에 관여한다.
- 빈의 초기화 되기 전, 초기화 된 후 2개의 기회를 제공한다.
- 빈 프로퍼티의 유효성 검사 등에 사용한다.
- 다른 초기화 메소드인 afterPopertiesSet()과 init-method가 호출되기 직전과 직후에 호출된다.

### 제공 메소드
- 초기화 되기 전 처리
```
Object postProcessBeforeInitialization(Object bean, String beanName) throw BeansException
``` 

- 초기화 된 후 처리
```
Object postProcessAfterInitialization(Object bean, String beanName) throw BeansException
```

### 빈 후처리 과정
1. 빈 객체(Bean Instance) 생성 (Constructor 또는 Factory Method 사용)
2. 빈 프로퍼티(Bean Property)에 값과 빈 레퍼런스 설정.
3. Aware Interface 에 정의된 Setter 메서드 호출
4. 빈 인스턴스(Bean Instance) 를 각 빈 후처리기 (Bean Post Processor)의 postProcessBeforeInitialization() 에 전달.
5. 초기화 Callback 메서드 호출
6. 빈 인스턴스(Bean Instance) 를 각 빈 후처리기 (Bean Post Processor)의 postProcessAfterInitialization() 에 전달.
7. 빈 사용 준비 완료
8. 컨테이너 종류 후, 소멸(Destructor) Callback 메서드 호출

### @Autowired는 어떻게 동작할까?

@Autowired 는 BeanPostProcessor 의 구현체인 ```AutowiredAnnotationBeanPostProcessor```에 의해 의존성 주입이 이루어진다.
```AutowiredAnnotationBeanPostProcessor```가 bean 초기화 라이프 사이클 이전에 @Autowired가 붙어 있는 bean을 찾아 주입해주는 작업을 한다.

1. BeanFactory(ApplicationContext)가 BeanPostProcessor 타입의 Bean을 찾는다.
2. IoC 컨테이너에 등록되어있는 다른 일반적은 Bean에게 BeanPostProcessor를 적용한다.
3. 다른 Bean에 @Autowored Annotation을 처리하는 ```AutowiredAnnotationBeanPostProcessor```의 로직이 적용된다.
4. 의존성 주입이 일어난다.


### ref
 - [스프링 공식 문서 - BeanPostProcessor](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/config/BeanPostProcessor.html)
 - [스프링 공식 문서 - AutowiredAnnotationBeanProcessor](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/AutowiredAnnotationBeanPostProcessor.html)