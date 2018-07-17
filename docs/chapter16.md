---
id: chapter16
title: 초기화 (Intialization)
---
> Translator : Quartet ( ungsik.yun@gmail.com )

_초기화_는 클래스, 구조체, 또는 열거형의 인스턴스를 사용하기 위한 준비 과정입니다. 이 과정은 해당 인스턴스의 각각의 저장된 속성의 초기값을 설정하는 것과 그 외의 다른 설정 또는 새 인스턴스를 사용하기 전에 필요한 초기화를 합니다. 
이 초기화 과정을 이니셜라이저(initializer)를 정의함으로서 구현할 수 있습니다. 이니셜라이저는 특정 타입의 새 인스턴스를 만들때 호출될 수 있는 특수 메소드입니다. 다른 오브젝티브 C의 이니셜라이저와는 달리 스위프트의 이니셜라이저는 값을 반환하지 않습니다. 이니셜라이저의 주 역할은 새 인스턴스가 처음 사용되기 전에 잘못된 곳이 없이 초기화가 되었는지 보장하는 것입니다.
또한 클래스 타입의 인스턴스는 디이니셜라이저(deinitializer)를 정의 할 수 있습니다. 디이니셜라이저는 할당 해제되기 바로 직전에 맞춤 정리를 수행합니다. 디이니셜라이저에 대해 더 많은 정보를 원하시면 **Deinitialization**을 보세요.


## 저장 속성에 초기값 설정하기
클래스와 구조체의 인스턴스가 생성될때에 맞춰서 인스턴스내의 저장된 속성은 적절한 초기값으로 설정이 되어야 합니다. 저장된 속성은 정해지지 않은 상태로 남아있을 수 없습니다.
이니셜라이저를 통해 저장 속성에 초기값을 설정하거나, 속성의 정의의 일부분으로서 기본 속성값을 지정 할 수 있습니다. 이 행동들은 뒤따르는 섹션에 설명되어 있습니다.

>NOTE
저장 속성에 기본값을 지정하거나, 이니셜라이저에서 초기값을 설정할 때, 어떠한 속성 감시자(observer)도 호출하지 않고 속성의 값이 직접 설정 됩니다.

### 이니셜라이져
이니셜라이져는 특정 타입의 새 인스턴스를 만들 때 호출됩니다. 제일 단순한 형태의 이니셜라이저는, `init`키워드를 사용하며, 파라메터가 없는 인스턴스 메소드의 형태입니다.
밑의 예제는 `Fahrenheit` 구조체를 정의하여 화씨 단위로 표현된 온도를 저장합니다. `Fahrenheit` 구조체는 `double` 타입의 `temperature` 저장 속성 단 하나만을 가지고 있습니다.
```
struct Fahrenheit {
    var temperature: Double
    init() {
        temperature = 32.0
    }
}
var f = Fahrenheit()
println("The default temperature is \(f.temperature)° Fahrenheit")
// prints "The default temperature is 32.0° Fahrenheit"
```
이 구조체는 파라메터가 없는 단일 이니셜라이저 `init`을 정의합니다. 이 이니셜라이져는 저장된 온도값을  화씨 단위로 표현했을때의 물의 어는점인 32.0으로 초기화합니다.

### 기본 속성 값
위에 보인 것처럼 이니셜라이저 안에서 저장 속성의 초기값을 설정할 수 있습니다. 또 다른 방법은, 속성 선언의 일부로 기본 속성 값을 지정하는 것입니다. 속성을 정의할때 초기 값을 속성에 할당 하는 것으로 기본 속성 값을 지정 할 수 있습니다.

>NOTE
만약 속성이 언제나 똑같은 초기값을 가진다면, 이니셜라이저 안에서 값을 설정하기보다는 기본 값을 주는 것이 낫습니다. 결과적으로는 같지만, 기본값이 속성의 선언에 더 근접해서 속성의 초기화를 합니다. 이로써 더 짧고 명확한 이니셜라이저를 작성할 수 있게 하고, 기본 값에서 속성의 타입을 개발자가 유추할 수 있게 합니다. 또한 기본값은 이 장의 뒤에서 설명되겠지만, 기본 이니셜라이저의 장점을 취하는 것과 이니셜라이저 상속을 쉽게 합니다.

`temperatur` 속성을 선언할때 기본값을 제공하는 것을 통해, 위해서 보인 단순한 형태로 `Fahrenheit` 구조체를 다시 작성 할 수 있습니다.
```
struct Fahrenheit {
    var temperature = 32.0
}
```

## 사용자 정의 초기화
이 섹션에서 설명할 것은, 입력 파라메터와 옵셔널 속성 타입을 이용하거나, 상수 속성을 초기화 과정중에 변경하는 것으로 초기화 과정을 사용자가 정의하는 것 입니다.

### 초기화 파라메터
사용자 정의 초기화의 타입들과 값의 이름들을 정의하기 위해 이니셜라이저의 정의중 일부분으로 _초기화 파라메터_를 제공할 수 있습니다. 초기화 파라메터는 함수나 메소드의 파라메터와 같은 기능과 문법을 가지고 있습니다.
다음의 예제는 섭씨 단위로 온도를 표현하여 저장하는 `Celsius` 구조체를 정의합니다. `Celsius` 구조체는 `init(fromFahrenheit:)`과 `init(fromKelvin:)` 두개의 이니셜라이저를 구현하여 다른 온도 단위에서 값을 받아와 새 인스턴스를 초기화합니다.
```
struct Celsius {
    var temperatureInCelsius: Double = 0.0
    init(fromFahrenheit fahrenheit: Double) {
        temperatureInCelsius = (fahrenheit - 32.0) / 1.8
    }
    init(fromKelvin kelvin: Double) {
        temperatureInCelsius = kelvin - 273.15
    }
}
let boilingPointOfWater = Celsius(fromFahrenheit: 212.0)
// boilingPointOfWater.temperatureInCelsius is 100.0
let freezingPointOfWater = Celsius(fromKelvin: 273.15)
// freezingPointOfWater.temperatureInCelsius is 0.0
```
첫번째 이니셜라이저는 하나의 초기화 파라메터 `fromFahrenehit`를 외부 이름으로 가지고 `fahrenheit`를 지역 이름으로 가집니다. 두번째 이니셜라이저는 하나의 초기화 파라메터 `fromKelvin`을 외부 이름으로 가지고 `kelvin`을 지	역 이름으로 가집니다. 두 이니셜라이저 모두 하나의 인자(argument)를 섭씨 단위로 변환해 `temperatureInCelsius` 라는 속성에 값을 저장합니다.

