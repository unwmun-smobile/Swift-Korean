---
id: chapter4
title: 기본 연산자 (Basic Operators)
---
> Translator : 해탈 (kimqqyun@gmail.com)

_연산자_는 값을 확인 변경 합치기 위해 사용하는 특수 기호나 문구입니다. 예를 들어 더하기 연산자(`+`)는 (`let i = 1 + 2`에서 쓰이는것 같이) 두 숫자를 더합니다. 
더 복잡한 연산자에 대해 예를 들자면,  (`if enteredDoorCode && passedRetinaScan`에서와 같이) 논리 AND 연산자 `&&`가 있고, `i`의 값을 `1`만큼 증가시키는 것을 축약해서 표현한 `++i` 증가 연산자가 있습니다. 

Swift 는 대부분의 표준 C 연산자를 지원하며 일반적인 코딩 오류를 제거하는 몇가지 기능을 향상 시켰습니다. 할당연산자 (`=`)대신 항등 연산자(`==`)를 사용하는 실수를 방지하기 위해 값을 반환하지 않습니다.
산술연산자(`+` ,` -` ,`*`,`/` ,`%` 등)가 오버플로우를 감지하고 그들을 저장하는 유형의 허용된 값의 범위보다 크거나 작아서 발생하는 예기치 않은 결과를 방지 할 수 있습니다. 
당신은 오버 플로우 연산자에 설명된대로 Swift의 오버플로우 연산자를 사용하여 오버플로 값을 선택할수 있습니다. 이것은 [Overflow Operaters]() 에 설명되어 있습니다.

C 와 달리, Swift는 부동 소수점 숫자에 나머지 (`%`) 계산을 수행 할 수 있습니다. 또한 Swift는 C언어에는 없는 (`A..B`)와 (`A...B`)의  2가지의 범위 연산자를 제공합니다. 이 연산자들은 값의 범위를 표현하기 위한 연산자입니다. 

이 장에서는 Swift의 일반적인 연산자를 설명합니다. 고급 연산자는 [고급 연산자(Advanced Operator)]() 장에 있습니다, 그리고 사용자 정의 연산자를 정의하고 사용자 정의 형식에 대한 표준 연산자를 구현하는 방법에 대해 설명합니다.

## 용어 (Teminology)
연산자는 단항, 이진, 그리고 삼항이 있습니다.

- _단항_ 연산자는 단일 대상에서 작동합니다. (예 `-a`) 단항 _전위_ 연산자를 바로 앞에 나타내고, (예 `!b`) 단항 _후위_ 연산자는 타겟이후에 즉시 나타납니다. (예 `i++`)
- 이항 연산자는 두 가지의 대상에 작동합니다. 이항연산자는 중위연산자이며 두 대상 사이에 나타납니다.  (예 `2 + 3`)
- 삼항 연산자는 세 가지 대상에 작동합니다. C 처럼 , Swift는 하나의 삼항연산자를 가지고 있습니다. 삼항 조건 연산자는 (`a ? b : c`) 입니다.

연산자에 영향을 주는 값은 피연산자입니다. 식 `1 + 2`에을 보면 `+` 기호는 이항 연산자이며 두가지의 피연산자 값인 `1` 과 `2`입니다.

## 할당 연산자
할당 연산자는 (`a = b`) 초기화자(initializes) 또는 `b` 의 값을 `a` 에 할당하는것입니다.
```
let b = 10
var a = 5
a = b
// a 는 이제 10 과 같습니다.
```

만약 오른쪽이 같은 여러 값을 가진 튜플의 경우에 그 요소는 한번에 여러개의 상수 또는 변수로 분해 될수있습니다.
```
let (x, y) = (1, 2)
// x 는 1 과 같고 y 는 2 와 같다.
```

C 와 Objective-C의 대입 연산자와는 달리, Swift의 대입 연산자 자체가 값을 반환하지 않습니다. 다음 구문은 유효하지 않습니다.
``` 
if x = y {
	// x = y가 값을 반환하지 않기 때문에 이것은 유효하지 않다, 
}
```

