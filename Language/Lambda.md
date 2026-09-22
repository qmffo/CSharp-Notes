# 람다식 (Lambda Expression)

메서드를 **간결한 표현식 형태로 작성하는 방법**이다.

주로 **델리게이트(Delegate)**, `Action`, `Func`, LINQ 등에서 사용한다.

익명 메서드보다 문법이 간결하며, 현대 C#에서 익명 함수를 작성할 때 많이 사용된다.

---

## 1. 기본 문법

```csharp
(매개변수) => 표현식
```

또는 여러 줄의 코드를 작성할 경우:

```csharp
(매개변수) =>
{
    // 실행할 코드
};
```

`=>`는 **람다 연산자(Lambda Operator)**라고 한다.

왼쪽에는 매개변수, 오른쪽에는 실행할 코드를 작성한다.

---

## 2. 기본 예제

```csharp
Func<int, int> func = number => number * 2;

int result = func(10);

Console.WriteLine(result);
```

```text
출력:
20
```

다음과 같이 해석할 수 있다.

```text
number를 전달받아서
number * 2를 반환한다.
```

---

## 3. 매개변수가 여러 개인 경우

매개변수가 2개 이상이면 괄호를 사용한다.

```csharp
Func<int, int, int> add = (a, b) => a + b;

int result = add(10, 20);

Console.WriteLine(result);
```

```text
출력:
30
```

### 매개변수 규칙

```csharp
// 매개변수 0개
() => 실행할 코드

// 매개변수 1개
number => 실행할 코드

// 매개변수 2개 이상
(a, b) => 실행할 코드
```

---

## 4. 여러 줄의 코드 작성

실행할 코드가 여러 줄이라면 `{ }`를 사용한다.

```csharp
Func<int, int> func = number =>
{
    int result = number * 2;

    return result;
};
```

여러 줄을 사용하는 람다식에서는 반환값이 필요한 경우 `return`을 사용한다.

---

## 5. 반환값이 없는 람다식

`Action`을 사용하면 반환값이 없는 람다식을 만들 수 있다.

```csharp
Action action = () =>
{
    Console.WriteLine("Hello");
};

action();
```

매개변수를 사용할 수도 있다.

```csharp
Action<string> action = message =>
{
    Console.WriteLine(message);
};

action("Hello");
```

---

## 6. 반환값이 있는 람다식

`Func`를 사용하면 반환값을 가지는 람다식을 만들 수 있다.

```csharp
Func<int, int> square = number => number * number;

int result = square(5);

Console.WriteLine(result);
```

```text
출력:
25
```

`Func`의 **마지막 제네릭 타입은 반환 타입**이다.

```csharp
Func<int, int, int>
```

위 코드는 다음과 같은 의미이다.

```text
첫 번째 int  → 첫 번째 매개변수
두 번째 int  → 두 번째 매개변수
세 번째 int  → 반환 타입
```

---

## 7. 익명 메서드와 비교

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

### 람다식 축약

```csharp
Func<int, int> func = number => number * 2;
```

같은 동작을 더 짧게 표현할 수 있다.

---

## 8. 조건식 사용

람다식 내부에서 조건식을 사용할 수도 있다.

```csharp
Func<int, string> checkNumber = number =>
    number > 0 ? "양수" : "0 또는 음수";

Console.WriteLine(checkNumber(10));
```

```text
출력:
양수
```

---

## 9. LINQ에서 사용

람다식은 LINQ에서 자주 사용된다.

```csharp
int[] numbers = { 1, 2, 3, 4, 5 };

var result = numbers.Where(number => number > 3);
```

`number => number > 3`은 다음과 같은 의미이다.

```text
number를 하나씩 확인해서
number가 3보다 큰 경우만 선택한다.
```

결과:

```text
4
5
```

---

## 10. 정렬에 사용

`Sort()`에서도 람다식을 사용할 수 있다.

```csharp
List<int> numbers = new List<int>
{
    5, 2, 8, 1, 3
};

numbers.Sort((a, b) => a.CompareTo(b));
```

결과:

```text
1, 2, 3, 5, 8
```

내림차순으로 정렬하려면:

```csharp
numbers.Sort((a, b) => b.CompareTo(a));
```

---

## 11. 외부 변수 사용

람다식도 자신이 선언된 외부 범위의 변수를 사용할 수 있다.

```csharp
int multiplier = 10;

Func<int, int> multiply = number => number * multiplier;

Console.WriteLine(multiply(5));
```

```text
출력:
50
```

람다식 외부의 지역 변수에 접근할 수 있으며, 이러한 동작은 **클로저(Closure)**와 관련이 있다.

---

## 12. 람다식의 형태

### 표현식 람다

실행할 코드가 하나의 표현식으로 끝나는 경우:

```csharp
number => number * 2
```

반환값이 자동으로 결정된다.

```csharp
Func<int, int> func = number => number * 2;
```

---

### 문장 람다

여러 문장을 작성해야 하는 경우:

```csharp
number =>
{
    int result = number * 2;

    return result;
}
```

`{ }`를 사용하며 반환값이 있다면 `return`을 작성한다.

---

## 13. 타입 추론

람다식의 매개변수 타입은 대상 델리게이트의 타입을 통해 추론할 수 있다.

```csharp
Func<int, int> func = number => number * 2;
```

`number`가 `int`라는 것을 직접 작성하지 않아도 컴파일러가 추론한다.

다음과 같이 타입을 직접 작성할 수도 있다.

```csharp
Func<int, int> func = (int number) => number * 2;
```

하지만 일반적으로는 타입 추론을 이용하여 간결하게 작성한다.

---

## 14. 주요 특징

| 특징     | 설명                                 |
| ------ | ---------------------------------- |
| 문법     | `=>` 사용                            |
| 목적     | 이름 없는 함수를 간결하게 표현                  |
| 매개변수   | 0개, 1개, 여러 개 사용 가능                 |
| 반환값    | 표현식 람다는 결과를 자동 반환                  |
| 여러 문장  | `{ }` 사용                           |
| 주요 사용처 | `Action`, `Func`, LINQ, 이벤트, 델리게이트 |
| 외부 변수  | 접근 가능                              |
| 장점     | 코드가 짧고 간결함                         |
| 단점     | 복잡한 로직에서는 가독성이 떨어질 수 있음            |

---

## 15. 핵심 문법 정리

```csharp
// 매개변수 없음
() => 실행할 코드

// 매개변수 1개
x => 실행할 코드

// 매개변수 여러 개
(x, y) => 실행할 코드

// 반환값
x => x * 2

// 여러 줄
x =>
{
    int result = x * 2;
    return result;
};
```

---

## 16. 핵심 요약

> **람다식은 이름이 없는 메서드를 `=>` 문법으로 간결하게 표현하는 방법이다.**

```csharp
// 일반 메서드
int Double(int number)
{
    return number * 2;
}

// 익명 메서드
Func<int, int> func = delegate(int number)
{
    return number * 2;
};

// 람다식
Func<int, int> func = number => number * 2;
```

### 기억할 것

```text
() => 코드
       ↑
     실행 내용

x => x * 2
↑      ↑
매개변수 반환값

(x, y) => x + y
  ↑         ↑
매개변수    반환값
```

**람다식 = `=>`를 이용해 익명 함수를 간결하게 표현하는 문법**
