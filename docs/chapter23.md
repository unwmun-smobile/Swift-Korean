---
id: chapter23
title: 프로토콜
---
*프로토콜*은 특정한 일이나 기능의 일부에 대한 메소드나 속성이나 다른 요구사항들의 전체적인 모습을 정의한다.
실제로 이런 요구사항들의 구현을 제공하지는 않고, 그 구현이 어떻게 보일지에 대해 명시한다.
이 요구사항들을 실제로 구현된 클래스, 구조체, 열거형 등에 그 프로토콜이 *적용*될 수 있다.
프로토콜의 요구사항을 만족하면 어떤 타입이라도 그 프로토콜에 *일치한다(conform)*라고 말한다.


프로토콜은 특정한 인스턴스 속성들, 인스턴스 메소드들, 타입 메소드들, 연산자들, 인덱스참조(subscript) 등을 갖는 타입을 가져야한다.

### 프로토콜 문법
프로토콜을 클래스, 구조체, 열거체와 매우 비슷한 방법으로 정의한다.

``` swfit
protocol SomeProtocol {
    // 프로토콜 정의가 여기 온다
}
```

타입을 정의하는 곳에서 타입의 이름 뒤에 콜론(:)으로 구분해서 프로토콜의 이름을 써서 프로토콜을 커스텀 타입에 적용시킨다. 여러 프로토콜을 쉼표(,)로 구분해서 사용할 수 있다.

``` swift
struct SomeStructure: FirstProtocol, AnotherProtocol {
    // 구조체 정의가 여기 온다
}
```

클래스가 부모를 가질 때는 프로토콜들 앞에 부모 클래스를 명시하고 쉼표로 구분해서 적용한다.

``` swift
class SomeClass: SomeSuperclass, FirstProtocol, AnotherProtocol {
    // 클래스 정의가 여기 온다
}
```

### 속성 요구사항
프로토콜은 특정한 이름과 속성을 갖는 인스턴스 속성과 타입 속성을 제공하는 타입이 될 수 있다. 프로토콜에는 이 속성이 저장된 속성이어야하는지 계산된 속성이어야 하는지에 대해 명시하지 않는다. 단지 속성의 이름과 타입만 명시할 뿐이다. 또한 각 속성에 대해 읽기(gettable)인지 읽기/쓰기(gettable/settable)가 필요한지 명시할 수 한다.

프로토콜의 속성에 읽기나 읽기/쓰기에 대한 명시가 있다면 그 속성은 저장된 상수값이나 읽기전용(read-only)의 계산된 값을 넣을 수 없다.
만약 읽기가 필요하다고만 명시가 되어있고 어떤 종류의 속성도 가능하며 필요하면 읽기를 만들어도 괜찮다.

속성 요구사항은 항상 `var` 키워드가 앞에 있는 변수 속성으로 선언된다. 읽기/쓰기 속성은 타입 뒤에 `{ get set }`을 써서 명시하며, 읽기는 `{ get }`으로 명시한다.

``` swift
protocol SomeProtocol {
    var mustBeSettable: Int { get set }
    var doesNotNeedToBeSettable: Int { get }
}
```

타입 속성은 `class` 키워드를 붙여서 정의할 수 있다. 구조체나 열거형에서 구현할 때는 `static`을 붙이면 된다.

``` swift
protocol AnotherProtocol {
    class var someTypeProperty: Int { get set }
}
```

인스턴스 속성 하나만 필요로 하는 프로토콜 예제가 있다.

``` swift
protocol FullyNamed {
    var fullName: String { get }
}
```

`FullyNamed` 프로토콜은 이름이 맞으면 종류에 관계없는 속성을 정의한다. 어떤 `종류`여야하는지 명시하지는 않았고 그저 풀네임을 젱고할 수만 있으면 된다. `String` 타입의 읽기 가능한 `fullName`이라는 인스턴스 속성을 가진 `FullNamed`라는 요구사항만 명시되어있다.

`FullyNamed` 프로토콜이 적용되어있고 일치하는 간단한 구조체 예제다.

``` swift
struct Person: FullyNamed {
    var fullName: String
}
let john = Person(fullName: "John Appleseed")
// john.fullName is "John Appleseed’
```

이 예제에서는 `Person`이라고 불리는 구조체를 정의했고, 특정한 이름을 갖는다고 나타나있다. `FullyNamed` 프로토콜을 정의 첫번째 줄에 적용한 것이 보인다.


`Person`의 인스턴스들은 `String` 타입의 `fullName` 속성 하나를 갖는다. `FullNamed` 프로토콜의 요구사항 하나와 일치하며, `Person`이 확실하게 프로토콜에 일치한다는 것을 의미한다. (스위프트에서는 프로토콜의 요구사항이 채워지지 않으면 컴파일타임에 에러를 낸다.)


조금 더 복잡한 클래스가 있고, `FullNamed` 프로토콜을 적용했고 일치한다.

``` swift
class Starship: FullyNamed {
    var prefix: String?
    var name: String
    init(name: String, prefix: String? = nil) {
        self.name = name
        self.prefix = prefix
    }
    var fullName: String {
    return (prefix ? prefix! + " " : "") + name
    }
}
var ncc1701 = Starship(name: "Enterprise", prefix: "USS")
// ncc1701.fullName is "USS Enterprise’
```

