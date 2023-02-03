
# my Swift Study - Chapter14[옵셔널 체이닝과 빠른종료]

## 옵셔널 체이닝
    ● 옵셔널에 속해 있는 nil일지도 모르는 프로퍼티, 메서드, 서브스크립션 등을 가져오거나 호출할 때 사용할 수 있는 일련의 과정
    ● 옵셔널을 반복 사용하요 옵셔널이 자전거 체인처럼 서로 꼬리를 물고 있는 모양이기 떄문에 옵셔널 체이닝 이라고 한다.
    ● 중첩된 옵셔널 중 하나라도 값이 존재하지 않는다면 결과적으로 nil을 반환한다.
    ● 옵셔널이 nil이 아니라면 정상적으로 호출, nil이라면 결괏값으로 nil 반환

+ 옵셔널 느낌표(!)<br>
옵셔널에 값을 추출해 반환하기 때문에 반환 값이 옵셔널이 아니다!!

> <b>사람의 주소 정보 표현 설계
```SWift
    class Room{
        var number: Int //방 번호

        init(number: Int){
            self.number = number
        }
    }

    class Building{
        var name: String //건물 이름
        var room: Room? //건물의 방 정보

      init(name: String){
          self.name = name
      }
    }

    struct Address{ //주소
        var province: String //지역 광역시/도
        var city: String //지역 시/군/구
        var street: String //도로명
        var building: Building? //
        var detailAddress: String? //건물 외 상세 주소
    }

    class Person{
        var name: String
        var address: Address?

        init(name: String){
            self.name = name
        }
    }

    let myInstance: Person = Person(name: "my")

    let myRoomViaOptionalChaining: Int? = my.address?.building?.room?.number
    print(myRoomViaOptionalChaining) //nil
```

> <b>옵셔널 바인딩 사용
```Swift
    class Room{
        var number: Int //방 번호

        init(number: Int){
            self.number = number
        }
    }

    class Building{
        var name: String //건물 이름
        var room: Room? //건물의 방 정보

      init(name: String){
          self.name = name
      }
    }

    struct Address{ //주소
        var province: String //지역 광역시/도
        var city: String //지역 시/군/구
        var street: String //도로명
        var building: Building? //
        var detailAddress: String? //건물 외 상세 주소
    }

    class Person{
        var name: String
        var address: Address?

        init(name: String){
            self.name = name
        }
    }

    let myInstance: Person = Person(name: "my")
    var roomNumber: Int? = nil
    if let myAddress: Address = myInstance.address{ 
        if let myBuilding: Building = myAddress.building{
            if let myRoom: Room = myBuilding.room{
                roomNumber = myRoom.number
            }
        }
    }

    if let number: Int = roomNumber{
        print(number)
    } else{
        print("not room number")
    }

```

> <b>옵셔널 체이닝 사용
```Swift
    class Room{
        var number: Int //방 번호

        init(number: Int){
            self.number = number
        }
    }

    class Building{
        var name: String //건물 이름
        var room: Room? //건물의 방 정보

      init(name: String){
          self.name = name
      }
    }

    struct Address{ //주소
        var province: String //지역 광역시/도
        var city: String //지역 시/군/구
        var street: String //도로명
        var building: Building? //
        var detailAddress: String? //건물 외 상세 주소
    }

    class Person{
        var name: String
        var address: Address?

        init(name: String){
            self.name = name
        }
    }

    let myInstance: Person = Person(name: "my")

    if let roomNumber: Int = myInstance.address?.building?.room?.number{
        print(roomNumber) //nil
    } else{
        print("not room Number")
    }

    // 옵셔널 체이닝의 결괏값은 옵셔널이기 때문에 옵셔널 바인딩과 결합할 수 있다.
    // 옵셔널 체이닝을 통해 값을 받아오기만 하는 것이 아니라 반대로 값을 할당해줄 수 있다.
```

