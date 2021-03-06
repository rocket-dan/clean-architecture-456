19장 정책과 수준

# 서론. 
컴퓨터 프로그램의 핵심. 
- 정책을 기술하는 것
- 각 입력을 출력으로 변화하는 정책을 상세하게 기술한 설명서. 

소프트웨어 아키텍처를 개발하는 기술
- 정책을 단위별로 알맞게 나누고 정의 하는 것. 
- 동일 시점에 변경되는 정책은 동일 *수준*을 유지하고 동일한 *컴포넌트*에  속해야한다.  
- 재편성된 컴포넌트들을 비순환 방향 그래프로 구성하는 기술을 포함 
- 그래프에서 node는 동일한 수준의 정책을 포함하는 컴포넌트에 해당되고, 방향이 있는 edge는 컴포넌트 사이의 *의존성*을 나타낸다.  
- edge는 다른 수준에 위치한 컴포넌트를 서로 연결한다.  
- 동일 시점에 변경되는 정책은 동일 *수준*을 유지하고 동일한 *컴포넌트*에  속해야한다.
- 재편성된 컴포넌트들을 비순환 방향 그래프로 구성하는 기술을 포함 
- 그래프에서 node는 동일한 수준의 정책을 포함하는 컴포넌트에 해당되고, 방향이 있는 edge는 컴포넌트 사이의 *의존성*을 나타낸다.
- edge는 다른 수준에 위치한 컴포넌트를 서로 연결한다.
- *의존성*은 소스코드, 컴파일 타임의 의존성이며 컴파일러가 제대로 동작하기 위해 필요. 
- 좋은 아키텍쳐라면 각 컴포넌트를 연결할 때 의존성 방향이 컴포넌트의 수준을 기반으로 연결되도록 만들어야 한다. 즉 저수준 컴포넌트가 고수준 컴포넌트에 의존하도록 설계
# 본론
## level
수준(= level) 
- 입력과 출력까지의 거리

 ### Level(feat. 데이터 흐름도) #### 데이터 흐름도 그리는 법
-데이터 흐름: 굽은 실선 화살표
-소스코드 의존성: 곧은 점선

￼
<입력 장치에서 문자를 읽어 테이블을 참조해서 문자를 번역후, 번역된 문자를 출력장치로 기록> 

#### 데이터 흐름 주의점
- 데이터 흐름과 소스 코드의 의존성이 항상 같은 방향 가르키지 x
- 소스 코드의 의존성은 level에 따라 결합되어야 하며 데이터 흐름 기준으로 결합 x 
- 만약 이를 어길시 부작용 	
- 읽고 번역하고 그대로 쓴다 
``` 
function encrypt() {
	while(true) 		writeChar(translate(readChar()); 
}
```
- encrypt 함수가 level이 높은데, 낮은 level인 readChar와 writeChar 함	수에 의존이 되어버린다. 
 ### Level(feat. 클래스 다이어그램)
-방금 보여준 부작용 있는 코드를 개선해보자(?)
￼
<이 경계를 횡단하는 의존성은 모두 경계속에만 있으며, 고 level 구성요소> -ConsoleReader, ConsoleWriter는 입력 출력과 가깝기에 저 level임으로 클래스로 표현
-이 구조에서 고level 암호화 정책을 저 level 입/출력 정책과 분리시켜 해당 기능에 변화가 일어나도 영향을 약화 
- 개선 개굿
 - 상기 -> 정책이 변경되는 방식에 따라 컴포넌트로 묶는 기준이 영향 받는다.
-단일 책임 원칙과 공통 폐쇄 원칙에 따르면 동일한 이유로 동일 시점에 변경되는 정책은 함께 묶인다.(?) 뭔소리지?
-즉 변경이 별로 없는 것을 고 level 정책으로 하고 더 빈번하게 변경되는 것을 저 level 정책으로 하면 좋다.
 -지금의 경우도 암호화 알고리즘은 변경될 가능성이 적어보이고 입출력 장치는 이 많아보인다. 
