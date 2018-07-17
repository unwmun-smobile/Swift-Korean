---
id: chapter19
title: 옵셔널 체인 (Optional Chaining)
---
> Translator : 허혁 (hyukhur@gmail.com)

옵셔널 체인(Optional chaining)란 `nil`이 될 수도 있는 옵션(options)을 가진 프로퍼티(property), 메소드(method), 서브 스크립트 (subscript)에 질의하고 호출하는 프로세스를 말한다.  만약 어떤 옵션이 값을 가진다면 프로퍼티, 메소드, 서브스크립트 호출은 성공하고 옵션이 `nil`이면, 프로퍼티, 메소드, 서브스크립트 호출은 `nil`을 반환하게 된다.
여러개의 질의도 함께 엮일 수 있으며, 만약 체인(chaining) 중간의 어떤 링크가 `nil`이라면 조용하게 전체 체인은 실패한다. 

>NOTE
스위프트(Swift)의 옵셔널 체인이 오브젝티브씨(Objective-C)에 있는 `nil`에 메시지 보내기와 유사하다. 그러나, 모든 타입(any type)에서 동작하고, 성공, 실패 여부를 확인할 수 있다는 점에서 차이가 있다.

## 강제 랩핑 해제(Forced Unwrapping) 대안으로써 옵셔널 체인
호출하고자 하는 프로퍼티, 메소드, 서브스크립트의 옵셔널 값(optional value)이 `nil` 아닐 때 옵션값 뒤에 물음표(`?`)를 두어 옵셔널 체인을 명시 할 수 있다. 이것은 옵션널 값 뒤에 느낌표(`!`)를 두어 그 값을 강제로 랩핑 해제하는 것과 유사하다. 가장 주요한 차이점은 옵셔널 체인은 옵션이 `nil`일 때 자연스럽게 실패한다는 것이고, 강제 랩핑 해제는 옵션이 `nil`인 경우 런타임 에러가 발생한다.
옵셔널 체인이 `nil` 값에도 호출할 수 있다는 사실을 반영하기 위해 옵셔널 체인 호출 결과는 항상 옵션널 값이다. 비록 질의한 프로퍼티, 메소드, 서브스크립트가 항상 옵션널 값이 아닌 결과를 도출해도 그렇다. 이 옵션널 반환 값을 사용해서 옵셔널 체인 호출이 성공했는지 ( 반환된 옵션이 값을 가지는 ) 체인 중간의 `nil` 값 ( 옵션 반환값이 `nil` ) 때문에 실패했는지를  확인할 수 있다.
구체적으로, 옵셔널 체인 호출 결과는 옵션으로 감싸여져 있음에도 기대한 반환값과 동일한 타입이다. 일반적으로 `Int`를 반환하는 프로퍼티는 옵셔널 체인에 따라 접근이 가능할때는 `Int?`를 반환할 것이다. 
다은 몇몇 코드 조각은 옵셔널 체인이 어떻게 강제 랩핑 해제와 다르고 성공 여부 확인을 가능케 하는지 보여준다.
먼저 `Person`과 `Residence`라는 클래스를 정의하자.
```
class Person {
    var residence: Residence?
}

class Residence {
    var numberOfRooms = 1
}
```
`Residence`인스턴스(Instance)는 기본값이 `1`인 `numberOfRooms`이라는 단 하나의 `Int` 프로퍼티를 가진다. `Person`인스턴스는 `Residence?`타입으로 `residence`이라는 옵셔널 프로퍼티를 가진다.
만약 `Person`이라는 인스턴스를 새로 만들면, 옵셔널 효과에 따라 기본적으로 `nil`로 설정된다. 아래 코드에서는 `john`는 `nil`로 된 `residence`프로퍼티를 가질 것이다.
```
let jone = Person()
```
만약 `Person`의 `residence`의 `numberOfRooms`프로퍼티를 그 값을 강제로 랩핑 해제를 하려고 느낌표를 붙여서 접근한다면 런타임 에러(Runtime Error)를 유발시킬 것이다. 왜냐하면 해제할 `residence`값 자체가 없기 때문이다.
```
let roomCount = john.residence.numberOfRooms
```
위 코드는 `john.residence`가 `nil`이 아닌 값을 성공하며 방 갯수에 적절한 숫자를 담고 있는 Int 값에 `roomCount`를 설정할 것이다. 그러나 이 코드는 위에 보여지는 것처럼 `residence`가 `nil`이라면 항상 런타임 에러를 유발 시킨다. 
옵셔널 체인은 `numberOfRooms`값에 접근하는데 대안법을 제공한다. 옵셔널 체인을 사용하기 위해 느낌표 자리에 물음표를 사용하면 된다.
```
if let roomCount = john.residence?.numberOfRooms {
    println("John's residence has \(roomCount) room(s).")
} else {
    println("Unable to retrieve the number of rooms.")
}
// prints "Unable to retrieve the number of rooms."
```
이것은 스위프트(swift)가 옵셔널 `residence`프로퍼티를 "묶고" 만약 `residence`가 있으면 `numberOfRooms`값을 가져온다는 것을 말해준다.

