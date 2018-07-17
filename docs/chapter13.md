---
id: chapter13
title: 메서드 (Methods)
---
> Translator : 북극산펭귄 (say8425@gmail.com)
> Translator : 허혁 (hyukhur@gmail.com)

# Methods

메서드Method는 타입type에 의존적인 함수입니다. 모든 클래스와 구조체 그리고 이너멀레이션Enumeration은, 타입이 정해진 인스턴스Instance가 수행하는 작업을 캡슐화하는 인스턴스 메소드를 정의 할 수 있습니다. 또한 타입 자체에 관여하는 타입 메소드를 정의 할 수 있습니다. 이 타입 메소드는 오브젝티브-C에서의 클래스Class Method와 유사합니다.

## Instance Methods

인스턴스 메소드Instance Method는 특정 클래스, 구조체 혹은 이너멀레이션의 인스턴스에 속하는 함수입니다. 이것은 인스턴스 속성에 접근하고 수정하는 방법이나, 인스턴스의 용도에 관련된 기능을 지원합니다. [함수]()에서 설명된대로 인스턴스 메소드는 특히 함수와 동일한 문법을 가집니다.

여러분은 인스턴스 메소드를 해당 타입이 속한 괄호내에서 작성합니다. 인스턴스 메소드는 다른 인스턴스 메소드와 해당 타입의 속성에 대한 암시적 권한Implict access을 가지고 있습니다. 인스턴스 메소드는 오직 해당 타입이 속한 특정한 인스턴스에 의해서만 호출 될 수 있습니다. 이것은 속해있는 인스턴스 없이 독립적으로 호출 될 수 없습니다.

여기 작업을 수행한 횟수를 세는, 카운터`Counter`클래스를 정의한 간단한 예제가 있습니다:
```
class Counter {
  var count = 0
  func increment() {
	  count++
  }
  func incrementBy(amount: Int) {
	  count += amount
  }
  func reset() {
	  count = 0
  }
}
```
이 `Counter`클래스는 세가지 인스턴스 메소드를 정의합니다. 
 
* `increment` 1만큼 counter를 증가시킵니다.
* `incrementBy(amount: Int)` 특정한 정수값만큼 counter를 증가시킵니다.
* `reset` counter를 0으로 재설정합니다.

또한 `Counter`클래스는 현재 카운터 값을 추적하기 위해 변수 프로퍼티Property, `count`를 선언하였습니다.

당신은 프로퍼티와 같은 점 문법으로 인스턴스 메소드를 호출합니다:
```
let counter = Counter()
// 초기 counter값은 0입니다
counter.increment()
// counter값은 이제 1입니다
counter.incrementBy(5)
// counter값은 이제 6입니다
counter.reset()
// counter값은 이제 0입니다
```    
### 메소드를 위한 지역 및 외부 변수 이름

함수 매개 변수는 외부 변수 이름 섹션에서 설명한대로 지역 이름(함수 바디에서 사용될)과 외부 이름(함수가 호출될 때 사용될)을 가질 수 있습니다. 메소드 매개 변수 또한 그렇습니다. 매소드는 그저 타입에 의존적인 함수와 마찬가지이기 때문입니다. 그러나 이 둘의 지역 이름과 외부 이름의 작명법은 다름니다.

스위프트의 메소드는 오브젝티브-C에서 사용하던 것과 매우 유사합니다. 오브젝티브-C에서 그러하였듯이, 스위프트에서 메소드 이름은, 앞서살펴본 `Counter` 클래스 예제의 `incrementBy` 메소드와 같이, 형식적으로 첫번째 파라미터parameter가 `with`, `for` 또는 `by`와 같은 전치사를 사용합니다. 이 전치사가 사용되었다는 점은 메소드가 호출될 때 문장처럼 읽히는 것을 가능하게 만듭니다. 스위프트는 이 관습적으로 인정받은 작명법을 매서드 파라미터에 다른 기본 접근법을 사용하여 함수 파라미터보다 작성하기 쉽게 만들었습니다.

