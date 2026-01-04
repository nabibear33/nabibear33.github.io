---
title: "Initialization"
excerpt: "초기화의 종류"
categories: [lang, cpp]
tags:
   - cpp
   - initialization
show_date: true
---

# 5 common initializations

## Default initialization
```cpp
int a;
```

Garbage value를 둔 채로 초기화

## Copy initialization
```cpp
int a = 5;
```

임시 객체를 만들고 이 값을 생성된 a의 주소에 넣는 방식이다. 역사적으로는 C언어의 잔재로 이어져 온 형태이다. C++11, C++14 등에서는 copy initialization이 클래스 초기화 시 불필요한 복사를 야기할 수 있어 자제되었으나, C++17에서 이를 해결하여 성능 문제를 제거하였다.

```cpp
// C++17 이전: 이론상 복사 2번
MyClass func() {
    return MyClass(42);  // 1. 임시 객체 → 반환값 복사
}
MyClass obj = func();    // 2. 반환값 → obj 복사

// C++17 이후: guaranteed elision으로 복사 0번
// obj 위치에 직접 생성
```

### copy elision

컴파일러가 불필요한 복사를 생략하는 최적화이다. c++17 이후에는 특정 상황에서 이를 의무화하는 guaranteed copy elision이 적용되었다.

사용 예시
- 함수의 return문에서의 임시 객체에 적용
- 함수 인자 전달 시
- try-catch 문에서 exception에 전달되는 e
- prvalue(pure rvalue)로부터 초기화될 때

### explicit 생성자와의 관계
Explicit 생성자는 implicit conversion을 하지 못하도록 막는데, copy initialization은 implicit conversion이 발생할 수 있기 때문에 사용하는 것이 불가능하다.

## Direct initialization
```cpp
int c(6);
```

초기 목적은 복잡한 객체(클래스)를 효율적으로 초기화하기 위해 도입되었다. 이후 C++11에서 direct-list initialization이 도입되면서 대체되었으나 특정한 경우들에서는 여전히 사용된다.

### static_cast
```static_cast<T>(d)```는 내부적으로
1. 임시 ```T``` 객체를 만든다.
2. 임시 객체를 ```d```로 초기화한다.
3. 임시 객체를 반환한다.

이 때 2번 단계에서 direct initialization이 동작한다. 그러면 일반 ()랑 무슨 차이일까? C 스타일의 ```(type)```은 강제성이 높기 때문에(const를 제거하거나, 포인터를 재해석하는 등) 런타임 시점에서 문제가 발생할 수 있다. 이를 해결하기 위해 논리적으로 타당한 변환만 허용하도록 컴파일하는 ```static_cast```를 사용한다.

```cpp
class Base { };
class Derived : public Base { };

Base* base = new Base();
Derived* derived1 = (Derived*)base;  // 위험! 런타임 문제 발생 가능
Derived* derived2 = static_cast<Derived*>(base);  // 컴파일 에러!

// 참고: static cast와 비슷한 function-style cast가 있긴 한데 일관성, 가독성 측면에서 static_cast 쓰는 걸 권장
int* q1 = (int*)p;
int* q2 = int*(p); // 위랑 헷갈림, 캐스팅 검색하기 불편
```

## Direct-list initialization
```cpp
int a {5};
```



### direct-list 사용시 문제가 발생하는 특정한 경우들
- initializer_list 생성자와의 우선순위 문제

```cpp
std::vector<int> v1(10, 5);   // direct-init: 10개의 5
std::vector<int> v2{10, 5};   // list-init: 2개 요소 [10, 5]

// 내부적으로
vector(size_t count, const T& value);  // (1) 생성자
vector(initializer_list<T> init);       // (2) 생성자

// 참고
std::vector<int> v3({1, 2});   // error, 괄호 내에는 expression만 사용가능(C++03 시절부터...). 그래서 C++11에서는 새로운 문법 규칙으로 대괄호를 바로 사용하는 방식으로 설계
```

- 함수 선언과의 충돌
- {} 초기화에서는 narrowing conversion이 금지됨
- auto와 함께 사용 시 예상치 못한 타입이 생성


## Value initialization
```cpp
int c {};
```

컴파일러가 적절한 기본값으로 알아서 초기화 시켜준다.