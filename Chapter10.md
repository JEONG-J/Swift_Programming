
# my Swift Study - Chapter10[프로퍼티와 메서드]
* 프로퍼티 => 클래스, 구조체 또는 열거형 등에 관련된 값을 뜻한다.
* 메서드 => 특정 타입에 관련된 함수를 뜻한다.

## 프로퍼티
     ● 저장 프로퍼티 : 인스턴스의 변수 또는 상수, 구조체와 클래스에서만 사용
     ● 연산 프로퍼티 : 특정 연산을 실행한 결괏값, 클래스, 구조체, 열거형에 쓰인다.
     ● 타입 프로퍼티 : 특정 타입에 사용되는 프로퍼티
     ● 프로퍼티 감시자 : 프로퍼티의 값이 변하는 것을 감시, 프로퍼티의 값이 변할 때 값의 변화에 따른 특정 작업 실행

## 저장 프로퍼티
     ● 구조체는 저장 프로퍼티를 모두 포함하는 이니셜라이저를 자동으로 생성
     ● 클래스의 저장 프로퍼티는 옵셔널이 아니라면 프로퍼티 기본값을 지정 또는 사용자 정의 이니셜라이저를 통해 반드시 초기화!!
     ● 이니셜라이저에서 옵셔널 프로퍼티에 꼭 값을 할당해주지 않아도 된다.

>저장 프로퍼티의 선언 및 인스턴스 생성
```Swift
    struct CoordinatePoint{
        var x: Int //저장 프로퍼티
        var y: Int //저장 프로퍼티
    }

    let point: CoordinatePoint = CoordinatePoint(x: 10, y: 5)

    class Position{
        var point: CoordinatePoint
        let name: String //저장 프로퍼티

        init(name: String, currentPoint: CoordinatePoint){
            self.name = name
            self.point = currentPoint
        }
    }

    let position: Position = Position(name: "hello", currentPoint: point) //사용자 정의 이니셜라이저 호출!!
```
>저장 프로퍼티의 초깃값 지정
```Swift
    struct CoordinatePoint{
        var x: Int = 0
        var y: Int = 0
    }

    let point: CoordinatePoint = CoordinatePoint()
    let twoPoint: CoordinatePoint = CoordinatePoint(x: 10, y: 4)

    class Position{
        var point: CoordinatePoint = CoordinatePoint()
        var name: String = "Unknown"
    }

    let position: Position = Position()

    position.point = twoPoint
    position.name = "hello"
```
>옵셔널 저장 프로퍼티
```Swift
    struct CoordinatePoint{
        var x: Int
        var y: Int
    }

    class Position{
        var point: CoordinatePoint?
        let name: String

        init(name: String){
            self.name = name
        }
    }

    let position: Position = Position(name: "Hello")
    //이름은 필수지만 위치는 모를 수 있다.

    position.point = CoordinatePoint(x: 10, y: 2)
    //위치 값 넣을 때마다 넣어주면 된다.
```

## 지연 저장 프로퍼티
    ● 호출이 있어야 값을 초기화 할 수 있다.
    ● lazy 키워드를 사용한다.
    ● var 키워드를 사용하여 변수로 정의한다.
    ● 복잡한 클래스나 구조체 구현할 때 많이 사용한다.

> 지연 저장 프로퍼티
```Swift
    struct CoordinatePoint{
        var x: Int = 0
        var y: Int = 0
    }

    class Position{
        lazy var point: CoordinatePoint = CoordinatePoint()
        let name: String

        init(name: String){
            self.name = name
        }
    }

    let position: Position = Position(name: "hello")
    print(position.point) //point 프로퍼티 처음 접근함으로 point 프로퍼티의 CoordinatePoint가 생성
```

## 연산 프로퍼티
    ● 특정 상태에 따른 값을 연산하는 프로퍼티
    ● 접근자 : 내/외부의 값을 연산하여 적절한 값을 돌려준다
    ● 설정자 : 은닉화된 내부의 프로퍼티 값을 간접적으로 설정
    ● set 메서드 내부에서 newValue 매개변수 이름 사용가능. 단, 괄호 안에 매개변수를 따로 표기하지 말아야 한다.
    ● 연산 프로퍼티를 읽기 전용으로 하려면 get 메서드만 사용
    ● 인스턴스 내부 값에 접근하기 위해 사용한다. 또한 프로퍼티가 메서드 형식보다 훨씬 더 간편하고 직관적이기 때문이다.

