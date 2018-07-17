---
id: chapter5
title: 문자열과 문자 (Strings and Characters)
---
> Translator : 해탈 (kimqqyun@gmail.com)

_문자열_은 `"hello, world"` 또는 `"albatross"` 와 같은 문자의 컬렉션입니다. Swift 문자열은 `String` 타입으로 표시되며 이는 문자 타입의 컬렉션 값 표현입니다.

Swift `String` 및 `Character` 타입은 코드와 함께 텍스트 작업에서 유니코드호환을 완벽호환하며 빠릅니다. 문자 생성 및 조작을 위한 구문은 C 문자열과 유사한 구문을 사용하여 가볍게 읽을 수 있습니다. 문자열 연결은 두 문자열을 추가할 때 `+` 연산자를 추가하는 것만큼 간단하며 문자열의 가변성은 Swift의 다른 값과 상수나 변수 그리고 다른 값들의 선택으로 관리됩니다.

Swift의 `String` 유형은 빠르고 현대적인 구현에도 불구하고 문법이 단순합니다. 모든 문자열 인코딩이 독립적인 유니코드 문자로 구성, 다양한 유니코드 표현에 접근하기 위한 지원을 제공합니다.

문자열 삽입 과정에서 상수, 변수, 리터럴 및 긴 문자열을 삽입할 수 있습니다. 이것은 사용자 정의 문자열 값을 만들어서 보여주거나 저장을 쉽게 할 수 있습니다.


> NOTE 
Swift의 `String` 타입은 Foundation의 `NSString` 클래스에 연결됩니다. 당신은 Cocoa 또는 Cocoa Touch의 Foundation 프레임워크로 작업하는 경우 `NSString`의 API를 이용하여 `String` 값 호출을 만드는 것이 가능하며 또한 이 장에서 설명한 `String` 기능도 사용 가능합니다. 또한, `NSString`의 API 인스턴스를 필요로 하는 `String` 값도 사용 가능합니다.
Foundation 과 Cocoa 에 대한 자세한 정보는 [Using Swift With Cocoa and Objective-C]() 를 참조하십시오.

## 문자열 리터럴

코드 내에서 미리 정의된 `String` 값인 리터럴등을 포함할 수 있습니다. 문자열 리터럴이란 큰따옴표로 둘러싸인 텍스트 문자의 고정된 순서입니다.

문자열 리터럴은 상수나 변수의 초기값을 제공하는것에 사용될 수 있습니다.
```
let someString = "Some string literal value"
```

Swift는 초기화된 문자열 리터럴 값으로 `someString` 상수에 대한 `String`의 형식을 유추합니다. 

문자열 리터럴은 다음과 같은 특수 문자를 포함할 수 있습니다.

- 이스케이프 특별 문자 `\0` (null 문자), `\\` (백슬래시), `\t` (수평 탭), `\n` (줄 바꿈), `\r` (캐리지 리턴), `\"` (큰따옴표), `\'` (작은따옴표)
- 1바이트 유니코드 스칼라는 `\xnn` 이며 `nn`은 두개의 16진수 숫자입니다.
- 2바이트 유니코드 스칼라는 `\unnnn` 이며 `nnnn`은 4개의 16진수 숫자입니다.
- 4바이트 유니코드 스칼라는 `\Unnnnnnnn` 이며 `nnnnnnnn`은 8개의 16진수 숫자입니다.

아래의 코드는 여러 종류의 특수문자의 예를 나타냅니다.
`wiseWords` 상수는 두 개의 이스케이프 문자가 포함되어 있습니다. `dollarSign` 과 `blackHeart` 및 `sparklingHeart` 상수는 세 가지 다른 유니코드 스칼라 문자 형식을 보여줍니다.

```
let wiseWords = "\"Imagination is more important than knowledge\" - Einstein"
// "Imagination is more important than knowledge" - Einstein
let dollarSign = "\x24"        // $,  Unicode scalar U+0024
let blackHeart = "\u2665"      // ♥,  Unicode scalar U+2665
let sparklingHeart = "\U0001F496"  // 💖, Unicode scalar U+1F496
```

## 빈 문자의 초기화 (Initializing an Empty String)

긴 문자열을 만들기 위한 포인트를 위해 빈 `String` 값을 만들려면 빈 문자열 리터럴을 변수에 할당하거나 초기화 문법을 사용하여 새 `String` 인스턴스를 초기화합니다.

