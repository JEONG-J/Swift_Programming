
# my Swift Study - Chapter11[인스턴스 생성 및 소멸]
* 초기화는 클래스나 구조체 또는 열거형의 인스턴스를 사용하기 위한 준비 과정
* 초기화가 완료된 인스턴스는 사용 후 소멸 시점이 오면 소멸한다.

## 인스턴스 생성
    ● 저장 프로퍼티의 초깃값을 설정하는 일
    ● 이니셜라이저를 정의하면 초기화 과정을 직접 구현할 수 있다
    ● 구현된 이니셜라이저는 새로운 인스턴스를 생성할 수 있는 특별한 메서드가 된다.
    ● 이니셜라이저의 역할은 그저 인스턴스의 첫 사용 을 위해 초기화하는 것!
    ● init 키워드를 사용한다.

## 프로퍼티의 기본값
    ● 구조체와 클래스의 인스턴스는 옵셔널 저장 프로퍼티를 제외한 모든 저장 프로퍼티에 초깃값을 할당해야 한다.
    ● 프로퍼티를 정의할 때 프로퍼티 기본값을 할당한다.
    ● 이니셜라이저를 통해 초깃값을 할당하거나, 프로퍼티 기본값을 통해 처음의 저장 프로퍼티가 초기활 될 때 프로퍼티 감시자 메서드가 호출되지 않는다.

> Area 구조체와 이니셜라이저
```Swift
    struct Area{
        var squareMeter: Double

        init(){
            squareMeter = 0.0 // 초기값 할당
        }
    }

    let room: Area = Area()
    print(room.squareMeter) //0.0
```
>프로퍼티 기본값 지정
```Swift
    struct Area{
        var squareMeter: Double = 0.0 //프로퍼티 기본 값 할당
    }

    let room: Area = Area()
    print(room.squareMeter) //0.0
```

## 이니셜라이저 매개변수
    ● 이니셜라이저도 매개변수를 가질 수 있다.
    ● 인스턴스를 초기화하는 과정에 필요한 값을 전달받을 수 있다.

> 이니셜라이저 매개변수
```Swift
    struct Area{
        var squareMeter: Double

        init(fromPy py: Double){
            squareMeter = py * 3.14
        }

        init(fromSquareMeter squareMeter: Double){
            self.squareMeter = squareMeter
        }

        init(value: Double){
            squareMeter = value
        }

        init(_ value: Double){ // 전달인자 value가 필요하지 않다면  네 번째 이니셜라이저 처럼 와일드카드 식별자를 사용하여 전달인자 레이블을 없애주면 된다.
            squareMeter = value
        }

        let roomOne: Area = Area(fromPy: 2.0)
        print(roomOne.squareMeter) //6.28

        let roomTwo: Area = Area(fromSquareMeter: 10.0)
        print(roomTwo.squareMeter) //10.0

        let roomThree: Area = Area(value: 20.0)
        let roomFour: Area = Area(40.0)
        
    }
```

## 옵셔널 프로퍼티 타입
    ● 인스턴스가 사용되는 동안에 값을 꼭 갖지 않아도 되는 저장 프로퍼티
    ● 값을 할당하지 않으면 자동으로 nil이 할당

> Person 클래스
```Swift
    class Person{
        var name: String
        var age: Int?

        init(name: String){
            self.name = name
        }
    }

    let my: Person = Person(name: "my")
    print(my.name) //"my"
    print(my.age) //nil

    my.age =  22
    print(my.age) //22
```

## 상수 프로퍼티
    ● 상수(let)으로 선언된 저장 프로퍼티는 인스턴스를 초기화하는 과정에서만 값을 할당할 수 있다.
    ● 처음 할당된 이후로는 값을 변경할 수 없다.

> 상수 프로퍼티의 초기화
```Swift
    class Person{
        let name: String //상수로 초기화
        var age: Int?

        init(name: String){
            self.name = name
        }
    }

    let my: Person = Person(name: "my")
    my.name = "yo" //오류 발생
```