> 연산 프로퍼티의 정의와 사용
```Swift
    struct CoordinatePoint{
        var x: Int //저장 프로퍼티
        var y: Int //저장 프로퍼티
    }

    var oppositePoint: CoordinatePoint{
        get{
            return CoordinatePoint(x: -x, y: -y)
        }
        set(opposite){
            x = -opposite.x
            y = -opposite.y
        }
    }

    var position: CoordinatePoint = CoordinatePoint(x: 10, y: 2)
    print(position) //10,2
    print(position.oppositePoint) // -10, -2
    position.oppositePoint = CoordinatePoint(x: 15, y: 10)

    print(position) // -15, -10
```

> 매개변수 이름 생략한 설정자
```Swift
    struct CoordinatePoint{
        var x: Int
        var y: Int
    }

    var oppositePoint: CoordinatePoint{
        get{
            return CoordinatePoint(x: -x, y: -y)
        }
        set{
            x = -newValue.x
            y = -newValue.y
        }
    }
```
> 읽기 전용 연산 프로퍼티
```Swift
    struct CoordinatePoint{
        var x: Int
        var y: Int

        var oppositePoint: CoordinatePoint{
            get{
                return CoordinatePoint(x: -x, y: -y)
            }
        }
    }

    var position: CoordinatePoint = CoordinatePoint(x:10, y:2)

    print(position) //10, 2
    print(position.oppositePoint) //-10, -2
    position.oppositePoint = CoordinatePoint(x:12, y:1) //오류 발생
```
## 프로퍼티 감시자
    ● 프로퍼티의 값이 변경됨에 따라 적절한 작업을 취할 수 있다.
    ● 프로퍼티의 값이 새로 할당될 때마다 호출한다.
    ● 변경되는 값이 현재의 값과 같더라도 호출한다.
    ● 연산 프로퍼티는 상속받았을 때만 프로퍼티 재정의를 통해 프로퍼티 감시자 사용
    ● willSet : 프로퍼티의 값이 변경되기 직전에 호출하는 메서드 => 전달인자: 변경될 값(newValue)
    ● didSet : 변경된 직후에 호출하는 메서드 => 전달인자: 변경되기 직전의 값(oldValue)

>프로퍼티 감시자
```Swift
    class Account{
        var credit: Int = 0{
            willSet{
                print("잔액이 \(credit)원에서 \(newValue)원으로 변경될 예정")
            }

            didSet{
                print("잔액이 \(oldValue)원에서 \(credit)원으로 변경됨")
            }
        }
    }

    let myAccount: Account = Account()//0원에서 1000원으로 변경될 예정
    myAccount.credit = 1000 //0원에서 1000원으로 변경됨
```

> 상속받은 연산 프로퍼티의 프로퍼티 감시자 구현
```Swift
    class Account{
        var credit: Int = 0{
             willSet{
                print("잔액이 \(credit)원에서 \(newValue)원으로 변경될 예정")
            }

            didSet{
                print("잔액이 \(oldValue)원에서 \(credit)원으로 변경됨")
            }
        }
        var dollarValue: Double{
            get{
                return Double(credit) / 1000.0
            }
            set{
                credit = Int(newValue * 1000)
                print("잔액을 \(newValue)달러로 변경 중입니다")
            }
        }
    }

    class ForeignAccount: Account{
        override var dollarValue: Double{
            willSet{
                print("잔액이 \(dollarValue)달러에서 \(newValue)달러로 변경될 예정")
            }

            didSet{
                print("잔액이 \(oldValue)달러에서 \(dollarValue)달러로 변경됨")
            }
        }
    }

    let myAccount: ForeignAccount = ForeignAccount()
    //잔액이 0원에서 1000원으로 변경될 예정
    myAccount.credit = 1000
    //잔액이 0원에서 1000원으로 변경

    //잔액이 1.0달러에서 2.0달러로 변경될 예정
    //잔액이 1000원에서 2000원으로 변경될 예정
    //잔액이 1000원에서 2000원으로 변경됨
    myAccount.dollarValue = 2

    //잔액을 2.0달러로 변경 중입니다.
    //잔액이 1.0달러에서 2.0달러로 변경됨
```

