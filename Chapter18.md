
# my Swift Study - Chapter18[상속]
* 어떤 클래스로부터 상속을 받으면 상속을 받은 클래스는 `자식클래스`
* 자식클래스에게 자신의 특성을 물려준 클래스를 `부모클래스`
* 부모클래스로부터 물려받은 메서드, 프로퍼티, 서브스크립트 등을 자신만의 내용으로 재정의할 수 있다.
* 다른 클래스로부터 싱속을 받지 않은 클래스를 `기반클래스`


## 클래스 상속
    ● 기반 클래스를 다른 클래스에게 물려받는 것을 말한다.
    ● 부모클래스의 메서드, 프로퍼티 등을 재정의하거나, 기반클래스의 기능이나 프로퍼티를 물려받고 자신의 기능을 추가할 수 있다.

```Swift
    class 클래스 이름: 부모클래스 이름{
            프로퍼티와 메서드들
        }
``` 

## Person클래스를 상속받은 Student 클래스
> <b> School클래스 서브스크립트 구현
```Swift
    class Person{
        var name: String = ""
        var age: Int = 0

        var introduction: String{
            return "이름 : \(name). 나이 : \(age)"
        }

        func speak(){
            print("hello")
        }
    }

    let my: Person = Person()
    my.name = "eui"
    my.age = 27
    print(my.introduction) //이름 : eui. 나이 : 27
    my.speak() // hello

    class Student: Person{
        var grade: String = "F"

        func study(){
            print("Study hard..")
        }
    }

    let myFriend: Student = Student()
    myFriend.name = "cha":
    myFriend.age = 27
    myFriend.grade = "A"
    print(myFriend.introduction) //이름 : cha. 나이 : 27
    myFriend.speak() // hello
    myFriend.study() // Study hard..


    class UniversityStudent: Student{
        var major: String = ""
    }

    let jin: UniversityStudent = UniversityStudent()
    jin.major = "computer"
    jin.speak() // hello
    jin.study() // study hard..

    // 다른 클래스를 상속받으면 똑같은 기능을 구현하기 위하여 코드를 다시 작성할 필요가 없으므로 코드를 재상용하기 용이하다.
    // 더불어 기능을 확장할 때 기존 클래스를 변경하지 않고도 새로운 추가 기능을 구현한 클래스를 정의할 수 있다.
```
## 재정의(오버라이드)

    ● 그대로 사용하지 않고 자신만의 기능으로 변경하여 사용할 수 있다.
    ● 상속 받은 특성들을 재정의하려면 새로운 정의 앞에 override 키워드 사용한다.
    ● 자식클래스에서 특성을 재정의 했지만 필요에 따라 부모클래스의 특성을 활용하고 싶을 때 super 키워드 사용


> <b> 메서드 재정의
```SWift
    class Person{
        var name: String = ""
        var age: Int = 0

        var introduction: String{
            return "이름 : \(name). 나이 : \(age)"
        }

        func speak(){
            print("Hello")
        }

        class func introduceClass() -> String{
            return " 평화 "
        }

    }

    class Student: Person{
        var grade: String = "F"

        func study(){
            print("study hard")
        }

        override func speak(){
            print("학생입니다.")
        }
    }

    class UniversityStudent: Student{
        var major: String = ""

        class func introduceClass(){
            print(super.introduceClass())
        }

        override class func introduceClass() -> String{
            return " a+ "
        }

        override func speak(){
            super.speak()
            print("대학생")
        }
    }

    let my: Person = Person()
    my.speak() // hello

    let jin: Student = Student()
    jin.speak() // 학생입니다.

    let cam: UniversityStudent = UniversityStudent()
    cam.speak() // 학생입니다. 대학생

    print(Person.introduceClass()) // 평화
    print(Student.introduceClass()) // 평화
    print(UniversityStudent.introduceClass() as String) // a+
    UniversityStudent.introduceClass() // 평화

    // introduceClass() 메서드에 override 키워드가 붙은 메서드와 그렇지 않은 메서드 두 가지가 있는 이유는 반환 타입이 다르기 때문이다.
    // 스위프트는 메서드의 반환 타입이나 매개변수가 다르면 서로 다른 메서드로 취급한다.
```

