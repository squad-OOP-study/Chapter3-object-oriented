# Chapter3-object-oriented

# 상속

## 상속 이란?

`상속(Inheritance)` 은 한 타입을 그대로 사용하면서 구현을 추가할 수 있도록 해주는 방법을 제공한다. 상속 받은 하위 클래스는 필요에 따라 상위 클래스에 정의된 메서드를 새롭게 구현할 수 있다. 이를 재정의(overriding)라 한다. 메서드를 재정의하면, 해당 메서드를 실행할 때 상위 타입의 메서드가 아닌 하위 타입에서 재정의한 메서드가 실행된다.

## 상속의 종류

1.  인터페이스 상속
    - 다중 상속을 지원하지 않는 언어에서 다형성을 구현하는 방법
    - 순수하게 타입만을 상속받는 상속
    - 인터페이스에서 정의만한 오퍼레이션을 직접 구현하는 방식으로 상속
    - 추상화를 구현하기 위한 핵심
2.  구현 상속(클래스 상속)
    - 상위 클래스에 정의된 기능을 재사용하기 위한 목적 ( 재사용성이 핵심 목적)
    - 재사용성 + 다형성의 기능을 제공

## **추상클래스 vs 인터페이스**

1. 추상클래스
    - abstract 메소드를 가지고 있는 클래스
    - abstract 사용 시 클래스나 메소드에 open 키워드를 따로 명시하지 안아도 된다
    - 자식 클래스는 추상 메소드를 반드시 구현해야 한다
    - 생성자를 가질 수 있다
    - 다중 상속 불가
2. 인터페이스
    - 일반적으로 모든 메소드가 abstract인 클래스를 의미하지만, java 8 부터는 default 키워드를 통해 메소드 구현이 가능!
    - 생성자x
    - 다중 상속 가능
    
    ```kotlin
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
    


# 다형성

## **다형성 개념**

`다형성(Polymorphism)` 은 한 객체가 `여러 가지(poly)` `모습(morph)` 을 갖는다는 것을 의미한다. 모습은 타입을 의미하며, 다형성이란 한 객체가 여러 타입을 가질 수 있는 것이다.

즉, 다형성이란

- 한 객체가 여러 타입을 가질 수 있다는 것을 의미
- 추상화
    - 객체 내부 구현 변경할 수 있는 유연함 제공하는 또 다른 방법
    - **데이터나 프로세스 등을 의미가 비슷한 개념이나 표현으로 정의하는 과정**
    - 타입도 추상화의 대상이 된다.
    - 각 구현 클래스를 추상화해서 인터페이스를 도출(공통된 개념을 도출해서 추상 타입을 정의)
    - 그러나 추상화를 한다는 것이 반드시 추상 타입을 만들어야 하는 것은 아니다.
        - 예) sum += mark;
        - 코드는 CPU 일련의 처리 과정을 개념적으로 추상화한 것이다.
        - for, while, do while 등과 같은 문법은 반복 한다는 구문을 추상화한 것이다.
    1. 많은 책임을 가진 객체로부터 책임을 분리하는 촉매제
    - 변화가 발생하는 곳부터 시작.

## <토끼책>

- 어떤 양상, 세부사항, 구조를 좀 더 명확하게 이해하기 위해 특정 절차나 물체를 의도적으로 생략하거나 감춤으로써 복잡도를 극복하는 방법
- 복잡성을 다루기 위해 추상화는 두 차원에서 이뤄진다
    - 구체적인 사물들 간의 공통점은 취하고 차이점은 버리는 일반화를 통해 단순하게 만드는 것
        - <앨리스 이야기>
            - 정원사, 병사, 신하, 왕자, 공주, 하트 여왕, 하트 왕 등 다양한 객체가 등장하지만 앨리스는 공통적인 외형을 가지고 이들을 모두 '트럼프'라고 복잡성을 극복하고 단순하게 생각함
    - 중요한 부분을 강조하기 위해 불필요한 세부사항을 제거함으로써 단순하게 만드는 것


# 다형성 구현 및 인터페이스 사용


## **인터페이스 도출**

다형성을 구현하기 위해 타입을 상속받는데, 타입 상속은 `인터페이스 상속` 과 `구현 상속` 이 존재한다.

1. 인터페이스 상속은 순전히 타입 정의만 상속받는 것. 클래스 구현 상속은 클래스 상속을 통해서 이루어진다. 클래스 상속은 구현을 재사용하면서 다형성도 함께 제공해준다.
2. 클래스는 다중 상속 지원을 하지 않아서 인터페이스를 이용해서 객체가 다형성을 갖는다.


## **인터페이스에 대고 프로그래밍**

객체 지향의 유명한 규칙 : **인터페이스에 대고 프로그래밍하기(program to interface)**

- 여기서 말하는 인터페이스는 오퍼레이션을 정의한 인터페이스
- 코틀린이나 자바 같은 언어는 자체적으로 인터페이스나 추상클래스를 이용해서 개념적인 인터페이스를 제공하고 있다.
- **인터페이스에 대고 프로그래밍하기(program to interface)** 규칙은 추상화를 통한 유연함을 얻기 위한 규칙


## **추상화 예시**

1. ByteSource 를 상속 하여 사용
- **data를 얻어낼 수 있다는 공통점으로부터 ByteSource 라는 인터페이스 추출**
- **data를 얻어낼 수 있는 객체들을 만들어낸 다는 공통점으로 ByteSourceFactory 라는 인터페이스 추출**

```kotlin
sourceType 에 따라서 분기하지 않고 팩토리 패턴안에서 type 에 맞는 객체를 받아낸다.
캡슐활 실현