이 클래스에서는 계산된 읽기전용 속성으로 `fullName` 속성을 구현했다.
각 `Starship` 클래스 인스턴스는 `name`을 필수로 `prefix`를 옵션으로 갖는다. `prefix` 값이 존재하면 `name`의 앞에 붙여서 우주선의 풀네임을 만들어 `fullName` 속성이 된다.


### 메소드 요구사항

프로토콜은 일치하는 타입을 구현하기 위해 인스턴스 메소드들과 타입 메소드들을 요구사항으로 명시할 수 있다.
중괄호나 메소드 구현체(body)만 없을 뿐, 일반적인 인스턴스 메소드나 타입 메소드를 정의하는 것과 정확히 같은 방법으로 정의된다.
일반적인 메소드와 같은 규칙으로 가변길이의 변수도 가능하다.

> 노트
> 
> 프로토콜은 일반적인 메소드들과 같은 문법을 사용하지만 인자로 기본값을 명시할 수 없다.

프로토콜에서 타입 속성을 정의할 때처럼 `class` 키워드를 타입 메소드 앞에 붙여주면 된다. 구조체나 열거형에서 구현할 때는 `static`을 붙여주면 된다.

``` swift
protocol SomeProtocol {
    class func someTypeMethod()
}
```

인스턴스 메소드 하나만 있는 프로토콜의 예제다.

``` swift
protocol RandomNumberGenerator {
    func random() -> Double
}
```

`RandomNumberGenerator` 프로토콜은 `Double`을 리턴하는 `random` 인스턴스 메소드를 갖는 어떤 타입에도 일치할 수 있다. (프로토콜에서는 명시되지 않았지만 `0.0`에서 `1.0` 사이의 값을 리턴할 것이라고 추정된다.)

`RandomNumberGenerator` 프로토콜만으로는 어떻게 난수를 생성할지에 대한 정보가 없다. 단지 새로운 난수를 만들어서 제공하는 발생기를 필요로 할 뿐이다.

`RandomNumberGenerator` 프로토콜을 적용하고 일치하는 클래스 구현체가 있다. *선형 합동 생성기(linear congruential generator)*라는 의사난수 생성 알고리즘을 구현했다.

``` swift
class LinearCongruentialGenerator: RandomNumberGenerator {
    var lastRandom = 42.0
    let m = 139968.0
    let a = 3877.0
    let c = 29573.0
    func random() -> Double {
        lastRandom = ((lastRandom * a + c) % m)
        return lastRandom / m
    }
}
let generator = LinearCongruentialGenerator()
println("Here's a random number: \(generator.random())")
// prints "Here's a random number: 0.37464991998171"
println("And another one: \(generator.random())")
// prints "And another one: 0.729023776863283"
```

### 변이(mutating) 메소드