## 기본 이니셜라이저와 멤버와이즈 이니셜라이즈
    ● 사용자 정의 이니셜라이저를 정의해주지 않으면 클래스나 구조체는 모든 프로퍼티에 기본값이 지정되어 있다는 전제하에 기본 이니셜라이저를 사용한다.
    ● 기본 이니셜라이저는 프로퍼티 기본값으로 프로퍼티를 초기화해서 인스턴스를 생성한다.
    ● 구조체는 사용자 정의 이니셜라이저를 구현하지 않으면 프로퍼티의 이름으로 매개변수를 갖는 이니셜라이저인 멤버와이즈 이니셜라이즈를 기본으로 제공
    ● 클래스는 멤버와이즈 이니셜라이저를 지원하지 않는다.

> Point 구조체와 Size 구조체의 선언과 멤버와이즈 이니셜라이저 사용
```Swift
    struct Point{
        var x: Double = 0.0
        var y: Double = 0.0
    }

    struct Size{
        var width: Double = 0.0
        var height: Double = 0.0
    }

    let point: Point = Point(x: 0, y: 0)
    let size: Size = Size(width: 50.0, height: 90.0)
    let somePoint: Point = Point(x: 2.0)
    let someSize: Size = Size(height: 1.0)
```

## 초기화 위임
    ● 구조체와 열거형은 코드의 중복을 피하기 위해 이니셜라이저가 다른 이니셜라이저에게 일부를 초기화를 위함하는 초기화 위임을 구현할 수 있다.
    ● 클래스는 초기화 위임을 할 수 없다.
    ● 이니셜라이저가 다른 이니셜라이저를 호출하려면 self.init을 사용한다.

>Student 열거형과 초기화 위임
```Swift
    enum Student{
        case elementary, middle, high
        case none

        init(){
            self = .none
        }

        init(koreanAge: Int){ //사용자 정의 이니셜라이저
            switch koreanAge{
                case 8...13:
                    self = .elementary
                case 14...16:
                    self = .middle
                case 17...19:
                    self = .high
                default:
                    self = .none
            }
        }
        init(bornAt: Int, currentYear: Int){ // 두 번째 사용자 정의 이니셜라이즈 ->> 태어난 해, 현재 연도 전달받아 나이로 계산한 후 첮 번째 이니셜라이저에 초기화를 위임한다.
        // 초기화 위임 방법을 사용하면 코드를 중복으로 쓰지 않고도 효율적으로 여러 case의 이니셜라이저를 만들 수 있다.
            self.init(koreanAge: currentYear - bornYear + 1)
        }
    }

        var my: Student = Student(koreanAge: 16)
        print(my) //middle

        my = Student(bornAt: 1998, currentYear: 2009)
        print(my) //high
```

## 실패 가능한 이니셜라이저
    ● 실패 가능한 이니셜라이저 : 어려 이유로 인스턴스 초기화에 실패할 수 있다. 이런 실패 가능성을 내포한 이니셜라이저
    ● 실패 가능한 이니셜라이저는 실패했을 때 nil을 반환
    ● 실패하지 않는 이니셜라이저와 실패 가능한 이니셜라이저를 같은 이름과 같은 매개변수 타입을 갖도록 정의할 수 없다.
    ● 열거형에서 rawValue를 통한 이니셜라이저는 기본적으로 실패 가능한 이니셜라이저로 제공

> 실패 가능한 이니셜라이저
```Swift
    class Person{
        let name: String
        var age: Int?

        init?(name: String){
            if name.isEmpty{
                return nil
            }
            self.name = name
        }
        init?(name: String, age: Int){
            if name.isEmpty || age < 0{
                return nil
            }
            self.name = name
            self.age = age
        }
    }

    let my: Person = Person(name: "my", age: 22)

    if let person: Person = my{
        print(person.name)
    } else{
        print("not person")
    }

    let riahn Person? = Person(name: "riahn", age: -10)

    if let person: Person = riahn{
        print(person.name)
    } else{
        print("not person")
    } // age < -10 , 그러므로 not person 출력한다.
```

