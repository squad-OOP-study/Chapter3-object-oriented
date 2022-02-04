---

# Chap03

## 들어가면서  
객체 지향이 주는 장점은 구현 변경의 유연함이다.  
유연함을 얻을 수 있도록 해주는 방법에는 추상화가 있는데 그 전에 추상화를 가능하게 해주는 다형성에 대해 
살펴보자.
<br>
<br/>
## 상속(inheritance)이란
- 상속(inheritance)이란 기존의 클래스에 기능을 추가하거나 재정의하여 새로운 클래스를 정의하는 것을 의미한다.
- 이러한 상속은 캡슐화, 추상화와 더불어 객체 지향 프로그래밍을 구성하는 중요한 특징 중 하나이다.

### 상속의 장점
- 기존에 작성된 클래스를 재활용할 수 있다.
- 자식 클래스 설계 시 중복되는 멤버를 미리 부모 클래스에 작성해 놓으면, 자식 클래스에서는 해당 멤버를 작성하지 않아도 된다.
- 클래스 간의 계층적 관계를 구성함으로써 다형성의 문법적 토대를 마련한다.  

<br>
<br/>

**코틀린 상속의 예시**
```kotlin
open class Beginner(val name: String) {
    open fun attack() {
        println("$name 가 기본 공격을 수행합니다.")
    }
}

class Wizard(name: String) : Beginner(name) {
    override fun attack() {
        super.attack() // super 키워드를 통해서 부모클래스의 attack 메서드 수행
        println("$name 가 마법 공격을 수행합니다.")
    }
}

fun main() {
    val wizard = Wizard("stark")
    wizard.attack()
}

// 실행 결과
// stark 가 기본 공격을 수행합니다.
// stark 가 마법 공격을 수행합니다.
```

## 다형성
이름이 동일한 것처럼 보이지만 실행 결과를 다르게 가질 수 있는 것을 다형성(polymorphism) 이라고 한다.

### 타입 상속을 통한 다형성 구현

**코틀린에서 타입 상속을 통한 다형성 구현**
```kotlin
open class Plane {
    open fun fly() {
        // 비행 관련 동작
    }
}

interface Turbo {
    fun boost()  // 인터페이스에서 메서드는 open 키워드 없이 해당 인터페이스를 구현하는 클래스에서 사용 가능하다.
}

class TurboPlane : Plane(), Turbo {
    override fun fly() {
        super.fly()
        // 구현할 추가 동작 구문
    }
    override fun boost() {
        // 구현할 구문
    }
}
```  

<br>
<br/>

위의 코틀린 코드에는 두 개의 클래스와 한 개의 인터페이스가 있다.  
이 중 TurboPlane 클래스는 Plane 클래스를 상속 받고 있고, Turbo 인터페이스도 같이 상속받고 있다.  
이런 타입 상속 관계를 갖는 경우 다음과 같이 TurboPlane 타입의 객체에 Plane 타입이나 Turbo 타입에 정의된 메서드의 실행을 요청할 수 있다.

```kotlin
val tp: TurboPlane = TurboPlane()
tp.fly() // TurboPlane 에서 부모 클래스인 Plane 에 정의/ 구현된 메서드 실행
tp.boost() // Turbo 인터페이스에 정의되고 TurboPlane 에 구현된 메서드 실행
```

<br>
<br/>

또한 TurboPlane 타입의 객체를 Plane 타입이나 Turbo 타입에 할당하는 것도 가능하다.

```kotlin
val tp: TurboPlane = TurboPlane()

val p: Plane = tp // TurboPlane 객체는 Plane 타입도 된다.
p.fly()

val t: Turbo = tp // TurboPlane 객체는 Turbo 타입도 된다.
t.boost()
```
<br>
<br/>

## 추상화
컴퓨터 과학에서 추상화란 복잡한 자료, 모듈, 시스템 등으로부터 핵심적인 개념 또는 기능을 간추려 내는 것을 말한다.  
추상화는 이처럼 구체적인 사물들간의 공통점을 취하고 차이점을 버리는 일반화를 사용하거나, 중요한 부분을 강조하기 위해 불필요한 세부 사항을 제거함으로써 단순하게 만든다. 결국 핵심은 불필요한 코드를 제거하고 중요한 부분을 살리는 것이다.  

