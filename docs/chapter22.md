---
id: chapter22
title: 확장 (Extensions)
---
> Translator : Dongwoo Son (easthelper@gmail.com)

확장(Extensions)은 이미 존재하는 클래스, 구조체, 열거형 타입에 새 기능성을 추가합니다. 이는  원본 소스코드에 접근할 수 없는 타입들도 확장할 수 있습니다. (Retroactive modeling) 확장은 Objective-c 의 카테고리 와 유사합니다.

Swift 의 확장이 할수있는 것:  

* computed properties, computed static properties의 추가
* 인스턴스 메소드와 타입 메소드 정의
* 새로운 이니셜라이저 제공
* 서브스크립트 정의
* 기존 타입에 프로토콜 적용시키기

> NOTE
만약 기존 타입에 새로운 기능성을 추가하기 위해 확장을 정의 한다면, 확장이 정의 되기 이전에 생성된 해당 타입의 모든 인스턴스들도 새 기능성이 적용이 됩니다. 

## 확장 문법
`extension` 키워드로 확장을 선언합니다:
```
extension SomeType {
    // SomeType에 추가할 새 기능
}
```
확장은 기존의 타입을 하나 이상의 프로토콜을 적용하기 위해서 확장시킬 수 있습니다. 이 경우 클래스 또는 구조체와 같은 방식으로 적용시킬 프로토콜 이름을 적습니다:
```
extension SomeType: SomeProtocol, AnotherProtocol {
    // 프로토콜의 요구사항을 이곳에 구현
}
```
확장으로 프로토콜 준수의 추가는 [Adding Protocol Conformance with an Extension]() 에 설명 되어 있습니다.

## 연산 속성
확장은 연산 인스턴스 속성과 연산 타입 속성을 기존의 타입에 추가할 수 있습니다. 이 예제는 거리 단위를 제공하기 위해 다섯개의 연산 인스턴스 속성을 Swift의 내장 `Double` 타입에 추가합니다. 
```
extension Double {
    var km: Double { return self * 1_000.0 }
    var m: Double { return self }
    var cm: Double { return self / 100.0 }
    var mm: Double { return self / 1_000.0 }
    var ft: Double { return self / 3.28084 }
}
let oneInch = 25.4.mm
println("One inch is \(oneInch) meters")
// prints "One inch is 0.0254 meters"
let threeFeet = 3.ft
println("Three feet is \(threeFeet) meters")
// prints "Three feet is 0.914399970739201 meters"
```
이러한 연산 속성들은 `Double` 값이 특정 길이의 단위로 간주됨을 나타냅니다. 연산 속성들로 구현되었지만 부동소수점 리터럴 값에 점 문법으로 속성의 이름을 덧붙여 리터럴 값을 거리값으로 변환시킬 수 있습니다.

예를들어, `1.0`이라는 `Double` 값은 "1 미터"로 간주됩니다. 때문에 `m` 연산 속성은 `self` 를 반환합니다. - `1.m` 표현은 `1.0` `Double` 값 입니다.

다른 단위들은 미터 측정값으로 표현되기 위한 변환이 필요합니다. 1 킬로미터는 1000 미터와 같습니다. 따라서 `km` 연산 속성은 미터로 표현되기 위해 `1_000.00` 을 곱합니다. 같은 방식으로 1 미터는 3.28024 피트입니다. 따라서 피트를 미터로 바꾸기 위해 `ft` 연산 속성은 `double` 값을 `3.28024` 로 나눕니다.

이 속성들은 읽기 전용 속성이고 간결함을 위해 `get` 키워드 없이 사용될 수 있습니다. 속성들의 반환 값은 `Double` 형이기 때문에 `Double` 을 사용하는 어느 곳에서나 수학적 계산과 함께 사용 될 수 있습니다.

```
let aMarathon = 42.km + 195.m
println("A marathon is \(aMarathon) meters long")
// prints "A marathon is 42195.0 meters long"
```

> NOTE
확장은 새로운 연산속성을 추가할 수 있습니다. 하지만 저장 속성 또는 기존 속성에 프로퍼티 옵저버를 추가할 수는 없습니다.

