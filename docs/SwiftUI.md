# SwitfUI

## 목차
[SubView 선언 규칙](#SubView-선언-규칙)</br>

## 코드 컨벤션

### SubView 선언 규칙

- `@ViewBuilder` 함수 사용보다 `struct`를 통해 SubView 선언합니다.
  - 부모뷰에 있는 `@State`를 변경하기 위해 `@ViewBuilder` 를 통해 함수 선언이 가능하지만, 위계를 명확하게 하기 위해 `@Binding`을 사용합니다.

```swift
  // Preferred
  struct SubView: View {
    @Binding private var isFavorited: Bool

    init(isFavorited: Binding<Bool>) {
      self._isFavorited = isFavorited
    }

    var body: some View {
      ...
    }
  }

  // Not Preferred
  @ViewBuilder 
  public func subView(title: String) -> some View {
    VStack {
      ...
      Button(title) { self.isFavorited.toggle() }
    }
  }
```

### SwiftUI 이름 규칙

- `SwiftUI`에서는 명확히 View가 아닌 이상 suffix 없이 사용합니다.

```swift
  // Preferred
  struct ChannelButton: View {
    ...
  }

  struct ChannelSwitch: View {
    ...
  }

  // Not Preferred
  struct ChannelButtonView: View {
    ...
  }

  struct ChannelSwitchView: View {
    ...
  }
```

### Spacer() 사용 규칙

- 단순한 뷰 크기 확장을 위한 경우 `Spacer()` 사용보다 `.frame()`를 사용합니다.
  - 수직 확장은 `maxHeight: .infinity`, 수평 확장은 `maxWidth: .infinity` 를 사용합니다.

```swift
// Preferred
  HStack {
    ...
  }
  .frame(maxWidth: .infinity, alignment: .leading)

// Not Preferred
  HStack {
    ...
    Spacer()
  }
```

### 뷰 Property 규칙

- 필수 Property는 init 시점에 적용합니다. 옵셔널한 Property는 ViewModifier함수로 참조합니다.
  - Button과 같이 액션이 따르는 View는 init 시점에 액션 함수를 받지만, 액션이 필수가 아닌 뷰에서는 ViewModifier함수로 옵셔널하게 관리합니다.

```swift
  // Required Action
  struct ChannelButton: View {
    private let title: String
    private let action: () -> ()
  
    init(title: String, action: @escaping () -> ()) {
      self.title = title
      self.action = action
    }
    ...
  }

  // Optional Action
  struct ChannelView: View {
    private let title: String
    private let optionalAction: (() -> ())?
  
    init(title: String) {
      self.title = title
    }
    ...

    func optionalAction(_ optionalAction: (() -> ())?) -> Self {
      var view = self
      view.optionalAction = optionalAction
      return view
    }
  }
```