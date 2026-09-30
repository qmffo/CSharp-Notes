# 리플렉션 (Reflection)

리플렉션(Reflection)은 **실행 중에 프로그램의 타입 정보를 조회하고 조작할 수 있는 기능**이다.

클래스, 메서드, 필드, 프로퍼티 등의 정보를 **런타임에 확인**할 수 있다.

---

## 1. 기본 개념

일반적으로 코드는 컴파일할 때 타입과 멤버를 결정한다.

리플렉션을 사용하면 프로그램 실행 중에 타입 정보를 확인할 수 있다.

```text
일반적인 코드
컴파일 → 타입 정보 결정 → 실행

리플렉션
컴파일 → 실행 → 타입 정보 조회/조작
```

---

## 2. Type

리플렉션의 기본은 `Type` 객체를 사용하는 것이다.

```csharp
Type type = typeof(Player);

Console.WriteLine(type.Name);
```

```text
Player
```

현재 객체의 타입을 가져올 수도 있다.

```csharp
Player player = new Player();

Type type = player.GetType();
```

### 주요 방법

```csharp
typeof(Player);  // 특정 타입의 Type 가져오기

player.GetType(); // 객체의 실제 타입 가져오기
```

---

## 3. 주요 정보 조회

클래스의 다양한 정보를 확인할 수 있다.

```csharp
Type type = typeof(Player);
```

```csharp
type.Name;          // 클래스 이름
type.FullName;      // 전체 타입 이름

type.GetMethods();  // 메서드
type.GetFields();   // 필드
type.GetProperties(); // 프로퍼티
```

```text
Type
 ├─ Methods
 ├─ Fields
 └─ Properties
```

---

## 4. 메서드 실행

리플렉션을 이용하면 메서드를 이름으로 찾아 실행할 수도 있다.

```csharp
MethodInfo method = typeof(Player).GetMethod("Attack");

method.Invoke(player, null);
```

`Invoke()`를 사용하여 해당 메서드를 실행할 수 있다.

---

## 5. 주요 용도

리플렉션은 다음과 같은 곳에서 활용된다.

* 런타임 타입 정보 확인
* 메서드 / 필드 / 프로퍼티 탐색
* 동적으로 메서드 실행
* 직렬화(Serialization)
* 의존성 주입(DI)
* ORM
* 테스트 프레임워크
* Unity의 일부 기능 및 에디터 도구

---

## 6. 장점과 단점

| 장점               | 단점                   |
| ---------------- | -------------------- |
| 런타임에 타입 정보 확인 가능 | 일반적인 코드보다 느릴 수 있음    |
| 동적인 코드 작성 가능     | 코드가 복잡해질 수 있음        |
| 다양한 자동화 기능 구현 가능 | 컴파일 타임의 타입 검사를 일부 우회 |
| 프레임워크 제작에 유용     | 과도한 사용 시 유지보수 어려움    |

---

## 7. 핵심 요약

> **리플렉션은 실행 중에 타입과 그 멤버의 정보를 조회하고 동적으로 사용할 수 있게 해주는 기능이다.**

```csharp
Type type = typeof(Player);

Console.WriteLine(type.Name);

foreach (var method in type.GetMethods())
{
    Console.WriteLine(method.Name);
}
```

### 기억할 것

```text
Reflection
    ↓
Type 정보 조회
    ↓
Method / Field / Property 탐색
    ↓
필요하면 동적으로 실행
```

**리플렉션 = 런타임에 타입 정보를 확인하고 동적으로 조작하는 기능**

[리플렉션 원리](Reflection_Principle)