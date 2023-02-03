# my Swift Study - Chapter2[스위프트 처음 시작하기]

## 기본 명령 규칙
    ● 명명 규칙은 없다
    ●  함수, 메서드, 인스턴스 => 첫 글자를 소문자로 사용(소문자 + 카멜케이스)
    ● 클래스, 구조체, 익스텐션, 프로토콜, 열거형 => 첫 글자를 대문자로 사용(대문자 카멜케이스)
 ## 콘솔 로그
    ● print() : 디버깅 콘솔에 간략한 정보 출력
    ● dump() : 조금 더 자세한 정보를 출력
   
```Swift
struct BasicInformation{
    let name: String
    let age : Int
}

var mySwift: BasicInformation = BasicInformation(name: "my", age: 25)

class Person{
    var height: Float = 177.2
    var weight: Float = 69.5
}

var yourSwift: Person = Person()
yourSwift.height = 175.1
yourSwift.weight = 78.5

print(mySwift) //BasicInformation(name: "my", age: 25)
dump(mySwift)
/*
BasicInformation
- name:"my"
- age:25
*/

print(yourSwift) //Person
dump(yourSwift)
/*
Person #0
- height = 175.1
- weight = 78.5
*/
```
## 문자열 보간법
    ● 문자열 내에 /(변수 or 상수)의 형태로 표기
```Swift
let name: String = "haha"
print("My name is \(name)")
// My name is haha
```

## 변수와 상수
    ● var(변수) : 생성 후 데이터값 변경 가능
    ● let(상수) : 한번 값을 설정하면 변경 불가능
    
    ex) [var or let] [이름]: [타입] = [값]

```Swift
// 변수 code
var name: String = "my"
var age: Int = 25
var job: String = "student"

age = 20
job = "백수"
print("내 이름은 \(name), 나이는 \(age), 직업은 \(job)")
// 내 이름은 my, 나이는 20, 직업은 백수
// age와 job이 바뀐 것을 확인 할 수 있다.
```
```Swift
//상수 code
let name: String = "my"
let age: Int = 25
let job: String = "student"

age = 20 //오류 발생
job = "백수" //오류 발생
print("내 이름은 \(name), 나이는 \(age), 직업은 \(job)")
// 내 이름은 my, 나이는 25, 직업은 student
// age와 job이 바뀌지 않은 것을 확인 할 수 있다.
```

---
야곰 지음 - 스위프트 프로그래밍3판을 공부하고 작성하였음
---