`numberOfRoom`에 대한 접근이가 잠제적으로 실패할 수 있기 때문에 옵셔널 체인은 `Int?`이나 "옵션널 `Int`"형 값을 반환하려고 한다. 위 예제처럼 `residence`가 `nil`인 경우는 `numberOfRooms`에 대한 접근이 불가능하다는 사실을 반영하기 위해서 이 옵셔널`Int` 역시 `nil`이 될 것이다.
`numberOfRooms`가 비옵셔널 `Int`임에도 불구하고 참인 것을 명심해라. 옵셔널 체인을 통해 질의한다는 것은 `numberOfRooms`가 `Int` 대신 `Int?`를 항상 반환할 것이라는 것을 의미한다.
`john.residence`에 `Residence` 인스턴스를 할당할 수 있는데 그러면 더이상 `nil`값은 존재하지 않게 된다.
```
john.residence = Residence()
```
`john.residence`는 실체 `Residence`인스턴스를 이제 가지게 되었다. 만약 예전과 동일한 옵셔널 체인을 사용해 접근하려고 하면, `1`이라는 `numberOfRooms`기본값을 가지는 `Int?`가 반환될 것이다.
```
if let roomCount = john.residence?.numberOfRooms {
    println("John's residence has \(roomCount) room(s).")
} else {
    println("Unable to retrieve the number of rooms.")
}
// prints "John's residence has 1 room(s)."
```
## 옵셔널 체인을 위한 모델(Model) 클래스(Class) 선언

프로퍼티, 메소드, 서브스크립트를 호출하는 것 같은 한단계 더 깊은 옵셔널 체인을 사용할 수 있다. 이는 상호관계있는 타입간의 복잡한 모델에서 서브 프로퍼티(subproperty)를 파고 들 수 있게 해주고 그 서브 프로터티에 프로퍼티와 메소드, 서브스크립트에 접근할 수 있는지 아닌지를 확인할 수 있게 해준다.
다음 코드 조각은 다단계 옵셔널 체인 예를 포함한 몇가지 순차적인 예제에서 사용될 4개의 모델 클래스를 정의한다. 이 클래스들은 위에 나온 `Person`과 `Residence` 모델에 `Room`과 `Address` 클래스를 추가하고 연관 프로퍼티와 메소드, 서브스크립트를 확장한다.
`Person` 클래스는 이전과 동일한 방법으로 정의한다.
```
class Person {
    var residence: Residence?
}
```
`Residence` 클래스는 이전보다 조금 복잡해졌다. 이번에는 `Residence` 클래스에 `Room[]` 타입의 빈 배열로 초기화된 `rooms`라는 변수 프로퍼티를 선언한다.
```
class Residence {
    var rooms = Room[]()
    var numberOfRooms: Int {
    return rooms.count
    }
    subscript(i: Int) -> Room {
        return rooms[i]
    }
    func printNumberOfRooms() {
        println("The number of rooms is \(numberOfRooms)")
    }
    var address: Address?
}
```

