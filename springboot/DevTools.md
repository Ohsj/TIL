# DevTools 란?

devtools(Developer Tools)는 스프링 부트에서 제공하는 개발 편의를 위한 모듈이다.
주로 변경된 코드를 서버 또는 화면에 신속하게 반영해 결과를 확인하기 위해서 사용한다.

### 제공하는 기능
1. Property Defaults
2. Automatic Restart
3. Live Reload
4. Global settings
5. Remote Applications

이와 같이 여러 기능들이 있지만 자주 사용하는 기능들에 대해 정리해보려고 한다.

### Property Defaults

Spring에서  제공하는 ```thymeleaf```는 기본적으로 캐싱 기능을 사용한다. 하지만 개발과정에서 캐싱이 되어있다면,
우리가 ```thymeleaf```를 수정해도, 반영되지 않는다. 그래서 따로 ```application.properties```나 ```application.yml```에 ```application cache``` 설정을 false로 줘야한다.
하지만 ```devtools```는 이런 작업을 제공해준다. 개발 시점과 배포 시점에 다른 설정을 기본적으로 개발 단계에 맞춰 설정해준다.

### Automatic Restart

Spring Boot Application 을 개발하다 보면 Application을 재시작 해야하는 경우가 많이 발생한다.
클래스 필드하나와 같이 간단한 것들을 수정하고 재시작 하는 번거로움이 생긴다. 
그래서 devtools에서는 파일 수정 후 저장을 하면 Classpath에 존재하는 파일의 변경을 감지하고, 자동으로 서버를 restart 해준다. 설정을 통해 원하는 디렉터리만을 트리거로 설정할 수도 있다.

### Live Reload

devtools에서는 파일을 수정하면 내부적으로 live reload 서버를 두고, 브라우저 확장프로그램과 통신하는 방식으로 동작한다. [LiveReload](https://chrome.google.com/webstore/detail/livereload/jnihajbhpnppcggbcgedagnkighmdlei?hl=ko) 링크를 통해 확장프로그램을 설치하고, enable 해주면 된다.


### ref
 - [Spring Boot Devtools 알아보기](https://velog.io/@bread_dd/Spring-Boot-Devtools)