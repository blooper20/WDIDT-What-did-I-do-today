![](https://velog.velcdn.com/images/blooper20/post/2dcde6ca-e1bd-44f8-9798-6527f8805c14/image.png)

# Composite Index(결합 인덱스)

```
인덱스를 생성할 때 두개 이상의 컬럼을 합쳐서 인덱스를 만드는 것
```

용도: 주로 SQL 문장에서 WHERE절의 조건 컬럼이 2개 이상 AND로 연결되어 함께 사용되는 경우에 많이 사용

> 예제)
> ** EMP 테이블에 인원 100명이 있는데, 그 중에서 남자 50명, 여자 50명이 있다.
> 남자 중에 이름이 'SMITH'인 사람이 단 2명이 있다.
> 성별이 남자 중에서 이름이 SMITH인 사람을 찾아라
> **

---

### 쿼리문

```
SELECT ename, sal
FROM emp
WHERE ename = 'SMITH'
AND sex = 'M'
;
```

### 결합 인덱스 생성 구문

```
CREATE INDEX idx_emp_comp
ON emp( ename, sex);
```

---

결합 인덱스는 AND 조건으로 검색되는 경우 성능에 아주 중요한 역할을 한다.
두개 이상의 조건이 OR로 조회되는 경우는 결합 인덱스를 만들면 안된다.
[참고한 블로그 링크](https://jhkang-tech.tistory.com/210)

---

# Composite Key

```
두 개 이상의 컬럼을 Key로 지정하는 것
```

기본키는 한 테이블에 한개만 존재할 수 있다.
하지만 꼭 한 테이블에 한 컬럼만 기본키로 지정할 수 있는 것은 아니다.

> 예제)
> ** 기본 키(Primary Key)를 두 컬럼에 지정 했을 경우**

```
create table two (
id bigint primary key,
seq bigint primary key);
```

\> ERROR 1068 (42000): Multiple primary key defined

---

** Composite Key를 활용 했을 경우**

```
create table composite (
id bigint not null,
seq bigint not null,
primary key(id, seq)
);
```

\> Query OK, 0 rows affected (0.01 sec)

---

이렇게 컬럼을 조합하여 기본키로 설정할 경우에는 여러 컬럼을 모두 조합해서 UNIQUE해야 한다.
[참고한 블로그 링크](https://gaemi606.tistory.com/entry/Composite-Key-%EB%B3%B5%ED%95%A9-%ED%82%A4)
