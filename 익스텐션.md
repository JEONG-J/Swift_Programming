
# my Swift Study - Chapter21[익스텐션]
* 구조체, 클래스, 열거형, 프로토콜 타입에 새로운 기능을 추가할 수 있다.
* 타입에 새로운 기능을 추가할 수 있지만, 기존에 존재하는 기능을 재정의할 수 없다.
* 외부에서 가져온 타입에 내가 원하는 기능을 추가하고자 할 때 익스텐션 사용

## 익스텐션 문법

```Swift
    extension 확장할 타입 이름{
        // 타입에 추가될 새로윤 기능 구현

    extension 확장할 타입 이름: 프로토콜 1, 프로토콜 2, 프로토콜 3{
        // 프롵토콜 요규사항 구현
    }
``` 



## 익스텐션으로 추가할 수 있는 기능

> <b> 연산 프로퍼티
```Swift
    extension Int {
        var isEven: Bool{
            return self % 2 == 0
        }

        var isOdd: Bool{
            return self % 2 == 1
        }
    }

    print(1.isEven) // false
    print(3.isOdd) // true


    /*
        연산 프로퍼티를 추가할 수 있지만, 저장 프로퍼티는 추가할 수 없다. 또 타입에 정의되어 있는 기존의 프로퍼티에 프로퍼티 감시자를 추가할 수 없다.
    */
```

> <b> 메서드
```SWift
    extension Int{
        func multiply(by n: Int) -> Int {
            return self * n
        }

        mutating func multiplySelf(by n: Int){
            self = self.multiply(by: n)
        }

        static func isIntTypeInstance(_ instance: Any) -> Bool {
            return instance is Int
        }
    }    

    print(3.multiply(by: 2)) // 6
    print(4.multiply(by: 5)) // 0

    var number: Int = 3

    number.multiplySelf(by: 2)
    print(number) // 6

    number.multiplySelf(by: 3)
    print(number) // 18

    Int.isIntTypeInstance(number) // true
    Int.isIntTypeInstance(3) // true
    Int.isIntTypeInstance(1.2) // false
```

## 이니셜라이저

    ● 익스텐션으로 클래스 타입에 편의 이니셜라이저는 추가할 수 있지만, 지정 이니셜라이저는 추가할 수 없다.


> <b> 익스텐션 이니셜라이저 추가
```Swift
    
    extension String{
        init(intTypeNumber: Int){
            self = "\(intTypeNumber)"
        }

        init(doubleTypeNumber: Double){
            self = "\(doubleTypeNumber)"
        }
    }

    let strigFromInt: String = String(intTypeNumber: 100) // "100"
    let stringFromDouble: String = String(doubleTypeNumber: 100.0)

    class Person{
    var name: String

    init(name: String){
        self.name = name
    }
    }

    extension person{
        convenience init(){
            self.init(name: "Unknown")
        }
    }

    let someOne: Person = Person()
    print(someOne.name) // "unknwon"
```

```Swift
    // 이니셜라이저 호출이 종료되는 시점까지 인스턴스가 정상적으로 완벽하게 초기화되는 것을 책임져야 한다.

    struct Size{
        var width: Double = 0.0
        var height: Double = 0.0
    }

    struct Point{
        var x: Double = 0.0
        var y: Double = 0.0
    }

    struct Rect{
        var origin: Point = Point()
        var size: Size = Size()
    }

    let defaultRect: Rect = Rect()
    let memberwiseRect: Rect = Rect(origin: Point(x: 2.0, y: 2.0), size(width: 5.0, height: 5.0))

    extension Rect{
        init(center: Point, size: Size){
            let originX: Double = center.x - (size.width / 2)
            let originY: Double = center.y - (size.height / 2)
            self.init(origin: Point(x: originX, y: originY), size: size)
        }
    }

    let centerRect: Rect = Rect(center: Point(x: 4.0, y: 4.0), size: Size(width: 3.0, height: 3.0))
```

> <b> 서브스크립트
```Swift
    extension String{
        subscript(appendValue: String) -> String{
            return self + appendValue
        }

        subscript(repeatCount: UInt) -> String{
            var str: String = ""

            for _ in 0..<repeatCount {
                str += self
            }

            return str
        }
    }

    print("abc"["def"]) //"abcdef"
    print("abc"[3]) //"abcabcabc
```







    