가끔 메소드에서 자신의 인스턴스를 수정(혹은 *변이*)할 필요가 있다. 밸류 타입(즉, 구조체와 열거형)의 인스턴스 메소드에서 메소드의 `func` 앞에 `mutating` 키워드를 써서 소속된 인스턴스를 바꾸거나 인스턴스의 속성을 수정할 수 있게 명시한다. 이 과정은 [인스턴스 메소드 내에서 밸류 타입의 수정](#)에서 설졍되어있다.

프로토콜이 적용된 타입의 인스턴스를 변이할 수 있다고 인스턴스 메소드에 명시하려면 프로토콜 정의세ㅓ `mutating` 키워드를 추가하면 된다. 이 프로토콜이 적용된 구조체와 열거형은 요구사항을 만족한다.

> 노트
> 
> 프로토콜을 `mutating`이라고 명시하면 클래스에서 메소드를 구현할 때는 `mutating` 키워드를 쓰지 않아도 된다.
> `mutating` 키워드는 구조체와 열거형에서만 쓰인다.

아래에는 `Togglable`이라는 프로토콜 예제인데, `toggle`이라는 인스턴스 메소드 하나만 정의되어있다. 이름에서 알 수 있듯 `toggle` 메소드는 보통 타입의 속성을 변환하는 것인데 프로토콜에 일치하는 타입의 속성을 토글하거나 반전한다.

`toggle` 메소드는 `Togglable` 프로토콜 정의에서 `mutating` 키워드로 이 메소드를 호출했을 때 인스턴스의 상태가 변이될 것을 예상할 수 있다.

``` swift
protocol Togglable {
    mutating func toggle()
}
```

`Togglable` 프로토콜을 구조체나 열거형으로 구현하려면, `mutating`이 명시된 `toggle` 메소드를 구현해야 프로토콜에 일치할 수 있다.

아래 예제는 `OnOffSwitch`라는 열거형이다. 이 열거형은 `On`과 `Off` 두가지 상태 사이를 토글한다. 열거형의 `toggle` 구현체는 `Togglable` 프로토콜의 요구사항에 맞게 `mutating`이 명시되어 있다.

``` swift
enum OnOffSwitch: Togglable {
    case Off, On
    mutating func toggle() {
        switch self {
        case Off:
            self = On
        case On:
            self = Off
        }
    }
}
var lightSwitch = OnOffSwitch.Off
lightSwitch.toggle()
// lightSwitch은 이제 .On과 같다.
```

### 타입으로서의 프로토콜
프로토콜은 그 자체로 어떤 기능도 갖고 있지 않다. 하지만 어떤 프로토콜도 코드에서 다른 타입처럼 쓰일 수 있다.

왜냐하면 프로토콜도 타입이므로 다른 타입들이 쓰이는 곳에서 사용될 수 있다.
- 함수, 메소드, 생성자에서 인자의 타입 혹은 리턴 타입으로
- 상수, 변수, 속성의 타입으로
- 배열, 사전, 다른 컨테이너에서 요소의 타입으로
사용될 수 있다.

> 노트
> 
> 프로토콜이 타입이므로 스위프트의 다른 타입(`Int`, `String`, `Double`같은)처럼 이름을 대문자(`FullyNamed`나 `RandomNumberGenerator`처럼)로 사용할 수 있다.

타입으로 프로토콜을 사용하는 예제다.

``` swift
class Dice {
    let sides: Int
    let generator: RandomNumberGenerator
    init(sides: Int, generator: RandomNumberGenerator) {
        self.sides = sides
        self.generator = generator
    }
    func roll() -> Int {
        return Int(generator.random() * Double(sides)) + 1
    }
}
```

이 예제에서는 *n*면체의 `Dice`라는 새로운 클래스가 정의되어있다. `Dice`의 인스턴스는 면을 얼마나 가지고 있는지를 나타내는 `sides`라는 정수 속성과 주사위를 굴렸을 때 난수를 생성해주는 `generator` 속성을 가지고 있다.

`generator` 속성은 `RandomNumberGenerator` 타입의 속성이다. 그러므로 `RandomNumberGenerator` 프로토콜을 적용한 *어떤* 타입의 인스턴스라도 할당할 수 있다.

`Dice`는 초기 상태를 설정하는 생성자도 가지고 있다. 생성자는 `RandomNumberGenerator` 타입의 `generator`를 인자로 받는다.
새로운 `Dice` 인스턴스를 만들 때 프로토콜에 일치하는 어떤 타입도 인자로 넘길 수 있다.

`Dice`는 하나의 인스턴스 메소드 `roll`이 있는데, 1에서 면의 수 사이에 해당하는 정수를 리턴한다. 이 메소드에서는 생성기의 `random` 메소드를 호출해서 `0.0`과 `1.0` 사이의 난수를 받아 정확한 범위의 값을 만든다. `generator`가 `RandomNumberGenerator`를 적용하고 있기 때문에 확실하게 `rondom` 메소드를 가지고 있다.


여기 `LinearCongruentialGenerator` 인스턴스를 난수생성기로 받는 6면체의 `Dice` 클래스가 어떻게 사용되는지 예제가 있다.

```
var d6 = Dice(sides: 6, generator: LinearCongruentialGenerator())
for _ in 1...5 {
    println("랜덤한 주사위값은 \(d6.roll())")
}
// 랜덤한 주사위값은 3
// 랜덤한 주사위값은 5
// 랜덤한 주사위값은 4
// 랜덤한 주사위값은 5
// 랜덤한 주사위값은 4
```

### 위임 (Delegation)
위임은 클래스나 구조체가 다른 타입의 인스턴스에게 책임의 일부를 넘길(혹은 *위임할*) 수 있는 디자인 패턴이다. 이 디자인 패턴에서는 위임된 책임을 캡슐화하는 프로토콜을 정의하는데, 거기에 일치하는 타입(대리자delegate로 알려진)은 위임받은 기능이 있다고 보장된다. 위임은 특정 액션에 대해 응답하거나, 외부에서 온 정보가 어떤 타입인지에 관계없이 데이터를 처리할 때 사용할 수 있다.

아래에 주사위를 사용한 보드게임에 두가지 프로토콜이 정의되어있다.

```swift
protocol DiceGame {
    var dice: Dice { get }
    func play()
}
protocol DiceGameDelegate {
    func gameDidStart(game: DiceGame)
    func game(game: DiceGame, didStartNewTurnWithDiceRoll diceRoll: Int)
    func gameDidEnd(game: DiceGame)
}
```

`DiceGame` 프로토콜은 주사위를 포함하는 어떤 게임에도 적용할 수 있는 프로토콜이다. `DiceGameDelegate` 프로토콜은 `DiceGame`의 진행을 기록할 수 있는 어떤 타입에도 적용할 수 있는 프로토콜이다.

앞서 [흐름 제어](#)에서 소개되었던 *뱀과 사다리*의 수정 버전이다. 이 버전에서는 주사위 굴리기를 위해 `Dice` 인스턴스를 사용한 `DiceGame` 프로토콜을 적용했고 과정을 `DiceGameDelegate`에 알리기 위해 `DiceGame` 프로토콜을 적용했다.

``` swift
class SnakesAndLadders: DiceGame {
    let finalSquare = 25
    let dice = Dice(sides: 6, generator: LinearCongruentialGenerator())
    var square = 0
    var board: Int[]
    init() {
        board = Int[](count: finalSquare + 1, repeatedValue: 0)
        board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
        board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
    }
    var delegate: DiceGameDelegate?
    func play() {
        square = 0
        delegate?.gameDidStart(self)
        gameLoop: while square != finalSquare {
            let diceRoll = dice.roll()
            delegate?.game(self, didStartNewTurnWithDiceRoll: diceRoll)
            switch square + diceRoll {
            case finalSquare:
                break gameLoop
            case let newSquare where newSquare > finalSquare:
                continue gameLoop
            default:
                square += diceRoll
                square += board[square]
            }
        }
        delegate?.gameDidEnd(self)
    }
}
```

*뱀과 사다리* 게임에 대한 설명을 원하면 [흐름 제어](#) 챕터의 [break](#) 섹션에 나와있다.

이 버전에서는 `DiceGame`을 적용한 `SnakesAndLadders`라는 클래스로 이루어졌다. `dice` 속성과 `play` 메소드을 가지고 있어 프로토콜에 일치한다. (`dice` 속성은 상수 속성으로 선언되었는데, 일단 생성되고 난 뒤에 변경될 필요가 없으며 프로토콜의 요구는 읽기만 가능하면 된다.)

*뱀과 사다리* 게임보드 설정은 클래스의 `init()` 생성자 내에서 이루어진다. 게임 로직 전체는 프로토콜의 `play` 메소드 내로 옮겨졌고, 주사위를 굴린 값을 얻기 위해 `dice` 속성을 필요로 하는 프로토콜을 사용한다.

`delegate` 속성은 `DiceGameDelegate` 옵션으로 되어있는데, 게임을 실행하는데 대리자가 꼭 필요하지는 않아서이다. 옵션값이기 때문에 `delegate` 속성은 자동으로 `nil`을 초기값으로 받는다. 그리고 나서 게임을 초기화할 때 적절한 위임자를 속성으로 받을 수도 있다.

`DiceGameDelegate`는 게임의 진행을 기록하기 위해 3가지 메소드를 제공한다. `play` 메소드 내부에서 사용되며 새로운 게임을 시작할 때, 턴이 시작될 때, 게임이 끝날 때 호출된다.

`delegate` 속성은 `DiceGameDelegate` 타입의 옵션값이기 때문에 `play` 메소드는 대리자에서 메소드를 호출할 때마다 옵션 연쇄를 사용한다. `delegate` 속성이 nil이면, 이 대리자는 에러없이 호출을 실패한다. `deleagte` 속성이 nil이 아니면 메소드를 호출하고 `SnakesAndLadders` 인스턴스를 인자로 넘긴다.

다음 예제는 `DiceGameTracker`라는 클래스로, `DiceGameDelegate` 프로토콜이 적용되었다.

``` swift
class DiceGameTracker: DiceGameDelegate {
    var numberOfTurns = 0
    func gameDidStart(game: DiceGame) {
        numberOfTurns = 0
        if game is SnakesAndLadders {
            println("뱀과 사다리의 새 게임을 시작한다")
        }
        println("게임은 \(game.dice.sides)면체 주사위를 사용할 것이다")
    }
    func game(game: DiceGame, didStartNewTurnWithDiceRoll diceRoll: Int) {
        ++numberOfTurns
        println("주사위는 \(diceRoll)")
    }
    func gameDidEnd(game: DiceGame) {
        println("게임은 \(numberOfTurns)턴을 사용했다")
    }
}
```

`DiceGameTracker`에서는 `DiceGameDelegate`에서 필요한 세가지 메소드를 모두 구현되어있다. 세가지 메소드를 사용해서 게임에서 몇턴이 진행되었는지 기록한다. 게임이 시작하면 `numberOfTurns` 속성을 0을 초괴화하고, 새 턴이 시작할 때마다 증가시키고, 게임이 끝났을 때 총 몇턴이 지났는지 출력한다.

위에서 나온 `gameDidStart`의 구현에서는 `game` 인자를 사용해 게임을 플레이하려고 할 때 안내 문구를 출력한다. `game` 인자는 `SnakesAndLadders`타입이 아니라 `DiceGame`타입을 갖는다. `gameDidStart`는 `DiceGame` 프로토콜에 있는 메소드와 속성들만 사용하고 접근한다. 하지만 메소드에서는 타입 케스팅을 통해 인스턴스가 어떤 타입인지 확인할 수도 있다. 이 예제에서는 `game`이 실제로 `SnakesAndLadders`의 인스턴스인지 확인하고 맞다면 적절한 문구를 출력한다.


`gameDidStart`는 인자로 받은 `game`의 `dice` 속성에도 접근한다. `game`은 `DiceGame` 프로토콜에 일치한다고 되어있으니 `dice` 속성을 가지고 있을 것이고 `gameDidStart` 메소드는 어떤 종류의 게임인지에 관계없이 주사위의 `sides` 속성을 출력할 수 있다.

`DiceGameTracker`가 어떻게 작동하는지 아래 있다.

``` swift
let tracker = DiceGameTracker()
let game = SnakesAndLadders()
game.delegate = tracker
game.play()
// 뱀과 사다리 새 게임을 시작한다
// 게임은 6면체 주사위를 사용할 것이다
// 주사위는 3
// 주사위는 5
// 주사위는 4
// 주사위는 5
// 게임은 4턴을 사용했다
```


### 확장을 프로토콜 일치에 추가

이미 존재하는 타입의 소스에 접근할 수 없어도 그 타입에 프로토콜을 적용하고 일치하도록 확장할 수 있다.
확장은 새 속성들, 메소드들, 인덱스 참조 등을 이미 존재하는 타입에 추가할 수 있고, 프로토콜에서 필요로 하는 요구사항들을 추가할 수도 있다.
확장에 대해 더 많은 정보는 [확장](#) 챕터에 있다.

> 노트
> 
> 확장을 타입에 추가하는 순간 이미 만들어놓은 인스턴스들에서도 프로토콜이 적용되고 일치하게 된다.

예를 들어, `TextRepresentable`이라는 프로토콜은 타입을 텍스트로 표현하는 방법을 구현할 수 있다. 인스턴스의 설명이 될 수도 있고, 현재 상태의 텍스트 표현이 될 수도 있다.

``` swift
protocol TextRepresentable {
    func asText() -> String
}
```

위에서 본 `Dice` 클래스에 `TextRepresentable`을 적용하고 일치시킬 수 있다.

``` swift
extension Dice: TextRepresentable {
    func asText() -> String {
        return "\(sides)면체 주사위"
    }
}
```

확장은 `Dice`를 위에서 구현했던 것과 정확히 같은 방법으로 새로운 프로토콜을 적용한다. 프로토콜 이름 뒤에 콜론으로 구분해서 프로토콜의 이름을 적고 중괄호 안에 프로토콜의 요구사항들 전부를 구현하면 된다.

이제 어떤 `Dice` 인스턴스들도 `TextRepresentable`로 처리할 수 있다. 


```
let d12 = Dice(sides: 12, generator: LinearCongruentialGenerator())
println(d12.asText())
// "12면체 주사위" 출력
```

비슷하게 `SnakesAndLadders` 게임 클래스도 `TextRepresentable` 프로토콜을 적용하고 일치하도록 확장할 수 있다.

``` swift
extension SnakesAndLadders: TextRepresentable {
    funcs asText() -> String {
        return "뱀과 사다리 게임은 \(finalSquare)"칸
    }
}
println(game.asText())
// "뱀과 사다리 게임은 25칸" 출력
```


### 확장과 동시에 프로토콜 적용 선언

타입이 이미 프로토콜의 모든 요구사항에 일치하고 있지만 프로토콜을 적용한다고 명시하지 않았을 때, 빈 확장과 함께 프로토콜을 적용시킬 수 있다.

``` swift
struct Hamster {
    var name: String
    func asText() -> String {
        return "햄스터 이름은 \(name)"
    }
}
extension Hamster: TextRepresentable {}
```

이제 `Hamster`의 인스턴스들은 `TextRepresentable`을 타입으로 사용할 수 있다.

``` swift
let simonTheHamster = Hamster(name: "Simon")
let somethingTextRepresentable: TextRepresentable = simonTheHamster
println(somethingTextRepresentable.asText())
// "햄스터 이름은 Simon" 출력
```

> 노트
> 
> 타입에서 요구사항을 만족했다고 자동으로 프토토콜이 적용되지는 않는다. 항상 명시적으로 프로토콜의 적용을 선언해줘야 한다.


### 프로토콜 타입의 콜렉션(Collection)들

프로토콜은 [타입으로서의 프로토콜](#)에서 이야기한 것처럼 배열이나 사전같은 콜렉션에 저장되는 타입으로 사용할 수 있다.
여기 `TextRepresentable`의 배열을 만든 예제가 있다.

``` swift
let things: TextRepresentable[] = [game, d12, simonTheHamster]
```

배열에서 아이템들을 반복하면서 각 아이템을 텍스트로 출력하는 것이 가능하다.

``` swift
for thing in things {
    println(thing.asText())
}
// 뱀과 사다리 게임은 25칸
// 12면체 주사위
// 햄스터 이름은 Simon
```

`thing` 상수는 `Dice`나 `DiceGame`이나 `Hamster` 타입이 아니고 `TextRepresentable` 타입이다. 하지만 `TextRepresentable`은 `asText` 메소드를 가지고 있기 때문에 반복문에서 `thing.asText`를 안전하게 호출할 수 있다.

### 프로토콜 상속

프로토콜은 하나 이상의 프로토콜을 *상속*받을 수 있고, 그 요구사항들 위에 다른 요구사항을 추가할 수도 있다. 프로토콜 상속 문법은 클래스 상속의 문법과 비슷하지만 쉼표를 구분해서 여러 프로토콜을 나열할 수 있다.

``` swift
protocol InheritingProtocol: SomeProtocol, AnotherProtocol {
    // 프로토콜 정의가 여기 온다
}
```

위의 `TextRepresentable` 프로토콜을 상속받는 예제가 있다.

``` swift
protocol PrettyTextRepresentable: TextRepresentable {
    func asPrettyText() -> String
}
```

이 예제에서는 `TextRepresentable`을 상속받는 `PrettyTextRepresentable`라는 새로운 프로토콜을 정의한다. `PrettyTextRepresentable`을 적용한 것들은 `TextRepresentable`의 요구사항을 모두 만족해야하고, 추가로 `PrettyTextRepresentable`의 요구사항도 만족해야한다. 이 예제에서는 `String`을 리턴하는 `asPrettyText`라는 인스턴스 메소드 하나가 요구사항으로 추가되었다.

`PrettyTextRepresentable`를 적용하고 일치하게 `SnakesAndLadders` 클래스를 확장할 수 있다.

``` swift
extension SnakesAndLadders: PrettyTextRepresentable {
    func asPrettyText() -> String {
        var output = asText() + ":\n"
        for index in 1...finalSquare {
            switch board[index] {
            case let ladder where ladder > 0:
                output += "▲ "
            case let snake where snake < 0:
                output += "▼ "
            default:
                output += "○ "
            }
        }
        return output
    }
}
```

이 확장은 `PrettyTextRepresentable` 프로토콜을 적용하고 `SnakesAndLadders` 타입에서 `asPrettyText` 메소드를 구현했다. `PrettyTextRepresentable`가 적용되었다면 `TextRepresentable`도 적용해야하므로, `asPrettyText`의 구현이 `TextRepresentable`의 `asText` 메소드를 호출하는 것으로 시작한다. `asText`에 콜론과 줄넘김을 붙이는 것을 시작으로 출력값을 만든다. 보드의 배열을 돌면서 각 칸에 해당하는 특수문자(emoji)를 붙인다.

- 칸의 값이 0보다 크고 사다리면 ▲로 표시
- 칸의 값이 0보다 작고 뱀 머리면 ▼로 표시
- 그 외에, 칸의 값이 0이고 "비어있는" 칸이면 ○으로 표시

이제 메소드구현으로 `SnakesAndLadders` 인스턴스의 내역을 보기좋게 표시하는데 사용할 수 있다.

``` swift
println(game.asPrettyText())
// 뱀과 사다리 게임은 25칸:
// ○ ○ ▲ ○ ○ ▲ ○ ○ ▲ ▲ ○ ○ ○ ▼ ○ ○ ○ ○ ▼ ○ ○ ▼ ○ ▼ ○
```


### 프로토콜 합성

한번에 여러 프로토콜에 일치하는 타입이 필요할 때 유용하게 쓸 수 있다. *프로토콜 합성*으로 여러 프로토콜을 하나의 요구사항으로 합칠 수 있다. 프로토콜 합성은 `protocol<SomeProtocol, AnotherProtocol>`같은 형태를 가진다. 꺾쇠(<>) 안에 쉼표로 구분해서 원하는만큼 프로토콜을 명시할 수 있다.

여기 `Named`와 `Aged` 두가지 프로토콜을 합성해서 하나의 함수 파라미터로 사용하는 예제가 있다.

``` swift
protocol Named {
    var name: String { get }
}
protocol Aged {
    var age: Int { get }
}
struct Person: Named, Aged {
    var name: String
    var age: Int
}
func wishHappyBirthday(celebrator: protocol<Named, Aged>) {
    println("\(celebrator.name)의 \(celebrator.age)번째 생일을 축하합니다!")
}
let birthdayPerson = Person(name: "Malcolm", age: 21)
wishHappyBirthday(birthdayPerson)
// "Malcolm의 21번째 생일을 축하합니다!" 출력
```

이 예제에서는 `String` 타입의 `name`이라는 읽기 속성 하나를 요구사항으로 가지는 `Named`라는 프로토콜을 정의한다. 또한 `Int` 타입의 `age`라는 읽기 속성 하나를 요구사항으로 가지는 `Aged`도 정의한다. 두 프로토콜 모두 `Person` 구조체에 적용된다.

`wishHappyBirthday`라는 함수를 만들어서 `celebrator`라는 인자를 하나 받는다. 이 인자의 타입은 `protocol<Named, Aged>`이며 "`Named`와 `Aged` 프로토콜에 모두 일치하는 어떤 타입"을 의미한다. 어떤 특정한 타입이 인자로 넘어오는지는 관계없으나 요구하는 프로토콜 양쪽 다 일치해야한다.


그리고나서 `birthdayPerson`이라는 `Person`의 인스턴스를 만들어서 `wishHappyBirthday`라는 함수의 인자로 넘긴다. `Person`이 프로토콜 양쪽 다 일치하기 때문에 유효하며 `wishHappyBirthday` 함수에서 생일축하 인사를 출력할 수 있다.

> 노트
> 
> 프로토콜 합성은 새로 영구적으로 프토토콜 타입을 만드는 것이 아니다. 합성에 있는 모든 프로토콜의 요구사항을 합친 하나의 프로토콜을 임시로 만드는 것이다.


### 프로토콜 일치를 확인하기

[타입 캐스팅](#)에서 설명했던 것처럼 특정 프로토콜로 캐스팅하기 위해서 프로토콜 일치를 확인하는데 `is`와 `as` 연산자를 사용할 수 있다. 타입을 확인하고 캐스팅하는 것과 정확히 같은 방법으로 프로토콜을 확인하고 캐스팅할 수 있다.

- `is` 연산자에서는 인스턴스가 프로토콜과 일치하면 `true`, 아니면 `false`를 리턴
- 'as?' 다운캐스팅 연산자는 프로토콜 타입의 옵션값을 리턴하는데 인스턴스가 프로토콜과 일치하지 않으면 `nil`이 된다
- `as` 연산자는 강제로 다운캐스팅하고 실패하면 런타임 에러가 난다.

`Double` 타입의 `area` 읽기 속성 하나를 요구사항으로 갖는 `HasArea` 프로토콜 예제가 있다.

``` swift
@objc protocol HasArea {
    var area: Double { get }
}
```

> 노트
> 
> `HasArea` 프로토콜 앞에 보이듯 프로토콜 일치를 확인하기 위해서는 `@objc` 속성(attribute)을 명시해줘야한다.
> *코코아와 Objective-C를 스위프트와 사용하기(Using Swift with Cocoa and Objective-C)*에서 설명하듯 이 속성은 Objective-C 코드에서 인식할 수 있을 것이라는 것을 명시한다.
> Objective-C를 함께 쓰지 않더라도 프로토콜 일치를 확인하고 싶다면 `objc`를 프로토콜에 명시해줘야한다.
> 
> `@objc` 프로토콜은 구조체나 열거형은 불가능하고 클래스에만 적용할 수 있다.

`HasArea` 프로토콜과 일치하는 `Circle`과 `Country` 두가지 클래스가 있다.

``` swift
class Circle: HasArea {
    let pi = 3.1415927
    var radius: Double
    var area: Double { return pi * radius * radius }
    init(radius: Double) { self.radius = radius }
}
class Country: HasArea {
    var area: Double
    init(area: Double) { self.area = area }
}
```

`Circle` 클래스는 저장된 `radius` 속성을 사용해 `area` 속성을 계산된 속성으로 구현했다. `Country` 클래스에서는 직접 저장된 속성으로 `area` 속성을 구현했다. 두가지 클래스 모두 `HasArea` 프로토콜에 정확히 일치한다.

여기 `Animal`이라는 클래스가 있고 `HasArea` 프로토콜에 일치하지 않는다.

```
class Animal {
    var legs: Int
    init(legs: Int) { self.legs = legs }
}
```

`Circle`, `Country`, `Animal` 클래스는 공통된 부모 클래스를 갖지는 않는다. 다만 이 클래스들과 클래스들의 인스턴스들 모두 `AnyObject` 배열의 값으로 초기화되는데 사용할 수  있다.


``` swift
let objects: AnyObject[] = [
    Circle(radius: 2.0),
    Country(area: 243_610),
    Animal(legs: 4)
]
```

`Circle` 인스턴스는 반지름 2로, `Country` 인스턴스는 영국의 면적으로, `Animal` 인스턴스는 다리 4개로 초시화되어 배열 표기를 통해 `objects` 배열에 초기화되었다.

`objects` 배열은 순환가능하며, 배열 내 각각의 객체는 `HasArea` 프로토콜에 일치하는지 확인할 수 있다.

``` swift
for object in objects {
    if let objectWithArea = object as? HasArea {
        println("넓이는 \(objectWithArea.area)")
    } else {
        println("넓이를 가지고 있지 않다")
    }
}
// 넓이는 12.5663708
// 넓이는 243610.0
// 넓이를 가지고 있지 않다
```

배열 내의 객체가 `HasArea` 프로토콜에 일치할 때마다 `as?` 연산자를 통해 `objectWithArea`라는 상수값으로 옵션값을 받을 수 있다. `objectWithArea` 상수는 `HasArea` 타입이며 `area` 속성을 접근할 수도 있고 출력도 가능하다.

캐스팅 과정에서 객체들이 변하지는 않는다. 여전히 `Circle`, `Country`, `Animal` 객체다. 하지만 `objectWithArea` 상수에 저장되면 `HasArea` 타입으로만 사용할 수 있고 `area` 속성에만 접근할 수 있다.

### 프로토콜 선택적 요구사항

프로토콜에서 *선택적 요구사항*을 정의할 수 있다. 프로토콜에 일치하기 위해서 이 요구사항들을 구현하지 않아도 된다. 선택적 요구사항들은 정의 앞에 `@optional` 키워드가 붙는다.

선택적 요구사항은 프로토콜에 일치하도록 요구사항이 구현되었는지 여부를 확인하는 옵션 연쇄와 같이 사용될 수 있다. 옵션 연쇄에 대해서는 [옵션 연쇄](#) 챕터에 정보가 나와있다.

요구사항(의 메소드나 이름)이 쓰일 때 `someOptionalMethod?(someArgument)`처럼 이름 뒤에 물음표가 붙는 구현을 확인할 수 있다.
선택적 속성 요구사항과 값을 리턴하는 선택적 메소드 요구사항은 그걸 접근해서 호출할 때 적절한 타입의 옵션값을 항상 반환하며, 구현이 되어있지 않을 수 있는 선택적 요구사항의 상태를 보여준다.

> 노트
프로토콜의 선택적 요구사항들은 `@objc` 속성이 명시되어있을 때만 사용할 수 있다.
Objective-C와 같이 사용하지 않더라도 선택적 요구사항을 사용하고 싶다면 `@objc` 속성을 명시해야한다.
구조체나 열거형이 아닌 클래스에서만 `@objc` 프로토콜을 적용할 수 있다.
따라서 선택적 요구사항을 사용하기 위해 `@objc`를 프로토콜에 명시했다면 프로토콜은 클래스 타입에만 적용될 수 있다.


다음 예제는 `Counter`라는 정수 카운터를 구현한 것으로 증가량을 외부에 제공하기 위해 사용한다. 선택적 요구사항 2가지를 가진 `CounterDataSource` 프로토콜이 정의되어있다.

``` swift
@objc protocol CounterDataSource {
    @optional func incrementForCount(count: Int) -> Int
    @optional var fixedIncrement: Int { get }
}
```

`CounterDataSource` 프로토콜에는 `incrementForCount`라는 선택적 메소드 요구사항과 `fixedIncrement`라는 선택적 속성 요구사항이 정의되어있다.
이 요구사항들은 `Counter` 인스턴스에 증가량을 전하기 위한 두가지 방법이다.

> 노트
엄밀히 이야기해서 프로토콜 요구사항을 *전혀* 구현하지 않고도 `CounterDataSource`에 일치하는 클래스를 만들 수 있다.
둘 다 선택적 요구사항이기 때문이다. 기술적으로는 가능하지만 괜찮은 방법은 아니다.

아래 정의한 `Counter` 클래스는 `CounterDataSource?` 타입을 `dataSource`라는 선택적 속성으로 가지고 있다.

```
@objc class Counter {
    var count = 0
    var dataSource: CounterDataSource?
    func increment() {
        if let amount = dataSource?.incrementForCount?(count) {
            count += amount
        } else if let amount = dataSource?.fixedIncrement? {
            count += amount
        }
    }
}
```

`Counter` 클래스는 `count` 속성에 현재값을 저장한다.
그리고 `increment`라는 메소드도 가지고 있는데 불릴 때마다 `count` 속성을 증가시킨다.

`increment` 메소드는 데이터 소스에서 `incrementForCount`가 구현되어있는지 확인하고 증가량을 가져온다. 옵션 연쇄로 `count`를 인자로 넘기는 `incrementForCount`를 호출한다.

여기 *두* 단계의 옵션 연쇄가 있다. 첫번째로는 `dataSource`가 `nil`일 수도 있으니 아닐 때만 `incrementForCount`를 호출하기 위해 물음표가 붙어있다. 두번째로는 `dataSource`가 *존재하긴* 하지만 선택적 요구사항인 `incrementForCount`가 구현되어있다는 보장이 없다. 그래서 `incrementForCount`에도 물음표가 붙어 있다.

`incrementForCount`를 호출하는데 두가지 이유로 실패할 수 있어서 *선택적* `Int`값이 리턴된다. 참이면 `incrementForCount`가 구현되어 있어서 `CounterDataSource` 정의에 따라 비선택적(non-optional) `Int` 값을 받는다.

`incrementForCount`가 호출되고 나면 실제로 받은 `Int` 값이 `amount`에 상수로 들어간다. 만약 선택적 `Int`가 값을 가지고 있으면, 즉 대리자와 메소드가 둘 다 존재하면 메소드는 값을 리턴할테니 `amount`의 실제값이 `count` 속성에 더해져 저장되고, 증가는 완료된다.

`dataSource`가 nil이거나 `incrementForCount`를 구현하지 않아서 `incrementForCount`에서 값을 가져올 수 *없다면* `increment` 메소드는 `fixedIncrement` 속성을 대신 가져온다.
`fixedIncrement` 속성은 선택적 요구사항이므로 끝에 물음표를 붙여서 실패할 수 있는 것을 명시한다. 앞에서처럼 `CounterDataSource` 프로토콜의 정의에서 `fixedIncrement`는 비선택적 `Int` 속성이지만, 결과적으로 선택적 `Int` 값이 리턴된다.

호출될 때마다 `3`을 리턴하는 간단한 `CounterDataSource` 구현이 있다. 선택적 `fixedIncrement` 속성을 구현했다.

``` swift
class ThreeSource: CounterDataSource {
    let fixedIncrement = 3
}
```

`ThreeSource`의 인스턴스를 새로운 `Counter` 인스턴스의 데이터 소스로 사용할 수 있다.

``` swift
var counter = Counter()
counter.dataSource = ThreeSource()
for _ in 1...4 {
    counter.increment()
    println(counter.count)
}
// 3
// 6
// 9
// 12
```

새로운 `Counter` 인스턴스를 만드는 위의 코드에서는 `ThreeSource` 인스턴스를 데이터 소스로 사용하고, `increment` 메소드를 4번 호출한다. 예상대로 카운터의 `count` 속성은 `increment`가 호출되는 3번동안 증가한다.

여기 `TowardsZeroSource`라는 조금 더 복잡한 데이터 소스가 있다. `Counter` 인스턴스에서는 현재 `count` 값에서 0을 향해 올리거나 내린다.

``` swift
class TowardsZeroSource: CounterDataSource {
    func incrementForCount(count: Int) -> Int {
        if count == 0 {
            return 0
        } else if count < 0 {
            return 1
        } else {
            return -1
        }
    }
}
```

`TowardsZeroSource` 클래스는 `CounterDataSource` 프로토콜의 선택적 `incrementForCount` 메소드를 구현했고 `count` 인자값을 사용해 0을 향하도록 한다. `count`가 이미 0이면 더이상 카운트할 필요가 없다고 명시하도록 `0`을 리턴한다.

`Counter` 인스턴스에 `TowardsZeroSource` 인스턴스를 상요해서 `-4`에서 0까지 증가시킨다. 카운터가 0에 도달하면 더이상 카운트하지 않는다.

``` swift
Counter.count = -4
counter.dataSource = TowardsZeroSource()
for _ in 1...5 {
    counter.increment()
    println(counter.count)
}
// -3
// -2
// -1
// 0
// 0
```
