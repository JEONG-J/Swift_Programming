
# my Swift Study - Chapter17[서브스크립트]
* 클래스, 구조체, 열거형에는 컬렉션, 리스트, 시퀸스 등 타입의 요소에 접근하는 단축 문법인 서브스크립를 정의할 수 있다.
* 클래스와 구조체는 필요한 만큼 얼마든지 서브스크립트를 구현할 수 있다.
* 서브스크립트는 여러 개의 매개변수를 가질 수 있고, 매개변수 기본값을 가질 수 있다. 하지만 입출력 매개변수는 가질 수 없다.

## 서브스크립트 문법
    ● 서브스크립트는 인스턴스의 이름 뒤에 대괄호로 감싼 값을 써줌으로써 인스턴스 내부의 특정값에 접근할 수 있다.
    ● 연산 프로퍼티나 인스턴스 메서드 문법과 유사하다.
    ● subscript 키워드를 사용하여 정의한다.
    ● 접근자와 설정자를 사용할 수 있는 연산 프로퍼티의 형태와 유사하다.

> <b> 서브스크립트 정의 문법
```Swift
    subscript(index: Int) -> Int{
        get{
            //결괏값 반환
        }
        set{
            //설정자 역할
        }
    }

    // 읽기 전용 서브스크립트
    subscipt(index: Int) -> Int{
        get{
            // 적절한 서브스크립트 값 반환
        }
    }
        subscript(index: Int) -> Int{
            // 서브스크립트 결괏값 반환
        }
``` 

## 서브스크립트 구현
> <b> School클래스 서브스크립트 구현
```Swift
    struct Student{
        var name: String
        var number: Int
    }

    class School{
        var number: Int = 0
        var students: [Student] = [Student]()

        func addStudent(name:String){
            let student: Student = Student(name: name, number: self.number)
            self.students.append(student)
            self.number += 1
        }

        func addStudents(names: String...){
            for name in names{
                self.addStudent(name: name)
            }
        }

        subscript(index: Int = 0) -> Student?{
            if index < self.number{
                return self.students[index]
            }
            return nil
        }
    }

    let highSchool: School = School()
    highSchool.addStudent(names: "jin", "riahn", "moon")

    let aStudent: Student? = highSchool[1]
    print("\(aStudent?.number) \(aStudent?.name)")
        // Optional(1)
    print(highSchool[]?.name)
        // 매개변수 기본값 사용 : Optional("jin")
    
    // 서브스크립트의 index 매개변수가 매개변수 기본값을 0으로 갖지만 필요하지 않으면 매개변수 기본값이 없어도 상관없다.
```
## 복수 서브스크립트

> <b> 복수의 서브스크립트 구현
```SWift
    struct Student{
        var name: String
        var number: Int
    }

    class School{
        var number: Int = 0
        var students: [Student] = [Student]()

        func addStudent(name:String){
            let student: Student = Student(name: name, number: self.number)
            self.students.append(student)
            self.number += 1
        }

        func addStudents(names: String...){
            for name in names{
                self.addStudent(name: name)
            }
        }

            // 첫 번째 서브스크립트
        subscript(index: Int) -> Student?{
            get{
                if index < self.number{
                    return self.students[index]
                }
                return nil
            }

            set{
                guard var newStuent: Student = newValue else{
                    return
                }
                var number: Int = index

                if index > self.number{
                    number = self.number
                    self.number += 1
                }

                newStudent.number = number
                self.students[number] = newStudent
            }
        }
            // 두 번째 서브스크립트
        subscript(name: String) -> Int?{
            get{
                return self.students.filter{$0.name == name}.first?.number
            }

            set{
                guard var number: Int = newValue else{
                    return
                }

                if number > self.number{
                    number = self.number
                    self.number += 1
                }

                let newStudent: Student = Student(name: name, number: number)
                self.students[number] = newStudent
            }
        }
            // 세 번째 서브스크립트
        subscript(name: String, number: Int) -> Student?{
            return self.students.filter{ $0.name == name && $0.number == number}.first
        }
    }

    let highSchool: School = School()
    highSchool.addStudent(names: "jin", "riahn", "moon")

    let aStudent: Student? = highSchool[1]
    print("\(aStudent?.number) \(aStudent?.name)")
    

    print(highSchool["moon"]) // optional(2)
    print(highSchool["kim"]) // nil

    highSchool[0] = Student(name: "kim", number: 0)
    highSchool["jin"] = 1

    print(highSchool["riahn"]) // nil
    print(highSchool["jin"]) // Optional(1)
    print(highSchool["moon", 2]) // Optional(Student(name: "moon", number: 3))


    // 첫 번째 서브스크립트 : 학생의 번호를 전달받아 해당하는 학생이 있다면 Student 인스턴스를 반환하거나 특정 번호에 학생을 할당하는 서브스크립트
    // 두 번째 서브스크립트 : 학생의 이름을 전달받아 해당하는 학생이 있다면 반환하거나 특정 이름의 학생을 해당 번호에 할당
    // 세 번쨰 서브스크립트 : 이름과 번호를 전달받아 해당하는 학생이 있다면 찾아서 Student 인스턴스를 반환
```

## 타입 서브스크립트

    ● 인스턴스가 아니라 타입 자체에서 사용할 수 있는 서브스크립트이다.
    ● 타입 서브스크립트를 구현하려면 서브스크립트를 정의할 때 static 키워드를 붙여줘야 한다. 

> <b> 타입 서브스크립트 구현
```Swift
    enum School: Int{
        case elementary = 1, middle, high, university

        static subscript(level: Int) -> School? {
            return Self(rawValue: level)
        }
    }
```


---
야곰 지음 - 스위프트 프로그래밍3판을 공부하고 작성하였음
---