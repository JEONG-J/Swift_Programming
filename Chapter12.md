
# my Swift Study - Chapter12[접근제어]
    ※ 객체지향 프로그래밍 패러다임에서 은닉화는 중요한 개념 중 하나이다.
## 접근제어란?
    ● 접근제어를 통해 코드의 상세 구현은 숨기고 허용된 기능만 사용하는 인터페이스를 제공할 수 있다.
    ● 꼭 필요한 부분만 제공을 해야하는데 전체 코드가 노출될 가능성이 있을 때 접근제어를 이용한다.

## 접근수준
    ● 각 타입에 특정 접근수준을 지정할 수 있다.
    ● open, public, internal, fileprivate, private가 있다.

## 공개 접근 수준 - public
    ● 주로 프레임워크에서 외부와 연결될 인터페이스를 구현하는데 많이 쓰인다.

## 개방 접근 수준 - open
    ● 공개 접근 수준 이상으로 높은 접근수준이며, 클래스와 클래스의 멤버에서만 사용할 수 있다.
    ● 모듈 밖의 다른 모듈에서도 상속할 수 있다.
    ● 모듈 밖의 다른 모듈에서도 재정의 할 수 있다.

## 내부 접근수준 - internal
    ● 기본적으로 모든 요소에 암묵적으로 지정하는 기본 접근수준
    ● 소스파일이 속해 있는 모듈 어디에서든 쓰일 수 있다

## 파일외부공개 접근 수준 - fileprivate
    ● 요소가 구현된 소스파일 내부에서만 사용할 수 있다.
    ● 해당 소스파일 외부에서 값이 변경되거나 함수를 호출하면 부작용이 생길 수 있다

## 비공개 접근수준 - private
    ● 가장 한정적인 범위.
    ● 비공개 접근수준으로 지정된 요소는 그 기능을 정의하고 구현한 범위 내에서만 사용할 수 있다.

## 접근제어 구현 참고사항
    ● 상위 요소보다 하위 요소가 더 높은 접근수준을 가질 수 없다.
    ● 함수의 매개변수로 특정 접근수준이 부여된 타입이 전달되거나 반환된다면, 그 타입의 접근수준보다 함수의 접근수준이 높게 설정될 수 없다.
    ● 함수뿐만 아니라 튜플의 내부 요소 타입 또한 튜플의 접근수준보다 같거나 높아야 한다.
    ● case의 접근수준은 열거형 자체의 접근수준을 따른다.
    ● 열거형의 원시 값 타입으로 열거형의 접근수준보다 낮은 접근수준의 타입이 올 수 없다.

> 튜플의 접근수준 부여

```Swift
    internal class InternalClass {} // 내부 접근수준 클래스
    private struct PrivateStruct {} // 비공개 접근수준 구조체

    public var publicTuple: (first: InternalClass, second: PrivateStruct) = (InternalClass(), PrivateStruct())
    //요소로 사용된 InternalClass, PrivateStruct가 publicTuple보다 접근수준이 낮기 때문에 사용 할 수 없다.

    private var privateTuple: (first: InternalClass, second:  PrivateStruct) = (InternalClass(), privateStruct())
    //요소로 사용된 InternalClass, PrivateStruct가 publicTuple보다 접근수준이 같거나 높기 때문에 사용할 수 있다.
```

> 접근수준에 따른 접근 결과
```Swift
    class AClass{
        func internalMethod(){}
        fileprivate func filePrivateMethod(){}
        var internalProperty = 0
        fileprivate var filePrivateProperty = 0
    }

    let aInstance: AClass = AClass()
    aInstance.internalMethod() // 같은 모듈이므로 호출 가능
    aInstance.filePrivateMethod() //다른 파일이므로 호출 불가
    aInstance.internalProperty = 1 //같은 모듈이므로 접근 가능
    aInstance.filePrivateProperty = 1 //다른 파일이므로 접근 불가

    //다른 모듈에서 특정 기능에 접근할 수 있도록 api로 사용할 기능을 공개 접근 수준으로 지정해주어야 한다.
```

## private와 fileprivate
    ● fileprivate : 같은 파일 어떤 코드에서도 접근할 수 있다
    ● private : 같은 파일 내부에 다른 타입의 코드가 있더라도 접근이 불가능하다

> 같은 파일에서의 private와 fileprivate 동작
```Swift
    public struct SomeType{
        private var privateVariable = 0
        fileprivate var fileprivateVariable = 0
    }

    extension SomeType{ // 같은 타입의 익스텐션에서는 private 요소에 접근 가능
        public func publicMethod(){
            print("\(self.privateVariable), \(self.fileprivateVariable)")
        }

        private func privateMethod(){
            print("\(self.privateVariable), \(self.fileprivateVariable)")
        }

         fileprivate func privateMethod(){
            print("\(self.privateVariable), \(self.fileprivateVariable)")
        }
    }

    struct AnotherType{
        var someInstance: SomeType = SomeType()

        mutating func someMethod(){
            self.someInstance.publicMethod() // 0, 0
            //public 접근수준에서는 어디서든 접근 가능

            self.someInstance.fileprivateVariable = 100
            self.someInstance.fileprivateMethod() // 0, 100
            // 같은 파일에 있으므로 접근 가능

            self.someInstance.privateVariable = 100
            self.someInstance.privateMethod()
            // 다른 타입 내부의 코드이므로 private 요소에 접근 불가
        }
    }
```

## 읽기 전용 구현
    ● 프로퍼티 값을 변경할 수 없도록 구현하고 싶다면 설정자만 더 낮은 접근수준을 갖도록 제한한다.
    ● 접근수준(set) 표현하면 설정자의 접근수준만 더 낮도록 지정할 수 있다.

> 설정자의 접근수준 지정
```Swift
    public struct SomeType{
        private var count: Int = 0

    // 공개 접근수준 저장 프로퍼티
    public var publicStoredProperty: Int = 0

    // 설정자는 비공개 접근 수준
    public private(set) var publicGetOnlyStoredProperty: Int = 0

    // 내부 접근수준 저장 프로퍼티
    internal var internalComputedProperty: Int{
        get{
            return count
        }
        set{
            count += 1
        }
    }

    // 내부 접근 수준 프로퍼티
    // 설정자는 비공개 접근수준
    internal private(set) var internalGetOnlyComputedProperty: Int{ //설정자는 비공개 접근 수준
        get{
            return count
        }
        set{
            count += 1
        }
    }

    public subscript() -> Int{ //공개 접근수준 서브스크립트
        get{
            return count
        }
        set{
            count += 1
        }
    }
    
    public internal(set) subscript(some: Int) -> Int{
        get{
            return count
        }
        set{
            count += 1
        }
    }
}

var someInstance: SomeType = SomeType()

print(someInstance.publicStoredProperty) //0
someInstance.publicStoredProperty = 100

print(someInstance.publicGetOnlyStoredProperty) //0
someInstance.publicGetOnlyStoredProperty = 100 //오류 발생

print(someInstance.internalComputedProperty) //0
someInstance.internalComputedProperty = 100

print(someInstance.internalGetOnlyComputedProperty) //1
someInstance.internalGetOnlyComputedProperty = 100 //오류 발생

print(someInstance[]) // 1
someInstance[] = 100 //외부에서 접근자 설정자 모두 사용 가능

print(someInstance[0]) //2
someInstance[0] = 100 //외부에서 접근자만, 같은 모듈 내에서는 설정자도 사용 가능
```
---
야곰 지음 - 스위프트 프로그래밍3판을 공부하고 작성하였음
---