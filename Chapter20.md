
# my Swift Study - Chapter20[프로토콜]
* 프로토콜 : 특정 역할을 하기 위한 메서드, 프로퍼티,, 기타 요구사항 등의 청사진
* 구조체, 클래스, 열거형은 프로토콜을 채택해서 특정 기능을 실행하기 위한 프로토콜의 요구 사항을 실제로 구현할 수 있다.
* 타입에서 프로토콜의 요구사항을 충족시키려면 프로토콜이 제시하는 청사진의 기능을 모두 구현해야 한다.
* 프로토콜은 정의를 하고 제시를 할 뿐이지 스스로 기능을 구현하지 않는다.

## 프로토콜 채택

```Swift
    protocol 프로토콜 이름{
        프로토콜 정의

        // 구조체, 클래스, 열거형 등에서 프로토콜을 채택하려면 타입 이름 뒤에 콜론(:)을 붙여준 후 채택할 프로토콜 이름을 쉼표(,)로 구분한다.

    struct SomeStruct: AProtocol, AnotherProtocol{
        // 구조체 정의
    }

    class SomeClass: AProtocol, AnotherProtocol{
        // 클래스 정의
    }

    enum SomeEnum: AProtocol, AnotherProtocol{
        // 열거형 정의
    }
``` 



## 프로퍼티 요구
    ● 자신을 채택한 타입이 어떤 프로퍼티를 구현해야 하는지 요구할 수 있다.
    ● 프로토콜을 채택한 타입은 프로토콜이 요구하는 프로퍼티의 이름과 타입만 맞도록 구현해주면 된다.
    ● 프로토콜의 프로퍼티 요구사항은 항상 var 키워드를 사용한다.


> <b> Coffe 클래스와 Coffe 클래스를 상속받은 Latte와 Americaano 클래스
```Swift
   protocol SomeProtocol{
    var settableProperty: String { get set }
    var notNeedToBeSettableProperty: String { get }
   }

   protocol AnotherProtocol{ // 타입 프로퍼티
    static var someTypeProperty: Int { get set }
    static var anoterTypeProperty: Int { get }
   }

   // 클래스의 타입 프로퍼티에는 상속 가능한 타입 프로퍼티인 class 타입 프로퍼티와 상속 불가능한 static 타입 프로퍼티가 있다. -> 프로토콜의 타입 프로퍼티 요구는 static으로 통일
```

```SWift
    protocol Sendable{
        var from: String { get }
        var to: String { get }
        }
        class Message: Senddable{
            var sender: String
            var from: String{
                return self.sender
            }
            var to: String

            init(sender: String, receiver: String){
                self.sender = sender
                self.to = receiver
            }
        }

        class Mail: Sendable{
            var from: String
            var to: String

            init(sender: String, receiver: String){
                self.from = sender
                self.to = receiver
            }
        }
    
```

## 메소드 요구

    ● 프로토콜은 특정 인스턴스 메서드나 타입 메서드를 요구할 수 있다.
    ● 메서드의 이름, 매개변수, 타입 등만 작성한다. 또한 가변 매개변수도 허용
    ● 타입 메서드를 요구할 때 타입 프로퍼티와 마찬가지로 앞에 static 키워드 명시
    ● static 키워드를 사용하여 요구한 타입 메서드를 클래스에서 실제 구현할 때는 static, class 사용해도 무방