## 프로퍼티 재정의

    ● 프로퍼티를 재정의할 때는 저장 프로퍼티로 재정의할 수는 없다.
    ● 프로퍼티 재정의는 프로퍼티 접근자, 설정자, 감시자 등을 재정의 한다.
    ● 조상 클래스에 읽기 전용 프로퍼티였더라도 자식클래스에서 읽고 쓰기가 가능한 프로퍼티로 재정의 가능하다. 하지만 읽기 쓰기 모두 가능했던 프로퍼티를 읽기 전용으로 재정의해줄 수 없다. 

> <b> 프로퍼티 재정의
```Swift
    class Person{
        var name: Stirng = ""
        var age: Int = 0
        var koreanAge: Int{
            return self.age + 1
        }
        
        var introduction: String{
            return "이름 : \(name). 나이 : \(age)"
        }
    }

    class Student: Person{
        var grade: String = "F"

        override var introduction: String{
            return super.introduction + " " + "학점 : \(self.grade)"
        }

        override var koreanAge: Int{
            get{
                return super.koreanAge
            }
            set{
                self.age = newValue - 1
            }
        }
    }

    let my: Person = Person()
    my.name = "eui"
    my.age = 99
    my.koreanAge = 100 // 오류 발생
    print(my.introduction) // 이름 : my. 나이 : 99
    print(my.koreanAge) // 100

    let jin: Student = Student()
    jin.name = "jin"
    jin.age = 10
    jin.koreanAge = 11
    print(jin.introduction) // 이름 : jin. 나이 : 10 학점 : F
    print(jin.koreanAge) // 11
```

## 프로퍼티 감시자 재정의

    ● 상수 저장 프로퍼티나 읽기 전용 연산 프로퍼티는 프로퍼티 감시자를 재정의할 수 없다.
    ● 프로퍼티 접근자와 프로퍼티 감시자는 동시에 정의할 수 없다.

> <b> 프로퍼티 감시자 재정의
```Swift
    class Person{
        var name: String = ""
        var age: Int = 0{
            didSet{
                print("Person age : \(self.age)")
            }
        }
        var koreanAge = Int{
            return self.age + 1
        }

        var fullName: String{
            get{
                return self.name
            }
            
            set{
                self.name = newValue
            }
        }
    }

    class Student: Person{
        var grade: Stirng = "F"
        override var age: Int{
            didSet{
                print("Student age : \(self.age)")
            }
        }

            // 접근자, 감시자 동시 오버라이딩 불가
        override var koreanAge: Int{
            get{
                return super.koreanAge
            }
            
            set{
                self.age = newValue - 1
            }

            didSet{}
        }

        override var fullName: String{
            didSet{
                print("full Name : \(self.fullName)")
            }
        }
    }

    let my: Person = Person()
    my.name = "eui"
    my.age = 14
    my.fullName = "eui chan"
    print(koreanAge) // 15

    let jin: Student = Student()
    jin.name = "jin"
    jin.age = 14
    // person age : 14
    // student age : 14
    jin.koreanAge = 15
    // person age : 14
    // student age : 14
    jin.fullName = "kim jin"
    print(jin.koreanAge) // 15
```

## 서브스크립트 재정의

    ● 자식클래스에서 재정의하려는 서브스크립트라면 부모클래스 서브스크립트의 매개변수와 반환 타입이 같아야한다.

```Swift
    class School{
        var students: [Student] = [Student]()
         subscript(number: Int) -> Student{
            print("School")
            return students[number]
         }
    }

    class MiddelSchool: School{
        var middleStudents: [Student] = [Student]()
    
        //부모 클래스에서 상속 받은 서브스크립트 재정으
        override subsciprt(index: Int) -> Student{
            print("Middle School")
            return middleStudents[index]
        }
    }

    let university: School = School()
    university.students.append(Student())
    university[0] //School

    let middle: MiddleSchool = MiddleSchool()
    middle.middleStudents.append(Student())
    middle[0] // Middle School
```

