# my Swift Study - Chapter3[데이터 타입 기본]

    ※ Swift는 데이터 타입에 굉장히 엄격하다

## Int vs UInt
    ● Int8, Int16, Int32, Int64, UInt8, UInt16, UInt32, UInt64 등
    ● UInt보다 Int 타입을 사용하는 것이 좋다
    ● Int와 UInt는 완전히 다르다

```Swift
//진수별 정수 표현
let decimalInterger: Int = 28 //10진수
let binaryInteger: Int = 0b11100 //2진수
let octalInteger: Int = 0o34 //8진수
let hexadecimalInteger: Int = 0x1c //16진수
```

## Bool
    ● true or false 값 가짐

```Swift
var boolean: Bool = true
boolean.toggle() //toggle() = 참, 거짓 반전
let you: Bool = false
let my: Bool = true
print(boolean, you, my)
//false, false, true
```
## Float vs Double
    ● 둘 중 하나 무엇을 사용해야 할지 모르는 상황이면 Double을 사용!!

```Swift
var floatValue: Float = 1234567890.1 //float이 수용할 수 있는 범위를 넘어선다

let doubleValue: Double = 1234567890. //double 충분히 수용
```
＊random 수 만들기
```Swift
Int.random(in: -100...100) //-100에서 100사이 만들기 단, Int 타입
UInt.random(in: 1...30) //1에서 30사이 만들기 단, UInt타입
Float.random(in: 1.5...4.3) //1.5에서 4.3사이 만들기 단, Float타입
Double.random(in: -0.5...1.5) //-0.5에서 1.5사이 만들기 단, Double타입
```

## Character
    ● 유니코드에서 지원하는 모든 언어 및 특수 기호 사용 가능
```Swift
let a: Character = "A"
let b: Character = "★"
let c: Character = "ㄱ"
//모든 문자 사용 가능
```

## String
    ● .append() => 문자열을 이어붙일 수 있다.
    ● .count() => 문자의 수를 셀 수 있다.
    ● .isEmpty() => 빈 문자열인지 확인 할 수 있다.
    ● .hasPrefix() => 접두어 확인
    ● .hasSuffix() => 접미어 확인
    ● .uppercased() => 대문자로 변환
    ● .lowercased() => 소문자로 변환
    ●  """ ~ """ => 세 개를 써주고 한 줄을 내려씀으로써 여러 줄 문자열 입력

```Swift
var writeCode: String = String() //빈 문자열 생성
writeCode.append("hi") //hi 문자열 추가
print(writeCode.count()) // writeCode 문자 수 출력 : 2
print(writeCode.isEmpty()) // 빈 문자열인지 확인 : false
print(writeCode.hasPrefix("h")) // 접두어 h인지 확인 : true
print(writeCode.hasSuffix("q")) // 접미어 q인지 확인 : false
print(writeCode.uppercased()) // HI
print(writeCode.lowercased()) // hi
```

## Any, Any Object
    ● Any : 모든 데이터 타입 사용
    ● Any Object : 클래스의 인스턴스 할당
    ● 되도록 사용하지 않는 편이 좋다 => Swift는 타입에 엄격하기 때문!!

```Swift
var someVar: Any = "hello"
someVar = 20 // Any타입이기 때문에 정수 할당 가능
someVar = 20.1 // 실수도 할당 가능
```

---
야곰 지음 - 스위프트 프로그래밍3판을 공부하고 작성하였음
---
