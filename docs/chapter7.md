---
id: chapter7
title: 흐름 제어  (Control Flow)
---
> Translator : 김나솔(nasol.kim@gmail.com)

Swift 언어에서는 C언어 같은 프로그래밍 언어에서 제공하는 것과 비슷한 제어문 관련 구조를 제공합니다. 이러한 구조에는 `for`나 `while`이 있으며, 이러한 키워드는 어떤 작업(task)을 여러 번 수행합니다. `if`와 `switch`문은 특정 조건이 충족되는지에 따라서 분기시켜서 코드 블럭을 실행시킵니다. 또한 `break`나 `continue` 같은 구문은 실행의 흐름을 코드 상의 다른 곳으로 이동시킵니다.

C언어에서는 `for`-조건부-증가(increment) 순환문(loop) 이런 방식을 전통적으로 사용하는데, Swfit에서는 `for-in` 순환문(loop)이라는 것이 있어서 배열이나 사전(dictionaries), ranges, 문자열(strings)등 sequence에 대해서iteration하기가 쉽습니다. 
 
C언어의 `switch`문과 비교했을 때, Swift의 `switch`문은 훨씬 더 강력합니다. Swift에서는 `switch`문이 "fall through" 하지 않습니다(역자주: fall through란, switch문에서 한 case에 대해서 처리하고 난 후 다음 case로 넘어가는 것). C언어에서는 실수로 `break`문을 써주지 않아서 에러가 생기는 경우가 있는데 Swift에서는 fall through 하지 않기 때문에 이런 에러를 방지할 수 있습니다. `swich`내의 case에 대해서 여러 종류의 pattern-maching을 사용할 수 있습니다. 수의 범위 match, 투플 match, casts to a specific type. `switch case`에서 match된 값을 임시 상수나 변수에 binding할 수도 있습니다. 이렇게 binding해두면 case의 본문(body) 내에서 이 상수나 변수를 사용할 수 있습니다. 또한 매칭 조건(matching condition)이 복잡한 경우에는, 각 case에 대해서 where절(where clause)을 사용해서 표현할 수 있습니다.  

## For 순환문(For Loops)

`for` 순환문(for loop) 사용하면 코드 블럭을 특정 횟수 만큼 수행할 수 있습니다. Swift에는 두 종류의 for 순환문이 있습니다:

* `for-in` : 어떤 범위나 sequence, collection, progression에 대해서, 이 안에 있는 각 항목(item)에 대해서 코드(a set of statement)를 실행합니다. 

* `for-condition-increment` : 특정 조건부가 참이 될 때까지 코드를 실행합니다. 보통 루프를 한 번 도는 것이 끝날 때마다 counter를 1씩 증가시킵니다.  

### For-In
 
여러 항목이 들어 있는 컬렉션(collection)이나, 어떤 범위, 배열 안에 들어 있는 항목(item)에 대해서, 또는 문자열에 들어 있는 각 문자에 대해서 iteration을 할 때 `for-in` loop를 사용합니다. 

다음의 예는 구구단의 5단에서 처음 몇 개를 출력해 줍니다: 
```
for index in  1...5 {       
	println("\(index) times 5 is \(index * 5)") 
 } 
// 1 times 5 is 5   
// 2 times 5 is  10   
// 3 times 5 is  15   
// 4 times 5 is 20   
// 5 times 5 is 25  
```

위 예에서는 범위가 1부터 5까지의 폐쇄 영역이며, 범위에 대해서 컬렉션(collection)안에 들어 있는 각 항목(item)에 대해 이터레이션을 돌고 있습니다. 단, 1...5라고 표현했을 때, `...`(closed range operator)를 보면 알 수 있듯이, 이 범위에는 1과 5가 포함됩니다. `index`의 값은 범위 내의 첫번째 수, 즉 1이 되며, 루프 내에 있는 구문이 실행됩니다. 위 예에서 루프 안에는 구문이 하나만 있습니다. `index`의 현재 값에 대해서 5단의 첫번째를 출력해주는 것입니다. 이 구문이 실행된 다음에 인덱스의 값은 범위 내의 두번째 값, 즉 2가 되도록 업데이트 됩니다. 그리고 `println` 함수가 다시 호출됩니다. 이 작업은 인덱스가 범위의 끝에 이를 때까지 계속됩니다.  

위 예에서 `index`는 상수(constant)이며, 이 상수의 값은 루프를 돌 때마다, 초반에 자동으로 값이 지정됩니다. 따라서 이 상수를 사용하기 전에 선언할 필요가 없습니다. 루프를 선언할 때 포함시키기만 해도 암묵적으로 선언한 셈이 됩니다. 즉 `let` 선언 키워드(declaration keyword)를 써줄 필요가 없습니다.  

> NOTE
`index` 상수는 루프의 스코프(scope)안에서만 존재합니다. 루프문이 끝난 다음에 이 `index`의 값을 확인하고 싶거나, 이 값을 상수가 아니라 변수로 사용하고 싶으면, 루프 안에서 변수로 직접 선언해 주어야 합니다. 
 
범위 내에 있는 값이 필요 없다면, 변수명 대신에 언더바(`_`, underscore)를 써주면 됩니다:

```
  let base  = 3 
  let power  =  10 
  var answer  =  1 
  for _ in  1...power { 
     answer *= base 
  } 
println("\(base) to the power  of \(power) is \(answer)")  

// prints "3 to the power of  10 is 59049"  
```

위 예는 어떤 수의 몇 승을 계산해 줍니다(이 예에서는 `3`의 `10`승을 계산했습니다). 시작하는 값은 `1`인데(이는 `3`의 `0`승입니다), 이 시작하는 값에 다시 3을 곱해줍니다. 이 때 0에서 시작해서 9에서 끝나는 half-closed loop를 사용하였습니다. 이 계산을 수행할 때, 루프를 도는 동안 counter의 값은 필요 없습니다. 정확한 횟수만큼 루프를 도는 것만이 중요합니다. 밑줄(`_`, underscore)은 위 예에서 루프 변수 자리에 쓰였는데요, 그 결과 루프를 돌 때의 counter 현재값에 접근할 수 없게 됩니다.

> 리뷰어 주: 10이 포함되는 것을 보면 반개구간이 아니라 폐쇄구간인 1~10인데, 말이 잘못쓰여있는듯. 이건 원문도 그렇네요.

배열 안에 들어 있는 항목(item)에 대해서 이터레이션(iteration)을 할 때에, `for-in` 루프를 사용하세요.

```
  let  names  = ["Anna", "Alex", "Brian", "Jack"] 
  for name in names { 
     println("Hello, \(name)!") 
  } 
 // Hello, Anna! 
 // Hello, Alex ! 
 // Hello, Brian! 
 // Hello, Jack!  
```


