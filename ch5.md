# chapter5  - 허혁

## 객체 지향 프로그래밍

### 서론
* OO의 정의에 대해 아래의 3가지 특성이 있는것으로 알려져 있습니다.

    - “데이터 함수의 조합”
    - “실제 세계를 모델링하는 새로운 방법”
    - “캡슐화, 상속, 다형성의 조합 또는 이 세 가지 요소를 반드시 지원하는 방법론”

1. 데이터 함수의 조합
    - OO를 발명하기 이전부터 데이터 구조를 함수로 전달하는 방식을 사용해 왔고 o.f()와 f(o)는 다르다는 의미를 내포하기 때문에 OO의 개념에 부합하지 않습니다.

2. 실제 세계를 모델링하는 새로운 방법
    - 말의 의미가 모호하며 목적또한 불분명하여 정의 자체가 너무도 모호하여 OO의 주요 구성요소로 볼 수 없습니다.

3. 마지막으로 캡슐화, 상속, 다형성의 조합 또는 세 요소의 필수적인 지원한다는 명제이며 이 장에서는 이에 대해 되짚어봅니다.

</br>

### 캡슐화?
    : 캡슐화가 OO를 정의하는 주요 개념이라고 볼 수 있는가?


* OO에서 데이터와 함수를 효과적으로 캡슐화 하는 방법을 제공하고 함수, 변수의 은닉이 용이하다라고 알려져 있지만 C언어제서도 header를 통해 완벽한 캡슐화가 가능하므로 OO만의 특성으로 설명하기에는 부족합니다.
</br>그 예로 아래의 코드를 보면 Point 스트럭처는 double 타입의 x,y 멤버변수를 가지고 있지만 외부에서 접근이나 인지할 방법이 없습니다.

```
// point.h

struct Point; 
struct Point* makePoint(double x, double y);  
double distance (struct Point *p1, struct Point *p2)    
// point.h 를 사용하는 쪽에서는 struct Point의 멤버는 접근할 수 없다.
```

```
// point.c
#include "point.h"
#include <stdih.h>
#include <math.h>

struct Point {
    double x, y;    // 헤더에 정의되지 않은 Point의 멤버변수는 외부 접근이 불가하다.
}

struct Point* makepoint(double x, double y) {
    struct Point* p = malloc(sizeof(struct Point));
    p->x = x;
    p->y = y;
    return p;
}

double distance(struct Point* p1, struct Point* p2) {
    double dx = p1->x - p1->x;
    double dy = p1->y - p2->y;
}

```

</br>

* 하지만 OO인 C++에서는 컴파일러가 인스턴스의 크기를 알 수 있어야 한다는 제약 때문에 헤더 파일에 멤버변수를 추가해야 해서 완전한 캡술화가 불가능해 집니다. </br>
컴파일러가 이를 막기는 하지만 사용자 입장에서는 멤버 변수의 존재가 인지 가능하므로 OO가 캡슐화를 완벽하게 제공한다라고 볼 수 없게 됩니다.</br>
또한 public, private, protected등의 접근 제한자로 보완은 하였으나 여전히 헤더에 멤버변수로 사용할 변수를 작성해야 하는 원칙에는 변함이 없어서 임시방편일 뿐이라고 합니다.</br>
자바와 C#에 이르러서는 헤더와 구현체로 분리하는 방식을 버렸고 이로 인해 캡슐화는 더욱 훼손되었으며 프로그래머가 올바른 행동을 함으로써 캡슐화된 데이터를 우회 사용하지 않을 것이라는 암묵적 약속에 의존하고있습니다.</br>
이와 같은 이유로 OO가 강력한 캡슐화가 가능하다는 명제는 성립하지 않으므로 캡슐화는 OO를 정의하는 요소로 볼 수 없습니다.


```
// point.h

class Point {
public:
    Point(double x, double y);
    double distance(const Point& p) const;

private:
    double x;
    double y;
};
// C++의 컴파일러는 클래스의 인스턴스 크기를 알 수 있어야 하므로 멤버 변수를 헤더에 선언해야 한다.
// 컴파일러가 멤버 변수의 접근은 제한하지만 사용자가 멤버 변수의 존재를 인지하게 된다.
// 이로인해 멤버 변수의 이름이 바뀌면 구현체 파일은 다시 컴파일해야 한다.
```

<br/>
<br/>


### 상속?

    : 상속이 OO를 정의하는 주요 개념이라고 볼 수 있는가?