구체적으로, 스위프트는 메소드내 첫 번째 파라미터 이름은 기본적으로 지역 파라미터 이름으로 지정합니다, 그리고 두 번째 파라미터부터는 지역 파라미터와 외부 파라미터 둘다 지정합니다. 이 관습은 오브젝티브-C 메소드에서 형식적으로 이름을 짓고,  작성하던 것과 유사합니다. 그리고 파라미터 이름에 자격을 부여할 필요없이 알아보기 쉬운 메소드 호출을 만들 수 있습니다.

`IncrementBy`메소드가 좀 더 복잡하게 정의된 또 다른 버전의 이 `Counter`클래스를 보십시오:
```
class Counter {
  var count: Int = 0
  func incrementBy(amount: Int, numberOfTimes: Int) {
	  count += amount * numberOfTimes
  }
}
```
이 `incrementBy`메소드는 `amount`와 `numberofTimes`라는 두가지 파라미터를 가지고 있습니다. 기본적으로 스위프트는 `amount`를 지역 이름으로만 취급합니다, 하지만 `numberofTimes`는 지역 이름과 외부 이름 두가지 모두로서 취급합니다. 다음 예제와 같이 호출 할 수 있습니다:
```
let counter = Counter()
Counter.incrementBy(5, numberOfTimes: 3)
// Counter value is now 15
```
당신은 첫번째 인수값에 대해 외부 파라미터 이름을 정의 해줄 필요가 없습니다, 왜냐면 `incrementBy`라는 함수 이름에서 그것의 용도는 명확해졌기 때문입니다. 하지만 두번째 인수는 메소드가 호출 되었을 때, 그 용도를 명확히하기 위해서 외부 파라미터 이름으로 규정됩니다. 

이 기본적인 동작은 `numberOfTimes`파라미터 앞에 해쉬 심볼(`#`)을 붙임으로서 좀 더 효율적으로 취급할 수 있습니다:
```
Func incrementBy(amount: int, #numberOfTimes: int) {
	Count += amount * numberOfTimes
}
```
위의 기본적인 동작은 스위프트의 메소드 정의는 오브젝티브-C의 문법 스타일과 유사하게 쓰인다는 것을 의미하며, 자연스럽게 호출되고 표현적으로 풍부하게 표현된다는 것을 의미합니다. 

### 메소드의 외부파라미터 이름 수정법

기본적인 방법은 아니지만, 가끔씩 메소드의 첫번째 파라미터에 외부 파라미터 이름을 제공하는 것이 유용 할 수 있습니다. 당신은 직접 명시적으로 외부 이름을 추가 할 수 있으며, 혹은 해쉬 심볼을 첫번째 파라미터의 이름 앞에서 붙여서 지역 이름을 외부 이름과 같이 사용할 수 있습니다.

반대로, 메소드의 두번째 파라미터나 추가 파라미터에 대해 외부 이름을 제공하고 싶지 않으면, 언더바(\_)로 사용해 해당 파라미터를 명시적 외부 파라미터이름으로 오버라이드override해줄 수 있습니다.

### `self` 프로퍼티

모든 인스턴스 타입은 인스턴스 자체와 명확하게 동일한, 셀프라고 불리는 명시적 프로퍼티를 가지고 있습니다. 이 명시적 셀프 프로퍼티는, 자신이 속한 인스턴스 메소드내에서 현재 인스턴스를 참조하는 데 사용 할 수 있습니다.

다음 예제의 `increment`메소드는 그 와같이 작성되었습니다.
```
func increment() {
	self.count++;
}
```
실제로는 여러분이 코드에서 `self`를 작성해줄 필요가 별로 없습니다.  여러분이 명시적으로 `self`를 작성하지 않았다면, 스위프트는 여러분이 메소드내에서 알려진 프로퍼티나 메소드를 사용할 때마다, 현재 인스턴스의 프로퍼티나 메소드를 참조할 것을 상정하고 있습니다. 

