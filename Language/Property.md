# 프로퍼티 (Property)

프로퍼티는 클래스의 필드에 **접근할 수 있는 통로를 제공하는 멤버**이다.

필드와 달리 `get`, `set`을 사용하여 **값을 읽거나 저장하는 동작을 제어**할 수 있다.

---

## 1. 기본 문법

```csharp
public int Age
{
    get { return age; }
    set { age = value; }
}
```

```text
get → 값을 가져옴
set → 값을 저장함
value → set을 통해 전달된 값
```

### 사용

```csharp
person.Age = 20;

Console.WriteLine(person.Age);
```

---

## 2. 필드와 프로퍼티 비교

### 필드

```csharp
private int age;
```

### 프로퍼티

```csharp
public int Age
{
    get { return age; }
    set { age = value; }
}
```

외부에서 직접 필드에 접근하는 대신 프로퍼티를 통해 값을 제어할 수 있다.

```text
외부 코드
   ↓
Property
   ↓
Field
```

---

## 3. 자동 구현 프로퍼티

별도의 필드가 필요하지 않다면 **자동 구현 프로퍼티**를 사용할 수 있다.

```csharp
public string Name { get; set; }
```

컴파일러가 내부적으로 필요한 저장 공간을 자동으로 생성한다.

```csharp
person.Name = "Player";

Console.WriteLine(person.Name);
```

---

## 4. 읽기 전용 프로퍼티

`get`만 작성하면 외부에서 값을 변경할 수 없다.

```csharp
public int Age { get; }
```

생성자에서 값을 초기화할 수 있다.

```csharp
public Person(int age)
{
    Age = age;
}
```

---

## 5. private set

외부에서는 읽을 수 있지만 클래스 내부에서만 값을 변경하도록 할 수 있다.

```csharp
public int Health { get; private set; }
```

```csharp
Health = 100;       // 클래스 내부 → 가능
```

```csharp
player.Health = 50; // 외부 → 불가능
```

---

## 6. 프로퍼티에서 값 검증

`set` 내부에서 조건을 검사할 수 있다.

```csharp
private int age;

public int Age
{
    get { return age; }

    set
    {
        if (value >= 0)
            age = value;
    }
}
```

프로퍼티를 이용하면 값이 변경되는 시점에 **유효성 검사 등의 로직**을 추가할 수 있다.

---

## 7. 표현식 본문 프로퍼티

간단한 `get`은 `=>`를 사용하여 작성할 수 있다.

```csharp
public int Age => 20;
```

다른 프로퍼티를 이용할 수도 있다.

```csharp
public int BirthYear { get; set; }

public int Age => DateTime.Now.Year - BirthYear;
```

---

## 8. 초기화 전용 프로퍼티

`init`을 사용하면 **객체 초기화 시점에만 값을 설정**할 수 있다.

```csharp
public string Name { get; init; }
```

```csharp
Person person = new Person
{
    Name = "Player"
};
```

초기화 이후에는 값을 변경할 수 없다.

---

## 9. 프로퍼티의 주요 형태

```csharp
// 읽기 + 쓰기
public int Age { get; set; }

// 읽기 전용
public int Age { get; }

// 외부 읽기 + 내부 쓰기
public int Age { get; private set; }

// 초기화 시 설정
public int Age { get; init; }

// 계산된 값
public int DoubleAge => Age * 2;
```

---

## 10. 프로퍼티와 캡슐화

프로퍼티는 클래스 내부의 데이터를 직접 노출하지 않고 **접근 방법을 제어**하는 데 사용할 수 있다.

```csharp
private int health;

public int Health
{
    get => health;

    private set
    {
        health = Math.Max(0, value);
    }
}
```

외부에서는 `Health`를 읽을 수 있지만 직접 값을 변경할 수 없다.

```text
외부
 ↓
Property
 ↓
필드
```

이를 통해 **캡슐화(Encapsulation)**를 구현할 수 있다.

---

## 11. 주요 특징

| 형태                      | 설명             |
| ----------------------- | -------------- |
| `get`                   | 값 읽기           |
| `set`                   | 값 쓰기           |
| `value`                 | `set`으로 전달된 값  |
| `{ get; set; }`         | 읽기 + 쓰기        |
| `{ get; }`              | 읽기 전용          |
| `{ get; private set; }` | 외부 읽기 + 내부 쓰기  |
| `{ get; init; }`        | 객체 초기화 시에만 설정  |
| `=>`                    | 계산된 프로퍼티 작성 가능 |

---

## 12. 핵심 요약

> **프로퍼티는 필드에 대한 접근을 제어하고 데이터를 안전하게 관리하기 위한 멤버이다.**

```csharp
public int Health { get; private set; }
```

```text
get    → 값 읽기
set    → 값 변경
private set → 클래스 내부에서만 변경
init   → 초기화할 때만 변경
```

### 기억할 것

```text
필드
 ↓
프로퍼티
 ↓
접근 제어 / 값 검증
 ↓
캡슐화
```

**프로퍼티 = 데이터에 대한 접근을 제어하는 C#의 문법**
