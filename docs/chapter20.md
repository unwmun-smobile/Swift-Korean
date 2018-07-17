---
id: chapter20
title: 타입 변환 (Type Casting)
---
> Translator : Snowcat8436 (snowcat8436@gmail.com)

## 타입변환(Type Casting)

타입 변환이란 인스턴스(instance)의 타입을 체크하기 위한 방법이며, 또한 이것은 인스턴스를 마치 해당 클래스가 가친 계층구조에서 온 상위클래스나 하위클래스처럼 다룬다.
Swift에서 타입 변환은 `is`와 `as`라는 연산자로 구현할 수 있으며, 이 두 연산자는 값의 타입을 체크하거나 다른 타입으로 변환하는 간단하고 표현적인 방법을 제공합니다.
또한 해당 타입이 프로토콜에 적합하지 아닌지 체크하기 위해서 타입 변환을 사용할 수 있으며 보다 자세한 사항은 [Protocol Conformance](링크)를 참조하시기 바랍니다.

## 타입 캐스팅을 위한 클래스 계층 정의(Defining a Class Hierarchy for Type Casting)

당신은 특정한 클래스의 인스턴스의 타입을 체크하거나 인스턴스를 같은 계층의 또다른 클래스로 변환하기 위해서 클래스들과 하위클래스들의 계층정보를 사용한 타입캐스팅을 할 수 있다.
아래의 세가지의 코드조각(code snippets)는 타입 캐스팅이 사용되는 예제를 보여주기 위한 계층적인 클래스들과 각각의 클래스들의 인스턴스를 포함하는 배열(array)를 정의하고 있습니다.
첫번째 코드 조각은 `MediaItem`이라는 새로운 기본 클래스(base class)를 정의하고 있습니다. 이 클래스는 디지털 미디어 라이브러리에 있는 모든 아이템들을 위한 기본적인 기능을 제공합니다. 특히   `String` 타입의 `name` 속성(Property)를 선언하고, `init name` initializer를 통해서 초기화 합니다(이것은 모든 미디어 아이템(영화나 노래)들이 이름을 가지고 있다고 가정합니다)

```
class MediaItem {
	var name: String
	init(name: String) {
		self.name = name
	}
}
```

다음 코드 조각은 `MediaItem`의 두가지 하위클래스(subclasses)들입니다. 첫번째 하위클래스인 `Moive`는 내부적으로 영화에 관한 추가적인 데이터를 가지고 있는데. 이는 `director`라는 속성 및 초기화 부분을 `MediaItem`클래스의 initalizer의 윗부분에 추가하는것으로 더할 수 있으며. 두번째 하위 클래스인 `Song`도 `artist` 속성의 관한 내용을 선언하고 base class의 윗부분에서 이를 초기화 한다:
```
class Movie: MediaItem { 
	var director: String
	init(name: String, director: String) {
		self.director = director
		super.init(name: name)
	}
}

class Song: MediaItem {
	var artist: String
	init(name: String, artist: String) {
		self.artist = artist
		super.init(name: name)
	}
}
```
마지막 코드조각은 2개의 `Movie` 인스탄스와 3개의 `Song` 인스턴스를 포함하는 `library`로 불리는 상수형 배열(constant array)를 만든다.
`library` 배열의 타입은 각각의 배열 내부의 콘텐츠를 초기화 하는 타입으로 추정할 수 있다.
Swift의 타입 체커는 `Movie`나 `Song`이 공통의 상위 클래스(superclass)인 `MediaItem`을 가진다고 추정할 수 있고, 따라서 `library`의 타입은  `MediaItem[]`로 추정할 수 있다 :
```
let library = [
	Movie(name: "Casablanca", director: "Michael Curtiz"),
	Song(name: "Blue Suede Shoes", artist: "Elvis Presley"),
	Movie(name: "Citizen Kane", director: "Orson Welles"),
    Song(name: "The One And Only", artist: "Chesney Hawkes"),
	Song(name: "Never Gonna Give You Up", artist: "Rick Astley")
]
// the type of "library" is inferred to be MediaItem[]
```
`library`에 저장된 요소들은 해당 Scenes뒤에서는 여전히 `Movie`와 `Song`인스탄스이다. 그러나 만일 네가 이 array의 컨텐츠들을 반복자 등을 이용하여 뽑아낸 다면, 네가 받게된 그 아이템들의 타입은 Song이나 Movie이 아닌 MediaItem일 것이다. 그것들을 원래의 타입으로 작업을 하기 위해얻고 싶다면, 당신은 그들의 타입을 체크하는 것이 필요하고, 또한 그들을 다운캐스트해서 다른 타입으로 변경하여야 한다. 이는 아래서 설명하도록 하겠다.