### 지역 파라메터 이름과 외부 파라메터 이름
함수나 메소드의 파라메터처럼, 초기화 파라메터 또한 이니셜라이저의 안에서 쓰일 지역 이름과 이니셜라이저를 호출할때 쓸 외부 이름을 가질 수 있습니다.
하지만 이니셜라이저는 함수나 메소드처럼 괄호 앞에 있는 함수 이름으로 식별 가능한 이름을 가지고 있지 않습니다. 그러므로 이니셜라이저의 파라메터가 가지는 이름과 타입들을 어떤 이니셜라이저가 호출되는지 확인하는 특별히 중요한 역할을 합니다. 이 때문에 시위프트는 이니셜라이저의 사용자가 외부 이름을 지정하지 않은 모든 파라메터에 대해 자동 외부 이름을 부여합니다. 이 자동 외부 이름은 모든 초기화 파라메터에 해쉬 심볼(`#`)을 붙인 것처럼, 지역 이름과 똑같이 지정 됩니다.

>NOTE
만약 이니셜라이저의 파라메터의 외부 이름을 지정하고 싶지 않다면, 언더스코어 (`_`)를 해당 파라메터의 명시적 외부 이름으로 하여 위에 설명된 기본 행동을 덮어 씌우십시오.

다음 예제는 `red`, `green`, `blue`를 상수 속성으로 가지는 `Color` 구조체를 정의합니다. 이 속성들은 `0.0` 부터 `1.0` 사이의 값을 저장하여 색안의 빨강, 초록, 파랑의 양을 나타냅니다.

`Color` 구조체는 `Double` 타입의 적절하게 이름지어진 파라메터 3개를 가지는 이니셜라이저를 제공합니다.
```
struct Color {
    let red = 0.0, green = 0.0, blue = 0.0
    init(red: Double, green: Double, blue: Double) {
        self.red   = red
        self.green = green
        self.blue  = blue
    }
}
```
새 `Color` 인스턴스를 만들때, 색의 세가지 구성요수를 외부 이름으로 사용하여 이니셜라이저를 호출 할 수 있습니다.
```
let magenta = Color(red: 1.0, green: 0.0, blue: 1.0)
```
이니셜라이저를 호출할때 외부 이름을 사용하지 않고 호출 할 수 없음에 주의하십시오. 외부 이름은 이니셜라이저 안에 반드시 언제나 사용되어야 하며, 생략하게 되면 컴파일 타임 에러를 냅니다.
```
let veryGreen = Color(0.0, 1.0, 0.0)
// this reports a compile-time error - external names are required
```

### 옵셔널 속성 타입
만약 저장 속성이 논리적으로 "값 없음"을 갖는게 허용이 된다면 - 어쩌면 초기화 과정중에 설정이 될 수 없다거나, 어느 순간 "값 없음"을 갖는게 허용이 되거나 - 그 속성을 옵셔널 타입으로 선언하십시오. 옵셔널 타입 속성은 자동적으로 `nil` 값으로 초기화가 됩니다.  그렇게 함으로써 해당 속성은 의도된 "아직 값 없음"을 초기화 과정중에 가지게 됩니다.
이어지는 예제는 `response:`를 속성으로 갖는 `SurveyQuestion` 클래스를 정의합니다.
```
class SurveyQuestion {
    var text: String
    var response: String?
    init(text: String) {
        self.text = text
    }
    func ask() {
        println(text)
    }
}
let cheeseQuestion = SurveyQuestion(text: "Do you like cheese?")
cheeseQuestion.ask()
// prints "Do you like cheese?"
cheeseQuestion.response = "Yes, I do like cheese."
```
설문 조사의 대답은 설문을 하기 전까지 알 수 없습니다. 그래서 `response` 속성은 `String?` 타입 또는 "optinal `String`" 타입입니다. 이는 `SurveyQuestion`의 새 인트섵스가 초기화 되었을때 자동적으로 기본 값을 `nil`로 할당하여 "no string yet"을 뜻하게 됩니다.

### 초기화 과정중에 상수 속성을 변경하기
상수 속성이 명확한 값을 가지며 초기화가 끝나기 직전까지 상수 속성의 값을 초기화 과정중 언제라도 바꿀 수 있습니다.

>NOTE
클래스 인스턴스는 상수 속성의 값을 오직 초기화 과정중에 해당 클래스에 의해서만 바꿀 수 있습니다. 상수 속성은 자식(sub) 클래스에 의해 변경될 수 없습니다.

위 예제의 `SurveyQuestion` 클래스의 `text` 속성을 상수 속성으로 바꾸어 재작성 할 수 있습니다. 질문은 한번 `SurveyQuestion` 클래스가 생성되고 나면 변경 될 수 없다는 것을 알리기 위해서죠. `text` 속성이 지금은 상수라 할지라도, 클래스의 이니셜라이저 안에서는 여전히 변경될 수 있습니다.
```
class SurveyQuestion {
    let text: String
    var response: String?
    init(text: String) {
        self.text = text
    }
    func ask() {
        println(text)
    }
}
let beetsQuestion = SurveyQuestion(text: "How about beets?")
beetsQuestion.ask()
// prints "How about beets?"
beetsQuestion.response = "I also like beets. (But not with cheese.)"
```

## 기본 이니셜라이저
스위프트는 기본값을 모든 속성에 지정했지만 이니셜라이저를 가지지 않은 구조체나 베이스 클래스에 대해 _기본 이니셜라이저_를 제공합니다. 기본 이니셜라이저는 단순히 새 인스턴스를 만들고, 속성들을 각각의 기본값으로 설정합니다.
이 예제는 구매 목록안의 아이템의 이름, 수량, 구매 상태를 캡슐화하는 `ShoppingListItem` 클래스를 정의합니다.
```
class ShoppingListItem {
    var name: String?
    var quantity = 1
    var purchased = false
}
var item = ShoppingListItem()
```
`ShoppingListItem` 클래스의 모든 속성이 기본 값을 가지고 있고, 부모(super) 클래스가 없는 베이스  클래스이기 때문에, `ShoppingListItem`은 자동적으로 기본 이니셜라이저를 구현하여 새 인스턴스가 생길때 속성들을 기본값으로 설정해줍니다. (`name` 속성은 온셔널 `String` 속성이어서 별달리 값이 코드에 쓰여있지 않아도 자동적으로 기본 값으로 `nil`을 받습니다.) 위의 예제에서 `ShoppingListItem` 클래스는 기본 이니셜라이저와 `ShoppingListItem()`이라 쓰여진 것처럼 이니셜라이저 문법을 이용하여 새 클래스 인스턴스를 만드는데 사용합니다. 만들어진 새 인스턴스는 `item` 변수에 할당 됩니다.