```
// namedPoint.h

struct NamePoint;

struct NamedPoint* makeNamedPorint(double x, double y, char* name);
void setName(struct NamedPoint* np, char* name);
char* getName(struct NamedPoint* np);
```

```
// namedPoint.c

#include "namedPoint.h"
#include <stdlib.h>

// c1
struct NamedPoint {
    double x, y;
    char* name;
};

// c2
struct NamedPoint* makeNamedPoint(double x, double y, char* name) {
    p->x = x;
    p->y = y;
    p->name = name;
    return p;
}

void setName(struct NamedPoint* np, char* name) {
    np->name = name;
}

char* getName(struct NamedPoint* np) {
    return np->name;
}

```


```
// main.c

#include "point.h"  // m1
#include "namedPoint.h"
#include <studio.h>

int main(int ac, char** av) {
    struct NamedPoint* origin = makeNamedPoint(0.0, 0.0, "origin"); // m2
    struct NamedPoint* upperRight = makeNamedPoint(1.0, 1.0, "upperRight"); // m3

    // m4
    printf("distance=%f\n", 
        distance(
            (struct Point*) origin,
            (struct Point*) upperRight)
        );
}
```

* 위의 namedPoint 클래스에는
    - c1 x, y, name으로 구성된 NamedPoint 구조체
    - c2 x, y, name인자를 할당받는 NamedPoint 타입의 makeNamedPoint 인스턴스가 있습니다.
    - (그 아래의 setName, getName 함수가 있지만 설명에는 사용되지 않습니다.)

* 그 아래 메인에서는
    - m1 point 클래스를 참조하고
    - m2 x, y값을 0.0으로 name값을 "origin"으로 하는 origin
    - m3 x, y값을 1.0으로 name값을 "upperRight"으로 하는 upperRight 인스턴스를 생성하고
    - m4 origin과 upperRight를 Point 타입으로 캐스팅하여 </br>point 클래스에 정의된 distance 함수를 통해 계산된 값을 프린트를 합니다.
        > distance는 함수는 point 클래스에 정의되어 있으며</br>
        > Point 타입의 인자 2개를 받아 </br>
        > 첫번째 인자의 x값에서 두번째 인자의 x값을 뺀 값과</br>
        > 첫번째 인자의 y값에서 두번째 인자의 y값을 뺀 값을 더하여 루트값을 구하는 함수입니다.

<br/>

* NamedPoint 타입의 객체를 Point 클래스에 선언된 distance 함수에 전달하여 실행하는 형태로 보이는데 이는 NamedPoint와 Point에 정의된 멤버변수의 순서가 동일하기 때문에 가능합니다.<br/>
결국 NamePoint가 `Point를 상속받은 형태처럼 보이는 구조`가 되며 OO 이전부터 상속이 사용되어온 방식이라는 의미가 됩니다.<br/>
하지만 어디까지나 상속 처럼 보이는 방식이고 사용이 용이하지 않으며 다중 상속을 구현하려면 복잡도가 더 올라갑니다.<br/>
또한 distance 함수가 Point타입을 인자로 받기 때문에 업캐스팅을 해야했고 객체지향에서는 이 과정이 암묵적으로 특징이 있습니다.<br/>
캡슐화가 OO를 정의하는데 어느 정도 영향은 있으나 절대적인 정의라고는 할 수 없다고 말하고 있습니다.

<br/>

### 다형성?

    : 다형성이 OO를 정의하는 주요 개념이라고 볼 수 있는가?

#### 예1

```
#include <stdio.h>

void copy() {
    int c;
    while ((c=getchar()) != EOF)
        putchar(c);
}
```

> c 값이 존재하는 동안 putchar를 실행하는 카피 함수가 있습니다.
getchar 함수는 STDIN에서 문자를 읽고 
putchar 함수는 STDOUT으로 문자를 씁니다.
getchar()와 putchar()의 행위가 STDIO에 의존하므로 다형적이라고 할 수 있습니다.

</br>


#### 예2
```
struct FILE {
    void (*open)(cahr* name, int mode);
    void (*close)();
    int (*read)();
    void (*read)(char);
    void (*seek)(long index, int mode);
}
```

> 열기, 닫기, 읽기, 쓰기, 탐색의 유닉스 표준 함수를 호출하는 드라이버 코드

</br>

```
#include "file.h"

void open(char* name, int mode) {/*...*/};
void close() {/*...*/};
int read() {int c;/*...*/ return c;};
void write(char c) {/*...*/};
void seek(long index, int mode) {/*...*/}

struct FILE console = {open, close, read, write, seek};
```
> 위의 드라이버를 연결하는 플러그인 코드