> <b> 다운캐스팅
```Swift
    // 수신받는 기능
   protocol Receiveable{
    func received(data: Any, from: Sendable)
   }

   // 발신 받는 기능
   protocol Sendable{
    var from: Sendable { get }
    var to: Receiveable? { get }

    func send(data: Any)

    static func isSendableInstance(_ instance: Any) -> Bool
   }

    // Sendable 프로토콜을 준수하는 타입의 인스턴스
   class Message: Sendable, Receiveable{
    var from: Sendable{
        return self
    }

    // 상대방은 수신 가능한 객체, 즉 Receiveable 프로토콜을 준수하는 타입의 인스턴스여야 한다.
    var to: Receiveable?

    // 메시지 발신
    func send(data: Any){
        guard let receiver: Receiveable = self.to else{
            print("Message has no receiver")
            return
        }
        // 수신 가능한 인스턴스의 received 메서드 호출
        receiver.received(data: data, from: self.from)
    }

    // 메시지 수신
    func received(data: Any, from: Sendable){
        print("Message received \(data) from \(from)")
    }

    // class 메서드이므로 상속이 가능하다.
    class func isSendableInstance(_ instance: Any) -> Bool {
        if let sendableInstance: Sendable = instance as? Sendable{
            return sendableInstace.to != nil
        }
        return false
    }
   }

   // 수신, 발신 가능한 Mail
   class Mail: Sendable, Receiveable{
    var from: Sendable{
        return self
    }

    var to: Receiveable?

    func send(data: Any){
        guard let receiver: Receiveable = self.to else{
            print("Mail has no receiver")
            return
        }

        receiver.received(data: data, from: self.from)
    }


   func received(data: Any, from: Sendable){
    print("Mail received \(data) from \(from)")
   }

   // static 메서드이므로 상속이 불가능하다.
   static func isSendableInstance(_ instance: Any) -> Bool{
    if let sendableInstance: Sendable = instace as? Sendalbe{
        return sendableInstnace.to != nil
    }
    return false
    }
   }

   // 두 Message 인스턴스를 생성한다.
   let myPhoneMessage: Message = Message()
   let yourPhoneMessage: Message = Message()

   // 아직 수신받을 인스턴스가 없다.
   myPhoneMessage.send(data: "Hello") // Message has no receiver

   // Message 인스턴스는 발신, 수신 모두 가능
   myPhoneMessage.to = yourPhoneMessage
   myPhoneMessage.send(data: "Hello") // Message received Hello from Message

   // 두 Mail 인스턴스 생성
   let myMail: Mail = Mail()
   let yourMail: Mail = Mail()

   myMail.to = yourMail
   myMail.snd(data: "hi") // Mail Received Hi from Mail

   myMail.to = myPhoneMessage
   myMail.send(data: "Bye") // Message received Bye from Mail

   Message.isSendableInstance(myPhoneMessage) // true
   Mail.isSendableInstance(yourPhoneMessage) // false


   /*
    Message, Mail 클래스는 Sendable Receiveable 프로토콜을 준수한다. 그래서 두 프로토콜에서 요구하는 프로퍼티와 메서드를 모두 구현
   */

   /*
    타입으로서의 프로토콜
        - 함수, 메서드, 이니셜라이저에 매개변수 타입이나 반환 타입으로 사용될 수 있다.
        - 프로퍼티, 변수, 상수 등의 타입으로 사용될 수 있다.
        - 배열, 딕셔너리 등 컨테이너 요소의 타입으로 사용될 수 있다.
   */

```

##  가변 메서드 요구

    ● func 키워드 앞에 mutating 키워드를 적어 메서드에서 인스턴스 내부의 값을 변경한다는 것을 명시
    ● 프로토콜이 어떤 타입이든 간에 인스턴스 내부의 값을 변경해야 하는 메서드를 요구하려면 프로토콜의 메서드 정의 앞에 mutating 키워드를 명시한다.
    ● 프로토콜에 mutating 키워드를 사용한 메서드 요구가 있다고 하더라도 클래스 구현에서는 mutating 키워드를 써주지 않아도 된다.

> <b> 가변 메서드 요구
```Swift
    protocol Resettable{
        mutating func reset()
    }

    class Person: Resettable{
        var name: String?
        var age: Int?

    func reset(){
        self.name = nil
        self.age = nil
    }
}
    
    struct Point: Resettable{
        var x: Int = 0
        var y: Int = 0

        mutating func reset(){
            self.x = 0
            self.y = 0
        }
    }

    enum Direction: Resettable{
        case east, west, sout, north, unknwon

        mutating func reset(){
            self = Direction.unknwon
        }
    }


    // Resettable 프로토콜에서 가변 메서드를 요구하지 않는다면 값 타입의 인스턴스 내부 값읇 변경하는 mutating 메서드는 구현이 불가능하다.
```

