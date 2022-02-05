# 🙋‍♂ Chap03_다형성과 추상 타입

## 📚 상속 개요

- 상속: 한 타입을 그대로 사용하면서 구현을 추가할 수 있도록 함
- 상속은 주로 기능을 확장하기 위해 사용
- override를 통해 기존의 있던 기능을 확장할 수 있음
- 코틀린에서는 아래와 같은 방식으로 상속을 구현할 수 있음

```
open class Test {
    open fun run() {
    println("테스트입니다")
}
    
class InheritanceTest: Test() {
    override fun run() {
    println("오버라이드했습니다"
    }
}
```

## 📚 다형성과 상속

- 다형성?
    - 한 객체가 여러 가지(poly) 모습(morph)을 갖는다는 의미, 즉 한 객체가 여러 타입을 가질 수 있다는 것
    - 객체지향의 사실과 오해(토끼책)
        - 동일한 요청(메시지)에 대해 서로 다른 방식으로 응답할 수 있는 능력

### 🔖 인터페이스 상속과 구현 상속

```
abstract class Test1  // 추상 클래스 (추상 메서드, 프로퍼티가 포함). open 키워드가 없어도 상속이 가능
interface Test2 // 인터페이스
open class Test3 // 상속이 가능한 클래스
interface Test4

class Inheritance1: Test1(), Test2, Test4
class Inheritance2: Test2(), Test2, Test4
```

- interface가 아니면 다중 상속은 불가
- abstract 메서드가 있으면 상속받은 객체에서 반드시 구현해야 한다
- 코틀린에서 class와 fun은 기본적으로 final 형태 (상속이 불가). 여기서 open 키워드를 붙이면 상속이 가능해진다
- 더 이상 상속을 원하지 않는다면 final 키워드를 쓰자 `final override fun test()`

## 📚 추상 타입과 유연함

- 추상화(abstraction)란?
    - 데이터나 프로세스 등을 의마가 비슷한 개념이나 표현으로 정의하는 과정
    - <토끼책>
        - 어떤 양상, 세부사항, 구조를 좀 더 명확하게 이해하기 위해 특정 절차나 물체를 의도적으로 생략하거나 감춤으로써 복잡도를 극복하는 방법
        - 복잡성을 다루기 위해 추상화는 두 차원에서 이뤄진다
            - 구체적인 사물들 간의 공통점은 취하고 차이점은 버리는 일반화를 통해 단순하게 만드는 것
                - <앨리스 이야기>
                    - 정원사, 병사, 신하, 왕자, 공주, 하트 여왕, 하트 왕 등 다양한 객체가 등장하지만 앨리스는 공통적인 외형을 가지고 이들을 모두 '트럼프'라고 복잡성을 극복하고 단순하게
                      생각함
            - 중요한 부분을 강조하기 위해 불필요한 세부사항을 제거함으로써 단순하게 만드는 것

- 추상 타입
    - 단순히 인터페이스가 아니라 각 객체의 기능/책임으로부터 동일한 개념으로 정의할 수 있는 것
    - <토끼책>
        - 개념
            - 일반적으로 우리가 인식하고 있는 다양한 사물이나 객체에 적용할 수 있는 아이디어나 관념
        - 객체
            - 객체란 특정한 개념을 적용할 수 있는 구체적인 사물
        - 추상화의 방법 중의 하나인 분류
            - 객체에 특정한 개념을 적용하는 작업
        - 타입
            - 타입은 개념이다
            - 공통점을 기반으로 객체들을 묶기 위한 틀
        - 객체의 타입
            - 어떤 객체가 어떤 타입에 속하는지를 결정하는 것은 객체가 수행하는 행동(책임)이다
            - 특정 객체가 다른 객체와 동일하게 분류되기 위한 기준은?
                - 해당 객체와 다른 객체가 동일한 `행동(책임)`을 할 때
                - 객체들이 각자 다른 데이터를 가지고 있더라도 동일한 행동을 하면 동일한 타입으로 분류할 수 있음
                - 반대로, 객체들이 같은 데이터를 가지고 있더라고 다른 행동을 하면 동일한 타입이 아니다
                    - 앞서 설명한 다형성에서 동일한 요청(메시지)에 대해 서로 다른 방식으로 응답할 수 있는 능력
                        - 동일한 요청에 대해서 응답하는 것 -> 동일한 행동/책임

```
class Dog {
    fun bark() {
        println("멍멍")
    }
}

class Cat {
    fun bark() {
        println("야옹")
    }
}
```

```
interface Pet {
    fun bark()
}

class Dog : Pet {
    override fun bark() {
        println("멍멍")
    }
}

class Cat : Pet {
    override fun bark() {
        println("야옹")
    }
}
```

### 🔖 추상 타입과 실제 구현의 연결

- 상속을 통해 추상 타입과 구현 클래스를 연결할 수 있음

```
val pet: Pet = Dog()
pet.bark() // 멍멍
pet= Cat()
pet.bark() // 야옹
```

- 위의 코드에서 각 하위 클래스에 따라 재정의된 bark() 메서드가 호출됨
- 이처럼 각 하위 타입들은 모두 상위 타입인 Pet 인터페이스에 정의된 기능을 구현하기 때문에, 콘크리트 클래스, 구현 클래스 또는 구상 클래스라 불린다

