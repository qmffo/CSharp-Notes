# 클로저 (Closure)

**클로저(Closure)**란 함수가 선언된 바깥쪽의 **지역 변수를 함수가 계속 사용할 수 있도록 유지하는 기능**이다.

C#에서는 주로 **람다식, 익명 메서드**를 통해 클로저가 만들어진다.

---

## 1. 기본 개념

일반적으로 지역 변수는 해당 메서드가 종료되면 더 이상 사용할 수 없다.

하지만 람다식이 해당 지역 변수를 참조하고 있다면, 해당 변수가 계속 유지된다.

```csharp
Func<int> CreateCounter()
{
    int count = 0;

    return () =>
    {
        count++;
        return count;
    };
}
```

```csharp
Func<int> counter = CreateCounter();

Console.WriteLine(counter());
Console.WriteLine(counter());
Console.WriteLine(counter());
```

```text
출력:
1
2
3
```

`CreateCounter()`가 종료된 이후에도 `count`가 유지된다.

이처럼 **함수와 함수가 참조하는 외부 변수를 함께 묶어서 유지하는 것**을 클로저라고 한다.

---

## 2. 클로저의 동작 원리

다음 코드를 살펴보자.

```csharp
Func<int> CreateCounter()
{
    int count = 0;

    return () => ++count;
}
```

실행 과정:

```text
CreateCounter()
    ↓
count = 0 생성
    ↓
람다식이 count를 참조
    ↓
CreateCounter() 종료
    ↓
count가 계속 유지됨
    ↓
counter() 호출
    ↓
count 증가
```

즉, `count`는 `CreateCounter()`가 종료되었다고 해서 바로 사라지지 않는다.

람다식에서 계속 사용하고 있기 때문이다.

---

## 3. 외부 지역 변수 참조

클로저는 람다식에서 외부 지역 변수를 사용할 때 쉽게 확인할 수 있다.

```csharp
int number = 10;

Func<int, int> multiply = x => x * number;

Console.WriteLine(multiply(5));
```

```text
출력:
50
```

람다식 내부의 `number`는 람다식의 매개변수가 아니다.

```text
number
  ↑
외부 지역 변수

x => x * number
     ↑
람다식에서 외부 변수 사용
```

이처럼 람다식이 외부 변수를 참조하면 클로저가 형성될 수 있다.

---

## 4. 변수의 값이 복사되는 것이 아니다

클로저에서 중요한 특징은 **외부 변수의 값을 단순히 복사해서 사용하는 것이 아니라는 점**이다.

```csharp
int number = 10;

Func<int> func = () => number;

number = 20;

Console.WriteLine(func());
```

```text
출력:
20
```

람다식을 만들었을 때 `number`가 `10`이었지만, 이후 `number`를 `20`으로 변경했기 때문에 `func()`는 `20`을 반환한다.

즉, 클로저는 외부 변수의 **현재 상태를 참조**한다.

---

## 5. 변수 변경

클로저 내부에서도 외부 변수를 변경할 수 있다.

```csharp
int count = 0;

Action increase = () =>
{
    count++;
};

increase();
increase();
increase();

Console.WriteLine(count);
```

```text
출력:
3
```

람다식에서 `count`를 변경했기 때문에 바깥쪽의 `count`도 변경된다.

---

## 6. 여러 클로저의 상태 유지

클로저를 여러 번 생성하면 **각각의 외부 변수를 독립적으로 유지**한다.

```csharp
Func<int> CreateCounter()
{
    int count = 0;

    return () => ++count;
}

Func<int> counter1 = CreateCounter();
Func<int> counter2 = CreateCounter();

Console.WriteLine(counter1());
Console.WriteLine(counter1());

Console.WriteLine(counter2());
Console.WriteLine(counter2());
```

```text
출력:
1
2
1
2
```

`counter1`과 `counter2`는 서로 다른 `count`를 가지고 있다.

```text
counter1
 └─ count = 2

counter2
 └─ count = 2
```

각각 `CreateCounter()`가 호출될 때 새로운 `count`가 생성되기 때문에 서로 영향을 주지 않는다.

---

## 7. 반복문에서 주의할 점

클로저는 반복문에서 사용할 때 주의해야 한다.

```csharp
var actions = new List<Action>();

for (int i = 0; i < 3; i++)
{
    actions.Add(() => Console.WriteLine(i));
}

foreach (var action in actions)
{
    action();
}
```

반복 변수의 캡처 방식 때문에 여러 람다식이 같은 변수를 참조할 수 있다.