딕셔너리(dictionary)에 대해서도 이터레이션(iteration)을 해서 키-값 쌍(key-value pairs)에 접근할 수 있습니다. 딕셔너리에 대해서 이터레이션을 하면, 딕셔너리의 각 항목이 `(key, value)` 투플의 형태로 반환됩니다. 그리고 이 `(key, value)` 쌍은 쪼개어져서 두 개의 상수의 값으로 들어갑니다. 이 값은 `for-in` 루프 내의 본문(body)내에서 사용할 수 있습니다. 아래의 예에서 딕셔너리의 키는 `animalName`이라는 상수에, 딕셔너리의 값은 `legCount`라는 상수에 값으로 들어갑니다:

```
  let numberOfLegs  = ["spider": 8, "ant": 6, "cat": 4] 
  for (animalName, legCount) in numberOfLegs { 
     println("\(animalName)s have \(legCount) legs") 
  } 
 // spiders have 8 legs 
 // ants have 6 legs 
 // cats have 4 legs 
```
`Dictionary`안에 있는 항목이 이터레이션 될 때, 딕셔너리 안에 들어 있는 순서대로 되지는 않습니다. `Doctionary`안에 들어 있는 데이터는 원래 정의상 순서가 없으며, 이터레이션을 돌 때에도, 어느 항목에 대해서 돌지 확신할 수 없습니다. 배열과 딕셔너리에 대해서 더 자세히 보시려면 [컬렉션 형(Collection Types)]() 장을 참조하세요.

이 이터레이션 되는 순서는 고정되어 있지 않습니다. 딕셔너리 안에 들어 있는 순서대로  배열과 딕셔너리 외에도, 문자열 내의 각 문자에 대해 이터레이션을 돌 때, `for-in` 루프를 사용할 수 있습니다:

```
 for character in "Hello" { 
     println(character) 
  } 
 // H 
 // e 
 // l 
 // l 
 // o  
```

### For-조건부-증가부 (For-Condition-Increment )

Swift는 `for-in`루프 말고도 C언어에서 쓰는, 조건부와 증가부가 들어 있는 `for` 루프를 지원합니다: 

```
 for var index  = 0; index  < 3; ++index { 
     println("index is \(index)") 
  } 
 // index is 0   
 // index is 1   
 // index is 2  
```
 다음은 이번에 다루는 루프 형식을 일반화해서 나타낸 것입니다.
 
 ```
 for   initialization ;   condition ;   increment  { 
	 statements 
 }  
```
 
루프의 정의부는 세 부분으로 구성되는데, C언어에서처럼 각 부분을 세미콜론`;`으로 구분하고 있습니다. C언어와는 다르게 Swift에서는 "초기화;조건부;증가부" 부분을 괄호로 감싸주지 않아도 됩니다. 

다음은 루프가 실행되는 단계를 나타냅니다:

1. 처음 루프에 들어가면, 초기화 표현식(initialization expression)이 검토되고 루프를 도는 데 필요한 변수나 상수의 값을 지정합니다.
2. 조건 표현식(condition expression)을 검토합니다. 조건부가 `false`이면, 루프는 종료하고, 코드 실행(code execution)은 `for` 루프를 닫는 중괄호(})다음 부분에서 계속됩니다. 조건부가 `true`이면, 코드 실행은 루프를 여는 괄호({) 안에서 구문들을 실행합니다. 
3. 모든 구문이 실행된 후에는 증가부 표현식(increment expression)이 검토됩니다. 검토 결과 카운터의 값이 증가할 수도 있고 감소할 수도 있습니다. 또는 초기화되었던 변수의 값이 실행된 구문의 결과값에 근거하여 새로운 값으로 대체될 수도 있습니다.  증가부 부분이 검토된 후, 코드 실행은 2단계로 돌아가며, 조건 표현식이 다시 검토됩니다.

위에서 설명한 루프문의 형식(format)과 코드 실행 절차를 개요로 나타내면 다음과 같습니다:
```
         initialization 
        while   condition  { 
              statements 
              increment 
        }  
```
초기화 부분에서 선언된 상수와 변수(예를 들면 var index=0)는 `for` 루프 스코프 내에서만 사용할 수 있습니다. 루프가 끝난 후에도 index의 마지막 값에 접근할 수 있으려면, 루프 스코프가 시작하는 지점 이전에 index를 선언해주어야 합니다 (즉 루프 스코프의 바깥에서 선언해 주어야 합니다):

```
 var index : Int 			// <= 이 부분에서 선언해 주어야.. 
 for index  = 0; index  < 3; ++index { 
     println("index is \(index)") 
  } 
 // index is 0
 // index is 1 
 // index is 2 
  println("The loop statements were executed \(index) times") 
 // prints "The loop statements were executed 3 times"  
```

위 예에서 한 가지 주의할 점이 있습니다. 루프가 끝났을 때 `index`의 마지막 값은 `2`가 아니라 `3`입니다. 마지막으로 증가부(increment)인 `++index`가 실행되었을 때, `index`의 값은 `3`이 됩니다. `index`가 `3`이 되니, `index < 3` 조건부 `false`가 되서 루프가 끝난 것입니다. 

## While 루프 (While Loops)

`while` 루프는 조건부가 거짓이 될 때까지 코드 블럭을 실행시킵니다. 이런 종류의 루프는 보통, 이터레이션이 시작하기 전에 이터레이션이 몇 번이나 돌 지 알지 못할 때 자주 사용합니다. Swift는 두 종류의 `while`루프를 지원합니다. 하나는 `while`인데, 이 루프는 루프를 돌기 시작할 때 조건부를 검토합니다. 다른 하나는 `do-while`인데, 이 루프는 루프를 돌고 나서 조건부를 검토합니다. 

### While 

`while` 루프는 한 개의 조건부를 검토하는 것에서 시작합니다. 조건부가 `true`면, 코드가 실행되며, 조건부가 `false`가 될 때까지 반복해서 실행됩니다.
다음은 `while`루프를 일반화해서 나타낸 것입니다:

```
       while   condition  { 
             statements 
       }  
```
       
이번에 사용할 예제는 뱀과 사다리 게임입니다.


![뱀과 사다리 게임](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/image__255.png)

게임의 규칙은 다음과 같습니다:

* 게임판에는 25개의 칸이 있으며, 목표는 25번 칸에 도달하거나 이를 넘는 것입니다. 
* 자기 차례가 오면, 참가자는 6면 주사위를 던지고 나온 수 만큼 점선으로 표시된 경로를 따라서 이동합니다. 
* 이동했을 때, 사다리의 아랫 부분에 도달하면, 사다리를 타고 올라갑니다.
* 이동했을 때 뱀의 머리에 도달하면, 뱀을 타고 내려갑니다. 

게임판은 `Int` 값들로 나타냅니다. 게임판의 크기는 `finalSquare` 상수로 정합니다. finalSquare는 배열을 초기화할 때 사용하며, 나중에 게임에 이겼는지 여부를 판별할 때도 사용합니다. 배열 board는 26개의 `Int`값 0으로 초기화됩니다. 25개가 아닙니다. (`0`부터 `25`까지, 26개입니다.)

```
 let finalSquare  = 25 
 var board  = Int[](count : finalSquare +  1, repeatedValue: 0)  
```
 
몇몇 칸에는 0이 아니라 특정한 값이 지정됩니다. 이 값은 뱀과 사다리 때문에 필요한 값입니다. 사다리의 밑부분이 들어 있는 칸은 게임판에서 앞으로 이동시키는 만큼의 양수(positive number)를 포함하고, 뱀 머리가 들어 있는 칸은 게임판에서 뒤로 이동시키는 만큼의 음수(negative number)를 포함합니다: 

```
 board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02 
 board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08  
```

3번 칸은 사다리의 밑 부분을 포함합니다. 여기에 가면 참가자는 11번 칸으로 이동합니다. 이것을 나타내기 위해서 `board[03]`에 `+08` 값을 지정합니다. 이는 정수값 `8`과 동일합니다(`11`과 `3`간의 차이). 단항 연산자인 plus operator(`+i`)는 단항 연산자 minus operator(`-i`)와 균형을 이룹니다. 또한 `10`보다 작은 수에는 10자리에 0을 넣어서(예: 8 -> 08) 줄이 맞추어져 가지런하게 보이도록 했습니다. (이처럼 0을 넣는 등 스타일에 변화를 주는 것은 꼭 필요한 것은 아닙니다만, 이렇게 하면 코드가 더 깔끔하게 보입니다.)

 
게임 참가자가 시작하는 칸은 "0번 칸"이며, 이 칸은 게임판의 좌측 하단 바깥에 있습니다. 주사위를 처음 던지면, 참가자는 항상 게임판 위로 이동하게 됩니다: 

```
 var square  = 0 
 var diceRoll   = 0 
 while square < finalSquare { 
     // 주사위를 던진다
     if ++diceRoll  == 7 { diceRoll  =  1 } 
     // 주사위를 던져 나온 수 만큼 이동한다
     square += diceRoll  
     if square  < board.count { 
         // 아직 게임판에 있다면, 뱀을 타고 내려가거나 사다리를 타고 올라간다
        //if we're still on the board, move up or down for a snake or a ladder  
 square += board[square] 
("Game over!")  
```

이 예제에서는 주사위를 던지는 부분을 간단하게 처리했습니다. 난수를 발생시키지 않고, `diceRoll`의 값을 0에서 시작하게 하고, 루프를 돌 때마다, diceRoll 값이 1씩 증가하도록 했습니다. `++i` (prefix increment operator)를 사용해서 말이죠. 그런 다음에 `diceRoll`의 값이 너무 크지 않은지 확인했습니다. `++diceRoll` 값은 `diceRoll`이 1만큼 증가한 값과 같습니다. `diceRoll` 값이 `7`과 같아지면, 값이 너무 커진 것이며, 이 때 `diceRoll` 값을 1로 해줍니다. 이렇게 하면 `diceRoll`의 값은 항상 `1`,`2`,`3`,`4`,`5`,`6`,`1`,`2`, 등등의 값을 가지게 됩니다.

주사위를 던진 후에, 게임 참가자는 `diceRoll` 값 만큼 칸을 이동합니다. 주사위에서 나온 수만큼 이동했는데, 참가자가 25번 칸을 넘어가는 경우가 생길 수 있습니다. 이 경우에는 게임이 끝납니다. 이 시나리오를 따르기 위해서, 코드는 현재의 `square` 값에다 `board[squre]`에 저장된 값을 더해서 참가자를 이동시키기 전에, `square`가 `board` 배열의 `count` 값보다 작은지 확인합니다.  

이렇게 확인작업을 해주지 않으면, `board[squre]`라고 썼을 때 `board` 배열의 범위를 넘어서는 값을 접근하려고 시도하게 되고, 에러가 날 것입니다. 예를 들어 `square`가 `26`과 같아지면, 코드는 `board[26]`의 값을 확인하려 할 것이고, 26은 배열의 수보다 큽니다.
이 `while` 루프 실행은 끝납니다. 그리고 루프의 조건부를 확인하고, 루프가 다시 실행되어야 하는지 확인합니다. 게임참가자가 25번 칸이나 25를 넘어서는 칸으로 이동했다면, 루프의 조건부는 거짓이 될 것이고 게임은 끝납니다. 
  위 예제의 경우에는 `while`루프를 사용하는 것이 적절합니다. 왜냐하면 `while` 루프가 시작되기 전에 게임가 얼마나 계속되어야 하는지 알지 못하기 때문입니다. `while` 루프를 쓰면 특정 조건이 충족될 때까지 계속해서 실행됩니다. 

### Do-While

`while`루프와 비슷하지만 약간 다른 루프도 있습니다. 이름은 `do-while`루프이며, 이 루프에서는 루프의 조건부가 검토되기 전에 루프 블록이 한 번 실행됩니다. 그런 다음 조건부가 거짓이 될 때까지 루프를 반복합니다. 

다음은 `do-while`문을 간단하게 일반화하여 나타낸 것입니다:

```
        do { 
              statements 
        } while   condition  
```
이번에도 뱀과 사다리 게임 예제를 사용하겠습니다. 다만 이번에는 `while` 루프 가 아니라 `do-while` 루프를 사용합니다. `finalSquare`, `board`, `square`, `diceRoll` 변수의 값은 앞에서 `while`루프를 사용했을 때와 동일한 값으로 초기화했습니다.

```
 let finalSquare  = 25 
 var board  = Int[](count : finalSquare +  1, repeatedValue: 0) 
 board[03]  = +08; board[06]  = +11; board[09]  = +09; board[10]  = +02   board[14]  = -10; board[19]  = -11; board[22]  = -02; board[24]  = -08 
 var square  = 0 
 var diceRoll  = 0  
```

이번 예제에서, 루프 내에서 처음으로 하는 작업은 사다리나 뱀이 들어 있는 지 확인하는 것입니다. 사다리를 타고 올라갔을 때 게임 참가자가 바로 25번 칸으로 이동하는 것은 불가능합니다. 즉 사다리를 타는 방법으로는 게임에서 이길 수 없습니다. 따라서 루프 안에서 칸에 뱀이나 사다리가 있는 지 여부를 확인하면 안전합니다. 

게임을 시작할 때, 참가자는 “0번 칸”에 있습니다. `board[0]`의 값은 항상 0이며, 이 값이 가지는 효과는 없습니다: 

```
do { 
     // move up or down for a snake or ladder square += board[square] 
// 주사위를 던진다
     if ++diceRoll  == 7 { diceRoll  =  1 } 
// 주사위를 던져서 나온 수만큼 이동한다
     square += diceRoll 
 } while square  < finalSquare 
println("Game over!")  
```

칸에 뱀이나 사다리가 있는지 프로그램이 확인한 후에, 주사위가 던져집니다. 게임 참가자는 `diceRoll` 값 만큼 칸을 움직입니다. 그런 다음 이번 루프 실행은 끝납니다. 

루프의 조건부의 내용(` while square < finalSquare`)은 이전 예제에서와 같지만, 이번에는 루프를 한 번 돈 다음에 이 조건부가 검토됩니다. 이 게임에는 `while`루프보다 `do-while` 루프의 구조가 더 적절합니다. 이번에 사용한 `do-while` 루프에서, `square += board[quare]` 이 부분은 루프의 `while` 조건부에서 `square`가 아직 `board` 상에 있다고 확인한 후에 항상 즉시 실행됩니다.  이렇게 하면 이전 예제에서 한 것처럼 배열의 범위를 확인할 필요가 없어집니다.
 
## 조건문  (Conditionals Statements)

특정 조건이 충족하는지에 따라 각각 다른 코드를 실행하는 것이 유용한 경우가 많습니다. 어떤 에러가 발생했을 때 특정 코드를 실행시키고 싶을 수도 있습니다. 또는 어떤 값이 너무 높거나 너무 낮을 때 메시지를 보여주고 싶을 수도 있습니다. 이렇게 하려면 코드를 조건문으로 쓰면 됩니다.

Swift에서 코드에 조건문을 추가하는 방법은 두 가지가 있습니다. 바로 `if`문과 `switch`문입니다. 보통 조건의 수가 많지 않을 때에는 보통 `if`문을 사용합니다. 한편 조건의 종류가 다양하고 복잡할 때에는 `switch`문이 더욱 적합하니다. 실행시킬 코드 브랜치를 선택하는데 패턴-매칭이 도움이 되는 경우에도 `switch`문을 사용합니다. 

### If

`if`문을 아주 단순하게 표현하면, 하나의 `if` 조건이 있습니다. 그럼 그 조건이 참인 경우에만 코드구문들이 실행됩니다. 

```
 var temperatureInFahrenheit  = 30    if temperatureInFahrenheit  <= 32 { 
      println("It's very cold. Consider wearing a scarf.") 
  } 
 // prints "It's very cold. Consider wearing a scarf."  
```

위 예제에서는 온도가 화씨 32도(물이 어는 온도)보다 낮은지 같은지 여부를 확인합니다. 온도가 화씨 32도 이하이면, 메시지가 출력됩니다. 그 외의 경우에는 아무런 메시지도 나오지 않으며 코드 실행은 `if`문을 닫는 중괄호(}) 다음으로 이동하여 계속됩니다.