## 전역변수와 지역변수
    ● 연산 프로퍼티와 프로퍼티 감시자는 전역변수와 지역변수 모두에 사용 가능
    ● 전역에서 쓰일 수 있는 변수와 상수에도 두 기능을 사용할 수 있다.
    ● 저장 변수 : 전역변수 또는 지역변수
    ● 저장변수는 저장 프로퍼티처럼 값을 저장
    ● 저장 변수를 연산변수로 구현할 수 있고, 프로퍼티 감시자를 구현할 수 있다.
    ● 지연 저장 프로퍼티처럼 처음 접근할 때 최초로 연산이 이루어진다.

> 저장변수의 감시자와 연산변수
```Swift
    var wonInPocket: Int = 2000{
        willSet{
            print("돈이 \(wonInPocket)원에서 \(newValue)원으로 변경될 예정")
        }
        didSet{
            print("돈이 \(oldValue)원에서 \(wonInPocket)원으로 변경됨")
        }
    }

    var dollarInPocket: Double{
        get{
            return Double(wonInPocket) / 1000.0
        }
        set{
            wonInPocket = Int(newValue * 1000.0)
            print("주머니의 달러를 \(newValue)달러로 변경 중")
        }
    }

    dollarInPocket = 3.5
    //돈이 2000원에서 3500원으로 변경될 예정
    //돈이 2000원에서 3500원으로 변경됨
    //주머니의 달러를 3.5달러로 변경 중
```

## 타입 프로퍼티
    ● 인스턴스가 아닌 타입 자체에 속하는 프로퍼티
    ● 저장 타입 프로퍼티 : 변수 또는 상수로 선언할 수 있다
    ● 연산 타입 프로퍼티 : 변수로만 선언할 수 있다.
    ● lazy 키워드를 사용하지 않는다.

>타입 프로퍼티와 인스턴스 프로퍼티
```Swift
    class AClass{
        static var typeProperty: Int = 0 // 저장 타입 프로퍼티

        var instanceProperty: Int = 0{ //저장 인스턴스 프로퍼티
            didSet{
                Self.typeProperty = instanceProperty + 100
            }
        }
        static var typeComputedProperty: Int{ //연산 타입 프로퍼티
            get{
                return typeProperty
            }
            set{
                typeProperty = newValue
            }
        }
    }

    AClass.typeProperty = 123

    let classInstance: AClass = AClass()
    classInstance.instanceProperty = 100
    print(AClass.typeProperty) //200
    print(AClass.typeComputedProperty) //200
``` 

>타입 프로퍼티의 사용
```Swift
    class Account{
        static let dollarExchangeRate: Double = 1000.0 //타입 상수
        var credit: Int = 0 // 저장 인스턴스 프로퍼티
        var dollarValue: Double{ //연산 인스턴스 프로퍼티
            get{
                return Double(credit) / Self.dollarExchangeRate
            }
            set{
                credit = Int(newValue * Self.dollarExchangeRate)
                print("잔액을 \(newValue)달러로 변경")
            }
        }
    }
```

## 키 경로
    ● 프로퍼티의 위치만 참조하도록 할 수 있다.
    ● WriteableKeyPath<Root, Value> : 값 타입에 키 경로 타입으로 읽고 쓸 수 있다
    ● ReferenceWriteableKeyPath<Root, Value> : 클래스 타입에 키 경로 타입으로 읽고 쓸 수 있다.
    ● \타입이름.경로.경로.경로

> 키 경로 타입의 타입 확인
```Swift
    class Person{
        var name: String

        init(name: String){
            self.name = name
        }
    }

    struct Stuff{
        var name: String
        var owner: Person
    }

    print(type(of: \Person.name)) //ReferenceWriteablekeyPath, String>
    print(type(of: \Stuff.name)) //WriteableKeyPath<Stuff, String>

    let keypath = \Stuff.owner
    let nameKeyPath = keypath.appending(path: \.name)
```

