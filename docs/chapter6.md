---
id: chapter6
title: 컬렉션 타입 (Collection Types)
---
> Translator : 유정협 (justin.yoo@aliencube.com)

스위프트는 여러 값들을 한꺼번에 저장하기 위해 배열과 딕셔너리로 알려진 두가지 _컬렉션 타입_을 제공한다. 배열은 동일한 타입을 가진 값을 순서대로 저장한다. 딕셔너리는 동일한 타입을 가진 값을 순서와 상관 없이 저장한다. 따라서, 딕셔너리는 유일한 식별자인 키를 통해 값을 찾고 참조하게 된다.

스위프트에서 배열과 딕셔너리는 항상 자신이 저장하고자 하는 키와 값의 타입을 확인한다. 이것은 다른 타입을 가진 값을 배열이나 딕셔너리에 실수로라도 저장하지 못한다는 것을 의미한다. 이는 또한 배열과 딕셔너리에서 값을 가져올 때 어떤 타입의 값을 가져올 수 있는지 확신할 수 있다는 의미이기도 하다. 스위프트에서 이렇게 명시적인 타입 컬렉션을 사용하는 것은 당신의 코드가 명확한 밸류 타입을 가져야 하게끔 하는 것이며 개발시 타입이 맞는지 아닌지를 바로바로 잡아낼 수 있게끔 해준다는 것이다.

