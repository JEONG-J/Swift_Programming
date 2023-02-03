
# my Swift Study - Chapter7[함수]
    ※ 스위프트에서 함수는 일급 객체이기 때문에 하나의 값으로도 사용할 수 있다.
## 함수와 메서드
    ● 메서드 : 구조체, 클래스, 열거형 등 특정 타입에 연관되어 사용하는 함수
    ● 모듈 전체에서 전역적으로 사용할 수 있는 함수

## 함수의 정의와 호출
    ● 재정의(오버라이딩) 중복 정의(오버로딩) 모두 지원
    ● ->를 사용하여 반환 타입을 명시한다.

```Swift
    func hello(name: String) -> String{
        return "hello "
    }

    let Hello: String = hello(name: "my")
    print(Hello)
    // hello my
    
    func introduce(name: String) -> String{
        return "my name is \(name)!!"
    }

    let introduceMy: String = introduce(name: "jay")
    print(introduceMy)
    // my name is jay
```
## 매개변수가 없는 함수와 매개변수가 여러개인 함수
    ● 매개변수 여러 개 경우 쉼표(,) 구분

```Swift
// 매개변수 없는 것
    func hello() -> String{
        return "hello"
    }

    print(hello())
    // hello

// 매개변수 여러 개
    func hello(name: String, your: String) -> String{
        return "hello \(your), my name is \(name)"
    }

    print(hello(name: "haha", your: "hoho"))
    // hello hoho, my name is haha
```

## 매개변수 이름과 전달인자 레이블
    ● 전달인자 레이블을 사용하면 함수 외부에서 매개변수의 역할을 좀 더 명확히 할 수 있다.
    ● 매개변수 이름 앞에 한 칸을 띄운 후 전달인자 레이블을 지정한다.
    ● 함수 내부에서 전달인자 레이블을 사용할 수 없으며, 함수를 호출할 때 매개변수 이름을 사용할 수 없다.
    ● 전달인자 레이블만 다르게 써주더라도 함수 중복 정의로 동작할 수 있다.

```Swift
    func sayHello(from myName: String, to name: String) ->  String{
     return "hello \(name), my name is \(myName)"
    }

    print(sayHello(from "haha", to "hoho"))
```

```Swift
// 전달인자 레이블 없는 경우
    func sayHello(_ name: String, _ times: Int) -> String{
        var result: String = ""
        for _ in 0..<times{
            result += "Hello \(name)!"
        }
        return result
    }

    print(sayHello("haha", 2))
```

```Swift
// 전달인자 레이블 변경을 통한 함수 중복 정의(오버로드)
    func sayHello(to name: String, _ times: Int) -> String{
        var result: String = ""

        for _ in 0..<times{
            result += "Hello \(name)! + "
        }
        return result
    }

    func sayHello(to name: String, repeatCount times: Int) -> String{
        var result: String = ""

        for _ in 0..<times{
            result += "Hello \(name)! + "
        }
        return result
    }

    print(sayHello(to: "hoho", 2))
    print(sayHello(to: "hoho", repeatCount: 2))
```

## 매개변수 기본값
    ● 매개변수 기본값이 있는 함수는 함수를 중복 저의한 것처럼 사용할 수 있다

```Swift
    func sayHello(_ name: String, times: Int = 3) -> String {
        var result: String = ""

        for _ in 0..<times{
            result += "hello \(name)!!"
        }
        return result
    } 

    print(sayHello("hoho")) //times 매개변수의 전달 값을 넘겨주지 않아 기본값 3을 반영
    print(sayHello("hoho", times: 2)) //times 매개변수의 전달값을 2로 넘겨주었기 때문에 두 번 출력
```

## 가변 매개변수와 입출력 매개변수
    ● 매개변수로 몇 개의 값이 들어올지 모를 때, 가변 매개변수를 사용할 수 있다.
    ● 함수마다 가변 매개변수는 하나만 가질 수 있다.
    ● 함수형 프로그래밍 패러다임을 사용할 때 입출력 매개변수를 사용하지 않는 것이 좋다.
    ● 참조는 inout 매개변수로 전달될 변수 또는 상수 앞에 &를 붙인다.
    ● 입출력 매개변수는 매개변수 기본값 x, 가변 매개변수 x, 전달인자 x
```Swift
    func sayHelloTofriends(me: String, friends names: String...) -> String{
        var result: String = ""

        for friend in names{
            result += "Hello \(friend)!!"
        }

        result += "I'm" + me + "!"
        return result
    }

    print(sayHelloToFriends(me: "haha", friends: "hoho", "hehe", "hihi"))

    print(sayHelloToFriends(me: "haha"))
```

