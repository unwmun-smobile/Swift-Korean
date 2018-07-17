---
id: chapter8
title: 함수 (Functions)
---
> Translator : Quartet( ungsik.yun@gmail.com )

함수는 특정 일을 수행하는 자기 완결성(Self-contained)을 가진 코드들의 집합입니다. 함수의 이름을 지으면서 이 함수가 무엇을 하는지 식별하게 할 수 있습니다. 그리고 그 이름으로 함수를 "호출(Call)"하여 필요할때 함수의 일을 수행하게 만들 수 있습니다.
스위프트(Swift)의 함수 문법은 파라메터가 없는 C스타일의 함수에서부터 지역 파라메터와 파라메터 이름 각각에 대한 외부 파라메터를 가지고 있는 복잡한 오브젝티브-C 스타일의 함수까지 전부 표현할 수 있습니다. 파라메터는 기본 값을 가질수 있어 단순한 함수 호출에 쓰일수 있습니다. 또한 In-out 파라메터로서 변수를 넘겨 변수가 함수의 실행후에 파라메터가 변경되게 할 수도 있습니다.
파라메터 타입과 반환(Return) 타입으로 이루어진 모든 스위프트의 함수들은 타입을 가집니다. 스위프트에 있는 다른 타입들과 마찬가지로, 함수의 타입들을 사용할 수 있습니다. 즉 함수를 다른 함수에 파라메터로서 넘겨주거나 함수를 다른 함수에서 반환받을 수 있습니다. 함수들은 유용한 기능 캡슐화를 위해 중첩된 함수안의 범위 내에서 쓰여질수도 있습니다.

## 함수 정의와 호출
함수를 정의할때 함수의 입력(파라메터)을 하나 이상의 이름이 있고 타입이 정해진 값으로 할 수 있습니다. 또한 값의 타입은 함수의 실행이 끝났을때 함수가 되돌려줄 수 있습니다 (반환 타입).
모든 함수는 함수명을 가지고 있으며, 함수명은 함수가 하는일을 설명해줍니다. 함수를 사용하기위해서 함수를 함수의 이름을 사용하여 "호출"하고 함수의 파라메터 타입들과 일치하는 입력 값들(아규먼트Arguments)을 넘겨줍니다. 함수의 입력값은 함수의 파라메터리스트와 언제나 일치해야합니다.
아래의 함수 예제 이름은 `greetingForPerson`입니다. 함수가 행하는 일이 바로 그것(사람에게 환영인사Greeting for person)이기 때문입니다. 입력으로 사람의 이름을 받아서 그 사람에 대한 환영 인사를 반환합니다. 이를 달성하기 위해 파라메터를 하나 정의하고 - `personName`이라는 `String` 값 - 반환 타입을 `String`으로 하여 사람에 대한 환영 인사를 하는 것입니다.

```
func sayHello(personName: String) -> String {
  let greeting = "Hello, " + personName + "!"
  return greeting
}
```
이 모든 정보들은 `func` 키워드 접두어를 쓰는 함수의 정의안에 포함이 되게됩니다. 함수의 반환 타입을 표시하기위해 함수 이름의 오른쪽에 화살표(하이픈과 우측 꺽괄호) `->` 와 반환할 타입을 표시합니다.
함수 정의는 함수가 무엇을 하는지, 무엇을 파라메터로 받는지 완료되었을때 무엇을 반환하는지 설명합니다. 함수 정의는 함수가 코드안에서 호출될때 명확하고 애매함이 없는 방법으로 사용될수 있게합니다:

