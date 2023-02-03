# my Swift Study - Chapter4[데이터 타입 고급]
    ※ Swift는 서로 다른 타입끼리의 데이터 교환은 타입캐스팅을 거친다.

## 데이터 타입 안심
    ● Int 변수 -> Character 변수 할당 시 컴파일 오류
    ● 타입 확인 : Swift가 컴파일 시 타입을 확인

## 타입 추론
    ● 변수나 상수를 선언할 때 타입을 명시하지 않아도 컴파일러가 할당 기준값으로 타입 결정
```Swift
var name = "myName" // 할당 값을 추론하여 String 타입 선언
name = 100 // 오류 발생
```

## 타입 별칭
    ● 데이터 타입에 임의로 별칭 부여 가능
    ● 기본 타입 이름과 이후에 추가한 별칭 사용 가능
```Swift
typealias myInt = Int
typealias yourInt = Int
typealias myDouble = Double

let age: myInt = 20
var year: yourInt = 2002
var avg: myDouble = 81.6
//별칭에 입력된 타입으로 변수 타임 결정 
```

## 튜플
    ● 타입의 이름은 없다
    ● 지정된 데이터의 묶음
    ● 타입 이름은 따로 없으므로 일정 타입의 나열만으로 튜플 타입을 생성
```Swift
var person: (String, Int, Double) = ("my", 20, 172.4)
print(person.0, person.1 person.2) // "my", 20, 172.4 출력

person.1 = 30
person.2 = 170

print(person.0, person.1 person.2) // "my", 30, 170 출력
/*
요소가 없으므로 판단하기 어렵다 그렇기 때문에 요소를 넣어주자!!
*/
-------------------------------------------------------------

var person: (name: String, age: Int, height: Double) = ("my", 20, 172.4)
print(person.name, person.age, person.height) // "my", 20, 172.4 출력

-------------------------------------------------------------

typealias Person = (name: String, age: Int, heghit: Double)

let my: Person = ("my", 20, 172.4)

print(my) // 위와 같이 출력!!, 별칭으로 지정하여 튜플 사용할 수 있다.
```
## 배열
    ● 대괄호를 묶어 Array 타입을 표현
    ● firstIndex(of:) => 해당 요소의 인덱스 확인 가능
    ● append(_:) => 맨 뒤에 요소 생성
    ● insert(at:) => 중간에 요소 삽입
    ● remove(at:) => 해당 자리 요소 삭제 후 반환
    ● removeFirst => 첫 요소 삭제 후 반환
    ● removeLast => 마지막 요소 삭제 후 반환

```Swift
var names: Array<String> = ["hello", "hi", "my", "you"]
-------------------------------------------------------------
var names: [String] = ["hello", "hi", "my", "you"] // 위와 동일한 표현
-------------------------------------------------------------
var emptyArray: [Any] = [Any]() // 빈 배열 생성
var emwptyArray: [Any] = Array[Any]() // 위와 같음
-------------------------------------------------------------
print(names.firstIndex(of: "hi")) // hi가 있는 인덱스 출력 : 1
print(names.first) // 배열의 첫 요소 출력
print(names.last) // 배열의 마지막 요소 출력
-------------------------------------------------------------
let firstArray: String = names.removeFirst() // names 배열 첫 요소 반환 후 삭제
let lastArray: String = names.removeLast() // names 배열 마지막 요소 반환 후 삭제
let selectArray: String = names.remove(at:1) // 1 인덱스 요소 반환 후, 삭제

print(firstArray) // Hello
print(lastArray) // you
print(selectArray) // my

names.append(contentsOf: ["max","min"]) // 마지막 자리에 max와 min 추가
names.insert("haha", at:2) // 인덱스 2의 자리에 haha 삽입
```
## 딕셔너리
    ● 딕셔너리 안의 키는 중복 불가능
    ● removeValue(forKey : ) => 키에 해당하는 값 반환 후, 삭제
