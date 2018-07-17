---
id: chapter21
title: 중첩 타입 (Nested Types )
---
> Translator : Alice Kim (mail@jua.kim)

열거형(Enumerations)은 종종 특정 클래스 또는 구조체(structure)의 기능을 지원하기 위해 만들어집니다. 마찬가지로, 복잡한 형태의 맥락에서 사용하기위한 유틸리티 클래스 또는 구조체를 정의하는데 유용합니다. 이를 위해 Swift는 중첩을 지원하는타입의 정의 안에 열거형, 클래스, 구조체를 내장타입으로 사용할 수 있게 함으로써 *중첩타입(Nested Types)*을 정의 할 수있습니다. 
>역자 주: 
예를 들면, 구조체 안에 클래스를 정의하고 그 클래스 안에 다시 열거형 또는 사용자가 정의한 구조체를 넣을 수있다는 얘기.

기존 타입안에 새로운 타입을 중첩하기 위해서는, 기존 타입이  둘러싸고 있는  중괄호(`{`,`}`) 안에서 정의를 작성합니다. 이러한 유형은 필요로 하는 만큼 여러 수준으로 중첩 될 수 있습니다.

## Nested Types in Action
아래의 예제에서는 블랙잭 게임에서 사용되는 게임 카드를 모델로 하는  `BlackjackCard`구조체를 정의하고 있습니다. `BlakcJack` 구조체는 내부에 `Suit`와 `Rank` 라는 이름의 두개의 열거형 타입을 가지고 있습니다.

블랙잭 게임에서 에이스 카드는 1또는 11의 값을 가지고 있습니다. 이러한 요소는 `Values`라는 구조체에 의해 표현됩니다. `Values` 구조체는 `Rank` 열거형 내부에 중첩되어 있습니다. 

```
struct BlackjackCard {
    // nested Suit enumeration
    enum Suit: Character {
        case Spades = "♠", Hearts = "♡", Diamonds = "♢", Clubs = "♣"
    }
    
    // nested Rank enumeration
    enum Rank: Int {
        case Two = 2, Three, Four, Five, Six, Seven, Eight, Nine, Ten
        case Jack, Queen, King, Ace
        struct Values {
            let first: Int, second: Int?
        }
        var values: Values {
            switch self {
                case .Ace:
                    return Values(first: 1, second: 11)
                case .Jack, .Queen, .King:
                    return Values(first: 10, second: nil)
                default:
                    return Values(first: self.toRaw(), second: nil)
            }
        }
    }
    
    // BlackjackCard properties and methods
    let rank: Rank, suit: Suit
    var description: String {
        var output = "suit is \(suit.toRaw()),"
        output += " value is \(rank.values.first)"
        if let second = rank.values.second {
            output += " or \(second)"
        }
        return output
    }
}
```

`Suit`열거형은 4가지 슈트들과 슈트의 그에 해당하는 `Character` 심볼 값을 함께 나타냅니다. 
>역자 주:
블랙잭에서 슈트란 카드에 있는 무늬를 말합니다.

`Rank` 열거형은 13가지 카드의 랭크와 그에 해당하는 `Int` 값을 나타냅니다. (`Int`형의 숫자 값은 Jack, Queen, King, Ace 카드에는 사용되지 않습니다.)

위의 코드를 보면 알 수 있듯이, `Rank` 열거형은 `Values`라는 추가적인 구조체를 포함하는 중첩구조의 형태를 취하고 있습니다. 이 구조는 대부분의 카드는 하나의 값을 가지지만, 에이스 카드는 두가지 값을 갖는다는 사실을 캡슐화합니다.`Values` 구조체는 다음과 표현하는 두가지 속성을 정의하고 있습니다. 
- `Int` 형의 `first`
- `Int?` 형 또는 `optional Int` 형의 `second`

`Rank`도 `Values`구조체의 인스턴스를 반환하는 계산된 `values` 속성을 정의합니다. 이 계산된 속성은 카드의 순위를 고려하여 그 순위에 따라 적절한 값을 가지는 새로운 `Values`인스턴스를 초기화 합니다. 이러한 속성은 `Jack`, `Queen`, `King`, `Ace` 과 같은 특별한 값을 위해 사용합니다. 숫자카드의 경우에는 지정되어 있는 `Int` 값을 사용합니다. 

`BlackjackCard` 구조체는 `rank`와 `suit`라는 두개의 속성을 가지고 있고, `description`이라는 계산된 속성도 정의하고 있습니다. 이 `description` 속성은 카드의 이름과 값에 대한 설명을 빌드하기 위해 `rank`와 `suit`에 저장된 값을 사용합니다.  

`BalckjackCard`구조체는 커스텀 이니셜라이저를 가지고 있지 않으므로, 앞 챕터의 [구조체 타입을 위한 멤버 단위의 이니셜라이저(Memberwise Initializers for Structure Types)](Intialization 챕터 쪽 링크필요)에서 설명한대로 암시적인 멤버단위 이니셜라이저(memberwise intializer)를 가지고 있습니다. 

```
let theAceOfSpades = BlackjackCard(rank: .Ace, suit: .Spades)
println("theAceOfSpades: \(theAceOfSpades.description)")
// prints "theAceOfSpades: suit is ♠, value is 1 or 11
```

`Rank`와 `Suit`가 `BlackjackCard`안에 중첩되어 있다고 해도 그들의 타입은 문맥으로 부터 추론될 수 았가 때문에 이 인스턴스의 초기화는 자신의 맴버 이름(`.Ace`와 `.Spades`)으로 열거형 멤버를 참조할 수 있습니다. 위의 예에서는 `description` 속성이 올바르게 Space Ace 카드가 `1` 또는 `11`의 값을 가지고 있는지 확인합니다. 

## 중첩 타입 참조하기 (Referring to Nested Types)
자신이 정의된 문맥 외부에서 중첩타입을 사용하려면, 자기를 포함하고 있는(중첩하고 있는)타입의 이름을 그 이름앞에 붙입니다. 

```
let heartsSymbol = BlackjackCard.Suit.Hearts.toRaw()
// heartsSymbol is "♡"
```

위의 예를 보면, `Suit`, `Rank`, `Values`와 같은 이름들은 자연스럽게 그들이 정의된 문맥에 의해 규정되기 때문에 의도적으로 짧게 유지할 수 있습니다.