- 이런식으로 소스 코드의 의존성 방향이 고 level 정책을 향할 수 있도록 정책을 분리 했다면 변경의 영향도가 줄어 아래처럼 플러그인 처럼 쓸 수 있다.

*쓸모없는 궁금증* 이건... 진짜 플러그의 플러그인이라는 건가? 아님 개발의 플러그인일까? yes
￼
 # 결론
-이 정책은 단일 책임 원칙, 개방 폐쇄 원칙, 공통 폐쇄 원칙, 의존성 역전 원칙, 안정된 의존성 원칙, 안정된 추상화 원칙 모두 포함.... 
- 저자도 알고 있다 우리보고 다시 읽으라는 것 보면(?) 다시읽으세요  


----
20장 업무 규칙
 #서론 애플리케이션을 업무규칙과 플러그인으로 구분하려면 실제로 무엇이 업무 규칙인지 이해해야 한다.

## 핵심 업무 규칙
-ex)은행에서 대출에 n%의 이자를 부과 -이 규칙은 사업 자체에 핵심적이며, 규칙을 자동화하는 시스템이 없더라도 그대로 존재하는 것 - *핵심 업무 데이터*를 요구   - ex) 대출 잔액, 이자율, 지급 일정 etc...  핵심 규칙과 핵심 데이터는 본질적으로 결합되어 있어 객체가 될 좋은 후보가 된다. 이러한 유형의 객체를 Entity  
# 본론 ## Entity ###Entity란? -Entity의 인터페이스는 핵심 업무 데이터를 기반으로 동작하는 핵심 업무 규칙을 구현한 함수들로 구성 - (어려운 말)컴퓨터 시스템 내부의 객체, 핵심 업무 데이터를 기반으로 동작하는 일련의 조그만 핵심 업무 규칙을 구체화 

###Entity(feat. UML class)

￼
<업무에 핵심적인 개념을 구현하는 소프트웨어만 모은 것, 구축 중인 다른 자동화 시스템(db, 사용자 인터페이스 등 )과는 분리> - 이 클래스는 어떤 시스템에서도 업무를 수행할 수 있다. - 꼭 class일 필요는 없고 핵심 업무 데이터와 업무 규칙을 하나로 묶어서 별도 모듈이기만 하면 된다....

Q. class가 아닌 것은 뭐가 있지?  
## 유스케이스
-핵심 업무 규칙은 자동화 시스템 없어도 사용할 수 있지만, 이 규칙의 경우 수동 환경에서는 사용 x, 자동화된 시스템이 사용되는 방법
-ex) 대출 시 신상 정보 화면 모두 채우고 검증 후 신용도가 하한선보다 높은지 확인 후 대출 견적 화면으로 진행
-사용자에게 보여줄 처리단계를 기술
-애플리케이션에 특화된 업무 규칙

사진
 - Entity 내부의 핵심 업무 규칙을 어떻게, 언제 호출할지를 명시하는 규칙을 담는다. 
- 해당 사진은 Customer Entity에 대한 참조이며, 은행과 고객의 관계를 결정짓는 핵심 업무 규칙이 해당 Entity에 들어간다.

### 유스케이스와 Entity 
- 유스케이스는 엔티티 내부의 핵심 업무 규칙을 어떻게 언제 호출 할지를 명시하는 규칙을 담는다.
-중요한 것은 유스케이스는 애플리케이션에 특화된 규칙을 설명하며, 이를 통해 사용자와 엔티티 사이의 상호작용을 규정 (유스케이스는 사용자 데이터가 들어오고 나가는 방식따위 기술 x)

- 유스케이스는 객체다.
	-애플리케이션에 특화된 업무 규칙을 구현하는 하나 이상의 함수를 제공 
  - 입력 데이터. 출력 데이터, 유스케이스가 상호작용하는 엔티티에 대한 참조 데이터 등의 데이터 요소 포함 
  - - 엔티티는 유스케이스에 대해서 모르며 이를 위반 시 의존성 역전 법칙의 의존성 방향이 달라지는 예 (아까 저 level은 고level을 알지만 고 level은 저 level을 알지 못했쥬?) 
