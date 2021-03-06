## 15장 아키텍쳐란

소프트웨어 아키텍처란? 소프트웨어 아키텍트가 하는 일은?\
소프트웨어 아키텍트는 최고의 프로그래머이며, 앞으로 계속 프로그래밍 작업을 맡을 뿐만 아니라 나머지 팀원들이 생산성을 극대화 할 수 있는 설계를 하도록 방향을 이끌어준다.\
소프트웨어 아키텍처란 시스템을 구축했던 사람들이 만들어낸 시스템의 형태로, 그 모양은 컴포넌트 분할, 배치, 의사소통 방식에 따라 정해진다. 또한 이 형태는 아키텍처 안에 담긴 소프트웨어가 쉽게 개발, 배포, 운영, 유지보수 되도록 만들어진다.\
이런 일을 용이하게 만들기 위해서는 가능한 많은 선택지를 가능한 오래 남겨두는 전략을 따라야 한다. \
아키텍처는 시스템이 제대로 동작하는걸 최우선 목표 중 하나로 지원해야 하지만 보통은 시스템의 동작 여부와는 거의 관련이 없다.\
아키텍처의 주된 목적은 시스템의 생명 주기를 지원하는 것이다. 좋은 아키텍처는 시스템 이해, 개발, 유지보수, 배포를 쉽게 해준다. 아키텍처의 궁극적인 목표는 시스템의 수명과 관련된 비용은 최소화 하고, 프로그래머의 생산성은 최대화 하는 것이다.

### 개발

팀 규모가 작다면 모놀리틱 시스템을 개발할 수 있고, 보통 상위 구조로 인한 장애물이 없기를 바라기 때문에 아키텍처 없이 시작할 수 있다.\
여러 팀이 시스템을 함께 개발하고 있으면 컴포넌트 단위로 잘 분리하지 않으면 개발이 진척되지 않는다. 이 시스템의 아키텍처는 팀별 단일 컴포넌트로 발전될 가능성이 높지만, 이 아키텍처가 최적일 가능성은 거의 없다.

### 배포

아키텍처는 시스템을 단 한 번에 쉽게 배포할 수 있도록 만드는 데 목표를 둬야 한다.\
안타깝지만 초기 개발 단계에선 배포전략을 거의 고려하지 않고, 때문에 배포하기 어려운 아키텍쳐가 만들어 질 수 있다.\
초기에 MSA 를 사용하기로 결정해도, 배포할 시기가 되면 너무 많은 마이크로 서비스를 발견할 수도 있다. 이들을 서로 연결하기 위해 설정하는 과정에서 오작동이 발생할 소지가 있다.

### 운영

운영에서 겪는 대다수의 어려움은 아키텍처보다 하드웨어 투입으로 해결할 수 있다.\
아키텍처가 비효율적이라면 스토리지나 서버 추가 만으로 제대로 동작하도록 만들 수 있을 때가 많다. → 하드웨어는 값싸고 인력은 비싸다.\
비용 공식 관점에선 운영보다 개발, 배포, 유지보수쪽이 더 중요하다.\
좋은 아키텍처는 시스템을 운영하는데 필요한 요구사항도 알려준다.

### 유지보수

모든 측면에서 소프트웨어 시스템에서 비용이 가장 많이 발생한다.\
유지보수의 가장 큰 비용은 탐사(spelunking)와 이로 인한 위험부담에 있다.\
탐사 - 새로운 기능 추가 또는 결함을 수정할때 어떤 방법으로 작업하는게 최선인지를 결정할 때 드는 비용\
좋은 아키텍처는 이 비용을 크게 줄여준다.

### 선택사항 열어두기

