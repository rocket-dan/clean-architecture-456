
# 4. Component principle

SOLID 에서 한 것 : 어떻게 이 벽돌을 잘 조합해서 벽이랑 방을 만들까?!
Coponent principle에서 할 것 : 그럼 이 방들을 모아서 어떻게 빌딩을 만들까?! 에 가깝다.

## 12. Components

- [ ] components : smallest entities that can be deployed as part of system.
	- [ ] Ex ) `.jar` file, `.gem` file, etc.

좋은 컴포넌트 설계는, 개별적으로 deploy 가능해야만 한다.

#### Relocatability

코드가 길어지고, function이 늘어나면서, 한계점을 넘어가기 시작할 때, 아래와 같은 해결책이 대두되었다.

The compiler was chhanged to output binary code thaat could be relocated in memory by a smart loader.
이 로더를 통해, 다시 재배치될 수 있는 코드를 어디로 배치시킬 지 알 수 있게 되었다.
결론적으로, 프로그래머들이 메모리 내 배치될 함수들을 고르고, 배치시킬 수 있게 됨.

If a program defined a library function, the compiler would emit that name as an external reference.
If a prograam defined a libraary function, the compielr would emit that name as an external definition.


#### Linkers

linking loader allowed programmers to divid etheir programs up onto separately compilable and loadable segments.
사실 linking loader는 좀 느린 감이 있었음. 그래서 loading / linking이 두 분야로 나뉘게 됨.
프로그램밍적으로 linking(느린 부분) 을 핵결하고, linker라고 따로 어플리케이션을 나눔(빠른 부분)

ex) C언어는 소스 코드를 컴파일해서 `.c` -> `.o` 파일로ㅗ 바꾸고, 링커에 보내 실행가능한 파일을 생성하는 방식으로 빨리 로드될 수 있게 만듦

그러나 이후에도 compile-link 시간이 오래 걸림. (자바 컴파일이 예전에 아주 느렸던 것처럼...)
시간이 지나며 기술이 발전해 이 시간은 많이 줄어듦.

## 13. Component Cohension

그래서 어떤 class를 어떤 components 에 넣을 것인가?
여기에 관한 세 가지 법칙이 있음.

 - [ ] REP : The Reuse / Releases Equivalence Principle
 - [ ] CCP : The Common Closure Principle
 - [ ] CRP : The Common Reuse Principle

#### REP : The Reuse / Releases Equivalence Principle

People who want to reuse software components cannot, and will noot, do soso unless those components are tracked through a release process and are given release numbers.

without relase numbers, there would be no way to ensure that aall the reused components are compatible with each other.

this principle means, that the classes and moduless that aare formed into a component must belong to a cohehssive group.

Classes and modules that are groupe dtogether into aa component should be releaasable together. > 같은 릴리즈 트랙킹, 같은 도큐먼트, 유저 사용에 걸맞아야

#### CCP : The Common Closure Principle

Gather into components those claasses thaat change for tha same reaason and at the ssame times. Do seperate vice versa!

class shooud nothave multiple reasosns to change. > If the code in an application must chaangge, you would rathee rthat all of the changes occur in one component, raather than being distributed across many components.

The principle is closely associated with the open closed principle(OCP)
OCP states that classses should be closed for modification but open for extension.

similarity with SRP
- [ ] SRP tells us to separate methods into different classes, if they change for different reasons.
- [ ] The CCP tells us to separate classes into different components, if they change for different reasons

#### CRP : The Common Reuse Principle

It states that classes and moodules that tend to be reused togethher belong in the same component. also tells us whichh clsses to put togehter into a component, ant which classes not to keep together.

we want to make sure that the classes that we put into a component are insseparable.- 하나엔 의존성이 있고 다른덴 아니고 하기가 어렵다. 불필요한 components를 다시 디플로이 하는 데에 시간 낭비 초래.
Don't depend on things you don't need.