## 재정의 방지
    ● 상속받는 자식클래스에서 몇몇 특성을 재정의할 수 없도록 제한하고 싶다면 재정의를 방지하고 싶은 특성 앞에 final 키워드 사용

```Swift
    class Person{
        final nar name: String = ""
        
        final func speak(){
            print("abc")
        }
    }

    final class Student: Person{
       
        //부모 클래스에서 final 사용하여 재정의 불가
        override var name: String{
            set{
                super.name = newValue
            }
            get{
                return "학생"
            }
        }

        // 재정의 불가
        override func speak(){
            print("student")
        }
    }

    // 부모 클래스 final 사용하여 상속 불가
    class UniversityStudent: Student{ }
```

## 클래스의 이니셜라이저 - 상속과 재정의
    ● 클래스에서는 지정 이니셜라이저와 편의 이니셜라이저로 역할을 구분한다.
    ● 클래스는 상속이 가능하므로 상속받았을 때 이니셜라이저를 어떻게 재정의하는지도 중요하다.

## 지정 이니셜라이저와 편의 이니셜라이저
    ● [지정 이니셜라이저]
        1. 필요에 따라 부모클래스의 이니셜라이저를 호출할 수 있다
        2. 클래스에 하나 이상 정의한다.
        3. 모든 클래스는 하나 이상의 지정 이니셜라이저를 갖는다. 조상클래스에서 지정 이니셜라이저가 자손 클래스의 지정 이니셜라이저 역할을 충분히 한다면 자손 클래스는 지정 이니셜라이저를 갖지 않을 수 있다.
    ●  [편의 이니셜라이저]
        1. 초기화를 쉽게 도와주는 역할을 한다.
        2. 지정 이니셜라이저를 자신 내부에서 호출한다.
        3. 지정 이니셜라이저를 사용하면 인스턴스를 생성할 때마다 전달인자로 초깃값을 전달해야 하지만 편의 이니셜라이저를 사용하면 항상 같은 값으로 초기화가 가능하다.

```Swift
    // 지정 이니셜라이저 정의
        init(매개변수) {
        초기화 구현
     }

     // 편의 이니셜라이저 정의
        conveniene init(매개변수들){
            초기화 구문
        }
```

## 클래스의 초기화 위임
    ● 자식클래스의 지정 이니셜라이저는 부모클래스의 지정 이니셜라이저를 반드시 호출해야 한다.
    ● 편의 이니셜라이저는 자신을 정의한 클래스의 다른 이니셜라이저를 반드시 호출해야 한다.
    ● 편의 이니셜라이저는 궁극적으로 지정 이니셜라이저를 반드시 호출해야 한다.
    ● 편의 이니셜라이저는 자신의 클래스에 구현된 이니셜라이저만 호출할 수 있으므로 부모클래스의 이니셜러이저는 호출할 수 없다.

```Swift
    class Person{
        var name: String
        var age: Int

        init(name: String, age: Int){
            self.name = name
            self.age = age
        }
    }

    class Student: Person{
        var major: String

        init(name: String, age: Int, major: String){
            self.major = "Swift"
            super.init(name: name, age: age)
        }

        convenience init(name: String){
            self.init(name: name, age: 7, major: "")
        }
    }
```

## 이니셜라이저 상속 및 재정의
    ● 이니셜라이저는 부모클래스의 이니셜라이저를 상속받지 않는다. -> 부모클래스의 이니셜라이저를 사용했을 때 자식클래스의 새로운 인스턴스가 완전하고 정확하게 초기화 되지 않는 상황을 방지하고자 함이다.
    ● 부모클래스의 이니셜라이저를 똑같은 이니셜라이저를 자식클래스에서 사용하고 싶다면 자식클래스에서 부모의 이니셜라이저와 똑같은 이니셜라이저를 구현해주면 된다.