### 🔖 추상 타입을 이용한 구현 교체의 유연함
- 왜 콘크리트 클래스를 바로 사용하지 않고 추상 타입을 이용해야 할까?

```
interface Pet {
    var name: String
    fun bark()
}

class Dog : Pet {
    override var name: String = "멍멍이"
    override fun bark() {
        println("멍멍")
    }
}

class Cat : Pet {
    override var name: String = "야옹이"
    override fun bark() {
        println("야옹")
    }
}


fun shoutName(pet:Any) {
    when (pet) {
        is Dog -> println("${pet.name}입니다")
        is Cat -> println("${pet.name}입니다")
        else -> println("동물이 아닙니다")
    }
```

- 여기에 앵무새가 추가

```
class Parrot: Pet {
    override var name: String = "앵무새"
  override fun bark() {
  println("안녕")
  }
  
fun shoutName(pet:Any) {
    when (pet) {
        is Dog -> println("${pet.name}입니다")
        is Cat -> println("${pet.name}입니다")
        is Parrot -> println("${pet.name}입니다")
        else -> println("동물이 아닙니다")
    }
```
- 추상 타입을 쓴다면?
```
interface Pet {
    var name: String
    fun bark()
}

class Dog : Pet {
    override var name: String = "멍멍이"
    override fun bark() {
        println("멍멍")
    }
}

class Cat : Pet {
    override var name: String = "야옹이"
    override fun bark() {
        println("야옹")
    }
}

class Parrot: Pet {
    override var name: String = "앵무새"
    override fun bark() {
        println("안녕")
    }
}

fun shoutName(pet:Any) {
    val pet: Pet = specify(pet)
    println("${pet.name}입니다")
    }
```
- specify(pet)함수는 입력받은 매개변수를 Pet 타입으로 할당해주는 메서드
  - shoutName 함수 안에 이름을 외치는 기능과 매개변수를 Pet 객체 타입으로 만드는 기능이 있다면 이를 분리하여 책임을 분담해야 한다
- 만약 추가 요구 사항으로 뱀, 햄스터 등등 다른 펫이 추가되어도?
  - specify 메서드에서만 변화가 생김
  - shoutName 메서드는 아무런 변화없이 입력받은 pet의 이름을 말할 수 있다.
- 즉, 추상 타입을 사용하는 이유
  - 이로 인해 코드의 재사용성과 유연성이 증가!
  - 추상 타입을 사용함으로써 책임을 분리할 수 있는 촉매제 역할을 할 수 있다

### 🔖 변화되는 부분을 추상화하기
- 유연한 설계를 할 때, 처음부터 변화될 부분을 추상화해서 설계하기는 쉽지 않다
- 만약 설계/구현할 때, 요구 사항이 추가되면 변화될 부분이 보인다면 해당 부분은 추상화를 통해(=추상 타입을 이용하여) 보다 유연한 설계/구현을 할 수 있다

### 🔖 인터페이스에 대고 프로그래밍하기
- 여기서 말하는 인터페이스는 자바/코틀린에서 제공하는 인터페이스가 아니라 추상 타입을 말함
- 인터페이스에 대고 프로그래밍하기는, 간단히 설명하여 앞서 예시로 든 코드처럼 콘크리트 클래스가 아니라 추상 타입을 이용하는 것
- 단, 추상 타입을 너무 자주 사용하면 코드가 너무 복잡해짐
- 따라서, 변화가 자주 발생할 것으로 예상되는 상황에서만 추상 타입(인터페이스)를 활용하자
  - 그럼 프로그래밍이 더 복잡해지지만 더 유연해지기 때문에 더 좋다

### 🔖 인터페이스는 인터페이스 사용자 입장에서 만들기
- 인터페이스를 작성할 때에는 그 인터페이스를 사용하는 코드 입장에서 작성하자
### 🔖 인터페이스와 테스트
- 인터페이스를 이용한다면 콘크리트 클래스가 완성되지 않은 상황에서도 테스트를 진행할 수 있다
- 실제 프로그래밍을 할 때는 테스트를 하기 위해 인터페이스를 직접 생성하기 보다는 Mockito나 jmock과 같은 프레임워크를 활용 (코틀린은 mockk 활용)
  - 위에서 말한 프레임워크들은 짜여진 인터페이스를 바탕으로 Mock 객체를 생성해준다

### 🔖 추가적으로 추상 타입으로부터 상속된 여러 객체를 같이 다룰 때 편리해진다
```
 val dog: Dog = Dog()
 val cat: Cat = Cat()
 val parrot: Parrot = Parrot()
 val petArray = arrayOf<Any> (dog,cat,parrot)
```

```
 val dog: Pet = Dog()
 val cat: Pet = Cat()
 val parrot: Pet = Parrot()
 val petArray = arrayOf<Pet> (dog,cat,parrot)
```

- 콜렉션을 이용할 때 추상 타입을 이용하면, 추상 타입으로부터 상속된 다양한 객체를 손쉽게 다룰 수 있다