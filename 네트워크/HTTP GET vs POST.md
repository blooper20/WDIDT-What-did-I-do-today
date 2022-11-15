# GET

```
 주로 읽거나(Read) 검색(Retrieve) 할 때에 사용되는 메소드
```

- GET 요청이 성공적으로 이루어진다면 XML이나 JSON과 함께 200 HTTP 응답 코드를 리턴
- 에러가 발생하면 주로 404(Not Found) 에러나 400(Bad Request) 에러가 발생
- HTTP 명세에 의하면 GET 요청은 오로지 데이터를 읽을 때만 사용되고 수정할 때는 사용하지 않는다.
- GET 요청은 idempotent하다.(같은 요청을 여러 번 하더라도 변함없이 항상 같은 응답을 받을 수 있다.)

# POST

```
주로 새로운 리소스를 생성(Create)할 때 사용
```

- POST는 하위 리소스(부모 리소스의 하위 리소스)들을 생성하는데 사용
- 성공적으로 creation을 완료하면 201(Created) HTTP 응답을 반환

# GET vs POST

- POST 요청은 클라이언트에서 서버로 전송할 때 추가적인 데이터를 body에 포함할 수 있다
- GET 요청은 모든 필요한 데이터를 URL에 포함하여 요청한다.

> ### HTML의 <form\>태그에 method="POST" 또는 method="GET"(기본값)을 모두 사용할 수 있다.
>
> 만약 GET 메소드를 사용하면 모든 form data는 URl로 인코딩되어 action URL에 query string parameters로 전달된다.
> POST 메소드를 사용하면 form data는 HTTP request의 messagebody에 나타날 것이다.

# [참고한 블로그 링크](https://im-developer.tistory.com/166)
