
# my Swift Study - Chapter15[맵, 필터, 리듀스]

## 맵
    ● 자신을 호출할 때 매개변수로 전달된 함수를 실행하여 그 결과를 다시 반환해주는 함수
    ● 맵을 사용하면 컨테이너가 담고 있던 각각의 값을 매개변수를 통해 받은 함수에 적용한 후 다시 컨테인에 포장하여 반환한다.
    ● 기존 컨테이너의 값은 변경되지 않고 새로운 컨테이너가 생성되어 반환된다.


> <b> for-in 구문과 맵 메서드의 사용 비교
```SWift
    let nums: [Int] = [0,1,2,3,4]

    var doubledNums: [Int] = [Int]()
    var strings: [String] = [String]()

    //for 구문 사용
    for num in nums {
        doubledNums.append(num * 2)
        strings.append("\(num)")
    }

    print(doubledNums) // [0,2,4,6,8]
    print(string) //["0", "1", "2", "3", "4"]

    //map 메서드 사용
    doubledNumbers = nums.map({(number: Int) -> Int in return number * 2})

    strings = nums.map({number: Int} -> String in return "\(number)")
```

> <b> 클로저 표현의 간략화
```Swift
    let numbers: [Int] = [0,1,2,3,4]

    //기본 클로저 사용
    var doubledNumbers = numbers.map({number: Int -> Int in return number * 2})

    //매개변수 및 반환 타입 생략
    doubledNumbers = numbers.map({return $0 * 2})

    //반화 키워드 생략
    doubledNumbers = numbers.map({$0 * 2})

    // 후행 클로저 사용
    doubledNumbers = numbers.map { $0 * 2 }

    // 같은 기능을 여러 번 사용할 것이라면 하나의 클로저를 여러 map 메서드에서 사용하는 편이 좋다.
```

> <b> 클로저 반복 사용
```Swift
    let evenNum: [Int] = [0,2,4,6,8]
    let oddNum: [Int] = [0,1,3,5,7]
    let multiplyTwo: (Int) -> Int = {$0 * 2}

    let doubledEvenNumbers = evenNum.map(multiplyTwo)
    print(doubledEvenNumbers) // [0,4,8,12,16]

    let doubledOddNumbers = oddNumbers.map(multiplyTwo)
    print(doubledOddNumbers) // [0,2,6,10,14]

    //여러 컨테이너 타입에 모두 적용이 가능하다.
```

> <b>다양한 컨테이너 타입에서의 맵의 활용
```Swift
   let alphabetDictionary: [String: String] = ["a":"A", "b":"B"]

   var keys: [String] = alphabetDictionary.map {(tuple: (String, String) -> String in return tuple.0)}

   keys = alphabetDictionary.map{ $0.0 }

   let values: [String] = alphabetDictionary.map{ $0.1 }
   
   print(keys) // ["b":"B"]
   print(values) // ["a":"A"]

   var numberSet: Set<Int> = [1,2,3,4,5]
   let resultSet = numberSet.map{$0 * 2}
   print(resultSet) // [2,4,6,8,10]

   let optionalInt: Int? = 3
   let resultInt: Int? = optionalInt.map {$0 * 2}
   print(resultInt) // 6

   let range: ContableClosedRange = (0...3)
   let resultRange = range.map{$0 *2}
   print(resulRnage) // [0,2,4,6]
```

## 필터
    ● 컨테이너 내부의 값을 걸러서 추출하는 역할을 하는 고차함수
    ● 새로운 컨테이너에 값을 담아 반환
    ● 맵처럼 기존 콘텐츠를 변형하는 것이 아니라, 특정 조건에 맞게 걸러내는 역할을 할 수 있다.
    ● filter 함수의 매개변수로 전달되는 함수의 반환 타입은 bool
    ● 새로운 컨테이너에 포함될 항목이라고 판단하면 true, 포함하지 않으면 false를 반환
    
> <B>필터 메서드의 활용

```Swift
  let numbers: [Int] = [0,1,2,3,4,5]

  let evenNumbers: [Int] =  numbers.filter {(number: Int -> Bool in return number % 2 == 0)}
  print(evenNumbers) // [0,2,4]

  let oddNumbers: [Int] = numbers.filter{$0 % 2 == 1}
  print(oddNumbers) // [1,3,5]
```

> <B> 맵과 필터 메서드의 연계 사용

```Swift
    let numbers: [Int] = [0,1,2,3,4,5]

    let mappedNumbers: [Int] = numbers.map{$0 + 3}

    let evenNumbers: [Int] = mappedNumbers.filter {( number: Int) -> Bool in return number % 2 == 0}

    print(evenNumbers) // [4,6,8]

    //여러 번 사용할 필요 없이 메서드를 체인처럼 연결하여 사용할 수 있다.

    let oddNumbers: [Int] = numbers.map{$0 + 3}.filter{$0 % 2 ==1}
    print(oddNumbers) // [3,5,7]

    // 맵과 필터를 연결하여 복잡한 연산도 손쉽게 실행할 수 있다.
```

## 리듀스
    ● 컨테이너 내부의 컨텐츠를 하나로 합하는 기능을 실행하는 고차함수
    ● 배열이라면 배열의 모든 값을 전달인자로 전달받은 클로저의 연산 결과로 합한다.

