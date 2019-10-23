# chapter2  - 허혁

## 두 가지 가치에 대한 이야기

### 행위
* 클라이언트의 요구를 만족시키는 프로그램을 작성하는 것을 행위(기능 구현)라 정의하며 이를 첫 번째 가치로 서술하고 있다.
* 많은 프로그래머가 이 과정에서 수행하는 구현과 디버깅을 주요 업무로 생각하지만 이는 주업무가 되어서는 안된다고 함.

### 아키텍처
* 소프트웨어란 변경이 용이한 툴이어야 하며 이를 두번째 가치로 정의하고 있다.클라이언트의 지속적으로 변경되는 요구사항과 함께 복잡도가 증가되는 것을 방지하며 변경에 대응 가능한 구조를 형성하여야 한다. 

### 더 높은 가치
* 정상동작하지만 수정 불가한 프로그램을 쓸모가 없고 불완전 하더라도 수정이 용이한 프로그램이라면 가치가 있다라고 서술하고 있다.

### 아이젠하워 매트릭스
* 긴급한 문제가 중요할 경우는 드물며 중요한 문제가 급한 경우는 거의 없다. 행위(기능 구현)는 긴급하지만 항상 중요도가 높은것은 아니다, 아키텍처는 중요하지만 긴급하게 필요로 하는 경우는 절대없다고 말하고 있다.
* 긴급하지만 중요하지 않은 일에 집중하게 되고 이로인해 아키텍처를 무시하게 되는 경우를 경계하여야 한다.

### 아키텍처를 위해 투쟁하라
* 개발자는 이러한 소프트웨어  보호에 대한 책임이 있다.
* 아키텍처가 후순위가 되면 개발 비용이 더욱 커지며 변경이 불가능한 시스템이 된다.
* 이런 상황이 되었다면 자신의 본분을 다하지 않은것이라는것을 강조하고 있다.