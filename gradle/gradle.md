#Gradle 이란?

```Gradle```은 ```Groovy```를 기반으로 한 빌드 도구이며, 빌드 자동화와 여러 언어 개발 지원에 집중한 빌드 툴이다.
```Ant```와 ```Maven```과 같은 이전 세대 빌드 도구의 단점을 보완하고 장점을 취합하여 만든 오픈소스로 공개된 빌드 도구이며,
어떤 플랫폼에서든 소프트웨어를 빌드, 테스트, 게시 및 배포 할 때 ```Gradle```은 코드 컴파일 및 패키징에서 웹사이트 게시에 이르기까지 전체 개발 수명주기를 지원할 수 있는 유연한 모델을 제공한다.
```Gradle은``` ```Java```, ```Scala```, ```Android```, ```Kotlin```, ```C``` / ```C++``` 및 ```Groovy```를 포함한 여러 언어 및 플랫폼에서 빌드 자동화를 지원하도록 설계 되었으며 ```eclipse```, ```Intellij```, ```Jenkins```를 포함한 개발 도구 및 지속적 통합 서버와 통합된다.

## Gradle 특징

Gradle은 ```Ant```와 ```Maven```이 가진 장점을 모아 만들었다. 의존성 관리를 위한 다향한 방법을 제공하고 빌드 스크립트를 XML언어가 아닌 JVM에서 동작하는 스크립트 언어 '그루비' 기반의 DSL(Domain Specific Language)를 사용한다.

Groovy는 자바 문법과 유사하여 자바 개발자가 쉽게 익힐 수 있는 장점이 있으며 ```Gradle Wrapper```를 이용하면 Gradle이 설치되지 않은 시스템에서도 프로젝트를 빌드할 수 있다.
심지어 Maven의 ```pom.xml```을 Gradle 용으로 변환할 수도 있으며 Maven의 중앙 저장소도 지원하기 때문에 라이브러리를 모두 그대로 가져다 사용할 수 있다.

## Gradle 설치

- [Gradle Install](./gradleInstall.md) 문서 참조 

### ref
- [Gradle 공식 GitHub](https://github.com/gradle/gradle)
- [Gradle이란 무엇일까?](https://madplay.github.io/post/what-is-gradle)
- [Gradle이란??](https://velog.io/@hwany/gradle)
- [Gradle: build.gradle vs settings.gradle vs gradle.properties](https://www.baeldung.com/gradle-build-settings-properties)