##  이니셜라이저 요구
    ● 이니셜라이저의 매개변수를 지정하기만 할 뿐, 중괄호를 포함한 이니셜라이저 구현은 하지 않는다.
```Swift
    protocol Named{
        var name: String { get }

        init(name: String)
    }

    struct Pet: Named{
        var name: String

        init(name: String){
            self.name = name
        }
    }


    class Person: Named{
        var name: String

        required init(name: String){
            self.name = name
        }
    }

    // 이니셜라이저 요구에 부합하는 이니셜라이저 구현할 떄는 required 식별자를 붙인 요구 이니셜라이저로 구현해야 한다.
    // 상속받는 클래스에 해당 이니셜라이저를 모두 구현해야 한다.



    // 상속 불가능한 클래스의 이니셜라이저 요구 구현
    final class Person: Named{
        var name: String

        init(name: String){
            self.name = name
        }
    } // 상속이 불가능한 final 클래스라면 required 식별자를 붙여줄 필요 없다.

    // 프로토콜이  요구하는 이니셜라이저가 이미 구현되어 있는 상황에서 그 클래스를 상속받은 클래스가 있다면, required + override 식별자 명시!

    class School{
        var name: String

        init(name: String)
            self.name = name
        }
    }

    class MiddleSchool: School, Named{
        required override init(name: String){
            supuer.init(name: nam`)
        }
    }
```

> <b> 실패 가능한 이니셜라이저 요규를 포함하는 Named 프로토콜과 Named 프로토콜을 준수하는 다양한 타입
```Swift
// 실패 가능한 이니셜라이저를 요구할 수도 있다. 실패 가능한 이니셜라이저 요구하는 프로토콜을 준수하는 타입은 해당 이니셜라이저를 구현할 때 실패 가능한 이니셜라이저로 구현해도, 일반적인 이니셜라이저로 구현해도 무방하다
    protocol Named{
        var name: String { get }

        init?(name: String)
    }

    struct Animal: Named{
        var name: String

        init!(name: String){
            self.name = name
        }
    }

    struct Pet: Named{
        var name: String

        init(name: String){
            self.name = name
        }
    }

    class Person: Named{
        var name: String

        required init(name: String){
            self.name = name
        }
    }

    class School: Named{
        var name: String

        required init?(name: String){
            self.name = name
        }
    }
```

##  프로토콜의 상속과 클래스 전용 프로토콜
    ● 프로토콜은 하나 이상의 프로토콜을 상속받아 기존 프로토콜의 요구사항보다 더 많은 요구사항 추가

```Swift
    protocol Readable{
        func read()
    }

    protocol Writeable{
        func wirte()
    }

    protocol ReadSpeakable: Readable{
        func speak()
    }

    protocol ReadWriteSpeakable: Readable, Writeable{
        func speak()
    }

    class SomeClass: ReadWriteSpeakable{
        func read(){
            print("Read")
        }
        func write(){
            print("Write")
        }
        func speak(){
            print("Speak")
        }
    }


    /*
        프로토콜의 상속 리스트에 class 키워드를 추가해 프로토콜이 클래스 타입에만 채택될 수 있도록 제한할 수 있다.
        제한을 주기 위해 프로토콜의 상속 리스트의 맨 처음에 class 키워드가 위치해야 한다.
    */

    protocol ClassOnlyProtocol: class, Readable, Writeable{
        // 추가 요구사항
    }

    class SomeClass: ClassOnlyProtocol{
        func read() {}
        func write() {}
    }

    struct SomeStruct: ClassOnlyProtocol {
        // 오류 발생
    }