`if`문을 사용할 때에는 `else` 절도 사용할 수 있는데, 여기에는 `if` 조건이 거짓일 때 실행되는 코드가 들어갑니다. `else` 키워드를 사용하여 나타냅니다:

```
  temperatureInFahrenheit  = 40 
  if temperatureInFahrenheit  <= 32 { 
      println("It's very cold. Consider wearing a scarf.") 
  } else { 
      println("It's not that cold. Wear a t-shirt.") 
  } 
 // prints "It's not that cold. Wear a t-shirt."  
```
 
위에서 두 개의 브랜치 중에서 하나는 항상 실행됩니다. 온도가 화씨 `40`도로 올라갔기 때문에, 스카프를 매라고 조언할 정도로 춥지 않습니다. 따라서 `else` 브랜치가 대신에 실행됩니다. 

추가적인 경우를 고려할 때에는 여러 개의 `if`문을 쓸 수도 있습니다:

```
  temperatureInFahrenheit  = 90 
  if temperatureInFahrenheit  <= 32 { 
      println("It's very cold. Consider wearing a scarf.") 
  } else if temperatureInFahrenheit  >= 86 { 
      println("It's really warm. Don't forget to wear sunscreen.") 
  } else { 
      println("It's not that cold. Wear a t-shirt.") 
  } 
 // prints "It's really warm. Don't forget to wear sunscreen."  
```

위 예에서, 추가적인 `if` 문이 들어갔습니다. 이 부분은 온도가 특별히 높을 때의 경우에 대한 것입니다. 

마지막에 쓴 `else` 절이 있습니다. 이 부분은 온도가 너무 높지도 낮지도 않은 경우에 보여주는 메시지를 출력합니다. 

하지만 마지막에 쓴 `else` 절은 선택적입니다. 조건들이 모든 경우를 다뤄야 하지 않는다면, `else`절을 안 써줘도 됩니다:

```
  temperatureInFahrenheit  = 72 
  if temperatureInFahrenheit  <= 32 { 
     println("It's very cold. Consider wearing a scarf.") 
  } else if temperatureInFahrenheit  >= 86 { 
     println("It's really warm. Don't forget to wear sunscreen.") 
  }  
```
 
이번 예제에서 온도는 너무 낮지도 너무 높지도 않았습니다. 그래서 `if`나 `else if`조건부에 들어 있는 코드는 실행되지 않았습니다. 그리고 아무 메시지도 출력되지 않았습니다: 

             
### Switch

`switch`문은 값을 검토해서 몇 가지 패턴과 비교합니다. 그런 다음, 처음 매칭되는 패턴이 있는 코드 블록을 실행시킵니다. `if`문을 사용할 때보다 `switch`문을 사용했을 때의 장점은, 여러 가지 경우에 대해서 처리할 수 있게 된다는 점입니다.

`switch`문에서는 어떤 값과 한 개 이상의 동일한 타입(type)의 값을 비교합니다: 

 ```
        switch   some value to consider  { 	
   	   //switch 고려하려는 값
        case   value  1 : 
              respond to value  1 
        case   value 2 , 
         value 3  :  
              respond to value 2 or 3 
        default : 
              otherwise, do something else 
        } 
```

`switch`문은 여러 가지 가능한 경우(case)로 구성되어 있습니다. 각 경우는 `case`라는 키워드로 시작됩니다. 특정 값과 비교할 수도 있지만, Swift에서는 더욱 복잡한 패턴과 비교하는(complex pattern matching) 여러 가지 방법이 있습니다. 이에 대해서는 이번 섹션의 뒷부분에 나옵니다. 

경우(switch case)의 본문 각각은 서로 독립된 코드실행 브랜치입니다(separate branch of code execution). 이는 `if` 문과 비슷합니다. `switch`문은 어느 브랜치를 선택할지 결정합니다. 이는 비교하려는 값에 대해서 스위치한다고 합니다.

`switch`문 안에는 가능한 경우(case)가 모두 고려되어야 합니다. 즉, 비교하려는 값이 가질 수 있는 모든 값이 해당하는 경우(case)가 반드시 있어야 합니다. 명시적으로 다루지 않는 값에 대해서는 디폴트 경우를 정의해서 처리할 수도 있습니다. 이렇게 할 때에는 키워드 `default`를 사용하며, 이 경우는 `switch`문 안에서 제일 마지막에 위치해야 합니다. 

