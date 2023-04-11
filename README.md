# ScoopsAndScones-iOS

해당 앱은 `VIP 패턴`을 적용하기 위한 `튜토리얼 앱`으로 학습 목적으로 생성하였습니다. 

모든 예제 코드와 내용에 대한 저작권([링크](https://www.kodeco.com/29416318-getting-started-with-the-vip-clean-architecture-pattern#toc-anchor-001))은 원저자에게 있습니다. 

## How to run

```
> cd Finished
> open ScoopsAndScones.xcodeproj
```

## App Apperance
<div>
<img src="https://user-images.githubusercontent.com/111719007/230912116-8a8feb11-a879-46d1-9f20-882b9ac188f6.png" height="450"/>
<img src="https://user-images.githubusercontent.com/111719007/230912120-690af930-c55e-4dc2-98ea-2addf996aee9.png" height="450"/>
<img src="https://user-images.githubusercontent.com/111719007/230912121-cf86ae74-9619-456f-8c0b-c90986f93169.png" height="450"/>
</div>


## VIP (View-Interactor-Presenter)

<img src="https://user-images.githubusercontent.com/111719007/230911412-89dc28c0-4723-45bb-a8d3-2b8cea859453.png" height="350"/>

- `VIP 패턴`은 `클린 아키텍쳐`를 구현하는 방법중 하나로, `구성 요소`간의 명확한 `논리 분리`를 `강조`하는 `디자인 패턴`입니다.
- 주로 `많은 수의 뷰 컨트롤러`에서 `발생하는 문제`를 해결하기 위해 만들어졌습니다. 
- 뷰 컨트롤러는 코드가 유지되는 주요 객체가 되는 경우가 많아서 뷰 컨트롤러의 숫자가 많아질수록 코드의 복잡성이 증가하고, 유지보수가 어려워집니다. 
- 따라서, 해당 문제를 해결하기 위해 VIP패턴이 설계되었습니다.
- Redux, Flux, MVI 등 다른 패턴과 같이 `단방향 아케틱쳐 패턴`으로, View Interactor, Presenter의 구성 요소로 이루어져 있습니다.
- 단방향 아키텍처 패턴이란, 데이터 흐름이 일방향이며 각 구성 요소가 서로 분리되어 있어 변경에 대한 영향을 최소화하고 유지보수성을 향상시키는 패턴입니다.
- VIP는 각 구성 요소에 단일 책임을 부여하여 코드를 논리적으로 구성하며, 어디에서 무엇이 일어나고 있는지 쉽게 파악할 수 있습니다.
- 이러한 접근 방식은 사용자 `인터페이스`와 `비즈니스 로직`을 `분리`하여 `유지 보수성`, `테스트 용이성` 및 `확장성`을 `향상`시키는 `장점`이 있습니다.
- 다음은 각 VIP 구성 요소에 대한 설명입니다.

<pre>1. View: iOS 애플리케이션의 UI 요소를 나타내는 부분으로, 사용자 입력을 수신하고 Presenter로 전달합니다.
2. Interactor: 비즈니스 로직을 처리하는 부분으로, 데이터 모델을 관리하고 Presenter로 결과를 전달합니다.
3. Presenter: View와 Interactor 간의 중개자 역할을 합니다.  View로부터 사용자 입력을 받아 Interactor에 전달하고, Interactor로부터 데이터를 받아 View에 표시합니다. </pre>

- 해당 구성 요소들은 서로 간에 강한 의존성을 가지지 않도록 설계되었습니다.
- 이를 통해 `단일 책임 원칙`과 `의존성 역전 원칙`을 준수하면서, `클린`하고 `유지보수성 높은` 애플리케이션을 개발할 수 있습니다.

## How to apply

#### 모델 만들기

- VIP에 적용할 모델을 다음과 같이 생성하였습니다.

```Swift
enum CreateIceCream {
  enum LoadIceCream {
    struct Request {}

    struct Response {
      var iceCreamData: IceCream
    }

    struct ViewModel {
      var cones: [String]
      var flavors: [String]
      var toppings: [String]
    }
  }
}
```

### 뷰 만들기
→ 뷰는 요청 개체를 생성하고 인터렉터에게 보냅니다.

```Swift
// CreateIceCreamBusinessLogic 프로토콜을 통해 인터랙터를 연결합니다.

var interactor: CreateIceCreamBusinessLogic?
```

```Swift
// 구성 요소는 프로토콜을 통해 서로 통신하고, 각 구성 요소가 다른 구성 요소에 덜 밀접하게 연결됩니다.

func fetchIceCream() {
    let request = CreateIceCream.LoadIceCream.Request()
    interactor?.loadIceCream(request: request)
}
```

```Swift
// 엡의 보기에서 fetchIceCream()을 호출합니다.

.onAppear {
    fetchIceCream()
}
```

### 인터렉터 만들기
→ 인터렉터는 요청 객체를 받아 작업을 수행하고, 그 결과를 프리젠터에게 보냅니다.

```Swift
/* 오류를 수정하는 프리젠터를 생성합니다.
인터랙터는 프리젠터에게 응답을 전달하지만 누가 데이터를 어떻게 제시하는지 모릅니다.*/

protocol CreateIceCreamBusinessLogic {
  func loadIceCream(request: CreateIceCream.LoadIceCream.Request)
}

class CreateIceCreamInteractor {
  var presenter: CreateIceCreamPresentationLogic?
}
```

```Swift
/* 위의 코드에서 json 파일을 IceCream 도메인 모델로 디코딩하고 iceCream 내부에 저장합니다.
   인터랙터는 앱의 두뇌 역할로서 데이터 로드, 삭제 또는 저장과 같은 모든 비즈니스 로직을 처리합니다. */

extension CreateIceCreamInteractor: CreateIceCreamBusinessLogic {
  func loadIceCream(request: CreateIceCream.LoadIceCream.Request) {
    let iceCream = Bundle.main.decode(IceCream.self, from: "icecream.json")
    let response = CreateIceCream.LoadIceCream.Response(iceCreamData: iceCream)
    presenter?.presentIceCream(response: response)
  }
}
```

<img src="https://user-images.githubusercontent.com/111719007/230911423-1ee37a89-7cdf-4186-aea6-23b463e5634e.png" height="300"/>

→ Interactor에 추가할 수 있는 Worker라는 또 다른 구성 요소가 있는데, 각각 특정 로직을 처리하는 여러 작업자를 가질 수 있습니다.

→ 앱이 API에서 데이터를 가져온 경우 NetworkWorker를 만들고 내부에 모든 네트워킹 로직을 포함합니다. 

→ 예를 들어서, CoreData를 사용하여 데이터를 저장한 경우 CoreDataWorker를 통해 처리합니다.


### 프리젠터 만들기
→ 프리젠터는 응답을 받고 결과를 뷰에 다시 보냅니다.

```Swift
/* 뷰 내부에 CreateIceCreamDisplayLogic 프로토콜을 생성합니다. 
   프리젠터가 인터랙터의 데이터 형식을 지정하고 뷰에 전달할 수 있도록 뷰를 정의합니다. */

protocol CreateIceCreamPresentationLogic {
  func presentIceCream(response: CreateIceCream.LoadIceCream.Response)
}

class CreateIceCreamPresenter {
  var view: CreateIceCreamDisplayLogic?
}
```

```Swift
/* 프리젠터는 응답을 세 개의 문자열 배열로 형식화하고 이를 ViewModel에 전달하며, 
   뷰 모델은 형식이 지정된 데이터를 표시하기 위해 displayIceCream(viewModel:)에 전달됩니다.*/

extension CreateIceCreamPresenter: CreateIceCreamPresentationLogic {
  func presentIceCream(response: CreateIceCream.LoadIceCream.Response) {
    let viewModel = CreateIceCream.LoadIceCream.ViewModel(
    cones: response.iceCreamData.cones,
    flavors: response.iceCreamData.flavors,
    toppings: response.iceCreamData.toppings
    )
    view?.displayIceCream(viewModel: viewModel)
  }
}
```

```Swift
/* 디스플레이 로직 프로토콜을 생성하고, displayIceCream(viewModel:)을 정의합니다. 
   ViewModel의 데이터가 UI를 업데이트하는데 사용되는 @ObservedObject에 추가되어 뷰에 렌더링됩니다. */

protocol CreateIceCreamDisplayLogic {
  func displayIceCream(viewModel: CreateIceCream.LoadIceCream.ViewModel)
}

extension CreateIceCreamView: CreateIceCreamDisplayLogic {
  iceCream.displayedCones = viewModel.cones
  iceCream.displayedFlavors = viewModel.flavors
  iceCream.displayedToppings = viewModel.toppings
}
```


### 컨피규레이터 만들기
→ Configurator는 VIP 주기의 구성 요소를 인스턴스화 하고, 연결하여 단방향 사이클을 생성합니다.

→ 모든 Scene에서 하나의 Configurator를 한번만 호출하여 사용해야 합니다.

```Swift
extension CreateIceCreamView {
  func configureView() -> some View {
    var view = self
    let interactor = CreateIceCreamInteractor()
    let presenter = CreateIceCreamPresenter()
    view.interactor = interactor
    interactor.presenter = presenter
    presenter.view = view

    return view
  }
}
```


## Unit Testing

- 개발자가 작성한 소스 코드의 각각의 기능 또는 모듈이 예상대로 작동하는지를 확인하기 위해 개발자가 작성하는 코드 테스트 기법 중 하나입니다.
- 즉, 작성한 코드의 작은 부분, 즉 "유닛(unit)"을 독립적으로 테스트하여 해당 유닛이 올바르게 동작하는지 여부를 검증하는 것입니다.
- 프트웨어의 전체적인 품질을 향상시키는 데 도움이 되며, 버그를 조기에 발견하고 수정하여 개발자가 코드를 안정적으로 유지할 수 있도록 합니다.
- 일반적으로 자동화된 테스트 도구를 사용하여 수행하며, 코드 실행을 자동화하고, 예상되는 결과와 실제 결과를 비교하여 문제가 있는 부분을 찾아내는 등의 작업을 수행합니다.

### 유닛 테스트 적용하기

```
> cd Starter
> cd Test Classes
```

해당 폴더의 3개의 테스트 파일을 Scenes의 IceCream 파일로 아래와 같이 이동합니다.

<img src="https://user-images.githubusercontent.com/111719007/230911406-e446847d-bef0-400f-b89c-45aaa87bb7ba.png" height="300"/>


```Swift
/* 현재 테스트 중인 시스템은 CreateIceCreamView이고, 이 뷰는 인터렉터에 요청을 전달합니다.
   인터랙터는 구성 요소들의 의존성을 분리하기 위해 스파이 테스트 더블을 생성합니다. 
   스파이는 특정 유형의 모의 객체 또는 테스트 더블로, 테스트 중인 시스템에서 생성된 출력 또는 효과를 기록하여 예상대로 작동하는지 확인할 수 있습니다. 
   CreateIceCreamBusinessLogic 프로토콜을 준수하기 때문에, 해당 프로토콜에서 정의된 메서드를 테스트할 수 있습니다. */
   
class CreateIceCreamInteractorSpy: CreateIceCreamBusinessLogic {
  var loadIceCreamCalled = false

  func loadIceCream(request: CreateIceCream.LoadIceCream.Request) {
    loadIceCreamCalled = true
  }
}

func testShouldLoadIceCreamOnViewAppear() {
  // Given
  sut.interactor = interactorSpy
  // When
  sut.fetchIceCream()
  // Then
  XCTAssertTrue(
    interactorSpy.loadIceCreamCalled,
    "fetchIceCream() should ask the interactor to load the ice cream"
  )
}
```

Command-U로 테스트 스위트를 실행하면 다음과 같은 log를 확인할 수 있습니다.

<img src="https://user-images.githubusercontent.com/111719007/230911397-267a8b43-4707-4122-a928-27db7bc0402f.png" height="70"/>
