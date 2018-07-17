---
id: chapter24
title: 제네릭스 (Generics)
---
> Translator: Hoon H. (Eonil, drawtree@gmail.com)

*제네릭 코드*는 정의된 요구사항에 따라 유연하고 재사용 가능한 함수들을 쓸 수 있도록 해줍니다. 반복을 피하고 의도를 명확하고 추상적으로 나타낼 수 있는 코드를 쓸 수 있습니다.

제네릭스는 Swift의 가장 강력한 기능 중 하나이며, Swift 기본 라이브러리의 많은 부분이 제네릭 코드로 만들어져 있습니다. 눈치채지 못했을 수도 있지만, 사실 제네릭스는 이 Language Guide에 이미 전반적으로 사용되고 있습니다. 예를 들어, Swift의 `Array`와 `Dictionary` 타입들은 모두 제네릭 타입입니다. `Int` 값을 담는 배열이나 `String` 값을 담는 배열을 만들 수 있습니다. 사실 어떤 타입의 배열든지 만들 수 있습니다. 비슷하게, 특정 형식의 값을 담는 사전(dictionary)도 만들 수 있으며, 선택 가능한 타입에는 어떤 제한도 없습니다.

## 제네릭이 해결하는 문제 (The Problem That Generics Solve)
여기에 두 개의 `Int`값을 교체하는 `swapTwoInts`라는 일반적인 비-제네릭 함수가 있습니다.
```
    func swapTwoInts(inout a: Int, inout b: Int) {
        let temporaryA = a
        a = b
        b = temporaryA
    }
```
이 함수는 [In-Out Parameters](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Functions.html#//apple_ref/doc/uid/TP40014097-CH10-XID_226)에 설명된 대로 두 값을 서로 바꾸기 위해 in-out 패러미터를 사용합니다.

`swapTwoInts` 함수는 `b`의 원본값을 `a`로, `a`의 원본 값을 `b`로 바꾸어 넣습니다. 두 개의 `Int` 변수들에 있는 값들을 바꾸기 위해 이 함수를 호출할 수 있습니다.
```
    var someInt = 3
    var anotherInt = 107
    swapTwoInts(&someInt, &anotherInt)
    println("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
    // prints "someInt is now 107, and anotherInt is now 3"
```    
해당 `swapTwoInts` 함수는 유용하지만, 오직 `Int` 값만 사용할 수 있습니다. 만약 두 개의 `String`값이나, 두 개의 `Double` 값을 바꾸려면 `swapTwoStrings`이나 `swapTwoDoubles`같은 함수를 더 작성해야 합니다.
```    
        func swapTwoStrings(inout a: String, inout b: String) {
        let temporaryA = a
        a = b
        b = temporaryA
    }
     
    func swapTwoDoubles(inout a: Double, inout b: Double) {
        let temporaryA = a
        a = b
        b = temporaryA
    }
```
이제 아마 `swapTwoInts`와 `swapTwoStrings`, `swapTwoDoubles`의 본체가 똑같다는 것을 눈치챘을 것입니다. 유일한 차이는 그들이 받아들이는(`Int`와 `String`, `Double`) 값의 타입입니다.

어떤 형식의 값이든 바꿀 수 있는 함수를 쓴다면 훨씬 더 유용하고, 더 유연하게 생각될 것입니다. 이런 종류의 문제가 바로 제네릭이 해결할 수 있는 문제입니다. (제네릭 버전의 이 함수들은 아래에 정의되어 있습니다)

> 노트
이 모든 세 함수에서 `a`와 `b`의 타입은 서로 같도록 정의되어 있습니다. 만약 `a`와 `b`의 타입이 같지 않다면 두 값을 바꾸는 것은 불가능할 것입니다. Swift는 타입-안전 언어이기에 (예를 들어) `String` 타입의 변수와 `Double` 타입의 변수를 서로 바꾸도록 지원하지 않습니다. 그런 시도는 컴파일-타임 에러를 생성합니다.

## 제네릭 함수들 (Generic Functions)
제네릭 함수들은 어떤 타입과도 같이 동작합니다. 여기에 `swapTwoValues`라 불리는, 위에 언급된 `swapTwoInts` 함수의 제네릭 버전이 있습니다:
```
func swapTwoValues<T>(inout a: T, inout b: T) {
  let temporaryA = a
  a = b
  b = temporaryA
}
```
`swapTwoValues` 함수의 본체는 `swapTwoInts` 함수와 같습니다. 하지만, `swapTwoValues` 함수의 첫줄은 `swapTwoInts`와 약간 다릅니다. 여기에 첫 줄의 비교가 있습니다:
```
func swapTwoInts(inout a: Int, inout b: Int)
func swapTwoValues<T>(inout a: T, inout b: T)
```
제네릭 버전의 함수는 (`Int`나 `String`, `Double`같은) 실제 타입 이름 대신 *placeholder* 타입 이름을 사용합니다. (여기에서는 `T`) placeholder 타입 이름은 `T`가 되어야 하는 타입이 어떤 것인지 말하지 않지만, `a`와 `b`가 같은 타입이어야 *한다*는 것을 말합니다. `T`의 자리에 사용될 실제 타입은 `swapTwoValues` 함수가 호출될 때마다 결정될 것입니다.

다른 차이점은 제네릭 버전의 `swapTwoValues` 함수 정의에서는 함수 이름(`swapTwoValues`) 뒤에 placeholder 타입 이름(`T`)이 꺽쇠(`<T>`) 안에 따라온다는 것입니다. 꺽쇠(angle bracket)는 Swift에게 `swapTwoValues` 함수 정의 안에 있는 `T`는 placeholder 타입이라는 것을 알려줍니다. `T`가 placeholder 타입이므로, Swift는 `T`라는 실제 타입을 찾지 않습니다.

`swapTwoValues` 함수는 *어떤* 타입의 두 값이든 사용할 수 있다는 것을 제외하면 이제 `swapTwoInts` 함수와 똑같이 호출될 수 있습니다. 단, 두 값의 타입은 서로 같아야 합니다. 매번 `swapTwoValues`이 호출될 때마다 함수에 전달된 값의 타입에 따라 `T`에 사용될 타입이 추정됩니다.
```
    var someInt = 3
    var anotherInt = 107
    swapTwoValues(&someInt, &anotherInt)
    // someInt is now 107, and anotherInt is now 3
     
    var someString = "hello"
    var anotherString = "world"
    swapTwoValues(&someString, &anotherString)
    // someString is now "world", and anotherString is now "hello"
```
> 노트
위에 정의된 `swapTwoValues` 함수는 Swift 표준 라이브라리에 있는 제네릭 버전 `swap` 함수에서 비롯되었습니다. 이 함수는 자동으로 앱 코드에 사용 가능합니다. `swapTwoValues`의 기능이 필요하다면 스스로 이 기능을 작성하기보다는 Swift에 미리 정의된 `swap` 함수를 쓰기 바랍니다.


## 타입 패러미터 (Type Parameters)
위의 `swapTwoValues` 예시에서 placeholder 타입 `T`는 `타입 패러미터`의 한 예시입니다. 타입 패러미터는 함수 이름 바로 뒤에 (`<T>`같이) 꺽쇠 쌍 안에 있으며, 하나의 placeholder 타입을 지정하고 이름을 붙입니다.

일단 한 번 지정되면, 타입 패러미터는 (`swapTwoValues` 함수의 `a`나 `b` 패러미터들같이) 함수의 패러미터나, 리턴 타입 또는 함수 안에서의 타입 표시(annotation)에 사용될 수 있습니다. 타입 패러미터로 표현된 placeholder 타입은 매번 함수가 호출될 때마다 *실제* 타입으로 바뀌집니다. (위의 `swapTwoValues` 예시에서는 처음 함수 호출에서는 `T`가 `Int`로, 두번째 호출에서는 `String`으로 바뀌었습니다.

꺽쇠 안에 쉼표로 구분된 여러개의 타입 패러미터를 써서 하나 이상의 타입 패러미터들을 제공할 수 있습니다.



## 타입 패러미터 이름짓기 (Naming Type Parameters)

제네릭 함수나 제네릭 타입이 하나의 placeholder 타입(위의 `swapTwoValues` 제네릭 함수나 `Array`같이 하나의 타입만 저장하는 제네릭 컬렉션)만 참조하는 간단한 경우, 타입 패러미터 이름에는 전통적으로 단일 문자 `T`를 사용합니다만, 패러미터 이름으로는 어떤 유효한 식별자든지 사용될 수 있습니다.

여러개의 패러미터를 지닌 좀 더 복잡한 제네릭 함수들이나 제네릭 타입들을 정의한다면, 좀 더 설명적인 타입 이름을 제공하는 것이 더 유용할 것입니다. 예를 들어, Swift의 `Dictionary` 타입은 각각 키와 값을 위한 두 개의 패러미터들을 갖고 있습니다. 만약 `Dictionary`을 스스로 작성해야 한다면 해당 타입 패러미터의 목적을 기억히기 쉽게 하기 위해 `KeyType`와 `ValueType`같은 이름을 사용하고 싶을 것입니다.

> 노트
값이 아닌 타입을 위한 placeholder임을 표시하기 위해, 타입 패러미터 이름은 항상 `UpperCamelCase`로 지정하세요. (예:  `T`, `KeyType`)


## 제네릭 타입들 (Generic Types)

Swift는 제네릭 함수는 물론, *제네릭 타입*도 제공합니다. 이들은 `Array`나 `Dictionary`와 비슷하게 어떤 타입과도 동작하는 사용자-지정 클래스, 구조체, 열거형들입니다.

이 섹션에서는 어떻게 `Stack`이라는 제네릭 컬렉션 타입을 만드는지를 보여줍니다. 스택은 배열과 비슷한 하나의 정렬된 값 집합이지만, Swift의 `Array` 타입보다는 좀 더 제한된 연산만 사용 가능합니다. 배열은 배열의 어떤 위체에나 새로운 아이템이 추가되거나 삭제되도록 지원합니다. 하지만 스택은 컬렉션의 끝에 추가하는 것만 허용합니다. (새 값을 스택에 *push*한다는 표현으로 알려져 있습니다.) 비슷하게 스택은 컬렉션의 끝에 있는 아이템만 삭제하도록 허용합니다. (값을 스택에서 *pop*한다는 표현으로 알려져 있습니다)

>노트
스택 컨셉은 네비게이션 계층 상에서 뷰-제어기를 모델링하기 위해 `UINavigationController` 클래스에 사용되었습니다. 하나의 뷰-제어기를 네비게이션 스택에 추가(또는 push)하기 위해 `UINavigationController` 클래스의 `pushViewController:animated:` 메서드를 호출하거나 , 하나의 뷰-제어기를 네비게이션 스택에서 삭제(또는 pop)하기 위해 `popViewControllerAnimated:` 메서드를 호출할 수 있습니다. 스택은 컬렉션에서 엄격한 "후입선출(last in, first oout)" 방식을 관리할 때 유용합니다.

아래 그림은 스택의 push/pop 동작을 보여줍니다.

![https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/stackPushPop_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/stackPushPop_2x.png)

1. 스택에 현재 새 개의 값이 있습니다.
2. 네번째 값이 스택의 최상위로 "push"되었습니다.
3. 스택이 이제 네 개의 값을 갖고 있습니다. 최신 값의 최상위에 있습니다.
4. 스택의 최상위 아이템이 삭제되었습니다. (또는 "pop")
5. 값을 pop한 후, 스택은 다시 세개의 값을 갖게 됩니다.

여기 `Int` 값 스택으로 어떻게 비-제네릭 버전의 스택을 쓰는지 보여줍니다.
```
    struct IntStack {
        var items = Int[]()
        mutating func push(item: Int) {
            items.append(item)
        }
        mutating func pop() -> Int {
            return items.removeLast()
        }
    }
```
해당 구조체는 스택에 값을 저장하기 위해 `items`라 불리는 `Array` 속성을 사용합니다. `Stack`은 스택에 값을 push하고 pop하기 위해 `push`와 `pop`이라는 두 개의 메서드를 제공합니다. 이 메서드들은 구조체의 `items` 배열을 수정(*mutate*)해야 하므로 `mutating`으로 표시되어 있습니다.

어쨌든, 위에 보여진 `IntStack` 타입은 `Int` 값들만 사용할 수 있습니다. 어떤 타입의 값이든 사용할 수 있는 제네릭 `Stack` 클래스를 정의할 수 있다면 아주 유용할 것입니다.

여기에 같은 코드의 제네릭 버전이 있습니다:
```
    struct Stack<T> {
        var items = T[]()
        mutating func push(item: T) {
            items.append(item)
        }
        mutating func pop() -> T {
            return items.removeLast()
        }
    }
```
제네릭 버전의 `Stack`이 `Int` 타입 대신 placeholder 타입 패러미터 `T`를 사용하는 것 외에는 본질적으로는 비-제네릭 버전과 같음에 주목하세요. 이 타입 패러미터는 구조체 이름 바로 뒤에 꺽쇠 쌍(`<T>`)으로 둘러싸여 쓰여 있습니다.

`T`는 나중에 제공될 "어떤 타입 `T`"를 위한 placeholder 이름을 정의합니다. 이 미래의 타입은 구조체 정의 어디에서나 "`T`"로 참조될 수 있습니다. 이 경우에는, `T`는 세 곳에서 placeholder로 쓰였습니다.

- `T` 타입 값의 빈 배열로 초기화된  `items`이라 불리는 속성.
- `T` 타입이 되어야 하는 `item`이라는 하나의 패러미터를 가지는 `push`라는 메서드.
- `T` 타입 값을 반환하는 `pop` 메서드.

초기화 문법으로 새 인스턴스를 만들 때, 개별 스택의 실제 타입을 타입 이름 뒤에 오는 꺽쇠 기호 안에 써서 `Array`나 `Dictionary`와 비슷하게 `Stack`의 인스턴스를 만들 수 있습니다. 
```
    var stackOfStrings = Stack<String>()
    stackOfStrings.push("uno")
    stackOfStrings.push("dos")
    stackOfStrings.push("tres")
    stackOfStrings.push("cuatro")
    // the stack now contains 4 strings
```
이 그림은 네 개의 값을 push한 뒤에 `stackOfStrings`가 어떻게 되는지를 보여지는지를 보여줍니다.

![https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/stackPushedFourStrings_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/stackPushedFourStrings_2x.png)

한 값을 pop하면 스택의 최상위에서 해당 값을 지우고 반환합니다. `"cuatro"`:
```
    let fromTheTop = stackOfStrings.pop()
    // fromTheTop is equal to "cuatro", and the stack now contains 3 strings
```
최상위 값을 pop한 뒤 스택은 이렇게 됩니다.

![https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/stackPoppedOneString_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/stackPoppedOneString_2x.png)

`Stack`은 제네릭 타입이므로 `Array`나 `Dictionary`와 비슷하게 Swift의 어떤 타입과도 같이 사용될 수 있습니다. 



## 타입 제약 (Type Constraints)
`swapTwoValues` 함수와 `Stack` 타입은 어떤 타입과도 같이 작동합니다. 하지만, 어떨 때는 제네릭 함수와 제네릭 타입에 같이 사용될 수 있는 *타입을 제약(type constraint)*하는 것이 더 유용합니다. 타입 제약은 타입 패러미터가 특정 클래스에서 상속되어야 한다거나 특정 프로토콜 또는 프로토콜 합성을 만족해야 한다거나 하는 것이 될 수 있습니다.

예를 들어, Swift의 `Dictionary` 타입은 사전 키로 사용될 수 있는 타입을 제한합니다. [Dictionaries](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/CollectionTypes.html#//apple_ref/doc/uid/TP40014097-CH8-XID_143)에 설명된대로, 사전 키의 타입은 반드시 *해시-가능(hashable)*해야 합니다. 이는 키가 자신을 유일하게 식별할 수 있는 방식을 제공해야 한다는 뜻입니다. `Dictionary`에서는 특정 키가 이미 등록되어 있는지 여부를 판별하기 위해 해시-가능한 키가 필요합니다. 이 요구사항이 없다면 `Dictionary`는 특정 키를 삽입해야 하는지 아니면 교체해야 하는지 여부를 알 수 없으며, 사전에서 특정 키로 값을 찾을 수도 없습니다.

이 요구사항은 `Dictionary`의 키 타입에 있는 타입 제약으로 강제됩니다. 해당 제약은 키가 반드시 Swift 기본 라이브러리에 정의된 특수 프로토콜인 `Hashable` 프로토콜을 따라야만 하도록 지정합니다. Swift의 모든 기본 타입들(`String`, `Int`, `Double`또는 `Bool`)은 기본적으로 해시-가능입니다.

사용자 지정 제네릭 타입을 만들 때, 독자적인 타입 제약을 정의할 수 있고, 이 제약들은 제네릭 프로그래밍의 강력함을 제공합니다. `Hashable`과 같은 추상 개념은 특정 타입 대신 개념적 특징을 제공합니다.



### 타입 제약 문법 (Type Constraint Syntax)
타입 제약은 타입 패러미터 목록의 일부로 콜론으로 나눠진 클래스나 프로토콜 제약을 타입 패러미터의 이름 뒤에 써서 만듭니다. 타입 제약의 기본적인 문법은 아래에 나타나 있습니다. (제네릭 타입도 문법이 같습니다):
```
    func someFunction<T: SomeClass, U: SomeProtocol>(someT: T, someU: U) {
        // function body goes here
    }
```
위에 있는 가상의 함수는 두 개의 타입 패러미터를 갖고 있습니다. 첫번째 타입 패러미터 `T`는 `T`가 `SomeClass`의 서브클래스가 되도록 요구합니다. 두번째 패러미터 `U`는 `U`가 `SomeProtocol` 프로토콜을 준수(conform)하도록 요구합니다.

### 타입 제약 활용 (Type Constraints in Action)

여기에 `findStringIndex`이라 불리는 비-제네릭 함수가 있습니다. 이 함수는 주어진 `String` 값의 배열에서 주어진 `String` 값을 찾아냅니다. `findStringIndex` 함수는 하나의 선택적(optional) `Int` 값을 반환하는데, 이는 해당 배열에서 찾아낸 처음으로 일치하는 문자열의 인덱스입니다. 일치하는 값이 없으면 `nil`을 반환합니다.
```
    func findStringIndex(array: String[], valueToFind: String) -> Int? {
        for (index, value) in enumerate(array) {
            if value == valueToFind {
                return index
            }
        }
        return nil
    }
```
`findStringIndex` 함수는 문자열 배열 내부에서 문자열 값을 찾기 위해 사용될 수 있습니다:
```
    let strings = ["cat", "dog", "llama", "parakeet", "terrapin"]
    if let foundIndex = findStringIndex(strings, "llama") {
        println("The index of llama is \(foundIndex)")
    }
    // prints "The index of llama is 2"
```
어쨌든,  배열에서 한 값을 찾아내는 원리는 문자열에만 유용한 것이 아닙니다. 문자열에 대한 언급을 어떤 타입 `T`로 바꿈으로서 같은 기능을 `findIndex`라 불리는 제네릭 함수로 쓸 수 있습니다.

여기 `findIndex`라 불리는 제네릭 버전의 `findStringIndex`를 예상해 볼 수 있습니다. 반환되는 타입이 그대로 `Int?`임에 주목하세요. 함수가 선택적 값이 아닌 선택적 인덱스를 반환하기 때문입니다. 하지만 주의하세요 — 이 함수는 컴파일이 되지 않습니다. 예시 다음에 그 이유를 설명합니다.
```
    func findIndex<T>(array: T[], valueToFind: T) -> Int? {
        for (index, value) in enumerate(array) {
            if value == valueToFind {
                return index
            }
        }
        return nil
    }
```
이 함수는 위에 쓰여진대로는 컴파일이 안됩니다. 문제는 동등성[1] 검사(`if value == valueToFind`)에 있습니다. Swift의 모든 타입이 동등(equal to) 연산자(`==`)로 비교 가능한 것이 아닙니다. 만약 복잡한 데이터 구조를 위해 사용자-지정 클래스나 구조체를 만든다면 Swift는 어떻게 이들의 동등성을 검사할 수 있는지 생각해낼수가 없습니다. 때문에, 모든 가능한 타입 `T`에 대해 이러한 코드가 작동하도록 보증하는 것은 불가능하며, 컴파일을 시도하면 적절한 에러가 보고될 것입니다.

[1]: 역주: equality(`==`)는 _동등성_으로 identity(`===`)는 _정체성_으로 번역함. 자세한 것은 "클래스와 구조체의" 하위 항목 [클래스는 참조 타입입니다][Classes Are Reference Types]에 있는 "정체성(Identity) 참조.

[Classes Are Reference Types]: https://developer.apple.com/library/prerelease/ios/documentation/swift/conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_105

하지만, 이것이 완전히 불가능하지는 않습니다. Swift 기본 라이브러리는 `Equatable`이라 불리는 프로토콜을 정의하는데, 이는 이를 준수하는 타입이 동등 연산자 `==`와 비등 연산자 `!=`을 정의하도록 요구합니다. 모든 Swift 기본 타입들은 자동으로 `Equatable` 프로토콜을 지원합니다.

`Equatable`인 모든 타입은 동등 연산자를 지원하므로 안전하게 `findIndex` 함수와 같이 쓰일 수 있습니다. 함수를 정의할 때 이 사실을 표현하기 위해 타입 패러미터의 일부러 `Equatable` 제약을 써야 합니다.
```
    func findIndex<T: Equatable>(array: T[], valueToFind: T) -> Int? {
        for (index, value) in enumerate(array) {
            if value == valueToFind {
                return index
            }
        }
        return nil
    }
```
`findIndex`의 단일 타입 패러미터는 `T: Equatable`로 쓰여 있는데, 이는 "`Equatable` 프로토콜을 준수하는 어떤 타입 `T`"를 의미합니다.

`findIndex` 함수는 이제 제대로 컴파일되며, `Double`이나 `String`같이 `Equatable`을 준수하는 어떤 타입이든 사용할 수 있습니다.
```
    let doubleIndex = findIndex([3.14159, 0.1, 0.25], 9.3)
    // doubleIndex is an optional Int with no value, because 9.3 is not in the array
    let stringIndex = findIndex(["Mike", "Malcolm", "Andrea"], "Andrea")
    // stringIndex is an optional Int containing a value of 2
```




## 연관 타입 (Associated Types)

프로토콜을 정의할 때, 하나 이상의 *연관 타입(associated types)*을 정의하는 것이 유용할 때가 있습니다. 연관 타입은 프로토콜의 일부로 사용되는 placeholder 이름(또는 별칭(*alias*))을 제공합니다. 해당 연관 타입은 프로토콜이 실제로 채용(adopt)될때까지 특정되지 않습니다. 연관 타입은 `typealias` 키워드로 지정됩니다.

### 연관 타입 활용 (Associated Types in Action)
여기에 `ItemType`이라는 연관 타입을 선언(declare)하는 `Container`라는 프로토콜 예시가 있습니다. 
```
    protocol Container {
        typealias ItemType
        mutating func append(item: ItemType)
        var count: Int { get }
        subscript(i: Int) -> ItemType { get }
    }
```
`Container` 프로토콜은 모든 컨테이너가 제공해야 하는 세 자기 능력들을 정의합니다.

- `append` 메서드로 새 아이템을 추가할 수 있어야 한다.
- `Int`값을 반환하는 `count` 속성으로 컨테이너에 있는 아이템의 수를 셀 수 있어야 한다.
- `Int` 인덱스 값을 사용하는 첨자(subscript)로 컨테이너 안의 개별 아이템을 얻을 수 있어야 한다.

이 프로토콜은 아이템들이 컨테이너 내에 저장되는 방식이나, 아이템의 타입을 지정하지 않습니다. 이 프로토콜은 단지 어떤 타입이든 `Container`로 취급되기 위해 제공해야 하는 세 가지 요소를 정의할 뿐입니다. 이를 준수하는 타입은 이 세 가지 요구사항을 만족하는 한 추가적인 기능을 제공할 수 있습니다.

`Container` 프로토콜을 준수하는 타입은 반드시 그들이 저장하는 값의 타입을 지정할 수 있어야 합니다. 특히, 바른 타입의 아이템들만이 컨테이너에 추가되고 첨자로 반환되도록 보증하는 것이 중요합니다.

이러한 요구사항들을 정의하기 위해 `Container` 프로토콜은 특정 컨테이너 타입에 대한 정보 없이도 컨테이너가 보유할 요소들의 타입을 아는 것이 중요합니다. `Container` 프로토콜은 `append` 메서드로 들어온 모든 값이 컨테이너의 요소 타입와 같은 타입이고, 첨자로 반환될 값도 컨테이너의 요소 타입과 같은 타입이라는 것을 지정해야 합니다.

이를 위해, `Container` 프로토콜은 `ItemType`이라는 연관 타입을 선언합니다. 이는 `typealias ItemType`로 쓰여집니다. 프로토콜은 `ItemType`이 어느 타입에 대한 별칭(alias)인지는 정의하지 않습니다 — 그 정보는 준수하는(conforming) 타입이 제공해야 합니다. 그럼에도 불구하고, `ItemType` 별칭은 모든 `Container`에 기대되는 행동양식을 강제하기 위해  `Container` 안에 있는 아이템들을 참조할 수 있는 방법을 제공하고, `append` 메서드와 첨자에 사용되는 타입을 정의합니다.

여기에 예전에 만든 비-제네릭 버전의 `IntStack` 타입이 `Container` 프로토콜을 준수하는 버전이 있습니다. 
```
    struct IntStack: Container {
        // original IntStack implementation
        var items = Int[]()
        mutating func push(item: Int) {
            items.append(item)
        }
        mutating func pop() -> Int {
            return items.removeLast()
        }
        // conformance to the Container protocol
        typealias ItemType = Int
        mutating func append(item: Int) {
            self.push(item)
        }
        var count: Int {
        return items.count
        }
        subscript(i: Int) -> Int {
            return items[i]
        }
    }
```
`IntStack` 타입은 `Container` 프로토콜의 모든 세 가지 요구사항을 구현하며, 이를 위해 개별적으로 `IntStack` 타입의 기존 기능들을 래핑합니다.

더 나아가, `IntStack`는 이 `Container` 구현에서 적절한 `ItemType`은 `Int` 타입이라는 것을 지정합니다.  이 `Container` 구현에서 `typealias ItemType = Int`라는 정의는 `ItemType`라는 추상 타입(abstract type)을 `Int`라는 구체 타입(concrete type)으로 바꾸어 줍니다.

Swift의 타입 추론 기능 덕분에, 실제로는 `ItemType`라는 구체 타입이 `Int` 타입이라는 것을 `IntStack` 정의 일부로 선언할 필요조차 없습니다. `IntStack`이 `Container` 프로토콜의 모든 요구사항을 준수하므로(conforms), Swift는 그냥 `append` 메서드의 `item` 패러미터와 첨자(subscript) 리턴 타입을 살펴보는 것만으로도 적절한 `ItemType`을 추정할 수 있습니다. 실제로, `typealias ItemType = Int` 줄을 위의 코드에서 지우더라도 모든것이 제대로 작동하는데, `ItemType`이 무슨 타입인지가 명백하기 때문입니다.

또한, 제네릭 `Stack` 타입이 `Container` 프로토콜을 준수하도록 만들수도 있습니다.
```
    struct Stack<T>: Container {
        // original Stack<T> implementation
        var items = T[]()
        mutating func push(item: T) {
            items.append(item)
        }
        mutating func pop() -> T {
            return items.removeLast()
        }
        // conformance to the Container protocol
        mutating func append(item: T) {
            self.push(item)
        }
        var count: Int {
        return items.count
        }
        subscript(i: Int) -> T {
            return items[i]
        }
    }
```
이번에는, placeholder 타입 패러미터 `T`가 `append` 메서드의 `item` 패러미터와 리턴, 그리고 첨자 타입으로 쓰였습니다. Swift는 `T`가 이 특정 컨테이너에서 `ItemType`에 적절한 타입임을 추정할 수 있습니다.


### 연관 타입을 지정하도록 기존 타입을 확장하기 (Extending an Existing Type to Specify an Associated Type)

[Adding Protocol Conformance with an Extension](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-XID_355)에 설명된대로 기존 타입에 특정 프로토콜을 준수(conformance)하도록 추가할 수 있습니다. 이는 연관 타입을 가진 프로토콜도 포함합니다.

Swift의 `Array` 타입은 이미 `append` 메서드와 `count` 속성, 그리고 `Int` 첨자를 제공합니다. 이 세 가지 능력들(capabilities)은 `Container` 프로토콜의 요구사항을 만족합니다. 이는 그냥 `Array`가 `Container`를 채용(adopt)한다고 선언하는 것만으로도 준수(conform)하게 됨을 의미합니다. 이것은 [Declaring Protocol Adoption with an Extension](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-XID_357)에 설명된대로 빈 확장(extension)을 사용함으로써 가능합니다.
```
    extension Array: Container {}
```
배열의 기존 `append` 메서드와 첨자는 Swift가 적절한 `ItemType` 타입을 추정할 수 있도록 해줍니다. 위에 있는 `Stack` 타입같이 말이죠. 이 확장을 정의한 후에는 `Array`를 `Container`로 사용할 수 있습니다.


##  Where절 (Where Clauses)

[타입 제약][Type Constraints]에 설명된 타입 제약은 제네릭 함수나 타입에 사용된 타입 패러미터에 요구사항을 정의하게 해줍니다. 

[Type Constraints]: https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Generics.html#//apple_ref/doc/uid/TP40014097-CH26-XID_244

연관 타입에 이런 제약을 정의할 수 있다면 쓸모가 많을겁니다. 이는 타입 패러미터 목록의 일부로 *where절(where clauses)*을 정의하는 것으로 가능합니다. Where절은 연관 타입이 특정 프로토콜을 준수하거나 해당 특정 타입 패러미터가 연관 타입과 같도록 요구할 수 있게 해줍니다. Where절을 만들기 위해서는, `where` 키워드를 패러미터 타입 목록 바로 뒤에 쓰고, 하나 이상의 연관 타입 제약을 쓰고, 타입과 연관 타입간의 일치 관계를 하나 이상 쓰면 됩니다.

다음의 예시는 두 `Container` 인스턴스들이 동등한 아이템을 같은 순서로 보유하는지 여부를 검사하는 `allItemsMatch`이라는 제네릭 함수를 정의합니다. 이 함수는 모든 아이템이 동등하면 `true`를, 아니라면 `false`를 반환합니다.

두 컨테이너들은 같은 타입일 필요가 없지만 (물로, 같아도 됩니다) 보유하는 아이템들의 타입은 같아야 합니다. 이 요구사항은 타입 제약과 where절의 조합으로 표현됩니다.
```
    func allItemsMatch<
        C1: Container, C2: Container
        where C1.ItemType == C2.ItemType, C1.ItemType: Equatable>
        (someContainer: C1, anotherContainer: C2) -> Bool {
            
            // check that both containers contain the same number of items
            if someContainer.count != anotherContainer.count {
                return false
            }
            
            // check each pair of items to see if they are equivalent
            for i in 0..someContainer.count {
                if someContainer[i] != anotherContainer[i] {
                    return false
                }
            }
            
            // all items match, so return true
            return true
            
    }
```
이 함수는 `someContainer`와 `anotherContainer`라 불리는 두 개의 인수를 받습니다. `someContainer` 인수는 `C1`의 타입이고, `anotherContainer` 인수는 `C2`의 타입입니다. `C1`과 `C2` 모두 함수가 호출될때 밝혀질 두 컨테이너 타입을 위한 placeholder 타입 패러미터입니다. 함수의 타입 패러미터 리스트는 두 타입 패러미터 목록에 다음 요구사항들을 추가합니다.

- `C1`은 반드시 `Container` 프로토콜을 준수해야 합니다. (예: `C1: Container`)
- `C2`도 반드시 `Container` 프로토콜을 준수해야 합니다. (예: `C2: Container`)
- `C1`의 `ItemType`은 `C2`의 `ItemType`과 같아야 합니다. (예: `C1.ItemType == C2.ItemType`)
- `C1`의 `ItemType`은 `Equatable` 프로토콜을 준수해야 합니다. (예: `C1.ItemType: Equatable`) 

세번째와 네번째 요구사항들은 where절의 일부로 정의되었고, `where` 키워드 뒤에 함수의 타입 패러미터 목록의 일부로 따라옵니다. 

이 요구사항들은:

- `someContainer`는 `C1`의 컨테이너 타입입니다.
- `anotherContainer`는 `C2`의 컨테이너 타입입니다.
- `someContainer`와 `anotherContainer`는 같은 타입의 아이템들을 가집니다.
- `someContainer` 안에 있는 아이템들은 비등 연산자(`!=`)를 사용해 서로 다른지 여부를 검사할 수 있습니다.

를 의미합니다. 세번째와 네번째 요구사항의 조합은 `anotherContainer`의 아이템들 또한 `!=` 연산자로 검사될 수 있음을 의미하는데, 왜냐면 그것들이 `someContainer`의 아이템 타입과 동일하기 때문입니다.

이 요구사항들은 두 개의 컨테이너 타입이 다른 경우에도 `allItemsMatch` 함수가 두 컨테이너들을 비교할 수 있게 해줍니다. 

`allItemsMatch` 함수는 컨테이너들이 같은 수의 아이템을 갖고 있는지부터 검사하는데, 아이템 수가 다르면 같다고 볼 수가 없기 때문입니다. 이 경우 함수는 `false`를 반환합니다.

이 검사가 끝난 후, 이 함수는 `someContainer` 안에 있는 모든 아이템들을 `for`-`in` 루프와 반폐쇄 범위 연산자(`..`)로 반복(iterate)합니다. 모든 아이템들에 대해, 이 함수는 `someContainer`의 아이템이 `anotherContainer`에 있는 대응 아이템과의 같은지(not equal) 여부를 검사합니다. 만약 둘이 다르면 두 컨테이너는 다른 것이고, 함수는 `false`를 반환합니다.

만약 저 루프가 다른 아이템을 찾지 못하고 종료된다면 두 컨테이너는 같은 것이고 함수는 `true`를 반환합니다.

`allItemsMatch` 함수의 실제 모습은 이렇습니다.
```
    var stackOfStrings = Stack<String>()
    stackOfStrings.push("uno")
    stackOfStrings.push("dos")
    stackOfStrings.push("tres")
     
    var arrayOfStrings = ["uno", "dos", "tres"]
     
    if allItemsMatch(stackOfStrings, arrayOfStrings) {
        println("All items match.")
    } else {
        println("Not all items match.")
    }
    // prints "All items match."
```

위의 예시는 `String` 값들을 저장하기 위한 `Stack` 인스턴스를 만들, 새 개의 문자열을 스택에 추가(push)합니다. 위 예시는 스택과 동일한 세 문자열을 포함하는 리터럴로 초기화된 `Array` 인스턴스도 만듭니다. 스택과 배열은 다른 타입이지만, 이들은 둘 다 `Container` 프로토콜을 준수하며, 같은 타입의 아이템들을 가집니다. 그러므로 이 두 컨테이너들을 인수로 사용해 `allItemsMatch` 함수를 호출할 수 있습니다. 위의 예시에서는 `allItemsMatch` 함수가 두 컨테이너 내의 모든 아이템들이 같다고 정확하게 보고합니다.










