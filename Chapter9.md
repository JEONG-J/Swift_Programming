
# my Swift Study - Chapter9[구조체와 클래스]
    ※ 프로퍼티와 메서드를 사용하여 구조화된 데이터와 기느을 가질 수 있다.

## 구조체
    ● 인스턴스를 생성하고 초기화하고자 할 때 기본적으로 이니셜라이즈 사용
    ● let 선언 : 프로퍼티 값 변경 x
    ● var 선언 : 프로퍼티 값 변경 o

```Swift
    struct BasicInformation{
        var name: String
        var age: Int

        var myInfo: BasicInformation = BasicInformation(name: "my", age: 22)

        myInfo.name = "you"
        myInfo.age = 20
    }
```

##  클래스
    ● 부모클래스가 없더라고 상속 없이 단독으로 정의 가능
    ● 클래스는 상속이 가능하기 때문에 상속 할 때 클래스 이름 뒤에 : 작성하고, 부모 클래스 이름을 명시한다.

```Swift
    class Person{
        var height: Float = 0.0
        var weight: Float = 0.0
    }
```

## 클래스 인스턴스의 생성과 초기화
    ● 초기화하고자 할 때 기본적인 이니셜라이저를 사용
    ● 기본 이니셜라이저, 사용자 정의 이니셜라이저 가능

```Swift
    class Person{
        var height: Float = 0.0
        var weight: Float = 0.0
    }

    var my: Person = Person()
    my.height = 123.1
    my.weight = 54.2
```

## 클래스 인스턴스의 소멸
    ● 클래스의 인스턴스는 참조타입!! 그래서 참조할 필요 없을 때 메모리에서 해제됨
    ● deinit 메서드(디이니셜라이즈) 구현해주면 소멸되기 직전 deinit 메서드 호출
    ● deinit 메서드는(디이니셜라이즈) 클래스당 하나만 구현
    ● 인스턴스 소멸 전에 데이터를 저장한다거나 다른 객체에 인스턴스 소멸을 알려야 할 때 deinit 메서드 구현
```Swift
    class Person{
        var height: Float = 0.0
        var weight: Float = 0.0
    
        deinit{
            print("Exit")
        }
    }

    var my: Person? = Person()
    my = nil // Exit
```

## 구조체와 클래스의 차이
    ● 구조체는 상속 불가
    ● 클래스의 인스턴스에만 타입캐스팅 가능
    ● 클래스의 인스턴스에만 디이니셜라이즈 가능
    ● 참조 횟수 계산은 클래스의 인스턴스에만 적용

## 값 타입과 참조 타입
    ● 구조체 : 값 타입 -> 전달 값 복사
    ● 클래스 : 참조 타입 -> 주소 전달
    ● 참조가 같은지 확인 할 때, === 사용

```Swift
    struct BasicInformation{
        let name: String
        var age: Int
    }

    var myInfor: BasicInformation = BasinInformation(name: "my", age: 22)
    myInfor.age = 21

    var yourInfor: BasicInformtion = myInfor
    // 값을 복사하여 할당

    print(yourInfor.age) // 21
    print(myInfor.age) // 21

    yourInfor.age = 11

    print(yourInfor.age) // 11
    print(myInfor.age) // 21

    class Person{
        var height: Float = 0.0
        var weight: Float = 0.0
    }

    var my: Person = Person()
    var your: Peroson = my

    print(my.height) // 0.0
    print(your.height) // 0.0

    your.height = 152.2

    print(my.height) // 152.2
    print(your.height) // 152.2
    // 참조하는 곳이 같기 때문에 같이 변경

    func changeBasicInfor(_ info: BasicInformation){
        var copiedInfor: BasicInformation = info
        copiedInfor.age = 1
    }
    func changePersonInfor(_ info: Person){
        info.height = 155.3
    }

    changeBasicInfor(myInfor)
    print(myInfor.age) // 21, 값 그대로!!

    changePersonInfor(my)
    print(my.height) // 155.3, 참조 전달되어 같이 변경!!
```

```Swift
    var my: Person = Person()
    let you: Person = my
    let friend: Person = Person()

    print(my === you) // true
    print(my === friend) // false
    print(you != friend) // true
```

## 구조체와 클래스 선택해서 사용
    ● 프로젝트의 성경에 따라, 데이터의 활용도에 따라, 특정 타입을 구현할 때 구조체와 클래스 둘 중 하나를 선택해서 사용
---
+ 다음의 경우 구조체 사용할 것!!

    + 연관된 간단한 값의 집합을 캡슐화하는 것만이 목적
    + 캡슐화한 값을 참조하는 것보다 복사하는 것이 합당할 때
    + 구조체에 저장된 프로퍼티가 값 타입이며 참조하는 것보다 복사하는 것이 합당할 때
    + 다른 타입으로부터 상속받거나 자신을 상속할 필요가 없을 때
---
야곰 지음 - 스위프트 프로그래밍3판을 공부하고 작성하였음
---