```
println(sayHello("Anna"))
// prints "Hello, Anna!"
println(sayHello("Brian"))
// prints "Hello, Brian!"
```
`sayHello` 함수를 괄호안에 `String` 타입의 인수를 넣어서 호출합니다. 예를들면 `sayHello("Anna")` 처럼 말이죠. `sayHello`가 `String`타입을 반환하기에 `sayHello`함수는 `println`로 싸여서 호출될 수 있습니다. 이렇게 함으로서 `println`함수가 `sayHello`함수의 반환값을 위에 보이는 것처럼 출력할 수 있습니다.
`sayHello`함수의 몸체는 `greeting`이라는 새 `String` 상수를 선언하는 것으로 시작합니다. 그리고 `greeting`을 `personName`에 대한 단순한 환영인사 메시지로 설정합니다. 이 환영 인사는 `return`키워드를 통해 함수의 밖으로 되돌려지게 됩니다. `return greeting`이 실행 되면 함수의 실행은 끝나게되고, `greeting`의 현재 값을 돌려주게됩니다.
`sayHello` 함수를 다른 입력값으로 여러번 호출할 수 있습니다. 위의 예제는 입력값이 "Anna", "Brian" 일때를 각각 보여주고 있습니다. 함수는 사람(입력값)에 맞게끔 환영인사를 각각의 경우에 맞추어 돌려줍니다.
함수 몸체를 단순화하기 위해서는, 메시지의 생성과 반환을 한줄로 합치면 됩니다:
```
func sayHelloAgain(personName: String) -> String {
	return "Helloagain, " + personName + "!"
}
println(sayHelloAgain("Anna"))
// prints "Helloagain, Anna!"
```
## 함수 파라메터와 반환값
스위프트에서 함수 파라메터와 반환값은 극도로 유연합니다. 이름없는 파라메터를 사용하는 단순한 기능성 함수에서부터 명시적 파라메터 이름(expressive parameter names)과 다른 파라메터 옵션을 가진 복잡한 함수에 이르기까지 무엇이든 정의할수 있습니다.

### 파라메터 복수 입력
함수는 괄호 안에서 콤마(`,`)로 구분되는 복수의 입력 파라메터를 가질수 있습니다. 
이 함수는 반개영역(half-open range)의 시작과 끝의 인덱스를 받아 얼마나 많은 요소(elements)들이 영역안에 있는지 계산합니다:
```
func halfOpenRangeLength(start: Int, end: Int) -> Int {
	return end - start
}
println(halfOpenRangeLength(1, 10))
// prints "9"
```
### 파라메터가 없는 함수
함수에 입력 파라메터를 정의할 필요는 없습니다. 밑의 예제는 입력 파라메터가 없는 함수입니다. 이 함수는 호출될때마다 언제나 같은 메시지를 반환합니다.
```
func sayHelloWorld() -> String {
	return "hello, world"
}
println(sayHelloWorld())
// prints "hello, world"
``` 
함수 정의는 아무런 파라메터를 받지 않는다고 해도 함수 이름뒤에 괄호를 포함해야 합니다. 함수가 호출될 때도 함수 이름뒤에 빈 괄호 한쌍을 표시해야 합니다.