#### 그렇다면  왜 엔티티는 고level이고, 유스케이스가 저 level일까? 
- 유스케이스는 단일 어플리케이션에 특화되었기에 입출력과 가깝다.
-  -수많은 어플리케이션에 사용될 수 있도록 일반화 된것
-   -그러므로 유스케이스는 엔티티에 의존하며, 엔티티는 유스케이스에 의존x  
-   
## 요청 및 응답 모델
-데이터를 사용자나 또 다른 컴포넌트와 주고 받는 방식에 대해 눈치 챌 수 없어야 하다. 
- 즉 단순한 요청 데이터 구조를 입력으로 받아들이고, 단순한 응답 데이터 구조를 출력으로 반환 
- - 의존성 제거 중요   
- - 요청 및 응답 모델이 독립적이지 않다면, 그 모델에 의존하는 유스케이스도 결국 해당 모델이 수반하는 의존성에 간접적 결합 
- - 엔티티 객체를 가리키는 참조를 요청 및 응답 데이터 구조에 포함하려는 유혹 받을 수 있다.
-두 객체 묶으면 공통 폐쇄 원칙과 단일 책임 원칙에 위배 된다. 
# 결론
업무 규칙은 사용자 인터페이스, 디비 같은 저level 관심사로 인해 오염 x 가장 독립적이고 가장 재사용 가능한 코드여야 한다



----
21장 소리치는 아키텍처
# 서론
아키텍트의 주제가 명확해야 한다.... 외쳐 스프링? 외쳐 도서관? ## 아키텍처의 테마 - 책 오브젝트 오리엔트 소프트웨어 엔지니어링 읽자   - 부제: 유스케이스 주도 접근법   - 소프트웨어 아키텍처는 시스템의 유스케이스를 지원한느 구조 -아키텍처는 프레임워크에 대한 것은 “절대” 아니고 프레임워크는 도구일 뿐  ## 아키텍처의 목적
-아키텍처는 유스케이스를 “중심”으로 “지원해주는 구조” 여야 한다. -좋은 아키텍처는 개발 환경 문제나 도구에 얽매이지 않기에 결정을 후반까지 미룰 수 있고 번복할 수 있게 해준다.


## 하지만 웹은? 
- 웹은 전달 매커니즘이다. 
- 애플리케이션이 웹을 통해 전달되는 사실은 세부사항, 시스템 구조를 지배해서는 절대 안된다.(웹,앱, 웹서비스 앱 어디로 전달할지는 중요 노노)  
 
## 프레임워크는 도구일뿐, 삶의 방식 아니다. 
-취하지 말아야 하는 것 -> 프레임워크가 모든 것을 하게 하자
-모든 것 하게 하면 비용 개많이 든다.
-즉 유스케이스에 중점을 둔채로 그대로 보존할 수 있을지 부터 생각하자... 
-프레임워크가 아키텍처의 중심을 차지하는 일을 막을 수 있는 전략으로나 개발하자  ## 테스트하기 쉬운 아키텍처
-아키텍처가 유스케이스를 최우선, 프레임워크와 적당한 거리 -> 프레임워크를 전혀 준비하지 않더라도 필요한 유스케이스 저부에 대해 단위 테스트를 할 수 있어야 한다.
-테스트를 돌리는데 웹 서버가 반드시 필요한 상황, 디비 연결 노노 
- 엔티티 객체는 pojo(에서 자바 뺀거)여야 하고, 유스케이스 객체가 엔티티 객체를 조작해야한다.

# 결론
-아키텍처가 시스템을 대표해야지, 시스템에 적용한 프레임워크에 대해 얘기해서는 x - 소스만 보고도 오 무슨 제품인지 알아야 한다.
- 심지어 뷰 컨트롤러 따위 세부사항이어야 한다.....  