val source : ByteSource = ByteSourceFactory.create(sourceType)

sourceType 과 무관하게 ByteSource 인터페이스 메서도 read를 사용하여 데이터를 얻는다.
추상화 및 다형성 활용
val data = source.read()
```

1. LogCollector Interface를 통한 추상화

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

```kotlin
//결과
FtpLogFileDownloader
SocketLogReader
DbTableLogGateway
```

1. abstract class를 통한 추상화

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

fun main() {
    val cars = arrayListOf<Car>(SuperCar(), SnowCar(), WaterCar())
    for (car in cars) {
        car.move()
    }
}

// 슈퍼카가 달립니다.
// 눈에서 달립니다.
// 물 위에서 달립니다.

// 일반화를 통해 추상화를 구현하자고 생각했고, 여기서 그 목적을 위한 방식은 바로 타입이다. 
// 좀 더 상위 타입을 정의함으로써 추상화와 동시에 다형성을 구현할 수 있게 되었다.
```


## 추상 타입의 장점

- 구현 교체의 유연함
    - 추가 요구 사항 발생시, 변화 되는 부분을 최소화 할 수 있다
    - 반복적으로 발생되는 if/else 부분 제거 가능
    - 상속 & 다형성으로 얻을 수 있는 장점을 깨지않고, 구조 확장 가능
- 공통된 개념을 도출해 중복성 제거
- 많은 책임을 가진 객체로 부터 책임을 분리해, 각 객체가 가지는 책임을 최소화하는 SRP 원칙 준수 가능
- Test 과정에서 드러나느 추상화의 장점
    - 일반적으로 객체지향적으로 프로그램을 작성한다 할 시 팀별 or 개발자별 클래스를 나누어 개발한다
    - 전체적인 기능을 테스트하고 싶을 때, 다른 클래스 개발이 아직 구현이 불안정하다면 일반적인 방법으로 테스트가 불가능함
    - but 추상타입에 의해 구조가 설계되있을 경우 별도의 Mock 객체를 생성함으로 테스트가 가능해진다
    - 별도의 Mock 객체를 직접 생성하기 보다는 Mockito나 jmock 같은 프레임워크를 이용해 생성하는 것이 일반적이다.


## **추상타입 구현에서 발생할 수 있는 문제점**

- 변경이 일어날 가능성이 적은 부분에 대해 인터페이스를 만들면, 구조의 복잡성만 올라가고, 유연성의 장점을 누릴 수 없는 상황이 된다
- 변경이 일어나는 부분을 정확히 캐치하여 인터페이스를 만들지 않으면, 상속 & 다형성으로 얻을 수 있는 장점이 깨질 수 있다.


## 변화되는 부분을 추상화 하기

- **추상화를 잘 하려면 다양한 상황에서 코드를 작성하고, 이 과정에서 유연한 사례를 만들어 보는 경험을 해봐야 한다.**
- 그러나 모든 개발자가 다양한 환경에서 많은 경험을 할 수 있는 것은 아니기 때문에, **변화될 부분을 미리 예측해서 추상화**하는 것은 쉽지 않다.
- **경험하지 않는 분야를 추상화하는 법**
    - 변화되는 부분을 추상화하여야 한다.
    - 요구사항이 바뀔 때 변화되는 부분은 이후에도 변경될 소지가 많다.
    - 이런 부분을 추상 타입으로 교체하면 향후 변경에 대처할 가능성 높아진다.
- 추상화가 되어 있지 않은 코드는 주로 동일 구조를 갖는 if-else블록으로 드러난다.
    
    ```kotlin
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
    /* 팩토리 클래스가 필요함 */
    ```
    
    위와 같은 코드를 factory 클래스를 통해서 구현하고, 싱글턴 패턴을 사용하는것이 좋다.
    
- 또한, 콘크리트 클래스를 직접 사용하지 않는 이유
    
    ```kotlin
    val reader = SocketLogReader()
    reader.collect()
    ```
    
    직접 사용해도 문제가 발생하지 않는다. 요구사항으로 소켓을 통해서가 아닌 DB에서 읽어오거나 파일을 통해서 읽어오라는 요구사항이 존재한다면,
    
    `val reader = SocketLogReader()val reader = FtpLogFileDownloader()` `val reader = DbTableLogGateway()` 을 직접 만들어야 한다. 그로 인해 많은 분기가 생기게 되고 복잡한 소스코드가 만들어지게 된다.
    

## 인터페이스 사용자 입장에서 인터페이스를 만들어야 한다

- 인터페이스를 사용하는 코드 입장에서 작성.
- 의미를 명확하게 드러내도록 네이밍.