```

##  프로토콜 조합과 프로토콜 준수 확인
    ● 하나의 매개변수에 여러 프로토콜을 한 번에 조합하여 요구할 수 있다.
    ● 하나읨 매개변수가 프로토콜을 둘 이상 요구할 수 있다. -> &를 여러 프로토콜 이름 사이에 써주면 된다.
    ● 구조체나 열거형 타입은 조합할 수 없다. 클래스 타입은 한 타입만 조합할 수 있다.

```Swift
    protocol Named{
        var name: String { get }
    }

    protocol Aged{
        var age: Int { get }
    }

    struct Person: Named, Aged{
        var name: String
        var age: Int
    }

    class Car: Named{
        var name: String

        init(name: String){
            self.name = name
        }
    }

    class Truck: Car, Aged{
        var age: Int

        init(name: String, age: Int){
            self.age = age
            super.init(name: name)
        }
    }

    func celebrateBirthday(to celebrator: Named & Aged){
        print("Happy birthday \(celebrator.name)!! Now you are \(celebrator.age)")
    }

    let my: Person = Person(name: "my", age: 11)
    celebrateBirthday(to: my) // Happy birthday my!! Now you are 11

    var someVariable: Car & Truck & Aged
    // 클래스 & 프로토콜 조합에서 클래스 타입은 한 타입만 조합 가능

    var someVariable: Car & Aged
    // Car 클래스의 인스턴스 역할 수행
    // Aged 프로토콜 준수하는 인스턴스만 수행

    someVariable = Truck(name: "Truck", age: 5)
    // Truck 인스턴스는 Car 클래스 역할도 할 수 있고 Aged 프로토콜도 준수하므로 할당할 수 있다.    
```

##  프로토콜의 선택적 요구
    ● 선택적 요구사항을 정의하고 싶은 프로토콜으 objc 속성이 부여된 프로토콜이어야 한다.
    ● objc 속성이 부여된 프로토콜은 아예 채택할 수 없다.
    ● 선택적 요구를 하면 프로토콜을 준수하는 타입에 해당 요구사항을 필수로 구현할 필요가 없다.
    ● 선택적 요구사항은 optional 식별자를 요구사항의 정의 앞에 붙여주면 된다.
    ● 선택적 요구사항으로 요구하게 되면 그 요구사항의 타입은 자동적으로 옵셔널이 된다.

```Swift
    import Foundation

    @objc protocol Moveable{
        func walk()
        @objc optional func fly()
    }

    // 걷기만 할 수 있는 호랑이
     class Tiger: NSObject, Movealbe{
        func walk(){
            print("Tiger walks")
            )
        }

    //걷고 날 수 있는 새
    class Bird: NSObject, Movealbe{
        func walk(){
            print("Bird walks")
        }

        func fly(){
            print("Bird flys")
        }
    }

    let tiger: Tiger = Tiger()
    let bird: Bird = Bird()

    tiger.walk() // Tiger walks
    bird.walk() // Bird walks


    /*
        objc 속성의 프로토콜을 사용하기 위해 Tiger와 Bird는 각각 Object-C의 클래스인 NSObject를 상속받는다.
    */
```

## 프로토콜 변수와 상수
    ● 프로토콜 변수나 상수를 생성하여 특정 프로토콜을 준수하는 타입의 인스턴스를 할당할 수 있다.

```Swift
    var someNamed: Named = Animal(name: "Animal")
    someNamed = Pet(name: "Pet")
    someNamed = Person(name: "Person")
```

## 위임을 위한 프로토콜
    ● 위임은 클래스나 구조체가 자신의 책임이나 임무를 다른 타입의 인스턴스에게 위임하는 디자인 패턴
    ● 위임 패턴은 애플의 프레임워크에서 사용하는 주요한 패턴 중 하나
    ● 위임 패턴을 위해 다양한 프로토콜이 xxxxDelegate라는 식의 이름으로 정의되어 있다.






    