----
22장 클린 아키텍처

# 서론
시스템 아키텍처의 목표는 관심사 분리이다. 

## 계층을 분리함으로서 달성 
-업무 규칙 위한 계층, 사용자와 시스템 인터페이스 위한 또 다른 계층 하나씩은 반드시 포함 
- 독립성 	
  - 프레임워크 독립성 	
  - - 테스트 용이성 	
  - - UI 독립성 
  - 	-DB 독립성 	
  - - 외부 에이전시 독립성



## 의존성 규칙
￼
<동심원은 소프트웨어에서 서로 다른 영역 표현하며, 안으로 들어갈 수록 고level 소프트웨어> - 바깥은 메커니즘 안쪽 원은 정책 - 소스 코드 의존성은 반드시 안쪽으로, 고 level 의 정책을 향해야 한다.
	- 즉 내부의 원에 속한 코드는 외부의 원에 선언된 어떤 것에 대해서도 그 이름을 영향/언급 하면 안된다.
	- 외부의 원에서 선언된 데이터 형식도 내부의 원에서 절대로 사용해서는 x 
### 엔티티
-전사적인 핵심 업무 규칙을 캡슐화 -기업의 다양한 어플리케이션에서 재사용할 수 있는 것이 핵심, 형태는 중요x -전사적이지 않은 단일 애플리케이션을 작성하고 있다면 엔티티는 해당 애플리케이션의 업무 객체가 되며 가장 일반적이며 고level 규칙을 캡슐화 	- 즉 운영 관점에서 특정 어플리케이션의 무언가가 변경 되더라도, 엔티티 계층에는 절대 영향 노노


### 유스케이스
-애플리케이션에 특화된 업무 규칙을 포함 -유스케이스 계층의 소프트웨어는 시스템의 모든 유스케이스를 캡슐화하고 구현
-엔티티로 들어오고 나가는 데이터 흐름을 조정, 엔티티가 자신의 핵심 업무 규칙을 사용해서 유스케이스의 목적을 달성하도록 이끈다.
-여기에서 발생한 변경이 엔티티에 영향 x 이고, 외부 요소(디비, 프레임워크 등)에서 발생한 변경도 영향 x -“하지만” 운영 관점에서 애플리케이션이 변경 시 유스케이스가 “반드시” 영향을 받는다.
 ### 인터페이스 어댑터 -일련의 어댑터로 구성, 외부 에이전시(디비나 웹)에게 가장 편리한 형식으로 변환 - 뷰컨트롤러, 프레젠터(이건 뭐지?) 모두 인터페이스 어댑터 계층 - 모델은 그저 데이터 구조일 뿐 컨트롤러에서 유스케이스로 전달되고, 다시 유스케이스에서 프레젠터와 뷰로 되돌아간다. - 이계층도 다른 계층에 대해 알면 안된다.
	- 데이터를 엔티티와 유스케이스에서 가장 편리한 형식에서 임의의 프레임워크(디비같은)가 사용하기 편리한 형식으로 변환
	- 이 계층이 디비를 담당하는 부분으로 제한 	- 데이터를 외부 서비스와 같은 외부적 형식에서 유스케이스나 엔티티에 사용되는 내부적인 형식으로 변환하는 또 다른 어댑터

### 프레임워크와 드라이버 -가장 바깥 계층으로 일반적으로 웹 프레임워크 같은 프레임워크나 도구들로 구성
-안쪽 원과 통신하기 위한 접합 코드 외에는 특별히 더 작성할 코드가 많지 않다.  ### 원이 네개여야 하나? -의존성의 방향 규칙만 적용되면 되지 개수는 중요치 않다. -안쪽으로 갈수록 추상화와 정책 레벨은 높아지며 캡슐화되고, 바깥으로 갈 수록 저레벨의 구체적이고 세부사항
 ### 경계 횡단하기
