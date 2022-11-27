![](https://velog.velcdn.com/images/blooper20/post/b36f1946-d253-4403-bf42-f3c3180bc8d6/image.png)

# REST란?

```
Representational State Transfer의 약자로 자원을 이름으로 구분하여 해당 자원의 상태를 주고받는 모든 것을 의미
```

- HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시
- HTTP Method(POST, GET, PUT, DELETE, PATCH 등)를 통해 해당 자원(URI)에 대한 CRUD Operation을 적용하는 것을 의미

> # CRUD Operation이란?
>
> CRUD는 대부분의 컴퓨터 소프트웨어가 가지는 기본적인 데이터 처리 기능인 Create(생성), Read(읽기), Update(갱신), Delete(삭제)를 묶어서 일컫는 말

```
Create : 데이터 생성(POST)
Read : 데이터 조회(GET)
Update : 데이터 수정(PUT, PATCH)
Delete : 데이터 삭제(DELETE)
```

## 구성요소

1. 자원: HTTP URI
2. 자원에 대한 행위: HTTP Method
3. 자원에 대한 행위의 내용: HTTP Message Pay Load

## 특징

- Server-Client
- Stateless
- Cacheable
- Layered System
- Unniform Interface

## 장단점

### 장점

- HTTP 프로토콜의 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구출할 필요가 없음
- HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있게 해 줌
- HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능
- Hypermedia API의 기본을 충실히 지키면서 범용성을 보장
- REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악
- 여러 가지 서비스 디자인에서 생길 수 있는 문제를 최소화
- 서버와 클라이언트의 역할을 명확하게 분리

### 단점

- 표준이 자체가 존재하지 않아 정의가 필요
- HTTP Method 형태가 제한적
- 브라우저를 통해 테스트할 일이 많은 서비스라면 쉽게 고칠 수 있는 URL보다 Header 정보의 값을 처리해야 하므로 전문성이 요구
- 구형 브라우저에서 호환이 되지 않아 지원해주지 못하는 동작이 많음(익스폴로어)

# REST API란?

```
RESPT API란 REST의 원리를 따르는 API
```

## REST API 설계 예시

1. URI는 동사보다는 명사를, 대문자보다는 소문자를 사용

```
Bad Example http://khj93.com/Running/
Good Example  http://khj93.com/run/
```

2. 마지막에 슬래시 (/)를 포함하지 않음

```
Bad Example http://khj93.com/test/
Good Example  http://khj93.com/test
```

3. 언더바 대신 하이폰을 사용

```
Bad Example http://khj93.com/test_blog
Good Example  http://khj93.com/test-blog
```

4. 파일확장자는 URI에 포함하지 않음

```
Bad Example http://khj93.com/photo.jpg
Good Example  http://khj93.com/photo
```

5. 행위를 포함하지 않음

```
Bad Example http://khj93.com/delete-post/1
Good Example  http://khj93.com/post/1
```

# RESTful이란?

```
RESTFUL이란 REST의 원리를 따르는 시스템을 의미
```

하지만 REST를 사용했다 하여 모두가 RESTful 한 것은 아니다.
REST API의 설계 규칙을 올바르게 지킨 시스템을 RESTful하다 말할 수 있으며, 모든 CRUD 기능을 POST로 처리 하는 API 혹은 URI 규칙을 올바르게 지키지 않은 API는 REST API의 설계 규칙을 올바르게 지키지 못한 시스템은 REST API를 사용하였지만 RESTful 하지 못한 시스템이라고 할 수 있습니다.

# [참고한 블로그 링크](https://khj93.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80)
