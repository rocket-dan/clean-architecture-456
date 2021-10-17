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

