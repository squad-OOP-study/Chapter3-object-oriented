# 🙋‍♂ Chap03_다형성과 추상 타입
## 📚 상속 개요
- 상속: 한 타입을 그대로 사용하면서 구현을 추가할 수 있도록 함

## 📚 다형성과 상속
- 다형성?
    - 한 객체가 여러 가지(poly) 모습(morph)을 갖는다는 의미, 즉 한 객체가 여러 타입을 가질 수 있다는 것
    - 객체지향의 사실과 오해(토끼책)
        - 동일한 요청(메시지)에 대해 서로 다른 방식으로 응답할 수 있는 능력

### 🔖 인터페이스 상속과 구현 상속
```
abstract class Test1  // 추상 클래스 (추상 메서드, 프로퍼티가 포함)
interface Test2 // 인터페이스
open class Test3 // 상속이 가능한 클래스
interface Test4

class Inheritance1: Test1(), Test2, Test4
class Inheritance2: Test2(), Test2, Test4
```
- interface가 아니면 다중 상속은 불가

## 📚 추상 타입과 유연함

### 🔖 추상 타입과 실제 구현의 연결

### 🔖 추상 타입을 이용한 구현 교체의 유연함

### 🔖 변화되는 부분을 추상화하기