위 구문이 유효하지 않은 이유는, 실수로 (`==`) 대신 (`=`) 연산자를 사용하는것을 방지하기 위해서입니다. `if x = y` 가 유효하지 않게 함으로써 Swift 코드에서 이러한 종류의 오류를 방지하는데 도움이 됩니다.

## 산술 연산자
Swift 는 4가지의 산술연산자가 모든 숫자 타입을 지원합니다.

- 덧셈 (`+`)
- 뺼셈 (`-`)
- 곱셈 (`*`)
- 나눗셈 (`/`)

```
1 + 2 			// 3
5 - 3 			// 2
2 * 3 			// 6
10.0 / 2.5		// 4.0
```
C 및 Objective-C의 산술 연산자와는 달리 Swift 산술 연산자는 값이 기본적으로 오버플로우하는것을 허용하지 않는다. Swift 오버플로우 연산자(`a &+ b`와 같은)를 사용하여 값 오버플로 동작을 선택할 수있습니다. [Overflow Operators]()를 참조하십시오.

또한 덧셈 연산자는 문자열을 지원합니다.
```
"hello, " + "world" // "hello, world" 와 같다
```

두 개의 `Character` 값이거나 하나는 `Character` 값 그리고 하나는 `String` 값일때 두 개를 함께 더해서 새로운 `String` 값을 만들 수 있습니다.

```
let dog: Character = "🐶🐶
let cow: Character = "🐮"
let dogCow = dog + cow
// dogCow is equal to "🐶🐮"
```
이것에 대해선 [문자열과 문자(Concatenating Strings and Characters)]()를 참조 바랍니다 

## 나머지 연산자
나머지 연산자는 (`a % b`) `b` 의 몇 배수가 `a`에 맞게 곱해지며 그리고 남아 있는 값을 반환합니다. (이는 _나머지_ 라고 불립니다.)

> NOTE   

> 나머지 연산자는 (`%`) 또한 _모듈로(modulo) 연산_으로 다른 언어에 알려져있다. 그러나 Swift에서의 동작은 음수를 의미한다. 엄격히 말하면, 모듈로 연산보다는 나머지 연산이다.

여기에 나머지 연산의 동작이 어떻게 되는지 나와있습니다. `9 % 4`을 계산해보면, 당신은 첫번째로 `9`안에 몇 개의 `4`가 들어갈 수 있는지 알아낼 것이다.
![remainderinteger_2x.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/remainderinteger_2x.png)

당신은 `4`들을 `9`에 맞추었고 그리고 나머지는 `1`이다. (오렌지 색깔을 보라)

Swift에서는 이렇게 쓰여집니다.
```
9 % 4 // 1과 같다
```

`a % b` 의 답을 측정해보면, `%` 연산자는 아래의 방정식을 계산하고, `remainder`를  반환합니다.

`a` = (`b` x `배수`) + `나머지`

`배수`는 `a` 에 들어갈 `b`의 최대의 숫자입니다.

`9` 와 `4`를 식에 대입 할경우

`9` = (`4` × `2`) + `1 `

`a` 의 값이 음수 일때도 같은 메소드가 지원되며 나머지 값이 음수가 나옵니다.

```
-9 % 4 // -1과 같다
```

`-9` 와 `4` 를 넣으면 다음과 같은 식이 나옵니다.

`-9` = (`4` × `-2`) + `-1`

나머지 값이 `-1`이 주어집니다.

`b`가 음수일때 부호는 무시됩니다. 이 뜻은 `a % b` 와 `a % -b`는 항상 같은 대답을 주고 있다는 것을 의미합니다.

## 부동 소수점 나머지 연산
C 와 Objective-C의 나머지 연산과는 달리, Swift의 나머지 연산은 부동 소수점 연산 또한 지원합니다.

```
8 % 2.5 // 2.5와 같음
```
  
