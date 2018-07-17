---
id: chapter14
title: 서브스크립트 (Subscripts)
---
> Translator : Snowcat8436 (snowcat8436@gmail.com)

 클래스, 구조체 그리고 열거형은 collection, list, sequence의 member element에 접근하기 위한 축약형인 _서브스크립트_로 정의할 수 있습니다. 또한 값의 설정이나 검색을 위한 별도의 메서드(seperate method)없이 index를 통해서 값을 설정하거나 검색하기 위해 서브스크립트를 사용할 수있습니다. 예를 들어서  `someArray[index]`와 같이 `배열`의 내부값(element)에 접근하거나 `someDictionary[key]`와 같이 사용하여 `딕셔너리`의 내부값(element)에 접근할 수 있습니다.
 
 한 타입을 위해서 여러개의 서브스크립트를 정의 할 수도 있고, 또한 
서브스크립트로 넘기는 index값의 타입을 기초로하여 사용하기 적절한 서브스크립트 overload(중복)를 선택할수 있다. 서브스크립트는 당신이 원하는 타입에 맞게 여러개의 입력 파라미터(input parameter)를 가지도록 정의할 수도 있다.

## 서브스크립트 문법(Subscript Syntax)

서브스크립트는 인스턴스의 이름 뒤에 있는 '[]'안에 한개 이상의 값을 적는 것으로 당신이 인스턴스들의 타입(instances of a type)를 요구할 수 있다. 그들의 문법은 인스턴스의 메서드나 computed property의 문법과 유사합니다.  인스턴스의 메서드들과 동일한 방식으로 `subscript`키워드와 함께 특정한 하나 이상의 입력 파라미터와 와 리턴타입을 통해서 서브스크립트를 정의할 수 있다. 다만 인스턴스의 메서드들과는 달리 서브스크립트는 읽고 쓰는 권한만 있거나 읽는 권한만을 가질 수 있다. 다음 코드는 서브스크립트가 computed property들과 동일한 방식으로 getter와 setter를 통해 작업하는 것을 보여줍니다
```
subscript(index: Int) -> Int {
  get {
	  // return an appropriate subscript value here
  }
  set(newValue) {
	  // perform a suitable setting action here
  }
}
```
`newValue`의 type은 해당 subscript의 리턴값과 동일합니다.
computed properties와 같이 당신은 setter의 파라미터인 `(newValue)`를 특정하게 선택할수 없습니다. 만일 당신이 setter를 위한 타입을 아무것도 제공하지 않는다면, 그제서야 기본 parameter인 `newValue`가 setter를 위해 제공될 것입니다.

읽기 전용의 computed properties와 같이 `get` 키워드를 없애서 읽기 전용의 서브스크립트를 만들 수 있다.:
```
subscript(index: Int) -> Int {
 // return an appropriate subscript value here
}
```
이곳에 정수를 n배 한 결과를 표시하는 `TimesTable structure`를 선언하기 을 위한 읽기 전용의 서브스크립트를 구현하는 예제가 하나 있습니다.
```
struct TimesTable {
  let multiplier: Int
  subscript(index: Int) -> Int {
	  return multiplier * index
  }
}
let threeTimesTable = TimesTable(multiplier: 3)
println("six times three is \(threeTimesTable[6])")
// prints "six times three is 18"
```
이 예제에서 새로운 `TimesTable`의 인스턴스는 3의 배수를 출력을 하도록 생성되고.
이것은 넘겨준 값인 3을 구조체의 `initializer`가 인스턴스의 `multiplier` 파라미터로 사용한 것을 의미합니다. 
해당 서브스크립트를 부르는 것으로 `threeTimesTable`의 인스턴스에게 요청할 수 있다. 보는 바와같이 `threeTimesTable[6]`과 같이 부르는 것으로 `threeTimesTable` 인스턴스의 서브스크립트를 부를 수 있다. 해당 요청은 6의 3배 테이블을 요청했으며 그 값은 18=6*3 이 된다.