```
var emptyString = ""               // 빈 문자열 리터럴
var anotherEmptyString = String()  // 초기화 문법
// 두 문자열 모두 비어있으며 서로 똑같다.

```

`isEmpty`의 불리언 속성을 체크하여 문자열 값이 비어있는지 여부를 확인할 수 있습니다.
```
if emptyString.isEmpty {
    println("여기엔 아무것도 보이지 않습니다.")
}
// prints 여긴 아무것도 보이지 않습니다."
```

## 문자열 가변성

특정 `String`을 변수에 할당하여(수정될 수 있는 경우) 수정(또는 변경)할 수 있는지를 나타내거나 상수(수정될 수 없는 경우)를 말합니다.
```
var variableString = "Horse"
variableString += " and carriage"
// variableString 은 "Horse and carriage" 입니다.

let constantString = "Highlander"
constantString += " and another Highlander"
// 컴파일 에러 - 상수 문자열은 변경될 수 없습니다.
```

> NOTE
> 이 방법은 Objective-C 또는 Cocoa에서 다른 방법으로 접근합니다. 문자열이 변경될 수 있는지를 나타내기 위해 두 개의 클래스 (`NSString` 또는 `NSMutableString`) 사이에서 선택할 수 있습니다.

## 문자열 값 타입 (Strings Are Value Types)

Swift의 `String` 타입은 값 타입입니다. 새 `String` 값을 만드는 경우에 상수 또는 변수에 할당되면 그 문자열 값이 함수나 메소드에 전달 될 때 복사됩니다. 각각의 경우에 기존의 `String` 값의 새 복사본이 전달되거나 복사되며 이는 원래의 버전이 아닙니다. 값 타입은 [Structurs and Enumerations Are Value Types]()를 참조하십시오.

>NOTE
이 동작은 Cocoa에 있는 `NSString` 과는 다릅니다. Cocoa에 있는 `NSString` 인스턴스를 생성할 때와 함수나 메소드에 전달하거나 변수에 할당 및 전달될 때 같은 단일 `NSString`에 대한 참조를 할당합니다. 특별히 요청하지 않는 한 문자열에 대해 어떠한 복사는 수행되지 않습니다.

Swift의 `String` 기본 복사 동작(copy-by-default)은 문자열 값이 함수나 메소드에의해 수행될 때 어디에서 오는지 상관없이 정확한 `String` 값을 소유하고 깨끗한지 확인합니다. 스스로 수정하지 않는 한 전달된 문자열이 수정되지 않는다는 것을 보장합니다. 

내부적으로 Swift의 컴파일러는 실제 복사가 반드시 필요한 경우에만 발생하도록 최적화하고 있습니다. 이 뜻은 문자열로 작업할 때 항상 좋은 성능을 의미합니다.

## 문자와 작업하기 (Working with Charaters)

Swift의 `String` 타입은 지정된 순서로 `Character` 값의 컬렉션을 나타냅니다. 각 `Character`의 값은 하나의 유니코드 문자를 나타냅니다. 각 `Character`에 대해 `for-in` 루프의 문자 반복을 사용하여 각각의 문자의 값에 접근할 수 있습니다.

```
for character in "Dog!🐶"{
	println(character)
}
// D
// o
// g
// !
// 🐶
```

`for-in` 루프에 대해서는 For Loops 를 참조하십시오 // 링크

또한, `Character` 타입 표시를 제공하여 단일 문자열 리터럴에서 독립(stand-alone) `Character` 상수나 변수를 만들 수 있습니다.

```
let yenSign: Character = "¥"
```

## 문자 세기 (Counting Characters)
문자열의 문자의 수를 검색하려면 전역 함수인 `countElements`를 호출하여 함수의 유일한 매개변수인 문자열을 전달합니다.

```
let unusualMenagerie = "Koala 🐨, Snail 🐌, Penguin 🐧, Dromedary 🐪"
println("unusualMenagerie has \(countElements(unusualMenagerie)) characters")
// prints "unusualMenagerie has 40 characters"
```