## 타입 체크(Checking Type)

어떠한 인스턴스가 확실히 하위클래스 타입인지 아닌지를 체크하기 위해서는 타입 체크 연산자인 `is`를 이용합니다. 이 타입체크용 연산자는 만일 해당 인스탄스가 해당 하위 클래스라면 `true`를, 아니라면 `false`를 반환합니다.
아래의 예시는 `library` 배열에 있는 `Movie`의 인스턴스의 수와 `Song`의 인스턴스의 수를 세기 위한 `movieCount`와 `songCount`라는 두개의 변수를 선언하는 것을 보여줍니다.
```
var movieCount = 0
var songCount = 0

for item in library {
	if item is Movie {
		++movieCount
	} else if item is Song {
		++songCount
	}
println("Media library contains \(movieCount) movies and \(songCount) songs")
// prints "Media library contains 2 movies and 3 songs"
```
이 예시에서는 `library`배열의 모든 아이템에 대해서 작업하며, 각각의 1번의 과정마다 `for-in` 루프는 배열에 `MediaItem` 상수를 가져옵니다.
각각의 아이템이 만일 `Movie` 인스탄스이면 `item is Movie`에서 `true`를 아니라면 `false`를 반환하고, 이와 유시하게 아이템이 만일 Song의 인스탄스인지 아닌지에 따라 `item is Song` 체크부분의 리턴값이 결정됩니다. `for-in` 루프의 마지막이 되면, `moveCount`와 `songCount`의 값을 보고 전체 `MediaItem` 인스탄스중에 각각의 타입이 얼마만큼의 수가 들어있는지 찾아낼 수 있다.

## 다운캐스팅( Downcasting )

