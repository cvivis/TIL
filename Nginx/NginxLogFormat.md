# 공식문서 사이트

[Nginx 공식 문서 - 로그 설정](https://nginx.org/en/docs/http/ngx_http_log_module.html#example)

# 기본설정

```java
log_format  main    '$remote_addr - $remote_user [$time_local] '
                    '"$request" $status $body_bytes_sent '
                    '"$http_referer" "$http_user_agent" "$request_time"';
access_log  /var/log/nginx/access.log  main;
```

```java
log_format main '$remote_addr $remote_user $time_local '
		        '$request $status $body_bytes_sent '
		        '$http_referer $http_user_agent '
		        '$request_time $upstream_response_time';
access_log  /var/log/nginx/access.log  main;
```

## $remote_addr

> 클라이언트의 ip

## $remote_user

> 인증된 사용자의 경우 사용자 이름이 표기됨 없으면 빈칸

## $time_local

> 서버가 요청을 처리한 지역 시간

## $request

> 클라이언트가 보낸 HTTP 요청을 나타냅니다. 요청 메서드 (GET, POST, 등)와 요청된 리소스의 경로가 여기에 포함

## $http_referer

> HTTP 요청 헤더의 일부로 페이지에서 페이지로 이동할 때 그 전 페이지의 URL 정보

## $**body_bytes_sent**

> 클라이언트에게 전송된 응답 바이트

## $request_time

> 클라이언트가 그 요청을 보내고 nginx가 응답을 보내기까지의 시간

- 클라이언트 → nginx
- nginx → 서버
- 서버 → nginx
- nginx가 다시 응답을 보내기까지의 시간의 합산

## $upstream_response_time

> nginx가 서버에 요청을 보내고 응답을 받기까지의 시간
