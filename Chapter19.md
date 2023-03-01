
# my Swift Study - Chapter19[타입캐스팅]
* 스위프트는 데이터 타입 안전을 위하여 각기 다른 타입끼리의 값 교환을 엄격히 제한한다.

## 스위프트의 타입 변환

```Swift
    var value: Double = 3.3
    var convertedValue: Int = Int(value)
    convertedValue = 5.5 // 오류
``` 

## 스위프트 타입캐스팅
    ● 스위프트에서는 다른 언어의 타입 변환 혹은 타입캐스팅을 이니셜라이저로 단순화!!
    ● 스위프트의 타입캐스팅은 인스턴스의 타입을 확인하거나 자신을 다른 타입의 인스턴스인양 행세할 수 있는 방법으로 사용할 수 있다.


> <b> Coffe 클래스와 Coffe 클래스를 상속받은 Latte와 Americaano 클래스
```Swift
   class Coffe{
    let name: String 
    let shot: Int

    var description: String{
        return "\(shot) shot(s) \(name)"
    }

    init(shot: Int){
        self.shot = shot
        self.name = "coffe"
    }
   }

   class Latte: Coffe{
    var flavor: String

    override var description: String{
        return "\(shot) shot(s) \(flavor) latte"
    }

    init(flavor: String, shot: Int){
        self.flavor = flavor
        self.init(shot: shot)
    }
   }

   class Americano: Coffe{
    let iced: Bool

    override var description: String{
        return "\(shot) shot(s) \(iced ? "iced" : "hot") ameriano"
    }

    init(shot: Int, iced: Bool){
        self.iced = iced
        super.iniet(shot: shot)
    }
   }
```
## 데이터 타입 확인

    ● 타입 확인 연산자인 is를 사용하여 인스턴스가 어떤 클래스의 인스턴스인지 타입을 확인해볼 수 있다.
    ● 자식 클래스의 인스턴스라면 ture, 그렇지 않다면 false를 반환
    ● is 연산자 외에도 타입 확인 가능 -> 메타 타입 타입 이용
    ● 클래스, 구조체, 열거형의 이름은 타입의 이름 -> .Type을 붙이면 메타 타입을 나타냄
    ● 프로토콜 타입의 메타 타입은 .Protocol
    ● .self 표현 시 -> 타입을 값으로 표현 가능하다.  


> <b> 데이터 타입 확인
```SWift
 class Coffe{
    let name: String 
    let shot: Int

    var description: String{
        return "\(shot) shot(s) \(name)"
    }

    init(shot: Int){
        self.shot = shot
        self.name = "coffe"
    }
   }

   class Latte: Coffe{
    var flavor: String

    override var description: String{
        return "\(shot) shot(s) \(flavor) latte"
    }

    init(flavor: String, shot: Int){
        self.flavor = flavor
        self.init(shot: shot)
    }
   }

   class Americano: Coffe{
    let iced: Bool

    override var description: String{
        return "\(shot) shot(s) \(iced ? "iced" : "hot") ameriano"
    }

    init(shot: Int, iced: Bool){
        self.iced = iced
        super.iniet(shot: shot)
    }
   }


   let coffee: Coffee = Cofffe(shot: 1)
   print(coffee.description) // 1 shot(s) coffe

   let myCoffe: Americano = Americano(shot: 2, iced: false)
   print(myCoffe.description) // 2 shot(s) hot americano

   let yourCoffe: Latte = Latte(flavor: "green tea", shot: 3)
   print(yourCoffe.description) // 3 shot(s) green tea latte

   print(coffee is Coffee) // true
   print(coffee is Americano) // false
   print(coffee is Latte) // false

   print(myCoffee is Coffee) // true
   print(yourCoffee is Coffee) // true

   print(myCoffee is Lattee) // false
   print(yourCoffee is Lattee) // true


   /*
   프로그램 실행 중에 인스턴스의 값을 알앙보고자 한다면 type(of:) 함수를 사용한다.
   type(of: someInstance).self 라고 표현하면 someInstance의 타입을 값으로 표현한 값을 반환
   */

   print(type(of: Coffee) == Coffee.self) // true
   print(type(of: myCoffee) == Lattee.self) // false
```

## 다운캐스팅

    ● 클래스의 상속 모식도에서 자식클래스보다 더 상위에 있는 부모클래스의 타입을 자식클래스의 타입으로 캐스팅한다고 해서 다운캐스팅이라고 부른다.
    ● Any 타입 등에서 다른 타입으로 캐스팅할 떄도 다운캐스팅을 사용!!
    ● 타입캐스트 연산자 -> as?, as!
    ● as? -> 옵셔널 반환, as! -> 옵셔널 아님
    ● 항상 성공하는 다운캐스팅 as 사용


