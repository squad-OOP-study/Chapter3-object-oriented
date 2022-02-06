# 다형성과 추상타입

### 상속이란?

한 타입을 그대로 사용하면서 구현을 추가할 수 있도록 해주는 방법

```kotlin
open class animal {
	fun sleep () {
		/* 구현 */
	}
}

class human : animal {
	/* 구현 */
}
```

- animal 클래스는 부모 클래스이고 human 클래스는 자식 클래스이다
- 자식 클래스는 부모 클래스에 정의된 구현을 물려받는다.

### Override

하위 클래스는 부모 클래스의 메서드를 새롭게 구현할 수도 있다.

```kotlin

Override fun sleep () { /* 재 정의 가능 */ }
```

## 다형성과 상속

### 다형성이란?

다형성은 한 객체가 여러가지 모습을 갖는다는 것을 의미한다. 여기서 모습이란 타입을 뜻하는데, 한 객체가 여러 타입을 가질 수 있다는 것을 뜻한다.

```kotlin
interface Pay {
    fun payment(x: Int, y: Int): Int
}

class CardPay : Pay {

    override fun payment(credit: Int, payment: Int): Int {
        return credit - payment
    }
}

class CashPay : Pay {

    override fun payment(cash: Int, payment: Int): Int {
        return cash - payment
    }
}

class Person() {
    lateinit var paymentType: Pay
    var Credit = 0
    var Cash = 0

    fun deposit(credit: Int, cash: Int) {
        Credit = credit
        Cash = cash
    }

    fun payment(amount: Int, payType: String) {
        if (payType == "Card") {
            paymentType = CardPay()
            Credit = paymentType.payment(Credit, amount)
        } else if (payType == "Cash") {
            paymentType = CashPay()
            Cash = paymentType.payment(Cash, amount)
        }
    }
}

fun main() {
    val person = Person()
    person.deposit(15000, 10000)
    person.payment(5000, "Cash")
    println(person.Cash)
}
```

이런식으로 Pay 라는 인터페이스는 CreditPay 와 CashPay의 두 모습을 가질 수 있다.

### 다형성 의 메모리 구조 살펴보기

다형성을 발생시키는 원인을 3가지로 요약할 수 있다.

1> 부모의 타입으로 자식의 객체를 생성할 수 있다.

2> 부모의 타입으로 자식의 객체를 받을 수 있다.

3> 부모의 메서드로 자식의 메서드를 호출할 수 있다. --> 오버라이딩 했다면  -> Virtual Method Invocation(VMI)에 의하여.

![a1](https://user-images.githubusercontent.com/83396157/152664430-299493bb-304c-4700-bfc2-d35a68957f6b.png)


Parent a3=new A();

는 부모의 타입으로 자식의 객체를 생성하는 경우다. 메모리를 볼때 설계도 영역에는 부모인 Parent 타입이 올라가고 힙에는 부모+자식(Parent+A)의 객체가 올라간다. 그리고 메모리의 특징에 의하여 a3는 원, 네모의 메서드만 사용할 수 있다.

**[출처]** [메모리 구조 기본 (1)-섹션 143](https://blog.naver.com/honnynoop/22805940)|**작성자** [자바자바](https://blog.naver.com/honnynoop)

### 메서드 오버로딩

![a2](https://user-images.githubusercontent.com/83396157/152664434-44a71d2d-5282-403f-99e7-872be659639b.png)

Parent d8  =  new A();

에서는 d8.원()을 호출하면 자식의 원() 메서드가 호출된다. 이것은 자식이 부모의 메서드를 오버라이딩했을 때 자동으로 부모의 메서드를 이용해서 자식의 메서드를 호출하는 메서드의 다형성이 된다. 원리는 부모의 메서드가 자식의 메서드를 자동으로 호출하는 데 이것을 VMI(Virtual Method Invocation)이라 한다. 물른 d8도 세모() 메서드를 사용할 수 없다. 네모() 메서드도 부모의 메서드다.

**[출처]** [메모리 구조 기본 (1)-섹션 143](https://blog.naver.com/honnynoop/22805940)|**작성자** [자바자바](https://blog.naver.com/honnynoop)

## 추상 타입과 유연성

추상화 는 데이터나 프로세스 등을 의미가 비슷한 개념이나 표현으로 정의하는 과정이다.

- 추상 타입과 실제 구현의 연결

추상 타입과 실제 구현 클래스는 상속을 통해서 연결된다.

- 변화 되는 부분을 추상화 하기

변화되는 부분을 추상화 하면 다른 클래스에서 적은 책임을 질 수 있도록 도와줄 수 있다. 그렇기 때문에 변화 되는 부분을 추상화 해주는 것이 좋다

- 인터페이스에 대고 프로그래밍 하기

기능을 정의한 인터페이스를 사용해서 프로그래밍 하라는 말. 하지만, 유연함을 얻는 과정에서 불필요하게 구조도 복잡해 지기 때문에 모든 곳에서 인터페이스를 사용해서는 안된다. 

- 인터페이스는 인터페이스 사용자 입장에서 만들기

인터페이스를 사용하는 클래스 입장에서 인터페이스의 기능이나 이름을 정의 하는것이 직관적이다.