> 열거형의 실패 가능한 이니셜라이저
```Swift
    enum Student: String{
        case elementary = "초등학생", middle = "중학교", high = "고등학생"

    init?(koreanAge: Int){
        switch koreanAge{
            case 8...13:
                self = .elementary
            case 14...16:
                self = .middle
            case 17...19:
                self = .high
            default:
                return nil
        }
    }

    init?(bornAt: Int, currentYear: Int){
        self.init(koreanAge = bornAt - currentYear + 1)
    }
}
    var my: Student? = Student(koreanAge: 20)
    print(my) //nil

    my = Student(bornAt: 2020, currentYear: 2016)
    print(my) //nil

    my = Student(rawValue: "고등학생")
    print(my) //high

    my = Student(rawValue: "대학생")
    print(my) //nil
```

## 함수를 사용한 프로퍼티 기본값 설정
    ● 사용자 정의 연산을 통해 저장 프로퍼티 기본값을 설정하고자 한다면 클로저나 함수를 사용하여 프로퍼티 기본값을 제공할 수 있다.
    ● 클로저나 함수의 반환 타입은 프로퍼티의 타입과 일치해야 한다.

> 클로저를 통한 student 프로퍼티 기본값 설정
```Swift
    struct Student{
        var name: String?
        var number: Int?
    }

    class SchoolClass{
        var students: [Student] = {
        var arr: [Student] = [Student]()

        for num in 1...15{
            var student: Student = Student(name: nil, number: num)
            arr.append(student)
        }
        return arr
    }() /*클로저 뒤에 소괄호가 붙은 이유는 클로저를 실행하기 위해서이다. 클로저뒤에 소괄호가 붙어 클로저를 실행한 결과괏은 프로퍼티의 기본값이 된다. 만약 소괄호가 없다면 프로퍼티의 기본값은 클로저 그 자체가 된다. */
}

let myClass: SchoolClass = SchoolClass()
print(myClass.students.count) //15
```

## 인스턴스의 소멸
    ● 클래스의 인스턴스는 디이니셜라이저를 구현할 수 있다.
    ● deinit 키워드 사용
    ● 클래스의 인스턴스에만 구현할 수 있다.
    ● 인스턴스를 메모리에서 소멸하기 직전에 인스턴스에 저장되어 있던 데이터를 디스크에 파일로 저장해줘야 하는 경우에 사용
    ● 클래스는 디이니셜라이저를 단 하나만 구현
    ● 인스턴스를 소멸하기 직전에 호출
    ● 프로그래머가 설계한 로직에 따라 인스턴스가 메모리에서 해제되기 직전에 적절한 작업을 하도록 할 수 있다.
    
> 디이니셜라이저의 구현
```Swift
    class SomeClass{
        deinit{
            print("exit")
        }
    }

    var instance: SomeClass? = SomeClass()
    instance = nil //exit
```

> FileManager 클래스의 디이니셜라이저 활용
```Swift
    class FileManager{
        var fileName: String

        init(fileName: String){
            self.fileName = fileName
        }

        func openFile(){
            print("OpenFile: \(self.fileName)")
        }
        func modifyFile(){
            print("ModifyFile: \(self.fileName)")
        }
        func writeFile(){
            print("WriteFile: \(self.fileName)")
        }
        func closeFile(){
            print("CloseFIle: \(self.fileName)")
        }

        deinit{
            pint("Exit")
            self.writeFile()
            self.closeFile()
        }
    }

    var fileManger: FileManager? = FileManager(fileName: "abc.txt")

    if let manager: FileManager = fileManager{
        manager.openFile()
        manager.modifyFile()
    }

    fileManager = nil
    //deinit 부분 실행된다.
```
---
야곰 지음 - 스위프트 프로그래밍3판을 공부하고 작성하였음
---