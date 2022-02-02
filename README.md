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