```Swift
typealias typeDictionary = [String: Int] // 별칭으로 타입 표현 가능
-------------------------------------------------------------
var testDictionary: Dictionary<String,Int> = Dictionary<String,Int>() // 빈 딕셔너리 생성
-------------------------------------------------------------
var testDictionary: [String: Int] = [String: Int]() // 위와 같은 표현
-------------------------------------------------------------
var testDictionary: typeDictionary = typeDIctionary() //위와 같은 표현
-------------------------------------------------------------
var testDictionary: [String: Int] = [:] //위와 같은 표현
-------------------------------------------------------------
var testDictionary: [String: Int] = ["name": 22, "hello": 23]
print(testDictionary.isEmpty()) // false
print(testDictionary.count()) // 2 출력
print(testDictionary.removeValue(forKey: "hello")) //23 출력 후. 삭제
print(tsetDictionary["hi", default: 0]) // hi 키에 값이 없으면 default 값 출력 
```

## 세트
    ● 순서가 중요하지 않거나 각 요소가 유일한 값
    ● 축약형이 없다
    ● insert(_:) => 요소 추가
    ● remove(_:) => 요소 삭제
    ● intersection() => 교집합
    ● symmetricDifference() => 여집합
    ● union() => 합집합
    ● subtraction() => 차집합
    ● isDisjoin(with: ) => 서로 배타적인지 확인
    ● isSubset(of: ) => 부분집합인지 확인
    ● isSuperset(of: ) => 전체집합인지 확인

```Swift
var names: Set<String> = Set<String>() //빈 집합 생성
-------------------------------------------------------------
var names: Set<String> = [] //위와 같음
-------------------------------------------------------------
var names: Set<String> = ["hello", "hi", "bye"]
names.insert("good") // ["hello", "hi", "bye", "good"]
print(names.remove("hi")) // hi를 반환하고, 삭제
-------------------------------------------------------------
let korean: Set<String> = ["one","two","three"]
let number: Set<String> = ["two", "three"]

let testOne: Set<String> = korean.intersection(number) // 두 집합의 교집합 반환
let testTwo: Set<String> = korean.symmetricDifference(number) //두 집합의 여집합 반환
let testThree: Set<String> = korean.union(number) //두 집합의 합집합 반환
let testFour: Set<String> = korean.subtraction(number) //두 집합의 차집합 반환

print(korean.isDisjoint(with: testTwo)) // true -> korean은 testTwo와 배타적?
print(number.isSubset(of: korean)) // true -> number는 korean의 부분집합?
print(testThree.isSuper(of: korean)) // true -> testThree는 korean의 전체집합?
```
## 컬렉션 임의의 요소 추출 및 뒤집기
    ● randomElement() => 임의 요소 출력
    ● shuffle() => 요소 임의로 섞기
```Swift
var array: [Int] = [1,2,3,4]
var set: Set<Int> = [1,2,3,4]
var dictionary: [String: Int] = ["a":1, "b":2, "c":3. "d":4]
var string: String = "String"

print(array.randomElement()) // 랜덤 요소 출력
print(array.shuffled()) //뒤죽박죽된!!  배열 출력 => 자신의 요소는 그대로 새로운 컬렉션에 저장
print(dictionary.shuffled()) // (키, 값) 튜플의 배열로 반환
print(string.shuffled()) // 위치 

array.shuffle() // 뒤죽박죽으로!! 섞기 => 자신의 요소 섞기
set.shuffle() // 오류 발생, 집합은 순서가 없기 때문에 섞지 못한다.
```

## 열거형
    ● 제한된 선택지를 주고 싶을 때
    ● 정해진 값 외에는 입력받고 싶지 않을 때
    ● 예상된 입력 값이 한정되어 있을 때
    ● 스위프트의 열거형은 각 항목이 그 자체로 고유의 값이 될 수 있다
    ● rawValue라는 프로퍼티를 통해 원시값 사용 가능

