![](https://velog.velcdn.com/images/blooper20/post/4e590ef3-900f-4818-b90c-9fdf3f5d697b/image.png)
iOS 앱 개발을 하면서 코드를 보다보면 자연스럽게 접하여 사용하게 되는 lazy 변수에 대해 알아보자

---

# lazy

```
선언된 프로퍼티가 사용되기 전까지 초기값이 계산되지 않는 프로퍼티
```

> ### 💡 lazy 프로퍼티를 이용할 때엔 반드시 변수로 선언해야 한다!
>
> 상수 프로퍼티는 초기화가 끝나기 전까지 반드시 값을 가져야한다.
> 하지만 lazy 프로퍼티의 초기값은 인스턴스 초기화가 완료된 시점까지 알 수 없기 때문에 변수로 선언해야 한다.

> #### lazy 프로퍼티 예제

```
class Test {
	var sometxt = "start lazy"
}
class LazyTest {
	lazy var test = Test()
    var notLazy = [String]()
}
let tester = LazyTest()
tester.notLazy.append("not use lazy var") 							<(1)>
tester.notLazy.append("not use lazy var too")						<(2)>
print(tester.test.sometxt)											<(3)>
```

( 1 ), ( 2 )에선 아직 lazy 프로퍼티가 사용되지않아 test 변수의 값은 nil이 된다.
( 3 )에서 lazy가 붙어있는 test 프로퍼티가 처음 사용될 때 Test의 인스턴스가 생성되고 메모리에 올라가게 된다.

---

## 고려 사항

1. lazy 프로퍼티를 이용할 때엔 반드시 변수로 선언해야 한다.

2. struct와 class에서만 사용 가능하다.

3. 연산 프로퍼티에(Computed Property) 사용될 수 없다.

   > lazy 프로퍼티는 처음 사용될 때 메모리에 값을 올리고, 그 이후엔 메모리에 올라온 값을 계속 사용한다. 때문에 값을 연산하는 연산 프로퍼티에 사용할 수 없다.

4. lazy 프로퍼티에 특별한 연산을 통해 값을 넣어주기 위해선 클로저를 사용한다.
   > 기본적으로는 일반 변수는 클래스가 생성된 이후에 접근이 가능하기 때문에 클래스 내의 다른 메서드나 프로퍼티에 self를 통한 접근이 불가능하다.
   > 하지만 lazy를 이용하면 생성 후 추후에 접근이라는 의미이기 때문에 클로저 내에서 self로 접근이 가능하다.

---

## 왜 사용할까?

```
메모리를 효율적으로 관리할 수 있다.
```

앱이 복잡할수록 생성된 프로퍼티와 인스턴스가 많은데 이 모든걸 한번에 메모리에 올리면 메모리가 과부하가 와서 앱이 종료될 위험이 있다.
하지만 인스턴스가 생성될 때 생성하여 메모리에 올린다면 효율적으로 관리할 수 있다.

---

# 정리

```
lazy가 붙은 변수는 사용되기 전까지 초기값이 계산되지 않다가 사용될 때 생성하여 메모리에 올린다.
```

# 참고한 블로그 링크

- https://declan.tistory.com/m/53
- https://bbiguduk.gitbook.io/swift/language-guide-1/properties
- https://velog.io/@minji0801/Swift-%EA%B8%B0%EC%B4%88-%EB%AC%B8%EB%B2%95-lazy-%ED%82%A4%EC%9B%8C%EB%93%9C
- https://hyunndyblog.tistory.com/155