다음의 예에서는 `switch`문을 사용해서 someCharacter 변수에 들어 있는 하나의 소문자를 검토합니다: 

```
  let someCharacter: Character  = "e" 
  switch someCharacter { 
  case "a", "e", "i", "o", "u": 
      println("\(someCharacter) is a vowel") 
case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",
 "n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z": 
      println("\(someCharacter) is a consonant") 
  default : 
      println("\(someCharacter) is not a vowel or a consonant") 
}
// prints "e is a vowel " 

```
위 예에서 `switch`문 안의 첫번째 경우(case)는 영어의 다섯 개 모음과 매치됩니다. 마찬가지로, 두번째 경우는 영어의 자음 소문자와 매치됩니다. 

자음과 모음 이외의 나머지 문자와 매치하는 경우(case)를 만들기 위해, 이 문자들을 다 써주려면 번거롭니다. 그래서 위 예제에서는 디폴트 경우(default case)를 사용하여, 자음과 모음 이외의 문자가 매치되는 경우를 처리했습니다. 이렇게 디폴트 경우를 써줌으로써 이 switch문은 빼먹은 경우(case)가 없는, 포괄적인 switch문이 됩니다. 
 
#### 디폴트로 다음 경우(case)로 넘어가지 않음! (No Implicit Fallthrough)

C언어나 Objective-C의 `switch`문과는 다르게, Swift에서는 디폴트로 다음 경우로 넘어가지 않습니다. 대신에, 한 경우와 매치되면, 그 경우에 해당하는 코드 블록이 실행된 후에, 전체 `switch`문이 종료됩니다. 이 때 명시적으로 `break`문을 써주지 않아도 됩니다. 이렇게 함으로써 C언어에서 보다 실수를 할 위험이 줄어듭니다. C언어에서는 실수로 `break`문을 빠뜨리면 의도하지 않게, 한 개 이상의 경우에 해당하는 코드블럭을 실행시킬 수도 있습니다.

> NOTE
필요하면, 매치된 경우 내의 코드블럭을 다 실행시키기 전에 빠져나올 수도 있습니다. 이에 대해서는 [Break in a Switch Statement]() 부분을 참고하세요.

각 경우의 본문은 적어도 한 개 이상의 실행가능한 구문을 포함해야 합니다. 다음의 예제코드처럼 쓰면 안됩니다. 왜냐하면 첫번째 경우의 본문이 비어 있기 때문입니다:

```
  let anotherCharacter: Character  = "a" 
  switch anotherCharacter { 
  case "a": 
  case "A": 
     println("T he letter A") 
  default : 
     println("Not the letter A") 
  } 
  // 컴파일 에러가 납니다.
```

C언어의 `switch`문에서는 anotherCharacter가 `“a”`와 `“A”` 경우 둘 다하고 매치되는 반면에, 위 예제의 `switch`문은 그렇지 않습니다. 위 예제에서는 컴파일 에러가 납니다. 왜냐하면 `“a”` 경우의 본문에 실행가능한 코드가 없기 때문입니다.  이런 방식 덕분에, 의도하지 않았는데 다음 경우로 넘어가는 실수를 방지할 수 있으며 의도가 더욱 명확하게 드러납니다. 

한 경우에 해당하는 매치를 여러 개 쓸 때에는 콤마`,`로 구분합니다. 그리고 길어지면 줄을 바꾸어 써도 됩니다:

```
switch   some value  to consider  { 
        case   value  1 , 
         value 2  : 
              statements 
          }  
```          
> NOTE
특정 경우의 본문이 실행된 후에, 다음 경우로 자동으로 넘어가게 하려면, 키워드 `fallthrough`를 사용하면 됩니다. 자세한 사용방법은 [Fallthrough]를 참고하세요.

#### 범위로 매치하기(Range Matching )

`switch` 경우 안에 있는 값에 대해서 어떤 범위 안에 들어있는지 여부를 확인할 수 있습니다. 아래의 예제는 수 범위를 사용하여, 수의 크기에 관계 없이, 어떤 수를 대략적으로 문자로 표현한 결과를 제공합니다:

```
  let count  = 3_000_000_000_000 
  let countedThings  = "stars in the Milky Way" 
  var naturalCount : String 
  switch count { 
  case 0: 
      naturalCount  = "no" 
  case  1...3: 
      naturalCount  = "a few" 
  case 4...9: 
       naturalCount  = "several"  
case 10...99:  
       naturalCount  = "tens of" 
  case 100...999: 
       naturalCount  = "hundreds of"  
case 1000...999_999:  
      naturalCount  = "thousands of" 
default: 
     naturalCount  = "millions and millions of" 
     println("There are \(naturalCount) \(countedThings).") 
// prints "There are millions and millions of stars in the Milky Way ."  
```

#### 튜플 (Tuples)

하나의 `switch`문 안에서 여러 개의 값을 사용할 때에는 튜플을 사용하면 됩니다. 튜플의 각 요소는 특정 값이나 값의 범위와 비교할 수 있습니다. 특정 값을 지정하지 않고 임의의 값을 나타내려면 밑줄 `_`을 사용하세요. 

아래의 예제에서는 (x, y)로 표현된 점을 취합니다. 이 점은 `(Int, Int)` 타입의 튜플로 나타내며, 이 점이 그래프 상에서 어느 구역에 위치하는지 분류합니다. 

```
   let somePoint  = (1,  1) 
   switch somePoint { 
   case (0, 0): 
        println("(0, 0) 은 원점에 있습니다") 
   case (_, 0): 
       println("(\(somePoint.0), 0)은 x축 상에 있습니다. ") 
   case (0, _): 
       println("(0, \(somePoint.1))은 y축 상에 있습니다.") 
   case (-2...2, -2...2): 
        println("(\(somePoint.0), \(somePoint.1))은 상자 안에 있습니다.")  
   default :  
         println("(\(somePoint.0), \(somePoint.1))은 상자 밖에 있습니다.")  
    }
    //prints "(1,  1) is inside the box"   
```
![그래프](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/image__256.png)

`switch`문은 점이 원점(0,0)에 있는지, 빨간선으로 표현된 x축 상에 있는지, 주황색으로 표현된 y축 상에 있는지, 그래프의 중앙에 파란색으로 칠한 4x4 상자 안에 있는지, 그 상자의 밖에 있는지를 판단합니다.

C언어와는 다르게, Swift에서는 하나의 값이나 값 묶음을 여러 개의 경우(case)와 비교할 수 있습니다. 사실 위 예제에서 점(0, 0)은 4개의 경우에 모두 해당됩니다. 하지만 여러 경우와 매치하는 경우에는, 첫번째 경우만 사용됩니다. 따라서 점(0,0)은 `case (0,0)` 하고만 매치하며, 다른 경우 안에 있는 코드는 실행되지 않습니다. 