> <b>옵셔널 체이닝을 통한 값 할당 시도
```Swift
    class Room{
        var number: Int //방 번호

        init(number: Int){
            self.number = number
        }
    }

    class Building{
        var name: String //건물 이름
        var room: Room? //건물의 방 정보

      init(name: String){
          self.name = name
      }
    }

    struct Address{ //주소
        var province: String //지역 광역시/도
        var city: String //지역 시/군/구
        var street: String //도로명
        var building: Building?
        var detailAddress: String? //건물 외 상세 주소
    }

    class Person{
        var name: String
        var address: Address?

        init(name: String){
            self.name = name
        }
    }

    let myInstance: Person = Person(name: "my")
    myInstance.address?.building?.room?.number = 505
    print(myInstance.address?.building?.room?.number) //nil
    //이유 : address 프로퍼티가 없으므로 그 하위 프로퍼티도 없다

    myInstance.address = Address(province: "경기도", city: "수원시", street: "경수대로", building: nil, detailAddress: nil)
    myInstance.address?.building = Building(name: "타워")
    myInstance.address?.building?.room = Room(number: 0)
    myInstance.address?.building?.room?.number = 505

    print(myInstance.address?.building?.room?.number) //Optional(505)

    // 옵셔널 체인에 존재하는 프로퍼티를 실제로 할당해준 후 옵셔널 체이닝을 통해 값이 정상적으로 반환되는 것을 확인 할 수 있다.
```
> <B>옵셔널 체이닝을 통한 메서드 호출
```Swift
    struct Address{
        var province: String
        var city: String
        var street: String
        var building: Building?
        var detailAddress: String?

        init(province: String, city: String, street: String){
            self.province = province
            self.city = city
            self.street = street
        }

        func fullAddress() -> String{
            var restAddress: String? = nil

            if let buildingInfo: Building = self.building{
                restAddress = buildingInfo.name
            } else if let detail = self.detailAddress{
                restAddress = detail
                }
            if let rest: String = resetAddress{
                var fullAddress: String = self.province
                fullAddress += " "  + self.city
                fullAddress += " "  + self.street
                fullAddress += " "  + self.rest

                return fullAddress
            } else{
                return nil
            }
        }

        func printAddress(){
            if let address: String = self.fullAddress(){
                print(address)
            }
        }
    }

    myInstance.address?.fullAddress()?.isEmpty //false
    myInstance.address?.printAddress() // 충북 청주시 충청대로 타워
```

> <B>옵셔널 체이닝을 통한 서브스크립트 호출
```Swift
    // Array, Dictionary에서 서브스크립트를 가장 많이 사용한다.
    // 옵셔널의 서브스크립트를 사용하고자 할 때는 대괄호보다 앞에 물음표를 사용한다.


    let optionalArray: [Int]? = [1,2,3]
    optionalArray?[1] //2

    var optionalDictionary: [String: [Int]]? = [String: [Int]]()
    optionalDictionary?["numberArray"] = optionalArray
    optionalDictionary?["numberArray"]?[2] //3
```

## 빠른 종료
    ● guard 키워드를 사용한다. => if 구문과 유사하게 bool 타입의 값으로 동작
    ● guard 뒤에 따라붙는 코드의 실행 결과가 true일 때 코드가 계속 실행된다.
    ● 항상 else 구문이 뒤에 따라와야 한다.
    ● guard 뒤에 따라오는 Bool 값이 false라면 else의 블록 내부 코드를 실행한다.
    ● else 구문의 블록 내부에는 꼭 자신보다 상위의 코드 블록을 종료하는 코드가 들어간다.
    ● 코드 블록을 종료할 때 return, break, continue, throw 등의 제어문 전환 명령을 사용
    ● guard 뒤에 따라오는 옵셔널 바인딩 표현에서 옵셔널이 값이 있는 상태라면 guard 구문에서 옵셔널 바인딩된 상수를 guard 구문이 실행된 아래 코드부터 함수 내부의 지역상수처럼 사용
    

> <B>형태

```Swift
    guard Bool 타입 값 else{
        예외사항 실행문
        제어문 전환 명령어
    }
```

> if와 guard 차이

```Swift
    for i in 0...3{
        if i == 2{
            print(i)
        } else{
            continue
        }
    }

    for i in 0...3{
        guard if i == 2 else{
            continue
        }
        print(i)
    }
```

> guard 구문의 옵셔널 바인딩 활용

```Swift
    func greet(_ person: [String: String]){
        guard let name: String = person["name"] else{
            return
        }
        print("hellp \(name)!")

        guard let location: String = person["location"] else{
            print("I hope the weather is nice near you")
            return
        }

        print("I jope the weather is nice near you \(location)")
    }

    var personInfo: [String: String] = [String: String]()
    personInfo["name"] = "jenny"

    greet(personInfo) //hello jenny! I hope the weather is nice near you

    personInfo["location"] = "Korea"

    greet(personInfo) //hello jenny! I hope the weather is nice near you Korea
```

> 메서드 내부에서 guard 구문의 옵셔널 바인딩 활용
```Swift
    func fullAddress() -> String? {
        var restAddress: String? = nil

        if let buldingInfo: Building = self.building{
            restAddress = buildingInfo.name
        } else if let detail = self.detailAddress{
            restAddress = detail
        }

        guard let rest: String = restAddress else{
            return nil
        }

        var fullAddress: String = self.province
        fullAddress += " " + self.city
        fullAddress += " " + self.street
        fullAddress += " " + self.rest

        return fullAddress
    }
```

> guard 구문에 구체적인 조건 추가
```Swift
    func enterClub(name: String?, age: Int?){
        guard let name: String = name, let age: Int = age, age > 19, name.isEmpty == false else{
            return
        }
        print("welcome \(name)")
    }

    //guard 구문의 한계는 자신을 감싸는 코드 블록(return, block, continue, throw) 등의 제어문 전환 명령어를 쓸 수 없는 상황이라면 사용이 불가능하다.
```


---
야곰 지음 - 스위프트 프로그래밍3판을 공부하고 작성하였음
---