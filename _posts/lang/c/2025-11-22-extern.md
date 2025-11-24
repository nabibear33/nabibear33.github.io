---
title: "extern 키워드"
excerpt: "extern 역할과 사용 예"
categories: [lang, c]
tags:
   - c
   - extern
show_date: true
---


## 왜 쓰게 됨?
프로그램의 규모가 커지면 여러 파일로 나누어 코드를 관리하게 된다. 이 때 공통으로 사용하는 변수를 각 코드 파일마다 선언하게 되면 유지보수의 측면에서 좋지 않다.

## 언제 씀?
- 다른 파일의 전역 변수를 사용할 때

```c
// config.c
int max_users = 100;  // 정의 (메모리 할당)

// main.c
extern int max_users;  // 선언 (다른 파일 변수 참조)
void func() {
    max_users = 200;  // config.c의 변수 사용
}
```

- 헤더 파일에서 전역 변수를 선언할 때

```c
// config.h
extern int max_users;  // 선언만 (여러 파일에서 include 가능)

// config.c
int max_users = 100;   // 실제 정의는 .c 파일에
```
- (가끔) 같은 파일 내에서도 의도를 명시하고자 할 때 (같은 이름의 지역 변수를 정의하는 것 방지 등의 목적)

## 특징
- 정의는 한 번만 하되, 여러 번 선언할 수 있다. (보통 .c에 정의하고 .h나 다른 .c에서 선언한다.)
- 같은 스코프에서 지역 변수와 동시에 선언할 수 없다.

```c
#include <stdio.h>

int a = 0;  // 전역

void func() {
    int a = 5;     // 지역
    extern int a;  // 이게 가능할까?
    printf("%d\n", a);
}

int main() {
    func();
    return 0;
}
```
```bash
error: extern declaration of 'a' follows declaration with no linkage
```

## 참고
헤더 파일에서 이런 식의 선언은 안된다. 왜 그럴까

```c
// config.h
int max_users = 100;  // extern 없이

// main.c, network.c 둘 다 #include "config.h"
```

전처리기는 ```config.h```의 내용을 ```main.c```, ```network.c```에 복사하고, 링커가 합칠 때 중복된 정의로 에러를 발생시킨다.

```bash
Error: multiple definition of 'max_users'
```