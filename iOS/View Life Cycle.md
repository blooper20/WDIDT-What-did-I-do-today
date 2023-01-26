![](https://velog.velcdn.com/images/blooper20/post/4f7b8741-164e-4617-b482-f223dbc1428b/image.png)

# View Life Cycle 이란?

```
View Controller의 생명 주기
```

![](https://velog.velcdn.com/images/blooper20/post/8351868c-776e-4645-a554-51422422bb1e/image.png)
앱은 여러개의 View Controller로 이루어져 각 View Controller가 생겼다가 사라지는 주기에 대해 정의해 놓은 것이 View Life Cycle이다.

이 주기는 여러 단계를 거쳐 뷰 컨트롤러가 생기고 사라진다.
또한, 메소드의 이름이 직관적이라 함수명으로 기능을 유추하기에도 쉽다.

## 1. loadView

```
컨트롤러가 관리하는 뷰를 만드는 역할
```

loadView가 뷰를 만들고, 메모리에 올린 후 viewDidLoad() 호출한다.
또한, 이 함수는 직접 호출 할 수 없다.

---

## 2. viewDidLoad

```
일반적으로 리소스를 초기화하거나 초기 화면을 구성하는 용도로 사용
```

위에서 말했듯이 뷰의 컨트롤러가 메모리에 로드 된 후에 호출이 된다.

또한, 화면이 처음 만들어 질때 **'한 번'**만 실행되기 때문에 처음 한 번만 실행해야 하는 초기화 코드가 있을 경우에 viewDidLoad() 내부에 작성하면 된다.

---

## 3. viewWillAppear

```
뷰가 로드 된 이후 뷰가 나타날거라는 신호를 컨트롤러에게 알리는 역할
```

즉 뷰가 나타나기 직전에 호출

> ### viewDidLoad와 viewWillAppear의 차이점
>
> 네비게이션 컨트롤러를 이용하여 HomeVC 에서 FirstVC로 갔다가 다시 HomeVC로 되돌아온다.
> 이 때 처음 HomeVC가 화면에 나타날 때 당연하게 viewDidLoad가 먼저 실행이 되고난 후에 viewWillAppear가 실행이 된다.

```
HomeVC
1. viewDidLoad
2. viewWillAppear
```

---

이 후, FirstVC로 화면을 옮기면 HomeVC와 똑같이 viewDidLoad가 먼저 실행이 되고난 후에 viewWillAppear가 실행이 된다.

```
HomeVC -> FirstVC
1. viewDidLoad
2. viewWillAppear
```

---

하지만, 여기서 네비게이션 컨트롤러를 이용하여 HomeVC로 되돌아간다고 했을땐 viewDidLoad는 실행이 되지않고 viewWillAppear가 실행이 된다.

```
FirstVC -> HomeVC
1. viewWillAppear
```

---

네비게이션 컨트롤러는 스택구조와 같이 뷰 컨트롤러가 push하여 쌓이는 방식이다.
이 때문에 HomeVC는 처음 viewDidLoad가 실행된 후에 FirstVC가 실행되어도 메모리에 남아있어 다시 HomeVC에 돌아와도 viewDidLoad를 할 필요가 없는 것이다.

---

하지만, 여기서 만약 다시 FirstVC로 간다면 이미 FirstVC에 대한 메모리는 pop을 하여 다시 viewDidLoad가 실행된다.

```
HomeVC -> FirstVC
1. viewDidLoad
2. viewWillAppear
```

---

### 레이아웃은 언제 구성 될까?

```
뷰가 생성되고 화면상에 보이기 전에 레이아웃이 구성된다
```

![](https://velog.velcdn.com/images/blooper20/post/39b55db8-52a7-487b-a456-91cdfb4c4283/image.png)

또한, 레이아웃도 3단계의 과정을 거쳐 구성된다.

1. Constraint: 뷰들의 Constraint들이 계산되고 설정

#### updateViewConstraints

```
Constraints를 업데이트
```

---

2. Layout: 뷰들의 Frame 그리고 그 자식 뷰들의 Frame이 배치

#### viewWillLayoutSubviews

```
레이아웃이 설정되기 전에 호출
```

#### viewDidLayoutSubviews

```
레이아웃이 다 설정된 이후에 호출
```

---

3. Draw(Display): layout과 다른 뷰들의 특성(텍스트, 크기, 색 등)들이 적용

---

## 4. viewDidAppear

```
뷰가 나타났다는 것을 컨트롤러에게 알려주는 역할
```

또한 화면에 적용될 애니메이션을 그려준다.

이 함수는 뷰가 화면에 나타난 직후에 실행된다는 점 말고는 viewWillAppear와 거의 같다.

---

## 5. viewWillDisappear

```
뷰가 삭제되려고 하고있는 것을 뷰 컨트롤러에 알려주는 역할
```

뷰가 사라지기 직전에 호출되는 함수이다.

> ### 주로 하는 작업

- 데이터를 잃지 않도록 저장
- 네트워크 작업 취소

---

## 6. viewDidDisappear

```
뷰 컨트롤러가 뷰가 제거되었음을 알려준다.
```

뷰 컨트롤러의 컨텐츠 뷰가 앱의 뷰 계층에 제거된 직후에 호출된다.

> ### 주로 하는 작업

- 추가적으로 액티비티를 해체하는 작업을 수행할 때

---

# 참고한 블로그 링크

- https://zeddios.tistory.com/43
- https://beingdesigner.tistory.com/59
- https://wlgusdn700.tistory.com/109
- https://baechukim.tistory.com/27
