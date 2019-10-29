# chapter5  - 허혁

## 객체 지향 프로그래밍

### 서론
* 이 장에서는 정의가 모호한 객체 지향의 개념을 이를 구성하는 최소한의 요소인 캡슐화, 상속, 다형성를 통해 설명한다.

</br>

### 캡슐화?
* **알려진 명제** : *OO가 데이터와 함수를 쉽고 효과적으로 캡슐화 하는 방법을 제공하고 이를 통해 데이터와 함수의 명확한 분리와 데이터 은닉에 효과적이므로 OO를 정의하는 중요한 요소중 하나*로 꼽힌다.
    * 그러나 C언어에서도 header 파일을 통해 완벽한 캡슐화가 가능하다.

```
// point.h

struct Point; 
struct Point* makePoint(double x, double y);  
double distance (struct Point *p1, struct Point *p2)    
// point.h 를 사용하는 쪽에서는 struct Point의 멤버는 접근할 수 없다.
```

</br>

* C++에서 이 완벽한 캡슐화가 깨지게 되었다
    * public, private, protected 키워드를 통해 어느 정도 보완은 하였으나 컴파일러가 헤더 파일에서 멤버 변수를 볼 수 있어야 하므로 (컴파일러는 클래스의 인스턴스 크기를 알 수 있어야 하므로) 임시방편일 뿐이었다.


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

</br>

* Java, C#은 헤더 구조를 버렸고 캡슐화는 더욱 어려워지게 되었고 클래스 선언과 정의를 구분하는것이 불가능하게 되었다.
* 결론적으로 OO가 강력한 캡슐화가 가능하다는 명제는 성립하지 않는다.
* 또한, OO는 프로그래머가 올바른 행동을 함으로써 캡슐화된 데이터를 우회 사용하지 않을 것이라는 암묵적 약속에 기반한다.

<br/>
<br/>


### 상속?

* **알려진 명제** : *OO는 상속을 확실히 제공한다*
    * 상속이란 단순히 어떤 변수와 함수를 하나의 유효 범위로 묶어서 재정의하는 일에 불과하다.
    * 이런 방식은 C 언어나 OO 언어가 없었던 시기에도 구현 가능했던 기능이다.

```
namedPoint.h

struct NamePoint;

struct NamedPoint* makeNamedPorint(double x, double y, char* name);
void setName(struct NamedPoint* np, char* name);
char* getName(struct NamedPoint* np);
```

```
main.c

#include "point.h"
#include "namedPoint.h"
#include <studio.h>

int main(int ac, char** av) {
    struct NamedPoint* origin = makeNamedPoint(0.0, 0.0, "origin");
    struct NamedPoint* upperRight = makeNamedPoint(1.0, 1.0, "upperRight");

    printf("distance=%f\n", 
        distance(
            (struct Point*) origin,
            (struct Point*) upperRight)
        );
}
```

* `위의 코드를 살펴보면 NamedPoint 데이터 구조가 Point 데이터 구조로 부터 파생된 구조인 것처럼 동작한다는 사실을 볼 수 있다.  이는 NamedPoint에 선언된 두 변수의 순서가 Point와 동일하기 때문이다. 다시 말해 NamedPoint는 Point의 가면을 쓴 것처럼 동작할 수 있는데, 이는 NamedPoint가 순전히 Point를 포함하는 상위 집합으로, Point에 대응하는 멤버 변수의 순서가 그대로 유지되기 떄문이다.
눈속임처럼 보이는 이 방식은 OO가 출현하기 이전부터 프로그래머가 흔히 사용하던 기법이었다. 실제로 C++에서는 이 방법을 이용해서 단일 상속을 구현했다.`
* 하지만 OO의 상속만큼 유용하고 합리적인 방식이 아니므로 OO 이전에 상속과 비슷한 기법이 사용되었다고 말하기에는 어폐가 있다.
* main에서 origin과 upperRight를 강제 타입 변환하였지만 OO에서는 암묵적 업캐스팅이 이루어진다.
* 결론적으로 OO가 새로운 개념을 만든것은 아니지만 사용성에 향상을 가져왔다는 점은 인정할 만 하다.

<br/>

### 다형성?

