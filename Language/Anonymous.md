# 익명 메서드 (Anonymous Method)

이름이 없는 메서드를 변수나 델리게이트에 직접 할당하여 사용하는 방법이다.

주로 **델리게이트(Delegate)** 또는 **이벤트(Event)**에 일회성으로 실행할 코드를 등록할 때 사용한다.

---

## 1. 기본 문법

```csharp
delegate 반환형 델리게이트이름(매개변수);

델리게이트이름 변수 = delegate(매개변수)
{
    // 실행할 코드
};
```

### 예제

```csharp
delegate void MyDelegate(string message);

MyDelegate del = delegate(string message)
{
    Console.WriteLine(message);
};

del("Hello");
```

```text
출력:
Hello
```

익명 메서드는 별도의 메서드 이름을 지정하지 않고 `delegate` 키워드를 사용하여 작성한다.

---

## 2. 일반 메서드와 비교

### 일반 메서드

```csharp
void PrintMessage(string message)
{
    Console.WriteLine(message);
}

MyDelegate del = PrintMessage;
```

### 익명 메서드

```csharp
MyDelegate del = delegate(string message)
{
    Console.WriteLine(message);
};
```

간단한 로직을 한 번만 사용할 경우 익명 메서드를 이용하면 별도의 메서드를 만들 필요가 없다.

---

## 3. 매개변수 생략

델리게이트의 매개변수를 사용하지 않는다면 매개변수를 생략할 수 있다.

```csharp
delegate void MyDelegate();

MyDelegate del = delegate
{
    Console.WriteLine("Hello");
};

del();
```

`delegate` 뒤의 `()`를 생략할 수 있다.

```csharp
delegate
{
    // 실행할 코드
}
```

---

## 4. 지역 변수 사용

익명 메서드는 자신이 선언된 외부 범위의 변수를 사용할 수 있다.

```csharp
int number = 10;

MyDelegate del = delegate
{
    Console.WriteLine(number);
};

del();
```

```text
출력:
10
```

이처럼 익명 메서드 외부에 있는 지역 변수에 접근할 수 있다.

이러한 특징은 **클로저(Closure)**와 관련이 있다.

---

## 5. 델리게이트와 함께 사용

익명 메서드는 델리게이트에 직접 할당하여 사용할 수 있다.

```csharp
Action action = delegate
{
    Console.WriteLine("실행");
};

action();
```

매개변수가 있는 경우:

```csharp
Action<int> action = delegate(int number)
{
    Console.WriteLine(number);
};

action(10);
```

반환값이 있는 경우:

```csharp
Func<int, int> func = delegate(int number)
{
    return number * 2;
};

int result = func(10);

Console.WriteLine(result);
```

```text
출력:
20
```

---

## 6. 익명 메서드와 람다식

익명 메서드는 **람다식(Lambda Expression)**으로 더 간결하게 작성할 수 있다.

### 익명 메서드

```csharp
Func<int, int> func = delegate(int number)
{
    return number * 2;
};
```

### 람다식

```csharp
Func<int, int> func = number =>
{
    return number * 2;
};
```

더 간단하게:

```csharp
Func<int, int> func = number => number * 2;
```

따라서 현대 C#에서는 간단한 익명 함수를 작성할 때 람다식을 사용하는 경우가 많다.

---

## 7. 주요 특징

| 특징       | 설명                         |
| -------- | -------------------------- |
| 이름       | 별도의 메서드 이름이 없음             |
| 키워드      | `delegate` 사용              |
| 주 사용처    | 델리게이트, 이벤트                 |
| 외부 변수 접근 | 가능                         |
| 람다식      | 익명 메서드를 더 간결하게 표현 가능       |
| 장점       | 간단한 코드를 별도의 메서드로 만들 필요가 없음 |
| 단점       | 복잡한 로직에서는 가독성이 떨어질 수 있음    |

---

## 8. 익명 메서드 → 람다식

```csharp
// 익명 메서드
Action<string> action = delegate(string message)
{
    Console.WriteLine(message);
};
```

```csharp
// 람다식
Action<string> action = message =>
{
    Console.WriteLine(message);
};
```

```csharp
// 람다식 축약
Action<string> action = message => Console.WriteLine(message);
```

### 핵심

> **익명 메서드는 이름이 없는 메서드를 `delegate` 키워드를 이용해 직접 작성하는 방법이다.**

```csharp
Action action = delegate
{
    Console.WriteLine("Hello");
};
```

현재 C#에서는 같은 목적이라면 **람다식을 사용하는 경우가 일반적**이다.