#### 값을 상수와 묶기, 상수에 바인딩하기 (Value Bindings)

`switch`문에서 경우는 매치된 값이나 값들을 임시 상수나 변수에 바인딩할 수 있습니다. 이렇게 바인딩한 상수나 변수는 그 경우의 본문에서 사용할 수 있습니다. 이렇게 하는 것을 보고 값을 묶는다, 바인딩(value binding)한다고 합니다. 왜냐하면 해당 경우의 본문 내에서 값이 특정 상수나 변수에 “묶여” 있기 때문입니다. 

아래의 예제는 `(x, y)` 점을 취합니다. 이 점은 (Int, Int) 튜플로 표현되며, 예제는 이 점을 그래프상의 어느 구역에 위치하는지 분류합니다:

```
  let anotherPoint  = (2, 0) 
  switch anotherPoint { 
  case (let x, 0): 
     println("x축 상에 있으며 x의 값은 \(x)값입니다.") 
  case (0, let y): 
     println("y축 상에 있으며 y의 값은 \(y)입니다.") 
  case let (x, y): 
     println("(\(x), \(y))에 있습니다.") 
  }  

// 다음을 출력합니다:  "x축 상에 있으며 x의 값은 2입니다"  
```
![그래프](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/image__257.png)

`switch`문은 이 점이 빨간색선인 x축 상에 있는지, 주황색선인 y축 상에 있는지, 또는 그 이외의 구역에 있는지를 판단합니다. 

위 예제에서 세 개의 `switch` case에서는 플레이스홀더인 상수 x와 상수 y를 선언하였습니다. 이 둘은 `anotherPoint`로부터 튜플 값을 취합니다. 첫번째 case에서 `case (let x, 0)`은 `y`값이 `0`이면 모두 매치합니다. 이 때 점의 `x`값은 임시 상수인 `x`에 들어갑니다. 마찬가지로, 두번째 case인 `case(0, let y)`는 `x` 값이 `0`이면 모두 매치합니다. 그리고 이 때 점의 y값은 임시 상수인 y에 들어갑니다.

일단 임시 상수가 선언되면, case의 코드 블록 내에서 사용될 수 있습니다. 위 예제에서는 `println` 함수에서 값을 출력할 때 점의 좌표값을 빠르게 표현하기 위해서 이 상수를 사용하였습니다. 
 
위 예제에는 디폴트 경우(default case)가 없다는 점에 유의하세요. 마지막 경우인 `case let (x, y)`에서는 두 개의 플레이스 홀더 상수로 이루어진 튜플을 선언하는데, 이는 모든 점과 매치합니다. 결국, 남아있는 모든 가능한 값은 이 경우와 매치한다는 의미이며, 스위치 문에서 가능한 모든 경우를 다 포괄하고 있으므로 이 때에는 디폴트 경우를 쓸 필요가 없습니다. 
 
위 예제에서는 `x`와 `y`의 값이 경우의 본문에서 바뀔 필요가 없습니다. 그래서 키워드 `let`을 사용해서 상수로 선언하였습니다. 값이 바뀔 필요가 있다면 키워드 `var`를 사용해서 변수로 선언하면 됩니다. 이렇게 하면 임시 변수가 만들어지고, 적절한 값으로 초기화됩니다. 이 변수 값의 변화는, 해당 경우 내의 본문 내에서만 영향을 미칩니다. 

#### 키워드 `Where`

`switch`의 case에는 `where`절을 사용해서 조건부를 작성할 수도 있습니다. 

아래의 예제는 어떤 점 (x, y)과 그래프의 어느 구역에 위치하는지를 분류합니다:
 
```
  let yetAnotherPoint  = (1, -1) 
  switch yetAnotherPoint { 
  case let (x, y) where x  == y : 
      println("(\(x), \(y)) 는 x==y인 곳에 있습니다.") 
  case let (x, y) where x  == -y : 
      println("(\(x), \(y)) 는 x==-y인 곳에 있습니다.")  
  case let (x, y): 
      println("(\(x), \(y)) 는 기타 구역에 있습니다.") 
}  
// prints "(1, -1) 은 x==-y인 곳에 있습니다."  

```
![image__258.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/image__258.png)

위 예제의 `switch`문은 이 점이 `x == y`인 녹색사선 상에 있는지, `x==-y`인 보라색 사선 상에 있는지, 그 이외의 지점에 있는지 판단합니다. 

위 예제에서 세 개의 case는 플레이스홀더 상수 x와 상수 y를 선언하였으며, 이 둘은 점의 좌표값을 받아서 튜플로 가지고 있습니다. 이 상수는 `where`절에서 사용되어, 동적인 필터를 만들었습니다. 어떤 경우의 `where`절 내의 조건이 참이 되어야 어떤 값이 그 경우와 매치합니다. 
이전 예제에서와 마찬가지로, 위 예제의 마지막에 나오는 경우는 나머지 모든 가능한 값과 매치합니다. 따라서 디폴트 경우는 쓸 필요가 없습니다.

## 흐름제어 이동문(Control Transfer Statements)

Control Transfer Statements 는 control을 특정 코드로부터 다른 코드로 이동시키는 방법으로 코드가 실행되는 순서를 바꿉니다. Swift에는 네 가지 흐름제어 이동문이 있습니다:

* continue
* break
* fallthrough
* return

`control`, `break`, `fallthrough`문에 대해서는 여기서 자세히 다루고, `return`문은 [함수(Functions)]() 부분에서 다룹니다.

### Continue

`continue`문은 루프에게 현재 하고 있는 것을 멈추고, 루프의 다음번 이터레이션을 시작하라고 명령합니다. 즉 루프를 빠져나가지 않고 “루프의 현재 이터레이션에 대해서는 끝났다”다고 말하는 셈입니다. 

> NOTE
`for-조건부-증가부` 루프에서 `continue` 문을 호출한 이후에도, 증가자(incrementer)의 참 거짓 여부를 검토합니다. 루프 자체는 계속됩니다. 다만 루프 내 본문에 있는 코드가 건너뛰어질 뿐입니다. 

다음의 예제는 소문자로 된 문자열에서 모든 모음과 빈 칸을 제거하여 암호 같은 문구를 만들어냅니다:

```
  let puzzleInput  = "great minds think alike" 
  var puzzleOutput  = "" 
  for character in puzzleInput { 
     switch character { 
     case "a", "e", "i", "o", "u", " ": 
         continue 
     default : 
         puzzleOutput += character 
     } 
 println(puzzleOutput) 
}
// prints "grtmndsthnklk"  
```