소프트웨어는 행위적 가치와 구조적 가치를 지닌다. 이 중에서 두번째 가치가 더 중요하다. 그 이유는 소프트웨어를 소프트하게 만드는 것이 구조적 가치이기 때문이다.\
소프트웨어를 부드럽게 유지하는 방법은 선택사항을 가능한 한 많이, 오랫동안 열어두는 것이다. 열어둬야 할 선택사항은 중요치 않은 detail 이다.\
모든 소프트웨어 시스템은 정책과 세부사항의 두가지 구성요소로 분해할 수 있다.\
정책 - 업무 규칙, 절차를 구체화\
세부사항 - 사람, 외부 시스템, 프로그래머가 정책과 소통할때 필요한 요소로, 입출력장치, DB, 웹시스템, 서버, 프레임워크 등이 있다.\
아키텍트의 목표는 정책을 핵심 요소로 식별하고 세부사항은 정책에 무관하게 만들 수 있ㄴ는 형태의 시스템을 구축하는 것이다. 이를 통해 세부사항을 결정하는 일은 미루거나 연기할 수 있다.\
ex ) 개발 초기에는 DB 를 선택할 필요가 없다. 신중한 아키텍트라면 고수준의 정책을 DB 종류와는 관련이 없도록 만들어야 한다.\
좋은 아키텍트는 결정되지 않은 사항의 수를 최대화 한다.

### 장치 독립성

코드를 입출력 장치를 제어하는 명령어들과 직접 결합해버린 예제 → 장치 종속적 코드\
장치를 바꾸었을 때 코드를 다시 작성해야 하는 문제가 있었다.\
1960 후반 장치독립성 등장\
오늘날의 운영체제는 입출력 장치를 소프트웨어 함수로 추상화 했고, 동일한 프로그램을 아무런 변경 없이도 여러 장치를 통해 읽고 쓸수 있게 되었다. (개방 폐쇄 원칙)

### 광고 우편

정책 - 이름, 주소 레코드 포맷 / 세부사항 - 장치 인 프로그램 예제

### 물리적 주소 할당

소프트웨어가 디스크 드라이브의 실린더, 섹터 등의 정보를 직접 이용하는 예제 → 새로운 디스크 드라이브로 업그레이드 해야 한다면 하드코딩된 모든 코드를 수정해야 함.\
주소 할당 체계를 변경하여 상대주소를 사용하도록 해결 → 고수준의 정책이 디스크의 물리적 구조로부터 독립되도록 수정

### 결론

좋은 아키텍트는 세부사항을 정책으로부터 신중하게 가려내고, 정책이 세부사항과 결합되지 않도록 분리. 세부사항에 대한 결정을 가능한 한 오랫동안 미룰 수 있는 방향으로 정책을 설계해야 한다.

---

## 16장 독립성

좋은 아키텍처는 다음을 지원해야 한다.\
시스템의 유스케이스 / 운영 / 개발 / 배포

### 유스케이스

시스템의 아키텍처는 시스템의 의도를 지원해야 한다.\
시스템이 장바구니 앱이라면 장바구니 관련된 유스케이스를 지원해야 한다.\
좋은 아키텍처가 행위를 지원하기 위해 할 수 있는 일 중 가장 중요한 사항은 행위를 명확히 하고 외부로 드러내며, 이를 통해 시스템이 지닌 의도를 아키텍처 수준에서 알아볼 수 있게 만드는 것이다.\
좋은 아키텍처를 가진 장바구니 앱은 장바구니 앱처럼 보일것이다.

### 운영

시스템이 초당 10만건을 처리해야 한다면, 아키텍처는 이 요구와 관련된 각 유스케이스에 걸맞은 처리량과 응답시간을 보장해야 한다.\
시스템에 따라 다르게 지원될 수 있는데, 어떤 시스템에서는 시스템의 처리요소를 작은 서비스들로 배열하여 서로 다른 많은 서버에서 병렬로 실행할 수 있게 만들어야 함을 의미하고, 다른 시스템에서는 경량의 수많은 스레드가 같은 프로세스에서 돌도록 만든다는 뜻일수도 있다.\
아키텍처에서 컴포넌트를 적절히 분리하고, 통신 방식을 특정형태로 제한하지 않는다면, 운영에 필요한 요구사항이 바뀌더라도 쓰레드, 프로세스, 서비스로 구성된 기술 스펙트럼 사이를 전환하는 일이 훨씬 쉬워질 것이다.

### 개발

