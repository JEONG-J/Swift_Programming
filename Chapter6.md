
# my Swift Study - Chapter6[조건문]
    ※ if, switch, guard 조건문이 있다.
## if 구문
    ● 다른 언어와는 달리 스위프트의 if 구문은 조건의 값이 꼭 Bool 타입!!
    ● if 키워드 뒤에 따라오는 조건수식을 소괄호로 감싸주는 것은 선택 사항

```Swift
    let first: Int = 5
    let second: Int = 7

    if first > second {
        print("first > second")
    } else if first < second {
        print("first < second ")
    } else {
       print("first == second")
    }
```
## switch
    ● 소괄호 생략 가능
    ● break 키워드 없이도 switch 구문이 종료
    ● switch 구문의 case를 연속 실행하려면 `fallthrough` 키워드 사용
    ● case에 들어갈 비교 값은 입력 값과 데이터 타입이 같아야 한다.
    ● 명확히 한정적인 값이 아닐 경우 `default`를 작성해줘야 한다.
    ● `where절`을 사용할 수 있다.

```Swift
// Int switch

    let integerValue: Int = 5

    switch integerValue{
    case 0:
        print("value == zero")
    case 1...10:
        print("value == 1~10")
        fallthrough
    case Int.min..<0, 101..<Int.max:
        print("value < 0 or value > 100")
    default:
        print("10 < value <= 100")
    }
    // value == 1~10
    // value < 0 or value > 100
```

```Swift
// String switch

    let stringValue: String = "Joker"

    switch stringValue {
        case "hello":
            print("He is hello")
        case "jay"
            print("He is jay")
        case "Joker"
            fallthrough
        case "Nova"
            print("he or she is \(stringValue)")
        default:
            print("not human")
    }

    // he or she is Joker
```

```Swift
// tuple switch
    typealias NameAge = (name: String, age: Int)

    let tupleValue: NameAge = ("hello", 99)

    switch tupleValue{
        case ("hello", 99):
            print("o")
        default:
            print("x")
    }
    // o
```

```Swift
// 와일드 카드(_) 사용한 tuple switch
    typealias NameAge = (name: String, age: Int)

    let tupleValue: NameAge = ("hello", 99)

    switch tupleValue{
        case ("hello", 50):
            print("o")
        case ("hello", _):
            print("이름만 맞춤")
        case (_, 99):
            print("나이만 맞춤")
        default:
            print("who?")
    }

    // 이름만 맞춤
```

```Swift
// 값 바인딩을 사용한 tuple switch
// 무시된 값을 직접 가져와야 하는 불편함 해소
    typealias NameAge = (name: String, age: Int)

    let tupleValue: NameAge = ("hello", 99)

    switch tupleValue{
        case ("hello", 50):
            print("o")
        case ("hello", let age):
            print("이름만 맞춤 나이는 \(age)")
        case (let name, 99):
            print("나이만 맞춤 이름은 \(name)")
        default:
            print("who")
    }

    // 이름만 맞춤
```

```Swift
// 열거형을 입력 값으로 하는 switch
    enum School{
        case primary, elementary, middle, high, college, university, graduate
    }

    let 최종학력: School.university

    switch 최종학력{
        case .primary:
            print("primary")
        case .elementary:
            print("elementary")
        case .middle:
            print("middle")
        case .high:
            print("high")
        case .college:
            print("college")
        case .university:
            print("university")
        case .graduate:
            print("graduate")
        @unknown case _: // 열거형에 case가 추가될 경우, 모든 case에 대응할 수 있도록 경고를 함
            print("학력이 뭐에요??")
    }
    // university
```

## 반복문
    ● for-in 구문, 딕셔너리 값은 넘겨받는 값의 타입이 튜플로 지정되어 넘어온다.
    ● while 구문
    ● repeat-while 구문

```Swift
// 간단한 for 구문
    for i in 0...5{
        if i.isMultiple(of: 2){
            print(i)
            continue
        }
        print("\(i) is odd")
    }
```
```Swift
    let friend: [String: Int] = ["jay" : 35, "jojo" : 22]

    for tuple in friend{
        print(tuple)
    }
    // ("jay", 35)
    // ("jojo", 22)

    let local: [String: String] = ["aaa" : "111", "bbb" : "222", "ccc" : "333"]

    for (key, value) in local{
        print(key, value)
    }

    // aaa 111
    // bbb 222
    // ccc 333
```

```Swift
// while 구문
  var number: [Int] = [1,2,3,4,5,6]

  while number.isEmpty == false{
      print(number.removeFirst())
  }
  // 1 2 3 4 5 6
```

```Swift
// repeat while 구문
    var number: [Int] = [1,2,3,4,5,6]

    repeat{
        print(number.removeFirst())
    } while number.isEmpty == false
    // 1 2 3 4 5 6
```

## 구문 이름표
    ● 반복문 중첩으로 사용 시 실수 방지
    ● 제어 키워드와 함께 구문 이름표를 함께 써준다.

```Swift
    var number: [Int] = [1,3434,2,52343]

    numberLoop: for num in numbers{
        if num > 5 || num < 1{
            continue numberLoop
        }

        var count: Int = 0

        printLoop: while true{
            print(num)
            count += 1

            if count == num{
                break printLoop
            }
        }

        removeLoop: while true{
            if number.first != num{
                break removeLoop
            }
            number.removeFirst()
        }
    }
    // 1
    // 2
    // 2
    // number = [3434,1,52343]
```
---
야곰 지음 - 스위프트 프로그래밍3판을 공부하고 작성하였음
---