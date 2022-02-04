# Chapter3-object-oriented

## 다형성과 추상 타입
- 객체 지향의 장점 중 하나인 구현 변경의 **유연함**을 얻기 위한 또다른 방법은 **추상화**에 있다.

---

## 상속
- `한 타입을 그대로 사용하면서 구현을 추가할 수 있도록 해주는 방법`을 말한다.
- 즉, 기존 기능을 확장해서 새로운 기능을 구현할 때 사용한다.
- 기존에 정의된 메소드를 `재정의(overriding)`해서 확장할 수도 있다.

```kotlin
// Coupon 클래스 정의
open class Coupon constructor(private val discountAmount: Int) {
    fun getDiscountAmount(): Int {
        return discountAmount
    }

    open fun calculateDiscountedPrice(price: Int): Int {
        if (price < discountAmount) return 0
        return price - discountAmount
    }
}

// Coupon클래스를 상속받아 LimitPriceCoupon클래스 정의
class LimitPriceCoupon constructor(private val limitPrice: Int,
                                   @get:JvmName("limitPriceCoupon") private val discountAmount: Int)
    : Coupon(discountAmount) {
    fun getLimitPrice(): Int {
        return limitPrice
    }

    override fun calculateDiscountedPrice(price: Int): Int {
        if (price < limitPrice) return price
        return super.calculateDiscountedPrice(price)
    }
}

fun main() {
    val lpCoupon = LimitPriceCoupon(5000, 1000)
    val discountAmount = lpCoupon.getDiscountAmount()
    println("discountAmount : $discountAmount")
    val limitPrice = lpCoupon.getLimitPrice()
    println("limitPrice: $limitPrice")
}
```

---

## 다형성과 상속
- `다형성(Polymorphism)`은 한 객체가 여러 가지 모습을 갖는다는 것을 말한다.
- 즉, 한 객체가 여러 타입을 가질 수 있다는 것을 뜻한다.
```kotlin
package chapter3_2

open class Plane {
    fun fly() { }
}

interface Turbo {
    fun boost()
}

class TurboPlane : Plane(), Turbo {
    override fun boost() { }
}

fun main() {
    val tp = TurboPlane()
    tp.fly()
    tp.boost()

    val p: Plane = tp
    p.fly()
    val t: Turbo = tp
    t.boost()
}
```
- 위 예시는 `TurboPlane`클래스가 `Turbo`인터페이스와 `Plane`클래스를 상속받고 정의된 메서드를 실행할 수 있으며, `TurboPlane`로 `Plane`, `Turbo`객체를 선언할 수도 있다. 
- 이처럼 타입을 상속받는 방법에는 크게 `인터페이스 상속`과 `구현(클래스) 상속`이 존재한다.
    - 인터페이스 상속 : 단순히 **타입 정의**만 상속받으며 구현은 상속받은 클래스에서 구현한다.
    - 구현(클래스) 상속 : 클래스 상속을 통해서 이루어지며, 구현 상속은 보통 상위 클래스에 정의된 기능을 재사용하기 위한 목적으로 사용된다. 때문에 클래스 상속은 `구현을 재사용`하면서 `다형성`도 함께 제공한다.

---

## 추상 타입과 유연함

### 추상화
- `추상화(abstraction)`는 데이터나 프로세스 등을 의미가 비슷한 개념이나 표현으로 정의하는 과정을 말한다.
- 단순히 구현 클래스에서 추상 타입을 이끌어 내는 것만 추상화가 아니라 `sum += mark`와 같은 코드도 실제로는 내부적으로 메모리에서 값을 읽고 변경하고 다시 재할당해주는 과정이지만, 이 과정이 위 코드에는 드러나지 않으며 컴퓨터가 수행하는 일련의 처리 과정을 개념적으로 `추상화하고 있다`고 볼 수 있다.
- 상속받은 `interface`에 정의된 메소드나 데이터를 `class`로 모두 구현한 것을 `콘크리트 클래스(concrete class)`라고 부른다. 

### 추상 타입을 이용한 구현 교체의 유연함
- 코드

    ```kotlin
    interface LogCollector {
        fun collect()
    }

    class FtpLogFileDownloader : LogCollector {
        override fun collect() { println("FtpLogFileDownloader") }
    }

    class SocketLogReader : LogCollector {
        override fun collect() { println("SocketLogReader") }
    }

    class DbTableLogGateway : LogCollector {
        override fun collect() { println("DbTableLogGateway") }
    }

    fun main() {
        val ftpLogReader = FtpLogFileDownloader()
        var logs = Logs(ftpLogReader)
        logs.read()

        val socketLogReader = SocketLogReader()
        logs = Logs(socketLogReader)
        logs.read()

        val dbTableLogGateway = DbTableLogGateway()
        logs = Logs(dbTableLogGateway)
        logs.read()
    }

    class Logs(private val logCollector: LogCollector) {
        fun read() = logCollector.collect()
    }
    ```
- 결과

    ```text
    FtpLogFileDownloader
    SocketLogReader
    DbTableLogGateway
    ```

- 만약 `FTP`로 로그 기록을 가져오던 코드를 `Socket`으로 가져올 수 있도록 수정 요청이 들어왔을 때 단순히 `SocketLogReader`클래스만 생성한 후 `Logs` 클래스의 생성자로 넘겨주게 되면 다른 부분은 수정할 필요가 없이 변경이 가능하다는 장점이 있다.
- 이렇게 요구 사항이 바뀔 때 변화되는 부분은 이후에도 변경될 소지가 많으므로 추상 타입으로 변경 시 이후의 요구 사항이 바뀔때도 유연하게 대처할 수 있다.

### 인터페이스에 대고 프로그래밍하기
- 실제 구현을 제공하는 콘크리트 클래스를 사용하지 말고 기능이 정의된 인터페이스를 사용해서 개발해야한다.
- 단, 모든 곳에서 인터페이스를 사용하게 되면 구조가 복잡해질 수 있는 문제가 있어 `변화 가능성이 높은 경우에만 사용`해야 한다.
- 또한, 인터페이스를 작성할 땐 그 인터페이스를 사용하는 입장에서 작성해야하는데, 예를 들어 위 코드에서 `LogCollector` 인터페이스를 `IftpLogCollector`로 작성하게 되면 사용하는 입장에서는 ftp만 사용가능한 것으로 오해할 수 있기 때문이다.

---
