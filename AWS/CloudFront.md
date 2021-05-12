# CloudFront 란?

```.html```, ```.css```, ```.js``` 및 ```이미지 파일```과 같은 정적 및 동적 웹 콘텐츠를 사용자에게 더 빨리 배포하도록 지원하는 ```CDN``` 웹 서비스이다.
```AWS Cloud Front```는 ```Edge Location```이라고 하는 데이터 센터의 전 세계 네트워크를 통해 콘텐츠를 제공한다. 
```Cloud Front```를 통해 서비스하는 콘텐츠를 사용자가 요청하면 지연 시간이 가장 낮은 ```Edge Location```으로 요청이 라우팅되므로 가능한 최고의 성능으로 콘텐츠가 제공된다.

- 콘텐츠가 이미 지연 시간이 가장 낮은 ```Edge Location```에 있는 경우 ```Cloud Front```가 콘텐츠를 즉시 제공.(Cache)
- 콘텐츠가 ```Edge Location```에 없는 경우 ```Cloud Front```는 콘텐츠의 최종 버전에 대한 소스로 지정된 Origin(```S3```, ```MediaPackage 채널```, ```HTTP 서버(예: 웹서버)```)에서 콘텐츠를 검색.

### CDN

Content Delivery Network의 약자인 ```CDN```은 지리적 제약 없이 전 세계 사용자에게 빠르고 안전하게 콘텐츠를 전송할 수 있는 콘텐츠 전송 기술을 의미하며
물리적 거리를 줄여 웹 페이지 콘텐츠 로드 지연을 최소화화는, 촘촘히 분산된 서버로 이루어진 플랫폼이다.

사용자가 원격지에 있는 ```Origin Server```로 부터 컨텐츠를 다운 받는 것 보다 가까이에 있는 서버로 부터 다운받는게 빠르다.
따라서 ```Origin Server```를 대신하여 사용자와 물리적으로 가까운 곳에 ```Pop(Points of Persence)```를 둔다.
```Pop```에는 자체 캐싱 서버가 포함되어 있으며 사용자에게 캐시된 콘텐츠를 제공한다.
```CDN```은 ```Pop```에 캐시서버가 있기 때문에 지연시간이 오래걸리지 않는 이점이 있고
```Origin Server```를 대신하여 ```Pop```에서 사용자의 요청에 응답함으로써 ```Origin Server```의 트래픽 부하를 줄이는 부하 분산의 효과도 가지고 있다.

### Signed URL & Signed Cookies

```Cloud Front```를 통해 특정 걍로, 인증을 통해서만 private한 데이터에 액세스할 수 있도록 권한을 제어할 수 있으며 2가지 방법이 많이 쓰인다.

1. Signed URL
    - RTMP(Real Time Message Protocol)을 지원
    - 개별 파일마다 제한 조건을 둘 수 있다.
    
2. Signed Cookies
    - RTMP(Real Time Message Protocol)을 지원하지 않는다.
    - 여러 파일들을 한꺼번에 제한 조건을 걸 수 있다.
    - 현재 제공되고 있는 URL을 변경하지 않아도 된다.
    
### Origin Access Identity (OAI)

```Amazon S3``` 버킷을 ```Cloud Front``` 배포의 오리진으로 처음 설정하면 모든 사용자에게 버킷의 파일에 대한 권한을 부여하게 된다.
이렇게 되면 누구나 ```Cloud Front``` URL 혹은 ```S3``` URL 두 경로 모두로 파일에 접근할 수 있게 된다.

이런 상황에서, ```Cloud Front```의 ```Signed URL``` 또는 ```Signed Cookies```를 사용하여 ```Amazon S3``` 버킷의 파일에 대한 액세스를 제한하는 경우에는
```Amazon S3``` URL로 ```Amazon S3``` 파일에 직접 액세스하는 사용자도 차단하는 것이 좋다. 서명된 URL, 서명된 쿠키와 관계 없이 S3 URL을 가지고 있다면 해당 파일에 바로 접근할 수 있기 때문입니다.

따라서 OAI라는 것을 만들어 다음과 같은 작업을 수행해야 한다.
- 특별한 ```Cloud Front``` 사용자인 OAI를 만들고 ```Cloud Front``` 배포와 연결한다.
- OAI만 읽기 권한을 가지도록 ```Amazon S3``` 버킷 권한을 변경한다. -> ```S3``` URL로 파일을 읽을 수 없게 된다.

### ref
 - [Amazon CloudFonrt란 무엇입니까?](https://docs.aws.amazon.com/ko_kr/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)
 - [CDN이란 무엇입니까?](https://www.akamai.com/kr/ko/cdn/what-is-a-cdn.jsp)
 - [함께 자라기](https://wbluke.tistory.com/57)