### 구조체 타입의 멤버 단위 이니셜라이저
위에 언급된 기본 이니셜라이저 외에도, 구조체 타입은 자동적으로 _멤버 단위 이니셜라이저_를 부여받습니다. 구조체의 모든 저장 속성에 기본값이 제공되었지만 사용자 정의 이니셜라이저가 정의되지 않았을때 말이죠.
멤버 단위 이니셜라이저는 새 구조체 인스턴스의 멤버 속성을 초기화 하는 단축 표현(shorthand)입니다. 새 인스턴스의 속성들의 초기값은 멤버 단위 이니셜라이저의 이름을 통해 전달 될 수 있습니다.
밑의 예제는 `Size` 구조체를 `width`와 `height` 속성 두개를 정의합니다. 두 속성은 전부 `0.0`이 할담 됨으로써 `Double` 타입임이 암시됩니다.
두 속성 전부 기본값을 가지기에 `Size` 구조체는 자동적으로 `Size` 인스턴스를 초기화 할 수 있는 `init(width:heigh:)` 멤버 단위 이니셜라이저를 부여받게 됩니다.
```
struct Size {
    var width = 0.0, height = 0.0
}
let twoByTwo = Size(width: 2.0, height: 2.0)
```

## 값 타입의 이니셜라이저 대리 수행Delegation
이니셜라이져는 인스턴스 초기화 수행의 일부로 다른 이니셜라이저를 호출 할 수 있습니다. 이 과정은 _이니셜라이저 델리게이션_이라 하며, 여러 이니셜라이저 사이의 중복 코드를 피할 수 있게 합니다.
값 타입과 클래스 타입에 따라 이니셜라이져 델리게이션이 어떤 형태로 허용 되는가, 어떻게 작동하는지 그 규칙은 어떤가가 다릅니다. 구조체나 열거형과 같은 값 타입은 상속을 지원하지 않습니다. 그렇기에 이니셜라이저 델리게이션의 과정은 비교적 간단합니다. 그저 다른 이니셜라이저가 제공한 것만을 대리 수행(delegation)하면 되기 때문입니다. 하지만 클래스는 **상속**에서 설명한 것처럼, 다른 클래스에서 상속 받을 수 있습니다. 이는 곧 클래스는 상속 받은 저장 속성들이 초기화 과정중에 올바르게 할당 되었는지 보장해야 하는 추가적인 책임이 있다는 것을 뜻합니다. 이러한 책임은 밑의 **클래스 상속과 초기화**에 설명되어 있습니다.
값 타입에서 사용자 정의 이니셜라이저를 작성할때, 다른 같은 타입의 다른 이니셜라이저를 참조하려면 `self.init`을 사용해야 합니다. 이니셜라이저 안에선 오직 `self.init`만 호출 할 수 있습니다.
만약 값 타입의 사용자 정의 이니셜라이저를 정의한다면, 더 이상 해당 값 타입의 기본 이니셜라이저에 접근 할 수 없게 됩니다. (그게 구조체라면 멤버 단위 구조체 이니셜라이저도 포함합니다.) 
이 제약으로 인해, 필수적인 설정을 하는 이니셜라이저 대신 의도치 않게 자동 기본 이니셜라이저를 실행함으로써일어 날 수 있는 문제를 방지합니다.
>NOTE
만약 사용자 정의 값 타입이 기본 이니셜라이저와 멤버 단위 이니셜라이저, 그리고 사용자 정의 이니셜라이저를 동시에 쓰길 원한다면 이니셜라이져를 값 타입의 원래 구현의 부분으로 작성하기 보다 확장(extension)으로 작성하십시오. 자세한 정보는 **확장**을 보세요.

다음 예제는 사용자 정의 `Rect` 구조체를 정의하여 기하학적 사각형을 표현합니다. 이 예제는 `Size`와 `Point`, 두개의 지지(supporting) 구조체를 요구합니다. 두 구조체 모두 속성들의 기본값으로 `0,0`을 제공합니다.
```
struct Size {
    var width = 0.0, height = 0.0
}
struct Point {
    var x = 0.0, y = 0.0
}
```
`Rect` 구조체는 세가지 방법중 하나로 초기화 될 수 있습니다. 기본값인 0으로 초기화된 `origin`과 `size` 속성 값을 이용하여, 특정 기점(origin point)과 사이즈를 제공하여, 특정 중앙점과 사이즈를 제공하여. 이 초기화 옵션들은 `Rect` 정의 안에서 사용자 정의 이니셜라이저로서 표현됩니다.
```
struct Rect {
    var origin = Point()
    var size = Size()
    init() {}
    init(origin: Point, size: Size) {
        self.origin = origin
        self.size = size
    }
    init(center: Point, size: Size) {
        let originX = center.x - (size.width / 2)
        let originY = center.y - (size.height / 2)
        self.init(origin: Point(x: originX, y: originY), size: size)
    }
}
```
첫번째 `Rect` 이니셜라이저인 `init()`은 기능적으로 사용자 정의 이니셜라이저를 가지지 않을때의 기본 이니셜라이저와 똑같습니다. 이 이니셜라이저는 빈 몸체를 가지며, 빈 중괄호 한쌍 `{}`으로 표현됩니다. 또한 아무런 초기화도 수행하지 않습니다. 이 이니셜라이져를 호출하면 `Rect` 인스턴스를 반환하며, 그 인스턴스의 `origin`과 `size`는 모두 속성에서 정의된 기본값인 `Point(x: 0.0, y:0,0)`과 `Size(width: 0.0, height: 0.0)`입니다.
```
let basicRect = Rect()
// basicRect's origin is (0.0, 0.0) and its size is (0.0, 0.0)
```
두번째 `Rect` 이니셜라이져인 `init(origin:size:)`은 기능적으로 사용자 정의 이니셜라이저를 가지지 않을때의 멤버 단위 이니셜라이져와 동일합니다. 이 이니셜라이져는 단순히 `origin`과 `size` 인수를 알맞은 저장 변수에 할당합니다.
```
let originRect = Rect(origin: Point(x: 2.0, y: 2.0),
    size: Size(width: 5.0, height: 5.0))
// originRect's origin is (2.0, 2.0) and its size is (5.0, 5.0)
```
세번째 `Rect` 이니셜라이저인 `init(center:size:)`은 조금 더 복잡합니다.  `center` 포인트와 `size`에서 계산한 적절한 기점에서 시작하게 됩니다. 그리고 나면 `init(origin:size:)` 이니셜라이져를 호출( 혹은 대리)합니다. 그 이니셜라이져는 알맞은 새 기점과 사이즈 값을 저장합니다.
```
let centerRect = Rect(center: Point(x: 4.0, y: 4.0),
    size: Size(width: 3.0, height: 3.0))
// centerRect's origin is (2.5, 2.5) and its size is (3.0, 3.0)
```
`init(center:size:)` 이니셜라이저는 새 `origin`과 `size`값을 적절한 속성에 할당받게 할 수 있습니다. 하지만 `init(center:size:)` 이니셜라이저를 이용하는게 더 편하고 의도가 명확하며, 이미 있는 이니셜라이저가 제공하는 기능을 활용할 수 있는 장점이 있습니다.