```Swift
    class Person{
        var name: String
        var age: Int

        init(name: String, age: Int){
            self.name = name
            self.age = age
        }

        convenience init(name: String){
            self.init(name: name, age: -)
        }
    }

    class Student: Person{
        var major: String

        override init(name: String, age: Int){
            self.major = "Swift"
            super.init(name: name, age: age)
        }

        convenience init(name: String){
            self.init(name: name, age: 7)
        }
    }
```

```Swift
    // 부모클래스의 실패 가능한 이니셜라이저를 자식클래스에서 재정의하고 싶을 때는 실패 가능한 이니셜라이저로 재정의해도 된다. 또한 필요에 따라 실패하지 않는 이니셜라이저로 재정의해줄 수 있다.

    class Person{
        var name: String
        var age: Int

        init(){
            self.name = "Unknown"
            self.age = 0
        }

        init?(name: String, age: Int){

            if name.isEmpty{
                return nil
            }
            self.name = name
            self.age = age
        }

        init?(age: Int){
            if age < 0 {
                return nil
            }
            self.name = "Unknown"
            self.age = age
        }
    }

    class Student: Person{
        var major: String

        override init?(name: String, age: Int){
            self.major = "Swift"
            super.init(name: name, age: age)
        }

        override init(age: Int){
            self.major = "Swift"
            super.init()
        }
    }
```

## 이니셜라이저 자동 상속
    ● 특정 조건에 부합한다면 부모클래스의 이니셜라이저가 자동으로 상속된다.
        1. 자식클래스에서 별도의 지정 이니셜라이저를 구현하지 않는다면, 부모클래스의 지정 이니셜라이저가 자동으로 상속된다.

```Swift
    class Person{
        var name: String

        init(name: String){
            self.name = name
        }
        
        convenience init(){
            self.init(name: "Unknown")
        }
    }

    // 이니셜라이저 자동 상속
    class Student: Person{
        var major: String = "Swift"
    }

    // 부모 클래스의 지정 이니셜라이저 자동 상속
    let my: Person = Person(name: "eui")
    let john: Student = Student(name: "john")
    print(my.name) // eui
    print(john.name) // john

    // 부모 클래스의 편의 이니셜라이저 자동 상속
    let tree: Person = Person()
    let su: Student = Student()
    print(tree.name) // Unknown
    print(su.name) // Unknown

```

```Swift
    // 편의 이니셜라이저 자동 상속
    class Person{
        var name: String

        init(name: String){
            self.name = name
        }

         convenience init(){
            self.init(name: "Unknown")
         }
    }

    class Student: Person{
        var major: String

        override init(name: String){
            self.major = "Unknown"
            super.init(name: name)
        }

        init(name: String, major: String){
            self.major = major
            super.init(name: name)
        }
    }

    let tree: Person = Person()
    let su: Student = Student()
    print(tree.name) // Unknown
    print(su.name) // Unknown
```

```Swift
    // 편의 이니셜라이저 자동 상속 2
    class Person{
        var name: String

        init(name: String){
            self.name = name
        }

        convenience init(){
            self.init(name: "Unknown")
        }
    }

    class Student: Person{
        var major: String

        convenience init(major: String){
            self.init()
            self.major = major
        }

        override convenience init(name: String){
            self.init(name: name, major: "Unknown")
        }

        init(name: String, major: String){
            self.major = major
            super.init(name: name)
        }
    }

    let tree: Person = Person()
    let su: Student = Student(major: "Swift")
    print(tree.name) // Unknown
    print(su.name) // Unknown
    print(su.major) // Swift

    // 자신만의 편의 이니셜라이저 convenience init(major:)를 구현했지만 편의 이니셜라이저 자동 상속애는 아무런 영향을 미치지 않는다.


        // 편의 이니셜라이저 자동 상속3
    class UniversityStudent: Student{
        var grade: String = "A+"
        var description: String{
            return "\(self.name) \(self.major) \(self.grade)"
        }

        convenience init(name: String, major: String, grade: String){
            self.init(name: name, major: major)
            self.grade = grade
        }
    }

    let nova: UniversityStudent = UniversityStudent()
    print(nova.description) // Unknown Unknown A+

    let raon: UniversityStudent = UniversityStudent(name: "raon")
    print(raon.description) // raon Unknown A+

    let joker: UniversityStudent = UniversityStudent(name: "joker", major: "Programming")
    print(joker.description) // joker Programming A+

    let chope: UniversityStudent = UniversityStudent(name: "chop", major: "Computer", grade: "C")
    print(chope.description) // chope Computer C

    // Student 클래스를 상속받은 UniversityStudent 클래스는 부모 클래스의 이니셜라이저를 모두 자동 상속 받는다. 게다가 자신만의 편의 이니셜라이저를 구현했지만 자동 상속에는 영향에는 미치지 않는다.
```

