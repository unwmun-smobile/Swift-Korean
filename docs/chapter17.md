---
id: chapter17
title: 해제 (Deinitialization)
---
> Translator : 물좀 (메일주소)

deinitializer는 임의의 class 인스턴스가 할당해제(deallocate) 되기 직전에 호출된다. initializer를 `init` 키워드 안에 기술했던 것처럼, deinitializer는 `deinit` 키워드 안에 적어 넣는다. deinitializer는 class 타입 인스턴스에서만 사용할 수 있다.

## Deinitialization (해제)의 원리

[Automatic Reference Counting]() 에서 설명된 바와 같이, Swift에서는 더 이상 사용되지 않는 인스턴스는 ARC에 의해 자동으로 할당해제 된다. 따라서 대부분의 경우 사용자가 직접 할당해제 할 필요가 없다. 하지만, 사용자의 인스턴스에서 직접 리소스를 할당하여 사용했다면, 해제할 때도 직접 해제해 주어야 한다. 가령, 사용자의 인스턴스에서 파일을 열어서 사용했다면, 해제할 때, 직접 파일을 닫아주어야 한다. class마다 오직 한 개의 deinitializer만을 사용할 수 있으며, 파라미터 없이 정의한다. 
```
deinit {
     // perform the deinitialization
}
```
deinitializer는 할당해제(deallocation)가 일어나기 직전에 자동으로 호출되기 때문에, 사용자가 직접 호출하는 것을 허용하지 않는다. subclass는 superclass의 deinitializer를 상속받기 때문에, subclass의 deinitializer가 호출되어 작업을 마친 후, superclass의 deinitializer가 자동으로 호출된다. superclass의 deinitializer는 subclass의 deinitializer가 정의되지 않았더라도 항상 호출된다. deinitializer가 아직 호출되지 않은 인스턴스는 해제(deallocation)가 되지 않은 상태이고, deinitializer는 자신이 속한 인스턴스의 모든 속성(가령, 닫아야할 파일의 이름과 상태)을 변경할 수 있다. 

## Deinitializer 사용 예

deinitializer를 사용하는 간단한 예를 들어보자. 간단한 게임을 만들기 위해, Bank와 Player 라는 두개의 데이터 타입을 정의하기로 하자. `Bank`는 화폐를 만드는데, 최대 10,000개의 동전을 유통할 수 있다. 게임에서는 오직 한 개의 `Bank`만 있다고 가정, `Bank`는 static으로 구현되었다. 
```
struct Bank {
     static var coinsInBank = 10000
     static func vendCoins(var numberOfCoinsToVend: Int) -> Int {
          numberOfCoinsToVend = min(numberOfCoinsToVend, coinsInBank)
          coinsInBank -= numberOfCoinsToVend
          return numberOfCoinsToVend
     }
     static func receiveCoins(coins: Int) {
     coinsInBank += coins
}
```
`Bank`클래스는 `coinsInBank` 속성으로 현 상태의 동전 수를 유지한다. `vendCoins`과  `receiveCoins` 메소드는 동전을 인출하거나 예치할 때 사용한다.

`vendCoins` 메소드는 은행에 동전이 남아 있는지 확인하고 Player에게 인출을 허용한다. Player 가 요청한 것보다 동전이 적게 남아 있으면, 남아 있는 만큼만 인출할 수 있다. (물론 은행에 동전이 전혀 없다면, ‘0’ 를 리턴한다). `numberOfCoinsToVend` 변수는 파라미터로 입력받아 변경이 가능하게 만들었다. `receiveCoins` 메소드는  단순히 Player가 예치하는 동전을 은행에 더해서 쌓도록 되어 있다. 

`Player` 클래스를 보면, `coinsInPurse` 속성에  게임 플레이어가 현재 보유한 동전을 기록한다. 
```
class Player {
     var coinsInPurse: Int
     init(coins: Int) {
          coinsInPurse = Bank.vendCoins(coins)
     }
     func winCoins(coins: Int) {
          coinsInPurse += Bank.vendCoins(coins)
     }
     deinit {
          Bank.receiveCoins(coinsInPurse)
     }
}
```
`Player` 인스턴스는 은행으로부터 동전을 받으면서 초기화된다. 경우에 따라 은행에 충분한 코인이 남아 있지 않다면 요청한 만큼의 동전보다 적게 받을 수도 있다. `winCoins` 메소드는 은행에서 (coins : Int) 만큼의 동전을 받아 Player의 지갑에 더해 준다. `Player` 클래스에는 deinitializer 가 정의되어 있는데, 앞서 설명한 바 처럼 `Player` 인스턴스가 해제(deallocate)되기 직전에 호출된다. 여기서는 단순히 플레이어가 가진 모든 동전을 다시 은행으로 되돌려 보내는 작업을 한다.
```
var playerOne: Player? = Player(coins: 100)
println("A new player has joined the game with \(playerOne!.coinsInPurse) coins")
// prints "A new player has joined the game with 100 coins"
println("There are now \(Bank.coinsInBank) coins left in the bank")
// prints "There are now 9900 coins left in the bank"
```
`Player` 인스턴스를 만들때, 은행에 100 코인을 요청한다. 이 `Player` 인스턴스는 optional 변수 `playerOne`에 저장된다. 여기서 optional 변수가 사용된 이유는 게임 player들이 수시로 게임에서 나갈 수 있기 때문이다. optional을 사용함으로써 그 인스턴스가 현재 게임에 있는지 아닌지를 추적할 수 있다. 

또한 느낌표 (`!`) 연산자를 사용, `coinsInPurse` 가 호출될때, optional으로 선언된 기본값 (100) 대신 현재 인스턴스가 가지고 있는 값 (2100)이 출력되게 할 수도 있다. 
```
playerOne!.winCoins(2000)
println("PlayerOne won 2000 coins & now has \(playerOne!.coinsInPurse) coins")
_// prints "PlayerOne won 2000 coins & now has 2100 coins"_
println("The bank now only has \(Bank.coinsInBank) coins left")
_// prints "The bank now only has 7900 coins left"_
```
결과를 보면, `playerOne`은 요청한 2000 코인 모두를 은행으로부터 받아서, 현재는 2100 코인을 가지고 있으며 은행에 남아 있는 코인 수는 7900이 되었다.
```
playerOne = nil
println("PlayerOne has left the game")
// prints "PlayerOne has left the game"
println("The bank now has \(Bank.coinsInBank) coins")
// prints "The bank now has 10000 coins"
```
`playerOne`을 `nil`로 셋팅함으로써, `playerOne`이 게임에서 나간 상태를 표현할 수 있으며, `playerOne`이 가지고 있던 코인은 모두 은행에 환수되었음을 알 수 있다. 
그 과정은  `playerOne` 변수의 참조(reference)는 더 이상 유효하지 않게 되고, denitializer가 호출되고, deallocation 과정으로 사용중이던 메모리도 환원된다. 