```
#include <stdio.h>

void copy() {
    int c;
    while ((c=getchar()) != EOF)
        putchar(c);
}
```
getchar는 STDIN에서 문자를 읽고 putchar는 STDOUT으로 문자를 쓴다.
getchar와 putchar는 STDIN과 STDOUT 타입에 행위를 의존하는 다형적인 함수이다.

</br>

* UNIX 운영체제는 모든 입출력 장치 드라이버가 표준 함수를 제공할 것을 요구한다.
```
struct FILE {
    void (*open)(cahr* name, int mode);
    void (*close)();
    int (*read)();
    void (*read)(char);
    void (*seek)(long index, int mode);
}
```

* 콘솔용 입출력 드라이버에 이 함수를 아래와 같이 정의한다.
```
#include "file.h"

void open(char* name, int mode) {/*...*/};
void close() {/*...*/};
int read() {int c;/*...*/ return c;};
void write(char c) {/*...*/};
void seek(long index, int mode) {/*...*/}

struct FILE console = {open, close, read, write, seek};
```

* 이제 STDIN을 FILE*로 선언하면, STDIN은 콘솔 데이터 구조를 가리키므로 getchar()는 아래와 같은 방식으로 구현할 수 있다.
```
extern struct FILE* STDIN;

int getchar() {
    return STDIN->read();
}
```
: getchar()는 STDIN으로 참조되는 FILE 데이터 구조의 read 포인터가 가리키는 함수를 호출한다.

* 함수를 가리키는 포인터를 응용한 것이 다형성이라고 정의할 수 있다.
* 40년대 후반 폰 노이만 아키텍처가 처음 구현된 후 다형적 행위를 수행하기 위해 함수를 가리키는 포인터를 사용해 왔으며 OO에서 새롭게 만든 것은 전혀 없다.
* OO에서는 다형성을 안전하고 편하게 사용할 수 있게 해준다.

</br>

#### 다형성이 가진 힘
위의 copy()는 입출력 드라이버의 소스 코드에 의존하지 않기 때문에 새로운 입출력 장치가 추가되어도 copy()함수는 어떠한 수정도 필요로 하지 않는 장치 독립적인 plugin 이라고 할 수 있다.
plugin architecture는 이처럼 입출력 장치 독립성을 지원하기 위해 만들어졌고 등장 이후 거의 모든 운영체제에서 구현되었다.
`그러나 대다수의 프로그래머는 이런 개념을 확장 적용하지 않았는데 함수를 가리키는 포인터를 사용하면 위험도가 올라가기 때문이다.`
OO의 등장으로 언제 어디서든 플러그인 아키텍처를 적용할 수 있게 되었다.

#### 의존성 역전
다형성 적용이 안전하고 편리해지기 전에는 고수준 함수가 저수준 함수를 호출하려면 피호출 함수를 포함한 클래스를 import 해야만 했다.

    1. ML1은 F()이라는 함수를 가지고 있다.
    2. 인터페이스I는 ML1을 상속한다
    3. HL1은 인터페이스I를 통해 F()를 간접 호출한다.

> ML1과 인터페이스I 사이의 소스 코드 의존성 (상속관계)는 제어흐름과 반대인 의존성 역전 형태가 된다. </br>
> OO가 다형성을 안전하고 편하게 제공한다는 의미는 의존성을 어디서든 의존(상속)관계를 역전시킬 수 있다는 의미이기도 하다.

* 의존성 역전을 이용해 UI와 Database가 업무 규칙에 의존하게 만들 수 있다. 
* 업무 규칙, UI, DB 3개의 분리된 컴포넌트로 컴파일할 수 있다. 
* 업무규칙, UI, DB는 서로 의존적이지 않게 되고 각각 독립적 배포가 가능해진다
* 이로인해 업무 규칙은 독립성을 갖게 되고 독립적인 배포 및 협업이 가능해진다.



<br/>

### 결론
*  OO란 다형성을 이용하여 소스 코드간 의존성의 절대적 제어 권한을 획득하는 것이라고 정의할 수 있다.
* OO를 사용하면 플러그인 아키텍처를 구성할 수 있고 
* 이를 통해 고수준의 정책을 포함하는 모듈은 저수준의 세부사항을 포함하는 모듈에 대해 독립성을 보장할 수 있다.
* 저수준 세부사항은 낮은 중요도의 플러그인 모듈을 만들수 있고 고수준 모듈과 독립적으로 개발하고 배포할 수 있다.