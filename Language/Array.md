# C# Array

## 1. Array란?

`Array`는 **같은 타입의 여러 데이터를 하나의 변수로 관리하는 자료구조**이다.

배열의 크기는 생성할 때 결정되며 이후 변경할 수 없다.

```csharp
int[] numbers = new int[3];

numbers[0] = 10;
numbers[1] = 20;
numbers[2] = 30;
```

---

## 2. 배열 초기화

```csharp
int[] numbers = { 10, 20, 30 };
```

또는

```csharp
int[] numbers = new int[] { 10, 20, 30 };
```

---

## 3. 인덱스

배열의 인덱스는 **0부터 시작**한다.

```csharp
int[] numbers = { 10, 20, 30 };

Console.WriteLine(numbers[0]); // 10
Console.WriteLine(numbers[2]); // 30
```

```text
numbers
[0] [1] [2]
 10  20  30
```

존재하지 않는 인덱스에 접근하면 `IndexOutOfRangeException`이 발생한다.

---

## 4. 배열의 크기

`Length`를 사용하면 배열의 요소 개수를 확인할 수 있다.

```csharp
int[] numbers = { 10, 20, 30 };

Console.WriteLine(numbers.Length); // 3
```

---

## 5. 반복문과 배열

```csharp
int[] numbers = { 10, 20, 30 };

for (int i = 0; i < numbers.Length; i++)
{
    Console.WriteLine(numbers[i]);
}
```

`foreach`를 사용하면 더 간단하게 순회할 수 있다.

```csharp
foreach (int number in numbers)
{
    Console.WriteLine(number);
}
```

---

## 6. 다차원 배열

여러 차원의 데이터를 저장할 수 있다.

```csharp
int[,] map =
{
    { 1, 2 },
    { 3, 4 }
};

Console.WriteLine(map[0, 1]); // 2
```

대표적인 배열 형태:

```text
int[]     → 1차원 배열
int[,]    → 2차원 배열
int[,,]   → 3차원 배열
```

---

## 7. 배열의 특징

| 특징    | 내용               |
| ----- | ---------------- |
| 자료형   | 같은 타입의 데이터 저장    |
| 크기    | 생성 후 변경 불가       |
| 인덱스   | 0부터 시작           |
| 접근    | 인덱스를 통한 빠른 접근    |
| 크기 확인 | `Length`         |
| 순회    | `for`, `foreach` |

## 핵심 요약

> **Array는 같은 타입의 여러 데이터를 연속적으로 저장하며, 생성할 때 크기가 결정되는 자료구조이다.**

```text
Array
 ├─ 같은 타입의 데이터
 ├─ 인덱스는 0부터 시작
 ├─ Length로 크기 확인
 └─ 생성 후 크기 변경 불가
```
