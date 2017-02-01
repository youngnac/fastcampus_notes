day2 2017/1/24

#소프트웨어 공학
>공학: 순수학문, 수학과 자연과학을 기초로 해서 인문, 사회과학의 지식을 이용하여 공동의 안전, 건설 복지를 위해서 유용한 사물이나 환경을 구축하는 것을 목적으로 하는 학문.

>다시 말해, 순수학문들을 적용하여 필요로 하는 무언가를 만드는 것. 

###소프트웨어 공학이란?
> 유용한 소프트웨어를 만든는 것을 목적으로 하는 학문
>소프트웨어의 개발, 운용, 유지보수 및 폐기에 대한 체계적인 접근 방법
>- 요구공항, 아키텍처, 개발방법론 등등...

###소프트웨어 개발 생명주기 모델
>Software Development Life Cycle Model
>소프트웨어를 어떻게 개발할것인가에 대한 전체적은 흐름

- 폭포수 waterfall: 단계적으로 진행. 
	- requirment: 요구 사항을 파악
	- design: 소프트웨어 설계
	- implementation: 이를 바탕으로 구현
	- verification: 검증/인증, 요구 사항을 comply하는지
	- maintenance
	
> 오류 발생시 다시 처음으로 돌아가야하고, 더 큰 문제가 발생가능하는 단점을 갖는다. 


- 프로토타이핑: 설계, 개발, 평가 각각의 단계에서 프로토타이핑을 만들어 검증하고 요구사항을 준수하는 확인한다. 

> 각각의 단계의 확인 절차를 진행하기 때문에 커뮤니케이션이 원할하지 못하면 프로젝트가 늘어지게 되고 이에 따른 비용 발생이 불가피하다. 

- 나선형: 이 또한 프로토타이핑과 비슷하게 시간이 많이 소요될 가능성이 있지만 조금 더 나은 모델로 큰 문제 발생을 피할 수 있다.

