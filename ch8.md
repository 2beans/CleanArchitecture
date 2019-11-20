# 8장 OCP (Open Closed Principle) 개발-폐쇄 원칙

## <span style="color:MediumSeaGreen">서론</span>

#### <span style="color:MediumSeaGreen">OCP 개방-폐쇄 원칙이란</span>
1980년 버트란트 마이어에 의해 만들어진 개념으로</br>
기존 코드를 수정하기보다 `새로운 코드를 추가하는 방식으로 시스템의 행위를 변경할 수 있도록 설계`해야만 시스템을 쉽게 변경할 수 있다는 원칙

* 소프트웨어 **개체**는 확장에는 열려 있고 변경에는 닫혀 있어야 한다.</br>
즉 행위는 확장 가능해야 하지만 산출물(output)을 변경해서는 안된다는 원칙</br>
* 이 원칙이 SW 아키텍처를 공부하는 가장 근본적인 이유이며</br>
* 작은 규모의 요구 사항을 적용하는데 많은 수정을 필요로 한다면 실패한 아키텍트</br>
* OCP는 클래스와 모듈의 설계보다 아키텍처 컴포넌트 수준에서 더 큰 의미를 가지며 다음의 사고 실험을 통해 이를 설명
</br></br>

#### <span style="color:MediumSeaGreen">사고 실험</span>

: 변경전 상황과 요구 사항
1. 재무제표를 웹 페이지로 보여주는 시스템으로
2. 재무제표 데이터를 스크롤하여 열람할 수 있고 음수는 빨간색으로 출력함
3. 이 정보를 보고서 형태로 변환해서 흑백 프린터로 출력하는 기능을 요청 받음
4. 출력물의 각 페이지에는 해더, 푸터, 넘버가 있으며 
5. 표의 각 열에는 레이블이 있고 음수는 괄호로 감싸서 표현
</br></br>

*코드 변경량이 0에 가까울 수록 이상적인 아키텍트이며 이를 위해 코드의 변경량 줄이기위해서는*
* 단일 책임 원칙(SRP)을 적용하여 서로 다른 목적으로 변경되는 요소를 분리하고
* 의존성 역전 원칙(DIP)을 적용하여 이 요소들 사이의 의존성 체계를 아래와 같이 분리함
    1. 재무 데이터를 분석하는 재무 분석 모듈
    2. 재무 분석 모듈을 통해 산출된 보고서용 재무 데이터를 웹으로 표시하는 모듈과 프린터로 출력하는 모듈로 분리

위 과정으로 `웹으로 표시하는 모듈`과 `프린터로 출력하는 모듈`은 서로 각자의 역할로 책임이 분리됨</br>
책임을 분리했다면 두 모듈중 어느 한쪽에 변경, 확장이 생기더라도 서로 간섭되지 않도록 의존성을 조직화 해야 함</br>
이러한 목적을 달성하려면 처리 과정을 클래스 단위로 분할하고 이 클래스들을 컴포넌트 단위로 구분하여야 함</br></br>


> 컨트롤러, 인터랙터, 데이터베이스, 프레젠터, 뷰 컴포넌트로 분리한 그림 8.2를 통해 설명

![그림 8.2](./fig8.2img.002.jpeg)

* 열린 화살표(녹색)가 A에서 B로 향하는 경우 A클래스가 B클래스를 사용하는 것을 나타냄
* 닫힌 화살표(주황색)가 A에서 B로 향하는 경우 A클래스가 B클래스를 구현, 상속하는 관계를 나타냄
* B에서는 A를 절대 호출하지 않음
* FinancialDataMapper는 구현을 통해 FinancialDataGateway를 알고 있지만 
* FinancialDataGateway 인터페이스는 FinancialDataMapper에 대해 아무것도 알지 못함.

</br></br>

> 컴포넌트 간 의존관계를 나타낸 그림 8.3

![그림 8.3](./fig8.2img.003.jpeg)

* 컴포넌트간의 관계가 오직 한방향으로만 이루어져 있고 화살표는 변경으로 부터 보호하려는 컴포넌트를 향하도록 그려진다.</br>
* A 컴포넌트에서 발생한 변경으로부터 B 컴포넌트를 보호하려면 A 컴포넌트가 B 컴포넌트에 의존해야 하는데</br>
* 그림 8.3은 Presenter가 Controller에 의존하므로 Presenter에서 발생한 변경으로부터 Controller를 보호하고<br>
View가 Presenter에 의존하므로 View에서 발생한 변경으로 부터 Presenter를 보호한다.</br>
* 가장 높은 수준의 정책인 업무 규칙을 포함하여 기능적 중요도가 가장 높은 Interactor는
* 의존 구조 최상위에 위치하게 되고 자신 이외의 모든 컴포넌트(Database, Controller, Presenter, View)에서 발생한 변경에 영향을 받지 않아 OCP를 가장 잘 준수할 수 있는 형태가 됩니다.

* Interactor > Controller > Presenter > View 순으로 중요도 순위가 구성되며 이 형태가 아키텍처에서 OCP가 동작하는 방식이다.
* 아키텍트는 기능이 언제, 어떻게, 왜 발생하지에 따라 계층구조로 조직화하여</br>
저수준 컴포넌트에서 발생한 변경으로부터 고수준 컴포넌트를 보호할 수 있다.
</br></br>

## <span style="color:MediumSeaGreen">방향성 제어</span>
: 방향성 제어 측면에서의 OCP

> 그림 8.2를 참조하여 설명

![그림 8.2](./fig8.2img.001.jpeg)

의존성을 역전시키기 위해 FinancialDataGateway 인터페이스는 FinacialReportGenerator와 FinancialDataMapper 사이에 위치함.</br>
FinacialDataGateway 인터페이스가 없다면 의존성이 Interactor 컴포넌트에서 Database 컴포넌트로 바로 향하게 되어 의존성 역전 구조를 갖지 못하게 된다.</br>
FinancialReportPresenter 인터페이스와 2개의 View 인터페이스(Screen View, Print View)도 같은 목적을 가짐
</br></br>


## <span style="color:MediumSeaGreen">정보 은닉</span>
: 정보 은닉 측면에서의 OCP


* FinancialReportRequester 인터페이스는 FinancialReportController 가 Interactor 내부에 대해 너무 많이 알지 못하도록하기 위해 존재하며
* 이 인터페이스가 없었다면 Controller가 FinancialReportGenerator를 직접 사용하게 되어 FinancialEntities에 대한 추이 종속성이 발생되며
* 이로 인해 '자신이 직접 사용하지 않는 요소에는 절대로 의존해서는 안 된다'는 소프트웨어 원칙을 위반하게 됨. </br>

Controller에서 발생한 변경으로부터 Interactor를 보호하는 일의 우선순위가 가장 높지만 반대로 Interactor에서 발생한 변경으로부터 Controller도 보호되기를 바라며 이를 위해 Interactor 내부를 은닉하기 위해 인터페이스가 사용됨</br></br>

## <span style="color:MediumSeaGreen">결론</span>
* OCP는 아키텍처를 떠받치는 원동력중 하나
* OCP의 목적은 확장, 변경이 쉬운 구조를 갖추어 시스템 변경으로인한 영향을 최소화 하는데 있다.
* 이를 위해 시스템을 컴포넌트 단위로 분리하고
* 저수준 컴포넌트에서 발생한 변경으로부터 고수준 컴포넌트를 보호할 수 있는 형태로 의존성 계층구조를 형성해야 한다.