>NOTE
`init()`과 `init(origin:size:)`을 작성하지 않고 위의 예제를 작성해보려면 **확장**을 보세요.

## 클래스 상속과 초기화
클래스의 모든 저장 속성은 - 부모 클래스에서 상속받은 어떠한 속성또한 포함하여 - 반드시 초기화 과정중에 초기 값을 할당받아야 합니다.
스위프트는 두 종류의 이니셜라이져를 제공하여 클래스 타입의 모든 저장 속성이 초기값을 갖게끔 보장합니다. 각각 지정 이니셜라이저와 편의 이니셜라이저라 합니다.

### 지정 이니셜라이저와 편의 이니셜라이저
_지정 이니셜라이저_는 클래스의 주 이니셜라이저입니다. 지정 이니셜라이저는 해당 클래스에서 접하는 모든 속성을 완전히 초기화하고, 적절한 부모 클래스 이니셜라이저를 호출하여 초기화 과정을 부모 클래스로 연쇄시킵니다.
클래스들은 매우 적은 수의 지정 이니셜라이저를 가지는 경향이 있으며, 보통의 경우 클래스는 오직 하나만 가집니다. 지정 이니셜라이저는 초기화가 이루어지는 곳의 "깔때기" 지점이며, 부모 클래스로 이어지는 초기화 과정 연쇄의 "깔때기" 지점입니다.
모든 클래스는 반드시 최소한 하나의 지정 이니셜라이저를 가져야합니다. 때때로, **자동 이니셜라이저 상속**에 설명된 것과 같이, 이 조건은 부모 클래스에서 하나 이상의 지정 이니셜라이저를 상속 받음에 따라 충족되는 경우가 있습니다.
_편의 이니셜라이저_는 클래스를 지탱하는 두번째 이니셜라이저입니다. 편의 이니셜라이저를 정의하여, 같은 클래스 내의 지정 이니셜라이저를 호출하는 편의 이니셜라이저를 만들 수 있습니다. 이 편의 이니셜라이져를 통해 호출하는 지정 이니셜라이저의 몇몇 파라메터를 기본 값으로 설정할 수 있습니다. 또한 편의 이니셜라이저를 정의하여 특정 쓰임새나 입력 값 타입에 대한 클래스 인스턴스를 만들 수 있습니다.
클래스가 필요로 하지 않는다면 편의 이니셜라이저를 제공할 필요는 없습니다. 편의 이니셜라이저는 보통 초기화 패턴을 단축하거나, 클래스의 의도를 명확하게 할때 만듭니다.

### 이니셜라이저 연쇄
지정 이니셜라이저와 편의 이니셜라이저의 관계를 단순화 하기 위해, 스위프트는 다음의 3개 규칙을 이니셜라이저간의 델리게이션에 적용합니다.

- **Rule 1**
    지정 이니셜라이저는 반드시 바로 위 부모 클래스의 지정 이니셜라이저를 호출한다.
- **Rule 2**
	편의 이니셜라이저는 반드시 같은 클래스 내의 호출 가능한 다른 이니셜라이저를 호출한다.
- **Rule 3**
    편의 이니셜라이저는 반드시 궁극적으로 지정 이니셜라이저를 호출하는 것으로 끝내야 한다.

간단히 기억하기 위한 방법은 이렇습니다.

- 지정 이니셜라이저는 반드시 _위_를 대리한다.
- 편의 이니셜라이저는 반드시 클래스 내부를 _가로질러_서 대리한다.

이 규칙은 다음의 그림으로 표현될 수 있습니다.

![initializerdelegation01_2x.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/initializerdelegation01_2x.png)

여기 부모 클래스는 하나의 지정 이니셜라이저를 가지고 있으며, 두개의 편의 이니셜라이저를 가지고 있습니다. 한 편의 이니셜라이저는 다른 편의 이니셜라이저를 호출하며, 그 이니셜라니저는 하나 있는 지정 이니셜라이저를 호출합니다. 이는 위의 규칙 2와 3을 충족시킵니다. 부모 클래스는 더이상의 부모 클래스를 가지지 않기에 규칙 1은 적용되지 않습니다.
그림의 서브 클래스는 두개의 지정 이니셜라이져와 하나의 편의 이니셜라이저가 있습니다. 편의 이니셜라이저는 반드시 두 지정 이니셜라이저중 하나의 이니셜라이저를 호출해야 합니다. 편의 이니셜라이저는 클래스 안의 다른 이니셜라이저만 호출 가능하기 떄문입니다. 이는 위의 규칙 2와 3을 만족시킵니다. 규칙 1을 만족시키기 위해, 두개의 지정 이니셜라이저는 반드시 부모 클래스에 하나 있는 지정 이니셜라이저를 호출해야 합니다.

