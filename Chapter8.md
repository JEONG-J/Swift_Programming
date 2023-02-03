
# my Swift Study - Chapter8[옵셔널]
    ※ 값이 있을 수도, 없을 수도 있다. 즉, 변수 또는 상수의 값이 nil 일 수도 있다.

## 옵셔널 사용
    ●  옵셔널 변수 또는 상수가 아니면 nil을 할당할 수 없다.
    ● 변수 또는 상수 데이터 타입 뒤에 ?를 붙여 표현
```Swift
    var myName: String
    myName = nil
    // 컴파일 오류
```

```Swift
//switch를 통한 옵셔널 값 확인
    let numbers: [Int?] = [2, nil, -4]

    for number in numbers{
        switch number{
            case .some(let value) where value < 0:
                print("negative \(value)")
            case .some(let value) where value > 10:
                print("large \(value)")
            case .some(let value):
                print("value \(value)")
            case .none:
                print("nil")
        }
    }
```

## 옵셔널 추출
    ● switch 구문을 통해 매번 확인하는 것은 불편!!
    ● 옵셔널 타입에서 값을 조금 더 안전하게 추출!!
    ● 옵셔널 값을 옵셔널이 아닌 값으로 추출

## 강제 추출
    ● 옵셔널 값의 뒤에 ! 표시
    ● 값이 nil이면 런타임 오류 발생

```Swift
    var myName: String? = "my"
    var you: String = myName!
    // 옵셔널이 아닌 값에 옵셔널 값 들어갈 수 없다. 그래서 추출해서 할당

    myName = nil
    you = myName // 오류

    if myName != nil{
        print(myName)
    } else{
        print("myName == nil")
    }
    // myName == nil
```

## 옵셔널 바인딩
    ● 옵셔널에 값이 있는지 확인할 때 사용
    ● 추출한 값을 일정 블록 안에서 사용할 수 있는 상수나 변수로 할당해서 옵셔널이 아닌 형태로 사용
    ● if 또는 while 구문 등과 결합하여 사용
    ● 쉼표를 사용해 한 번에 여러 옵셔널의 값 추출

```Swift
// 옵셔널 바인딩을 사용한 옵셔널 값 추출

    var myName: String? = "Jin"

    if let name = myName{
        print(myName)
    } else{
        print("my name == nil")
    }
    // Jin

    if var name = myName{
        name = "Que"
        print(name)
    } else{
        print("myName == nil")
    }
    // Que
```


```Swift
// 옵셔널 바인딩 사용한 여러 개의 옵셔널 값 추출

    var myName: String? = "Jin"
    var yourName: String? = nil

    if let name = myName, let friend = yourName{
        print(name, friend)
        //friend가 nil 이므로 실행 x
    }

    yourName = "weak"

    if let name = myName, let friend = yourName{
        print(name, friend)
        // Jin, weak
    }
```

## 암시적 추출 옵셔널
    ● 타입 뒤에 ? 대신 ! 사용
    ● 일반 값처럼 사용 가능, nil도 할당 가능
    ● nil일 때 접근을 시도하면 런타임 오류 발생

```Swift
    var myName: Stirng! = "Jin"
    print(myName)
    // Jin
    myName = nil

    if let name = myName{
        print(name)
    } else{
        print("myName == nil")
    }
    myName == nil

    myName.isEmpty // error 발생 -> nil 상태에서 접근 시도
```
---
야곰 지음 - 스위프트 프로그래밍3판을 공부하고 작성하였음
---