이번 버전 `Residence`는 `Room`인스턴스 배열을 저장하기 때문에, 그 `numberOfRooms`프로퍼티는 저장된 프로퍼티가 아닌 계산된 프로퍼티로 구현했다. 계산된 `numberOfRooms`프로퍼티는 단순히 `rooms`배열에서 `count`프로퍼티의 값을 반환한다.
그 `rooms`배열에 접근하기 위한 바로가기로 이번 버전 `Residence`는 읽기만 가능한 서브 스크립트를 제공하는데 서브스크립트에게 전달받는 인덱스(index)가 적합할 것이라는 가정으로 시작해보겠다. 만약 인덱스가 적합하다면, 서브스크립트는 `rooms` 배열의 요청받은 인덱스의 방 정보를 반환할 것이다.
또한 이번 버전 `Residence`는 `printNumberOfRooms`라는 이름의 메소드를 제공하는데 단순히 `Residence`에 방 갯수를 출력한다.
마지막으로 `Residence`에 `Address?`이란 타입으로 `address`라는 옵셔널 프로퍼티를 선언한다. 이를 위한 `Address`클래스 타입은 밑에 정의하겠다.
`rooms`배열에 사용하는 `Room`클래스는 `name`이라는 프로퍼티 하나를 가지는 간단한 클래스인데 이는 적절한 방이름을 설정하기 위한 초기화 역할(initializer)을 한다.
```
class Room {
    let name: String
    init(name: String) { self.name = name }
}
```

이 모델의 마지막 클래스는 `Address`이다. 이 클래스는 `String?`타입의 옵셔널 프로퍼티를 3개 가지고 있다. 그 중 2개는 `buildingName`과 `buildingNumber` 인데 주소를 구성하는 특정 빌딩에 대한 구분을 짓기 위한 대체 수단이다. 3번째 프로퍼티인 `street`는 그 주소의 도로이름에 사용한다.
```
class Address {
    var buildingName: String?
    var buildingNumber: String?
    var street: String?
    func buildingIdentifier() -> String? {
        if buildingName {
            return buildingName
        } else if buildingNumber {
            return buildingNumber
        } else {
            return nil
        }
    }
}
```
또한 `Address`클래스는 `String?`반환값을 가지는 `buildingIdentifer`이란 이름의 메소드를 제공한다. 이 메소드는 `buildingName`과 `buildingNumber`프로퍼티를 확인해서 만약 `buildingName`이 값을 가진다면 그 값을 혹은 `buildingNumber`이 값을 가진다면 그 값을, 둘다 값이 없다면 `nil`을 반환한다.

## 옵셔널 체인를 통한 프로퍼티 호출
강제 랩핑 해제(Forced Unwrapping) 대안으로써 옵셔널 체인에서 봤던 것처럼 옵셔널 체인을 온션값에 대한 프로퍼티 접근에 접근할 수 있는지 만약 프로퍼티 접근이 가능한지 확인하기 위해 사용할 수 있다. 그러나선택 묶임를 통해 프로퍼티의 값을 설정하는 것은 할 수 없다.
위에 정의한 새로운 `Person` 인스턴스를 사용해 클래스를 만들어 이전처럼 `numberOfRooms` 프로퍼티에 접근하기를 시도해본다.
```
let john = Person()
if let roomCount = john.residence?.numberOfRooms {
    println("John's residence has \(roomCount) room(s).")
} else {
    println("Unable to retrieve the number of rooms.")
}
// prints "Unable to retrieve the number of rooms."
```
`john.residence`가 `nil`이기 때문에 이 옵셔널 체인을 예전과 동일한 방식으로 호출했지만 에러 없이 실패한다.