## 요구 이니셜라이저
    ● required 수식어를 클래스의 이니셜라이저 앞에 명시해주면 이 클래스를 상속받은 자식 클래스에서 반드시 해당 이니셜라이저를 구현해주어야 한다. 즉, 상속받을 때 반드시 재정의해야 한다.
    ● 자식클래스에서 요구 이니셜라이저를 재정의할 때는 override 수식어 대신에 required 수식어를 사용한다.

```Swift
    class Person{
        var name: String
        required init(){
            self.name = "Unknown"
        }
    }

    class Student: Person{
        var major: String = "Unknwon"

        //지정 이니셜라이저 구현
        init(major: String){
        self.major = major
        super.init()
    }

    required init(){
        self.major = "Unknown"
        super.init()
        }
    }

    class UniversityStudent: Student{
        var grade: String

        // 자신의 지정 이니셜라이저 구현
        init(grade: String){
            self.grade = grade
            super.init()
        }
    }

    let su: Student = Student()
    print(su.major) // Unknown

    let my: Student = Student(major: "Swift")
    print(my.major) // Swift

    let john: UniversityStudent = UniversityStudent(grade: "A+")
    print(john.grade) // A+


    // 클래스는 자신만의 지정 이니셜라이저를 구현했다. 그래서 부모클래스의 이니셜라이저를 자동 상속받지 못했다.
```

```Swift
    // 자신의 클래스로부터 요구 이니셜라이저로 변경할 수도 있다.
    // requied override 명시 해주어 재정의됨과 동시에 요구 이니셜라이저가 될 것임을 명시한다.

    // 편의 이니셜라이저도 요구 이니셜라이저로 변경될 수 있다.
    // required convienience

    class Person{
        var name: String

        init(){
            self.name = "Unknwon"
        }
    }

    class Student: Person{
        var major: String = "Unknown"

        init(major: String){
            self.major = major
            super.init()
        }

        // 부모클래스의 이니셜라이저를 재정의함과 동시에 요구 이니셜라이저로 변경
        required override init(){
            self.major = "Unknown"
            super.init()
        }

        // 이 요구 이니셜라이저는 앞으로 계속 요구함
        required convenience init(name: String){
            self.init()
            self.name = name
        }
    }

    class UniversityStudent: Student{
        var grade: String

        init(grade: String){
            self.grade = grade
            super.init()
        }

        // student 클래스에서 요구했으므로 구현해야한다.
        required init(){
            self.grade = "F"
            super.init()
        }

        // student 클래스에서 요구했으므로 구현해야 한다.
        required convenience init(name: String){
            self.init()
            self.name = name
        }
    }

    let my: UniversityStudent = UniversityStudent()
    print(my.grade) // F

    let su: UniversityStudent = UniversityStudent(name: "su")
    print(su.name) // su

    // Student 클래스에서 Person의 init() 이니셜라이저를 재정의하면서 요구 이니셜라이저로 변경했다.
    // UniversityStudent 클래스에서 init() 이니셜라이저를 요구 이니셜라이저로 구현해야 한다.
    // Student 클래스의 init(name:) 요구 이니셜라이저로 지정되었기 때문에 UniversityStudent 클래스에서 다시 구현해주어야 한다.

```