콘웨이의 법칙 - 시스템을 설계하는 조직이라면 어디든지 그 조직의 의사소통 구조와 동일한 구조의 설계를 만들어 낼 것이다.\
많은 팀이 협업하는 경우 각 팀이 독립적으로 행동하기 편한 아키텍처를 확보하여 팀들이 서로를 방해하지 않도록 해야한다.

### 배포

좋은 아키텍처라면 시스템이 빌드된 후 즉각 배포할 수 있도록 지원해야 한다.\
이러한 아키텍처를 만드려면 컴포넌트로 잘 분할해야 하는데, 여기엔 마스터 컴포넌트도 포함된다. 마스터 컴포넌트는 시스템 전체를 하나로 묶고 각 컴포넌트를 올바르게 구동하고 통합하고 관리해야 한다.

### 선택사항 열어놓기

현실에서는 위 관심사들간의 균형을 잡기가 매우 어렵다. 우리가 도달하려는 목표는 뚜렷하지 않고, 시시각각 변하기 때문이다.\
이러한 변화속에서도 몇몇 아키텍처 원칙들은 가성비가 좋으며 균형을 잡는데 도움이 된다.

### 계층 결합 분리

아키텍트는 유스케이스 전부를 알진 못하지만 시스템의 기본적인 의도는 분명히 알고있다.\
따라서 단일 책임 원칙과 공통 폐쇄 원칙을 적용해 의도의 백락에 따라서 서로 다른 이유로 변경되는 것들은 분리하고, 동일한 이유로 변경되는 것들은 묶는다.\
서로 다른 이유로 변경되는 예 - 사용자 인터페이스와 업무 규칙 / DB, 쿼리 언어, 스키마 등과 업무 규칙\
시스템을 서로 결합되지 않은 수평적인 계층으로 분리할 수 있고, 예로 UI, 앱에 특화된 업무규칙, 앱과 독립적인 업무규칙, DB 등이 있다.

### 유스케이스 결합 분리

서로 다른 이유로 변경되는 것에는 유스케이스 자체도 있다. 예로, 주문을 추가하는 것과 삭제하는 유스케이스.\
유스케이스는 시스템의 수평적인 계층을 가로지르도록 자른 수직으로 좁다란 조각이기도 하다.\
따라서 시스템을 수평적 계층으로 분할하면서 동시에 해당 계층을 가로지르는 얇은 수직적인 유스케이스로 시스템을 분할할 수 있다.\
유스케이스들이 각 계층에서 서로 겹치지 않게 잘 분리해야 한다.