> NOTE
 다른 유니코드 문자와 같은 유니코드 문자의 다른 표현은 메모리의 저장된 다른 양을 필요로 할 수 있습니다. 이 때문에 Swift의 문자는 각 문자의 표현에서 동일한 양의 메모리를 차지하지 않습니다. 결과에 따라 문자열의 길이는 차례로 그 문자의 각각 반복하지 않고는 계산될 수 없다. 당신이 특히 긴 문자열 값으로 작업하는 경우 `CountElements` 기능이 해당 문자열에 대한 정확한 글자수를 계산하기 위해 문자열에서 문자 세기를 반복해야 한다는 것을 인식해야 합니다.
또한 `countElements`에 의해 반환된 문자 수는 항상 같은 문자가 포함되어있는 `NSSString`의 길이 속성과 동일하지 않습니다. 길이는 `NSString`을 기초로 한 문자열 UTF-16 표현 내의 16bit 유닛 숫자에 기반을 두고 문자열에서 유니코드 문자의 수에 기반을 두지는 않습니다.
이 사실을 반영하기 위해 길이 속성은 Swift가 `NSString` 문자열 값에 접근할 때 `utf16count`라고 합니다.

## 문자열 및 문자 합치기(Concatenating Strings and Characters)

`String` 및 `Character`를 덧셈 연산자(`+`)와 함께 추가하여 새로운 문자열(또는 연결된) 값을 만들 수 있습니다.

```
let string1 = "hello"
let string2 = "there"
let character1: Character = "!"
let character2: Character = "?"

let stringPlusCharacter = string1 + character1        // equals "hello!"
let stringPlusString = string1 + string2              // equals "hello there"
let characterPlusString = character1 + string1        // equals "!hello"
let characterPlusCharacter = character1 + character2  // equals "!?"
```
또한 덧셈 할당연산자(+=)로 기존의 `String` 변수에 `String`이나 `Character` 값을 추가할 수 있습니다.
```
var instruction = "look over"
instruction += sting2
// instriction 은 "look over there" 와 같습니다.

var welcome = "good mornig"
welcome += character1
// welcome 은 "good morning!" 과 같습니다.
```

> NOTE
`Character` 값은 하나의 문자만을 포함해야만 하기 때문에 기존의 `Character` 변수에 `String`이나 `Character`를 추가할 수 없습니다.

## 문자열 삽입

문자열 삽입은 상수, 변수, 리터럴 그리고 표현식을 혼합하여 이용 및 문자열 안에 문자 값을 포함하여 새로운 `String` 값을 만드는 방법입니다. 문자열 리터럴에 삽입된 각 항목은 백슬래시가 앞에 있으며 한 쌍의 괄호로 싸여있습니다.
```
let multiplier = 3
let message = "\(multiplier) times 2.5 is \(Double(multiplier) * 2.5)"
// message is "3 times 2.5 is 7.5"
```
위의 예에서 `multiplier`의 값은 `\(multiplier)` 문자열 리터럴로 삽입됩니다. 이 플레이스홀더는 실제 문자열 삽입이 평가될 때 `multiplier`의 실제 값으로 치환됩니다.

`multiplier`의 값은 큰 문자열 표현식 나중의 일부입니다. 이 표현식은 `Double(mutiplier) * 2.5` 의 값을 계산하고 문자열로 결과 (`7.5`)를 삽입됩니다. 이 경우에 문자열 리터럴 내부에 포함된 경우 표현은 `\(Double(multiplier) * 2.5)`로 기록됩니다.


