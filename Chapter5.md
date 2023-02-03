
# my Swift Study - Chapter5[연산자]
    ※ Swift는 데이터 타입에 굉장히 엄격하므로 서로 다른 자료형끼리의 연산을 엄격히 제한한다.
## 참조 비교 연산자
    ● 값의 비교 연산자에는 ==를 사용하고 클래스의 인스턴스인 경우에만 ===를 사용
```Swift
    let valueA: Int = 3
    let valueB: Int = 5
    let ValueC: Int = 5

    let isSameValueAB: Bool = valueA == valueB // false
    let isSameValueBC: Bool = valueB == valueC // true

    let referenceA: SomeClass = SomeClass()
    let referenceB: SomeClass = SomeClass()
    let referenceC: SomeClass = referenceA

    let isSameReferenceAB: Bool = referenceA === referenceB // false
    let isSameReferenceAC: Bool = referenceA === referenceC //true
```

## 범위 연산자
|연산자|부호|설명|
|:------:|:---:|:---:|
|폐쇄범위 연산자|A... B|A부터 B까지|
|반폐쇄 범위 연산자|A..< B|A부터 B미만까지|
|단방향 범위 연산자|A...|A이상|
||... A|A이하|
||..< A|A미만|

## 오버플로 연산자
|연산자|부호|설명|
|:------:|:---:|:---:|
|오버플로 더하기 연산|&+|오버플로에 대비한 덧셈 연산|
|오버플로 빼기 연산|&-|오버플로에 대비한 뺄셈 연산|
|오버플로 곱하기 연산|&*|오버플로에 대비한 곱셈 연산|

```Swift
    var unsignedInteger: UInt8 = 0
    let errorUnderflowResult: UInt8 = unsignedInteger - 1 // error
    let underflowedValue: UInt8 = unsignedInteger &- 1 // 255

    unsignedInteger = UInt8.max
    let errorOverflowResult: UInt8 = unsignedInteger + 1 // error
    let overflowedValue: UInt8 = unsignedInteger &+ 1 // 0
```

## 기타 연산자
|연산자|부호|설명|
|:------:|:---:|:---:|
|nil 병합 연산자|A ?? B|A가 nil이 아니면 A를 반환하고, A가 nil이면 B를 반환한다|
|부호변경 연산자|-A|A의 부호 변경|
|옵셔널 강제 추출 연산자|O!|O의 값을 강제|
|옵셔널 연산자|V?|V를 안전하게 추출, V가 옵셔널임을 표현|

## 전위 연산자 정의와 구현
    ● 전위 연산자를 만들고 싶다면 연산자 정의를 먼저 해주어야 한다.
    ● prefix operator 선언

```Swift
//전위 연산자 구현과 사용
    prefix operator **

    prefix func ** (value: Int) -> Int{
        return value * value
    }
    let number: Int = 5
    let result: Int **number

    print(result) // 25
```

```Swift
// 전위 연산자 기능 추가
    prefix func ! (value: String) -> Bool{
        return value.isEmpty
    }

    var stringValue: String = "hi"
    var isEmptyString: Bool = !stringValue

    print(isEmptyString) // false

    stringValue = ""
    isEmptyString = !stringValue

    print(isEmptyString) // true
```

```Swift
// 전위 연산자 중복 사용
    prefix operator **

    prefix func ** (value: Int) -> Int{
        return value * value
    }
    
    prefix func ** (value: String) -> String{
        retrurn value + " " + value
    }

    let result: String = **"hi"
    print(result) // hi hi
```
## 후위 연산자 정의와 구현
    ● 후위 연산자를 만들고 싶다면 연산자 정의를 먼저 해주어야 한다.
    ● postfix operator 선언
    ● 전위 연산과 후위 연산을 한 줄에 사용하면 후위 연산을 먼저 수행
```Swift
//후위 연산자 정의와 구현
    postfix operator **
     
    postfix func ** (value: Int) -> Int{
        return value + 10
    }

    let number: Int = 5
    let result: Int = number**

    print(number) // 15
```

```Swift
// 전위연산자와 후위 연산자 동시 사용
    prefix operator **
    postfix operator **

    prefix func ** (value: Int) -> Int{
        return value * value
    }

    postfix func ** (value: Int) -> Int{
        return value + 10
    }

    let number: Int = 5
    let result: Int = **number**

    print(number) // 15 * 15 = 255
```

## 중위 연사자의 정의
    ● infix operator 선언
```Swift
    import Foundation

    infix operator ** : MultiplicationPrecedence

    func ** (lhs: String, rhs: String) -> Bool{
        return lhs.contains(rhs)
    }
    let hello: String = "hello"
    let hi: String = "hi"
    let result: String = hello**hi // false
```

## 클래스 및 구조체 비교 연산자 구현
```Swift
    class Car{
        var modelYear: Int?
        var modelName: String?
    }

    struct SamrtPhone{
        var company: String?
        var model: String?
    }

    func == (lhs: Car, rhs: Car) -> Bool{
        return lhs.modelName == rhs.modelName
    }

    func == (lhs: SmartPhone, rhs: SmartPhone) -> Bool {
        return lhs.model == rhs.model
    }

    let myCar = Car()
    myCar = "KIA"

    let yourCar = Car()
    yourCar = "KIA"

    var myPhone = SmartPhone()
    myPhone = "apple"

    var yourPhone = SmartPhone()
    yourPhone = "samsung"

    print(myCar == yourCar) // true
    print(myPhone == yourPhone) // false
```

```Swift
// 타입 메서드로 구현
    class Car{
        var modelYear: Int?
        var modelName: String?

        static func == (lhs: Car, rhs: Car) -> Bool{
            return lhs.modelName == rhs.modelName
      }
    }

    struct SamrtPhone{
        var company: String?
        var model: String?
        
        static func == (lhs: SmartPhone, rhs: SmartPhone) -> Bool {
            return lhs.model == rhs.model
        }
    }

    let myCar = Car()
    myCar = "KIA"

    let yourCar = Car()
    yourCar = "KIA"

    var myPhone = SmartPhone()
    myPhone = "apple"

    var yourPhone = SmartPhone()
    yourPhone = "samsung"

    print(myCar == yourCar) // true
    print(myPhone == yourPhone) // false

```
---
야곰 지음 - 스위프트 프로그래밍3판을 공부하고 작성하였음
---