### 반환값이 없는 함수
함수에 반환 타입을 정의할 필요는 없습니다. 밑의 예제는 `sayHello`의 `waveGoodbye`라 불리는 버전입니다. 값을 반환하지 않고 자신의 `String`값을 출력합니다.
```
func sayGoodbye(personName: String) {
	println("Goodbye, ` \(personName)! ")
}
sayGoodbye("Dave")
// prints "Goodbye, Dave!"
```    
반환값이 없기 때문에 함수 정의는 반환 화살표(`->`)나 반환 타입을 포함하지 않습니다.

> NOTE
엄밀히 말하자면, `sayGoodbye` 함수는 반환값이 정의되어있지 않아도 여전히 반환값을 가집니다.  반환값이 정의되어있지 않은 함수는 `Void`타입의 특수값을 반환합니다. `()`로 쓰여질수 있는 단순한 빈 튜플(Tuple)이며, 사실상 요소를 갖고있지 않은 튜플입니다.

함수가 호출되었을때 함수의 반환값은 무시될수 있습니다.
```
func printAndCount(stringToPrint: String) -> Int {
  println(stringToPrint)
  return countElements(stringToPrint)
}
func printWithoutCounting(stringToPrint: String) {
  printAndCount(stringToPrint)
}
printAndCount("hello, world")
// prints "hello, world" and returns a value of 12
printWithoutCounting("hello, world")
// prints "hello, world" but does not return a value
```
첫번째 함수인 `printAndCount`는 문자열을 출력하고 출력한 문자열의 캐릭터 갯수를 세서 Int 타입으로 반환합니다. 두번째 함수인 `printWithoutCounting`은 첫번째 함수를 호출합니다. 하지만 반환값은 무시합니다. 두번째 함수가 호출되면 메시지는 첫번째 함수에 의해 여전히 출력되지만, 첫번째 함수의 반환값은 사용되지 않습니다.

> NOTE
반환값은 무시될수 있습니다. 하지만 함수는 언제나 값을 반환할것입니다. 반환 타입이 정의된 함수는 값을 반환하지 않은채로 함수가 실행 될수 없습니다. 그렇게 하려고 시도할 경우 컴파일 에러를 낼 것입니다. 

### 여러개의 반환값을 가지는 함수
튜플 타입은 하나의 합성된 반환값으로서 함수의 반환에 사용될 수 있습니다.
아래의 예제는 `count`라는 함수의 정의입니다. 이 함수는 아메리칸 영어에서 사용되는 표준 모음과 자음을 기반으로 모음과 자음 그리고 다른 문자들을 문자열에서 셉니다.
```
func count(string: String) -> (vowels: Int, consonants: Int, others: Int) {
  var vowels = 0, consonants = 0, others = 0
  for character in string {
    switch String(character).lowercaseString {
      case "a", "e", "i ", "o", "u":
      ++vowels
      case "b", "c", "d", "f", "g", "h", "j", "k", "l ", "m",
      "n", "p", "q", "r", "s", "t", "v", "w ", "x", "y", "z":
      ++consonants
      default:
      ++others
    }
  }
  return (vowels, consonants, others)
}
```
이 `count` 함수를 이용함으로서 임의의 문자열의 문자 갯수를 셀수 있습니다.  그리고 세 개의 이름있는 `Int` 값으로 구성된 튜플로 그 값을 받아옵니다.
```
let total  = count("some arbitrary string! ")
println("\(total.vowels) vowels and \(total.consonants) consonants")
// prints "6 vowels and 13 consonants"
```
튜플의 멤버들은 함수 내에서 반환할때 이름을 지을 필요가 없습니다. 함수 정의시에 함수의 반환 타입에 이미 명시가 되어있기 때문입니다. 

## 함수 파라메터 이름
위의 모든 함수들은 함수 자신의 파라메터로 파라메터 이름을 정의하고 있습니다.
```
func someFunction(parameterName: Int) {
  // function body goes here, and can use parameterName
  // to refer to the argument value for that parameter
}
```
하지만 그러한 파라메터 이름들은 오직 함수 자신의 몸체(Body) 안에서만 사용될 수 있습니다. 또한 함수를 호출할때는 사용할 수 없습니다. 그러한 종류의 파라메터 이름은 지역 파라메터 이름(local parameter names)이라고 합니다. 오직 함수의 내부(Body)에서만 사용할 수 있기 때문입니다.

### 외부 파라메터 이름(External Parameter Names)
때때로 각각의 파라메터의 이름을 함수를 호출할때 지어주는 것이 유용할때가 있습니다. 함수에게 어떤 인수가 어떤 목적인지 지시하기 위해서죠.
함수 사용자에게 파라메터 이름을 제공하고 싶다면 지역 파라메터 이름과 외부 파라메터 이름을 정의하면 됩니다. 외부 파라메터 이름은 지역 파라메터 이름 바로 앞에 공백으로 구분해서 작성합니다.
```
func someFunction(externalParameterName localParameterName: Int) {
  // function body goes here, and can use local ParameterName
  // to refer to the argument value for that parameter
}
```
> NOTE
만약 외부 파라메터 이름이 파라메터에 대해 제공된다면, 외부 파라메터 이름은 언제나 함수 호출시에 사용되어야 합니다.

예를 들어 다음과 같은 함수가 있다고 합시다. 이 함수는 두 문자열 사이에  `joiner` 문자열을 삽입해 연결하는 함수입니다.
```
func join(s1: String,  s2: String,  joiner: String) -> String {
	return s1 + joiner + s2
}
```    
이 함수를 호출할때 함수로 전달되는 세 문자열의 목적이 불분명합니다.
```
    join("hello", "world", ", ")
    // returns "hello, world"