>NOTE
이 규칙들은 각각의 클래스를 생성하는 방법에 영향을 주지 않습니다. 위 다이어그램의 어느 이니셜라이저라도 자기가 속해야 될, 완전히 초기화된 클래스 인스턴스를 만드는데 쓰일 수 있습니다. 이 규칙은 오직 클래스 구현의 작성에만 영향을 끼칩니다.

밑의 그림은 더 복잡한 4개의 클래스 간의 계층도를 나타냅니다. 이 그림은 지정 이니셜라이저가 어떻게 이 계층도의 클래스 초기화 과정에서 "깔때기"처럼 작동하고, 연쇄에서 클래스간의 관계를 단순화하는지 보여줍니다.

![initializerdelegation02_2x.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/initializerdelegation02_2x.png)


### 이 단계 초기화
스위프트의 초기화는 두 단계의 과정을 거칩니다. 첫번째 단계에서는 해당 클래스가 가지는 각각의 저장 속성에 초기값을 할당합니다. 모든 저장 속성의 초기 상태가 정해지고 나면, 두번째 단계가 시작됩니다. 두번째 단계에서는 클래스 인스턴스가 사용될 준비가 되기 전까지, (역주: 상속 트리 상에서의)각각의 클래스가 저장 속성을 사용자 정의할 기회를 가집니다.
이 단계 초기화 과정을 사용하는 것은 초기화를 안전하게 하면서도, 클래스 상속 계층 상에서 각각의 클래스가 완전한 유연성을 가지게 합니다. 이 단계 초기화는 속성 값이 초기화 되기 전에 접근되는 것을 방지하며, 다른 이니셜라이저에 의해 의도치 않게 다른 값이 설정되는 것을 방지합니다.

>NOTE
스위프트의 이 단계 초기화 과정은 오브젝티브 C의 초기화와 비슷합니다. 주요한 차이점은 첫번째 단계에 있습니다. 오브젝티브 C는 0이나 널(null) 값(`0` 또는 `nil`)을 모든 속성에 할당합니다. 스위프트의 초기화 흐름은 좀 더 유연하려 사용자 정의 초기값을 설정할 수 있게 해줍니다. 그리고 
`0`이나 `nil`이 기본값으로 유효하지 않은 타입에 대처할 수 있게 합니다.

스위프트의 컴파일러는 이 단계 초기화가 에러없이 완료 될 수 있게 4가지 안전 점검(safety check)을 수행합니다.

- **안전 점검 1**
    지정 이니셜러이저는 해당 클래스에서 도입된 모든 속성이 초기화 되었는지를 부모 클래스의 이니셜라이저를 대리하기 전에 확실히 하여야 합니다.

위에 언급된 것처럼, 객체를 위한 메모리는 저장 속성의 초기 상태가 알려져야 완전히 초기화 되었다고 간주합니다. 이 규칙을 만족시키기 위해서 지정 이니셜라이저가 초기화 연쇄를 위로 전달하기 전에 자신의 속성이 초기화 되었음을 확실히 해야합니다.

- **안전 점검 2**
    지정 이니셜라이저는 상속받은 속성에 값을 할당하기 전에 부모 클래스의 이니셜라이저를 대리 수행해야 합니다. 만약 그렇게 하지 않는다면, 지정 이니셜라이저가 할당한 새 값은 부모 클래스의 초기화 과정중에 덮어 씌워질 것입니다.

- **안전 점검 3**
    편의 이니셜라이저는 (역주: 클래스 상속 계층상) 같은 클래스 내부에서 정의된 속성을 포함한, 어떤 속성에라도 값을 할당하기 전에 다른 이니셜라이저를 대리 수행해야 합니다. 그렇게 하지 않을 경우 편의 이니셜라이저가 할당한 새 값은 해당 클래스의 지정 이니셜라이저에 의해 덮어씌워질 것입니다.
    

- **Safety check 4**
    이니셜라이저는 어떠한 인스턴스 메소드도 호출 할 수 없습니다. 어떠한 인스턴스의 속성도 읽을 수 없습니다. 또한 `self`를 초기화의 첫 단계가 끝나기 전에 참조할 수 없습니다.

클래스 인스턴스는 첫 단계가 끝나기 전까지는 완전히 유효하지 않습니다. 첫 단계가 끝나고 클래스 인스턴스가 유효하다고 알려져야만 속성에 접근 가능하고, 메소르를 호출 할 수 있습니다.

여기서 어떻게 위의 4가지 안전 점검에 의한 두 단계 초기화가 진행되는지 설명합니다.

**Phase 1**

- 클래스의 지정 이니셜라이저 또는 편의 이니셜라이저가 호출됩니다.
- 클래스 인스턴스를 위한 메모리가 할당됩니다. 메모리는 아직 초기화 되지 않았습니다.
- 클래스의 지정 이니셜라이저가 해당 클래스에 의해 도입된 모든 저장 속성이 값을 가졌음을 확인합니다. 해당 저장 속성을 위한 메모리는 이제 초기화 되었습니다.
- 지정 이니셜라이저는 부모 클래스 이니셜라이저로 같은 작업은 해당 클래스가 행하게 (순서를) 넘깁니다.
- 이 작업의 연쇄는 클래스 상속 계층의 맨 꼭대기에 다다를때까지 계속 됩니다
- 연쇄의 꼭대기에 다다르면, 연쇄의 마지막 클래스는 모든 저장 속성이 값을 가진 것을 확실히 합니다. 그러면 인스턴스의 메모리는 완전히 초기화 되었다고 간주되고, 첫 단계가 끝납니다.

**Phase 2**

- 연쇄의 꼭대기에서 거꾸로 내려오면서 작업을 하여, 연쇄안의 각각의 지정 이니셜라이저는 추가로 인스턴스를 사용자 정의할 수 있는 선택권이 있습니다. 이니셜라이저는 이제 `self`에 접근 가능하고, 자신의 속성을 변경하거나, 인스턴스 메소드를 호출하거나 할 수 있습니다.
- 마지막으로, 연쇄 안의 어떤 편의 이니셜라이저든 인스턴스를 사용자 정의 할 수 있는 선택권이 있으며, `self`를 이용하여 작업할 수 있습니다.

초기화 호출의 첫 단계가 어떻게 보이는지 가상의 자식 클래스와 부모 클래스를 이용하여 보여줍니다.
![twophaseinitialization01_2x.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/twophaseinitialization01_2x.png)