이 가정은 `Counter`의 세가지 인스턴스 메소드 내부에서 (`self.count` 대신)`count`를 사용함으로서 실증되었습니다.

인스턴스 메소드의 파라미터 이름이 해당 인스턴트의 속성과 동일한 이름을 가진 경우, 이 규칙의 주요 예외가 발생합니다. 이렇게 된다면, 파라미터 이름은 우선적으로 좀 더 확실하게 속성을 참조할 필요가 있습니다. 여러분은 `self`속성을 확실하게 사용해서 파라미터 이름과 프로퍼티 이름을 구분지을 수 있습니다.

여기 `self`는 `x`라고 불리는 메소드 파라미터와 역시 `x`라고 불리는 인스턴스 파라미터 사이에서 명확하게 구분지어주고 있습니다.
```
struct Point {
	var x = 0.0, y = 0.0
	func isToTheRightOfX(x: Double) -> Bool {
		return self.x -> x
	}
}
let somePoint = Point(x: 4.0, y: 5.0)
if somePoint.isToTheRightOfX(1.0) {
	println(“This point to the right of the line where x == 1.0”)
}
// prints “This point is to the right of the line where x == 1.0”
```
`self` 접두사가 없다면, 아마도 스위프트는 두 `x` 모두 메소드 파라미터 `x`를 참조한다고 여깁니다.

### 인스턴스 메소드 안에서의 값타입 변경
구조체와 열거형는 값타입이다. 기본적으로 값타입의 프로퍼티는 인스턴스 메소드 안에서 변경될 수 없다.
그러나 만약 특정 메소드 안에서 구고체나 열거형을 변경할 필요가 있다면, 그 메소드에 변화(`mutating`)동작을 선택할 수 있다.  그러면 메소드는 자신 안에서 해당 프로터리를 변화(즉, 변경)시킬 수 있고, 적용된 모든 변경은 메소드가 끝나면 원본 구조체에 쓰여지게 된다. 메소드는 내포된 `self` 프로퍼티에 완전히 새로운 인스턴스를 할당할 수도 있다. 
어떤 메소드 `func`키워드 앞에 `mutating` 키워드를 둬서 이 동작을 선택할 수 있다.
```
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveByX(deltaX: Double, y deltaY: Double) {
        x += deltaX
        y += deltaY
    }
}
var somePoint = Point(x: 1.0, y: 1.0)
somePoint.moveByX(2.0, y: 3.0)
println("The point is now at (\(somePoint.x), \(somePoint.y))")
// prints "The point is now at (3.0, 4.0)
```
상단의 `Point`구조체는 `moveByX` 변화할 메소드로 정의했는데, 이는 `Point` 인스턴스를 특정 크기만큼 옮긴다. 그런 프로퍼티를 변경시킬 수 있게 만들기 위해서 `mutating` 키워드를 그 정의에 추가했다.
구조체 타입의 상수에 변화메소드를 호출할 수 없다는 것을 유의해라. 왜냐하면 비록 해당 프로퍼티가 변수형태로 되어 있어도 그 프로퍼티는 변경될 수 없기 때문이다. [상수 구조체 인스턴스의 저장속성]()에 설명되어 있다.
```
let fixedPoint = Point(x: 3.0, y: 3.0)
fixedPoint.moveByX(2.0, y: 3.0)
// this will report an error
```

