
# my Swift Study - Chapter16[모나드]
* 순서가 있는 연산을 처리할 때 자주 활용하는 디자인 패턴
* 모나딕이라고도 한다.
* 함수객체와 모나드는 특정 기능이 아닌 디자인 패턴 혹은 자료구조라고 할 수 있다.
* 모나드가 갖춰야 할 조건 ::

        + 타입을 인자로 받는 타입
        + 특정 타입의 값을 포장한 것을 반환하는 함수
        + 포장된 값을 변환하여 같은 형태로 포장하는 함수가 존재

## 컨텍스트

```Swift
    // 컨텍스트에 들어있지 않은 순수 값인 2를 전달하면 정상적으로 함수를 실행한다.
    func addThree(_ num: Int) -> Int{
        return num + 3
    }

    addThree(2) // 5
    // 옵셔널을 전달 인자로 사용하면 오류 발생!! ->> 순수한 값이 아닌 옵셔널이라는 컨텍스트로 둘러싸여 전달

```

## 함수객체

```Swift
    // 맵은 컨테이너 값을 변형시킬 수 있는 고차함수이다.
    // 옵셔널은 컨테이너와 값을 갖기 때문에 맵 함수를 사용할 수 있다.
    Optional(2).map(addThree) // Optional(5)

    // 옵셔널에 맵 메서드와 클로저의 사용
    var value: Int? = 2
    value.map{$0 + 3} // Optioal(5)
    value = nil
    value.map{$0 + 3} // nil

    // 1. 맵이 함수를 인자로 ㅂ다음
    // 2. 함수객체에 맵이 전달 받은 함수를 적용
    // 3. 새로운 함수 객채를 반환
```
## 모나드

    ● 닫힌 함수 객체 : 자신의 컨텍스트와 같은 컨텍스트의 형태로 맵핑할 수 있는 함수객체를 닫힌 함수객체라고 한다. => 모나드
    ●  컨텍스트에 포장된 값을 처리하여 값을 컨텍스트에 다시 반환하는 함수를 적용할 수 있다.
    ●  위와 같은 매핑의 결과가 함수객체와 같은 컨텍스트를 반환하는 함수객체를 모나드라고 한다.
    ●  플랫맵 메서드 활용한다.


> <b> 플랫맵 사용
```SWift
   func doubledEven(_ num: Int) -> Int? {
        if num.isMultiple(of: 2){
            return num * 2
    }
    return nil
    }

    Optional(3).flatMap(doubledEven) // nil

    // 맵과 다른점
    // 컨텍스트 내부의 컨텍스트를 모두 같은 위상으로 평평하게 펼쳐준다.
    // 포장된 값 내부의 포장된 값의 포장을 풀어서 같은 위상으로 펼침
```


> <b> 맵과 플랫맵(컴팩트)의 차이
```Swift
    let optinals: [Int?] = [1,2,nil,5]
    let mapped: [Int?] = optionals.map{$0}
    let compactMapped: [Int] = optionals.compactMap{$0}

    print(mapped) // [Optional(1), Optional(2), ...]
    print(compactMapped) // [1,2,5]
```

> <b> 중첩된 컨테이너에서 맵과 플랫맵의 차이
```Swift
    let multipleContainer = [[1,2,Optional.none], [3, Optional.none], [4,5,Optional.none]]

    let mappedMultipleContainer = multipleContainer.map{$0.map{$0}}
    let flatmappedMultipleContainer = multipleContainer.flatMap{$0.flatMap{$0}}

    print(mappedMultipleContainer)
    // [[Optional(1), Optional(2), nil], [Optional(3), nil, ...]]
    print(flatmappedMultipleContainer)
    // [1,2,3,4,5]

    // 플랫맵은 내부의 값을 1차원적으로 펼쳐놓는 작업도 하기 때문에 값을 꺼내어 모두 동일한 위상으로 펼쳐놓는 모양새를 갖춘다.
    
```

> <b> 플랫맵의 활용
```Swift
    // 스위프트에서 옵셔널에 관련된 여러 컨테이너의 값을 연달아 처리할 때, 바인딩을 통해 체인 형식으로 사용할 수 있기에 맵보다는 플랫맵이 더욱 유용하게 쓰인다.

   func stringToInteger(_ string: String) -> Int? {
    return Int(string)
   }

   func integerToString(_ integer: Int) -> String?{
    return "\(integer)"
   }

   var optionalString: String? = "2"

   let flattenResult = optinalString.flatMap(stringToInteger).flatMap(integerToString).flatMap(stringToInteger)

   print(flattenResult) // Optional(2)

   let mappedResult = optinalString.map(stringToInteger) // 체인 연결 불가
   print(mappedResult) //Optional(Optional(2))

   // 플랫맵을 사용하여 체인을 연결했을 때 결과는 옵셔널 타입이다.
   // 플랫맵은 항상 같은 컨텍스트를 유지할 수 있다.

    // 옵셔널 바인딩을 통한 연산
   var result: Int?
    if let string: String = optionalString{
        if let number: Int = stringToInteger(string){
            if let finalString: String = integerToString(number){
                if let finalNumber: Int = stringToInteger(finalString){
                    result = Optional(finalNumber)
                }
            }
        }
    }

    print(result) // Optional(2)

    if let string: String = optinalString,
        let number: Int = stringToInteger(string),
        let finalString: String = integerToString(number),
        let finalNumber: Int = StringToInteger(finalString){
            result = Optional(finalNumber)
        }

        print(result) //Optional(2)

    // 플랫맵 체이닝 중 빈 컨텍스트를 만났을 때의 결과
    func integerToNil(param: Int) -> String{
        return nil
    }

    optionalString = "2"

    result = optionalString.flatMap(stringInteger).flatMap(integerToNil).flatMap(stringToInteger)

    print(result) //nil

    // 중간에 Optional.none을 반환받기 때문에 이후에 호출되는 메서드는 무시한다.

    // 기본 모나드 타입이 아니더라도 플랫맵 모양의 모나드 연산자를 구현하면 사용자 정의 타입도 모나드로 사용할 수 있다.
    
```