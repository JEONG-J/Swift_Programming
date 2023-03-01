
# my Swift Study - Chapter22[제네릭]
* 제네릭을 이용해 코드는 구현하면 어떤 타입에도 유연하게 대응할 수 있다.
* 재사용하기도 쉽고, 코드의 중복을 줄일 수 있다.

## 익스텐션 문법

```Swift
    제네릭을 사용하고자 하는 타입 이름 <타입 매개변수>
    제네릭을 사용하고자 하는 함수 이름 <타입 매개변수> (함수의 매개 변수)
``` 

> <b> 전위 연산자 구현과 사용
```Swift
    prefix func ** (value: Int) -> Int {
        return value * value
    }

    let minusFiive: Int = -5
    let sqrtMinusFive: Int = **minusFive

    print(sqrtMinusFive) // 25
```
> <b> 프로토콜과 제네릭을 이용한 전위 연산자 구현과 사용
```Swift
    prefix operator **

    prefix func ** <T: BinaryInteger>(value: T) -> T{
        return value * value
    }

    let minusFive: Int = -5
    let five: UInt = 5

    let sqrtMinusFive: Int = **minustFive
    let sqrtFive: UINt = **five

    print(sqrtMinusFive) // 25
    print(sqrtFive) // 25
```


> <b> 제네릭 사용 x,SwapTwoInts
```SWift
    func swapTwoInts(_ a: inout Int, _ b: inout Int){
        let temporaryA: Int = a
        a = b
        b = temporaryA
    }

    var numberOne: Int = -5
    var numberTwo: Int = 10

    swapTwoInts(&numberOne, &numberTwo)

    // Int 타입이 아닌 Double이나 String 타입의 변숫값을 서로 교환하고 싶을 경우 다시 구현

    func swapTwoInts(_ a: inout Int, _ b: inout Int){
        let temporaryA: Int = a
        a = b
        b = temporaryA
    }

    var stringOne: String = "A"
    var stringTwo: String = "B"

    swapTwoInts(&numberOne, &numberTwo)


    /*
        ::제네릭::
        같은 타입인 두 변수의 값을 교환한다는 목적을 타입에 상관없이 할 수 있도록 단 하나의 함수로 구현한다.
    */

    func swapTwoValues<T>(_ a: inout T, _ b: inout T){
        let temporaryA: T = a
        a = b
        b = temporaryA
    }

    swapTwoValues(&numberOne,&numberTwo)
    print("\(numberOne), \(numberTwo)") // 10, 5

    swapTwoValues(&stringOne,&stringTwo)
    print("\(stringOne), \(stringTwo)") // "B","A"

    /*
        제네릭 함수는 실제 타입이름을 써주는 대신에 플레이스홀더(함수 T)를 사용한다.
        플레이스홀더 타입(T)는 타입의 종류를 알려주지 않지만 말 그대로 어떤 타입이란느 것은 알려준다.
        T의 실제 타입은 함수가 호출되는 그 순간 결정된다.
        함수 이름 오른쪽의 홀화살괄호 기호 <> 안쪽에 플레이스홀더 이름들을 나열한다.
        여러 개의 타입 매개변수를 갖고 싶다면 <T, U, V>
    */
```

## 제네릭 타입

> <b> 제네릭을 사용하지 않는 IntStack 구조체 타입
```Swift
    struct IntStack{
        var items = [Int]()
        mutating func push(_ item:Int){
            items.append(item)
        }
        mutating func pop() -> Int{
            return items.removeLast()
        }
    }

    var integerStack: IntStack = IntStack()

    integerStack.push(3)
    print(integerstack.items) // [3]

    integerStack.push(2)
    print(integerStack.items) // [3,2]

    integerStack.pop()
    print(integerStack.items) // [3]

    integerStack.pop()
    print(integerStack.items) // []
    
    /* 모든 타입을 대상으로 동작하는 스택 */
    struct Stack<Element> {
        var items = [Element]()
        mutating func push(_ items: Element){
            items.append(item)
        }
        mutating func pop() -> Element {
            return items.removeLast()
        }
    }

    var doubleStack: Stack<Double> = Stack<Double>()

    doubleStack.push(1.0)
    doubleStack.push(2.0)

    print(doubleStack) // [1.0, 2.0]

    var stringStack: Stack<String> = Stack<String>()

    stringStack.push("A")
    stringStack.push("B")

    print(stringStack) // ["A", "B"]


    var anyStack: Stack<Any> = Stack<Any>()

    anyStack.push(1)
    anyStack.push(1.2)
    anyStack.push("A")

    print(anyStack) // [1, 1.2, "A"]
    
```

## 제네릭 타입 확장

    ● 제네릭을 사용하는 타입에 기능을 추가하고자 한다면 익스텐션 정의에 타입 매개변수를 명시하지 않아야 한다.
    ● 원래의 제네릭 정의에 명시한 타입 매개변수를 익스텐션에 사용할 수 있다.
```Swift
    extension Stack{
        var topElement: Element? {
            return self.items.last
        }
    }

    prinnt(doubleStack.topElement) // Optional(1.0)
   
```