- 하단 다이어그램에 원의 경계를 횡단하는 방법을 보여주는 예시
	-> 커트롤러와 프레젠터가 다음 레벨에 속한 유스케이스와 통신하는 모습 확인
- 횡단을 사용해야 하는 상황 	- 제어흐름 		-컨트롤러 -> 유스케이스 -> 프레젠터에서 실행
	-소스코드 의존성
		- 각 의존성이 유스케이스를 향해 안쪽
-만능 해결법 	- 제어 흐름과 의존성 방향이 명백히 반대여야 하는 경우 의존성 역전 원칙을 사용해 해결 	-  별다른 조치 없이 제어 흐름을 따라 구현하면 안쪽 원의 코드가 바깥쪽 원의 코드를 호출하게 되는데 이 지점에서 소스 코드 의존성을 제어흐름과 반대로 역전 시켜 바깥쪽 원의 코드가 안쪽 원의 코드를 호출하게 한다. 
### 경계 횡단하는 데이터는 어떤 모습인가? - 격리되어 있는 간단한 데이터 구조가 경계를 가로질러 전달만 되면 된다.
	-그렇다고 엔티티 객체나 row를 전달하면 안된다.
	-데이터 구조가 어떤 의존성을 가져 의존성 규칙 위배 되면 x
-내부 원에 사용하기 편리한 형태이기만 하면 된다.

## 전형적인 시나리오
다른 사람들은 어떻게 했찌?

# 결론
단지 겁나 쉬운 계층만 분리하고 의존성 규칙만 준수하면 테스트도 쉽고 행복해진다(?) 

# 23장 프레젠터와 험블 객체
아까 궁금했던게 나오는 군요
# 서론
## 프레젠터 is
- 험블객체 패턴을 다르는 형태
- 아키텍처 경계를 식별하고 보호하는데 도움 
# 본론 ## 험블 객체 패턴
-테스트 하기 어려운 행위와 쉬운 행위를 단위 테스트 작성자가 분리하기 쉽게 하는 방법
- 아이디어:행위들을 두 개의 모듈 또는 클래스로 나누고 이 모듈 중 하나가 험블...
-기본적인 본질을 남기고 테스트하기 어려운 행위를 모두 험블 객체로 옮긴다. - 나머지 모듈에는 험블 객체에 속하지 않은, 테스트하기 쉬운 행위를 모두 옮긴다.
-예) gui의 경우 테스트하기 쉬운 gui에서 수행하는 행위와 테스트 하기 어려운 위치를 확인하는 행위를 분류한다. 험블 객체 패턴을 사용해, 두 부류의 행위를 분리해 서로 다른 클래스를 만든다.

## 프레젠터와 뷰
- 뷰
	-험블 객체이고 테스트가 어렵기에 가능한 간단하게 유지한다.
	-데이터를 gui로 이동시키지만, 데이터를 직접 처리 x
- 프레젠터 	- 테스트 하기 쉬운 객체 	-에플리케이션으로부터 데이터를 받아 화면에 표현할 수 있는 포맷으로 만들기
	- 뷰가 단지 데이터를 화면으로 전달하는 일만 처리하도록 한다.  ## 테스트와 아키텍처
-테스트 용이성은 좋은 아키텍처가 지녀야 할 속성
-험블 객체 패턴이 좋은 예 -> 행위를 난이도를 분류해 아키텍처 경계가 정의되기 때문

## 데이터베이스 게이트웨이
-유스케이스 인터랙터와 데이터베이스 사이에 위치
-애플리케이션이 디비에 수행하는 crud 작업과 관련된 모든 메스드를 포함
-sql 따위 허용 x, 즉 어제 로그인한 모든 사용자의 성을 알고 싶으면getLastNamesOfUsersWhoLoggedInAfter 같은 메서드를 제공해야한다. -이 구현체도 험블 객체다.
	- 구현체에서 직접 sql을 사용하거나 디비에 대한 임의의 인터페이스를 통해 게이트웨이의 메서드에서 필요한 데이터에 접근
