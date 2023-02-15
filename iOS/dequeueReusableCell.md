![](https://velog.velcdn.com/images/blooper20/post/bf8da1d2-7a93-4770-b068-beace0eab7aa/image.png)

# dequeueReusableCell(withIdentifier:)

TableView를 사용하다 보면 자연스럽게 접하고 사용하게 되는 코드가 있다.

```
dequeueReusableCell(withIdentifier:)
```

보통 cell을 생성할 때 이 메서드를 사용하여 셀을 초기화 시켜 테이블을 완성해준다.

이 때 셀을 구성하는 요소 중 nil 값이 하나 있다면 테이블엔 아무 내용이 표시가 안되는게 당연하다.

```
let cell = [0, 1, 2, 3, 4, 5, nil, 7]
```

위 배열을 테이블의 셀에 순차적으로 넣는다고 가정하면 화면은 밑에처럼 나올것이다.

```
---------테이블뷰 시작-----------
0
1
2
3
4
5
ㅤ
7
---------테이블뷰 끝------------
```

여기서 배열의 크기를 더욱 늘려보자.

```
let cell = [0, 1, 2, 3, 4, 5, nil, 7, 8, 9, 10, 11, nil, 13, 14, 15]
```

이 상태에서 위와 같이 테이블을 출력해 보면 똑같이 생각한 대로 순차적으로 나올 것이다.
하지만 이때 위아래로 테이블을 움직이다 보면 아무 값도 없는 nil 대신 다른 요소가 채워주고 있음을 확인할 수 있다.

## 어떤 이유일까?

이 현상은 위에서 소개했던 `dequeueReusableCell(withIdentifier:)` 메서드를 사용하여 생긴 일이다.
테이블뷰, 컬렉션뷰는 모두 셀을 재사용(Reuse)한다.
또한 이런 셀의 재사용 구조는 Queue로 되어있다.

테이블뷰에서 위로 스크롤 되어 화면 밖으로 사라지는 셀들은 큐로 들어가게 되고 큐의 앞에 있는 셀이 화면의 하단에서 올라와 화면에 나타나지는 셀로 사용된다.
이렇게 dequeue, reuse 되는 과정이 흔히 사용했던 `cellForRowAt`의 delegate 메서드에서 `dequeueReusableCell(withIdentifier:)` 함수를 통해 이루어졌었던 것이다.
그래서 셀에 configure 되는 데이터 소스의 내용은 다르지만 셀 자체는 그대로 재사용 되기 때문에 앞에 남아있던 정보가 nil을 대신하여 채우고 있는 것이다.

![](https://velog.velcdn.com/images/blooper20/post/95234426-51af-45e6-af15-de5ef91571f5/image.png)
위에 사진을 보면 무슨 뜻인지 이해가 더 쉬울 것이다.

## 해결 방법

위에서 발생하는 문제는 셀의 속성들을 초기화 시켜주지 않은 상태로 재사용하기 때문에 발생한다.
그렇기 때문에 재사용되는 셀의 속성을 초기화해주면 간단히 해결할 수 있다.

테이블뷰 셀은 이를 더욱 간단하게 초기화 시켜줄 수 있는 `prepareForReuse` 함수를 제공해 준다.

```
// tableViewCell.swift
override func prepareForReuse() {
    super.prepareForReuse()
    // 셀을 초기화 해주는 코드 작성
}
```

여기서 셀의 뷰 측면의 속성들을 초기화 시켜주면 된다.
![](https://velog.velcdn.com/images/blooper20/post/c8786f7b-6daf-4eb0-a5c4-89ee13de6a65/image.png)
위 사진과 같이 보면 더욱 이해가 쉬울 것이다.

# 참고한 블로그 링크

- https://sihyungyou.github.io/iOS-dequeueReusableCell/
- https://gyuios.tistory.com/72

> #### 수정할 내용, 추가할 내용, 더 좋은 레퍼런스들은 댓글에 남겨주시면 감사하겠습니다.