```
문자열 값들의 목적을 명확하게 하기 위해, 외부 파라메터를 `join`함수의 각각의 파라메터에 정의합니다.

```
func join(string s1: String,  toString s2: String,  withJoiner joiner: String) 
	-> String {
		return s1 + joiner + s2
}
```
 
이 버전의 join 함수에서는 첫번째 파라메터의 외부 이름은 `string`이며 지역 이름은 `s1`이다. 두번째 파라메터는 외부 이름으로 `toString`을 쓰고 지역 이름은 `s2`이다. 그리고 세번째 파라메터는 외부 이름으로 `withJoiner`를 쓰고 지역 이름은 `joiner`이다.
이제 외부 파라메터 이름을 사용하여 함수를 호출할때 명확하고 애매하지 않은 방법으로 호출할 수 있게 되었다.
```
join(string: "hello",  toString: "world",  withJoiner: ", ")
// returns "hello, world"
```    
외부 파라메터 이름의 사용은 이 두번째 `join`함수를 명시적이며 말이 되는(sentence-like) 방법으로 사용자들이 호출할 수 있게 합니다. 함수 몸체는 여전히 가독성이 좋고 명확한 의도를 가진채로 말이죠. (while still providing a function body that is readable and clear in intent.)

>NOTE
누군가가 코드를 처음 보았을때 명확하지 않을 수 있다면 외부 파라메터 이름을 쓰는것을 언제나 고려하십시오. 만약 함수가 호출될때 각각의 파라메터들의 목적이 명확하고 모호하지 않다면 외부 파라메터 이름을 정할 필요는 없습니다.

### 단축 외부 파라메터 이름
만약 함수의 외부 파라메터 이름을 제공하려 할때 이미 해당 파라메터의 내부 이름(local parameter name)이 이미 적절한 이름을 가지고 있다면, 똑같은 이름을 두번 쓸 필요가 없습니다. 대신 파라메터 이름을 한번 쓰고, 이름의 접두어로 해시 심볼(hash symbol) (`#`)을 붙입니다. 이렇게 함으로서 스위프트는 해당 이름을 외부 파라메터 이름과 지역 파라메터 이름으로 동시에 쓰게 됩니다.
이 예제는 `containsCharacter` 함수를 정의하고 호출합니다. 해당 함수는 두 입력 파라메터에 `#`을 붙여서 같은 이름으로 외부 파라메터 이름과 내부 파라메터 이름으로 쓰이게 했습니다.
```
func containsCharacter(#string: String,  #characterToFind: Character) -> Bool {
  for character in string {
    if character == characterToFind {
    	return true
    	}
    }
  return false
  }
```
이 함수의 파라메터 이름 선정은 함수 몸체를 명확하고 가독성있게 하며 동시에 함수 호출시에 모호함이 없게 했습니다.
```
let containsAVee = containsCharacter(string: "aardvark", characterToFind: "v")
// containsAVee equals true,  because "aardvark" contai ns a "v"
```
### 기본(default) 파라메터 값
함수 정의의 일부로서 파라메터의 기본 값을 지정해줄 수 있다. 기본값이 지정되어 있으면 함수를 호출할때 해당 파라메터를 생략할 수 있다.

>NOTE
기본값을 가지는 파라메터는 함수의 파라메터 리스트에서 마지막에 둔다. 이렇게 함으로써 함수 호출이 기본값을 가지지 않는 파라메터들이 언제나 같은 순서임을 보장할 수 있고, 매번 함수가 호출될 때마다 같은 함수가 호출되게 한다.

이 함수는 앞서 보인 `join`함수의 `joiner` 파라메터에 기본값을 부여한 버전입니다.
```
func join(string s1: String, toString s2: String, withJoiner joiner: String = " ") 
-> String {
	return s1 + joiner + s2
}
```
만약 `join`함수의 `joiner` 문자열 값이 주어지면, 앞서 보았던 것처럼 해당 문자열 값이 두 문자열을 붙이는데 사용됩니다.
```
join(string: "hello", toString: "world", withJoiner: "-")
// returns "hello-world
```
하지만 아무런 값이 `joiner`에 주어지지 않는다면, 기본값인 공백 한칸 (`" "`)이 대신 사용됩니다.
```
join(string: "hello", toString: "world")
// returns "hello world"
```
### 기본값을 가지는 외부 파라메터 이름
대부분의 경우 외부 파라메터 이름에 기본값을 제공(외부 파라메터이기에 요구되기도 하는)하는 것은 유용하다. 그렇게 함으로써 함수가 호출될때 인수가 파라메터에 대해 가지는 목적이 명확해집니다.
이 과정을 쉽게 하기위해, 외부 이름을 부여하지 않은 파라메터에 대해 스위프트는 자동 외부 이름을 기본값이 정의되어 있는 파라메터에 대해 제공합니다. 자동 외부 이름은 앞서 본 해시 심볼(`#`)을 사용한 것처럼, 지역 이름과 똑같은 이름이 됩니다. 
여기에 `joiner` 문자열 값에 기본값을 부여하였지만, 파라메터 일체에 외부 파라메터 이름은 주지 않은 버전의 `join`함수가 있습니다.
```
func join(s1: String, s2: String, joiner: String = " ") -> String {
	return s1 + joiner + s2
}
```
이 경우에 스위프트는 자동적으로 외부 파라메터 이름을 기본값이 있는 파라메터 `joiner`에 대해 부여합니다. 그러므로 외부 이름은 반드시 함수가 호출 될 때에 제공되어야 하며, 파라메터의 목적을 명확하고 모호하지 않게 합니다.
```
join("hello", "world", joiner: "-")
// returns "hello-world"
```
>NOTE
함수를 정의할때 명시적인 외부 이름을 쓰는 것 대신에 밑줄(`_`)을 씀으로써 이 동작을 수행하지 않게 할 수 있습니다. 하지만 기본값을 가진 파라메터에 적절한 외부 이름을 제공하는것은 언제나 바람직합니다.