> NOTE
문자열에 삽입된 괄호안에 쓰는 표현으로 이스케이프 큰 따옴표 (`"`) 또는 백 슬래시(`\`)와 캐리지 리턴 및 줄바꿈을 포함할 수 없습니다.

## 문자열 비교
Swift는 `String` 값을 비교하는 세가지 방법을 제공합니다 : 문자열 같음, 전위 같음, 후위 같음 // 디스커션에 올림

### String Equality 
두개의 `String` 값이 동일한 순서로 포함되어 있는 경우 두개의 문자열 값이 동일한 것으로 간주됩니다.

```
let quotation = "We're a lot alike, you and I."
let sameQuotation = "We're a lot alike, you and I."
if quotation == sameQuotation {
	pinrtln("These two strings are considered equal")
}
// prints "These two strings are considered equal"
```


### Prefix and Suffix Equality

문자열이 특정 문자열의 전위 또는 후위가 있는지를 확인하여 문자열의 `hasPrefix` 및 `hasSuffix` 메서드를 호출, `String` 타입의 단일 인수인 부울값을 각각 반환합니다. 두 가지 방법은 기본 문자열과 전위나 문자열 사이에 문자별 비교를 수행합니다. 두 가지 방법은 기본 문자열과 전위나 후위 및 문자열 사이의 문자별 비교를 수행합니다.


아래의 예는 _셰익스피어의 로미오와 줄리엣_ 의 처음 두 액트인 장면의 위치를 나타내는 문자열의 배열을 고려하였습니다.

```
let romeoAndJuliet = [
    "Act 1 Scene 1: Verona, A public place",
    "Act 1 Scene 2: Capulet's mansion",
    "Act 1 Scene 3: A room in Capulet's mansion",
    "Act 1 Scene 4: A street outside Capulet's mansion",
    "Act 1 Scene 5: The Great Hall in Capulet's mansion",
    "Act 2 Scene 1: Outside Capulet's mansion",
    "Act 2 Scene 2: Capulet's orchard",
    "Act 2 Scene 3: Outside Friar Lawrence's cell",
    "Act 2 Scene 4: A street in Verona",
    "Act 2 Scene 5: Capulet's mansion",
    "Act 2 Scene 6: Friar Lawrence's cell"
]
```
Act 1의 장면의 수를 `romeoAndJuliet` 배열에 `hasPrefix`를 사용하여 계산할 수 있습니다.

```
var act1SceneCount = 0
for scene in romeoAndJuliet {
    if scene.hasPrefix("Act 1 ") {
        ++act1SceneCount
    }
}
println("There are \(act1SceneCount) scenes in Act 1")
// prints "There are 5 scenes in Act 1"
```

마찬가지로 `hasSiffix` 메소드를 사용하여 Capulet's mansion and Friar Lawrence's cell의 장면의 수를 계산합니다.

```
var mansionCount = 0
var cellCount = 0
for scene in romeoAndJuliet {
    if scene.hasSuffix("Capulet's mansion") {
        ++mansionCount
    } else if scene.hasSuffix("Friar Lawrence's cell") {
        ++cellCount
    }
}
println("\(mansionCount) mansion scenes; \(cellCount) cell scenes")
// prints "6 mansion scenes; 2 cell scenes"
```

## 대문자와 소문자 문자열
`uppercaseString` 과 `lowercaseString` 속성을 가진 문자열에 대문자와 소문자 버전에 접근할 수 있습니다.

```
let normal = "Could you help me, please?"
let shouty = normal.uppercaseString
// shouty is equal to "COULD YOU HELP ME, PLEASE?"
let whispered = normal.lowercaseString
// whispered is equal to "could you help me, please?"
```

## 유니코드
유니코드는 국제 표준 인코딩 및 텍스트를 나타내는 것입니다. 유니코드는 표준화된 형태로 거의 모든 문자를 표시하고 텍스트 파일 또는 웹페이지와 같은 외부 소스로부터 해당 문자를 읽고 쓸 수 있습니다.

Swift의 `String` 및 `Character` 유형은 유니코드를 완벽하게 준수합니다. 아래에 설명으로 그들은 서로 다른 유니코드 인코딩의 숫자를 지원합니다.

### 유니코드 용어

유니코드의 모든 문자는 하나 이상의 유니코드 스칼라로 표현될 수 있습니다. 유니코드 스칼라는 문자 또는 수정에 대한 고유한 21bit(그리고 이름) 입니다, 이러한 `U+0061`나 `LOWERCASE LATINLETTER A("a")` 과 같이 `U+1F425`와 `FRONT-FACING BABY CHICK("🐥")` 같은 경우입니다.

유니코드 문자열이 텍스트 파일이나 다른 저장소에 기록될 때 이러한 유니코드 스칼라는 여러 유니코드 정의 중 하나의 형식으로 인코딩됩니다. 각 형식은 코드 단위로 알려진 작은 덩어리의 문자열을 인코딩합니다. 이들은 UTF-8 (8bit 코드 단위로 문자열을 인코딩) 형식과 UTF-16 (16bit 코드 단위로 문자열을 인코딩) 형식을 포함하고 있습니다.

### 문자열의 유니코드 표현

Swift는 문자열의 유니코드 표현에 접근할 수 있는 여러 가지 방법을 제공합니다.

유니코드 문자로 개별 `Character` 값에 접근을 `for-in` 구문으로 반복할 수 있습니다. 이 과정은 [문자와 작업하기]()에 설명되어있습니다.

또한, 유니코드 호환 표현 중 하나의 `String` 값에 접근:

- UTF-8 코드단위의 컬렉션(문자열의 `UTF-8` 속성에 접근)
- UTF-16 코드단위의 컬렉션 (문자열의 `UTF-16` 속성에 접근)
- UTF-21bit 유니코드 스칼라값의 컬렉션 (문자열의 `unicodeScalars` 속성에 접근)

아래의 각 예제에서는 D,O,G,! 및 (DOG FACE) 문자로 구성되어 있으며 문자열은 다른 표현을 보여줍니다. (`DOG FACE` 또는 `유니코드 스칼라 U+1F436)