이 예제에서 초기화는 자식 클래스의 편의 이니셜라이저를 호출 하는 것으로 시작합니다. 이 편의 이니셜라이저는 아직 어떤 속성도 변경할 수 없습니다. 편의 이니셜라이저는 같은 클래스 안의 지정 이니셜라이저를 대리 실행합니다.
지정 이니셜라이저는 안전 점검 1에 따라, 모든 자식 클래스의 속성이 값을 가졌는지 확실히 합니다. 그 후에 지정 이니셜라이저는 부모 클래스의 지정 이니셜라이저를 불러 초기화 연쇄를 위로 올립니다.
부모 클래스의 지정 이니셜라이저는 부모 클래스의 속성이 모두 값을 가졌는지 확실히 합니다. 초기화를 해야할 부모 클래스가 없기 때문에, 더 이상의 대리 수행(delegation)은 필요치 않습니다.
부모 클래스의 속성들이 초기 값을 가지는 순간부터, 인스턴스의 메모리는 완전히 초기화가 되었다고 간주되며, 첫 단계는 끝이 납니다.

여기 두번째 단계가 같은 초기화 호출에서 어떻게 보이는지 설명이 있습니다.
![twophaseinitialization02_2x.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/twophaseinitialization02_2x.png)

부모 클래스의 지정 이니셜라이저는 인스턴스를 추가적으로 사용자 정의할 수 있는 기회가 있습니다. 물론 하지 않아도 됩니다.
부모 클래스의 지정 이니셜라이저가 종료되면, 자식 클래스의 이니셜라이저가 추가 사용자 정의를 수행할 수 있습니다. 이번에도 물론, 하지 않아도 됩니다.
마지막으로, 자식 클래스의 지정 이니셜라이저가 종료되면, 처음에 지정 이니셜라이저를 호출한 편의 이니셜라이저가 추가적인 사용자 정의를 수행합니다.

### 이니셜라이저 상속과 오버라이딩
오브젝티브 C의 자식 클래스와는 다르게, 스위프트의 자식 클래스는 부모 클래스의 이니셜라이저를 기본적으로 상속받지 않습니다. 스위프트의 이러한 접근 방식은 부모 클래스의 단순한 이니셜라이저가 자동적으로 더 복잡한 자식 클래스에 상속되어, 자식 클래스의 새 인스턴스를 생성할때 완전하지 않게 또는 올바르지 않게 초기화되는 것을 방지합니다.
만약 자식 클래스가 부모 클래스와 똑같은 이니셜라이저를 하나 이상 가지게 하고 싶다면, - 아마도 약간의 사용자 정의를 초기화 과정중에 수행할  -  같은 이니셜라이져를 사용자 정의 자식 클래스에서 오버라이드 하려 구현해 제공할 수 있습니다.
만약 오버라이드 하려는 이니셜라이저가 _지정_ 이니셜라이져라면, 그 구현을 자식 클래스에서 오버라이드합니다. 그리고 부모 클래스 버전의 이니셜라이저를 오버라이딩 버전에서 호출합니다.
만약 오버라이드 하려는 이니셜라이저가 _편의_ 이니셜라이저라면, 오버라이드한 이니셜라이저는 반드시 해당 자식 클래스 안의 지정 이니셜라이저를 호출해야 합니다. 위의 이니셜라이저 연쇄에서 설명한 것처럼 말이죠.
>NOTE
메소드, 속성, 서브스크립트와는 달리 이니셜라이는 오버라이드 할때 `override` 키워드가 필요하지 않습니다.

### 자동적 이니셜라이저 상속
위에서 언급한 것처럼, 기본적으로 자식 클래스는 부모 클래스의 이니셜라이저를 상속받지 않습니다. 하지만 부모클래스의 이니셜라이저가 특정 조건을 만족한다면 자동적으로 상속이 됩니다. 실제로 이것이 뜻하는 바는 다음과 같습니다. 많은 일반적인 경우에 이니셜라이져를 오버라이드 할 필요가 없습니다. 또한 상속 받는 것이 안전할때, 부모 클래스의 이니셜라이져를 최소의 노력으로 상속 받을 수 있습니다. 
자식 클래스에 의해 도입된 새로운 어떤 속성이라도 기본 값을 제공받는 다고 가정했을때, 다음의 두 규칙이 적용됩니다.

- **Rule 1**
    자식 클래스가 어떠한 지정 이니셜라이저도 정의하지 않았을 경우, 부모 클래스의 지정 이니셜라이저를 자동적으로 상속 받습니다.

- **Rule 2**
    자식 클래스가 부모 클래스의 _모든_ 지정 이니셜라이저를, 위의 규칙 1에 의해 지정 이니셜라이저를 상속받아서 구현하든가, 자식 클래스 정의의 일부로서 사용자 정의 구현으로 제공한다면, 부모 클래스의 모든 편의 이니셜라이저를 자동적으로 상속받게 됩니다.

이 규칙은 자식 클래스가 추가적인 편의 클래스를 더할때도 적용이 됩니다.

>NOTE
자식 클래스는 규칙 2를 만족시키는 것의 일부로서 부모 클래스의 지정 이니셜라이저를 자식 클래스의 편의 이니셜라이저로 구현할 수 있습니다.

### 지정 이니셜라이저와 편의 이니셜라이저의 문법
클래스의 지정 이니셜라이저는 값 타입을 위한 단순 이니셜라이저와 같은 방식으로 작성됩니다.
```
init(parameters) {
    statements
}
```
편의 이니셜라이저는 `init` 키워드 앞에 `convenience` 키워드를 공백으로 구분하여 위치하게 하는 것을 제외하면 같은 방식으로 작성됩니다.
```
convenience init(parameters) {
    statements
}
```

### 실제로 해보는 지정 이지셜라이저와 편의 이니셜라이저
다음 예제는 이정 이니셜라이저와 편의 이니셜라이저, 그리고 자동적 이니셜라이저 상속을 실제로 해몹니다. 이 예제는  `Food`, `RecipeIngredient` 그리고 `ShoppingListItem` 클래스들의 상속 계층을 정의합니다. 그리고 클래스들 간의 이니셜라이저가 어떻게 상호작용하는지 보여줄 것입니다.
상속 계층의 베이스 클래스는 `Food`입니다. 이 클래스는 음식의 이름을 캡슐화 합니다. `Food` 클래스는 `String` 타입의 `name` 속성을 도입합니다. 그리고 `Food` 인스턴스를 생성하는데 두 개의 이니셜라이저를 제공합니다.
```
class Food {
    var name: String
    init(name: String) {
        self.name = name
    }
    convenience init() {
        self.init(name: "[Unnamed]")
    }
}
```
밑의 그림은 `Food` 클래스의 이니셜라이저 연쇄가 어떻게 되는지 보여줍니다.
![initializersexample01_2x.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/initializersexample01_2x.png)