예를 들어 `8`을 `2.5`로 나누었을때 `3`과 같으며 나머지는 `0.5`와 같습니다. 그리고 나머지 연산이 반환하는 값은 `Double` 타입의 `0.5` 입니다.
![remainderfloat_2x.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/remainderfloat_2x.png)


## 증가연산자와 감소 연산자
C와 같이, Swift는 증가 연산자(`++`)와 감소 연산자(`--`)를 제공한다. 이것은 숫자 변수 `1`를 증가시키거나 감소시키는 축약형입니다. 정수형과 부동소수점형을 연산자와 같이 사용 가능합니다.
```
var i = 0
++i // i 는 이제 1과 같다
```

만약 `++i` 호출마다 `i`의 값은 `1` 씩 증가됩니다. 기본적으로 `++i` 는 `i = i + 1` 의 약어입니다. 마찬가지로 `--i`를 `i = i - 1` 의 약어로 사용할 수 있습니다.

`++` 와 `--` 기호는 전위연산자 또는 후위연산자로 사용이 가능합니다. `++i` 와 `++i`는 둘다` i`의 값을` 1` 증가시키는 방법입니다. 비슷하게, `--i` 와 `i--`는 `i`의 값을 `1` 감소시키는 방법입니다.

이러한 수정연산자는 `i` 와 그리고 반환값 까지 변화시킵니다. 만약 `i`에 저장된 값을 증가 또는 감소 시킬 경우 반환값을 무시 할 수도 있습니다. 그러나 반환된 값을 사용할 경우, 당신은 다음과 같은 규칙에 따라 연산자의 전위연산자나 후위연산자를 사용하는지 여부에 따라 달라집니다.

- 만약 변수 앞에 쓰여질 경우, 값이 증가한 후에 반환된다.
- 반약 변수 뒤에 쓰여질 경우, 값이 반환된 뒤에 증가된다.

예제 코드 (For example:)
```
var a = 0
let b = ++a
// a 와 b 둘다 1과 같다.
let c = a++
// a 는 지금 2 입니다. 그러나 c는 이전의 값인 1이 이미 설정되어있습니다.
```

위의 예제코드에서 `let b = ++a` 는 `a`를 반환하기 전에 `a`를 증가시킨다. 이 방법은 `a` 와 `b` 의 새로운 값이 동등한 이유이다.

그러나, `let c = a++` 는 `a`를 후에 반환한 뒤 `a`를 증가시킨다. 이 뜻은 `c`가 없은 값은 예전의 값인 `1`이며 `a`에게는 업데이트 된 `2`와 같습니다.

`i++`의 특정동작을 필요로 하지 않는한,  `++i` 나 `--i`를 사용하는것이 좋습니다. 왜냐하면 그것은 모든 경우에 `i`를 결과를 반환하고 수정하는 예상된 동작을 가지기 때문입니다. 

## 단항 마이너스 연산자
숫자 값의 부호는 전위연산자 `-`를 사용하여 전환할 수 있다. 이것은 단항 마이너스 연산자로 알려진것입니다.

```
let three = 3
let minusThree = -three // minusThree equal -3
let plusThree = -minusThree // plus equal 3, or "minus minus 		three"
```

단항 마이너스 연산자는 공백없이 값 바로 앞에 추가됩니다.

## 단항 플러스 연산자
단항 플러스 연산자(`+`)는 간단하게 값 앞에 추가되며 값을  변경하지 않고 값을 반환합니다.

```
let minusSix = -6
let alsoMinusSix = +minusSix // alsoMinusSix equals -6
```

플러스 연산자가 있음에도 불구하고 실제로 아무것도 하지 않지만, 당신은 또한 단항 마이너스 연산자를 사용하는 경우에 양수에 대한 코드대칭에 사용할 수 있습니다.

## 복합 할당 연산자
C와 같이 Swift는 다른 작업에 할당(`=`)을 결합하는 복합 할당 연산자를 제공합니다. 한 예를 들어 덧셈 할당 연산자입니다 (`+=`):
```
var a = 1
a += 2
// a 는 3과 같다
```