상수와 변수의 명확한 클래스 타입은 사실은 아마도 the scenes뒤에 있는 하위 클래스의 인스탄스에 속할것이다. 
당신이 위와 같은 케이스를 믿는 경우, 당신은 타입 변환 연산자인 `as`를 통하여 하위클래스타입으로 다운캐스팅을 시도 할 수 있다.
다운캐스팅은 실패할 수 있기때문에, 타입 캐스팅연산자는 두가지의 다른 형태를 가집니다.
하나는 `as?`와 같은 연산자를 사용하는 optional form으로 다운캐스팅을 시도하여 optional value를 리턴합니다.
다른 하나는 `as`와 같은 연산자를 사용하는 forced form으로 다운 캐스팅을 시도하고 강제로 unwrap한 결과를 한번에 합한 작업을 합니다.
네가 만일 다운캐스트가 성공할지 확신을 가지지 못한다면 타입변환 연산자인 `as?`를 이용하는 optional form을 사용한다. 위 연산자를 사용하는 form은 항상 optional value를 리턴하며 그래서 만일 다운 캐스트가 가능하지 않은 경우에는 `nil`을 리턴할 수 있도록 할 수 있다. 이 것은 당신이 다운 캐스팅의 성공 유무를 체크할 수 있도록 하게 한다.
오직 당신이 다운캐스트가 항상 성공할 것이라는 확신이 있다면 타입 변환 연산자인 `as`를 이용하는 forced form을 사용할 수 있다. 위의 연산자를 사용하는 form은 만일 올바르지 않은 클래스 타입으로 다운캐스팅을 시도했을 시에 런타임 에러를 발생시킨다.
아래에 `library` 내의 각 `MediaItem`을 반복해가면서 각 아이템들을 위한 적절한 설명을 출력하는 예시를 만들었다. 이를 위해서 각 아이템이 단순히 `MediaItme`이 아닌 진정으로 `Movie`나 `Song`인지 억세스 해볼 필요가 있다. 이를 위해서 설명을 출력하기 위해서 `Movie`나 `Song`의 `director`나 `artist` 속성에 접근할수 있게 할 필요가 있다.
예시에서 배열내의 각각의 item은 `Movie`이거나 `Song`이라고 생각된다. 당신은 각각의 아이템이 실제 어떠한 클래스인지 미리 알 수가 없습니다. 그러므로 optional form을 위한 `as?` 연산자를 사용하여 루프를 통해 각 케이스마다 다운캐스팅을 체크하는 것이 적절합니다:
```
for item in library {
	if let movie = item as? Movie {
		println("Movie: '\(movie.name)', dir. \(movie.director)")
	} else if let song = item as? Song {
		println("Song: '\(song.name)', by \(song.artist)")
	}
}
 
// Movie: 'Casablanca', dir. Michael Curtiz
// Song: 'Blue Suede Shoes', by Elvis Presley
// Movie: 'Citizen Kane', dir. Orson Welles
// Song: 'The One And Only', by Chesney Hawkes
// Song: 'Never Gonna Give You Up', by Rick Astley
```
이 예시는 현재 아이템이 `Movie`라고 생각하고 다운 캐스팅을 시도하는 것으로 시작합니다. 아이템이 `MediaItem` 인스탄스이므로 이 아이템은 `Movie`일 수 있습니다, 또한 똑같은 이유로 `Song`도 가능합니다, 혹은 오로지 단순히 `MediaItem`일수도 있습니다. 이것이 불확실 하기 때문에, `as?` 타입변환 연산자를 사용하여 하위 클래스로의 다운캐스팅을 시도시에 optional value를 반환합니다. 그 결과 `item as Moive`의 결과는 `Move?` 타입, 즉 optional `Movie`이 됩니다.
`library` 배열안의 두개의 `Song` 인스탄스에 해당 내용을 적용하여 `Movie`로 다운캐스팅을 할경우 실패한다.  이것에 대처하기 위해, 위의 예시에서는 결과로 나온 optional `Movie`값이 실제로 값을 가지고 있는지 체크하기 위한(이 경우는 다운캐스팅이 성공했는지 아닌지 찾는 과정이다) optional binding을 사용한다. 
이 optional binding 은 `if let movie = is as? Moive`와 같이 적히며, 이는 다음과 같이 해석될 수 있다: "해당 아이템을 `Movie`로 생각하고 접근을 시도한다. 만일 해당 작업이 상공하면, 반환된 optional `Movie`값을 저장할 `movie`라고 불리는 새로운 임시 상수값을 설정한다.
만일 다운 캐스팅이 성공한다면, `movie`의 속성들을 가지고 `director`와 같은 `Moive` 인스탄스를 위한 설명을 출력하는데 사용할 수 있습니다. 
비슷한 원리로 `Song` 인스턴스를 위한 체크를 하여, `library`에서 `Song` 인스탄스를 찾기만 한다면, `artist`와 같은 적절한 설명을 출력할 수 있습니다.

>NOTE
변환(Casting)은 실제로 해당 인스턴스를 수정하거나 그 값을 바꾸는 것이 아닙니다. 근본적인 인스턴스는 처음상태 그대로 남아있습니다. 이것은 간단히 특별한 것이며, 캐스팅된 타입의 인스턴스로서 접근이 가능한 것입니다.

## Type Casting for Any and AnyObject

Swift는 특정한 타입을 가지지 않는 상태로 작업하기 위한 두가지의 특별한 타입을 제공합니다:

`AnyObject`는 어떠한 클래스타입의 인스턴스라도 표현할 수 있습니다
`Any`는 함수형의 타입을 제외하고는 어떠한 타입의 인스턴스라도 표현할 수 있습니다.

>NOTE
`Any`나 `AnyObject`는 오로지 당신이 명시적으로 behavior나 그들이 제공하는 능력들이 필요한 경우에만 사용합니다. 이는 항상 당신의 코드 속에서당신이 예상한 특정한 형태의 타입으로 작동하는 것이 더 낫습니다.

## AnyObject

