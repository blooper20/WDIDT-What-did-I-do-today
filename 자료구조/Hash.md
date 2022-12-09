# Hash

![](https://velog.velcdn.com/images/blooper20/post/0cbe9a26-7694-4f10-80d3-a3d67675a10a/image.png)

```
데이터를 다루는 기법 중 하나로 검색과 저장이 빠르게 진행
```

> ### 빠르게 진행 될 수 있는 이유

- 데이터를 검색 할 때 사용할 key와 실제 데이터의 값(value)이 한 쌍으로 존재
- key값이 배열의 인덱스로 변환되기 때문에 검색과 저장의 평균적인 시간 복잡도가 O(1)에 수렴

---

## Hash Function

```
임의의 길이의 데이터를 고정된 길이의 데이터로 매핑하는 함수
```

이 때 매핑 전 원래 데이터의 값을 key, 매핑 후 데이터의 값을 hash value, 매핑하는 과정을 hashing이라고 합니다.

- 입력하는 key값이 같으면 항상 같은 value가 출력
- 매우 큰 범위의 입력 데이터를 hash 함수를 이용해서, 고정된 크기의 디지털 데이터에 맵핑하기 위한 목적으로 사용

---

## Hash Table

```
많은 양의 데이터에서 데이터의 위치를 빠르게 찾기위해 사용
```

- hash 함수에 key를 넣으면 value를 리턴하는데 이 value를 인덱스로 해서 key에 대한 값이 저장된 위치를 찾는 식이다.
  ![](https://velog.velcdn.com/images/blooper20/post/46e34a06-e7ff-47a5-92b0-1bd4ba0916f6/image.png)

---

# 참고한 블로그 링크

- https://siyoon210.tistory.com/85
- https://www.joinc.co.kr/w/man/12/hash
- https://medium.com/@su_bak/crypto-%ED%95%B4%EC%8B%9C-hash-%EB%9E%80-6962be197523