표현식 `a += 2` 는 `a = a + 2` 의 축약형입니다. 효과적으로 한 연산자가 가산 및 할당이 동시에 결합과 작업이 됩니다.

>NOTE
복합 할당 연산자는 값을 반환하지 않습니다. 당신은 `let b = a += 2` 이러한 코드를 작성할수 없습니다. 예를 들어 이러한 코드는 위의 증가 및 감소 연산자와는 다릅니다. 

복합 할당 연산자의 전체 목록은 [Expressions]() 에서 찾을 수 있습니다.

## 비교 연산자
Swift는 C의 표준 비교연산자를 지원합니다.

- 같음 연산자 (`a == b`)
- 같지 않음 연산자 (`a != b`)
- 보다 큰 (`a > b`)
- 보다 작은(`a < b`)
- 보다 크거나 같은 (`a >= b`)
- 보다 작거나 같은 (`a <= b`)

>NOTE
Swift는 또한 두 개체 참조가 동일한 인스턴스 객체를 참조하고 있는지 여부를 테스트 하는 연산자를 지원합니다. (`===` 와 `!==`) 자세한 내용은 [Classes and Structures]()를 참조하십시오.

비교 연산자의 각 문장이 참인지 여부를 나타내는 `Bool` 값을 반환합니다 :

```
1 == 1   // true, because 1 is equal to 1
2 != 1   // true, because 2 is not equal to 1
2 > 1    // true, because 2 is greater than 1
1 < 2    // true, because 1 is less than 2
1 >= 1   // true, because 1 is greater than or equal to 1
2 <= 1   // false, because 2 is not less than or equal to 1
```

비교 연산자는 종종 `if`문 같은 조건문에 사용됩니다 :

```
let name == "world"
if name == "world" {
	println("hello, world")
} else {
	println("I'm sorry \(name), but I don't recognize you")
}
// prints "hello, world", because name is indeed equal to "world”
```
`if`에 대한 더 많은 정보는 [Control Flow]()를 참조하기 바랍니다.

## 삼항 조건 연산자
삼항 조건 연산자는 특별한 연산자와 세개의 파트로 이루어져있습니다.
식은 이러합니다. (`question ? answer1 : answer2`) 
이 `question`을 기초로하여 참인지 거짓인지에 따라 두 식중 하나를 평가하기 위한 축약어입니다. 만약 `question` 이 참이면 `answer1`을 계산하고 값을 반환합니다; 그렇지 않으면 `answer2`를 계산하고 값을 반환합니다.

삼항 조건 연산자는 아래의 코드에 대한 단축 표현입니다.

```
if question {
	answer1
} else {
	answer2
}
```

이것은 테이블 행의 픽셀 높이를 계산하는 예제입니다. 행의 헤더가 있다면 컨텐츠의 높이가 50 픽셀이상이고 행의 헤더가 없다면 20픽셀 보다 큰것입니다.: 

```
let contentHeight = 40
let hasHeader = true 
let rowHeight = contentHeight + (hasHeader ? 50 : 20)
// rowHeight 는 90과 같다
```

위의 예제코드는 아래 코드의 속기입니다.

```
let contentHeight = 40
let hasHeader = true
var rowHeight = contentHeight
if hasHeader {
	rowHeight = rowHeight + 50
} else {
	rowHeight = rowHeight + 20
}
```

첫번째 예제의 삼항 조건 연산자의 사용은 `rowheight`에 단 한줄의 코드를 이용하여 올바른 값으로 설정될 수 있음을 의미합니다. 이것은 두 번째 예제코드보다 간결하고 그 값이 `if` 문 내에서 수정될 필요가 없기 떄문에 이것은 `rowheight`가 변수가 될 필요성이 없어집니다.