### 가변 갯수(Variadic) 파라메터
가변 갯수 파라메터는 특정 타입의 값을 0개 이상 받을 수 있습니다. 가변 갯수 파라메터를 사용함으로써 함수 호출시 입력 값들이 임의의 갯수가 될수 있다고 정할 수 있습니다. 파라메터의 타입 이름의 뒤에 마침표 세개(`...`)를 삽입하는 것으로 가변 갯수 파라메터를 작성할 수 있습니다.
가변 갯수 파라메터로 함수의 내부에 전달된 값들은 적절한 타입의 배열(`array`)로 만들어집니다. 예를 들어 `numbers`라는 이름의 가변 갯수 파라메터의 타입이 `Double...`이라면 함수의 내부에서는 `Double[]`타입의 `numbers` 이름을 가진 배열로 만들어집니다.
밑의 예제는 평균이라 불리는 산술 평균(arithmetic mean)을 임의의 갯수를 가진 숫자의 목록에서 구합니다.
```
func arithmeticMean(numbers: Double...) -> Double {
 	 var total : Double = 0
	  for number in numbers {
  		total += number
	}
	return total / Double(numbers.count)
}
arithmeticMean(1, 2, 3, 4, 5)
// returns 3.0, which is the arithmetic mean of these five numbers
arithmeticMean(3, 8, 19)
// returns 10.0, which is the arithmetic mean of these three numbers
```
> NOTE
함수는 최대 한개의 가변 갯수 파라메터를 가질 수 있습니다. 그리고 가변 갯수 파라메터는 언제나 파라메터 목록의 마지막에 배치되어야 합니다. 이렇게 함으로써 복수의 파라메터를 가진 함수를 호출할때 생기는 모호함을 피할 수 있습니다.
만약 함수의 파라메터중 하나 이상의 파라메터가 기본값을 가지고, 그와 동시에 가변 갯수 파라메터를 가진다면 가변 갯수 파라메터는 기본 값을 가지는 파라메터의 맨 마지막에 두어야합니다.

### 상수(Constant)와 가변(Variable) 파라메터
함수의 파라메터들은 기본적으로 상수들입니다. 함수의 내부에서 파라메터의 값을 바꾸려 시도하는 것은 컴파일 에러를 냅니다. 이렇게 함으로써 실수로 파라메터가 바뀌지 않게 합니다.
하지만 때로는 함수가 파라메터의 값을 복사하여 다양하게 사용하는 것이 유용할때가 있습니다. 새로운 변수(variable)를 정의하지 않고 대신 가변 파라메터를 하나 이상 지정하여 함수 내부에서 사용할 수 있습니다. 가변 파라메터는 상수보다는 변수처럼 사용 가능하며, 함수가 이용하는 파라메터의 변경 가능한 사본을 제공합니다.
가변 파라메터를 정의하려면 파라메터의 이름 앞에 `var` 키워드를 접두어로 사용합니다.
```
func alignRight(var string: String, count: Int, pad: Character) -> String {
    let amountToPad = count - countElements(string)
    for _ in 1...amountToPad {
        string = pad + string
    }
    return string
}
let originalString = "hello"
let paddedString = alignRight(originalString, 10, "-")
// paddedString is equal to "-----hello"
// originalString is still equal to "hello"
```
이 예제는 `alignRight`라는 함수를 새로 정의하고 있습니다. 이 함수는 입력 문자열을 오른쪽 가장자리로 정렬하고 더 긴 출력 문자열을 만듭니다. 문자열의 왼쪽에 생긴 공간에는 정해진 채움 문자로 채워집니다. 이 예제에서는 "hello"라는 문자열이 "-----hello"로 변환되었습니다.
`alignRight`함수는 입력 파라메터 `string`을 가변 파라메터로 정의하고, `string`이 지역 변수(variable)로서 사용될 수 있는 문자열 값으로 초기화 되며, 함수 내부에서 변경될 수 있습니다.
이 함수는 우측 정렬된 전체 문자열 안에 얼마나 많은 채움 문자가 `string`의 왼쪽에 들어가야 할지 계산하는 것으로 시작합니다. 이 값은 지역 상수인 `amountToPad`에 저장됩니다. 그리고 함수는 `amountToPad`만큼 `pad`문자를 기존 문자열의 왼쪽에 붙여넣고 그 값을 반환합니다. 이러한 문자열 변경 과정에서 `string` 가변 파라메터가 사용됩니다.