Cocoa APIS를 이용하여 작업을 할때, 보통 `AnyObject`[] 타입의 배열을(`AnyObject` 타입의 값을 가진 배열) 받는것이 일반적입니다. 이것은 Objective-C가 명시적인 타입의 배열을 가지지 못하기 때문입니다. 그러나 당신이 종종  당신이 알고있는 API가 제공하는 배열을 포함한 여러가지 정보를 포함한 오브젝트들의 타입에 대해서 자신이 있을 수 있다.
이러한 상황에서, 당신은 optional unwrapping이 필요하지 않은 경우에 한하여 배열의 각각의 아이템을 특정한 클래스의 타입으로 바꾸는 다운캐스팅을하기 위한 타입 변환 연산자 `as`로 강제로 변경한 형태를 사용할 수 있습니다.
```
let someObjects: AnyObject[] = [
	Movie(name: "2001: A Space Odyssey", director: "Stanley Kubrick"),
	Movie(name: "Moon", director: "Duncan Jones"),
	Movie(name: "Alien", director: "Ridley Scott")
]
```
이 배열은 오로지 `Moive` 인스턴스만 가지는 것을 이미 알고 있으므로, 당신은 다운캐스팅 및 타입 변환 연산자 `as`를 이용하여 non-optional `Moive`로 강제로 형태를 바꾸는 unwrap를 할 수 있습니다.
```
for object in someObjects {
	let movie = object as Movie
	println("Movie: '\(movie.name)', dir. \(movie.director)")
}
// Movie: '2001: A Space Odyssey', dir. Stanley Kubrick
// Movie: 'Moon', dir. Duncan Jones
// Movie: 'Alien', dir. Ridley Scott
```
루프를 조금더 짧게 만들기 위해서, 각 아이템을 다운캐스팅하는 대신에 `someObjects` 배열을 `Movie[]` 타입으로 다운 캐스팅 할 수도 있습니다:
```
for movie in someObjects as Movie[] {
	println("Movie: '\(movie.name)', dir. \(movie.director)")
}
// Movie: '2001: A Space Odyssey', dir. Stanley Kubrick
// Movie: 'Moon', dir. Duncan Jones// Movie: 'Alien', dir. Ridley Scott
```

## Any

이곳에 non-class타입을 포함한 여러가지 다른 타입을 섞어서 작업하기 위한 `Any`를 사용한 예제가 있다. 이 예제는 `Any`타입의 값을 저장할 수 있는 `things`이라는 한 배열을 생성한다.
```
var things = Any[]()
 
things.append(0)
things.append(0.0)
things.append(42)
things.append(3.14159)
things.append("hello")
things.append((3.0, 5.0))
things.append(Movie(name: "Ghostbusters", director: "Ivan Reitman"))
```
`things`배열은 두개의 `int` 값, 두개의 `Double`값, 하나의 `String`값, 하나의 (`Double`,`Double`)타입의 tuple, 그리고 "Ghostbusters"의 name과 "Ivan Retiman"의 director속성을 가진 Moive를 한개 가지고 있다.
당신은 `Any`나 `AnyObject`로 알고있는 변수에서 특정한 타입의 상수나 변수 찾기 위한 스위치 구문의 `case` 항목에 `is`와 `as` 연산자를 사용할수 있습니다.
아래의 예제는 아이템들의 `things` 배열의 각 아이템을 반복하면서 스위치 문을 통해서 각각의 타입을 요청한다.
몇몇의 `switch`문의 `case`항목에서 비교항목과 동일한 값과 타입을 가지는 상수의 경우는 해당 값과 형태를 출력한다:
```
for thing in things {
    switch thing {
    case 0 as Int:
        println("zero as an Int")
    case 0 as Double:
        println("zero as a Double")
    case let someInt as Int:
        println("an integer value of \(someInt)")
    case let someDouble as Double where someDouble > 0:
        println("a positive double value of \(someDouble)")
    case is Double:
        println("some other double value that I don't want to print")
    case let someString as String:
        println("a string value of \"\(someString)\"")
    case let (x, y) as (Double, Double):
        println("an (x, y) point at \(x), \(y)")
    case let movie as Movie:
        println("a movie called '\(movie.name)', dir. \(movie.director)")
    default:
        println("something else")
    }
}
 
// zero as an Int
// zero as a Double
// an integer value of 42
// a positive double value of 3.14159
// a string value of "hello"
// an (x, y) point at 3.0, 5.0
// a movie called 'Ghostbusters', dir. Ivan Reitman
```

>NOTE
`switch`문의 `case`항목들은 체크 및 특정한 타입으로의 변환을 위해서 `as`나 `as?`를 통해서 강제로 변경된 형태를 사용한다. 이런 체크는 `switch`문의 문맥안에 있는 이상 항상 안전하다. 