클래스는 기본 멤버 단위 이니셜라이저를 가지고 있지 않습니다. 그리고 `Food` 클래스는 단일 인자 `name`을 받는 지정 이니셜라이저를 제공합니다. 이 이니셜라이저는 특정 이름으로 `Food` 인스턴스를 생성하는데 사용될 수 있습니다.
```
let namedMeat = Food(name: "Bacon")
// namedMeat's name is "Bacon"
```
`Food` 클래스가 제공하는 `init(name: String)` 이니셜라이저는 `Food` 인스턴스의 모든 저장 속성이 완전히 초기화 되는 것을 보장하기에 _지정_ 이니셜라이저 입니다. `Food` 클래스는 부모 클래스를 가지지 않습니다. 또한 `init(name: String)` 이니셜라이저도 초기화를 완료하기 위해 `super.init()`을 호출할 필요가 없습니다.
`Food` 클래스는 인자가 없는 `init()` 편의 이니셜라이저 또한 제공합니다. `init()` 이니셜라이저는 `Food` 클래스의 `init(name: String)` 을 대리하여, `[Unnamed]`값을 가진 `name`을 이름의 기본 플레이스홀더(placeholder)로서 제공합니다.
```
let mysteryMeat = Food()
// mysteryMeat's name is "[Unnamed]"
```
클래스 상속 계층에서 두번째 클래스는 `Food` 클래스의 자식 클래스인 `RecipeIngredient` 입니다. `RecipeIngredient` 클래스는 요리 조리법의 재료를 모델링합니다. 이 클래스는 (`Food`에서 상속받은 `name` 속성에 더해) `Int` 타입의 `quantity` 속성을 도입합니다. 그리고 `RecipeIngredient`를 생성하기 위한 이니셜라이저 두개를 제공합니다.
```
class RecipeIngredient: Food {
    var quantity: Int
    init(name: String, quantity: Int) {
        self.quantity = quantity
        super.init(name: name)
    }
    convenience init(name: String) {
        self.init(name: name, quantity: 1)
    }
}
```
밑의 그림은 `RecipeIngredient`의 이니셜라이저 연쇄가 어떠한지 보여줍니다.
![initializersexample02_2x.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/initializersexample02_2x.png)

`RecipeIngredient` 클래스는 한개의 지정 이니셜라이저 `init(name: String, quantity: Int)`을 가지고 있습니다. 이 이니셜라이저는 새 `RecipeIngredient` 인스턴스의 모든 속성을 채우는데 쓰입니다. 이 이니셜라이저는  `RecipeIngredient` 클래스가 도입한 유일한 새 속성인  `quantity` 속성에 넘겨받은 `quantity` 인자를 할당하는 것으로 시작합니다. 그리고 위의 이니셜라이저는 `Food` 클래스의 `init(name: String)` 이니셜라이저를 대리 수행합니다. 이 과정은 위의 **이 단계 초기화**에 다온 안전 점검 1을 만족합니다.

`RecipeIngredient`는 편의 이니셜라이저 `init(name: String)` 또한 정의합니다. 이 이니셜라이저는 넘겨받은 이름을 가진 `RecipeIngredient`인스턴스를 생성합니다. 이 편의 이니셜라이저는 생성될때 명시적인 재료 수량이 정해지지 않았다면 갯수가 1개라고 간주합니다. 이 편의 이니셜라이저를 정의함으로써 `RecipeIngredient` 인스턴스를 빠르고 더 편하게 생성 할 수 있습니다. 그리고 재료 수량이 한개인 `RecipeIngredient` 인스턴스를 몇개 만들때의 코드 중복을 피하게 합니다. 이 편의 이니셜라이저는 단순히 클래스의 지정 이니셜라이저를 대리합니다. 

눈여겨 봐야 할 것은, `RecipeIngredient`이 제공하는 `init(name: String)` 편의 이니셜라이저가, `Food`의 지정 이니셜라이저인 `init(name: String)`과 같은 파라메터를 받는다는 것 입니다. `RecipeIngredient`가 이 이니셜라이저를 편의 이니셜라이저로서 제공한다고 해도, `RecipeIngredient`는 부모 클래스의 모든 지정 이니셜라이저를 제공한 셈이 됩니다. 따라서 `RecipeIngredient`는 자동적으로 부모 클래스의 편의 이니셜라이저를 모두 상속받게 됩니다.

이 예제에서 `RecipeIngredient`의 부모 클래스인 `Food`는 하나의 편의 이니셜라이저 `init()`을 제공합니다. 따라서 이 이니셜라이저는 `RecipeIngredient`에게 상속되게 됩니다. `init()`의 상속된 버전은 정확하게 `Food` 버전과 똑같은 기능을 합니다. 대리 수행하는 이니셜라이저`init(name: String)`을 `Food` 버전이 아니라 `RecipeIngredient` 버전을 사용하여 대리 한다는 것을 제외하면 말이죠.

이 세 이니셜라이저를 새 `RecipeIngredient` 인스턴스를 생성하는데 전부 사용할 수 있습니다.
```
let oneMysteryItem = RecipeIngredient()
let oneBacon = RecipeIngredient(name: "Bacon")
let sixEggs = RecipeIngredient(name: "Eggs", quantity: 6)
```
클래스 상속 계층에서 세번째이자 마지막 클래스는 `RecipeIngredient`의 자식 클래스인 `ShoppingListItem`입니다. `ShoppingListItem`는 구매 목록에 있는 조리법의 재료를 모델링합니다.

구매 목록에 있는 모든 품목(itme)들은 "unpurchased" 상태로 시작하게 됩니다. 이 사실을 표현하기 위해 `ShoppingListItem`은 기본값을 `false`로 가지는 `puchased` 불리언 속성을 도입합니다. `ShoppingListItem`은 또한 `ShoppingListItem` 인스턴스를 글로 설명하기 위해 산출한 `description` 속성을 추가합니다.
```
class ShoppingListItem: RecipeIngredient {
    var purchased = false
    var description: String {
    var output = "\(quantity) x \(name.lowercaseString)"
        output += purchased ? " ✔" : " ✘"
        return output
    }
}
```