위 예제코드에서는 모음이나 빈 칸을 만났들 때 `continue` 키워드를 호출합니다. 그러면 그 때 돌던 이터레이션은 즉시 종료하고 다음번 이터레이션의 시작 지점으로 이동합니다. 이렇게 하면 `switch` 블록이 모음과 빈 칸일 때만 매치하도록 할 수 있습니다. 출력해야 하는 모든 문자를 다 case로 검토할 필요가 없죠. 

### Break

`break`문은 흐름제어문 전체를 즉시 종료시킵니다. `break`문은 `switch`문이나 루프문 안에서 사용할 수 있는데요, `switch`문이나 루프문을 일찍 종료시키고 싶을 때 사용합니다. 

#### 루프문 안에서 쓴 break

`break`는 루프문 안에서 쓰이면, 루프의 실행을 즉시 종료시키고, 루프를 닫는 중괄호(`}`) 뒤에 나오는 코드로 코드 실행을 이동시킵니다. `break` 뒤에 있는 나머지 코드부분은 이터레이션을 돌지 않습니다. 그리고 그 다음 이터레이션은 시작되지 않습니다. 

#### Switch 문 안에서 쓴 break

`break`를 `switch`문 안에서 사용하면, `switch`문 전체를 즉시 종료시킵니다. 그리고 코드 실행을 `switch`문이 끝나는 부분(}) 이후로 이동시킵니다. 
`switch`문에서 한 가지 이상의 경우(case)에 매치하는 경우를 무시해야 할 때, 사용할 수 있습니다. 

Swift에서 `switch`문은 가능한 모든 경우를 다루어야 하며, 각 경우는 모두 실행가능한 코드를 포함해야 합니다. 하지만 때로는 어떤 case에 해당하면 그 경우에는 아무것도 하지 않고 넘어가야 할 수도 있습니다. 이렇게 할 때 아무것도 하지 않고 넘어가야 하는 case의 본문에 `break`문을 사용하면 됩니다. 그 case가 매치되는 경우, 본문에 있는 `break`문이 `switch`문의 시행을 즉시 종료시킵니다. 

> NOTE
case 본문에 주석만 있는 경우에도 컴파일 에러가 납니다. 주석은 실행문이 아니므로, 그 case를 실행하지 않고 넘어가려면 `break`문을 써야 합니다.  

다음의 예제는 `Character`변수의 값(Character value)이 무엇인가에 따라서, 이 값이 네 개 언어 중 하나에서 숫자를 나타내는지 여부를 판단합니다. 과감하게 몇 가지 값은 하나의 case와 매치되도록 했습니다: 

```
let numberSymbol: Character = "三"  // Simplified Chinese for the number 3
var possibleIntegerValue: Int?
switch numberSymbol {
case "1", "١", "一", "๑":
    possibleIntegerValue = 1
case "2", "٢", "二", "๒":
    possibleIntegerValue = 2
case "3", "٣", "三", "๓":
    possibleIntegerValue = 3
case "4", "٤", "四", "๔":
    possibleIntegerValue = 4
default:
    break
}
if let integerValue = possibleIntegerValue {
    println("The integer value of \(numberSymbol) is \(integerValue).")
} else {
    println("An integer value could not be found for \(numberSymbol).")
}
// prints "The integer value of 三 is 3."
```

위 예제는 `numberSymbol`이 라틴어, 아랍어, 중국어, 태국어의 숫자 1에서 4에 해당하는지 판단합니다. 매치하는 경우가 나타나면, `switch`문은 `possibleIntegerValue`에 해당하는 정수값을 넣습니다. 이때 `possibleIntegerValue`는 선택적입니다. 즉 필수로 선언해야 하는게 아니라 case 본문 내에서 사용할 때에 선언하는 것이죠.

`switch`문이 다 실행된 후에, 원하는 값을 찾았는지 판단하기 위해 바인딩을 사용합니다. 이 바인딩도 선택적으로 사용하는 것입니다. 변수 `possibleIntegerValue`는 선택적 자료형이기 때문에 초기값으로 nil을 갖고 있습니다. `nil`값을 명시적으로 넣어주지 않아도 암묵적으로 `nil` 값을 갖습니다. 따라서 `possibleIntegerValue`가, 네 개 case중 어느 하나와 매치가 되어서, 어떤 실제 값을 갖게 되었을 때에야 바인딩이 일어날 것입니다. 

위 예제에서 매치가 가능한 모든 문자를 case로 만드는 것은 너무 일이 많습니다. 그래서 default case를 두어서 매치되지 않은 문자에 대해서 다루도록 했습니다. 이 default case는 아무 작업도 하지 않아도 됩니다. 그래서 본문에 `break`문만 두었습니다. 
`default` case와 매치한 경우에, `break`문은 `switch`문의 실행을 종료시키고, 코드 실행은 if let문으로 이동합니다. 

### Fallthrough

Swift에서 `switch`문은 한 case와 매치한 후에, 다음 case로 넘어가지 않습니다(fallthrough). 대신헤 한 case와 매치하고 그 본문에 있는 코드가 실행된 후에, `switch`문 전체가 종료됩니다. 이와 달리 C언어에서는 fallthrough가 일어나지 않게 하려면 명시적으로 각 case 본문 끝부분에 `break`를 써주어야 합니다. 이 점을 보면 C언어보다 Swift에서 `switch`구문이 더욱 예측가능하다고 할 수 있습니다. 즉 실수로 의도하지 않았는데 case를 여러 개 실행시키는 것을 방지할 수 있습니다. 

C언어에서 일어나는 fallthrough를 꼭 사용해야 한다면, `fallthrough` 키워드를 사용해서 필요할 때 사용할 수도 있습니다. 다음 예제는 어떤 수를 묘사하는 데에 `fallthrough`를 사용합니다. 

```
let integerToDescribe  = 5 
  var description  = "수 \(integerToDescribe) 는" 
  switch integerToDescribe { 
  case 2, 3, 5, 7, 11, 13, 17, 19: 
      description += "소수이며, 또한" 
      fallthrough 
  default : 
      description += " 정수입니다." 
  } 
println (description)  
//prints "수 5는 소수이며, 또한 정수입니다."  
```

위 예제에서는 이름이 `description`인 `String` 변수를 하나 만들어서 초기값을 넣습니다. 그런 다음 이 함수는 `switch`문을 사용해서 `integerToDescribe`의 값이 어느 경우에 해당하는지 검토합니다. `integerToDescribe`의 값이 목록에 있는 소수에 해당하면, 함수는, ‘소수이며 또한’이라는 묘사부분을 `description` 변수의 끝에다가 붙입니다. 그런 다음 키워드 `fallthrough`를 사용해서 `default` case의 경우에 해당하는지도 봅니다. `default` case는 추가적인 텍스트를 `description` 변수의 끝에 붙입니다. 이제 `switch`문 실행이 완료되었습니다. 

`integerToDescribe`의 값이 목록 안에 있는 소수에 해당하지 않으면, 첫번째 case와는 매치하지 않습니다. 그런 다음 `integerToDescribe`는 어떤 값이더라도 매치하는 `default` case와 매치합니다. 

