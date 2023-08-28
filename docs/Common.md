# 공통

## 목차
[한줄 최대 길이](#한-줄-최대-길이)</br>
[들여쓰기 규칙](#들여쓰기-규칙)</br>
[Guard 규칙](#guard-규칙)</br>
[final 규칙](#final-규칙) </br>
[접근자 규칙](#접근자-규칙)</br>
[함수정의 줄내림 규칙](#함수정의-줄내림-규칙)</br>
[Enum 줄내림 규칙](#enum-줄내림-규칙) </br>
[조건문 줄내림 규칙](#조건문-줄내림-규칙) </br>
[연산자 줄내림 규칙](#연산자-줄내림-규칙)</br>
[삼항연산자 규칙](#삼항연산자-규칙)</br>
[self 규칙](#self-규칙) </br>
[Array 선언 규칙](#array-선언-규칙) </br>
[메모리 관리 규칙](#메모리-관리-규칙) </br>
[클로저 사용 규칙](#클로저-사용-규칙)</br>
[Unwrapping 규칙](#unwrapping-규칙)</br>
[자주 사용되는 값 체크에 확장 변수 사용하기](#자주-사용되는-값-체크에-확장-변수-사용하기)</br>
[상수 선언 규칙](#상수-선언-규칙)</br>
[RxSwift 스케쥴러 지정 규칙](#rxswift-스케쥴러-지정-규칙)</br>
[VIPER 모듈 사이의 콜백 전달 규칙](#viper-모듈-사이의-콜백-전달-규칙)</br>

## 코드 컨벤션

### 한 줄 최대 길이

- 한 줄은 최대 120자를 넘지 않도록 합니다.
- Xcode에서 **Preferences -> Text Editing -> Display -> Page guide at column** 부분을 120로 설정해서 사용해주세요.

### 들여쓰기 규칙

- Indent는 2칸으로 지정합니다.
- Xcode에서 **Preferences -> Text Editing -> Display -> Line wrapping** 부분을 2 spaces로 설정해서 사용해주세요.

### Guard 규칙

- `guard`는 코드에서 분기를 빨리 끝낼 때, 과도한 조건문 복잡도가 생길 때 사용합니다.
- `guard ~ else` 문이 한줄에 써진다면, 한줄로 둔다
- `Swift 5.7`부터 Shorthand syntax 사용이 가능하여 Optional Binding에 단축 구문을 사용합니다.

  ```swift
    // Preferred
    var number: Int?
    guard let number else { return }
  
    // Not Preffered
    var number: Int?
    guard let number = number else { return }
  ```
  
- `guard`의 condition이 하나이고, `else`가 여러줄이라면 다음과 같이 사용합니다.

  ```swift
    guard let number else {
      ....
      return
    }
  ```
  
- `guard`의 condition이 여러개라면 다음과 같이 사용합니다. ( 단, 최대 한줄로 작성이 가능한 경우는 한줄로 작성합니다.  )

  ```swift
    // Preferred  
    guard let number, number > 0  else {
      ....
      return
    }
  
    guard 
      let name,
      let number,
      isFavorited
    else { return }
  
    // Not Preffered
    guard let name,
      let number,
      isFavorited
    else {
      return
    }
  ```
  
- `guard`가 끝난 이후, 한 줄 띄우고 코드를 작성합니다.

    ```swift
    guard let number else { return }
    
    if number > 0 {
      ...
    } else {
      ...
    }
  ```
  
- `guard`가 중간에 오는 경우, 위 아래로 한줄씩 띄우고 코드를 작성합니다.

    ```swift
    let number: Int? = 0
    
    guard let number else { return }
    
    if number > 0 {
      ...
    } else {
    ...
  }
  ```
  
  
### final 규칙

- 더이상 상속이 일어나지 않는 class는 `final`을 붙여서 명시해줍니다.
  
  ```swift
    final class Channel {
      ...
    }
  ```
  
### 접근자 규칙

- class 내부에서만 쓰이는 변수는 `private`으로 명시해줍니다.
- `fileprivate`는 필요한 경우가 아니면 피하고, `private`으로 써줍니다.

  ```swift
    final class Channel {
      private var number = 0
      ...
    }
  
### 함수정의 줄내림 규칙

- 함수 정의가 길 경우 다음과 같이 줄내림 합니다.

  ```swift
    // Preferred
    func changeChannel(
      name: String,
      number: Int,
      isFavorited: Bool
    ) {
      ...
    }
    
    // Not Preferred
    func changeChannel(
      name: String,
      number: Int,
      isFavorited: Bool) {
      ...
    }
  ```

### Enum 줄내림 규칙

- 모든 `case` 내의 코드가 한줄이고 return 문이라면 붙여서 사용합니다.
  ```swift
  // Preferred
  switch channelNumber {
  case .main: return 0
  case .sub: return 1
  }

  // Not Preferred
  switch channelNumber {
  case .main:
    return 0
  case .sub:
    return 1
  }
  ```

- 단, switch 문 case 내 로직이 포함되는 경우 아래와 같이 줄내림 후 한 줄 띄우고 다음 case를 작성합니다.
  ```swift
  // Preferred
  switch checkChannel {
  case .main:
    guard channel.number != 0 else { return }

    channel.changeChannel(number: 0)

  case .sub:
    channel.changeChannel(number: 1)
  }

  // Not Preferred
  switch checkChannel {
  case .main:
    guard channel.number != 0 else { return }

    channel.changeChannel(number: 0)
  case .sub:
    channel.changeChannel(number: 1)
  }
  ```
  
### 조건문 줄내림 규칙

- `if` 나 `guard`에서 조건문이 여러개인 경우 줄이 길면 다음과 같이 줄내림 해줍니다.
  - `,`로 나뉠 땐 인덴트를 한번 넣어줍니다.
  - `||`, `&&`는 한줄 내에서 줄내림 된거처럼 한번 더 인덴트를 넣어줍니다.

  ```swift
    if let currentNumber,
      let number = channel.number,
      currentNumber > 0
        || number > 0
        || currentNumber == number,
      let isFavorited = isFavorited {
      ....
    }
    
    guard 
      let currentNumber,
      let number = channel.number,
      currentNumber > 0
        || number > 0
        || currentNumber == number,
      let isFavorited = isFavorited {
      ....
    }
        
  ```
  
### 연산자 줄내림 규칙

- `+`, `||`, `&&` 등의 연산자에 대한 줄내림은 연산자를 같이 내려줍니다.

  ```swift
    // Preferred
    if number > 0
      || isFavorited == ture {
      ...
    }
    
    var name = "this"
      + " is"
      + " main"
      
    let isSuccess = !channel.isEmpty
      && isFavorited
      && channel.number > 0
    
    // Not Preferred
    if number > 0 ||
      isFavorited == ture {
      ...
    }
    
    var name = "this" + 
      " is" + 
      " main"
      
    let isSuccess = !channel.isEmpty &&
      isFavorited && 
      channel.number > 5
  ```

### 삼항연산자 규칙

- `if ~ else`로 묶인 `return` 또는 값대입인 경우 삼항연산자로 줄일 수 있으면 줄여줍니다.

  ```swift
    // Preferred    
    return number == 0 ? .main : .sub
    
    var channel = number == 0 ? .main : .sub
    
    // Not Preferred
    if number == 0 {
      return .main
    } else {
      return .sub
    }
    
    var result: Channel
    if number > 0 {
      result = .main
    } else {
      result = .sub
    }
  ```

- 줄내림이 필요한 경우 `?`를 기준으로 내려줍니다.

  ```swift
    // Preferred
    return number == 0
      ? .main : .sub
      
    // Not Preferred
    return number == 0 ? 
      .main : .sub
  ```
  
- 조건에 따른 단순 분기일 때는 삼항연산자를 피해줍니다.

  ```swift
    // Preferred
    if isFavorited {
      channel.turnOn()
    } else {
      channel.turnOff()
    }
      
    // Not Preferred
    isFavorited ? channel.turnOn() : channel.turnOff()
  ```

- 삼항 연산자가 너무 길어질 경우 가독성을 위해 분리해줍니다.

  ```swift
    // Preferred
    let firstCondition = c == 2 ? d : e
    let secondCondition = b == 1 : firstCondition : f
    return test == 0 ? a : secondCondition
    // 또는 적절히 if를 나눠서 구현해준다.
    
    // Not Preferred
    return test == 0
      ? a
      : b == 1
      ? c == 2
      : d
      : e
      : f
  ```

### self 규칙

- 클래스와 구조체 내부에서는 `self`를 명시적으로 표시해줍니다.
  
### Array 선언 규칙

- Array를 선언할 때는 다음과 같은 포맷을 지향합니다.

  ```swift
    // Preferred
    var managers: [Manager] = []
    var counts: [String: Int] = [:]
    
    // Not Preferred
    var managers = [Manager]()
    var counts = [String: Int]()
  ```
  
### 메모리 관리 규칙

- Retain cycle이 발생하지 않도록 `weak self`를 이용합니다. 필요하다면 `guard let self = self else`를 통해서 unwrapping을 해줍니다.

  ```swift
    self.closePopup() { [weak self] _ in
      guard let self else { return }
  
      self.popAllController()
    }
  ```
  
### 클로저 사용 규칙

- 클로저의 파라미터는 괄호를 빼고 사용합니다.

  ```swift
    // Preferred
    { manager, user in
      ...
    }
  
    // Not Preferred
    { (manager, user) in
      ...
    }
  ```
  
- 클로져가 파라미터중 하나만 있고, 마지막에 항목이 클로져라면 파라미터 명을 생략해줍니다.
  ```swift
    // Preferred
    UIView.animate(withDuration: 0.25) {
      ...
    }
    
    // Not Preferred
    UIView.animate(withDuration: 0.25, animations: { () -> Void in
      ...
    })
  ```
- 파라미터의 타입 정의는 가능하다면 생략해줍니다.

  ```swift
    // Preferred
    { manager, user in
      ...
    }
    
    // Not Preferred
    { (manager: Manager, user: User) -> Void in
      ...
    }
  ```
  
- 클로져 밖의 괄호는 가능한 생략해 줍니다.
  ```swift
    // Preferred
    self.channelView.snp.makeConstraints {
      $0.leading.equalToSuperview().inset(xMargin)
      $0.trailing.equalToSuperview().inset(xMargin)
    }
    
    // Not Preferred
    self.channelView.snp.makeConstraints ({
      $0.left.equalToSuperview().offset(-xMargin)
      $0.right.equalToSuperview().offset(xMargin)
    })
  ```

### unwrapping 규칙

- 최대한 force unwrapping은 피해줍니다. 옵셔널(`?`)의 경우는 optional chaining 등으로 풀어서 사용해주시고 `!`는 최대한 피해줍니다.

  ```swift
    // Preferred    
    func getResultText(with text: String?) -> String {
      if let resultText = text {
        return resultText
      }
     ...
    }
  
    // Not Preferred
    func getResultText(with text: String?) -> String {
      return text!
     ...
    }
  ```

### 자주 사용되는 값 체크에 확장 변수 사용하기

- `nil`이나 `0`과 같은 값을 체크할 때 정의된 확장 변수가 있다면 등호/부등호를 사용하는 대신 해당 확장 변수를 사용합니다.

  ```swift
    // Preferred
    if optionalValue.isNil { ... }
    if optionalValue.isNotNil { ... }
    if numberValue.isZero { ... }
    if optionalBoolValue.beTrue { ... }
    if optionalBoolValue.beFalse { ... }

    // Not Preferred
    if optionalValue == nil { ... }
    if optionalValue != nil { ... }
    if numberValue == 0 { ... }
    if optionalBoolValue == true { ... }
    if optionalBoolValue == false { ... }
  ```

### 상수 선언 규칙

- 코드 상단에 `private`로 정의하여 일반적인 상수를 단순히 정의할때는 `struct` 대신 `enum`을 사용해줍니다.
  Generic 사용 시 class 내부에서 static 선언이 어렵기 때문에 class 밖에서 사용합니다.
  
  - Snapkit 등에서 autolayout을 설정할 때 상수는 위쪽에 `Metric`으로 빼줍니다.
  - 여러번 쓰이는 폰트는 `Font`로 빼줍니다
  - 테이블뷰 등의 Section 관련은 `Section`으로 빼줍니다.
  - 테이블뷰 등의 row 관련은 `Row`로 빼줍니다.
  - 그외 내부적으로 쓰이는 상수는 `Constant`로 빼줍니다.

  ```swift
    private enum Metric {
      static let avatarLength = 3.f
      ...
    }
    
    private enum Font {
      static let titleLabel = UIFont.boldSystemFont(ofSize: 14)
      ...
    }
    
    private enum Section {
      static let managers = 0
      static let users = 1
      ...
    }
    
    private enum Row {
      static let name = 0 // managers 섹션의 0번째 row
      static let username = 0 // users 섹션의 0번째 row
      ...
    }
    
    private enum Constant {
      static let maxLinesWithOnlyText = 2
      ...
    }
  ```
- Localize 상수는 기존 상수 규칙과 다르게, enum - case로 정리합니다. 규칙은 다음과 같습니다.
   ```swift
   private enum Localized {
    case leavedThread
    case managersCount(String)
    ...
    
    var rawValue: String {
      switch self {
        case .leaveThread: return "thread.header.leave_thread".localized
        case .managersCount(let count): return "team_chat_info.manager_list.title".localized(with: count)
        ...
      }
    }
  }
  ```

### RxSwift 스케쥴러 지정 규칙

- `Observable`에 대해서 `subscribe(on:)`과 `observe(on:)` 함수를 호출해 어떤 어떤 스케쥴러에서 작동할지 지정할 수 있는데, 이를 `Observable`을 생성하는 곳이 아닌 생성된 `Observable`을 사용하는 곳에서 지정합니다.
- 자세한 내용은 ["SubscribeOn / ObserveOn 논의 정리"](https://www.notion.so/channelio/SubscribeOn-ObserveOn-dfd918eee039412ea3ac9d72d3da08fd?pvs=4)를 확인해주세요.

  ```swift
    // Preferred
    func someObservable() -> Observable<Void> { ... }

    someObservable()
      .subscribe(on: ConcurrentDispatchQueueScheduler(qos: .background))
      .observe(on: MainScheduler.instance)
      .subscribe { _ in 
        ...
      }

    // Not Preferred
    Observable
      .create { ... }
      .subscribe(on: ConcurrentDispatchQueueScheduler(qos: .background))
      .observe(on: MainScheduler.instance)
    ...
  ```

### VIPER 모듈 사이의 콜백 전달 규칙

- VIPER 모듈이 다른 VIPER 모듈을 생성하면서 생성한 모듈로부터 어떤 동작이 완료되었다는 콜백을 받고 싶을 때, 이를 클로져를 사용하기보다는 `PublishRelay`, `PublishSubject`와 같은 옵저버 타입을 전달하도록 합니다.

  ```swift
    // Preferred
    extension CreateValueRouter {
      static func createModule(createValueSignal: PublishRelay<Value>) {
        ...
      }
    }

    class CreateValueModuleClass {
      var createValueSignal: PublishRelay<Value>?
      ...
      func valueCreated(_ value: Value) {
        self.createValueSignal?.accept(value)
      }
    }

    // Not Preferred
    extension CreateValueRouter {
      static func createModule(valueCreated: (Value) -> Void) {
        ...
      }
    }

    class CreateValueModuleClass {
      var valueCreated: ((Value) -> Void)?
      ...
      func valueCreated(_ value: Value) {
        self.valueCreated?(value)
      }
    }
  ```