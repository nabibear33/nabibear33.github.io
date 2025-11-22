---
title: "정의(definition)과 선언(declaration)"
excerpt: "정의와 선언의 비교 및 예시"
categories: [lang]
tags:
   - definition
   - declaration
show_date: true
---


## 뜻이 어떻게 다름?
### 선언
- 메모리에 할당되지 않음
- "이따가 이거 쓸거야"
- 컴파일러에게 전달될 요소들 포함

### 정의
- 메모리에 할당
- 링커가 원하는 요소들 포함
- 한 번만 되어야 한다.
   - 정의가 없는 경우, 링커는 참조할 정의를 찾지 못한다.
   - 정의가 여러 개인 경우, 링커는 어떤 정의를 참조할 지 모른다.

## 예시
```c
// 선언만 (어딘가에 정의가 있다고 알림)
extern int x;              // 변수 선언
int add(int a, int b);     // 함수 선언

// 정의 (선언 + 실제 생성)
int x = 10;                // 변수 정의
int add(int a, int b) {    // 함수 정의
    return a + b;
}
```

## 왜 헷갈림?
번역의 문제(?)도 있는 것 같고 상황에 따라 좀 모호한 부분도 있어 보인다.

## 이걸 구분하는게 중요할까?
뭔가 정의를 직접적으로 알고있다기 보다는 컴파일이나 링커가 동작할 때 발생할 수 있는 문제되는 코드 예시를 알고있는게 더 중요해 보인다.

- 에러 예시: 헤더 파일에서 함수가 definition되는 경우 링커 에러

```c
// utils.h
int add(int a, int b) {
    return a + b;
}

// file1.cpp
#include "utils.h"  // add 함수 코드가 여기 복사됨

// file2.cpp  
#include "utils.h"  // add 함수 코드가 여기도 복사됨
```
```bash
>> gcc file1.c file2.c utils.h
multiple definition of 'add'
```

## 참고
C++에서 템플릿 함수 여러번 인스턴스화 될 수 있는데, 링커가 중복을 탐지하여도 에러를 발생시키지 않는다. 그래서 헤더에도 사용이 가능하다. (오히려 헤더에서 정의해야한다. .cpp에 정의하게 되면 컴파일러는 템플릿을 함수 코드로 작성하지 않을 것이고 링커에서 에러가 발생)

```cpp
// utils.h
template<typename T>
T max(T a, T b) {
    return a > b ? a : b;
}

// file1.cpp
#include "utils.h"
int x = max(3, 5);      // max<int>

// file2.cpp
#include "utils.h"
int y = max(7, 2);      // max<int> (같은 타입!)
```