```Swift
enum School: String{
    case primary
    case elementary
    case middle = "중학교"
    case high
    case college
    case university
    case graduate
}

var highestEducation: School = School.university
-------------------------------------------------------------
var highestEducation: School = .university // 위와 같은 표현
-------------------------------------------------------------
highestEducation = .graduate // 같은 열거형 내 항목으로 변경 가능
-------------------------------------------------------------
var middleSchool: School = .middle
print(middleSchool.rawValue) // 중학교 출력
-------------------------------------------------------------
let primary = School(rawValue: "중학교") // 원시값에 해당하는 열거형 할당
let graduate = School(rawValue: "대학교") // nil
```

## 연관 값
    ● 소괄호를 사용하여 표현 할 수 있다.
    ● 열거형 내의 항목이 자신과 연관된 값을 가질 수 있다

```Swift
enum MainDish{
    case pasta(taste: String)
    case pizza(dough: String, topping: String)
    case chicken(withSauce: Bool)
    case rise
}

var dinner: MainDish = MainDish.pasta(taste: "크림")
dinner = .pizza(dough: "치즈크러스트", topping: "불고기")
dinner = .chicken(withSauce: true)
dinner = .rice
```
## 열거형의 응용
```Swift
enum PastaTaste{
    case cream, tomato
}
enum PizzaDough{
    case cheeseCrust, thin, original
}
enum PizzaTopping{
    case pepperoni, cheese, bacon
}
enum MainDish{
    case pasta(taste: PastaTaste)
    case pizza(dough: PizzaDough, topping:PizzaTopping)
    case chicken(whithSauce: Bool)'
    case rise
}

var dinner: MainDish = MainDish.past(taste: PastaTaste.cream)
dinner = MainDish.pizza(dough: PizzaDough.cheeseCrust, topping:PizzaTopping.pepperoni)
```

## 항목 순회
    ● 열거형에 포함된 모든 케이스 알아야 할 때 사용한다.
    ● :CaseIterable 사용
    ● 열거형에 .allCases 사용

```Swift
enum School: String, CaseIterable{
    case primary = "유치원"
    case middle = "중학교"
    case high = "고등학교"
}

let allCase: [School] = School.allCases // School 열거형의 값이 전부 입력된다.
```

## 순환 열거형
    ● 연관 값이 열거형 자신의 값이고자 할 때 사용
    ● indirect 키워드 사용

```Swift
// 특정 항목에 순환 열거형
enum ArithmaticExpression{
    case number(Int)
    indirect case addition(ArithmaticExpression, ArithmaticExpression)
    indirect case multiplication(ArithmaticExpression, ArihmaticExpression)
}

// 열거형 전체 순화 열거형
indirect enum ArithmaticExpression{
    case number(Int)
    case addition(ArithmaticExpression, ArithmaticExpression)
    case multiplication(ArithmaticExpression, ArihmaticExpression)
}
```
## 열거형 비교
    ● Comparable 사용

```Swift
enum alphabet: Comparable{
    case b
    case a
    case d
    case c
}
let testOne: alphabet = alphabet.a
let testTwo: alphabet = alphabet.b

if testOne > testTwo{
    print("testOne")
} else{
    print("testTwo")
} // b가 a보다 위에 있으므로 testTwo가 크다

enum Device: Comparable{
    case galaxyPhone(version: String)
    case galaxyTab(version: String)
    case notebook
}

var devices: [Device] = []
devices.append(Device.galaxyPhone(version: "22"))
devices.append(Device.notebook)
devices.append(Device.galaxyTab(version: "21"))

let sortedDevice: [Device] = devices.sorted()
print(sortedDevice)
// [Device.galaxyPhone(version: "22"), Device.galaxyTab(version: "21"),Device.notebook]
```

---
야곰 지음 - 스위프트 프로그래밍3판을 공부하고 작성하였음
---