![spiral model](https://www.tutorialspoint.com/sdlc/images/sdlc_spiral_model.jpg)

###소프트웨어 개발 방법론이란?
> 소프트웨어를 생산하는데 필요한 반복적인 과정들을 정리한 것
> 
> 구조적 프로그래밍, 객체지향, 고속 개발방법론, 익스트림 프로그래밍(agile), 스크럼(agile), UP

- Agile 개발 프로세스: 다른 고전적인 방법론과 구별되는 가장 큰 차이점은 less doceumet-oriented 즉 문서를 통한 개발 방법이 아니라, code-oriented 실질적인 코딩을 통한 방법론

>앞을 예측하며 개발을 하지 않고 일정한 주기를 가지고 끊임없이 프로토타입을 만들어내며 그때 그때 필요한 요구를 더하고 수정하여 하나의 커다란 소프트에어를 개발해 나가는 adaptive style
>
>특정 개발론을 가리키는 말은 아니고 개발에 적합한 여러 다양한 방법론을 콤비하는 방법론

####UML
>Unified Modeling Language 
>
>통합 모델링 언어/표준화된 범용 모델링 언어
>
>객체 지향 소프트웨어 집약 시스템을 개발할 때 산출물을 명세화, 시각화, 문서화할 때 사용

####TDD(test driven development)
1. 결함을 점검하는 자동화된 테스트 케이스를 작성
2. 케이스를 통과하기 위한 최소한의 양의 코드를 생성
3. 새 코드를 표준에 맞도록 리팩토링

> 클래스, method 단위로 test 작성
> 시간이 오래걸릴 수도 있으며 생각지 못한 case들이 발생할 수 있지만 차후에 버그 발생률을 낮출 수 있다. 

####PDD(plan driven development)
>: 계획 기반 개발
>계획을 세우고 그 계획을 실천하는데에 많은 시간과 노력을 할애하는 개발 방법

####형상관리
: SW개발 및 유지보수 과정에서 발생하는 소스코드, 문서, 인터페이스 등 각종 결과물에 대해 형상을 만들고, 이를 형상에 대한 변경을 체계적으로 관리, 제어하기 위한 활동
######프로젝트관리: 문서, 일정, 예산, 인력, 고객, 위험, 품질관리 ...

####버전관리

#프로그래밍 언어

> 이 3언어는 저급 언어 0,1로 바뀌는 시점에 따라 구분된다.
> 
> 변화하는 과정이 컴파일러
> 
> ![컴파일과 인터프리터 비교](https://ruslanspivak.com/lsbasi-part1/lsbasi_part1_compiler_interpreter.png)


###컴파일 언어: C, C++, Go 
> 다 작성하고 컴파일언어는 배포까지 0,1로 컴파일해서 실행
>
> 장점: 한꺼번에 컴파일 된 상태이기에 실행속도가 빠르다. 해석되서 사용자가 사용하기에 원본 소스이 유출이 없다. 
>
> 단점: 규모가 커지면 컴파일이 오래걸려 빌드, 검증과정이 길어진다. 생산성이 떨어진다. 
> 
> 사용: 무거운 프로그램을 만들때

###바이트코드 언어: Java, C#.
>가상 컴퓨터에서 돌아가는 실행프로그램을 위한 2진 표현법이다.

> **자바 -> 중간언어 -> (시스템에 맞게 해석하는 해석기) virtual machine -> 기계언어**
> ![](http://1.bp.blogspot.com/-0XOWFJAvjdA/VB6myvVTWmI/AAAAAAAABzY/cENBcPg0n64/s1600/jvm.png)
> 
> 장점: 시스템마다 빌드를 해주지 않아도 된다. 여러 시스템에 배포 가능하다. 
> 
> 단점: 최적화가 어렵다. 인터프리터보다는 빠르지만 컴파일언어보다 무겁게 돌아간다. 시스템 소스 소모가 많다.

###인터프리터 언어: BASIC, JAVA Script, Python , Ruby
> 바로바로 전환...
> 
> 장점: 빠르게 개발할 수 있게 해준다. 사용자가 사용하고 있을 때도 동시 수정이 가능하다. 생산성이 높음. 
>
> 단점: 시스템 자원 소모율, 소스코드 노출 가능성도 있음, 느리다. 이를 극복하기 위해 미리 컴파일을 하여 속도를 높일 수 있다.(Just-In-Time complier)
>
> 사용: 개발과 수정(유지보수)이 빠르게 일어날 때, 위기대처가 중요할 때. 

#프로그래밍 패러다임
>: 프로그램을 보는 시각, 프로그래밍의 관점
>
>현재의 트랜드는 객체지향에서 함수형으로 바뀌는 추세. 이는 병렬구조의 작업을 가능하기 때문이다. 
>
>**Python: 다중 패러다임**

###객체지향 프로그래밍 패러다임
>: Objective-C, Python, Swift, C++, C#, Smalltalk, Perl Ruby, Java
>
>프로그램을 상호작용하는 객체들의 집합으로 표현

###함수형 프로그래밍 패러다임
>: Python, Swift, C++, C#, Perl, Ruby, Java
>
> 프로그램을 상태값을 지니지 않는 함수값들의 연속으로 표현


##객체지향 프로그래밍 패러다임
>: 컴퓨터 프로그램을 명령어의 목록으로 보는 시각에서 벗어나 여러개의 독립된 단위, 즉 **"객체"**들의 모임으로 파악하여, 객체간의 상호작용으로 프로그램의 동작을 구현하고자 하는 것.
>
>객체: 의식과 행위를 가지는 형체
>
>클래스: 객체의 공통점/성질들의 표현하는 코드, 객체가 가질 수 있는 속성과 행위를 정의하는 틀 (설계도, 템플릿)
>
>인스턴스 (instance): 메모리 상의 모든 요소들. 이 들중 클래스로 부터 상속되어 본딴 instance가 object

> ![](https://image.slidesharecdn.com/introductiontoobjectorientedprogramming-141212062931-conversion-gate01/95/introduction-to-object-oriented-programming-23-638.jpg?cb=1448026569)

> ![](http://net-informations.com/faq/oops/img/class.png)


#프로그래밍 용어
> 개발자 developer: 만드는 사람. 
> ex: 프로그래머, 디자이너, 기획자

####Software developer
> 소프트웨어를 만드는 사람

####Server/Client
> 데이터를 제공하고 받는 관계

####Front-end/Back-end
> front-end(API): 클라이언트와 맞다는 부분
> 
> back-end:  자원들에 가깝게 있거나, 또는 요구되는 자원들과 교신할 수 있는 능력을 가지는 등을 통해 프론트엔드 서비스를 간접적으로 지원한다
![frontend and backend](http://3nytechnology.com/wp-content/uploads/2014/09/Website-Frontend-and-Backend.jpg)

####Thread
> 프로세스 내에서 작업이 실행되는 흐름의 단위
> 
> **Multi Thread**: 프로세스 안에서의 병렬적 구조의 작업 흐름

####Library
> 특정기능을 수행할 수 있는 클래스 또는 함수의 집합체
> 
>- 수학 라이브러리 
>- 애니메이션 라이브러리
>- 문자열 라이브러리

####API
>Application Programming Interface
>응용소프트에어 프로그래밍 접합부 (프로그램간의 소통/데이터 전달 창구)
>
>응용소프트웨어와 프레임워크 사이의 중간매체(방법)
>
>소프트웨어 간의 통신을 위해 메시지를 전달하는 방식 등이 결절된 것

####Framework
>*구조적으로 고정된 부분을 재사용할 수 있도록 하고, 응용별 특정 기능을 추가적인 사용자 작성 코드에 의해 선택적으로 구현 가능하도록 하는 포괄적인 추상 구조, 그리고 이를 지원하는 소프트웨어 플랫폼.*
>
>다시 말해, 프로그래밍을(으로 웹서버, 앱을 만들때,,,) 위한 구조와 환경(자주 쓰는 기능..등)을 제공하는 것이다. 

####디자인패턴
> 프로그램 개발에서 자주 나타나는 과제를 해결하기 위한 방법 중 하나
> 
>  과거의 소프트웨어 개발 과정에서 발견된 설계의 노하우에 이름을 붙여 이후에 재이용하기 좋은 형태로 묶어서 정리한 것.
> 
> 다시 말해, 설계패턴을 뜻하는 것이다. 
> 
> MVC(model view controller), MVVM(model view view model), Observer, Singleton, Prototype
> 
> 장고는... MVC
 
####Reference Document
> 레프런스 문서, API에 대해 서술해 놓은 문서

####IDE
> Integrated Development Enviornment 통합 개발 환경
> 
>ex - Pycharm, Visual studio

####SDK
>Software Development Kit 소프트웨어 개발에 필요한 도구의 모음
>
>IDE + FrameWork + Tools ...
>
>보통 framework 제작사들이 배포