![image](https://user-images.githubusercontent.com/5154845/139529785-3825cbb2-c4ba-4d9b-afb3-4854ee6db685.png)


### 결합 분리 모드

이렇게 결합을 분리하면 운영 관점에서 높은 처리량을 보장해야 하는 유스케이스와 낮은 처리량으로도 충분한 유스케이스는 이미 분리되어있을 가능성이 높다.\
운영측면에서 이점을 살리기 위해선 결합을 분리할 때  적절한 모드를 선택해야 한다. 분리된 컨포넌트를 서로 다른 서버에서 실행해야 하면, 서로 독립된 서비스가 되어야 한다.\
이런 컴포넌트를 서비스 또는 마이크로 서비스라고 한다. 구분 기준은 모호한 면이 있다.\
핵심은 우리는 때때로 컴포넌트를 서비스 수준까지도 분리해야 한다는 것이다.

### 개발 독립성

컴포넌트가 분리되면 팀 사이의 간섭은 줄어든다.\
계층과 유스케이스의 결합이 분리되는 한 아키텍처는 그 팀 구조를 뒷받침 해줄 것이다.

### 배포 독립성

유스케이스와 계층의 결합이 분리되면 배포 측면에서도 고도의 유연성이 생긴다. \
제대로 분리했다면 운영중인 시스템에서 계층과 유스케이스를 hot swap 할수도 있다.

### 중복

일반적으로 소프트웨어에서 중복은 나쁜것이다.\
진짜 중복 - 한 인스턴스가 변경되면, 동일한 변경을 그 인스턴스의 모든 복사본에 적용해야 함\
거짓된 또는 우발적인 중복 - 중복으로 보이는 두 코드가 각자의 경로로 발전하는 경우\
두 유스케이스의 화면 구조가 매우 비슷하다면 코드를 통합하고 싶을 것이다. 하지만 이는 우발적 중복일 가능성이 높으므로 통합하지 않는 것이 좋다.\
자동 반사적으로 중복을 제거해버리는 잘못을 저지르는 유혹을 떨쳐내고 중복이 진짜 중복인지 확인해야 한다.

### 결합 분리모드 (다시)

소스 수준 분리모드 - 소스 코드 모듈 사이의 의존성을 제어할 수 있다. 이 모드에서는 모든 컴포넌트가 같은 주소공간에서 실행된다. 모놀리틱 구조라고도 한다.\
배포수준 분리모드 - jar 파일, DLL, 공유 라이브러리와 같이 배포 가능한 단위들 사이의 의존성을 제어할 수 있다. 많은 컴포넌트가 여전히 같은 주소공간에 상주하며, 어떤 컴포넌트는 동일 프로세서의 다른 프로세스에 상주하고, 프로세스간 통신, 소켓, 공유메모리 등으로 통신할 수 있다.\
서비스 수준 분리모드 - 의존하는 수준을 데이터 구조 단위까지 낮출 수 있고, 네트워크 패킷을 통해서만 통신하도록 만들 수 있다. (예시 - 서비스 또는 마이크로 서비스)\
어떤 모드가 최적인지 초기엔 알기 어렵다.\
한가지 해결책은 단순히 서비스 수준에서의 분리를 기본 정책으로 삼는것이다. 비용이 많이 들고, 결합이 큰단위에서 분리된다는 문제가 있다. 또한 시스템 자원 측면에서도 비용이 많이 든다.\
컴포넌트가 서비스화 될 가능성이 있다면 컴포넌트 결합을 분리하되 서비스가 되기 직전에 멈추는 방식을 선호한다.\
좋은 아키텍처는 결합 분리 모드를 선택사항으로 남겨두어서 배포 규모에 따라 가장 적합한 모드를 선택해 사용할 수 있게 만들어 준다.

### 결론

시스템의 결합 분리 모드는 시간이 지나면서 바뀌기 쉬우며, 이러한 변경을 예측하여 큰 무리없이 반영할 수 있도록 만들어야 한다.

---

## 17 장 선 긋기

경계는 소프트웨어 요소를 서로 분리하고, 경계 너머의 요소를 알지 못하도록 만든다.\
인적자원의 효율을 떨어뜨리는 요인은 특히 너무 일찍 내려진 결정에 따른 결합이다.\
이른 결정이란 유스케이스와 아무런 관련이 없는 결정이다. 프레임워크, DB, 의존성 주입 등.\
이런 결정을 연기할 수 있는것이 좋은 아키텍처이다.

### 두 가지 슬픈 이야기

P사 이야기 - 서버팜을 통해 분산될 수 있는 3티어 리치 아키텍처를 미리 채택\
W사 이야기 - 서비스 중심으로 구조화된 SOA 를 약속하는 일련의 도구들을 너무 일찍 채택

### FitNesse

FIT 도구를 기반으로 하는 간단한 위키페이지\
메이븐이 등장하여 jar 파일 문제를 해결하기 전 이야기. \
jar 파일을 둘 이상 다운로드하도록 만들어서는 안된다는 원칙을 세움 'Download and Go'\
초기 결정 
- 웹서버를 직접 작성하기로 함. 좋은 결정이었던 이유는 구현이 간단하고 프레임워크 사용에 대한 결정을 나중으로 연기할 수 있게 해줌
- 어떤 DB 를 사용하더라도 상관없게 설계

업무 규칙과 DB 사이에 경계선을 그음으로써 결정을 늦추고 시간을 절약할 수 있었음

### 어떻게 선을 그을까? 그리고 언제 그을까?

관련이 있는 것과 없는 것 사이에 선을 긋는다.\
GUI 와 업무규칙, DB 와 GUI, DB 와 업무규칙 등\
DB 는 업무 규칙이 간접적으로 사용할 수 있는 도구다. 경계선은 DB 인터페이스 바로 아래에 그을 수 있다.

![image](https://user-images.githubusercontent.com/5154845/139529814-cdbd4518-d0a8-4d9a-bc35-7fa857f96074.png)

컴포넌트로 보면 DB 가 업무 규칙에 대해 알고있는데, 이는 DB 인터페이스가 업무규칙 컴포넌트에 속한다는 것을 뜻한다. 또한 업무규칙에게 있어 DB 는 문제가 되지 않지만 DB 는 업무 규칙 없이는 존재할 수 없다는 사실을 알 수 있다.

![image](https://user-images.githubusercontent.com/5154845/139529827-9b7199ae-79d1-4101-b494-32a8f7234544.png)


### 입력과 출력은?

개발자와 고객은 종종 GUI 가 시스템이라고 생각하곤 한다. 그러나 입력과 출력은 중요하지 않다.\
인터페이스 뒤에는 인터페이스를 조작하는 모델이 있고, 이 모델은 인터페이스를 전혀 필요로 하지 않는다.\
컴포넌트로 보면 GUI 가 업무 규칙에 의존하고 있음을 알 수 있다.

![image](https://user-images.githubusercontent.com/5154845/139529833-ab0064fe-601f-4a26-86ae-733217ee9002.png)

### 플러그인 아키텍처

DB 와 GUI 에 대해 내린 두가지 결정을 보면 컴포넌트 추가와 관련한 일종의 패턴을 볼 수 있다. - 플러그인\
선택적이거나 또는 수많은 다양한 형태로 구현될 수 있는 나머지 컴포넌트로부터 핵심적인 업무 규칙은 분리되어 있고, 또한 독립적이다.

![image](https://user-images.githubusercontent.com/5154845/139531697-5f7db6b6-67c3-4a96-85a6-99b3357b6134.png)

인터페이스와 DB 를 플러그인 형태로 고려하였기 때문에 다양한 임의의 종류의 기술로 대체할 수 있다.

### 플러그인에 대한 논의

Resharper (made by JetBrains) 와 Visual Studio (made by MS)\
의존성 구조를 보면 Resharper 팀은 절대 VS 팀을 건드릴 수 없고, VS 팀은 Resharper 팀을 무력화 할 수 있다.

![image](https://user-images.githubusercontent.com/5154845/139529864-b532beae-9d55-4b03-a14d-8064151e8e22.png)

이 비대칭적인 관계는 우리가 원하는 특정 모듈이 나머지 모듈에 영향받지 않기를 바라는 점을 해결한다.\
경계는 변경의 축이 있는 지점에 그어진다. 단일 책임 원칙은 어디에 경계를 그어야 할지를 알려준다.

### 결론

소프트웨어 아키텍처에서 경계선을 그리려면 먼저 시스템을 컴포넌트 단위로 분리하고, 컴포넌트 사이의 화살표가 핵심 업무를 향하도록 컴포넌트 소스를 배치한다.

---

## 18 장 경계 해부학

시스템 아키텍처는 일련의 소프트웨어 컴포넌트와 이들을 분리하는 경계에 의해 정의된다.\
흔한 몇가지 형태를 살펴보자.

### 경계 횡단하기

'런타임에 경계를 횡단한다' - 경계 한쪽에 있는 기능에서 반대편 기능을 호출하여 데이터를 전달하는 일\
적절한 위치에서 횡단하게 하는 비결은 소스코드 의존성 관리.\
소스코드인 이유 -  소스 모듈 하나가 변경되면 의존성 있는 다른 모듈도 변경 가능성이 있기 때문.\
경계는 이런 변경이 전파되는 것을 막는 방화벽을 구축하고 관리하는 수단으로써 존재.

### 두려운 단일체

가장 단순하고 흔한 아키텍처 형태는 물리적으로 엄격하게 구분되지 않는 형태다. (소스 수준 분리모드)\
배포 관점에서는 경계가 드러나지 않는다. 그렇다고 해서 모놀리스 구조에서 경계가 실제로 존재하지 않거나 무의미한 것은 아니다.\
이런 아키텍처는 거의 모든 경우에 특정한 동적 다형성에 의존하여 내부 의존성을 관리한다. \
정적 다형성 - 제네릭, 템플릿\
객체 지향 패러다임의 발전으로 컴포넌트를 잘 분리할 수 있게 됨.\
가장 단순한 형태의 경계 횡단은 저수준 클라이언트에서 고수준 서비스로 향하는 함수 호출이다.\
런타임 의존성과 컴파일 타임 의존성은 모두 저수준 → 고수준 컴포넌트로 향한다.\
주목할 점은 경계에서 호출되는 쪽에 Data 에 대한 정의가 위치한다는 사실이다.

![image](https://user-images.githubusercontent.com/5154845/139529883-af705d62-e7fe-4aaf-a929-0d8654c27a22.png)

고수준 클라이언트가 저수준 서비스를 호출해야 한다면 동적 다형성을 이용하여 제어흐름과는 반대 방향으로 의존성을 역전시킬 수 있다.\
주목할점은 의존성은 모두 고수준 컴포넌트를 향하며, 데이터 구조의 정의는 호출하는 쪽에 위치한다는 것.

![image](https://user-images.githubusercontent.com/5154845/139529888-565df043-1040-4688-9591-ae0fb7330222.png)

모놀리틱 구조라도 이처럼 규칙적으로 구조를 분리하면 프로젝트를 개발, 테스트, 배포하는데 큰 도움이 된다.\
단일체에서 컴포넌트간 통신은 전형적인 함수 호출에 지나지 않기 때문에 매우 빠르고 값싸다.

### 배포형 컴포넌트

아키텍처의 경계가 물리적으로 드러날 수도 있는데, 그중 가장 단순한 형태는 동적 링크 라이브러리다.\
컴포넌트를 이 형태로 배포하면 따로 컴파일하지 않고 곧바로 사용할 수 있다. (배포수준 결합분리모드)\
배포 과정에서만 차이가 날 뿐, 배포수준의 컴포넌트는 단일체와 동일하다.\
단일체와 마찬가지로 경계를 가로지르는 통신은 전형적인 함수 호출에 지나지 않기 때문에 매우 빠르고 값싸다.

### 스레드

단일체와 배포형 컴포넌트는 모두 스레드를 활용할 수 있다. \
스레드는 아키텍처 경계도 아니며 배포 단위도 아니다. 실행 계획과 순서를 체계화 하는 방법에 가깝다.

### 로컬 프로세스

훨씬 강한 물리적 형태를 띠는 아키텍처 경계로는 로컬 프로세스가 있다.\
로컬 프로세스들은 동일한 또는 여러 프로세서들에서 실행되지만 각각 독립된 주소공간에서 실행된다.\
대개 소켓, 메일박스, 메시지 큐와 같이 운영체제에서 제공하는 통신기능을 이용하여 서로 통신한다.\
각 로컬 프로세스는 정적으로 링크된 단일체 또는 동적으로 링크된 여러 컴포넌트로 구성될 수 있다.\
로컬 프로세스는 컴포넌트간 의존성을 동적 다형성을 통해 관리하는 저수준 컴포넌트로 구성된다.\
로컬프로세스간 분리 전략은 동일하게 소스코드 의존성의 화살표가 항상 고수준 컴포넌트를 따른다.\
로컬프로세스에서 고수준 프로세스가 저수준 프로세스의 이름, 물리주소 등을 절대 포함해선 안된다.

### 서비스

물리적인 형태를 띠는 가장 강력한 경계.\
서비스는 프로세스로, 자신의 물리적 위치에 구애받지 않는다. 모든 통신은 네트워크를 통해 이뤄진다고 가정한다.\
서비스 경계를 지나는 통신은 함수 호출에 비해 매우 느리다. 지연에 따른 문제를 고수준에서 처리할 수 있어야 한다.\
저수준 서비스는 반드시 고수준 서비스에 플러그인 되어야 한다.

### 결론

단일체를 제외한 대다수의 시스템은 한 가지 이상의 경계전략을 사용한다. \
대체로 한 시스템 안에서도 통신이 빈번한 로컬 경계와 지연을 중요하게 고려해야 하는 경계가 혼합되어 있음을 의미한다.
