#9장 LSP 리스코프 치환 법칙
* 1988년 바바라 리스코프는 하위 타입을 아래와 같이 정의 했다.

> 여기에서 필요한 것은 다음과 같은 치환 원칙이다. S 타입의 객체 o1 각각에 대응하는 T 타입 객체 o2가 있고, T 타입을 이용해서 정의한 모든 프로그램 P에서 o2의 자리에 o1을 치환하더라도 P의 행위가 변하지 않는다면, S는 T의 하위 타입니다.

## 상속을 사용하도록 가이드하기

```Kotlin
class Billing {
  fun calcFee(license: License) {
    license.calcFee()
  }
}

interface License {
  fun calcFee()
}

class Personal {
  override fun calcFee(){
  ...
  }
}

class BusinessLicense {
  ovrride fun clacFee(){
  ...
  }
  
  val users: List<User>
}

```

* 이 설계는 LSP를 준수한다.
* Billing 애플리케이션의 행위가 License 하위 타입 중 무엇을 사용하는지에 전혀 의존하지 않기 때문이다.
* 이들 하위 타입은 모두 License 타입을 치환할 수 있다.

## 정사각형/직사각형 문제
LSP를 위반하는 전형적인 문제로는 유명한 정사각형/직사각형 문제가 있다.

```Kotlin
open class Rectangle {
  var h: Int = 0
  var w: Int = 0
  
  fun setH(h: Int){
    this.h = h
  }
  fun setW(w: Int) {
    this.w = w
  }
  
  fun area(): Int = w * h
}

class Square(){
  
  fun setSide(side: Int){
    this.w = side
    this.h = side
  }
  
  fun setH(h: Int){
    setSide(h)
  }
  fun setW(w: Int) {
    setSide(w)
  }
}
```

* 이경우에 ```Square``` 는 ``` Rectangle```의 하위 타입으로는 적핮하지 않다.
** ```Rectangle```의 높이와 너비는 서로 독립적으로 변경될 수 있다.
** ```Square```의 높이와 너비는 반드시 함께 번경된다.

```Kotlin
val r = Rectangle()
r.setW(5)
r.setW(2)
assert(r.area() == 10)  // Success
```

```Kotlin
val s = Square()
s.setW(5)
s.setW(2)
assert(r.area() == 10) // Fail
```

* 이런 형태의 LSP 위반을 막기 위한 유일한 방법은 (if문 등을 이용해서)  ```Square``` 나 ``` Rectangle``` 을 사용하는 쪽에서 검사하는 메커니즘을 추가해야 한다.
** 이렇게 된다면 사용하는 쪽의 행위가 타입에 의존하게 되므로 차환해서 사용할 수 없다.

## LSP 아키텍처

* 객체 지향이 등장한 초기에는 앞선 예제처럼 LSP는 상속을 사용하도록 가이드 하는 방법 정도로 간주되었다. 
* 시간이 지나면서 LSP는 인터페이스와 구현체에도 적용되는 더 광범위한 소프트웨어 설계 원칙으로 변모해 왔다.
** 자바라면 인터페이스 하나와 이를 구현하는 여러개의 클래스로 구성
** 루비라면 동일한 메서드 시그니처를 공유하는 여러개의 클래스로 구성
* LSP는 이보다 더 많은 경우에 적용 할 수 있다.
** 잘 정의된 인터페이스와 그 인터페이스의 구현체끼리의 상호 치환 가능성에 기대는 사용자들이 존재하기 때문이다
* 아키텍처관점에서 LSP를 이해하는 최선의 방법은 이 원칙을 어겼을 때 시스템 아키텍처에서 무슨 일이 일어나느지 관찰하는 것이다.

## LSP 위배 사례

* 다양판 택시 파견 서비스를 통합하는 애플리케이션을 만들고 있다고 가정해보자.
* 택시 파견 REST 서비스의 RUI가 운전기사 데이터베이스에 저장되어 있다고 가정해보자.  
** 시스팀에 고객에게 알맞은 기사를 선택하면, 해당 기사의 레코드로부터 URI 정보를 얻은 다음, 그 URI 정보를 이용하여 해당 기사를 고객 위치로 파견한다.  

```purplecab.com/driver/Bob```  

필요한 정보를 덧붙이면 다음과 같다.

```
purplecab.com/driver/Bob
  /pickupAddress/24 Maple St.
  /pickupTime/153
  /destination/ORD
```
* 이 예제에서 분명한 점은 파견 서비스를 만들 때 **다양한 택시 업채**에서 **동일한 REST 인터페이스를 반드시 준수**하도록 만들어야 한다.
* 다른 택시업체(acme)에서 이 룰을 따르지 안하고 ```destnation```필드를 ```dest```로 축약해서 사용했다면 어떻게 될까? 그리고 그 회사가 합쳐졌다면 어떻게 될까? 

```
if(driver.getDispatchUri().startsWith("acme.com"))...
```
이런 식의 if문을 추가해야 된다.
* 하지만 실력있는 아키텍트라면 당연히 이런 식으로 구성하는 것을 용납하지 않는다.
** "acme.com"라는 단어를 코드 자체에 추가하면, 끔찍할뿐만 아니라 이해할 수도 없는 온갖 종류의 사이드 이펙트가 발생 할 수 있다.
** 또다른 회사를 통합한다면 또 다른 if문을 추가해야 할 수도 있다.
* 아키텍트는 REST 서비스들의 인터페이스가 서로 치환 가능하지 않다는 사실을 처리하는 중요하고 복잡한 매커니즘을 추가해야 한다.

## 결론
* LSP는 아키텍쳐 수준까지 확장할 수 있고, 반드시 확장해야만 한다.
* 치환 가능성을 조금이라도 위배하면 시스템 아키텍처가 오염되어 상당량의 별도 메커니즘을 추가해야 할 수 있기 때문이다.
