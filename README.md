# IOS Code Convention Giuide(Swift)

해당 문서는 **채널톡** ios팀 멤버들의 swift code convention을 맞추기 위해서 작성되었습니다. 팀원들의 합의에 따라서 언제든 바뀔 수 있으며, 가능한 해당 컨벤션을 맞춰주세요.
가능하다면 컨벤션 관련 코드리뷰때는 해당 깃헙에서 항목의 링크를 붙여주면 커뮤니케이션에 좋습니다.

다음 Reference들을 참고하여 맞게 수정하여 만들어졌습니다.([StyleShare](https://github.com/StyleShare/swift-style-guide), [raywenderlich](https://github.com/raywenderlich/swift-style-guide))

## TODO 리스트
- [ ] 현재까지 코드 컨벤션 추가하기
- [ ] 종류별로 구분, 순서 정리 및 목차 설정하기
- [ ] What is next?

## 코드 컨벤션

### 한 줄 최대 길이

- 한 줄은 최대 99자를 넘지 않도록 합니다.
- Xcode에서 **Preferences -> Text Editing -> Display -> Page guide at column** 부분을 99로 설정해서 사용해주세요.

### Guard 규칙

- `guard`는 코드에서 분기를 빨리 끝낼 때, 과도한 조건문 복잡도가 생길 때 사용합니다.
- `guard ~ else` 문이 한줄에 써진다면, 한줄로 둔다

  ```swift
    guard let count = count else { return }
  ```
  
- `guard`의 condition이 하나이고, `else`가 여러줄이라면 다음과 같이 사용합니다.

  ```swift
    guard let count = count else {
      ....
      return
    }
  ```
  
- `guard`의 condition이 여러개라면 다음과 같이 사용합니다.

  ```swift
    guard 
      let count = count,
      count > 0 
    else {
      ....
      return
    }
  ```
  
### final 규칙
  
- 더이상 상속이 일어나지 않는 class는 `final`을 붙여서 명시해줍니다.
  
  ```swift
    fianl class ChannelViewController {
      ...
    }
  ```
  
### 접근자 규칙

- class 내부에서만 쓰이는 변수는 `private`으로 명시해줍니다.
- `fileprivate`는 필요한 경우가 아니면 피하고, `private`으로 써줍니다.

  ```swift
    fianl class ChannelViewController {
      private var count = 0
      ...
    }
  
### 함수정의 줄내림 규칙

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
  
### Snapkit 관련 규칙

- superView에 대해서는 inset을, 다른 뷰에 대해서는 offset을 지향해주세요.
- layout warning이 나면 모두 없애주시길 바랍니다.
- right, left보단 leading, trailing을 사용해주세요.

  ```swift
    // Preferred
    self.channelView.snp.makeConstraints { make in
      make.leading.equalToSuperview().inset(xMargin)
      make.traling.equalToSuperview().inset(xMargin)
    }
    
    // Not Preferred
    self.channelView.snp.makeConstraints { make in
      make.left.equalToSuperview().offset(-xMargin)
      make.right.equalToSuperview().offset(xMargin)
    }
  ```

### 상수 선언 규칙
- 아래에 해당되는 값들은 class 아래 다음과 같이 따로 빼서 정리합니다.
  다른 class 에서 사용되지않는다면 `private`를 꼭 붙입니다.
  일반적인 상수를 단순히 정의할때는 `struct` 대신 `enum`을 사용해줍니다.
  
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
  
### 조건문 줄내림 규칙

- `if` 나 `guard`에서 조건문이 여러개인 경우 줄이 길면 다음과 같이 줄내림 해줍니다.
  - `,`로 나뉠 땐 인덴트를 한번 넣어줍니다.
  - `||`, `&&`는 한줄 내에서 줄내림 된거처럼 한번 더 인덴트를 넣어줍니다.

  ```swift
    if let test = test,
      let count = count,
      test > 0
        || count > 0
        || test == count,
      let check = check {
      ....
    }
    
    guard 
      let test = test,
      let count = count,
      test > 0
        || count > 0
        || test == count,
      let check = check {
      ....
    }
        
  ```
  
### 연산자 줄내림 규칙

- `+`, `||`, `&&` 등의 연산자에 대한 줄내림은 연산자를 같이 내려줍니다.

  ```swift
    // Preferred
    if count > 0
      || check == ture {
      ...
    }
    
    var text = "test"
      + " is"
      + " perfect"
      
    let isSuccess = !isEmpty
      && isValid
      && text.count > 5
    
    // Not Preferred
    if count > 0 ||
      check == ture {
      ...
    }
    
    var text = "test" + 
      " is" + 
      " perfect"
      
    let isSuccess = !isEmpty &&
      isValid && 
      text.count > 5
  ```
  
### 삼항연산자 규칙

- `if ~ else`로 묶인 `return` 또는 값대입인 경우 삼항연산자로 줄일 수 있으면 줄여줍니다.

  ```swift
    // Preferred    
    return test == 0 ? 0 : 1
    
    var result = count > 0 ? a : b
    
    // Not Preferred
    if test == 0 {
      return 0
    } else {
      return 1
    }
    
    var result = 0
    if count > 0 {
      result = a
    } else {
      result = b
    }
  ```

- 줄내림이 필요한 경우 `?`를 기준으로 내려줍니다.

  ```swift
    // Preferred
    return test == 0
      ? 0 : 1
      
    // Not Preferred
    return test == 0 ? 
      0 : 1
  ```
  
- 조건에 따른 단순 분기일 때는 삼항연산자를 피해줍니다.

  ```swift
    // Preferred
    if check == true {
      self.showSuccess()
    } else {
      self.showFail()
    }
      
    // Not Preferred
    check == true ? self.showSuccess() : self.showFail()
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
    var counts: [String:Int] = [:]
    
    // Not Preferred
    var manager = [Manager]()
    var counts = [String:Int]()
  ```
  
### 들여쓰기 규칙

- Indent는 2칸으로 지정합니다.
- Xcode에서 **Preferences -> Text Editing -> Display -> Line wrapping** 부분을 2 spaces로 설정해서 사용해주세요.

### 이니셜라이징 규칙

- 뷰나 객체들을 initialize할때 가능하면 `then`을 이용해서 초기에 해줄 수 있는 설정들은 최대한 해줍니다.

  ```swift
  let nameLabel = UILabel().then {
    $0.text = "manager name"
  }
  ```
  
### 메모리 관리 규칙

- Retain cycle이 발생하지 않도록 weak self를 이용합니다. 필요하다면 guard let self = self else 를 통해서 unwrapping을 해줍니다.

  ```swift
    self.closePopup() { [weak self] _ in
      guard let self = self else { return }

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
    self.channelView.snp.makeConstraints { make in
      make.leading.equalToSuperview().inset(xMargin)
      make.traling.equalToSuperview().inset(xMargin)
    }
    
    // Not Preferred
    self.channelView.snp.makeConstraints ({ make in
      make.left.equalToSuperview().offset(-xMargin)
      make.right.equalToSuperview().offset(xMargin)
    })
  ```