### 변하는(`Mutating`) 메소드안에서 `self`에 할당하기
변하는(`Mutating`) 메소드는 암시적으로 `self`프로퍼티에 완전한 새 인스턴스를 할당할 수도 있다. 위 예제에서 보여준 `Point`는 아래와 같이 재 작성해볼 수 있다.
```
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveByX(deltaX: Double, y deltaY: Double) {
        self = Point(x: x + deltaX, y: y + deltaY)
    }
}
```
이 버전의 변하는 `moveByX` 메소드는 `x`값과 `y`값을 받아 대상 위치에 설정에 완전히 새로운 구조체를 만든다. 대안 버전의 메소드 호출 최종 결과는 이전 버전 호출에서와 정확하게 동일할 것이다.
열거형에서 변하는 메소드는 동일 열거형에서 다른 구성원이 될 수 있게 암시적 `self` 매개변수를 설정할 수 있다.
```
enum TriStateSwitch {
    case Off, Low, High
    mutating func next() {
        switch self {
        case Off:
            self = Low
        case Low:
            self = High
        case High:
            self = Off
        }
    }
}
var ovenLight = TriStateSwitch.Low
ovenLight.next()
// ovenLight is now equal to .High
ovenLight.next()
// ovenLight is now equal to .Off
```
이 예제에서 열거형은 3가지 상태 전이가 정의되어 있다. 3가지 전력 상태 (끔 `Off`, 낮음 `Low`, 높음 `High`) 사이의 전이 주기는 `next` 메소드가 호출되는 매회이다.

## 타입 메소드
위에서 설명한대로, 인스턴스 메소드는 특정타입의 인스턴스에서 호출되는 메소드이다. 타입 자체에서 호출하는 메소드 또한 정의할 수 있다. 이런 종류의 메소드를 타입 메소드라고 한다. 클래스를 위한 타입 메소드는 `func` 키워드 앞에 `class` 키워드를 써서 그리고 구조체와 열거형을 위한 타입 메소드는 `func` 키워드 앞에 `static` 키워드를 써서 지칭할 수 있다.

>노트
오브젝티브씨에서는 오브젝티브씨 클래스를 위한 타입 단계 메소드만을 정의할 수 있었다. 스위프트에서는 모든 클래스, 구조체, 열거형에 타입 단계 메소드를 정의할 수 있다. 개별 타입 메소드는 지원하는 타입에 대해 명시적으로 범위를 지정한다.