>NOTE
n배 테이블은 고정된 숫자값을 출력하는 규칙에 기반합니다. 따라서 `newValue`등을 통하여 `treeTimesTable[someindex]`를 따로 설정하는 것은 적절하지 않으며 그러기에 위의 `TimesTable`을 위한 서브스크립트는 읽기전용의 서브스크립트로 선언되었습니다.

## 서브스크립트 사용(Subscript Usage)

아주 정확한 의미의 "subscript"는 그것이 사용되는 문맥에 따라 결정된다. 서브스크립트는 일반적으로 collection, list, 또는 sequence에 특정 member elements에 접근하기 위한 단축형이라는 의미로 사용되며, 당신은 특별한 클래스나 구조체의 기능을 위해 적절한 방식으로 자유롭게 대부분의 서브스크립트를 구현할 수있다.
예를 들어서 Swift의 `Dictionary` 타입은 `Dictionary` 인스턴스에 저장된 값들을 설정하고 검색하기 위한 하나의 서브스크립트로 구현했다.
당신은 딕셔너리 안에 딕셔너리의 키의 타입의 키값을 서브스크립트의 '[]'안에 넣는 것으로 값을 세팅할 수 있으며 딕셔너리 안에 들어갈 값을 서브스크립트에 할당할 수도 있다:
```
var numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
numberOfLegs["bird"] = 2
```
위 예제는 `numberOfLegs`라는 변수를 선언하고 이를 3가지 key-value 쌍을 가진 딕셔너리 literal로 초기화 하고있다.
`numberOfLegs` 딕셔너리의 타입은 `Dictionary<String, Int>` 를 뜻하며. 딕셔너리가 생성이 된 후, 이 예제는 서브스크립트 assignment을 사용하여 `String` 키인 `"bird"`와 `Int` 값인 2를 딕셔너리에 추가하는 것을 볼 수있다.
딕셔너리 서브스크립트에 관한 보다 많은 정보를 원한다면, [Accessing and Modifying a Dicionary](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/CollectionTypes.html#//apple_ref/doc/uid/TP40014097-CH8-XID_142)를 참고하기 바란다.

>NOTE
Swift의 딕셔너리 타입은 내부적인 key-value 서브스크립트가 요구하고 반환하는 타입이 옵셔널 타입인 서브스크립트로 구현하였습니다. 위에서 `numberOfLegs` 딕셔너리를 보면, key-value 서브스크립트를 요구하고 반환하는 타입이 `Int`일까요?, 그렇지 않으면 옵셔널 `Int`일까요? 딕셔너리 타입은  '모든 키가 값을 가지는 것은 아니다'라는 사실을 위한 모델을 지원하기 위해 옵셔널 서브스크립트 타입을 사용합니다. 그리고 어떤 키에 값을 삭제하는 방법을 제공하는데, 이 경우 해당 키값의 값는 `nil`로 할당된다.

## 서브스크립트 옵션(Subscript Options)

서브스크립트는 어떠한 숫자의 입력 파라미터들도 처리가 가능하다.그리고 그리고 이 입력 파라미터들은 어떠한 타입도 가능하다. 서브스크립트는 또한 어떠한 타입으로도 리턴이 가능하다. 서브스크립트는 변수 파라미터와 variadic parameters도 가능하지만, in-out parameters 나 default parameter 값은 지원하지 않습니다.
클래스나 구조체는 필요한 만큼의 서브스크립트를 구현하는 것이 가능하며, 적절한 서브스크립트는 보통 각각의 서브스크립트가 사용되는 요소요소에서 서브스크립트에 포함되어 서로 대비하도록 한 값이나 값들의 타입이 기초라고 생각할 수 있습니다. 
이러한 다수의 서브스크립트에 관한 정의는 _서브스크립트 overloading_으로도 알려져 있습니다.
대부분의 한개의 파라미터만을 요구하는 서브스크립트와는 다르게, 만일 당신이 만들 것에 필요하다면, 다수의 파라미터를 요구하는 서브스크립트를 선언할 수도 있습니다.
다음 예제는 `Double`값을 가지는 2차원 행렬을 표현하는 `Matrix`라는 구조체를 선언하고 있습니다. `Matrix` 구조체의 서브스크립트는 두개의 정수형 파라미터를 요구 하고 있습니다:
```
struct Matrix {
  let rows: Int, columns: Int
  var grid: Double[]
  init(rows: Int, columns: Int) {
	  self.rows = rows
	  self.columns = columns
	  grid = Array(count: rows * columns, repeatedValue: 0.0)
  }
  func indexIsValidForRow(row: Int, column: Int) -> Bool {
	  return row >= 0 && row < rows && column >= 0 && column < columns
  }
  subscript(row: Int, column: Int) -> Double {	
      get {
        assert(indexIsValidForRow(row, column: column), "Index out of range")
        return grid[(row * columns) + column]
      }
      set {
        assert(indexIsValidForRow(row, column: column), "Index out of range")
        grid[(row * columns) + column] = newValue
      }
  }
}
```
`Matrix`는 `rows`와 `columns`이라는 두개의 파라미터를 요구하는 initializer 를 제공하며, `Double` 타입으로 `rows * columns`를 충분히 저장할 수 있을만큼 큰 배열을 생성합니다. 각 `Matrix`의 위치의 초기값은 `0.0`으로 주어지며. 이러한 작업이 모두 이루어 진 다음에는 만들어진 배열을 배열의 initializer로 보내서 올바른 크기의 배열을 만듭니다. 이 initializer에 다한 자세한 사항은 [Creating and Initializing an Array](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/CollectionTypes.html#//apple_ref/doc/uid/TP40014097-CH8-XID_142)를 참고하세요.

이제 다음과 같은 방식으로  적절한 row와 column을  initializer에 넘기는 것으로 새로운 `Matrix` 인스턴스를 생성할 수 있습니다.
```
var matrix = Matrix(rows: 2, columns: 2)
```
아래의 예제는 2x2의 크기를 가진 새로운 `Matrix` 인스턴스를 생성하는 예제입니다. `Matrix` 인스턴스를 효과적이도록 평평하게 펴서 보여주기 위한 `grid` 배열을 참고하면 왼쪽위에서부터 오른쪽 아래로 읽어 나가는 것을 볼 수 있습니다.

![](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/subscriptMatrix01_2x.png)

`matrix`에 값을 넣을때는 row,column를 이용해서 서브스크립트에 맞는 형태로 값을 넘겨주면 설정할 수 있습니다:
```
matrix[0, 1] = 1.5
matrix[1, 0] = 3.2
```
이 두 문장은 우측 상단의 값([0, 1])을 `1.5`로, 좌측 하단의 값([1, 0])을 `3.2`로 설정합니다:

![](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/subscriptMatrix02_2x.png)

`Matrix`의 서브스크립트의 getter와 setter는 모두 올바른 `row`와 `column`값이 들어오는지 체크하는 assertion을 포함하고 있습니다. 이 assertion들을 돕기 위하여 `Matrix`는 자체적으로 `indexIsValid`라는 convenience method를 가지고 있으며 이는 주어진 값이 `matrix`의 범위를 넘어가는지 아닌지를 체크합니다:
```
func indexIsValidForRow(row: Int, column: Int) -> Bool {
 return row >= 0 && row < rows && column >= 0 && column < columns
}
```
만일 `matrix`의 경계를 넘어가는 값이 서브스크립트로 들어오게 된다면 assertion이 발생합니다:
```
let someValue = matrix[2, 2]
// this triggers an assert, because [2, 2] is outside of the matrix bounds 
```