>NOTE
가변 파라메터에 생긴 변화는 각각의 함수 호출이 끝난 뒤에는 남아있지 않습니다. 또한 함수의 외부에서는 보이지(visible)않습니다. 가변 파라메터는 함수 호출이 되는 동안만 유지됩니다.

### 입출력(In-Out)파라메터
위에 설명 된 것과 같이 가변 파라메터는 오직 함수 자신의 내부에서만 변경 될 수 있습니다. 만약 함수가 파라메터의 값을 변경하고 그 변경이 함수 호출이 종료된 후에도 계속되길 원한다면, 파라메터를 _입출력_ 파라메터로 정의하면 됩니다.
_입출력_파라메터를 정의하기 위해서는 `inout` 키워드를 파라메터 정의의 시작점에 작성하면 됩니다. 입출력 파라메터의 값은 함수의 _안으로_ 전달 되어, 함수에 의해 변경되고, 함수에서 다시 _나와서_ 원래의 값을 대체합니다.
입출력 파라메터로 넘길 수 있는 값은 인수(argument)뿐입니다. 상수나 문자 값은 입출력 파라메터로 넘겨질 수 없습니다. 상수나 문자값은 변경될 수 없기 때문입니다. 인수를 입출력 파라메터로 넘길때 변수의 이름 바로 앞에 앰퍼샌드(`&`)를 붙여서 이 파라메터가 함수에 의해 변경될 수 있음을 표시합니다.

>NOTE
입출력 파라메터는 기본값을 가질 수 없습니다. 또한 가변 갯수 파라메터도 `inout`으로 지정할 수 없으며 `var`나 `let`으로 표시될 수도 없습니다. 

여기에 `swapTwoInts`라는 함수 예제가 있습니다. 이 함수는 두개의입출력 정수(integer) 파라메터인 `a`와 `b`를 가지고 있습니다.
```
func swapTwoInts(inout a: Int, inout b: Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```
`swapTwoInts` 함수는 단순히 두 값을 교환하여 `b`를 `a`의 값으로 하고, `a`를 `b`의 값으로 합니다. 이 함수는 `a`의 값을 임시 상수인 `temporaryA`에 저장하고, `b`의 값을 `a`로 할당합니다. 그리고 `temporaryA`의 값을 `b`로 할당합니다.
`swapTwoInts` 함수는 두 `Int` 타입의 변수를 가지고 서로의 값을 교환하는 함수라고 할 수 있습니다다. 주의할것은 `someInt`와 `anotherInt`는 앰퍼샌드 접두어가 쓰여서 함수에 전달되었다는 것입니다.
```
var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
println("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
// prints "someInt is now 107, and anotherInt is now 3"
```
위의 예제는 `someInt`와 `anotherInt`가 함수 외부에서 정의되었음에도, 그 값이 `swapTwoInts` 함수에 의해 변경 되었음을 보여주고 있습니다.

>NOTE
입출력 파라메터는 함수가 값을 반환하는 것이 아닙니다. 위의 `swapTwoInts` 예제는 반환 타입이나 반환값을 정의하고 있지 않습니다. 하지만 `someInt`와 `anotherInt`의 값을 변경하죠. 입출력 파라메터는 함수가 함수 밖의 범위(scope)에 영향을 끼칠 수 있는 또다른 방법입니다.