삼항 조건 연산자는 두 식의 어떤 결정을 고려하는것을 위해 효율적인 속기를 제공합니다. 그러나 삼항 조건 연산자는 주의해서 다뤄야 합니다. 남용하면 그 간결함은 읽기 어려운 코드로 이어질 수 있습니다. 하나의 복합 구문에 삼항 조건 연산자와 다중 인스턴스를 결합하는것을 피하십시오.

## 범위 연산자
Swift는 두 개의 범위연산자를 지원하며 이 축약어는 값의 범위를 표현합니다.

### 폐쇄 범위 연산자
폐쇄 범위 연산자(`a...b`)는 `a`에서 `b` 까지의 범위를 정의합니다. 그리고 `a`와 `b`의 값을 포함합니다.

폐쇄 범위 연산자는 `for-in` 루프와 같이 사용하고자 하는 값 범위에서 반복할때 폐쇄 범위 연산자는 유용합니다.

```
for index in 1...5 {
	println("\(index) time 5 is \(index * 5)")
}
// 1번쨰 반복 5 is 5
// 2번쨰 반복 5 is 10
// 3번쨰 반복 5 is 15
// 4번쨰 반복 5 is 20
// 5번쨰 반복 5 is 25
```
`for-in` 루프에 대해서는 [Control Flow]() 항목을 참조하십시오

### 반 폐쇄 범위 연산자
반 폐쇄 범위 연산자 (`a..b`)는 `a` 에서 `b` 로 실행되는 범위를 정의하지만 `b`가 포함되어 있지 않습니다. 처음 값은 포함하고 있지만 최종값은 아니기 때문에 반폐쇄라고 합니다.

반 폐쇄 범위는 특히 0을 기반으로한 리스트 또는 배열로 작업할때 유용합니다. 그것은 리스트의 길이(포함안되는)까지 계산하는데 유용합니다.
```
let names = ["Anna", "Alex", "Brian", "Jack"]
let count = name.count
for i in 0..count {
	println("Person \(i + 1) is called \(names[i]")
}
// Person 1 is called Anna
// Person 2 is called Alex
// Person 3 is called Brian
// Person 4 is called Jack
```
배열에는 4개의 항목이 포함되어있습니다. 하지만 반 폐쇄 범위기 때문에 `0..count` 는 단지 3까지만 카운트 합니다. (배열의 마지막 항목의 인덱스)
배열에 대해 더 참조하고 싶다면 [Arrays(배열)]()을 참조하세요.

## 논리 연산자
논리 연산자는 `true`와 `false` 불리언 논리 값을 수정하거나 결합합니다. Swift는 C 기반 언어의 세 가지 표준 논리 연산자를 지원합니다.

- NOT (`!a`)
- AND (`a && b`)
- OR (`a || b`)

## 논리 NOT 연산자
논리 NOT 연산자(`!a`)는 불리언 논리 값인 `true` 값을 반전시키고 `false` 값은 `true` 가 됩니다.

논리 NOT 연산자는 전위 연산자입니다. 값 앞에 연산을 공백없이 즉시 표현 할 수 있습니다. 이것은 "`not a`"로 바로 읽을 수 있으며 다음의 예제에서 볼 수 있습니다.

```
let allowedEntry = false
if !allowedEnrty {
	println("ACCESS DENIED")
}
// prints "ACCESS DENIED"
```

`if !allowedEntry` 는 "if not allowed entry" 로 읽을 수 있습니다.
즉 `allowedEntry`이 `false`인 경우 라인 이후의 `not allowed entry` 가 `true`인 경우에 해당할 경우로 실행됩니다.
이 예제에서와 같이 불리언 상수와 변수 이름의 주의 깊은 선택은 이중 부정 또는 혼란한 논리구문을 피하면서 읽기 쉽고 간결한 코드를 유지하는데 도움이 될 수 있습니다.

## 논리 AND 연산자
논리 AND 연산자(`a && b`)의 전체 표현식은 두 값이 모두 `true`이어야 `true`가 됩니다.

