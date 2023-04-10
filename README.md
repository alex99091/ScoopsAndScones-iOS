# ScoopsAndScones-iOS

해당 앱은 VIP 패턴을 적용하기 위한 [Tutorial App](https://www.kodeco.com/29416318-getting-started-with-the-vip-clean-architecture-pattern#toc-anchor-001)으로 학습 목적으로 생성하였습니다. 모든 예제 코드와 내용에 대한 저작권은 원저자에게 있습니다. 

### How to run

```
> cd Tutorial
> open ScoopsAndScones.xcodeproj
```

### App Feature


### VIP (View-Interactor-Presenter) Pattern??

- VIP (View-Interactor-Presenter) 패턴은 Clean Architecture를 구현하는 방법중 하나로, 구성 요소간의 명확한 논리 분리를 강조하는 디자인 패턴입니다.
- VIP는 주로 Massive View Controllers(많은 수의 뷰컨)에서 발생하는 문제를 수정하기 위해 만들어졌습니다. 
- View Controller는 코드가 유지되는 주요 객체가 되는 경우가 많아서 View Controller가 많아질수록 코드의 복잡성이 증가하고, 유지보수가 어려워집니다. 
- 따라서, 해당 문제를 해결하기 위해 VIP패턴이 설계되었습니다.
- Redux, Flux, MVI 등 다른 패턴과 같이 단방향 아케틱쳐 패턴으로, View Interactor, Presenter의 구성 요소로 이루어져 있습니다.
- 단방향 아키텍처 패턴은 데이터 흐름이 일방향이며, 각 구성 요소가 서로 분리되어 있어서 변경에 대한 영향을 최소화하고 유지보수성을 향상시키는 패턴입니다.
- VIP는 각 구성 요소에 단일 책임을 부여하여 코드를 논리적으로 구성하며, 어디에서 무엇이 일어나고 있는지 쉽게 파악할 수 있습니다.
- 이러한 접근 방식은 사용자 인터페이스와 비즈니스 로직을 분리하여 유지 보수성, 테스트 용이성 및 확장성을 향상시키는 장점이 있습니다.
- 다음은 각 VIP 구성 요소에 대한 설명입니다.

<pre>1. View: iOS 애플리케이션의 UI 요소를 나타내는 부분으로, 사용자 입력을 수신하고 Presenter로 전달합니다.
2. Interactor: 비즈니스 로직을 처리하는 부분으로, 데이터 모델을 관리하고 Presenter로 결과를 전달합니다.
3. Presenter: View와 Interactor 간의 중개자 역할을 합니다.  View로부터 사용자 입력을 받아 Interactor에 전달하고, Interactor로부터 데이터를 받아 View에 표시합니다. </pre>

- 해당 구성 요소들은 서로 간에 강한 의존성을 가지지 않도록 설계되었습니다.
- 이를 통해 단일 책임 원칙과 의존성 역전 원칙을 준수하면서, 클린하고 유지보수성 높은 애플리케이션을 개발할 수 있습니다.