예를 들어, for, while, do while 등과 같은 문법은 반복 한다는 구문을 추상화한 것이다. 실제로는 CPU의 명령을 통해서 반복이 구현되겠지만, 이 구현으로부터 반복이라는 개념을 뽑아내서 for, while, do~while 등으로 추상화한 것이라고 할 수 있다.  

<br>
<br/>

### 코틀린 추상화 예제
추상화는 일반화를 통해서 구현될 수 있다. 일반화는 구체적인 사물들간의 공통점을 취하고 차이점을 버리는 것이다.

예를 들어, 아래와 같이 SuperCar, SnowCar, FastCar 3개의 클래스가 있다고 가정하자.

```kotlin
class SuperCar {
    fun move() {
        println("슈퍼카가 달립니다.")
    }
}

class SnowCar {
    fun move() {
        println("눈에서 달립니다.")
    }
}

class WaterCar {
    fun move() {
        println("물 위에서 달립니다.")
    }
}
```

이들을 움직이게 하기 위해서는 아래와 같이 코드를 작성하게 된다.

```kotlin
fun main() {
    val superCar = SuperCar()
    val snowCar = SnowCar()
    val waterCar = WaterCar()
    
    superCar.move()
    snowCar.move()
    waterCar.move()
}

// 슈퍼카가 달립니다.
// 눈에서 달립니다.
// 물 위에서 달립니다.
```

<br>
<br/>

여기서 움직인다는 행위는 같으므로 차의 공통된 속성인 'move' 를 상위 클래스로 뽑아낼 수 있다. 또한 똑같은 자동차들이므로 Car 라는 상위 클래스를 두어 상속 관계를 만들어도 무방하다.

```kotlin
// 추상 클래스
abstract class Car {
    abstract fun move()
}

class SuperCar : Car() {
    override fun move() {
        println("슈퍼카가 달립니다.")
    }
}

class SnowCar : Car() {
    override fun move() {
        println("눈에서 달립니다.")
    }
}

class WaterCar : Car() {
    override fun move() {
        println("물 위에서 달립니다.")
    }
}
```

<br>
<br/>

그리고 하위 클래스들의 추상 메서드를 재정의 하면 된다. 이들을 움직이기 위한 코드는 아래와 같이 작성할 수 있다.

```kotlin
fun main() {
    val cars = arrayListOf<Car>(SuperCar(), SnowCar(), WaterCar())
    for (car in cars) {
        car.move()
    }
}

// 슈퍼카가 달립니다.
// 눈에서 달립니다.
// 물 위에서 달립니다.
```
<br>
<br/>

결과적으로 우리는 일반화를 통해 추상화를 구현하자고 생각했고, 여기서 그 목적을 위한 방식은 바로 타입이다. 좀 더 상위 타입을 정의함으로써 추상화와 동시에 다형성을 구현할 수 있게 된 것이다.

<br>
<br/>

**추상화는**
- 공통된 개념을 도출해서 추상 타입을 정의해준다.
- 많은 책임을 가진 객체로 부터 책임을 분리하는 촉매재가 되기도 한다.

<br>
<br/>

## 인터페이스에 대고 프로그래밍
객체 지향의 유명한 규칙 : **인터페이스에 대고 프로그래밍하기(program to interface)**
- 여기서 말하는 인터페이스는 오퍼레이션을 정의한 인터페이스
- 코틀린이나 자바 같은 언어는 자체적으로 인터페이스나 추상클래스를 이용해서 개념적인 인터페이스를 제공하고 있다.
- **인터페이스에 대고 프로그래밍하기(program to interface)** 규칙은 추상화를 통한 유연함을 얻기 위한 규칙

**주의할 점**
- 주의할 점은 유연함을 얻는 과정에서 타입(추상 타입)이 무분별하게 증가하면 프로그램 복잡도를 높일 수 있다.
- 인터페이스를 사용해야 할 때는 변화 가능성이 높은 경우에 사용해야 한다.
- 변경 가능성이 희박한 클래스에 대해 인터페이스를 만든다면 오히려 프로그램의 구조만 복잡해지고 유연함의 효과는 누릴 수 없는 상황이
발생할 수 있다.

---