## 타입 제약

    ● 타입 제약은 클래스 타입 또는 프로토콜로만 줄 수 있다. 열거형, 구조체 등의 타입은 타입 제약의 타입으로 사용할 수 없다.
    ● 타입 매개변수 뒤에 콜론을 붙인후 원하는 클래스 타입 또는 프로토콜을 명시하면 된다.
    ● Hashable -> Int, String, Bool 등등 스위프트 기본 타입을 준수하는 프로토콜


> <b> 제네릭 타입 제약
```Swift
    func swapTwoValues<T: BinaryInteger>(_a: inout T, _b: inout T){
        // 함수 구현
    }

    struct Stack<Element: Hashable>{
        // 구조체 구현ㄴ
    }

    //제네릭 타입 제약 추가
    //여러 제약을 추가하고 싶다면 콤마로 구분해주는 것이 아니라 where 절을 사용한다.
    //T는 BinaryInteger 프로토콜을 준수하고, FloatingPoint 프로토콜도 준수하는 타입만 사용
    func swapTwoValues<T: BinaryInteger>(_ a: inout T, _ b: inout T) where T:
    FloatingPoint{
        // 함수 구현
    }
```

## 프로토콜이 연관 타입
    ● 프로토콜 정의할 떄 연관 타입을 함께 정의하면 유용할 떄가 있다.
    ● 연관 타입은 프로토콜에서 사용할 수 있는 플레이스홀더 이름
    ● "종류는 알 수 없지마느 어떤 타입이 여기에 쓰일 것이다."라고 표현 -> 타입 매개변수의 그 역할을 프로토콜에서 수행할 수 있도록 만들어진 기능

```Swift
    potocol Container{
        associatedtype ItemType
        var count: Int { get }
        mutating func append(_ item: ItemType)
        subscript(i: Int) -> ItemType { get }
    }

    /*
        ItemType을 연관 타입으로 정의하여 프로토콜 정의에서 타입 이름으로 활용한다.
        제네릭의 타입 매개변수와 유사한 기능으로 프로토콜 정의 내부에서 사용할 타입이 "그 어떤 것이라도 상관없지만, 하나의 타입임은 분명하다"라는 의미이다.
    */

    class MyContainer: Container {
        var items: Array<Int> = Array<Int>()

        var count: Int{
            return items.count
        }

        func append(_ item: Int){
            items.append(item)
        }

        subscript(i: Int) -> Int{
            return items[i]
        }
    }
    
    /*
        ItemType이라는 연관 타입만 정의했을뿐, 특정 타입을 지정하지 않았기 때문에 실제 타입인 Int 타입으로 구현함
    */


    // IntStack 구조체의 Contaiter 프로토콜 준수

    struct IntStack: Container {
        // 기존 IntStack 구조체 구현
        var items = [Int]()
        mutating func push(_ item: Int){
            items.append(item)
        }
        mutating func pop() -> Int{
            return items.removeLast()
        }

        // Container 프로토콜 준수를 위한 구현
        mutating func append(_ item: Int){
            self.push(item)
        }

        var count: Int{
            return items.count
        }
        subscript(i: Int) -> Int{
            return items[i]
        }
    }

    /*
        ItemType을 어떤 타입으로 사용할지 조금 더 명확하게 해주고 싶다면 IntStack 구조체 구현부에 typealias ItemType = Int 타입 별칭을 지정한다.
    */

    struct IntStack: Container {

        typealias ItemType = Int
        // 기존 IntStack 구조체 구현
        var items = [ItemType]()
        mutating func push(_ item: ItemType){
            items.append(item)
        }
        mutating func pop() -> ItemType{
            return items.removeLast()
        }

        // Container 프로토콜 준수를 위한 구현
        mutating func append(_ item: ItemType){
            self.push(item)
        }

        var count: ItemType{
            return items.count
        }
        subscript(i: Int) -> ItemType{
            return items[i]
        }
    }


    // 제네릭 타입에서는 연관 타입과 타입 매개변수를 대응시킬 수 있다.

    struct Stack<Element>: Container{
        var items = [Element]()
        mutating func push(_ item: Element){
            items.append(item)
        }
        mutating func pop() -> Element {
            return items.removeLast()
        }

        mutating func append(_ item: Element){
            self.push(item)
        }
        var count: Int{
            return items.count
        }
        subscript(i: Int) -> Element{
            return items[i]
        }
    }
```

## 제네릭 서브스크립트

```Swift
    extension Stack{
        subscript<Indices: Sequence>(indices: Indices) -> [Element] where Indices.Iterator.Element == Int {
            var result = [ItemType]()
            for index in indices {
                result.append(self[index])
            }
            return result
        }
    }

    var integerStack: Stack<Int> = Stack<Int>()
    print(integerStack[0...2]) // [1,2,3]

    /*
        Indices라는 플레이스홀더를 사용하여 매개변수를 제네릭하게 받아들일 수 있다.
        Indices는 Sequence 프로토콜을 준수하는 타입으로 제약이 추가되어 있다.
        Indices 타입 Iterator Element 타입이 Int 타입이어야 하는 제약이 추가된다.
        Indices 타입의 indices라는 매개변수로 인덱스 값을 받을 수 있다.
        indices 시퀸스의 인덱스 값에 해당하는 스택 요소의 값을 배열로 반환
    */
```





    
