
# my Swift Study - Chapter13[클로저]
* 일정 기능을 하는 코드를 하나의 블록으로 모아놓은 것, 함수는 클로저의 한 형태
* 변수나 상수가 선언된 위치에서 참조를 획득하고 저장할 수 있다.
* 함수의 모습이 아닌 하나의 블록의 모습으로 표현될 수 있는 방법
## 클로저의 세 가지 형태
    ● 이름이 있으면서 어떤 값도 획득하지 않은 전역함수의 형태
    ● 이름이 있으면서 다른 함수 내부의 값을 획득할 수 있는 중첩된 함수의 형태
    ● 이름이 없고 주변 문맥에 따라 값을 획득
##  클로저의 다양한 형태
    ● 타입을 문맥을 통해 유츄할 수 있다. 매개변수와 반환 값의 타입을 생략할 수 있다.
    ● 단 한 줄만의 표현만 들어있다면 암시적으로 이를 반환 값으로 취급한다.
    ● 축약된 전달인자 이름을 사용한다.
    ● 후행 클로저 문법을 사용할 수 있다.
## 기본 클로저
> sorted(by:) : 기존의 배열은 변경하지 않고 정렬된 배열을 새로 생성하여 반환<br> 
```Swift
    let names: [String] = ["riahn", "eric", "jenny"]
    
    func backwards(first: String, second: String) -> Bool{
        print("\(first) \(second) 비교")
        return first > second //값이 더큰 first 문자열이 second 문자열보다 앞쪽에 정렬된다.
    }

    let reserved: [String] = names.sorted(by: backwards)
    print(reserved) // ["rian", "eric", "jenny"]
```
> 함수 이름부터 매개변수 표현까지 부가적인 표현이 많다. 그러므로 클로저 표현을 사용하여 간결하게 할 수 있다.

> 클로저 표현 형식
```Swift
    { (매개변수) -> 반환타입 in
            실행코드
    }
```

> sorted(by:)메서드에 클로저 전달
```Swift
    let reserved: [String] = names.sorted(by: {(first: String, seocnd: String) -> Bool in
        return first > second
    })

    print(reserved) //["rian", "eric", "jenny"]
```

## 후행 클로저
    ● 맨 마지막 전달인자로 전달되는 클로저에만 해당
    ● 전달인자로 클로저 여러 개를 전달할 때는 맨 마지막 클로저만 후행 클로저 사용할 수 있다.
    ● 단 하나의 클로저만 전달인자로 전달하는 경우에는 소괄호를 생략할 수 있다.
    ● 매개변수에 클로저가 여러 개 있는 경우, 다중 후행 클로저 문법을 사용할 수 있다.
    ● 다중 후행 클로저를 사용하는 경우, 중괄호를 열고 닫음으로써 클로저를 표현할 수 있다. 첫 번째 클로저의 전달인자 레이블은 생략한다.
> 후행 클로저 표현
```Swift
    //후행 클로저
    let reserved: [String] = names.sorted(){ (first: String, second: String) -> Bool in
     return first > second
     }

     // 후행 클로저 소괄호 생략
     let reserved: [String] = names.sorted { (first: String, seocnd: String) -> Bool in
     return first > second
     }

     func doSometing(do: (String) -> Void, onSuccess: (Any) -> Void, onFailure: (Error) -> Void){...
     }

     //다중 후행 클로저 사용
     doSomething{ (someString: String) in //do closure
     } onSuccess: {(result: Any) in //success closure
     } onFailure: {(error: Error) in //failure closure
     }
```

## 클로저 표현 간소화