## 옵셔널 체인을 통한 메소드 호출
옵셔널 체인을 사용해서 옵션값을 호출하고 메소드 호출이 성공했는지 여부를 확인해볼 수 있다. 설렁 메소드가 반환값을 정의하지 않더라고 할 수 있다.
`Residence` 클래스에 있는 `printNumberOfRooms`메소드는 `numberOfRooms`의 현재 값을 출력한다. 그 메소드는 다음과 같을 것이다.
```
func printNumberOfRooms() {
    println("The number of rooms is \(numberOfRooms)")
}
```
이 메소드는 반환값을 명시하지 않았다. 그러나 반환형이 없는 함수와 메소드는 `Functions Without Return Values`에 나와 있는 것처럼 암시적으로 `Void`타입을 반환하게 된다.
만약 옵셔널 체인에 있는 옵션값에 이 메소드를 호출한다면, 메소드 반환형은 `Void`가 아니라 `Void?`이 될 것이다. 옵셔널 체인을 통해 호출될 때 옵셔널 타입은 항상 반환 값을 가지기 때문이다. 이는 메소드가 반환값이 정의되어 있지 않더라도 `printNumberOfRooms`메소드를 호출이 가능한지를 `if`문을 써서 확인할 수 있게 한다. `printNumberOfRooms`에서 암시적 반환값은 만약 메소드가 옵셔널 체인를 통해 성공적으로 호출되었다면 `Void`와 동일할 것이고 그렇지 않다면 `nil`과 동일할 것이다.
```
if john.residence?.printNumberOfRooms() {
    println("It was possible to print the number of rooms.")
} else {
    println("It was not possible to print the number of rooms.")
}
// prints "It was not possible to print the number of rooms."
```

## 옵셔널 체인을 통한 서브스크립트 호출
옵셔널값에 대한 서브스크립트에서 값을 가져와서 서브스크립트 호출이 성공했는지 확인하기 위해 옵셔널 체인을 사용할 수 있다. 그러나 을 통해 서브스크립트로 값을 설정하는 것은 할 수 없다.

>NOTE
옵셔널 체인를 통해 옵션값에 대한 서브스크립트를 접근할 때 서브스크립트 꺽은 괄호(bracket) 앞에 물음표를 놓아야 한다. 뒤가 아니다. 옵셔널 체인 물음표는 항상 옵셔널 표현식의 뒤에 바로 따라나와야 한다.

아래 예는 `Residence`클래스에 정의되어 있는 서브스크립트를 사용하는 `john.residence` 프로퍼티의 `rooms`배열에 있는 첫번째 방이름을 집어오려고 하는 것이다. `john.residence`가 현재 `nil`이기 때문에 서브스크립트는 실패한다.
```
if let firstRoomName = john.residence?[0].name {
    println("The first room name is \(firstRoomName).")
} else {
    println("Unable to retrieve the first room name.")
}
// prints "Unable to retrieve the first room name."
```

이 서브스크립트 호출 속에 있는 옵셔널 체인 물음표는 `john.residence`바로 뒤, 서브스크립트 꺽은 괄호 전에 존재해야한다. 왜냐하면, `john.residence`가 옵셔널 체인을 꾀할 옵션값이기 때문이다.
만약, `john.residence`에 `rooms`배열에 한개 이상의 `Room`인스턴스도 같이 실제 `Residence`를 만들어서 할당한다면 옵셔널 체인을 통해 `rooms`배열안의 실제 아이템에 접근하기 위해서 `Residence`서브스크립트를 사용할 수 있다. 
```
let johnsHouse = Residence()
johnsHouse.rooms += Room(name: "Living Room")
johnsHouse.rooms += Room(name: "Kitchen")
john.residence = johnsHouse
if let firstRoomName = john.residence?[0].name {
    println("The first room name is \(firstRoomName).")
} else {
    println("Unable to retrieve the first room name.")
}
// prints "The first room name is Living Room."
```
## 다단계 묶임 연결하기
프로퍼티와 메소드, 서브스크립트를 사용해 모델 깊이 파고들기 위해서 옵셔널 체인을 여러 단계로 함께 엮을 수 있다. 그러나 다단계 옵셔널 체인으로 반환값에 더 많은 옵셔널 단계를 넣을 수는 없다.
다른 방식으로:

- 만약 집어오려고 하는 타입이 옵셔널이지 않으면, 옵셔널 체인으로 인해 옵셔널로 변경될 것이다.
- 만약 집어오려고 하는 타입이 이미 옵셔널이라면, 옵셔널 체인으로 인해 더 옵셔널로 변경되지는 않을 것이다.

그러므로:

