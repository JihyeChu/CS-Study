## 다형성이란?

하나의 인터페이스로 여러 가지 형태를 가질 수 있는 성질을 말한다.

즉, 동일한 메서드나 클래스가 서로 다른 방식으로 동작할 수 있도록 한다.

오버로딩, 오버라이딩, 업캐스팅, 다운캐스팅, 인터페이스, 추상메서드, 추상 클래스 방법이 모두 다형성에 속해 있다.

### 오버로딩(Overloading)

- overload는 과적, 과부화라는 뜻을 가지고 있다. 여기서 과적은 하나의 메소드 이름으로 여러 기능을 구현할 수 있기에 붙여준 것이 아닐까 싶다.
- 한 클래스 내부에서 메서드를 확장하기 위한 개념이다.

  → 같은 이름을 가진 여러 메서드

- 메서드 이름이 일치하고, 매개변수의 개수 또는 리턴 값이 달라야 한다.

### 오버라이딩(Overriding)

- override는 재정의하다, 덧씌우다 라는 뜻을 가지고있다. 부모 클래스의 메서드를 자식 클래스에서 재정의 하여 다른 동작을 수행할 수 있도록 한다.
- 메서드 이름, 매개변수, 리턴 값이 모두 같아야 한다.

### **인터페이스(Interface)**

- 인터페이스는 특정 기능을 정의한 메서드 집합이다. 구현은 상속받은 클래스에서 정의한다.

```java
// 오버로딩
class Calculator {
    public int add(int a, int b) {
        return a + b;
    }

    public double add(double a, double b) {
        return a + b;
    }
}

// 오버라이딩
class Animal {
    public void sound() {
        System.out.println("Some sound");
    }
}

class Dog extends Animal {
    @Override
    public void sound() {
        System.out.println("Bark");
    }
}

// 인터페이스
interface Animal {
    void sound();
}

class Dog implements Animal {
    public void sound() {
        System.out.println("Bark");
    }
}

class Cat implements Animal {
    public void sound() {
        System.out.println("Meow");
    }
}
```

### 업캐스팅(Upcasting)
- 자식 클래스 객체를 부모 클래스 타입으로 변환하는 것이다.

### 다운캐스팅(Downcasting)
- 부모 클래스 타입의 객체를 자식 클래스 타입으로 변환하는 것이다.
```java
class Animal {
    public void sound() {
        System.out.println("Some sound");
    }
}

class Dog extends Animal {
    @Override
    public void sound() {
        System.out.println("Bark");
    }

    public void fetch() {
        System.out.println("Fetching...");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal = new Dog(); // 업캐스팅
        animal.sound(); 

        // 다운캐스팅
        Dog dog = (Dog) animal;  // 다운캐스팅
        dog.fetch();  
    }
}
```
### 추상 클래스(Abstract Class)
- 추상 클래스는 인스턴스를 생성할 수 없고, 자식 클래스에서 구현해야 할 메서드를 선언할 수 있는 클래스이다.
- 추상 클래스를 사용하면 공통된 기능을 제공하면서도 자식 클래스에서 구체적인 동작을 정의할 수 있다.

### 추상 메서드(Abstract Method)
- 추상 메서드는 구현이 없는 메서드로, 추상 클래스에 선언된다.
- 이 메서드는 자식 클래스에서 반드시 구현해야 하고, 자식 클래스는 추상 메서드를 오버라이드하여 구체적인 동작을 정의해야 한다.
```java
abstract class Animal {  // 추상 클래스
    public abstract void sound();  // 추상 메서드
}

class Dog extends Animal {
    @Override
    public void sound() {
        System.out.println("Bark");
    }
}
```