</br>

```
extern struct FILE* STDIN;

int getchar() {
    return STDIN->read();
}
```

> FILE에 정의된 read() 함수를 호출하는 getcahr() 함수를 선언하여 플러그인 코드를 활용

* C++에서는 모든 가상함수가 vtable이라는 테이블에 포인터를 가지고 있고 모든 가상 함수 호출이 이 테이블을 거칩니다. 파생 클래스의 생성자는 생성하려는 객체의 vtable을 단순히 파생 클래스의 함수로 덮어씁니다.</br>
이런 형태로 함수를 가리키는 포인터를 응용한 것이 다형성이며 40년대에 폰 노이만 아키텍처가 구현된 이후 다형적 행위를 수행하기 위해 함수를 가리키는 포인터를 사용해 왔으므로 OO에서 새롭게 만들어진 것이 아닙니다.</br>
하지만 포인터를 직접 사용하는것은 
프로그래머가 이들 포인터를 초기화하는 관례를 준수해야 하고 관례를 망각한 경우 버그가 발생하고 찾아내 해결하기가 매우 힘들어 집니다.</br>
OO는 이런 관례를 없애줘서 다형성의 강력한 기능을 안전하게 사용 가능하도록 해줍니다.

</br>

#### 다형성이 가진 힘

* 위에서 작성한 copy() 함수는 입출력 드라이버의 소스코드에 의존하지 않기 떄문에 입출력 드라이버가 FILE에 정의된 표준함수를 구현한다면 copy() 함수는 재사용이 가능해집니다.</br>
드라이버가 플러그인의 형태가 되고 copy() 함수는 장치 독립적이라는 의미가 됩니다.</br>
이런 형태의 플러그인 아키텍처는 등장 이후 거의 모든 운영체제에서 구현되었으나 
함수를 가리키는 포인터를 사용하는것이 위험하기 때문에 대다수의 프로그래머에게 수용되지 못하였는데
OO의 등장으로 위험도가 낮아져 수용이 가능해 졌습니다.

#### 의존성 역전

고수준 함수(참조 하는 쪽)가 저수준 함수(참조 당하는 쪽)를 사용하기 위해 include, import, using등의 구문으로 참조할 클래스를 지정합니다. 이로 인해 제어흐름이 저수준에서 고수준으로 의존하게되는 구조가 됩니다.

> 그림 5.2에는 
> F() 함수를 가진 인터페이스와</br>
> F() 함수가 구현된 ML1 클래스</br>
> 인터페이스를 통해 간접적으로 ML1의 F() 함수를 호출하는 HL1 클래스의</br>
> 도식이 있습니다.

* ML1과 I 인터페이스 사이의 의존성은 제어흐름과 반대인 형태로 되어있고 이를 의존성 역전이라고 부릅니다.

* OO는 다형성을 안전하고 편하게 제공하므로 소스 코드 의존성을 어디에서든 적용 가능하게 해줍니다.
이로 인해 의존성 흐름의 제어가 완전히 가능해집니다.

* 이것이 아키텍트 관점에서 OO의 지향하는 바이고 OO의 정의라고 할 수 있다고 합니다.

* 이러한 의존성 역전을 활용하면 UI와 DB가 비지니스 로직에 의존하게 만들 수 있습니다.</br>
그림 5.3의 업무 규칙은 고정되고 독립적인 형태가 되고 UI와 DB가 플러그인이 됩니다. 업무 규칙에서 UI나 DB를 호출하지 않게 됩니다.</br>
업무규칙, UI, DB는 각각 분리된 컴포넌트로 컴파일할 수 있고 별도로 배포가 가능해집니다. 이로인해 각 모듈을 독립적으로 개발할 수 있고 개발 독립성을 확보할 수 있게 됩니다.


<br/>

### 결론
* OO란 <br/>
    - 다형성을 이용하여 전체 시스템의 소스 코드 의존성 제어 권한을 확보한다.
    - 플러그인 아키텍처를 구성할 수 있게되어 
    - 고수준 정책을 포함하는 모듈은 저수준의 세부사항을 포함하는 모듈에 대해 독립성을 보장할 수 있다.
    - 저수준의 세부사항은 중요도가 낮은 플러그인 모듈로 만들 수 있고 고수준의 정책을 포함하는 모듈과 별개로 개발 배포할 수 있다.

    로 정의할 수 있습니다.