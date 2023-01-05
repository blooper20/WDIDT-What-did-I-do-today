# SQL Injection

![](https://velog.velcdn.com/images/blooper20/post/dfc9cb44-38d6-4715-8c88-cf99a2436600/image.png)

```
보안상의 취약점을 이용해 임의의 SQL문을 주입하고 실행되게 하여
데이터베이스가 비정상적인 동작을 하도록 조작하는 행위
```

> 공격이 비교적 쉽고 공격에 성공할 경우 큰 피해를 입힐 수 있는 공격

---

## 종류

### Normal SQL Injection

```
논리적 취약점을 이용한 SQL 인젝션
```

>

```
select * from client where name = 'sss' and password='1111'
->
select * from client where name = 'sss' and password='1'='1'or'1'='1'
```

- or은 앞,뒤 중 하나만 참이어도 참이다.
- '1'='1'이 참이므로 위 구문은 참이 되어 로그인에 성공한다.

---

### Error based SQL Injection

```
에러를 이용한 SQL 인젝션
```

에러 노출을 기준으로 진행된다.

---

### UNION based SQL Injection

```
UNION 명령어를 이용한 SQL 인젝션
```

> SQL UNION이란?
> 여러개의 SQL문을 합쳐 하나의 SQL문으로 만들어주는 방법

```
select name from classa
union
select name from classb;
```

- 클래스 A와 클래스 B의 이름들이 합쳐져서 출력(중복 제외)

>

```
[외부입력]
ID: test' UNION SELECT 1,1 --
PW: anything
[실행쿼리]
SELECT * FROM users 테이블에 등록된 id와 pw 목록을 전부 조회할 수 있게 된다.
```

실행 쿼리대로 하면 users 테이블에 등록된 id와 pw 목록을 전부 조회 가능하다.

---

### Blind SQL Injection

```
쿼리가 참, 거짓일 때 서버의 반응만으로 데이터를 얻어낼 수 있는 공격기법
```

#### Bollean based Blind SQL Injection

```
평범한 SQL 삽입과 같이 원하는 데이터를 가져올 삽입하는 기술
```

웹에서 SQL 삽입에 취약하나 데이터베이스 메시지가 공격자에게 보이지 않을 때 사용

> #### 평범한 SQL 삽입과 다른점
>
> 평범한 SQL 삽입은 쿼리를 삽입하여 원하는 데이터를 한번에 얻어낼 수 있는데에 비해
> Blind SQL 삽입은 참과 거짓, 쿼리가 참일 때와 거짓일 때의 서버의 반응 만으로 데이터를 얻어내는 기술이다.
> 즉, 쿼리를 삽입했을 때에 쿼리의 참과 거짓에 대한 반응을 구분할 수 있을 때에 사용된다.

---

#### Time based SQL Injection

```
응답의 결과가 항상 닽아 해당 결과만으론 참, 거짓을 판별할 수 없을 때에
시간을 지연시키는 쿼리를 주입하여 응답시간의 차이로 참, 거짓 여부를 판별하는 기술
```

---

## 대응방안

> ### MySQL
>
> mysqli_real_escape_string(); 함수를 사용
> SQL에서 특별한 의미를 갖는 문자들을 이스케이프해서 SQL 삽입을 방지하는 방법

> ### PHP
>
> prepare 함수를 사용
> 사용자의 입력이 쿼리문으로부터 분리되어 SQL 삽입을 효과적으로 방어

> ### 이외
>
> 웹 방화벽 도입, 시큐어 코딩, 취약점 점검, 모니터링 등 여러 방법이 존재

---

# 참고한 블로그 링크

- https://noirstar.tistory.com/264
- https://m.blog.naver.com/lstarrlodyl/221837243294
