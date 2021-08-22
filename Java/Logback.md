# Logback

2021.04.17 ~ 2021.05.01

---
## Logback 이란?

Logback은 Java에서 가장 많이 사용되었던 로깅 라이브러리 log4j의 후속 버전이며 간단히 더 좋아진 log4j라고 생각된다.
Spring boot에서는 기본 로깅 모듈로 채택하고 있다.

Logback을 이용하여 로깅을 수행하기 위해 필요한 주요 설정 요소로 3가지가 있다.
- Logger : 실제 로깅을 수행하는 구성요소로 Level 속석을 통해서 출력할 로그의 레벨을 조절.
- Appender : 로그 메시지가 출력될 대상을 결정하는 요소
- Encoder : Appender에 포함되어 사용자가 지정한 형식으로 표현 될 로그메시지를 변환하는 역할

## 장점
 - log4j 보다 약 10배 정도 빠르게 수행되도록 내부가 변경되었으며, 메모리 효율성도 좋다.
 - log4j때부터 광범위한 테스트를 진행한 경험을 가지고 있으며, logback은 더욱 높은 레벨의 테스트를 통해 검증되었다.
 - 문서화가 잘 되어 있다.
 - 설정 파일을 변경하였을 경우, 서버 재기동 없이 변경 내용이 자동으로 갱신된다.
 - 서버 중지 없이 I/O Faliure에 대한 복구를 지원한다.
 - RollingFileAppender를 사용할 경우 자동적으로 오래된 로그를 지워주며 Rolling 백업을 처리한다.

## 참고 순서
1. classpath에 ```logback-spring.xml``` 파일이 있다면 설정파일을 읽어간다.
2. ```logback-spring.xml``` 파일이 없다면 ```.yml(.properties)``` 설정을 읽어간다.
3. ```logback-spring.xml``` 파일과 ```.yml(.properties)``` 이 동시에 있다면 ```.yml(.properties)``` 설정 적용 후 ```logback-spring.xml```이 적용된다.

## Logging Level
 1. ERROR : 요청을 처리하는 중 오류가 발생한 경우 표시한다.
 2. WARN : 처리 가능한 문제, 향후 시스템 에러의 원인이 될 수 있는 경고성 메시지를 나타낸다.
 3. INFO : 상태변경과 같은 정보성 로그를 표시한다.
 4. DEBUG : 프로그램을 디버깅하기 위한 정보를 표시한다.
 5. TRACE : 추적 레벨은 Debug보다 훨씬 상세한 정보를 나타낸다.

## ref
- [LOGBACK이란? LOG4J의 후속작 로그백](https://dololak.tistory.com/632)
- [갓대희의 작은공간](https://goddaehee.tistory.com/45)
- [Logback 설정 방법](https://bamdule.tistory.com/29)