# 캡슐화란 ?

**캡슐화**란, 데이터와 해당 데이터를 처리하는 메서드를 하나로 묶고, **외부에서의 직접적인 접근을 제한하는 개념**이다.  
이를 통해 **데이터의 변경을 안전하게 관리하고, 무분별한 접근을 차단하여 안정성을 높일 수 있다.**

쉽게 말해, **객체의 속성과 기능을 하나로 묶고, 외부에는 꼭 필요한 기능만 노출하며 나머지는 내부로 감추는 것**이다.

---

## 접근 제어자

캡슐화를 설명하려면 먼저 접근 제어자에 대한 이해가 선행되어야 한다.

자바는 총 4 가지의 접근 제어자를 제공하며, `private` `default` `protected` `public` 등이 그것이다.

접근 제어자의 핵심은 **속성과 기능을 외부로부터 숨기는 것**이다.

```java
private 은 나의 클래스 안으로 속성과 기능을 숨길 때 사용, 외부 클래스에서 해당 기능을 호출할 수 없다.
default 는 나의 패키지 안으로 속성과 기능을 숨길 때 사용, 외부 패키지에서 해당 기능을 호출할 수 없다.
protected 는 상속 관계로 속성과 기능을 숨길 때 사용, 상속 관계가 아닌 곳에서 해당 기능을 호출할 수 없다.
public 은 기능을 숨기지 않고 어디서든 호출할 수 있게 공개한다.
```

---

## 캡슐화를 적용하는 이유

캡슐화에는 크게 두 가지 측면이 있다.
> 1. 외부로부터 필드와 메서드에 대한 불필요한 접근을 막는다.
> 2. 외부로부터 클래스의 구체적인 구현 내용을 감춘다.

이러한 캡슐화의 개념을 적용시키지 않았을 때 무슨 문제가 발생하는지 예시를 통해 알아보도록 하자

---

### A 씨와 B 씨의 이야기

A 씨는 간단한 `BankAccount` 클래스를 만들었다. 계좌의 잔액을 나타내는 `balance` 변수를 `public`으로 선언하여 누구나 직접 접근할 수 있도록 했다.
A 씨는 `BankAccount` 클래스를 작성한 후, 동료 B 씨에게 사용하도록 했다. 하지만 B 씨는 실수로 잔액을 직접 변경하는 코드를 작성했다.

```java
class BankAccount {
    public int balance; // 외부에서 직접 접근 가능

    public BankAccount(int balance) {
        this.balance = balance;
    }
}

public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount(1000);
        
        // B 씨가 실수로 잔액을 마음대로 변경함
        account.balance = -500; // 말도 안 되는 음수 잔액 발생!
        
        System.out.println("잔액: " + account.balance); // 잔액: -500 (비정상적인 값)
    }
}
```
위 코드를 살펴보면 B 씨가 실수로 `balance` 값을 음수(-5000)로 설정해버렸다.
정상적인 은행 시스템에서는 잔액이 음수가 되면 안 됨에도 불구하고, A 씨는 이를 막지 못했다.

해당 사례를 통해 캡슐화를 신경 쓰지 않는다면 다음과 같은 문제점이 발생한다.

>1. 필드와 메서드의 불필요한 접근은 예상치 못한 오류를 야기할 수 있다.
    >	-> ex ) `balance` 에 -500 과 같은 비정상적인 값이 설정될 수 있음.
>2. 필드가 여러 곳에서 수정될 수 있기 때문에 유지보수성이 저하된다.
    >	-> ex ) 다른 개발자가 balance를 잘못 수정하는 경우, 어디에서 문제가 발생했는지 추적하기 어려움
>3. 내부 구현이 그대로 노출되기 때문에, 클래스 내부 로직을 보호할 방법이 없다.
    >	-> ex ) `balance` 를 외부에서 직접 변경하면, 은행 시스템에서 잔액 변경 로그를 기록하는 기능을 우회할 수도 있음
>4. 객체의 일관성이 깨진다.
    >	-> ex ) 특정 로직에서는 `balance` 를 음수로 설정할 수 없도록 구현했지만, 외부에서 `balance = -5000;` 처럼 직접 변경하면 무용지물이 됨

---

A 씨는 실수를 깨닫고, `balance` 변수를 `private` 으로 감추고 잔액을 안전하게 변경할 수 있는 메서드를 추가했다.

```java
class BankAccount {
    private int balance; // 외부에서 직접 접근하지 못하도록 변경

    public BankAccount(int balance) {
        if (balance < 0) {
            throw new IllegalArgumentException("초기 잔액은 0 이상이어야 합니다.");
        }
        this.balance = balance;
    }

    // 현재 잔액을 조회하는 메서드
    public int getBalance() {
        return balance;
    }

    // 잔액을 변경할 때 유효성 검사 추가
    public void deposit(int amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("입금 금액은 0보다 커야 합니다.");
        }
        balance += amount;
    }

    public void withdraw(int amount) {
        if (amount > balance) {
            throw new IllegalArgumentException("잔액이 부족합니다.");
        }
        balance -= amount;
    }
}
```


```java
public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount(1000);

        account.deposit(500); // ✅ 정상 입금
        System.out.println("현재 잔액: " + account.getBalance()); // 1500 출력

        account.withdraw(2000); // ❌ 예외 발생 (잔액 부족)
    }
}
```

이제 `BankAccount` 클래스에 캡슐화를 적용하여 `balance` 값을 외부에서 임의로 변경하는 것을 막을 수 있게 되었다.
B 씨는 더 이상 `balance` 변수를 직접 조작할 수 없으며, `deposit()` 과 `withdraw()` 메서드를 통해서만 잔액을 변경할 수 있다.
또한, 이 두 메서드에서 유효성 검사를 수행하여 잘못된 값의 입력을 방지함으로써 데이터의 무결성을 유지할 수 있게 되었다.

즉, **잔액을 어떻게 저장하고 변경하는지는 숨기고, 외부에서는 정해진 메서드(deposit(), withdraw())만 사용하게 강제**하는 구조가 된 것이다 !

---

## 결론

캡슐화를 적용하지 않으면 **데이터 보호가 어렵고**, **시스템의 안정성이 떨어지며, 유지보수가 복잡해지는 문제가 발생한다.**
따라서, 중요한 데이터는 반드시 `private` 으로 보호하고, 필요한 경우 `getter/setter` 또는 특정 메서드를 통해 안전하게 접근하도록 하자 !