---
이 세 가지 규칙의 다이어그램에서 현재 상황에 맞춰 잘 조정해 나가되, 계속 변화하는 상황에도 귀를 기울여야 한다.

## 14. Component Coupling

There are three principles regarding the relationships between components:
● ADP: Acyclic Dependencies Principle
● SDP: Stable Dependencies Principle
● SAP: Stable Abstractions Principle


#### 1. The Acyclic Dependencies Principle

- [ ] the morning after syndrome : 어제까지만 해도 잘되던게 갑자기 안돼서 아놔 ㅅㅂ 뭐야? 하고 보니 누가 뭐 하나를 바꿔서 그거에 의존하는 모든 게 갑자기 안되기 시작하는 일

어떻게 방지하냐면 1. weekly build 이랑 2. Acyclic Dependencies Principle (ADP).

a. The Weekly Build

Usually used in medium-sized projects. Developers work the first four days of the week, ignoring the others. Without any integration concerns, they develop the code as if the code will run independently. After four days, on Friday, all changes and the system are integrated.

Although this approach provides a great advantage for the first four days, the fifth day causes a serious problem.

 As the project grows, the cost of integration increases and the burden on other days begins. As integration decreases, so does the efficiency of the development team

b. Eliminating Dependency Cycles

This problem can be resolved by dividing the development environment into releasable components. These components become the work of a developer or a team and can be made available to other developers.

릴리즈 할 때마다 타팀들도 그 릴리즈 적용할 시간 필요

However, each change made to a component may not be critical to other teams. It is a decision of the team to decide whether the work will be adjusted according to the new release.

Typical component diagram

directed acyclic graph로도 불림 (사진어케넣음 ㅋㅋ

c. Breaking the Cycle

  - [ ] By applying DIP, an interface between Entities and Authorizer can be placed and the connection can be reversed.

Inverting the dependency between Entities and Authorizer
A component to which Entites and Authorizer components are dependent can be created. This is usually applied when a temporary solution is required. Creating new components means that the dependency structure will also grow.

The new component that both Entities and Authorizer depend on

#### 2. Top-Down Design

the component structure cannot be designed from top to bottom
Component structure is not one of the first things to be done when designing the system, it should develop according to the growth and change of the system.

시작부터 디자인 끝 땅땅 하기 어렵고, The changes are localized as much as possible in a specific region, and can be achieved by having the class distributions of the components where regional changes can be made by giving importance to SRP and CCP.

As application begins to grow, reusable element formation becomes a concern. 
At this point, components are started to be combined using CRP.
The loops resulting from this process are also broken using ADP and component dependency is eliminated, but the graph is growing.

디자인 없이는 디펜던시랑 재사용  가능한 부분을 찾아내기 힘들 수 밖에 없음.

#### 3. The Stable Dependencies Principle

The design cannot be completely static.
To be a sustainable design, it needs to be a little volatility.

At the same time, it is necessary to make sure that these temporary components are not dependent on the components that are difficult to change.
Failure to meet this requirement makes it very difficult to change the component, which is designed to be susceptible to change. Therefore, it can be ensured that such dependence does not occur by using the Stable Dependencies Principle (SDP).

Stability - 많은 의존도가 있을 때 컴포넌트를 바꾸기는 어려울 수 밖에 없음. 변ㄴ경하기 위해서는 의존하는 모든 객체도 신경써야 하기 때문에. 반대로 의존도가 높은 컴포넌트를 stable하다고 생각할수도 있고.

#### 4. The Stable Abstraction Principle

어떤건 자주 안 바뀌는 경우도 있음(ex- high-level architecture and policy decisions)
However, this makes architecture inflexible.

In such cases, by applying OCP, it is possible to create flexible and non-modification classes with the help of Abstract classes.
Stable Abstraction Principle (SAP) provides a relationship between stability and abstractness. On the one hand, it says that the stable component should be abstact, so it doesn’t prevent it from being extended, on the other hand, it says that the unstable component must be concrete and unstable, and also that concrete code should be easily modified.