> <b> 다운캐스팅
```Swift
    class Coffe{
    let name: String 
    let shot: Int

    var description: String{
        return "\(shot) shot(s) \(name)"
    }

    init(shot: Int){
        self.shot = shot
        self.name = "coffe"
    }
   }

   class Latte: Coffe{
    var flavor: String

    override var description: String{
        return "\(shot) shot(s) \(flavor) latte"
    }

    init(flavor: String, shot: Int){
        self.flavor = flavor
        self.init(shot: shot)
    }
   }

   class Americano: Coffe{
    let iced: Bool

    override var description: String{
        return "\(shot) shot(s) \(iced ? "iced" : "hot") ameriano"
    }

    init(shot: Int, iced: Bool){
        self.iced = iced
        super.iniet(shot: shot)
    }
   }


   let coffee: Coffee = Cofffe(shot: 1)
   print(coffee.description) // 1 shot(s) coffe

   let myCoffe: Americano = Americano(shot: 2, iced: false)
   print(myCoffe.description) // 2 shot(s) hot americano

   let yourCoffe: Latte = Latte(flavor: "green tea", shot: 3)

    if let actingOne: Americano = coffee as? Americano{
    print("This is Americano")
   } else{
    print(coffee.description)
   }
   // 1 shot(s) coffee

   if let actingOne: Lattee = coffee as? Latte{
    print("This is Latte")
   } else{
    prnt(coffee.description)
   }
   // 1 shot(s) coffee

   if let actionOne: Americano = myCoffee as? Americano {
    print("This is Americano")
   } else{
    print(coffee.description)
   }
   // This is Americano 

   if let actiongOne: Coffee = myCoffee as? Coffee{
    print("This is just Coffee")
   } else{
    print(coffee.description)
   }
   // This is just Coffee

   let castedCoffee: Coffee = yourCoffee as! Coffee
   // Success

   /*
    타입 캐스팅의 의미
     -> 실제로 인스턴스를 수정하거나 값을 변경하는 작업이 아니다. 인스턴스는 메모리에는 똑같이 남아있을 뿐이다.
   */
   
```

##  Any, AnyObject의 타입캐스팅

    ● Any, AnyObject라면 전달받은 데이터가 어떤 타입인지 확인하고 사용해야 한다. -> 스위프트는 암시적 타입 변환을 허용하지 않는다. 타입에 굉장히 엄격하다.

> <b> AnyObject 타입 확인
```Swift
    func checkType(of item: AnyObject){
        if item is Latte{
            print("item is Latte")
        } else if item is Americano{
            print("item is Americano")
        } else if item is Coffee{
            print("item is Coffee")
        } else{
            print("Unknown Type")
        }
    }

    checkType(of: coffee) // item is Coffee
    checkType(of: myCoffee) // item is Americano
    checkType(of: yourCoffee) // item is Latte
    checkType(of: actingConstant) // item is Latte

    // 타입 판단과 동시에 해당 타입의 인스턴스로 사용할 수 있도록 캐스팅

    func castTypeToAppropriate(item: AnyObject){
        if let castedItem: Latte = item as? Latte{
            print(catedItem.description)
        } else if let catedItem: Americano = item as? Americano{
            print(castedItem.description)
        } else if let catedItem: Coffee = item as? Coffee{
            print(castedItem.description)
        } else {
            print("Unknown Type")
        }
    }

    castTypeToAppropriate(item: coffee) // 1 shot(s) coffee
    castTypeToAppropriate(item: myCoffee) // 2 shot(s) hot americano
    castTypeToAppropriate(item: yourCoffee) // 3 shot(s) greent tea latte
    castTypeToAppropriate(item: actingConstant) // 2 shot(s) vanilla latte


    // Any는 모든 타입의 인스턴스를 취한다. -> 함수, 구조체, 클래스. 열거형 등 모든 타입의 인스턴스를 의미힌다.

    func checkAnyType(of item: Any){
        switch item{
            case 0 as Int:
                print("zero as an Int")
            case 0 as Double:
                print("zero as a Double")
            case let someInt as Int:
                print("an integer value of \(someInt)")
            default:
                print("something else : \(type(of: item))")
        }
    }

    checkAnyType(of: 0) // zero as an Int
    checkAnyType(of: 0.0) // zero as a Double
    checkAnyType(of: 42) // an integer value of 42


    /*
        Any 타입의 값으로 사용하고자 한다면 as 연산자를 사용하여 명시적 타입 캐스팅을 해주면 경고 메시지를 받지 않는다.
    */

    let optionalValue: Int? = 100
    print(optionalValue) // 컴파일러 경고 발생
    print(optionalValue as Any) // 경고 없음
```