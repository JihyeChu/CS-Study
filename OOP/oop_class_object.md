# 클래스(Class)?

클래스는 객체를 생성하기 위한 설계도 또는 템플릿이다.

클래스는 속성(필드)과 동작(메서드)로 이루어져 있으며, 실제 실행 시 JVM의 클래스(Class) 영역에 로드된다.

> Q: **클래스가 JVM 메모리의 클래스 영역에 로드된다는 것이 빈(Bean)으로 등록된다는 것과 같은 의미인가?**
> A: 클래스가 JVM의 클래스 영역에 로드된다는 것은 해당 클래스의 바이트코드가 메모리에 올라간다는 것을 의미한다.
> 반면, 빈(Bean)은 Spring 컨테이너가 관리하는 객체를 뜻한다. 클래스 자체는 빈이 아니고, `@Component`, `@Service`, `@RestController` 등의 어노테이션을 사용하면 Spring이 해당 클래스를 스캔해서 빈으로 등록된다.

```java
public class CategoryController {

    private final CategoryService categoryService; // 속성(필드)
    
    public ResponseEntity<?> search() { ... } // 동작(메서드)  
}
```

---

## 객체(Object)?

객체는 **클래스의 인스턴스(Instance)** 로 메모리에 할당되어 실제 동작하는 실체이다.

객체는 **상태(속성)와 동작(메서드)** 를 가지며, 다른 객체와 메시지를 주고받으며 동작한다.

```java
CategoryController controller = new CategoryController();
```

- CategoryController ⇒ 클래스 (설계도)
- controller ⇒ 참조 변수
- new CategoryController() ⇒ 객체

객체는 식별이 가능하다는 특징을 가지고 있다.
```java
CategoryController controller1 = new CategoryController();
CategoryController controller2 = new CategoryController();
CategoryController controller3 = new CategoryController();
```
동일한 클래스로부터 여러 객체를 만들더라도, 각각의 객체는 고유한 메모리 주소를 가지기 때문에 서로 다른 존재로 식별할 수 있다.

필드 없이 메서드만 있어도 객체

```java
public class Printer {
    public void printMessage() {
        System.out.println("Hello, World!");
    }
}

public class Main {
    public static void main(String[] args) {
        Printer printer = new Printer(); 
    }
}
// -----------------------------------------------

public class MathUtils {
    public static int add(int a, int b) {
        return a + b;
    }
}
```

메서드와 필드가 모두 있어도 객체

```java
public class UserDTO {
    private String name;
    private int age;

    public UserDTO(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public static void main(String[] args) {
        UserDTO user = new UserDTO("Alice", 25); 
    }
}
```
---

## 인스턴스(Instance)?

객체와 인스턴스는 사실상 같은 개념이다.

객체가 좀 더 포괄적인 개념이라면, 인스턴스는 실제 메모리에 존재하는 객체를 가리킨다.