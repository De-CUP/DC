# C# Reflection이란?

Reflection(리플렉션)이란, 프로그램이 **런타임에 자기 자신의 구조를 탐색하고 조작**할 수 있는 기능이다. <br>
`System.Reflection` 네임스페이스를 통해 클래스, 메서드, 프로퍼티, 필드 등의 메타데이터를 동적으로 읽고, 경우에 따라 호출하거나 수정할 수 있다.


즉, 컴파일 타임에 타입을 알지 못해도, 런타임에 타입 정보를 꺼내어 동적으로 활용할 수 있는 메커니즘이다.

## 핵심 개념

Reflection의 진입점은 `Type` 객체다. 이를 통해 해당 타입의 모든 구조 정보에 접근할 수 있다.

```csharp
Type type = typeof(Person);         // 컴파일 타임에 타입을 알 때
Type type = obj.GetType();          // 런타임 인스턴스로부터
Type type = Type.GetType("MyApp.Person"); // 문자열로 타입 이름을 알 때
```

## 예시

### 1. 타입 정보 탐색

```csharp
Type type = typeof(Person);

Console.WriteLine(type.Name);       // "Person"
Console.WriteLine(type.Namespace);  // "MyApp"
Console.WriteLine(type.IsClass);    // true
```

### 2. 프로퍼티 정보 읽기

```csharp
Type type = typeof(Person);

foreach (var prop in type.GetProperties())
{
    Console.WriteLine($"{prop.Name} : {prop.PropertyType.Name}");
}
```

> `GetProperties()`는 기본적으로 public 프로퍼티만 반환한다. <br>
> private 멤버까지 접근하려면 `BindingFlags`를 함께 전달해야 한다.

```csharp
var props = type.GetProperties(BindingFlags.NonPublic | BindingFlags.Instance);
```

### 3. 런타임 인스턴스 생성

컴파일 타임에 타입을 몰라도 문자열 이름만으로 객체를 생성할 수 있다.

```csharp
Type type = Type.GetType("MyApp.Person");
object instance = Activator.CreateInstance(type);
```

### 4. 메서드 동적 호출

```csharp
Type type = typeof(Calculator);
object obj = Activator.CreateInstance(type);

MethodInfo method = type.GetMethod("Add");
object result = method.Invoke(obj, new object[] { 3, 5 });

Console.WriteLine(result); // 8
```

## 활용 사례

- **직렬화/역직렬화**: `System.Text.Json`, `Newtonsoft.Json`이 내부적으로 Reflection을 사용해 프로퍼티를 읽고 쓴다.
- **DI 컨테이너**: ASP.NET Core의 DI가 생성자 파라미터 타입을 Reflection으로 분석해 자동 주입한다.
- **ORM**: Entity Framework가 엔티티 클래스의 프로퍼티를 Reflection으로 읽어 SQL을 생성한다.
- **커스텀 Attribute 처리**: 특정 Attribute가 붙은 멤버를 런타임에 찾아 처리하는 로직에 활용된다.


## 정리

Reflection은 런타임 동적성이 반드시 필요한 상황, 즉 프레임워크나 라이브러리 개발, 플러그인 시스템, 범용 직렬화 등에서 진가를 발휘한다. 하지만 일반 비즈니스 로직에서는 성능과 타입 안전성을 해치는 과도한 사용은 지양하는 것이 좋다.