-인터랙터는 애플리케이션에 특화된 업무 규칙을 캡슐화 하기 때문에 험블 객체 x, 모의 객체로 적당히 교체할 수 있기 때문

## 데이터 매퍼
-하이버네이트 같은 orm은 어떤 계층일까? - orm(object relational mapper)같은 건 사실 존재하지 않는다(?) -객체를 사용하느 사람 관점에서 객체는 데이터 구조는 아니다.
- 일단 데이터는 모두 private로 선언되므로 객체의 사용자는 데이터를 볼 수 없다.
- 단순한 오퍼레이션 집합이며 차라리 데이터 매퍼에 가깝다.
-그러므로 데이터베이스 계층에 가깝다.
-orm은 게이트웨이 인터페이스와 디비 사이에서 일종의 또 다른 험블 객체 경계를 형성

## 서비스 리스너
- 외부로부터 데이터를 수신하는 서비스의 경우, 서비스 리스너가  서비스 인터페이스로부터 데이터를 수신하고, 데이터를 에플리케이션에 사용할 수 있게 데이터 구조로 포맷 변경
-그 후 이 데이터 구조는 서비스 경계를 가로질러 내부로 전달

# 결론
-각 아키텍처 경계마다 험블 객체 발견할 수 있음으로 험블 객체 패턴을 사용하면 전체 시스템의 테스트 용이성 높아진다.
 





# 24장 부분적 경계

#서론
 - 아키텍처 경계를 갓벽하게 하기는너무 힘듬,  -그래서 부분적 경계가 차선책!  #본론
## 부분적 경계를 생성하는 방법
-독립적으로 컴파일하고 배포할 수 있는 컴포넌트를 만들기 위한 작업은 모두 수행 후, 단일 컴포넌트에 그대로 모아만 두는 것
- 모두를 단일 컴포넌트로 컴파일 해서 배포하는 것
-완벽한 경계를 만드는 것 만큼의 코드량과 사전 설계가 필요하지만, 다수의 컴포넌트를 관리하는 작업은 필요 x 	- ex) 버전 번호, 배포 관리 부담
- Fitnesse 전략
	- 웹 서버 컴포넌트는 위키나 테스트 영역과 분리되도록 설계
	- 그러나 시간이 흐르며 분리한 웹 컴포넌트가 재사용 될 가능성이 전혀 없음으로, 위키 컴포넌트와 웹 컴포넌트 구분 약화

## 일차원 경계
-양방향 격리된 상태를 유지해야 하므로 쌍방향 바운더리 인터페이스를 사용 - 지속적으로 유지할 때 비용이 많다
추후 완벽한 형태의 경계로 확장할 수 있는 공간을 확보하고자 할 때 활용할 수 있는 깐단한 구조가 strategy 패턴
그림 24.1
￼
 - 미래에 필요할 아키텍처 경계를 위한 무대를 마련한다는 점은 명백
-client를 serviceimpl로부터 격리시키느데 필요한 의존성 역전이 이미 적용되었기 때문
-“그러나” 코드레벨에서 잘못된 사용을 막을 수는 없다.

## 퍼사드
- 그림 24.2
￼
 -의존성 역전까지 희생
-경계는 facade 클래스로만 간단 정의 	
- facade 클래스에는 모든 서비스 클래스를 메서드 형태로 정의 	- 서비스 호출이 발생하면 해당 서비스 클래스로 호출을 전달 	
- 클라이언트는 이들 서비스 클래스에 직접 접근 x 
-but 	
  -client가 모든 서비스 클래스에 종속성 가진다.
	-정적 언어면 서비스 클래스 중 하나에서 소스 코드가 변경되면 client 무조건 재 컴파일 	
  -비밀 통로 또한 쉽게 만듐  

# 결론
세 전략 모두 비용과 장점있으며 적용될 상황 또한 다르다.
아키텍트가 잘 선택해보즈앙