>keyPath 서브스크립트와 키 경로 활용
```Swift
    class Person{
        let name: String
        init(name: String){
            self.name = name
        }
        struct Stuff{
            var name: String
            var owner: Person
        }

        let one = Person(name: "one")
        let two = Person(name: "two")
        let macbook = Stuff(name: "macbook", owner: one)
        var imac = Stuff(name: "imac", owner: one)
        let iphone = Stuff(name: "iphone", owner: iphone)

        let stuffNameKeyPath = \Stuff.name
        let ownerkeyPath = \Stuff.owner

        let ownerNameKeyPath = ownerkeyPath.appending(path: \.name)
        //키 경로와 서브스크립트를 이용해 프로퍼티에 접근 값 가져옴
        print(macbook[keyPath: stuffNameKeyPath]) //macbook
        print(imac[KeyPath: stuffNameKeyPath]) //imac
        print(iphone[KeyPath: stuffNameKeyPath]) //iphone

        print(macbook[KeyPath: ownerNameKeyPath]) //one
        print(imac[KeyPath: ownerNameKeyPath]) //one
        print(iphone[KeyPath: ownerNameKeyPath]) //two

        //키 경로와 서브스크립트를 이용해 프로퍼티에 접근하여 값 변경
        imac[keyPath: stuffNameKeyPath] = "imac pro"
        imac[keyPath: ownerkeyPath] = "two"
        
        print(imac[KeyPath: ownerNameKeyPath]) //two
        print(imac[KeyPath: stuffNameKeyPath]) //imac pro

    }
```

## 메서드
    ● 특정 타입에 관련된 함수
    ● 클래스, 구조체, 열거형 등은 실행하는 기능을 캡슐화한 인스턴스 메서드를 정의

## 인스턴스 메서드
    ● 특정 타입의 인스턴스에 속한 함ㄴ수
    ● 인스턴스 내부의 프로퍼티 값을 변경하거나 특정 연산 결과를 반환하는 등 인스턴스와 관련된 기능을 실행
    ● 구조체나 열거형 등은 값 타입이므로 메서드 앞에 mutating 키워드를 붙여 해당 메서드가 인스턴스 내부의 값을 변경한다는 것을 명시

> 클래스트의 인스턴스 메서드
```Swift
    class LevelClass{
        var level: Int = 0{
            didSet{
                print("Level \(level)")
            }
        }
        func levelUp(){
            print("level up")
            level += 1
        }
        func levelDown(){
            print("level down")
            level -= 1
            if level < 0{
                reset()
            }
        }
        func jumpLevel(to: Int){
            print("jump to \(to)")
            level = to
        }
        func reset(){
            print("reset")
            level = 0
        }
    }

    var levelClassInstance: LevelClass = LevelClass()
    levelClassInstance.levelUp()
    //Level 1
    levelClassInstance.levelDown()
    //Level 0
    levelClassInstance.levelDown()
    //Level -1
    //reset
    //Level 0
    levelClassInstance.jumpLevel(to: 3)
    //Level 3
```

>mutating 키워드 사용
```Swift
    struct LevelStruct{
        var level: Int = 0{
        didSet{
            print("Level \(level)")
        }
    }
    mutating func levelUp(){
        print("Level Up!")
        level += 1
    }
    mutating func levelDown(){
        print("Level Down")
        level -= 1
        if level < 0{
            reset()
        }
    }
    mutating func jumpLevel(to: Int){
        print("jump to \(to)")
        level = to
    }
    mutating func reset(){
        print("reset")
        level = 0
    }
}

var levelStruct: LevelStruct = LevelStruct()
levelStruct.levelUp()
levelStruct.levelDown()
```

## self 프로퍼티
    ● self 프로퍼티는 인스턴스를 더 명확히 지칭하고 싶을 때 사용한다.
    ● 값 타입 인스턴스 자체의 값을 치환할 수 있다.
    ● 구조체, 열거형 등은 self 프로퍼티를 사용하여 자신 자체를 치환할 수 있다.
    ● 지역변수 -> 메서드 매개변수 -> 인스턴스의 프로퍼티 순으로 자동으로 유추한다.

