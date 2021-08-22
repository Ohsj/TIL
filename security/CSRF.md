# CSRF공격 이란?

CSRF(Cross Site Request Forgery)는 웹사이트 취약점 공격의 하나로, 사용자가 자신의 의지와는 무관하게 공격자가 의도한 행위(수정, 삭제, 등록)를
특정 웹사이트에 요청하게 하는 공격을 말한다.
CSRF는 사용자의 웹 브라우저를 신용하는 상태를 노린 것이다. 일단 사용자가 웹사이트에 로그인한 상태에서 사이트간 요청 위조 공격 코드가 삽입된 페이지를 열면,
공격 대상이 되는 웹사이트는 위조된 공격 명령이 믿을 수 있는 사용자로부터 발송된 것으로 판단하게 되어 공격 받게 된다.

### 방어 방법
- Referrer 검증
    - request header에 있는 요청을 한 페이지의 정보가 담긴 referrer 속성을 검증하여 차단하며 일반적으로 이 방법만으로도 대부분 방어가 가능할 수 있다.

- CSRF Token 사용
    - 랜덤한 수를 사용자의 세션에 저장하여 사용자의 모든 요청(Request)에 대하여 서버단에 검증하는 방법.
    
- CAPTCHA 사용
    - 캡차 이미지 상의 숫자/문자가 아니라면 해당 요청을 거부하는 것이다.

### Spring Security CSRF Token

CSRF방어 기능은 Spring Security 3.2.0 이후부터 지원되며
CSRF Token은 서버에 들어온 요청이 실제 서버에서 허용한 요청이 맞는지 확인하기 위한 토큰이다.
서버에서는 뷰 페이지를 발행할 때 랜덤으로 생성된 Token을 같이 준 뒤 사용자 세션에 저장해둔다.
그리고 사용자가 서버에 작업을 요청할 때 페이지에 Hidden으로 숨어있는 Token 값이 서버로 전송되는데,
서버에서는 이 Token 값이 세션에 저장된 값고 일치하는지 확인하여 해당 요청이 위조된게 아니라는 것을 확인한다.
일치 여부를 확인한 Token은 바로 폐기하고 새로운 뷰 페이지를 발행할 때마다 새로 생성한다.

### ref
- [wiki CSRF](https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9D%B4%ED%8A%B8_%EA%B0%84_%EC%9A%94%EC%B2%AD_%EC%9C%84%EC%A1%B0)
- [CSRF(Cross Site Request Forgery)란 무엇인가?](https://sj602.github.io/2018/07/14/what-is-CSRF/)
- [Spring Security_CSRF Token의 개념과 사용 방법](https://codevang.tistory.com/282)