> <B> 첫 번째 리듀스 사용
```Swift
    public func reduce<Result>(_ initialResult: Result, _ nextPartialResult: (Result, Element) throws -> Result) rethrows -> Result

    // initialResult 이름의 매개변수로 전달되는 값을 통해 초기값을 지정해 줄 수 있다.
    // nextPartialResult 이름의 매개변수로 클로저를 전달받는다.
    // nextPartialResult 클로저의 첫 번째 매개변수는 리듀스 메서드의 initialResult 매개변수를 통해 전달받은 초깃값 또는 이전 클로저의 결괏값이다.
```

> <B> 첫 번째 리듀스 사용
```Swift
    public func reduce<Result>(into initialResult: Result, _ updateAccumulatingResult: (inout Result, Element) throws -> ()) rethrows -> Result

    // updateAccumulatingResult 매개변수로 전달받는 클로저의 매개변수 중 첫 번째 매개변수를 inout 매개변수로 사용한다.
    // updateAcccumulatingResult 클로저의 첫 번째 매개 변수는 리듀스 메서드의 initialResult 매개변수를 이용해 전달받은 초깃값 또는 이전에 실행된 클로저 때문에 변경되어 있는 결괏값이다.
```


> <B> 리듀스 메서드의 사용
```Swift
    let numbers: [Int] = [1,2,3]

    // 첫 번째 형태인 reduce(_:_:) 메서드의 사용

    // 초깃값이 0이고 정수 배열의 모든 값을 더한다.

    var sum: Int = numbers.reduce(0, {(result: Int, next: Int) -> Int in print("\(result) + \(netx)")
    return result + next})
    // 0 + 1
    // 1 + 2
    // 3 + 3

    print(sum) // 6

    // 초깃값이 0이고 정수 배열의 모든 값을 뺌
    let subtract: Int = numbers.reduce(0, {(result: Int, next: Int) -> Int in print("\(result) - \(next)")
    return result - next})
    // 0 - 1
    // -1 - 2
    // -3 - 3

    print(subtract) // -6

    // 초깃값 3이고 정수 배열의 모든 값을 더함
    let sumFromThree: Int = numbers.reduce(3) {print("\($0) + \($1)")
    return $0 + $1}
    // 3 + 1
    // 4 + 2
    // 6 + 3

    print(sumFromThree) // 9

    // 초깃값이 3이고, 정수 배열의 모든 값을 뺌
    var subtractFromThree: Int = numbers.reduce(3){print("\($0) - \($1)")
    return $0 - $1}
    // 3 - 1
    // 2 - 2
    // 0 - 3

    print(subtractFromThree) // -3

    //문자열 배열을 reduce(_:_:) 메서드를 이용해 연결시킴
    let names: [String] = ["Python", "Java", "Swift"]

    let reduceNames: String = names.reduce("language : "){
        return $0 + ", " + $1
    }

    print(reduceNames) // language : , Python, Java, Swift"

    // 두 번째 향태인 reduce(into:_:) 메서드의 사용

    // 초깃값이 0이고 정수 배열의 모든 값을 더한다.
    // 첫 번째 리듀스 형태와 달리 클로저의 값을 반환하지 않고 내부엣 직접 이전 값을 변경한다
    sum = numbers.reduce(into: 0, {(result: inout Int, next: Int) in print("\(result) + \(next)") result += next})
    print(sum) //6

    // 위와 같은 방법이지만 초깃값이 3이고 정수 배열의 모든 값을 뺀다
    subtractFromThree = numbers.reduce(into: 3 {print("\($0) - \($1)")
    $0 -= $1})

    print(subtractFromThree) // -3

    // 첫 번째 리듀스 형태와 다르기 때문에 다른 컨테이너에 값을 변경하여 넣어줄 수 있다.
    // 이렇게 하면 맵이나 필터와 유사한 형태로 사용할 수 있다.

    // 홀수는 걸러내고 짝수만 두 배로 변경하여 초깃값인 [1,2,3] 배열에 직접 연산한다.
    var doubledNumbers: [Int] =  numbers.reduce(into: [1,2]) {(result: inout [Int], next: Int) in print("result: \(result) next: \(next)")
    guard next.is else{
        return
    }
    
    print("\(result) append \(next)")
    result.append(next * 2)}

    print(doubledNumbers) //[1,2,4]

    // 필터와 맵을 사용한 모습
    doubledNumbers = [1,2] + numbers.filter{$0.isMultiple(of:2)}.map{$0 * 2}
    print(doubledNumbers) // [1,2,4]

    // 이름을 모두 대문자로 변환하여 초깃값인 빈 배열에 직저 연산한다.
    var upperCasedNames: [String]
    upperCasedNames = names.reduce(into: [] {$0.append($1.uppercased())})

    print(upperCasedNames) // ["PYTHON", "JAVA", "SWIFT"]

    // 맵을 사용한 모습
    uppercaseNames = names.map{$0.uppercased()}
```



> <B> 맵, 필터, 리듀스 메서드의 연계 사용
```Swift
    let numbers: [Int] = [1,2,3,4,5,6,7]

    //짝수를 걸러내어 각 값에 3을 곱해준 후 모든 값을 더한다.

    var result: Int = numbers.filter{$0.isMultiple(of:2)}.mpa{$0 * 3}.reduce(0){$0 + $1}
    print(result) // 36

    // for ~ in 구문 사용 시
    result = 0

    for number in numbers{
        guard number.isMultiple(of:2) else{
            continue
        }
        result += number * 3
    }

    print(result) // 36
```
