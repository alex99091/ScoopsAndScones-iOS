# ScoopsAndScones-iOS

해당 앱은 VIP 패턴을 적용하기 위한 [Tutorial App](https://www.kodeco.com/29416318-getting-started-with-the-vip-clean-architecture-pattern#toc-anchor-001)으로 학습 목적으로 생성하였습니다. 모든 예제 코드와 내용에 대한 저작권은 원저자에게 있습니다. 

### How to run

```
> cd Tutorial
> open ScoopsAndScones.xcodeproj
```

### App Feature


### VIP (View-Interactor-Presenter) Pattern??

- VIP (View-Interactor-Presenter) 패턴은 Clean Architecture의 일부로, iOS 애플리케이션에서 사용되는 디자인 패턴 중 하나입니다. 
- VIP 패턴은 사용자 인터페이스와 비즈니스 로직을 분리하여 유지 보수성, 테스트 용이성 및 확장성을 향상시키는 장점이 있습니다.


- 다음은 각 VIP 구성 요소에 대한 설명입니다.

<pre>1. View: iOS 애플리케이션의 UI 요소를 나타내는 부분으로, 사용자 입력을 수신하고 Presenter로 전달합니다.
2. Interactor: 비즈니스 로직을 처리하는 부분으로, 데이터 모델을 관리하고 Presenter로 결과를 전달합니다.
3. Presenter: View와 Interactor 간의 중개자 역할을 합니다.  View로부터 사용자 입력을 받아 Interactor에 전달하고, Interactor로부터 데이터를 받아 View에 표시합니다. </pre>

- 이러한 VIP 패턴의 구성 요소들은 서로 간에 강한 의존성을 가지지 않도록 설계되었습니다.
- 이를 통해 단일 책임 원칙과 의존성 역전 원칙을 준수하면서, 클린하고 유지보수성 높은 애플리케이션을 개발할 수 있습니다.