각 반복의 값을 별도 지역 변수에 저장하면 이를 명확하게 분리할 수 있다.

```csharp
var actions = new List<Action>();

for (int i = 0; i < 3; i++)
{
    int index = i;

    actions.Add(() => Console.WriteLine(index));
}
```

각 람다식이 별도의 `index`를 캡처하게 된다.

---

## 8. 클로저와 람다식의 관계

클로저와 람다식은 같은 개념이 아니다.

### 람다식

```csharp
x => x * 2
```

단순히 함수를 표현하는 문법이다.

### 외부 변수를 사용하는 람다식

```csharp
int number = 10;

x => x * number
```

이처럼 람다식이 **자신의 외부에 있는 변수를 참조하고 해당 변수가 람다식의 수명 동안 유지되어야 하는 경우** 클로저가 만들어진다.

```text
람다식
  │
  ├─ 외부 변수 사용 X
  │    → 일반적인 람다식
  │
  └─ 외부 변수 사용 O
       → 클로저가 형성될 수 있음
```

따라서

> **모든 람다식이 클로저인 것은 아니다.**

---

## 9. 익명 메서드에서도 사용 가능

클로저는 람다식뿐만 아니라 익명 메서드에서도 사용할 수 있다.

```csharp
int number = 10;

Func<int> func = delegate
{
    return number;
};

number = 20;

Console.WriteLine(func());
```

```text
출력:
20
```

익명 메서드 역시 외부 변수를 캡처할 수 있다.

---

## 10. 클로저의 장점

| 장점     | 설명                                    |
| ------ | ------------------------------------- |
| 상태 유지  | 메서드가 종료되어도 필요한 지역 변수를 유지할 수 있음        |
| 간결한 코드 | 별도의 클래스를 만들지 않고 상태를 가진 함수를 만들 수 있음    |
| 데이터 은닉 | 특정 함수에서만 사용하는 상태를 외부에 직접 노출하지 않을 수 있음 |
| 활용성    | 콜백, 이벤트, LINQ 등에서 유용하게 사용 가능          |

---

## 11. 클로저의 주의점

### ① 변수의 수명이 길어질 수 있음

클로저가 외부 변수를 계속 참조하고 있다면 해당 변수도 클로저가 유지되는 동안 함께 유지될 수 있다.

필요 이상으로 큰 객체나 많은 데이터를 캡처하면 메모리 사용에 영향을 줄 수 있다.

---

### ② 외부 변수의 변경에 주의

```csharp
int value = 10;

Func<int> func = () => value;

value = 100;

Console.WriteLine(func());
```

```text
출력:
100
```

람다식이 생성될 당시의 `10`이 아니라 현재의 `100`을 사용한다.

---

### ③ 반복문에서 주의

```csharp
for (...)
{
    actions.Add(() => ...);
}
```

반복 변수나 외부 변수를 캡처할 경우 여러 람다식이 같은 변수를 참조할 수 있다.

반복문에서 람다식을 사용할 때는 **캡처되는 변수의 범위와 수명**을 확인하는 것이 중요하다.

---

## 12. 핵심 개념 정리

```text
외부 변수
   ↓
람다식 / 익명 메서드가 변수 참조
   ↓
변수 캡처
   ↓
클로저 형성
   ↓
함수와 외부 변수의 상태 유지
```

### 대표적인 예제

```csharp
Func<int> CreateCounter()
{
    int count = 0;

    return () => ++count;
}

Func<int> counter1 = CreateCounter();
Func<int> counter2 = CreateCounter();

Console.WriteLine(counter1()); // 1
Console.WriteLine(counter1()); // 2

Console.WriteLine(counter2()); // 1
Console.WriteLine(counter2()); // 2
```

각 클로저는 자신만의 `count`를 유지한다.

```text
counter1 → count = 2
counter2 → count = 2
```

---

## 13. 핵심 요약

> **클로저는 함수가 자신이 선언된 외부의 변수를 캡처하여, 함수가 실행되는 동안 해당 변수를 계속 사용할 수 있도록 하는 기능이다.**

```csharp
Func<int> CreateCounter()
{
    int count = 0;

    return () => ++count;
}
```

```text
CreateCounter()
      ↓
 count = 0
      ↓
람다식이 count 캡처
      ↓
메서드 종료
      ↓
 count 유지
      ↓
counter() 호출
      ↓
 count 증가
```

**클로저 = 함수가 외부 변수를 캡처하여 그 변수의 상태를 계속 사용할 수 있게 하는 기능**