- `Int`타입을 옵셔널 체인을 통해 집어오려고 하면, 항상 `Int?`가 반환될 것이다. 얼마나 많은 단계의 체인이 사용되었는지는 중요하지 않다.
- 유사하게, `Int?`값을 집어오려고 하면, 항상 `Int?`가 반환될 것이다.  얼마나 많은 단계의 체인이 사용되었는지는 중요하지 않다.

아래 예는 `john`의 `residence`프로퍼티의 `address`프로퍼티의 `street`프로퍼티에 접근하려는 것을 보여준다. 여기에 사용되는 2개의 옵셔널 체인 단계가 있는데 `residence`와 `address`로 둘은 엮여 있고 둘다 옵셔널타입이다.
```
if let johnsStreet = john.residence?.address?.street {
    println("John's street name is \(johnsStreet).")
} else {
    println("Unable to retrieve the address.")
}
// prints "Unable to retrieve the address."
```
`john.residence`의 값은 현재 적합한 `Residence`인스턴스를 포함하고 있다. 그러나 `john.residence.address`의 값은 현재 `nil`이다. 이때문에, `john.residence?.address?.street`호출은 실패한다.
위 예제를 잘 생각해보자. `street`프로퍼티 값을 집어오고자 했다. 이 프로퍼티는 `String?`이다. 그러므로  `john.residence?.address?.street`의 반환값 역시 두단계 옵셔널 체인으로 프로퍼티가 옵셔널타입에 추가로 더해 적용되었음에도 불구하고 `String?`이다.
만약 `john.residence.address`의 값으로써 실제 `Address`인스턴스를 설정하고 그 `Adress`의 `street`프로퍼티에 실제 값을 설정한다면, 다단계 옵셔널 체인을 통해 그 프로퍼티 값을 접근할 수 있을 것이다.
```
let johnsAddress = Address()
johnsAddress.buildingName = "The Larches"
johnsAddress.street = "Laurel Street"
john.residence!.address = johnsAddress
if let johnsStreet = john.residence?.address?.street {
    println("John's street name is \(johnsStreet).")
} else {
    println("Unable to retrieve the address.")
}
// prints "John's street name is Laurel Street."
```
`john.residence.address`의 `address`인스턴스에 할당하기 위해서 느낌표를 사용한 것을 잘보자. `john.residence`프로퍼티는 옵셔널타입을 가지기에 `Residence`의 `address`프로퍼티에 접근하기 전에 느낌표를 사용해서 그 실제 값을 까볼 필요가 있다.

## 옵셔널 반환값을 사용해서 메소드 체인
이전 예제는 옵셔널 체인을 사용해서 옵셔널타입의 프로퍼티의 값을 어떻게 집어오는지 보여주었다. 또한 옵셔널 체인을 사용해서 옵셔널타입 값을 반환하는 메소드를 호출하고 필요하다면 그 메소드의 반환값을 연결할 수 있었다.
아래 예제는 옵셔널 체인을 통해 `Address` 클래스의 `buildingIndentifer` 메소드를 호출한다. 이 메소드는 `String?` 타입의 값을 반환한다. 이전에 설명한대로, 옵셔널 체인에 따라 호출된 메소드의 최종 반환값 또한 `String?`이 된다.
```
if let buildingIdentifier = john.residence?.address?.buildingIdentifier() {
    println("John's building identifier is \(buildingIdentifier).")
}
// prints "John's building identifier is The Larches."
```
만약 이 메소드 반환값 이상의 옵셔널 체인을 실행하기 원한다면, 메소드 둥근 괄호(parentheses) 다 음옵셔널 체인음 물음표를 두면 된다.
```
if let upper = john.residence?.address?.buildingIdentifier()?.uppercaseString {
    println("John's uppercase building identifier is \(upper).")
}
// prints "John's uppercase building identifier is THE LARCHES."  
```  
>NOTE
위 예제에서 둥근 괄호 다음에 옵셔널 체인 물음표를 놓았는데, 묶고자 하는 옵션값이 `buildingIndentifer` 자체가 아니라 `buildingIndentifer` 메소드의 반환값이기 때문이다.