## 함수 타입
모든 함수들은 특유의 함수 타입을 가지고 있습니다. 함수 타입은 함수의 파라메터 타입들과 반환 타입으로 구성됩니다.
예를 들면 이렇습니다.
```
func addTwoInts(a: Int, b: Int) -> Int {
    return a + b
}
func multiplyTwoInts(a: Int, b: Int) -> Int {
    return a * b
}
```
이 예제는 `addTwoInts`와 `multiplyTwoInts`, 두개의 단순한 수학 함수를 정의합니다. 함수들은 각각 두개의 `Int`값을 취하고 `Int`값을 계산의 적절한 결과로서 반환합니다.
위 두 함수의 타입은 `(Int, Int) -> Int`입니다. 이것을 "함수 타입은 `Int`타입의 파라메터가 두개며 반환값의 타입은 `Int`다." 라고 말할 수 있습니다.
여기의 또다른 예제는 파라메터나 반환값이 없는 함수입니다.
```
func printHelloWorld() {
    println("hello, world")
}
```
이 함수의 타입은 `()->()`이며, "함수는 파라메터가 없고 `Void`를 반환한다."고 할 수 있습니다. 반환값이 정해지지 않은 함수는 언제나 `Void`를 반환하며, 이는 빈 튜플인 `()`로 표시될 수 있습니다.

### 함수 타입을 사용하기
함수 타입 역시 스위프트의 다른 타입들처럼  사용될 수 있습니다. 예를들어 함수 타입의 상수나 변수를 만들어서 적절한 함수를 할당할 수 있습니다.
```
var mathFunction: (Int, Int) -> Int = addTwoInts
```
위 코드는 "두개의 `Int` 값을 취하며 `Int`값을 반환하는 함수 타입 `mathFuntion`을 정의하고, 이 새로운 변수가 `addTwoInts` 함수를 참조(refer)하도록 한다."고 할 수 있습니다.
위에서 본 `addTwoInts` 함수는 `mathFunction`변수와 같은 타입입니다. 따라서 스위프트의 타입 체커에 의해 할당이 허용되죠.
이제 `mathFunction`을 이용해 할당된 함수를 호출할 수 있습니다.
```
println("Result: \(mathFunction(2, 3))")
// prints "Result: 5"
```
함수 타입이 아닌 변수가 그러하듯이, 일치하는 타입의 다른 함수 또한 같은 변수에 할당 될 수 있다.
```
mathFunction = multiplyTwoInts
println("Result: \(mathFunction(2, 3))")
// prints "Result: 6"
```
다른 타입과 마찬가지로, 함수를 상수나 변수에 할당할때 스위프트가 타입을 추론하게 내버려 둘 수 있다.
```
let anotherMathFunction = addTwoInts
// anotherMathFunction is inferred to be of type (Int, Int) -> Int페
```
### 파라메터 타입으로서의 함수 타입
`(Int, Int) -> Int`와 같은 함수 타입을 파라메터 타입으로 함수에 이용할 수 있다. 이로서 함수 구현의 일부를 함수가 호출 될때 함수를 호출하는 쪽에 맡기는 것이 가능하게 된다.
이 예제는 위에서 가져온 수학 함수의 결과를 출력한다.
```
func printMathResult(mathFunction: (Int, Int) -> Int, a: Int, b: Int) {
    println("Result: \(mathFunction(a, b))")
}
printMathResult(addTwoInts, 3, 5)
// prints "Result: 8"
```
이 예제는 세 개의 파라메터를 가지는 `printMathResult`함수를 정의합니다. 첫번째 파라메터는 타입이 `(Int, Int) -> Int`인 `mathFunction`입니다. 함수 타입이 맞는 함수라면 인수로서 첫번째 파라메터에 어느것이나 넘길 수 있습니다. 두번째와 세번째 파라메터는 `a`와 `b`이며 둘 다 `Int`타입입니다. 이 둘은 제공된 수학 함수의 두 입력값으로 사용됩니다.
`printMathResult` 함수가 호출되면 `addTwoInts`함수와, 정수 값으로 `3`과 `5`를 넘깁니다. 이 함수는 제공받은 함수를 호출할때 `3`과 `5`를 이용합니다. 그리고 결과인 `8`을 출력합니다. 
`printMathResult`의 역할은 적절한 타입의 수학 함수의 실행 결과를 출력하는 것입니다. 이 함수는 넘겨받는 함수의 구현이 실제로 무엇을 하는지 상관하지 않습니다. 오직 함수가 정확하게 일치하는 타입인지만 봅니다. 이로써 `printMathResult`함수가 타입에 안전한 방식(type-safe way)으로 자기 기능의 일부를 호출자에게 넘길 수 있게 됩니다.

