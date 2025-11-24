---
title: "Static Variables"
excerpt: "static 변수를 언제 쓰고 왜 유용한지"
categories: [lang, c]
tags:
   - c
   - static
show_date: true
---

## 

## 왜 ```static```일까?

원래 함수의 수명에 따라 달라지는 지역 변수의 수명을 정적으로 고정한다는 의미로 사용할 수 있고, 링크 범위도 고정되므로 의미적으로 그렇게 생각할 수 있다. 원래 목적은 이와 같이 저장 기간 및 링킹을 제어하기 위해 만들어졌다.

## 함수 외부의 ```static```

해당 파일 내에서만 접근할 수 있도록 한다. 파일 내부에서만 변수를 수정할 수 있도록 하고 싶을 때 사용하여, 외부에서 ```extern```으로 참조 시 linker error를 일으킨다.

```c
// file1.c
#include <stdio.h>

static int secret = 100;  // Only visible in file1.c

void print_secret() {
    printf("Secret: %d\n", secret);  // OK - same file
}
```

```c
// file2.c
#include <stdio.h>

extern int secret;  // Trying to access secret from file1.c

void try_access() {
    printf("%d\n", secret);  // Will this work?
}
```

**Error:**
```
Compile: ✓ Both files compile fine
Link:    ✗ undefined reference to 'secret'
```

## 함수 내부의 ```static```

함수 내부에서도 ```static``` 변수를 사용할 수 있다.

```c
#include <stdio.h>

void counter() {
    static int count = 0;
    count++;
}

int main() {
    counter();
    printf("%d\n", count);  // Trying to access count outside function
    return 0;
}
```

**Error:**
```
error: 'count' undeclared (first use in this function)
```

## 예시
- 비싼 초기화 방지
```c
void get_config() {
    static int initialized = 0;
    static Config config;
    
    if (!initialized) {
        config = load_expensive_config();  // Only do this ONCE
        initialized = 1;
    }
    return config;
}
```
- UUID 생성
```c
int generate_id() {
    static int next_id = 1;
    return next_id++;
}
```
- 전역 상태 관리
```c
void process_event(Event e) {
    static State current_state = IDLE;
    current_state = transition(current_state, e);
}
```

## 정리
- 메모리의 data segment에 할당되어 프로그램 실행 종료까지 유지하여 초기화 비용을 줄인다.
- 다른 파일/함수에서 강제로 변수에 접근하는 것을 막는다.

## 추가
- C++의 경우에는 클래스에서도 ```static```을 사용한다.
