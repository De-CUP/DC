# Syntax Sugar란?

Syntax Sugar(문법 설탕)란, 프로그래밍 언어에서 기존 기능을 더 간결하고 읽기 쉽게 표현할 수 있도록 제공하는 문법적 편의 기능이다. <br>
실제 동작은 동일하지만, 코드 작성자와 독자 모두가 더 쉽게 이해할 수 있도록 설계된 것이 특징이다.

즉, 컴파일러 혹은 인터프리터가 내부적으로 동일하게 처리하지만, 개발자에게 더 달콤하고 편한 문법을 제공하는 것이다.

## 예시 - C#

### 1. `var` - 타입 추론 (암묵적 타입 선언)

변수 선언 시 타입을 명시하는 대신, 컴파일러가 우변의 값을 보고 타입을 자동으로 추론한다.


```csharp
// Before
string name = "Sean-mn";
int age = 19;
List items = new List();

// After
var name = "Sean-mn";
var age = 19;
var items = new List();
```

> 컴파일 타임에 타입이 결정되므로 동적 타입과는 다르다. 런타임 성능 차이도 없다.

---

### 2. 자동 구현 프로퍼티 (Auto-Implemented Property)

전통적인 프로퍼티는 별도의 백킹 필드를 선언해야 했지만, 자동 구현 프로퍼티를 사용하면 컴파일러가 알아서 처리해준다.

```csharp
// Before
private string _name;
public string Name
{
    get { return _name; }
    set { _name = value; }
}

// After
public string Name { get; set; }
```

---

### 3. 객체 초기화 구문 (Object Initializer)

생성자를 호출한 뒤 프로퍼티를 한 줄씩 설정하는 대신, 객체 생성 시점에 중괄호 블록으로 초기화할 수 있다.

```csharp
// Before
var person = new Person();
person.Name = "Sean-mn";
person.Age = 19;

// After
var person = new Person { Name = "Sean-mn", Age = 19 };

```

### 4. 문자열 보간 (String Interpolation)

`string.Format()`으로 포맷을 지정하던 방식 대신, `$` 접두사를 붙여 문자열 안에 직접 표현식을 삽입할 수 있다.

```csharp
string name = "Sean-mn";
int age = 19;

// Before
string message = string.Format("이름: {0}, 나이: {1}", name, age);

// After 
string message = $"이름: {name}, 나이: {age}";
```

---

### 5. 람다 표현식 (Lambda Expression)

익명 메서드를 더 간결하게 표현한 것으로, `delegate` 키워드를 명시하던 방식을 `=>` 연산자로 축약한다.

```csharp
List numbers = new List { 1, 2, 3, 4, 5 };

// Before
List evens = numbers.FindAll(delegate(int n) { return n % 2 == 0; });

// After
List evens = numbers.FindAll(n => n % 2 == 0);
```

---


## 정리

문법 설탕은 코드 가독성과 생산성을 높이는 강력한 도구이지만, 내부 동작 방식을 이해하지 못한 채 남용하면 디버깅이 어려워질 수 있다. 적재적소에 활용하는 것이 중요하다.