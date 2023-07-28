# UIKit

## 목차
[Snapkit 관련 규칙](#Snapkit-관련-규칙) </br>
[상수 선언 규칙](#상수-선언-규칙)</br>
[Metric 명명 규칙](#Metric-명명-규칙)</br>
[super 함수 호출시 줄내림 규칙](#super-함수-호출시-줄내림-규칙)</br>
[View property 이니셜라이징 규칙](#View-property-이니셜라이징-규칙)</br>
[Subview 추가 규칙](#Subview-추가-규칙)</br>

## 코드 컨벤션
  
### Snapkit 관련 규칙

- superView, safeArea에 대해서는 `inset`을, 다른 뷰에 대해서는 `offset`을 지향해주세요.
- layout warning이 나면 모두 없애주시길 바랍니다.
- `right`, `left`보단 `leading`, `trailing`을 사용해주세요.
- 상위 뷰와 관계를 설정할 때는 `equalToSuperview()`를 사용해주세요.
- 동적으로 레이아웃을 변경해야 할 때는 별개의 `Constraint` 참조를 생성하기보다는 해당 뷰에 대한 `updateConstraints` 함수를 호출해서 수정해주세요.


  ```swift
    // Preferred
    self.channelView.snp.makeConstraints {
      $0.leading.equalToSuperview().inset(xMargin)
      $0.trailing.equalToSuperview().inset(xMargin)
    }
  
    self.channelView.snp.makeConstraints {
      $0.directionalEdges().inset(xMargin)
    }

    self.channelView.snp.updateConstraints {
      $0.bottom.equalToSuperview().inset(keyboardHeight)
    }

    // Not Preferred
    self.channelView.snp.makeConstraints {
      $0.left.equalToSuperview().offset(-xMargin)
      $0.right.equalToSuperview().offset(xMargin)
    }
  
    self.channelView.snp.makeConstraints {
      $0.edges().inset(xMargin)
    }

    var constraint: Constraint?
    ...
    self.channelView.snp.makeConstraints {
      constraint = $0.bottom.equalToSuperview().inset(0).constraint
    }
    constraint?.update(inset: 10)
  ```

### Metric 명명 규칙

- Metric은 다음과 같은 규칙을 따릅니다.
    - 상수로서의 의미로 계산의 편의를 위해 양수로 지정합니다.(사용하는 곳에서 방향에 관련된 값이나 -를 붙여줍니다.)
    
  ```swift
    private enum Metric {
      static let imageViewTop = 3.f
    }
    
    self.imageView.snp.makeConstraints {
      $0.top.equalToSuperview().inset(-Metric.imageViewTop)
    }
  ```
  
  - View와 View의 관계를 명명에 표현하지 않습니다.
  - 해당 View에 대해 View 이름(선언한 변수명) 또는 대상을 prefix로, 
    (Top, Bottom, Leading, Trailing, center, centerX, centerY, Edge, Length, Width, Height)을 postfix로 
    붙여 나타내줍니다.  
  - Padding, Margin, Inset, Offset 이라는 단어는 붙이지 않습니다.
  - 두가지를 한번에 명명할때는 (Top -> Bottom -> Leading -> Trailing) 4개만 해당 순서대로 묶어서 표현하며, 4개에 모두 사용될시 Edge를 사용합니다.
  - 가로 세로 길이가 같다면 Size 대신에 Length로 표현해줍니다. 가로 세로길이가 다르다면 Width, Height로 합니다.
  
  ```swift
    // Preferred
    private enum Metric {
      static let imageViewTop = 3.f
      static let nameLabelTopBottom = 3.f
      static let avatarViewTopLeading = 3.f
      static let textViewLeadingTrailing = 3.f
      static let iamgeViewEdge = 6.f
      static let imageViewLength = 7.f
    }
    
    // Not Preferred
    private enum Metric {
      static let imageViewNameLabelMargin = 3.f
      static let imageViewTopBottomLeadingTrailing = 3.f
      static let imageViewSize = 7.f
    }
  ```
  
### super 함수 호출시 줄내림 규칙

- 어떤 클래스를 상속하는 클래스가 super의 함수를 호출할 때 바로 아래에 빈 라인을 추가합니다.

  ```swift
    // Preferred
    override func viewDidLoad() {
      super.viewDidLoad()

      self.callSomeFunction()
    }

    // Not Preferred
    override func viewDidLoad() {
      super.viewDidLoad()
      self.callSomeFunction()
    }
  ```

### View property 이니셜라이징 규칙

- 뷰나 객체들을 initialize할때 가능하면 `then`을 이용해서 초기에 해줄 수 있는 설정들은 최대한 해줍니다.

  ```swift
  let nameLabel = UILabel().then {
    $0.text = "manager name"
  }
  ```
  
### Subview 추가 규칙

- `UIView`나 `UIStackView`에 복수의 하위 뷰를 추가할 때 되도록이면 `addSubviews`, `addArrangedSubviews`를 사용합니다.

  ```swift
    // Preferred 
    self.containerView.addSubviews(self.titleView, self.imageView)
    self.stackView.addArrangedSubviews(self.topView, self.bottomView)

    // Not Preferred
    self.containerView.addSubview(self.titleView)
    self.containerView.addSubview(self.imageView)
    self.stackView.addArrangedSubviews(self.topView)
    self.stackView.addArrangedSubviews(self.bottomView)
  ```