```
let dogString = "Dog!🐶"
```

#### UTF-8

문자열의 UTF-8 속성을 반복하여 `String`의 `UTF-8` 표현에 접근할 수 있습니다.
`UTF8View` 타입의 속성은 부호 없는 8 bit(`UInt8`) 값의 모음이며 문자열의 UTF-8 의 각 바이트 문자열 표현입니다.:

```
for codeUnit in dogString.utf8 {
    print("\(codeUnit) ")
}
print("\n")
// 68 111 103 33 240 159 144 182
```

위의 예에서 첫 번째 네개의 십진수 `codeUnit` 값(`68`,`111`,`103`,`33`)은 그 문자 UTF-8로 표현과 동일한 `D`,`o`,`g` 그리고 `!`를 나타내며 이것들은 ASCII의 표현과 동일합니다. 마지막 네개의 `codeUnit`의 값(`240`,`159`,`144`,`182`)은 `DOG FACE`의 4바이트 UTF-8 표현입니다.

#### UTF-16

UTF-16 속성에 반복하여 UTF-16 표현에 접근할수 있습니다. `UTF16View` 타입의 속성은  부호 없는 16 bit(`UInt16`)값의 모음이며 문자열의 UTF-16의 각 바이트 문자열 표현입니다.:

```
for codeUnit in dogString.utf16 {
    print("\(codeUnit) ")
}
print("\n")
// 68 111 103 33 55357 56374

```

다시 처음 4가지 `codeUnit`의 값(`68`,`111`, `103`, `33`)은 UTF-16 코드 단위의 값은 UTF-8의 문자열 표현과 같은 값을 가지며 `D`,`o`,`g` 그리고 `!`의 문자를 표현합니다.

다섯 번째와 여섯 번째 `codeUnit`의 값(`55357` 과 `56374`)는 `DOG FACE` 문자를 UTF-16을 써로게이트 페어로 표현한것이다. 이 값은 `U+D83D`(십진수 값 `55357`)의 lead 써로게이트 값과 `U+DC36`(십진수 값 `56374`)의 trail 써로게이트 값입니다.  

#### 유니코드 스칼라

`unicodeScalars` 속성을 반복하여 `String` 값의 유니코드 스칼라 표현에 접근할 수 있습니다. 이 속성타입은 `UnicodeScalarView` 이며 `UnicodeScalar` 값 타입의 컬렉션입니다. 유니코드 스칼라 21bit 코드 포인트는 lead 써로게이트나 trail 써로게이트가 아닙니다.

각 `UnicodeScalar`는 값 속성(value property)이 있으며 이것은 스칼라의 21bit 값을 반환합니다. `UInt32` 안의 값을 표현한 것입니다.:

```
for scalar in dogString.unicodeScalars {
    print("\(scalar.value) ")
}
print("\n")
// 68 111 103 33 128054
```

`Value` 속성들은 처음 4개의 `UnicodeScalar` 값(`68`, `11`, `103`, `33`)을 다시 문자 `D`, `o`, `g` 와 `!`를 표현합니다.
다섯 번째이면서 마지막인 `UnicodeScalar`의 `Value` 속성은 십진법의 `12804`이며 16진법 `1F436`과 같습니다. 이는 `DOG FACE` 문자인 유니코드 스칼라 `U+1F436`과 같습니다.

`Value` 속성들을 쿼리하는 대신 각 `UnicodeScalar` 값은 또한 문자열 삽입으로 새로운 `String` 값을 생성하는데 사용될 수 있습니다.

```
for scalar in dogString.unicodeScalars {
    println("\(scalar) ")
}
// D
// o
// g
// !
// 🐶
```