>NOTE
`ShoppingListItem`은 `purchased`의 초기값을 제공하는 이니셜라이저를 정의하지 않습니다. 여기 모델링된 구매 목록에 추가되는 아이템은 언제나 구매되지 않은(unpurchased) 상태로 시작하기 때문입니다.

이 클래스가 도입한 모든 속성의 초기값을 제공하고, 어떤 이니셜라이저도 스스로 정의하지 않기 때문에 `ShoppingListItem`은 자동적으로 _모든_ 지정 이니셜라이저와 편의 이니셜라이저를 부모 클래스에서 상속받습니다.

밑의 그림은 전체 클래스 세개의 이니셜라이저 연쇄를 보여줍니다.

![initializersexample03_2x.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/initializersexample03_2x.png)

`ShoppingListItem` 인스턴스를 만드는데 세개의 상속받은 이니셜라이저 전부를 이용할 수 있습니다.
```
var breakfastList = [
    ShoppingListItem(),
    ShoppingListItem(name: "Bacon"),
    ShoppingListItem(name: "Eggs", quantity: 6),
]
breakfastList[0].name = "Orange juice"
breakfastList[0].purchased = true
for item in breakfastList {
    println(item.description)
}
// 1 x orange juice ✔
// 1 x bacon ✘
// 6 x eggs ✘
```
`breakfastList`라는 새 배열은 배열 문자(array literal)로 만들어져 세개의 새 `ShoppingListItem` 인스턴스를 담습니다. 배열의 타입은 `ShoppingListItem[]`가 될 것으로 추론하게 됩니다. 배열이 생성된 후, 배열의 첫번째 `ShoppingListItem`의 이름은 `"[Unnamed]"`에서 `"Orange juice"`로 바뀌게 됩니다. 그리고 구매가 되었다고 표시하게 됩니다. 배열 안의 각 품목의 설명을 출력하게 하여 기대한 대로 기본 상태가 설정되었음을 보일 수 있습니다.

## 클로저나 함수로 기본 속성 값을 설정하기

만약 저장 속성의 기본 값이 약간의 사용자 정의나 설정을 요구한다면, 클로저나 전역 함수를 이용하여 사용자 정의된 기본 값을 속성에 제공할 수 있습니다. 해당 속성이 속해있는 새 인스턴스가 초기화 될 때마다, 해당 클로저나 함수가 호출됩니다. 그리고 그 반환 값이 속성의 기본 값으로 사용됩니다.

그러한 클로저나 함수들은 대개 속성의 타입과 같은 임시 값을 만들어서, 그 값을 원하는 초기 상태로 맞춰주고, 그 임시 값을 속성의 기본 값으로 사용되게 반환합니다.

이 예제는 클로저가 어떻게 속성의 기본값을 제공할 수 있게 되는지 전체적인 뼈대를 보여줍니다.
```
class SomeClass {
    let someProperty: SomeType = {
        // create a default value for someProperty inside this closure
        // someValue must be of the same type as SomeType
        return someValue
        }()
}
```
클로저의 닫는 중괄호 바로 뒤에 빈 괄호 한쌍이 있는 것에 주의해 주십시오. 이는 스위프트에게 클로저를 즉시 실행 시키라고 지시합니다. 만약 이 괄호를 생략한다면 클로저의 반환 값이 아니라, 클로저 그 자체를 속성에 할당하려 시도하는 것이 됩니다.

>NOTE
만약 클로저를 이용하려 속성을 초기화 하려고 한다면 다음을 기억해 두십시오. 클로저의 실행이 된 시점에서는 인스턴스의 나머지 부분은 아직 초기화가 되지 않은 상태입니다. 이는 클로저 안에서 다른 속성 값에 접근 할 수 없다는 것을 뜻합니다. 속성들이 기본값을 가지고 있다고 해도 말이죠. 또한 암시적 `self` 속성을 사용하거나, 인스턴스의 다른 메소드를 호출 할 수 없습니다.

이 예제는 `Checkerboard` 구조체를 정의하여 _Draughts_라고도 알려진 _체커_ 게임의 보드를 모델링합니다.

![checkersboard_2x.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/checkersboard_2x.png)

_체커_ 게임은 흑백 칸이 번갈아 있는 10 * 10 판 위에서 플레이합니다. 이 게임판을 표현하기 위해 `Checkerboard` 구조체는 길이가 100이고, `Bool` 값을 가지는 `boardColors`라는 단일 속성을 가집니다. 배열에서 `ture` 값은 검은 칸을 표현하고, `false` 값은 흰색 칸을 표현합니다. 배열의 첫번째 아이템은 게임판에서 제일 좌상단의 칸을 표현하고, 마지막 아이템은 제일 게임판에서 우하단의 칸을 표현합니다.

`boardColors` 배열은 색상 값을 설정하기 위해 클로저를 사용하여 초기화가 됩니다.
```
struct Checkerboard {
    let boardColors: Bool[] = {
        var temporaryBoard = Bool[]()
        var isBlack = false
        for i in 1...10 {
            for j in 1...10 {
                temporaryBoard.append(isBlack)
                isBlack = !isBlack
            }
            isBlack = !isBlack
        }
        return temporaryBoard
        }()
    func squareIsBlackAtRow(row: Int, column: Int) -> Bool {
        return boardColors[(row * 10) + column]
    }
}
```
새 `Checkerboard` 인스턴스가 생성될때, 해당 클로저가 실행되어 `boardColors`의 기본 값이 계산되고 반환됩니다. 위 예제에서 보이는 클로저는 게임판 위의 각각의 칸에 알맞은 색을 계산하여 임시 배열인 `temporaryBoard`에 설정합니다. 그리고 설정이 끝나면 이 임시 배열은 클로저의 반환값으로서 반환이 됩니다. 이 반환된 배열 값은 `boardColors` 에 저장이 되고, 기능성 함수 `squareIsBlackAtRow`에 의해 조회 될 수 있습니다.
```
let board = Checkerboard()
println(board.squareIsBlackAtRow(0, column: 1))
// prints "true"
println(board.squareIsBlackAtRow(9, column: 9))
// prints "false"
```