타입 메소드는 인스턴스 메소드처럼 점표기법(dot syntax)으로 호출한다. 그러나 타입에 대한 타입 메소드를 호출해야지 그 타입에 대한 인스턴스를 호출하는 것이 아니다. 여기에 어떻게 `SomeClass`라는 클래스에 타입 메소드를 호출하는지가 있다.
```
class SomeClass {
    class func someTypeMethod() {
        // type method implementation goes here
    }
}
SomeClass.someTypeMethod()
```
타입 메소드 본체 안에서는 암시적 `self` 프로퍼티는 타입에 대한 인스턴스가 아니라 타입 그 자체를 가르킨다. 구조체와 열거형에서는 마치 인스턴스 프로퍼티와 인스턴스 메소드 매개변수에서 그랬던 것처럼 `static` 프로퍼티와 `static` 메소드 매개변수 사이의 명확하게 하기 위해 `self`를 쓸 수 있다라는 것을 뜻한다.
좀 더 일반적으로, 어떤 타입 메소드의 본체 안에서 사용하는 제한없는 메소드와 프로퍼티는 다른 타입단계 메소드와 프로퍼티를 참조 할 것이다. 타입 메소드는 어떤 다른 타입 메소드의 이름과 함께 또다른 타입 메소드를 호출할 수 있다. 비슷하게, 구조체와 열거형 타입 메소드는 타입 이름 접두사 없이 정적 프로퍼티 이름을 사용해서 정적 프로퍼티에 접근할 수 이다.
아래 예제는 여러 레벨이나 게임 단계를 통해 플레이어의 진척도를 추적하는 `LevelTracker`이란 구조체를 정의한다. 어떤 레벨을 끝낼때마다 그 레벨은 장비에 있는 모든 플레이어를 풀 수 있다.  `LevelTracker` 구조체는 정적 프로퍼티와 메소드를 사용해 게임이 풀리는 레벨에 도달했는지 여부를 추적한다. 또한 개별 플레이어의 현재 수준에 대해 또한 추적한다.
```
struct LevelTracker {
    static var highestUnlockedLevel = 1
    static func unlockLevel(level: Int) {
        if level > highestUnlockedLevel { highestUnlockedLevel = level }
    }
    static func levelIsUnlocked(level: Int) -> Bool {
        return level <= highestUnlockedLevel
    }
    var currentLevel = 1
    mutating func advanceToLevel(level: Int) -> Bool {
        if LevelTracker.levelIsUnlocked(level) {
            currentLevel = level
            return true
        } else {
            return false
        }
    }
}
```
`LevelTracker` 구조체는 어떤 플레이어가 락을 풀고 도달한 가장 높은 레벨를 추적한다. 이 값은 `highestUnlockedLevel`이라고 불리는 정적 프로퍼티이다.
`LevelTracker`는 또한 `highestUnlockedLevel`프로퍼티와 동작하는 두가지 타입 함수를 정의한다. 첫번째는 `unlockLevel`이란 타입 함수인데 언제 새 레벨이 풀리는지에 상관없이 `highestUnlockedLevel`값을 갱신한다. 두번째는 `levelIsUnlocked`이라는 편리한 타입 함수로 만약 특정 레벨이 이미 풀렸으면 `true`를 반환한다. (이 타입 함수들이 `LevelTracker.highestUnlockedLevel`이라고 쓸 필요 없이 `highestUnlockedLevel` 정적 프로퍼티에 접근할 수 있다는 것을 알아둬라.
정적 프로퍼티와 타입 메소드에 추가로 `LevelTracker`은 개별 플레이어의 게임 전반에 걸친 진행 상태를 추적한다. 플레이어가 현재 진행하고 있는 레벨을 추적하기 위해 `currentLevel`이라는 인스턴스 프로퍼티를 사용한다.
`currentLevel` 프로퍼티를 관리하는데 도움이 되고자, `LevelTracker`은 `advanceToLevel`이라는 인스턴스 메소드를 정의했다. `currentLevel`를 업데이트 하기 전에 이 메소드는 요청 받은 새 레벨이 이미 풀렸는지 아닌지를 확인한다. `advanceToLevel` 메소드는 실제로 `currentLevel`에 설정할 수 있는지 아닌지를 알려주기 위해 `Boolean`값을 반환한다.
`LevelTracker` 구조체는 `Player` 클래스와 사용하는데, 아래 보여지는 것처럼, 개별 플레이어의 진행 상태를 추적하고 갱신한다.
```
class Player {
    var tracker = LevelTracker()
    let playerName: String
    func completedLevel(level: Int) {
        LevelTracker.unlockLevel(level + 1)
        tracker.advanceToLevel(level + 1)
    }
    init(name: String) {
        playerName = name
    }
}
```
`Player` 클래스는 플레이어의 진행상태를 추적하기 위해 `LevelTracker`의 새 인스턴스를 생성한다. 또한 `completedLevel`이란 메소드를 제공하는데 언제 플레이어가 특정 레벨을 완료했을 때 호출한다. 이 메소드는 모든 플레이어의 다음 레벨을 풀고 플레이어을 다음 레벨로 이동시키기 위해 진행 상태를 갱신한다. ( `advanceToLevel`의 `Boolean`반환값은 무시되는데 왜냐하면 레벨이란 이전줄의 `LevelTracker.unlockLevel`을 호출해서 풀렸는지를 알기 위함이기 때문이다.)
신규 플레이어를 위해 `Player` 클래스의 인스턴스를 만들어서 플레이어가 레벨 1을 달성 했을때 어떤일이 벌어지는지 보여주겠다.
```
var player = Player(name: "Argyrios")
player.completedLevel(1)
println("highest unlocked level is now \(LevelTracker.highestUnlockedLevel)")
// prints "highest unlocked level is now 2"
```
만약 게임에서 어떤 플레이어도 아직 풀지 못한 레벨로 옮기려는 두번째 플레이어를 생성한다면, 플레이어의 현재 레벨을 설정하려는 시도는 실패할 것이다.
```
player = Player(name: "Beto")
if player.tracker.advanceToLevel(6) {
    println("player is now on level 6")
} else {
    println("level 6 has not yet been unlocked")
}
// prints "level 6 has not yet been unlocked"
```