## 이니셜라이저
확장은 기존 타입에 새로운 이니셜라이저를 추가할 수 있습니다. 이는 다른 타입들이 여러분의 커스텀 타입을 이니셜라이저의 인자로 받을 수 있도록 하거나 또는 타입의 기본 구현에 포함되어 있지 않은 추가 적인 이니셜라이저 옵션을 제공할 수 있도록 확장하는 것을 가능하게 합니다.

확장은 새 convenience 이니셜라이저를 클래스에 추가할 수 있습니다. 하지만 새 designated 이니셜라이저 또는 디이니셜라이저를 추가할 수는 없습니다. designated 이니셜라이저와 디이니셜라이저는 반드시 본래의 클래스 구현에서 제공되어야 합니다.

> NOTE
만약 확장을 사용해서 모든 저장 속성의 기본 값을 제공하는 값 타입에 새로운 이니셜라이저를 추가하고, 어떠한 커스텀 이니셜라이저도 정의하지 않았다면, 기본 이니셜라이저와 memberwise 이니셜라이저를 호출 할 수 있습니다. 
[Initializer Delegation for Value Type]()에서 설명한 것 처럼 이니셜라이저를 값 타입의 본래 구현에 작성을 한 경우에는 해당 되지 않습니다. 

아래의 예제는 직사각형을 나타내기 위한 커스텀 `Rect` 구조체를 정의합니다. 또한 모든 속성의 기본값이 `0.0`인 `Size`와 `Point`구조체를 정의합니다. 

```
struct Size {
    var width = 0.0, height = 0.0
}
struct Point {
    var x = 0.0, y = 0.0
}
struct Rect {
    var origin = Point()
    var size = Size()
}
```

[Default Initailizers]() 에서 언급했던 것처럼 `Rect` 구조체는 모든 속성의 기본값을 제공하기 때문에 기본 이니셜라이저와 memberwise 이니셜라이저를 자동으로 받습니다. 이 이니셜라이저들은 새로운 `Rect` 인스턴스를 생성하기 위해 사용될 수 있습니다.

```
let defaultRect = Rect()
let memberwiseRect = Rect(origin: Point(x: 2.0, y: 2.0),
    size: Size(width: 5.0, height: 5.0))
```

`Rect` 구조체에 특정 중심점과 크기를 받기 위한 추가 이니셜라이저를 제공하기 위해 확장할 수 있습니다.
```
extension Rect {
    init(center: Point, size: Size) {
        let originX = center.x - (size.width / 2)
        let originY = center.y - (size.height / 2)
        self.init(origin: Point(x: originX, y: originY), size: size)
    }
}
```

이 새 이니셜라이저는 처음에 제공된 `center` 값과 `size`값을 기반으로 적절한 origin point를 계산합니다. 그 다음 구조체의 자동 memberwise 이니셜라이저 `init(origin:size:)`를 호출하여 새 origin 과 size 값을 적절한 속성에 저장합니다.

```
let centerRect = Rect(center: Point(x: 4.0, y: 4.0),
    size: Size(width: 3.0, height: 3.0))
// centerRect's origin is (2.5, 2.5) and its size is (3.0, 3.0)
```

>NOTE
확장으로 새 이니셜라이저를 제공 할 경우 이니셜라이저가 완료되었을 때 각 인스턴스가 완전히 초기화 되었는지 확인하는 책임은 작성자에게 있습니다.

## 메소드
확장은 기존 타입에 새 인스턴스 메소드와 타입 메소드를 추가할 수 있습니다. 다음 예제는 새 인스턴스 메소드  `repetitions` 를 `Int` 타입에 추가합니다:
```
extension Int {
    func repetitions(task: () -> ()) {
        for i in 0..self {
            task()
        }
    }
}
```
`repetitions` 메소드는 매개변수가 없고 반환 값이 없음을 나타내는 하나의 `()->()`인자를 받습니다.

이 확장을 정의한 후에 여러번의 반복작업을 위해 어느 정수값에서 `repetitions` 메소드를 호출 할 수 있습니다.

```
3.repetitions({
    println("Hello!")
    })
// Hello!
// Hello!
// Hello!
```

