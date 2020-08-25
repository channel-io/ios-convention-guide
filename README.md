# IOS Code Convention Giuide(Swift)

해당 문서는 **채널톡** ios팀 멤버들의 swift code convention을 맞추기 위해서 작성되었습니다. 팀원들의 합의에 따라서 언제든 바뀔 수 있으며, 가능한 해당 컨벤션을 맞춰주세요.

## TODO 리스트
- [ ] 현재까지 코드 컨벤션 추가하기
- [ ] 종류별로 구분, 순서 정리 및 목차 설정하기
- [ ] What is next?

### 코드 컨벤션

## 한 줄 최대 길이

- 한 줄은 최대 99자를 넘지 않도록 합니다.
- Xcode에서 **Preferences -> Text Editing -> Display -> Page guide at column** 부분을 99로 설정해서 사용해주세요.

## Guard 규칙

- `guard`는 코드에서 분기를 빨리 끝낼 때, 과도한 조건문 복잡도가 생길 때 사용합니다.
- `guard ~ else` 문이 한줄에 써진다면, 한줄로 둔다

  ```swift
    guard let count = count else { return }
  ```
  
- `guard`의 condition이 하나이고, `else`가 여러줄이라면 다음과 같이 사용합니다.

  ```swift
    gurad let count = count else {
      ....
      return
    }
  ```
  
- `guard`의 condition이 여러개라면 다음과 같이 사용합니다.

  ```swift
    gurad 
      let count = count,
      count > 0 
    else {
      ....
      return
    }
  ```
  
## final 규칙
  
- 더이상 상속이 일어나지 않는 class는 `final`을 붙여서 명시해줍니다.
  
  ```swift
    fianl class ChannelViewController {
      ...
    }
  ```
  
## 접근자 규칙

- class 내부에서만 쓰이는 변수는 `private`으로 명시해줍니다.

  ```swift
    fianl class ChannelViewController {
      private var count = 0
      ...
    }
  
## 함수정의 줄내림 규칙

- 함수 정의가 길 경우 다음과 같이 줄내림 합니다.

  ```swift
    // Preferred
    func ChannelFunction(
      a: String,
      b: Int,
      c: Member
    ){
      ...
    }
    
    // Not Preferred
    func ChannelFunction(
      a: String,
      b: Int,
      c: Member){
      ...
    }
  ```
  
## Array 선언 규칙

- Array를 선언할 때는 다음과 같은 포맷을 지향합니다.

  ```swift
    // Preferred
    var managers: [Manager] = []
    var counts: [String:Int] = [:]
    
    // NOt Preferred
    var manager = [Manager]()
    var counts = [String:Int]()
  ```