### 함수 타입과 반환 타입
함수 타입을 다른 함수의 반환 타입으로 사용할 수 있습니다. 이는 완전한 함수 타입을 반환할 함수 정의의 반환 화살표 (`->`)바로 뒤에 작성함으로서 할 수 있습니다.
다음 예제는 두개의 간단한 함수인 `stepForward`와 `stepBackward`를 정의하고 있습니다. `stepForward`함수는 입력값보다 1이 더 큰 값을 반환하고 `stepBackward`함수는 입력 값보다 1이 작은 값을 반환합니다. 두 함수의 타입은 모두 `(Int) -> Int`입니다.
```
func stepForward(input: Int) -> Int {
    return input + 1
}
func stepBackward(input: Int) -> Int {
    return input - 1
}
```
여기 `chooseStepFunction` 함수가 있습니다.  이 함수의 반환 타입은 "`(Int) -> Int`를 반환하는 함수"입니다. `chooseStepFunction`은 `backwards` 불리언 파라메터에 따라 `stepForward`함수와 `stepBackward`함수중 하나를 반환합니다.
```
func chooseStepFunction(backwards: Bool) -> (Int) -> Int {
    return backwards ? stepBackward : stepForward
}
```
`chooseStepFunction`를 이용하여 어느 한쪽 방향으로 나아가는(증가 또는 감소하는) 함수를 얻을 수 있다.
```
var currentValue = 3
let moveNearerToZero = chooseStepFunction(currentValue > 0)
// moveNearerToZero now refers to the stepBackward() function
```
앞서 한 예제들은 `currentValue`변수에 따라 점진적으로 0이 되기 위해 필요한 증가나 감소 방향을 산출한다.`currentValue`의 초기값은 `3`이며, 이는 곧 `currentValue > 0`은 `true`를 반환하게 합니다. 그리고  `chooseStepFunction`이 `stepBackward`함수를 반환하게 합니다. 반환된 함수에 대한 참조(reference)는 `moveNearerToZero` 상수에 저장됩니다.
이제 `moveNearerToZero`가 올바른 함수를 참조하게 되었기에, 0까지 세는데 이용할 수 있습니다.
```
println("Counting to zero:")
// Counting to zero:
while currentValue != 0 {
    println("\(currentValue)... ")
    currentValue = moveNearerToZero(currentValue)
}
println("zero!")
// 3...
// 2...
// 1...
// zero!
```
## 중첩된 함수들
여기까지 이 챕터에서 마주친 모든 함수들은 모두 전역 범위(global scope)에 정의된  _전역 함수_의 예제였습니다. 또한 _중첩된 함수_라 불리는, 함수 내부에 다른 함수를 정의할 수 있습니다.
중첩 함수는 범위 밖의 세계에서 기본적으로 숨겨져 있습니다. 하지만 감싸고 있는 함수에 의해 여전히 이용될 수 있습니다. 감싸고 있는 함수는 중첩된 함수들을 반환하여 다른 범위에서 함수가 사용될 수 있게 할 수 있습니다.
위 예제의 `chooseStepFunction`을 다음과 같이 중첩된 함수를 이용하여 재작성 할 수 있습니다.
```
func chooseStepFunction(backwards: Bool) -> (Int) -> Int {
    func stepForward(input: Int) -> Int { return input + 1 }
    func stepBackward(input: Int) -> Int { return input - 1 }
    return backwards ? stepBackward : stepForward
}
var currentValue = -4
let moveNearerToZero = chooseStepFunction(currentValue > 0)
// moveNearerToZero now refers to the nested stepForward() function
while currentValue != 0 {
    println("\(currentValue)... ")
    currentValue = moveNearerToZero(currentValue)
}
println("zero!")
// -4...
// -3...
// -2...
// -1...
// zero!
```