`switch`문 실행이 완료한 후에, 수에 대해 기술한 `description`이 `println` 함수를 사용하여 출력됩니다. 위 예제에서 `5`는 소수라고 확인되었습니다. 

> NOTE
키워드 `fallthrough`를 사용할 때, 조건을 걸어서 다음으로 넘어갈 case를 지정할 수는 없습니다. C언어에서 처럼, `fallthrough` 키워드를 사용하면 단지, 다음에 나오는 case로 넘어갈 수 있을 뿐입니다.   

### 이름표가 붙은 구문 (Labeled Statements)

Swift에서는 루프문이나 `switch`문 안에서 루프문이나 `switch`문을 중첩해서 사용함으로써 복잡한 흐름 제어구조를 만들 수도 있습니다. 그리고 루프문과 `switch`문 안에서 `break`문을 사용해서 이들을 바로 종료시킬 수도 있습니다. 이런 경우에 `break`로 종료시키려는 것이 어느 루프문 또는 switch문인지 명시적으로 표시해주면 좋습니다. 마찬가지로, 여러 개의 루프문을 중첩했을 때 `continue`문이 어느 루프문에 영향을 미치는지 명시적으로 표시해주면 좋습니다. 

영향을 미칠 대상을 명시적으로 표시하기 위해서는, 루프문이나 `switch`문에다가 구문 이름표(statement label)를 붙일 수 있습니다. 그리고 `break`문이나 `continue`문을 사용할 때 어느 것을 종료시킬지 구문 이름표를 붙임으로써 명시적으로 표시할 수 있습니다. 

구문에다가 구문 이름표를 붙이는 방법은 구문의 introducer 키워드 앞에다가 이름을 쓰고 그 뒤에 콜론`:`을 찍는 것입니다. 다음은 `while` 루프 안에서 구문 이름표를 사용한 예입니다. 이 사용방법은 다른 루프문이나 `switch`문의 경우에도 동일합니다:

```
        label name  : while   condition  { 
             statements 
       } 

```

다음 예제에서는 앞에서 다루었던 뱀과 사다리 게임을 약간 변형시킨 것인데, `break`문과 `continue`문을 사용할 때 구문 이름표가 붙은 `while`루프를 사용합니다.
이번에는 게임 규칙이 하나 추가됩니다:

* 게임에서 이기려면, 정확하게 25번 칸 위에 위치해야 합니다. 

주사위를 던졌더니, 25번 칸 보다 넘어가는 경우에는, 25번 칸으로 이동할 수 있는 수가 나올 때까지 주사위를 다시 던져야 합니다.

게임판은 이전에 사용한 것과 동일합니다:
![뱀과 사다리 - 게임판](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/image__260.png)


변수 `finalSquare`, `board`, `square`, `diceRoll`은 이전 예제에서와 동일한 값으로 초기화합니다:

```
  let finalSquare  = 25 
 var board  = Int[](count : finalSquare +  1, repeatedValue: 0) 
  board[03]  = +08; board[06] = +11; board[09]  = +09; board[10]  = +02 
  board[14]  = -10; board[19]  = -11; board[22]  = -02; board[24]  = -08 
 var square  = 0 
 var diceRoll  = 0  

```

이번 게임에서는 `while`루프와 `switch`문을 사용해서 게임을 구현합니다. `while` 루프의 구문 이름표는 `gameLoop`라고 붙여서, 이 루프가 뱀과 사다리 게임 전체에 대한 주요 루프임을 나타냅니다. 

`while` 루프의 조건부는 `while squre != finalSquare`라고 하여 25번 칸에 정확하게 위치해야 한다는 규칙을 반영합니다: 

```
 gameLoop: while square  != finalSquare { 
if ++diceRoll  == 7 { diceRoll  =  1 } 
      switch square + diceRoll { 
      case finalSquare: 
          // 주사위에서 나온 수만 큼 이동해서 마지막 칸에 도달하면, 게임이 끝납니다. 
          break gameLoop 
      case let newSquare where newSquare  > finalSquare: 
          // 주사위에서 나온 수만 큼 이동했을 때 마지막 칸을 넘어가면, 게임이 계속되고 주사위를 다시 던집니다. 
          continue gameLoop 
      default : 
 // 주사위에서 나온 수 만큼 이동합니다. 
           square += diceRoll 
           square += board[square] 
}
println("Game over!")  
```

루프가 돌 때마다 초반에 주사위가 던져집니다. 이 때 게이머를 바로 이동시키지 않고 `switch`문을 사용해서 이동시켰을 때의 결과를 검토해서 이동시킬지 여부를 판단합니다: 

- 주사위에서 나온 수가 게이머를 마지막 칸으로 이동시키면 게임은 끝납니다. `break gameLoop`라고 쓴 부분은 코드 실행을 `while` 루프문 바깥에 있는 줄로 이동시키며, 게임을 끝냅니다. 
- 주사위에서 나온 수가 게이머를 마지막 칸을 넘어간 칸으로 이동시키면, 게이머는 이동할 수 없고, 다시 주사위를 던져야 합니다. `continue gameLoop`라고 쓴 부분은 현재의 while loop 이터레이션을 종료시키며, 루프의 다음번 이터레이션을 돌게 합니다. 
- 이 외의 나머지 경우에 대해서는, 게이머는 주사위에서 나온 만큼 이동할 수 있습니다. 게이머는 `diceRoll`값 만큼 앞으로 이동합니다. 그리고 프로그램은 이동한 칸에 뱀이나 사다리가 있는지 확인합니다. 그런 다음 루프는 끝나고, 코드 실행은 `while` 조건부로 이동하여, 게임이 지속될지 여부를 판단합니다.


> NOTE
위 예제에서 `break`문 안에 `gameLoop`라는 구문 이름표를 쓰지 않으면, `while`문이 아니라 `switch`문에서만 빠져나오게 됩니다.  `gameLoop`라는 이름표를 사용하였기 때문에 어느 흐름제어문(control statement)가 종료되어야 하는지 명확해 집니다.
또한 엄밀히 말하면, 루프의 다음번 이터레이션으로 넘어가기 위해서 `continue gameLoop`를 호출할 때에, 반드시 `gameLoop`를 써주어야 하는 것은 아닙니다. 게임 내에서 루프는 하나 밖에 없기 때문에, `continue`를 썼을 때 어느 루프의 다음 이터레이션으로 넘어갈지에 대해 애매모호함이 발생하지 않습니다. 그렇더라도, 이 때 `gameLoop` 이름표를 사용해서 나쁠 것은 없습니다. 오히려 이렇게 써주면, `break`문에서 이름표를 사용한 것과 일관성을 유지하는 효과가 있으며, 코드에서 게임의 규칙(game’s logic)을 읽어내고 이해하는데에 도움을 줍니다.

