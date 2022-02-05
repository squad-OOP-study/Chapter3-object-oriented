# ch3 다형성과 추상 타입

## < 상속 >

### < 상속? >
- 한 타입을 그대로 사용하면서 구현을 추가할 수 있도록 해주는 방법
- 자식 클래스를 만들 때 상위 클래스(부모 클래스)의 속성과 기능을 물려 받는다
- 상위 클래스의 프로퍼티와 메서드가 하위 클래스에 적용된다
<br>

### < 상속 in 코틀린 >
- 자바의 클래스와 메소드는 기본적으로 상속에 대해 열려있다
- but 코틀린의 클래스와 메소드는 기본적으로 final 이다
- 어떤 클래스의 상속을 허용하려면 코틀린에서는 클래스 앞에 open 변경자를 붙여야 한다
- 오버라이드를 허용하고 싶은 메소드나 프로퍼티 앞에도 open 변경자를 붙여야 한다
![그림1](https://user-images.githubusercontent.com/69443895/152631133-48dbfc6c-5618-4f33-a929-b7c6cab0b33a.png)

<br>

```kotlin
open class Coupon constructor(disCountAmount : Int){  // 부모 클래스
    private var disCountAmount = disCountAmount

    fun getDisCountAmount() = disCountAmount

    open fun calculateDiscountedPrice(price: Int) : Int{
        if(price < disCountAmount)
            return 0
        return price - disCountAmount
    }
}

class LimitPriceCoupon(limitPrice : Int, disCountAmount: Int) : Coupon(disCountAmount){ // 자식 클래스
    private var limitPrice = limitPrice

    fun getLimitPrice() = limitPrice

    override fun calculateDiscountedPrice(price: Int): Int {
        if(price < limitPrice) // 제품 가격이 제한 금액보다 작으면
            return price
        return super.calculateDiscountedPrice(price)
        
    }

}
```
- LimitPriceCoupon 클래스에는 getDiscountAmount 메소드가 정의되어 있지 않지만 Coupon 클래스를 상속하고 해당 메소드 또한 open 으로 정의가 되어 있으므로 오버라이딩 할 수 있다
- 메소드를 재정의하면, 해당 메소드를 실행할 때 상위 타입의 메소드가 아닌 하위 타입에서 재정의한 메소드가 실행된다
***
<br>

## < 다형성과 상속>
- 다형성(Polymorphism)은 한 객체가 여러 가지 모습을 갖는다는 것을 의미한다
- 모습? -> 타입을 뜻함 -> 다형성은 한 객체가 여러 타입을 가질 수 있다는 것!
  + 컴파일 타임에 결정되는 다형성 - 메서드 오버로딩
  + 런 타임에 결정되는 다형성 - 동적으로 구성되는 오버라이딩된 메서드 사용 시
<br>

- java, c++, c# 등의 언어는 다형성을 구현하기 위해 타입을 상속받는 방법 사용 : 1. 인터페이스 상속 / 2. 구현 상속
- #### 1. 인터페이스 상속
- 순전히 타입 정의만을 상속받음
- 인터페이스를 상속받은 클래스에서 인터페이스의 메서드를 구현한다

- #### 2. 구현 상속
- 클래스 상속을 통해 이루어짐
- 보통 상위 클래스에 정의된 기능을 재사용하기 위한 목적으로 사용
***
<br>

## < 추상 타입과 유연함 >

- 추상화 : 데이터나 프로세스 등을 의미가 비슷한 개념이나 표현으로 정의하는 과정
- 추상 타입은 구현을 제공할 수 없기 때문에, 보통 구현을 제공하지 않는 타입 (자바의 인터페이스 or c++의 추상메서드로만 구성된 추상 클래스)을 이용해서 정의한다
<br>

- 1. 추상 타입과 실제 구현 클래스는 상속을 통해 연결
- 구현 클래스가 추상 타입을 상속받는 방법으로 연결
<br>

- 2. 추상 타입을 이용한 구현 교체의 유연함
- 유지/보수에 용이
- 추가적인 요구사항 대응에 유리

### < 추상화 장점 >
- 공통된 개념을 도출해서 추상 타입을 정의해 주기도 하지만, 많은 책임을 가진 객체로부터 책임을 분리하는데 도움이 된다

### < 재사용 >
- 교재의 FileDataReader과 같이 조금 더 구체적이고 세부적인 기능을 수행하는 하위 수준의 로직보다 FlowController과 같이 전체적인 흐름을 제어하는 상위 수준의 로직을 재사용할 수 있도록 설계하는 것이 전체적인 유지/보수 측면에서 중요하다
- 이를 위해 책임을 분리하는 것이 중요
- 프로그램의 흐름을 제어하는 로직 부분으로부터 특정 조건에 따라 객체를 생성하는 등 다른 책임을 분리하는 방향으로 개발하기

***
<br>

## < 추상클래스 vs 인터페이스 >
1. 추상클래스
- abstract 메소드를 가지고 있는 클래스
- abstract 사용 시 클래스나 메소드에 open 키워드를 따로 명시하지 안아도 된다
- 자식 클래스는 추상 메소드를 반드시 구현해야 한다
- 생성자를 가질 수 있다
- 다중 상속 불가
<br>

2. 인터페이스
- 일반적으로 모든 메소드가 abstract인 클래스를 의미하지만, java 8 부터는 default 키워드를 통해 메소드 구현이 가능!
- 생성자x
- 다중 상속 가능
```java
public interface Animal {
  void eat(); 
  default void introduce() { 
  Systemp.out.println("안녕하세요");
  } 
}
```

- 코틀린에서는 인터페이스는 프로퍼티, abstarct 메소드, 일반 메소드 모두 가질 수 있다
```kotlin
interface Runner { 
    fun run() 
} 

interface Eater { 
    fun eat() { 
        println ("음식을 먹습니다")
    } 
} 

class Dog : Runner, Eater { 
    override fun run() {
        println ("뜁니다")
    }
    override fun eat() {
        println ("사료를 먹습니다")
    } 
}
```


***
<br>

참고링크
- https://yoo-young.tistory.com/89
- https://waves123.tistory.com/52
