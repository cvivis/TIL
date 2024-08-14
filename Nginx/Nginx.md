# Nginx Explained in 100 Seconds

https://www.youtube.com/watch?v=JKxlsvZXG7c

- Nginx는 인터넷과 백엔드 인프라 사이에 게이트웨이와 같은 역할을 한다.
- 웹 서버에 리퀘스트를 요청 후 요청된 자원을 확인 후 응답으로 해당 자원을 다시보내는데 이때 응답의 서버 헤더에서 Nginx를 볼 수 있다.
- Nginx는 이벤트 기반 아키텍처를 통해 1만 개 이상의 동시 연결을 처리 할 수 있어 고트래픽을 처리 할 수 있다.
- Nginx는 트래픽을 분해 하는 리버스 프록시를 일반적으로 사용하며 동시에 보안과 성능을 위한 캐싱을 제공한다.
- Nginx는 주로 Linux 서버에 설치되며, 구성 파일은 etc 디렉토리에서 찾을 수 있다.
- Nginx 서버의 동작을 정의 하려면 `디렉티브`(기본 구조)를 정의해야하며 디렉티브는 키-값의 쌍이거나 중괄호를 사용한 컨텍스트로 정의할 수 있고 컨텍스트안에 디렉티브가 포함 될 수 있다.

```jsx
http {
	location / {
		root /app/www;
	}
}
```

- 전역 컨텍스트에서 사용자 이름 , 오류 로그 저장 위치가 지정되며 주로 HTTP 컨텍스트에서 대부분 구성한다.
- HTTP 컨텍스트에서 하나 이상의 서버를 정의 가능하며 각 서버는 포트를 통해 구별된다.
- 이미지및 HTML 과 같은 정적 콘텐츠 제공가능하다.
- Nginx는 서버에 원본 파일을 어디에서 찾을 것인지 location컨텍스트에서 수행한다.
- 사용자가 도메인에 접속 할 때 HTML 파일을 location으로 지정 후 , 이미지를 처리하기 위한 두 번째 location을 설정할 수 있다.
- 서버 구성에서 SSL인증서, 리다이렉션, 프록시 서버로의 경로 설정을 할 수 있다.
- Root를 proxy_pass로 대체하면 아예 다른 인터넷 서버를 가리 킬 수 있는 리버스 프록시를 설정할 수 있으며 이를 통해 캐싱, 익명성 보장, 로드 밸런싱을 다룰 수 있다.

# Nginx Store MSA 트레이닝 센터

[Nginx Conf 설정 Context Logic 배우기](../Nginx/NginxStoreMSA/NginxConf설정.md)

[Nginx를 Reverse Proxy로 설정하기](../Nginx/NginxStoreMSA/Nginx를ReverseProxy로설정.md)

~~[Nginx를 Load Balancer로 설정하기]()~~

~~[NGINX를 API Gateway로 배포하기]()~~
