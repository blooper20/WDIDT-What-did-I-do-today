![](https://velog.velcdn.com/images/blooper20/post/c5349f06-87e7-46aa-b13d-4ca161612dab/image.png)

# 참조 타입과(Reference) 힙(Heap)

ARC를 알아보기 전에 참조타입과 힙에 대해 먼저 알아보자

Swift는 인스턴스, 클로저 등 참조타입은 자동으로 힙에 할당한다.
사용자가 따로 힙에 대한 조작이 필요 없이 자동으로 할당하여 사용

> #### 예)

```
class Man {
	var name: String?
    var height: Int?
    init(name: String?, height: Int?) {
    	self.name = name
        self.height = height
	}
}
let dwaewoo = Man(name: "Dwae Woo", height: 180)
```

**Man 클래스**에 **dwaewoo 인스턴스**를 생성하고 값을 초기화
dwaewoo가 현재 전역 변수로 선언 되었지만 한 클래스 내에 생성된 지역 변수라고 가정을 한다.

---

#### 메모리 저장 방식

```
			[스택 영역] 				[힙 영역]
dwaewoo |	0x11111111	|	 |	Man Instance	|
		|				| -> |	name: "Dwae Woo"|	<- 0x11111111
        |               |	 |	height: 180		|
```

지역 변수 dwaewoo는 스택에 할당
실제 Man 인스턴스는 힙에 할당
스택에 있는 dwaewoo는 힙 영역에 있는 인스턴스를 참조하고 있는 형태이다.
따라서 dwaewoo 안에는 힙에 할당된 인스턴스의 주소값이 들어가 있다.

```
let copy = dwaewoo
```

이 코드를 위에 예제 본문코드에 추가를 하면

```
			[스택 영역] 				[힙 영역]
dwaewoo |	0x11111111	|	 |	Man Instance	|
		|				| 	 |	name: "Dwae Woo"|	<- 0x11111111
        |               |	 |	height: 180		|
copy	|	0x11111111	|	 |					|
```

인스턴스는 복사가 되지 않기 때문에 다음과 같이 실제 메모리는 같은 힙 영역의 인스턴스를 가리키고 있다.

---

힙의 중요한 특징 중 하나는 **'사용하고 난 후엔 반드시 메모리 해제를 해줘야 한다.'** 인데 메모리를 해제하기 위해선 release, free 등의 방법이 있다.
하지만 실제로 개발하면서 한번도 저런 함수를 통해 인스턴스를 직접 메모리에서 해제한 기억이 없다.

---

스택에 있는 dwaewoo, copy가 함수 종료 시점에 사라지고 나면

```
			[스택 영역] 				[힙 영역]
		|				|	 |	Man Instance	|
		|				| 	 |	name: "Dwae Woo"|	<- 0x11111111
        |               |	 |	height: 180		|
		|				|	 |					|
```

힙에 남아 쓰지도 않는 인스턴스는 메모리 해제를 해주지 않아 **메모리 leak**으로 이어져 어플이 날라가기 전까지 아무도 사용하지 못하게 된다.

---

가 아니라 ARC가 해제해준다.

---

# ARC란 무엇인가?

```
ARC: Automatic Reference Counting
```

> [애플 공식문서]![](https://velog.velcdn.com/images/blooper20/post/47eeeae0-8ba5-4026-b997-bf22fe9f2b38/image.png)
> 공식문서에 마지막 밑줄 친 부분을 해석하면

```
ARC는 클래스 인스턴스가 더 이상 필요하지 않을 때 메모리를 자동으로 해제한다.
```

이다.
개발하면서 힙에 메모리를 자동 할당하며 사용해왔지만 손수 메모리를 해제해주지 않아도 됐던 이유가 위에서 말했듯이 **ARC가 메모리를 자동으로 해제**해주기 때문이다.

최종적으로 ARC에 대해 정의를 내리면

```
힙에 할당된 인스턴스의 메모리를 알아서 관리해준다.
```

---

## GC vs RC

힙 영역의 메모리를 관리하는 방법엔 GC와 RC가 있다.
ARC는 RC에 포함되고, GC는 자바에서 사용되는데 이 둘을 비교하겠다.

자바의 GC와 스위프트의 RC의 가장 큰 차이점은 참조를 계산하는 시점이다.
우선 GC는

### GC(Garbage Collection)

- 참조 계산 시점: Run Time (어플 실행동안 주기적으로 참조를 추적하여 사용하지 않는 instance를 해제)
- 장점: 인스턴스가 해제될 확률이 높음 (RC에 비해)
- 단점: 개발자가 참조 해제 시점을 파악할 수 없음, Run Time 시점에 계속 추적하는 추가 리소스가 필요하여 성능저하가 발생될 수 있음

### RC(Reference Counting)

- 참조 계산 시점: Compile Time (컴파일 시점에 언제 참조되고 해제되는 지 결정되어 런타임 때 그대로 실행
- 장점: 개발자가 참조 해제 시점을 파악할 수 있음, Run Time 시점에 추가 리소스가 발생하지 않음
- 단점: 순환 참조가 발생 할 때 영구적으로 메모리가 해제되지 않을 수 있음

---

## MRC(MRR) vs ARC

간단히 따지면 앞에 같은 RC(Reference Counting)이지만 앞에 붙은
**Manual vs Automatic**의 차이이다.

### MRC

MRC는 힙에 메모리를 직접 할당/해제를 해주는 것이다.
2011년 이전엔 Object-C에서 이 방법을 사용

---

# ARC의 메모리 관리 방법

```
메모리 참조 횟수(RC)를 계산하여, 참조 횟수가 0이 되면 더 이상 사용하지 않는 메모리라 생각하여 해제
```

> #### RC(Reference Count)
>
> 이 인스턴스를 현재 누가 가리키고 있냐 없냐(참조의 유무)를 숫자로 나타낸 것

만약 참조 횟수(RC)가 10이면 해당 인스턴스가 10군데에서 참조되있단 뜻이고, 0이라면 참조되고 있는 곳이 없으니 필요없다. -> 메모리를 해제해도 좋다.

따라서 모든 인스턴스는 자신의 RC 값을 가지고 있다.
(인스턴스가 생성될 때 힙에 같이 저장)

## 참조 횟수

### Count UP(+)

```
참조 횟수가 +1이 되는 순간은 인스턴스의 주소 값을 변수에 할당할 때에 Count Up이 된다.
```

쉽게 두가지로 나눠보자.

1. 인스턴스를 새로 생성할 때

```
let dwaewoo = Man(name: "dwaewoo", height:  180)
```

아까 봤던 예제 코드가 실행되는 시점에 지역 변수 dwaewoo는 스택에 할당되고 실제 인스턴스는 힙에 할당된다 했었다.
그리고 dwaewoo엔 힙에 할당된 인스턴스 주소 값이 들어간다.
이렇게 인스턴스를 새로 생성할 때(새로운 변수에 대입할 때) 해당 인스턴스에 대한 RC가 증가한다.

```
			[스택 영역] 							[힙 영역]
dwaewoo |	0x11111111	|	 			|	Man Instance	|
		|				| --[RC +1]--> 	|	name: "Dwae Woo"|	<- 0x11111111
        |               |		 		|	height: 180		|		RC = 1
```

2. 기존 인스턴스를 다른 변수에 대입할 때

```
let copy = dwaewoo
```

기존 인스턴스를 다른 변수에 대입할 때에도 당연히 참조에 의하기 때문에 RC값이 증가

```
			[스택 영역] 							[힙 영역]
dwaewoo |	0x11111111	|	 			|	Man Instance	|
		|				| --[RC +1]--> 	|	name: "Dwae Woo"|	- 0x11111111
        |               |		 		|	height: 180		|		RC = 2
copy    |	0x11111111	| --[RC +1]-->	|					|
```

### Count Down(-)

참조 횟수가 내려가는 경우는 4가지가 있다.

**1. 인스턴스를 가리키던 변수가 메모리에서 해제되었을 때**

아까 dwaewoo 인스턴스를 생성하는 작업을 밑과 같이 바꾼다.

```
func makeCopy(_ origin: Man) {
	let copy = origin								// 2. Instance RC : 2
}
let dwaewoo = Man(name: "dwaewoo", height: 180)		// 1. Instance RC : 1
makeCopy(dwaewoo)
___________________________________________________
													// 3. Instance RC : 1
```

위 주석이 이해가 되었다면 지금까지의 내용을 잘 따라온 것이다.

- dwaewoo가 생성되는 순간 인스턴스의 RC가 +1
- makeCopy 함수가 실행되어 dwaewoo를 참조하는 copy 변수가 생성되는 순간 인스턴스의 RC가 +1
- 함수가 종료되어 지역변수 copy가 스택에서 해제되는 순간 인스턴스의 RC가 -1

간단히 정리하면 인스턴스를 참조하고 있던 변수가 메모리에서 해제되면 해당 인스턴스의 RC 값은 -1이 된다.

**2. nil이 지정되었을 때**

우선 nil이 지정된다는 소리는 당연히 옵셔널 타입이라는 소리이다.

```
var dwaeoo: Man? = .init(name: "dwaewoo", height: 180)	// 1. Instance RC : 1
var copy = dwaewoo										// 2. Instance RC : 2
copy = nil												// 3. Instance RC : 1
dwaewoo = nil											// 4. Instance RC : 0 (메모리 해제)
```

**3. 변수에 다른 값을 대입한 경우**

```
var dwaewoo: Man? = .init(name: dwaewoo, height: 180)	// 1. dwaewoo Instance RC : 1
var copy: Man? = .init(name: "dwaewoo", height: 160)	// 2. copy Instance RC : 1
dwaewoo = copy											// 3. copy Instance RC : 2, dwaewoo Instance RC : 0 (메모리 해제)
```

dwaewoo에 copy 값을 대입하면
dwaewoo RC는 -1
copy의 RC는 +1

당연히 dwaewoo 변수에 저장된 주소 값이 바껴 참조 횟수도 변한다.
따라서 dwaewoo가 가리키던 인스턴스는 RC가 0이 되었으므로 ARC에 의해 자동으로 메모리에서 해제

**4. 프로퍼티의 경우, 속해 있는 클래스 인스턴스가 메모리에서 해제될 때**

```
class Info {
    var mail: String?
    var phone: Int?

    init(mail: String?, phone: Int?) {
        self.mail = mail
        self.phone = phone
    }
    deinit { print("Info Deinit)" } }
}

class Man {
    var name: String?
    var height: Int?
    var info: Info? = .init(mail: "dwaewoo@", phone: 01012341234)

    init(name: String?, height: Int?) {
        self.name = name
        self.height = height
    }
    deinit { print("Man Deinit)" } }
}

let dwaewoo: Man? = .init(name: "dwaewoo", height: 180)
dwaewoo = nil
```

Man 클래스 안에 info 클래스 인스턴스가 프로퍼티로 존재
따라서 Man 인스턴스 참조하는 dwaewoo를 생성하면 dwaewoo와 프로퍼티인 info도 생성되며 두 인스턴스 각각의 RC가 증가

```
			[스택 영역] 							[힙 영역]
dwaewoo |	0x11111111	|	 			|	Man Instance	  |
		|				| --[RC +1]--> 	|	name: "Dwae Woo"  |	- 0x11111111
        |               |		 		|	height: 180		  |		RC = 1
		|				|	 	   /----|	info: 0x22222222  |
----------------------------    [RC +1] ----------------------------------------
	    |				| 		   \--->|	Info Instance	  | - 0x22222222
	    |				| 				|	mail: "dwaewoo@"  |		RC = 1
	    |				| 				|	phone: 01012341234|
```

info는 dwaewoo가 가리키던 인스턴스에 소속된 프로퍼티이기 때문에 dwaewoo가 가리키던 인스턴스가 메모리에서 해제될 경우, info의 RC가 하나 감소

쉽게 풀어서 단계별로 설명하자면

- dwaewoo가 nil이 되어 Man 인스턴스 RC가 1에서 -1이 되어 0이됨
  , Info 인스턴스는 그대로
- Man 인스턴의 RC가 0이므로 메모리에서 해제
  이때, Man 인스턴스가 품고있던 Info 프로퍼티도 같이 메모리에서 해제되어 info 프로퍼티가 가리키고 있던 Info 인스턴스의 RC가 1 감소
- Info 인스턴스의 RC도 0이 되었으므로 메모리에서 해제

여기서 주의할 점은 dwaewoo가 가리키던 Man 인스턴스가 메모리에서 해제된다고 프로퍼티인 info가 가리키던 Info 인스턴스의 메모리가 같이 해제되는 것이 아닌 Info 인스턴스의 RC가 1 감소하는 것이다.

만약 RC가 감소했을 때 위의 예시처럼 RC = 0 이되면 메모리가 해제되는 것이다.

위의 예제의 실행 결과를 찍어보면

```
Man deinit
Info deinit
```

dwaewoo가 가리키던 Man 인스턴스의 RC가 먼저 0이되어 deinit 된 후에
Man 인스턴스의 프로퍼티인 info가 가리키던 Info 인스턴스의 RC가 0이 되어 deinit

---

# 공부를 하며

ARC에 관한 블로그를 탐색하며 공부하던 중에 한 블로그에서 좋은 질문을 발견하였다.

```
Q.
ARC가 Compile Time에 실행되는데 어떻게 동적으로 실행되는 것들의 Reference Count를 세고 메모리를 관리를 할 수 있나?
```

> #### 블로거의 답변
>
> ![](https://velog.velcdn.com/images/blooper20/post/8ac6bbc1-e5c1-4f4f-a11f-80a4501822ac/image.png)

> #### 나였다면?
>
> 레퍼런스 카운트를 세기 위해 2가지의 카운트 업 방식과 4가지의 카운트 방식이 있습니다
> 우선 카운트 업 방식에는 인스턴스가 생성할 때와 기존의 인스턴스를 다른 변수에 대입할 때이고, 카운트 다운 방식은 인스턴스를 가리키던 변수가 메모리에서 해제 되었을 때, 인인스턴스에 nil이 대입되었을 때, 변수에 다른 값을 대입했을 때,프로퍼티일 경우에 참조하고 있던 인스턴스가 메모리에 해제가 되었을 때입니다. 이렇게 레퍼런스 카운트를 세면서 0이 되었을 때 메모리 해제를 합니다.

무엇이 더 좋은 대답인지는 아직 모르겠지만 나의 답변은 너무 길고 면접에서 답할 시간을 기다려줄까 의문또한 든다.
또한 방금까지 ARC에 대해 공부를 했는데도 한번에 주르륵 나오는 것이 아닌 기억을 더듬으며 대답을 작성하였다. 이런 점을 고친 더욱 좋은 답변을 고민해봐야겠다.

# 참고한 블로그 링크

- https://babbab2.tistory.com/26
- https://sujinnaljin.medium.com/ios-arc-%EB%BF%8C%EC%8B%9C%EA%B8%B0-9b3e5dc23814