```Swift
//inout 매개변수 활용
    var numbers: [Int] = [1,2,3]

    func nonReferenceParameter(_ arr: [Int]) {
        var copiedArr: [Int] = arr
        copiedArr[1] = 1
    }

    func referenceParameter(_ arr: inout [Int]){
        arr[1] = 1
    }

    nonReferenceParameter(numbers)
    print(numbers[1]) // 2 이유 : 값이 복사되어 전달 되었기 때문

    referenceParameter(&numbers)
    print(numbers[1]) //1 이유 : 값이 참조되었기 때문
```

## 반환이 없는 함수
    ● 반환  타입이 Void이거나 생략되어 있으면 반환 값이 없는 함수

```Swift
    func sayHello(){
        print("Hello")
    }
    sayHello() // Hello

    func goodBye() -> Void{
        print("good bye")
    }
    goodBye() // good bye

```

## 데이터 타입으로서의 함수
    ● 함수는 매개변수 타입과 반환 타입으로 구성된 하나의 타입으로 사용
    ● 함수를 전달인자로 받을 수 있고, 반환 값으로 돌려줄 수 있다.
```Swift
// 함수의 타입 사용
    typealias CalculateTwoInts = (Int, Int) -> Int

    func addTwoInts(_ a: Int, _ b: Int) -> Int{
        return a + b
    }

    func multiplyTwoInts(_ a: Int, _ b: Int) -> Int{
        return a * b
    }

    var mathFunction: CalculateTwoInts = addTwoInts

    print(mathFunction(2,5)) // 7

    mathFunction = multiplyTwoInts

    print(multiplyTwoInts(2,5)) // 10

```

```Swift
    func printMathResult(_ mathFunction: CalculateTwoInts, _ a: Int, _ b: Int){
        print(mathFunction(a,b))
    }

    printMathResult(addTwoInts(3,2)) // 5
```

```Swift
//특정 조건 함수 반환
    func chooseMathFunction(_ toAdd: Bool) -> CalculateTwoInts {
        return toAdd ? addTwoInts: multiplyTwoInts
    }

    printMathResult(chooseMathFunction(true), ,2 5) // 7
```

##  중첩 함수
    ● 함수 내부에 함수를 넣는다.
    ● 함수가 하나의 하나의 반환 값으로 사용될 수 있으므로 중첩 함수를 담은 함수가 중첩 함수를 반환

```Swift
//함수 중첩 x
    typealias MoveFunc = (Int) -> Int

    func goRight(_ currentPosition: Int) -> Int{
        return currentPosition + 1
    }

    func goLeft(_ currentPosition: Int) -> Int{
        return currentPosition - 1
    }

    func functionForMove(_ shouldGoLeft: Bool) -> MoveFunc{
        shouldGoLeft ? goLeft: goRight
    }

    var position: Int = 3
    let moveToZero: MoveFunc = functionForMove(position > 0)
    print("원점 출발")

    while position != 0{
        print(position)
        position = moveToZero(position)
    }
    print("도착")
```

```Swift
//함수 중첩 o
    typealias MoveFunc = (Int) -> Int

    func functionForMove(_ shouldGoLeft: Bool) -> MoveFunc{

        func goRight(_ currentPosition: Int) -> Int{
        return currentPosition + 1
    }

    func goLeft(_ currentPosition: Int) -> Int{
        return currentPosition - 1
    }

        return shouldGoLeft ? goLeft: goRight
    }

    var position: Int = 3
    let moveToZero: MoveFunc = functionForMove(position > 0)
    print("원점 출발")

    while position != 0{
        print(position)
        position = moveToZero(position)
    }
    print("도착")
```

##  종료되지 않는 함수
    ● return 되지 않는 함수
    ● 비반환 함수, 비반환 메서드 -> 정상적으로 끝날 수 없는 함수
    ● 오류를 보고하는 등의 일을 하고 프로세스를 종료
    ● 반환 타입 `Never`

```Swift
    func crushAndBurn() -> Never{
        fatalError("Something very")
    }

    crushAndBurn() // 프로세스 종료 후 보고

    func someFunction(isAllWell: Bool){
        guard isAllWell else{
            print("침입")
            crushAndBurn()
        }
        print("isAllWell")
    }

    someFunction(isAllWell: false) // 침입
    someFunction(isAllWell: true) // isAllWell
```

## 반환 값을 무시할 수 있는 함수
    ● 의도적으로 함수의 반환 값을 사용하지 않을 경우, 반환 값 무시 할 수 있다.
    ● @discardableResult 속성 부여

```Swift
    func say(_ something: String) -> String{
        print(someting)
        return something
    }

    @discardableResult func discardableResultSay(_ something: string) -> String{
        print(something)
        return something
    }

    say("hello") // hello + 반환 값 미사용 경고
    discardableResultSay("hello") // hello + 경고 x
```
---
야곰 지음 - 스위프트 프로그래밍3판을 공부하고 작성하였음
---