반대로 두 값이 `false` 이면 전체 표현식 또한 `false` 입니다. 사실 첫번째 값이 `false` 인 경우 두번째 값이 평가되지 않습니다. 그것을 가능할수 없기 때문에 전체표현식이 `true`와 같게 됩니다. 이는 _short-circuit evaluation_ 로 불립니다.

이 예제에서는 두 개의 `Bool`값을 고려하여 만약 두 값이 `true` 에만 접근할 수 있습니다.

```
let enteredDoorCode = true
let passedRetinaScan = false
if enteredDoorCode && passedRetinaScan {
	println("Welcome!")
} else {
	println("ACCESS DENIED")
}
// prints "ACCESS DENIED"
```

## 논리 OR 연산자
논리 OR 연산자(`a || b`)는 인접한 파이프 문자로 만든 중위연산자 입니다. 전체표현식이 `true`가 될 때 까지 두 개의 값 중 하나만이 참이어야 하는 논리식을 만드는데 사용합니다.

위의 논리 AND 연산자처럼 논리 OR 연산자는 식을 고려할떄 short-circuit evaluation을 사용합니다. 논리 OR식의 좌측에 `true`가 해당하는 경우는 전체 표현식의 결과를 변경 할수 있기 때문에 우측은 계산되지 않습니다.

아래의 예제에서 첫 번째 `Bool` 값(`hasDoorKey`)은 `false`이지만 두 번째 값(`knowsOverridePassword`)는 `true`이다. 하나의 값 이`true`이기 떄문 전체표현식은 `true`로 평가하고 접근이 허용됩니다.

```
let hasDoorKey = false
let knowOverridePassword = true
if hasDoorKey || knowOverridePassword {
	println("Welcome!")
} else {
	println("ACCESS DENIED")
}
// prints "Welcome!"
```

## 복합 논리 연산자
당신은 여러 논리 연산자를 결합하여 복합 논리 연산자를 만들 수 있습니다.

```
if enteredDoorCode && passedRetinaScan || hasDoorKey || knowOverridePassword {
	println("Welcome!")
} else {
	println("ACCESS DENIED")
}
// prints "Welcome!"
```

이 예제는 `&&` 및 `||` 연산자를 여러개 사용하여 긴 복합 표현식을 만들었습니다. 그러나 `&&` 와 `||` 연산자는 여전히 두 개의 값에 대해 작동하므로 이는 실제로 서로 세개가 연결된 작은 표현입니다.

만약 우리가 문의 코드를 입력하고 망막 검사를 통과한경우; 우리가 유효한 도어 키가 있는 경우이거나  긴급 재정의 암호를 알고있는 다음에 접근할 수 있습니다.

`enteredDoorCode` 와 `passedRetinaScan` 그리고 `hasDoorKey` 의 값에 기초하여 처음 두 개의 작은 표현식은 `false` 입니다. 그러나 긴급 재정의 암호가 `true`로 알려져있습니다 ,그래서 전체 복합 표현식은 여전히 `true`로 평가됩니다. 

## 괄호 명시
괄호가 엄격히 필요하지 않은경우, 읽기 복잡한 표현의 의도록 쉽게 만들수 있는 경우에 괄호가 포함되는것이 유용한 경우가 종종 있다.

위의 door access 예제 코드에서 그것의 의도를 명시적으로 확인하기 위해 복합 표현식의 첫번째 부분을 괄호를 추가하는데에 유용합니다.

```
if (enteredDoorCode && passedRetinaScan) || hasDoorKey || knowOverridePassword {
	println("Welcome!")
} else {
	println("ACCESS DENIED")
}
// prints "Welcome!"
```

괄호는 처음 두 값을 전체 논리에서 별도의 가능한 상태의 일부로 분명히 간주되게 만듭니다. 복합식의 출력이 변하지는 않지만 전체적인 목적이 독자에게 명확해집니다. 가독성은 항상 간결함을 선호합니다; 괄호의 사용은 당신의 의도를 확실히 파악하는데 도움이 됩니다.