> self 프로퍼티와 mutating 키워드
```Swift
    class LevelClass{
        var level: Int = 0

        func reset(){
            self = LevelClass()
            //오류 : self 프로퍼티 참조 변경 불가
        }
    }

    struct LevelStruct{
        var level: Int = 0

        mutating func levelUp(){
            print("Level UP")
            level += 1
        }
        mutating func reset(){
            print("reset")
            self = LevelStruct()
        }
    }   
    
    var levelStructInstance: LevelStruct = LevelStruct()

    levelStructInstance.levelUp() //LevelUp
    print(levelStructInstance.level) //1

    levelStructInstance.reset() //reset
    print(levelStructInstance.level) //0

    enum OnOffSwitch{
        case on, off
        mutating func nextState(){
            self = self == .on ? .off : .on
        }

        var toggle: OnOffSwitch = OnOffSwitch.off
        toggle.nextState()
        print(toggle) //on
    }
```

## 인스턴스를 함수처럼 호출하도록 하는 메서드
    ● 특정 타입의 인스턴스를 문법적으로 함수를 사용하는 것처럼 보이게 한다.
    ● 인스턴스를 함수처럼 호출할 수 있도록 하려면 메서드를 구현하면 된다.
    ● mutating, throws, rethrows 사용 가능
    ● 매개변수 타입, 전달인자 레이블, 반환 타입이 겹치지 않ㄴ느다면 원하는 만큼 구현할 수 있다.
    ● 메서드를 호출하는 것 외에 함수 표현으로는 사용할 수 없다.

>callAsFunction 메서드 구현
```Swift
    struct Puppy{
        var name: String = "멍멍이"

        func callAsFunction(){
            print("멍멍")
        }

        func callAsFunction(destination: String){
            print("\(destination)으로 간다")
        }

        func callAsFunction(something: String, times: Int){
            print("\(something)를 \(times)번 반복한다")
        }

        func callAsFunction(color: String) -> String{
            return "\(color) 응가"
        }

        mutating func callAsFunction(name: String){
            self.name = name
        }
    }

    var doggy: Puppy = Puppy()
    
    doggy.callAsFunction() //멍멍
    doggy() 멍멍
    doggy.callAsFunction(destination: "집")
    //집으로 간다
    doggy.callASFunction(something: "점프", times: 2)
    //점프를 2번 반복
    doggy(name: "댕댕이")

    print(doggy.name)//댕댕이
```

## 타입 메서드
    ● 메서드에도 인스턴스 메서드와 타입 메서드가 있다.
    ● 타입 자체에 호출이 가능한 메서드
    ● 메서드 앞에 static 키워드를 사용한다.
    ● 클래스의 타입 메서드는 static 키워드와 class 키워드를 사용한다.
    ● static으로 정의하면 상속 후 메서드 재정의가 불가능
    ● class로 정의하면 상속 후 메서드 재정의가 가능
    ● 타입 메서드의 self는 타입을 가리킨다. => 타입 메서드 내부에서 self는 타입 이름과 같다.

> 클래스의 타입 메서드
```Swift
    class AClass{
        static func staticTypeMethod(){
            print("AClass staticTypeMethod")
        }
        class func classTypeMethod(){
            print("AClass classTypeMethod")
        }
    }

    class BClass: AClass{
        override static func staticTypeMethod(){
        
        }//오류 발생 -> static이므로 재정의 불가

        override classs func classTypeMethod(){
            print("BClass classTypeMethod")
        }
    }

    AClass.staticTypeMethod()
    //AClass staticTypeMethod
    AClass.classTypeMethod()
    //AClass classTypeMethod
    BClass.classTypeMethod()
    //BClass classTypeMethod
```

> 타입 프로퍼티와 타입 메서드의 사용
```Swift

    /* 인스턴스 메서드에서는 self가 인스턴스를 가리킨다면 타입 메서드의 self는 타입을 가리킨다. */


    struct SystemVolume{
        static var volume: Int = 5 //타입 프로퍼티 즉, 유일한 값
        static func mute(){
            self.volume = 0 //타입 메서드
        }
    }

    class Navigation{
        var volume: Int = 5 //타입 프로퍼티가 아니므로 인스턴스마다 각각의 볼륨값을 가진다.

        func guideWay(){
            SystemVolume.mute()
        }

        func finishGuideWay(){
            SystemVolume.volume = self.volume
        }
    }

    SystemVolume.volume = 10

    let myNavi: Navigation = Navigation()

    myNavi.guideWay() //SystemVolume = 0
    myNavi.finishGuideWay() //SystemVolume = 5sx
```