● [문맥을 이용한 타입 유추](#문맥을-이용한-타입-유추)<br>
● [단축 인자 이름](#단축-인자-이름)<br>
● [암시적 반환 표현](#암시적-반환-표현)<br>
● [연산자 함수](#연산자-함수)<br>

## 문맥을 이용한 타입 유추
    ● 매개변수의 타입이나 개수, 반환 타입 등이 같아야 전달인자로서 전달할 수 있다.
    ● 전달인자로 전달할 클로저는 이미 적합한 타입을 준수
    ● 전달인자로 전달하는 클로저를 구현할 때는 매개변수의 타입이나 반환 값의 타입을 굳이 표현해주지 않고 생략할 수 있다.

> 클로저의 타입 유추
```Swift
//클로저의 매개변수 타입과 반환 타입을 생략하여 표현할 수 있다.
    let reserved: [String] = names.sorted {(first, second) in
    return first > second
    }
```

## 단축 인자 이름
    ● 단축 인자 이름은 첫 번째 전달인자부터 $0, $1, $2 ... 순서로 $와 숫자의 조합으로 표현한다.
    ● 매개변수 및 반환 타입과 실행 코드를 구분하기 위해 사용했던 키워드 in을 사용할 필요가 없다.

> 단축 인자 이름 사용
```Swift
    let reserved: [String] = names.sorted{
        return $0 > $1
    }
```

## 암시적 반환 표현
    ● 클로저가 반환 값을 갖는 클로저이고 클로저 내부의 실행문이 단 한 줄이라면, 암시적으로 그 실행문을 반환 값으로 사용할 수 있다.

> 암시적 반환 표현의 사용
```Swift
    let reserved: [String] = names.sorted { $0 > $1
    }
```

## 연산자 함수
    ● 매개변수의 타입과 반환 타입이 연산자를 구현한 함수의 모양과 동일하다면 연산자만 표기하더라도 알아서 연산하고 반환한다.
> 클로저의 연산자 함수 사용
```Swift
//연산자 함수를 클로저의 역할로 사용
    let reserved: [String] = names.sorted(by: >)
```
## 값 획득
    ● 클로저는 자신이 정의된 위치의 주변 문맥을 통해 상수나 변수를 획득할 수 있다.
    ● 주변에 정의한 상수나 변수가 더 이상 존재하지 않더라도 해당 상수나 변수의 값을 자신 내부에서 참조하거나 수정할 수 있다.
    ● 클로저가 `비동기 작업`에 많이 사용된다.
    ● 다른 함수의 영향도 전혀 받지 않는다.
    ● 각각 자신만의 참조를 미리 획득한다.

> makeIncrementer(forIncrement:)함수<br>
> incrementer()함수는 자신 주변에 있는 runningTotal과 amout 두 값을 획득<br>
> incrementer는 클로저로서 makeIncrementer 함수에 의해 반환

```Swift
    func makeIncrementer(forIncrement amount: Int) -> (() -> Int){ //함수 객체를 반환한다는 의미, 호출 할 때마다 Int 타입의 값을 반환
        var runningTotal = 0
        func incrementer() -> Int{
            runningTotal += amount
            return runningTotal
        }
        return incrementer
    }

    let incrementByTwo: (() -> Int) = makeIncrementer(forIncrement: 2)

    let first: Int = incrementByTwo() //2
    let second: Int = incrementByTwo() //4
    let third: Int = incrementByTwo() //6

    //각각의 incrementer의 동작
    let incrementByTwo2: (() -> Int) = makeIncrementer(forIncrement: 2)
    let incrementByTen: (() -> Int) = makeIncrementer(forIncrement: 10)

    let first2: Int = incrementByTwo2() //2
    let second2: Int = incrementByTwo2() //4
    let third2: Int = incrementByTwo2() //6

    let ten: Int = incrementByTen() //10
    let twenty: Int = incrementByTen() //20
    let thirty: Int = incrementByTen() //30

    // 각각의 incrementer 함수는 다른 함수의 영향을 전혀 받지 않는다. 자신만의 runningTotal의 참조를 미리 획득했기 때문이다.
```

## 클로저는 참조 타입
    ● 함수와 클로저는 참조 타입이다
    ● 함수나 클로저를 상수나 변수에 할당할 때마다 상수나 변수에 함수나 클로저의 참조를 설정하는 것이다.
    ● 값을 할당하는 것이 아니라 해당 클로저의 참조를 할당하는 것이다.
    ● 클로저의 참조를 다른 상수에 할당해준다면 두 상수가 모두 같은 클로저를 가리킨다.

> 참조 타입인 클로저
```Swift
    let incrementByTwo: (() -> Int) = makeIncrement(forIncrement: 2)
    let sameWithIncrementByTwo: (() -> Int) = incrementByTwo

    let first: Int = incrementByTwo() //2
    let second: Int = sameWithIncrementByTwo() //4
    // 같은 클로저를 참조하기 때문에 동일한 클로저가 동작
```

## 탈출 클로저
    ● 함수의 전달인자로 전달한 클로저가 함수 종료 후에 호출될 때 클로저가 함수를 탈출한다.
    ● 클로저를 매개변수로 갖는 함수를 선언할 때 매개변수 이름의 콜론(:) 뒤에 @escaping 키워드를 사용하여 클로저가 탈출하는 것을 허용
    ● 비동기 작업으로 함수가 종료되고 난 후 호출할 필요가 있는 클로저를 사용해야 할 때 탈출 클로저가 필요하다.
    ● 비교 연산하기 위해 전달인자로 전달하는 클로저는 비탈출 클로저다.
    ● @escaping 키워드를 따로 명시하지 않는다면 매개변수로 사용되는 클로저는 기본적으로 비탈출 클로저이다.
    ● 함수 외부에 정의된 변수나 상수에 저장되어 함수가 종료된 후에 사용할 경우이다.
    ● @escaping 키워드를 사용하여 탈출 클로저임을 명시한 경우, 클로저 내부에 해당 타입에 접근하려면 self 키워드를 명시적으로 사용

> 탈출 클로저를 매개변수로 갖는 함수
```Swift
    var completionHandlers: [() -> Void] = []

    func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void){
        completionHandlers.append(completionHandler)
    }
```

> 함수를 탈출하는 클로저의 예
```Swift
    typealias VoidVoidClosure = () -> Void
    let firstClosure: VoidVoidClosure = {
        print("Closure A")
    }
    let secondClosure: VoidVoidClosure = {
        print("Closure B")
    }

    //first와 second 매개변수 클로저는 함수의 반환 값으로 사용될 수 있으므로 탈출 클로저이다
    func returnOneClosure(first: @escaping VoidVoidClosure, second: @escaping VoidVoidClosure, shouldReturnFirstClosure: Bool) -> VoidVoidClosure{
        return shouldReturnFirstClosure ? first : second
        //전달인자로 전달받은 클로저를 함수 외부로 다시 반환하기 때문에 함수를 탈출하는 클로저
    }

    let returnClosure: VoidVoidClosure = returnOneClosure(first: firstClosure, second: secondClosure, shouldReturnFirstClosure: true)
    // 함수에서 반환한 클로저가 함수 외부의 상수에 저장

    returnClosure() //Closure A

    var closures: [VoidVoidClosure] = []

    func appendClosure(closure: @escaping VoidVoidClosure){
        closures.append(closure)
        //전달인자로 전달받은 클로저가 함수 외부의 변수 내부에 저장되므로 함수를 탈출한다.

    // 탈출 클로저임을 명시하지 않으면 컴파일 오류가 발생한다.
    }
```

> 클래스 인스턴스 메서드에 사용되는 탈출, 비탈출 클로저
```Swift
    // @escaping 키워드를 사용하여 탈출 클로저임을 명시한 경우, 클로저 내부에서 해당 타입의 프로퍼티나, 메서드, 서브스크립트에 접근하려면 self 키워드를 명시적으로 사용

    typealias VoidVoidClosure = () -> Void

    func functionWithNoescapeClosure(closure: VoidVoidClosure){
        closure()
    }
    func functionWithEscapingClosure(competionHandler: @escaping VoidVoidClosure) -> VoidVoidClosure{
        return completionHandler
    }

    class SomeClass{
        var x = 10

        func runNoescapeClosure(){
            functionWtihNoescapeClosure { x = 200 }
        }

        func runEscapingClosure() -> VoidVoidClosure{
            return functionWithEscapingClosure { self.x = 100 }
        }
    }

    let instance: SomeClass = SomeClass()
    instance.runNoescapeClosure()
    print(instance.x) //200

    let returnedClosure: VoidVoidClosure = instance.runEscapingClosure()
    returnedClosure()
    print(instance.x) //100
```

## 자동 클로저
    ● 함수의 전달인자로 전달하는 표현을 자동으로 변환해주는 클로저
    ● 자동 클로저는 전달인자를 갖지 않는다.
    ● 어려운 클로저 문법을 사용하지 않고도 클로저로 사용할 수 있도록 문법적 편의를 제공한다.
    ● 자동 클로저는 클로저가 호출되기 전까지 클로저 내부의 코드가 동작하지 않는다.
    ● 연산을 지연시킬 수 있다. 그러므로 코드의 실행을 제어하기 좋다

> 클로저를 이용한 연산 지연
```Swift
    var customersInLine: [String] = ["Yoang","Sang","
    Sung","Ha"]
    print(customersInLine.count) //4

    // 클로저를 만들어두면 클로저 내부의 코드를 미리 실행하지 않고 가지고만 있는다.
    let customerProvider: () -> String = {
        return customersInLine.removeFirst()
    }

    print(customersInLine.count) //4

    print(customerProvider()) //Yoang
    print(customersInLine.count) //3
```
> 함수의 전달인자로 전달하는 클로저
```Swift
    var customersInLine: [String] = ["Yoang","Sang","
    Sung","Ha"]

    func serveCustomer(_ customerProvider: () -> String) {
        print(customerProvider)
    }

    serveCustomer({ customerInLine.removeFirst() })
```

> 자동 클로저의 사용
```Swift
    var customersInLine: [String] = ["Yoang","Sang","
    Sung","Ha"]

    func serveCustomer(_ customerProvider: @autoclosure () -> String) {
        print(customerProvider)
    }

    serveCustomer(customerInLine.removeFirst())

    /*
    1. 자동 클로저 속성을 부여한 매개변수는 클로저 대신에 customerInLine.removeFirst() 코드의 실행 결과인 String 타입의 문자열을 전달인자로 받는다
    2. 매개변수가 없는 String 값을 반환하는 클로저로 변환
    */
    // 자동 클로저는 전달인자를 갖지 않기 때문에 반환 타입의 값이 자동 클로저의 매개변수로 전달되면 이를 클로저로 바꿔줄 수 있다.
```

> 자동 클로저의 탈출
```Swift
    var cumtomerInLine: [String] = ["minjae", "innoceive", "sopress"]

    func returnProvider(_ customerProvider: @autoclosure @escaping () -> String) -> (() -> String){
        return customerProvider
    }

    let customerProvider: () -> String = returnProvider(customerInLine.removeFirst())
    print(customerProvider) //minjae
```
---
야곰 지음 - 스위프트 프로그래밍3판을 공부하고 작성하였음
---