> NOTE
스위프트의 `Array` 타입은 상수나 변수에 지정될 때, 혹은 함수나 메소드에서 사용될 때 다른 타입들과 다른 행동을 보여준다. 더 자세한 내용은 [컬렉션의 변경 가능성(Mutability of Collections)]() 섹션과 [컬렉션 타입에서 할당과 복사 형태(Assignment and Copy Behavior for Collection Types]() 섹션을 참고하도록 하자.


## 배열 (Arrays)
배열은 같은 타입을 가진 여러개의 값을 순서대로 저장한다. 한 배열 안에서는 같은 값이 여러 다른 위치에서 나타날 수 있다.

스위프트에서 배열은 특정한 종류들의 값들을 저장할 수 있다. 이것은 Objective-C의 `NSArray`와 `NSMutableArray` 클라스와는 다르다. `NSArray`와 `NSMutableArray` 클라스는 어느 종류의 객체든 저장할 수 있고, 반환하는 객체의 속성에 대한 어떠한 정보도 제공하지 않는다. 반면에 스위프트에서는 특정 배열에 저장할 수 있는 밸류 타입은 항상 명시적인 타입 선언을 통하거나 타입 추정을 통해 확인한다. 굳이 클라스 타입이 될 필요는 없다. 예를 들어 만약 당신이 `Int` 타입 배열을 하나 생성한다고 하면, `Int` 값이 아닌 어떤 값도 이 배열에 대입할 수 없다. 스위프트는 타입 지정에 대해 안전하고, 배열 안에 무슨 타입이 들어있는지를 혹은 들어갈지를 항상 확인한다.


### 배열 타입 축약 문법 (Array Type Shorthand Syntax)

스위프트 배열 타입을 정확하게 쓰려면 `Array<SomeType>` 형태로 해야 한다. 여기서 `SomeType`은 배열에 저장할 타입을 의미한다. 또한 축약 형태인 `SomeType[]`으로도 배열을 사용할 수 있다. 이 두 가지 형태가 기능적으로는 동일할지라도, 축약 형태를 사용하는 것을 권장한다. 이 축약 형태의 배열이 이 가이드 문서에서도 계속 쓰일 것이다.


### 배열 표현식 (Array Literals)

배열은 배열 표현식을 통해서 초기화를 시킬 수 있다. 배열 표현식은 하나 또는 그 이상의 값들을 배열 컬렉션에 담는 축약 형태를 가리킨다. 배열 표현식은 대괄호로 둘러싸고, 콤마로 값들을 구분하는 형태로 하여 여러개의 값들을 표현한다.

```
[value1, value2, value3]
```

아래는 `String` 타입의 값들을 저장하는 `shoppingList`라는 배열을 생성하는 예제이다.

```
var shoppingList: String[] = ["Eggs", "Mink"]

// shoppingList has been initialized with two initial items
```

`shoppingList` 변수는 "`String` 타입의 값들을 갖는 배열"로 정의했기 때문에 `String[]` 타입으로 배열 타입을 지정했다. 이렇게 `String` 타입을 갖는 것으로 배열 타입을 지정했기 때문에 이 배열은 오직 `String` 값들만을 저장할 수 있다. 여기서 `shoppingList` 배열은 두 "`Eggs`", "`Mink`" `String` 값을 배열 표현식으로 지정하여 초기화를 시켰다.

> NOTE
이 `shoppingList` 배열은 다음에 나올 예제에서 더 많은 쇼핑 목록을 추가하기 때문에 상수를 위한 `let` introducer가 아닌 `var` introducer를 통해 변수로 지정했다.

이 경우에 배열 표현식은 두 `String` 값 이외에는 다른 것을 포함하지 않는다. 이것은 `shoppingList` 변수의 타입 정의 &ndash; 오직 `String` 타입의 값들만 저장할 수 있는 배열 &ndash; 와 일치한다. 따라서, 배열 표현식을 이용하여 `shoppingList` 변수를 초기화 하는 것이 허용된다.

스위프트의 타입 추정 덕분에 당신은 배열 표현식을 이용하여 같은 타입을 갖는 변수를 초기화 시킨다면 배열 타입을 쓸 필요가 없다. 따라서, `shoppingList` 변수의 초기화는 아래와 같이 좀 더 간결한 형태로도 가능하다.

```
var shoppingList = ["Eggs", "Mink"]
```

배열 표현식의 모든 값들이 모두 같은 타입이기 때문에 스위프트는 `String[]`이 `shoppingList` 변수의 사용에 맞는 타입이라고 추정할 수 있다.


### 배열의 접근 및 수정 Accessing and Modifying an Array

배열은 메소드와 프로퍼티를 통해 접근과 수정이 가능하다. 혹은 subscript 문법을 사용할 수도 있다.

배열 안에 값이 몇 개나 있는지를 확인하기 위해 읽기 전용 속성인 `count` 프로퍼티를 사용한다:

```
println("The shopping list contains \(shoppingList.count) items.")
// prints "The shopping list contains 2 items."
```

불리언 값을 반환하는 `isEmpty` 프로퍼티를 이용하면 `count` 프로퍼티 값이 `0`인지 아닌지 곧바로 확인할 수 있다:

```
if shoppingList.isEmpty {
    println("The shopping list is empty.")
} else {
    println("The shopping list is not empty.")
}

// prints "The shopping list is not empty."
```

새로운 값을 배열의 마지막에 추가하는 것은 `append` 메소드를 이용하면 된다:

```
shoppingList.append("Flour")
// shoppingList now contains 3 items, and someone is making pancakes
```

추가 할당 연산자인 `+=`를 이용하여 배열의 마지막에 새로운 값을 추가할 수도 있다.

```
shoppingList += "Baking Powder"
// shoppingList now contains 4 items
```

같은 타입을 갖는 배열 표현식을 이용하여 한꺼번에 추가시킬 수도 있다:

```
shoppingList += ["Chocolate Spread", "Cheese", "Butter"]
// shoppingList now contains 7 items
```

배열로부터 값을 찾는 것은 배열 변수 바로 뒤에 대괄호를 사용해서 찾고자 하는 값의 인덱스값을 이용하면 된다:

```
var firstItem = shoppingList[0]

// firstItem is equal to "Eggs"
```

배열의 첫번째 값이 갖는 인덱스는 `0`이다. `1`이 아님을 명심하자. 스위프트에서 배열의 인덱스는 항상 0부터 시작한다.

Subscript 문법을 사용하면 지정한 인덱스에 이미 존재하는 값을 바꿀 수도 있다:

```
shoppingList[0] = "Six eggs"

// the first item in the list is now equal to "Six eggs" rather than "Eggs"
```

Subscript 문법을 이용하면 범위를 줘서 한꺼번에 값을 바꿀 수도 있다. 심지어는 바꾸려고 하는 범위가 실제 값의 크기와 달라도 그게 가능하다. 아래 예제는 `shoppingList` 배열에 있는 "`Chocolate Spread`", "`Cheese`", "`Butter`" 값을 "`Bananas`", "`Apples`"으로 바꾸어 버린다:

```
shoppingList[4...6] = ["Bananas", "Apples"]

// shoppingList now contains 6 items
```

> NOTE
Subscript 문법을 사용해서 새 값을 배열의 마지막에 추가하는 것은 안된다. 만약에 배열의 크기보다 큰 인덱스 값을 사용해서 배열에 접근하려 한다면 런타임 에러를 확인할 수 있을 것이다. 하지만 유효한 인덱스 값은 사용 전에 배열의 `count` 프로퍼티를 이용하여 확인이 가능하다. `count` 프로퍼티 값이 `0`인 경우 &ndash; 빈 배열인 경우 &ndash; 를 제외하면 배열에서 가장 큰 인덱스 값은 항상 `count - 1`이 될 것이다. 인덱스는 항상 `0`에서 시작하기 때문이다.

특정한 인덱스에 배열 값을 넣고 싶다면 배열의 `insert(atIndex:)` 메소드를 이용한다:

```
shoppingList.insert("Maple Syrup", atIndex: 0)
// shoppingList now contains 7 items
// "Maple Syrup" is now the first item in the list
```

이것은 `insert` 메소드를 이용하여 "`Mayple Syrup`"이란 새로운 값을 `shoppingList` 배열의 가장 앞  `0` 인덱스 값을 가진 곳에 넣는 것이다.

비슷한 방식으로 배열에서 값을 지울 수도 있다. `removeAtIndex` 메소드를 이용하면 되는데, 이 메소드는 배열내 주어진 인덱스에서 특정 값을 지우고 난 후 그 지워진 값을 반환한다. 이 지워진 값은 필요하지 않다면 무시해도 좋다.

```
let mapleSyrup = shoppingList.removeAtIndex(0)

// the item that was at index 0 has just been removed
// shoppingList now contains 6 items, and no Maple Syrup
// the mapleSyrup constant is now equal to the removed "Maple Syrup" string
```

배열에서 값을 지우고난 다음에 생기는 공백은 자동으로 지워진다. 따라서, `0` 인덱스에 해당하는 값은 이제 "`Six eggs`"이다:

```
firstItem = shoppingList[0]

// firstItem is now equal to "Six eggs"
```

만약 배열의 마지막 값을 지우고 싶다면 `removeLast` 메소드를 이용한다. 이 메소드를 이용하면 `removeAtIndex` 메소드를 `count` 프로퍼티와 함께 사용하는 불필요한 수고를 피할 수 있다. `removeAtIndex` 메소드와 마찬가지로 `removeLast` 메소드 역시 지워진 값을 반환한다:

```
let apples = shoppingList.removeLast()

// the last item in the array has just been removed
// shoppingList now contains 5 items, and no cheese
// the apples constant is now equal to the removed "Apples" string
```

### 배열에서 반복문 사용하기 Iterating Over an Array

`for-in` 반복문을 사용하면 배열 안의 모든 값들에 접근할 수 있다:

```
for item in shoppingList {
println(item)
}

// Six eggs
// Milk
// Flour
// Baking Powder
// Bananas
```

만약 배열 안의 개별적인 값들과 그에 해당하는 인덱스가 함께 필요하다면 전역 함수인 `enumerate`를 사용해서 배열을 돌릴 수 있다. `enumerate` 함수는 배열내 각각의 값에 대해 인덱스와 결합한 튜플 값을 반환한다. 반복문을 돌리는 도중 이 튜플을 변수나 상수로 분리하여 사용할 수 있다:

```
for (index, value) in enumerate(shoppingList) {
println("Item \(index + 1): \(value)")
}

// Item 1: Six eggs
// Item 2: Milk
// Item 3: Flour
// Item 4: Baking Powder
// Item 5: Bananas
```

`for-in` 반복문에 대해서는 [For 반복문]() 항목을 참고하도록 하자.


### 배열의 생성과 초기화 Creating and Initializing an Array

배열의 초기화 문법을 이용하면 초기값 할당 없이 특정 타입을 가진 빈 배열을 만들 수 있다:

```
var someInts = Int[]()
println("someInts is of type Int[] with \(someInts.count) items.")

// prints "someInts is of type Int[] with 0 items."
```

`someInts` 변수의 타입은 `Int[]`로 추정 가능한데, 이것은 `Int[]`로 초기화를 했기 때문이다.

또한 만약 컨텍스트 상에서 함수의 인자라든가 이미 타입 선언이 된 변수 혹은 상수라든가 하는 식으로 해서 이미 타입 정보를 갖고 있다면, 빈 배열을 곧바로 빈 배열 표현식을 이용하여 만들 수 있다. 빈 배열 표현식은 `[]`와 같이 대괄호만을 이용한다:

```
someInts.append(3)
// someInts now contains 1 value of type Int
someInts = []
// someInts is now an empty array, but is still of type Int[]
```

스위프트의 `Array` 타입도 특정 크기와 기본 값을 갖는 배열을 만들 수 있는 생성자를 제공한다. 배열에 들어갈 수 있는 값의 갯수(`count` 인자)와 기본 값(`repeatedValue` 인자)을 생성자에 제공하여 배열을 만들 수 있다:

```
var threeDoubles = Double[](count: 3, repeatedValue: 0.0)
// threeDoubles is of type Double[], and equals [0.0, 0.0, 0.0]
```

생성자를 사용할 때 기본 값에서 타입을 추정하기 때문에 배열 생성시 굳이 타입 지정을 할 필요가 없다:

```
var anotherThreeDoubles = Array(count: 3, repeatedValue: 2.5)
// anotherThreeDoubles is inferred as Double[], and equals [2.5, 2.5, 2.5]
```

마지막으로 이미 존재하는 같은 타입의 두 배열을 `+` 연산자를 통해 합치는 것만으로 새로운 배열을 생성할 수도 있다. 이렇게 만들어진 새로운 배열의 타입은 합치기 전 두 배열의 타입으로부터 추정 가능하다:

```
var sixDoubles = threeDoubles + anotherThreeDoubles

// sixDoubles is inferred as Double[], and equals [0.0, 0.0, 0.0, 2.5, 2.5, 2.5]
```


## 딕셔너리 Dictionaries

_딕셔너리_는 같은 타입을 가진 여러개의 값을 저장하는 하나의 컨테이너이다. 각각의 값은 유일한 키 값에 물려 있으며, 이 키 값은 딕셔너리 안에서 해당 값을 찾기 위한 식별자의 역할을 한다. 배열의 값들과 달리 딕셔너리 안에 저장된 값은 어떤 순서가 정해져 있지 않다. 실제로 사전에서 어떤 단어의 정의를 찾는 것과 매우 같은 방식으로 딕셔너리 안에 정의된 식별자를 이용해서 값을 찾는다.

스위프트의 딕셔너리는 특정한 타입의 키와 그에 따른 값을 저장한다. 이는 Objective-C에서 제공하는 `NSDictionary`와 `NSMutableDictionary` 클라스와는 다르다. `NSDictionary`와 `NSMutableDictionary` 클라스는 어느 종류의 객체든 키와 값으로 저장이 가능한 반면 그 저장된 객체의 속성에 대한 어떠한 정보도 제공하지 않는다. 스위프트에서는 특정 딕셔너리에 저장할 수 있는 키 타입과 밸류 타입은 항상 명시적인 타입 선언을 하거나 타입 추정을 통해 확인한다.

스위프트의 딕셔너리 타입은 `Dictionary<KeyType, VaueType>` 형태로 쓰인다. 여기서 `KeyType`은 딕셔너리의 키 값으로 쓰이는 값에 대한 타입이고, `ValueType`은 딕셔너리의 키 값에 맞추어 저장하고자 하는 밸류의 타입을 정의하는 것이다.

딕셔너리가 갖고 있는 유일한 제약사항은 반드시 `KeyType`은 해시 가능한 타입이어야 한다. 즉, 그 자체로 유일하게 표현이 가능한 방법을 제공해야 한다는 것이다. 스위프트의 모든 기본 타입들 (`String`, `Int`, `Double`, `Bool`)은 기본적으로 해시 가능한 것들이므로 딕셔너리의 키 타입으로 사용 가능하다. 연관된 값이 없는 열거형의 멤버 값들 역시도 기본적으로 해시 가능한 타입이다. ([Enumerations]() 참조)


### 딕셔너리 표현식 Dictionary Literals

딕셔너리는 딕셔너리 표현식을 통해서 초기화를 시킬 수 있다. 딕셔너리 표현식은 앞에서 살펴봤던 배열 표현식과 비슷한 문법을 갖는다. 딕셔너리 표현식은 하나 또는 그 이상의 키/밸류 쌍을 딕셔너리 컬렉션에 담는 축약 형태를 가리킨다.

키/밸류 쌍은 키와 밸류의 조합이다. 딕셔너리 표현식에서 각각의 키/밸류 쌍 안에서 키와 밸류는 콜론으로 나뉜다. 키/밸류 쌍은 리스트로써, 콤마로 나뉘고 대괄호로 감싼다:

```
[ key 1 : value 1 , key 2 : value 2 , key 3 : value 3 ]
```

아래 예제는 국제공항들의 이름들을 저장하는 딕셔너리를 생성한다. 이 딕셔너리에서 키 값은 국제공항 코드 (IATA 코드)를 나타내는 세글자 코드이며 밸류는 공항의 이름이다:

```
var airports: Dictionary<String, String> = ["TYO": "Tokyo", "DUB": "Dublin"]
```

`airports` 딕셔너리는 `Dictionary<String, String>` 타입을 갖게끔 정의했으며 이것은 "`Dictionary` 타입으로서 `String` 타입의 키, `String` 타입의 밸류를 갖는다"는 것을 의미한다.

>NOTE
`airports` 딕셔너리는 `let` introducer를 이용한 상수형 대신 `var` introducer를 이용하여 변수로 정의하였다. 이는 아래 예제들에서 이 딕셔너리에 계속해서 공항들을 추가할 것이기 때문이다.

`airports` 딕셔너리는 두 개의 키/밸류 쌍을 포함하는 딕셔너리 표현식을 통해 초기화를 시켰다. 첫번째 쌍은 "`TYO`" 라는 키에 "`Tokyo`" 라는 밸류를 갖는다. 두번째 쌍은 "`DUB`" 라는 키에 "`Dublin`" 이라는 밸류를 갖는다.

이 딕셔너리 표현식은 두개의 `String:String` 쌍을 포함한다. 이것은 `airports` 타입의 정의인 `String` 타입의 키와 `String` 타입의 밸류를 갖는 딕셔너리와 일치한다. 따라서 딕셔너리 표현식을 이용해서 `airpots` 딕셔너리 변수를 두개의 초기값으로 초기화 시킬 수 있다.

배열과 같이 딕셔너리 표현식의 키/밸류 쌍이 갖는 타입이 일정하다면 딕셔너리 타입을 정의할 필요가 없다. `aiports`의 초기화는 아래와 같은 축약 형태로 표현할 수 있다:

```
var airports = ["TYO": "Tokyo", "DUB": "Dublin"]
```

딕셔너리 표현식 안의 모든 키 값의 타입이 서로 같고, 마찬가지로 모든 밸류 타입이 서로 같기 때문에, 스위프트는 `Dictionary<String, String>` 타입이 `airports` 딕셔너리에 적용 가능하다고 추정할 수 있다.


### 딕셔너리의 접근 및 수정 Accessing and Modifying a Dictionary

딕셔너리는 메소드와 프로퍼티를 통해 접근과 수정이 가능하다. 혹은 subscript 문법을 사용할 수도 있다. 배열과 같이 딕셔너리 안에 값이 몇 개나 있는지를 확인하기 위해 읽기 전용 속성인 `count` 프로퍼티를 사용한다: 

```
println("The dictionary of airports contains \(airports.count) items.")
// prints "The dictionary of airports contains 2 items."
```

딕셔너리에 새 아이템을 추가하기 위해 subscript 문법을 사용할 수 있다. 같은 타입의 새 키를 subscript 인덱스로 사용하여 같은 타입의 새로운 밸류를 할당한다:

```
airports["LHR"] = "London"
// the airports dictionary now contains 3 items
```

Subscript 문법을 사용하여 특정 키에 물려 있는 값을 변경시킬 수도 있다:

```
airports["LHR"] = "London Heathrow"
// the value for "LHR" has been changed to "London Heathrow"
```

또다른 subscripting 방법으로써, 딕셔너리의 `updateValue(forKey:)` 메소드를 사용하여 특정 키에 해당하는 값을 설정하거나 변경할 수 있다. 위의 Subscript 예제와 같이 `updateValue(forKey:)` 메소드는 만약 키가 존재하지 않을 경우에는 값을 새로 설정하거나 키가 이미 존재한다면 기존의 값을 수정한다. 하지만 subscript와는 달리 `updateValue(forKey:)` 메소드는 업데이트를 하고난 뒤 이전 값을 반환한다. 이렇게 함으로써 실제로 업데이트가 일어났는지 아닌지를 확인할 수 있게 된다.

`updateValue(forKey:)` 메소드는 딕셔너리의 밸류 타입에 해당하는 `Optional` 값을 반환한다. 예를 들어 어떤 딕셔너리가 `String` 밸류를 저장한다면 이 메소드는 `String?` 타입 또는 "Optional `String`" 타입의 밸류를 반환한다. 이 Optional 밸류는 만약 키가 이미 있었다면 수정하기 이전 밸류를, 아니라면 `nil`을 갖는다:

```
if let oldValue = airports.updateValue("Dublin International", forKey: "DUB") {
   println("The old value for DUB was \(oldValue).")
}
// prints "The old value for DUB was Dublin."
```

Subscript 문법을 이용하면 특정 키 값에 대응하는 밸류를 딕셔너리에서 찾을 수 있다. 값이 존재하지 않는 키를 요청할 수 있기 때문에 딕셔너리는 딕셔너리의 밸류 타입에 해당하는 Optional 밸류를 반환한다. 만약 딕셔너리가 요청한 키에 대응하는 밸류를 갖고 있다면, Subscript 는 그 키에 대응하는 밸류를 Optional 밸류를 반환한다. 아니라면 Subscript는 `nil`을 반환한다:

```
if let airportName = airports["DUB"] {
    println("The name of the airport is \(airportName).")
} else {
    println("That airport is not in the airports dictionary.")
}
// prints "The name of the airport is Dublin International."
```

Subscript 문법을 이용해 `nil` 값을 특정 키에 할당하는 것으로 딕셔너리에서 키/밸류 쌍을 삭제할 수 있다:

```
airports["APL"] = "Apple International"
// "Apple International" is not the real airport for APL, so delete it
airports["APL"] = nil
// APL has now been removed from the dictionary
```

또는 키/밸류 쌍을 딕셔너리에서 삭제할 때 `removeValueForKey` 메소드를 이용할 수 있다. 이 메소드는 키/밸류 쌍을 삭제하고 삭제된 값을 반환하거나 값이 없다면 `nil`을 반환한다:

```
if let removedValue = airports.removeValueForKey("DUB") {
    println("The removed airport's name is \(removedValue).")
} else {
    println("The airports dictionary does not contain a value for DUB.")
}
// prints "The removed airport's name is Dublin International."
```

### 딕셔너리에서 반복문 사용하기 Iterating Over a Dictionary 

`for-in` 반복문을 사용하면 딕셔너리 안의 모든 키/밸류 쌍에 접근할 수 있다. 딕셔너리 각각의 아이템은 `(key, value)` 튜플을 반환하고, 반복문을 돌리는 도중 이 튜플의 멤버들을 분리하여 임시 상수 혹은 변수에 할당하여 사용할 수 있다:


```
for (airportCode, airportName) in airports {
    println("\(airportCode): \(airportName)")
}
// TYO: Tokyo
// LHR: London Heathrow
```

`for-in` 반복문에 대한 자세한 내용은 "For 반복문"\*링크필요\* 섹션을 참고하도록 하자.

또한 딕셔너리의 `keys`, `values` 프로퍼티를 이용하면 키 또는 밸류 컬렉션을 반복문으로 돌릴 수 있다:

```
for airportCode in airports.keys {
    println("Airport code: \(airportCode)")
}
// Airport code: TYO
// Airport code: LHR

for airportName in airports.values {
    println("Airport name: \(airportName)")
}
// Airport name: Tokyo
// Airport name: London Heathrow
```

만약 딕셔너리의 키 콜렉션, 밸류 콜렉션을 `Array` 인스턴스를 이용하고 싶다면, 딕셔너리의 `keys`, `values` 프로퍼티를 배열로 초기화하여 사용할 수 있다:

```
let airportCodes = Array(airports.keys)
// airportCodes is ["TYO", "LHR"]

let airportNames = Array(airports.values)
// airportNames is ["Tokyo", "London Heathrow"]
```

> NOTE
스위프트의 `Dictionary` 타입은 순서를 정하지 않는 컬렉션이다. 키, 밸류, 키/밸류 쌍의 순서는 반복문을 돌릴때 정해지지 않는다.


### 빈 딕셔너리 만들기 Creating an Empty Dictionary 

배열과 마찬가지로 초기화 문법을 이용하여 비어있는 딕셔너리 타입을 만들 수 있다:

```
var namesOfIntegers = Dictionary<Int, String>()
// namesOfIntegers is an empty Dictionary<Int, String>
```

이 예제는 `Int, String` 타입을 갖는 빈 딕셔너리를 만든다. 키는 `Int` 타입, 밸류는 `String` 타입이다.

만약 컨텍스트에서 이미 해당 타입에 대한 정보를 제공한다면 빈 딕셔너리 표현식을 이용하여 딕셔너리를 초기화하여 만들 수 있다. 빈 딕셔너리 표현식은 `[:]` 으로 나타낼 수 있다:

```
namesOfIntegers[16] = "sixteen"
// namesOfIntegers now contains 1 key-value pair
namesOfIntegers = [:]
// namesOfIntegers is once again an empty dictionary of type Int, String
```

>NOTE
스위프트의 배열과 딕셔너리 타입은 제너릭 컬렉션을 구현한다. 제너릭 타입과 컬렉션에 대한 더 자세한 내용은 [제너릭]() 섹션을 참고하도록 하자.


## 컬렉션의 변경 가능성 Mutability of Collections

배열과 딕셔너리는 하나의 컬렉션 안에 여러개의 값을 저장한다. 만약 어떤 변수를 배열이나 딕셔너리 형태로 만든다면 이 컬렉션은 변경이 가능하다. 이는 컬렉션이 초기화된 후에도 여기에 아이템을 더 추가한다거나 뺀다거나 하는 식으로 컬렉션의 크기를 변경시킬 수 있다는 것을 의미한다. 반면에 배열이나 딕셔너리를 상수에 할당한다면 이때에는 컬렉션의 값도, 크기도 바꿀 수 없다.

이러한 불변성 딕셔너리는 기존의 키에 대응하는 값을 바꿀 수 없다는 것을 의미한다. 다시 말해서 불변성 딕셔너리라면 한 번 값이 설정된 후에는 절대로 바꿀 수 없다.

그러나 배열에서 이러한 불변성은 살짝 다른 의미를 갖는다. 불변성 배열의 크기를 바꿀 가능성이 있는 어떤 것도 할 수 없지만, 기존의 배열 인덱스에 새로운 값을 설정하는 것은 가능하다. 이것은 배열의 크기가 고정될 경우, 스위프트의 `Array` 타입에 배열 연산과 관련하여 최적의 성능을 제공한다.

스위프트가 제공하는 `Array` 타입의 변경 가능성은 또한 어떻게 배열 인스턴스가 생성되고 변경되는지에 대해서도 영향을 미친다. 더 자세한 내용은 [컬렉션 타입에서 할당과 복사 형태]() 섹션을 참조하도록 하자.

> NOTE 
컬렉션의 크기를 변경시킬 필요가 없는 경우에는 불변성 컬렉션을 만드는 것이 좋다. 이렇게 함으로써 스위프트 컴파일러가 컬렉션의 퍼포먼스에 최적화를 시킬 수 있다.