호출을 더 간결하게 하기위해 후행 클로저 문법을 사용:
```
3.repetitions {
    println("Goodbye!")
}
// Goodbye!
// Goodbye!
// Goodbye!
```

### Mutating 인스턴스 메소드
확장을 이용해 인스턴스 메소드 추가함으로써 인스턴스 스스로 또한 수정할 수 있습니다. `self` 또는 자신의 속성을 수정하는 구조체와 enumeration 메소드들은 반드시 인스턴스 메소드를 `mutating`으로 표시 해야합니다.

아래 예제는 원래의 값을 제곱하는 새 mutating 메소드 `square` 를 Swift의 `Int`타입에 추가합니다. 

```
extension Int {
    mutating func square() {
        self = self * self
    }
}
var someInt = 3
someInt.square()
// someInt is now 9
```

## Subscripts
확장은 기존 타입에 새 subscripts 를 추가할 수 있습니다. 이 예제는 integer subscript 를 Swift 내장 `Int` 타입에 추가합니다. 이 subscript `[n]` 는 수의 오른쪽으로 부터 `n`번째 자리에 있는 10진수 숫자 하나를 반환합니다:

* `123456789[0]` returns `9`
* `123456789[1]` returns `8`

... 기타 등등:

```
extension Int {
    subscript(digitIndex: Int) -> Int {
        var decimalBase = 1
            for _ in 1...digitIndex {
                decimalBase *= 10
            }
            return (self / decimalBase) % 10
    }
}
746381295[0]
// returns 5
746381295[1]
// returns 9
746381295[2]
// returns 2
746381295[8]
// returns 7
```

만약 `Int` 값이 길이가 요구된 인덱스 보다 적다면 수 왼쪽이 0들로 채워져 있다 여기고 `0`을 반환합니다. 

```
746381295[9]
// 다음을 요청한것 같이 처리되어 0 을 반환 합니다:
0746381295[9]
```

## Nested Types
확장은 새 Nested 타입을 기존 클래스, 구조체, enumeration에 추가할 수 있습니다.
```
extension Character {
    enum Kind {
        case Vowel, Consonant, Other
    }
    var kind: Kind {
    switch String(self).lowercaseString {
    case "a", "e", "i", "o", "u":
        return .Vowel
    case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",
    "n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z":
        return .Consonant
    default:
        return .Other
        }
    }
}
```
이 예제는 새 nested enumeration을 `Character`에 추가합니다. 이 `Kind` enumeration 은 각 문자의 종류를 나타냅니다. 특히 문자가 표준 로마자에서 모음 또는 자음인지(강세나 지역적 다양성을 고려하지 않고), 또는 그 외의 문자인지를 나타냅니다.

이 예제는 또한 새 연산 인스턴스 속성 `kind`을 `Character`에 추가합니다. 이 속성은 해당 문자에 적절한 `Kind` enumeration 멤버를 반환합니다.

이제 `Character` 값에서 nested enumeration 을 사용할 수 있습니다.

```
func printLetterKinds(word: String) {
    println("'\(word)' is made up of the following kinds of letters:")
    for character in word {
        switch character.kind {
        case .Vowel:
            print("vowel ")
        case .Consonant:
            print("consonant ")
        case .Other:
            print("other ")
        }
    }
    print("\n")
}
printLetterKinds("Hello")
// 'Hello' is made up of the following kinds of letters:
// consonant vowel consonant consonant vowel
```

`printLetterinds` 함수는 `String` 값을 받아서 문자열의 각 문자를 iterate 합니다. 각 문자에 대해서 `kind` 연산 속성에 따라 그 글자에 알맞는 종류를 출력합니다. 위 "Hello" 단어의 결과 처럼 `printLetterinds` 함수를 호출해서 단어 안의 모든 문자의 종류들을 출력할 수 있습니다.

> NOTE
`character.kind` 는 이미 `Character.Kind` 타입으로 알려져 있기 때문에 모든 `Character.Kind` 멤버 값들은 `switch` 문에서 `Character.Kind.Vowel`보다 `.Vowel`같이 생략된 형식으로 쓸 수 있습니다. 
