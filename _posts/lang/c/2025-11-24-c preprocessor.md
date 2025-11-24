---
title: "전처리기(Preprocessor)"
excerpt: "전처리기가 하는 역할들"
categories: [lang, c]
tags:
   - c
   - preprocessor
show_date: true
---

## 역할
- 헤더 파일 가져오기 : ```#include```를 사용한 경우
- 매크로를 실제로 대입하기 : ```#define``` 키워드를 사용한 경우
- 조건부 : ```#if, #elif, #ifndef, #endif``` 등을 통해 조건 분기
- 때로는 매크로를 끊어주기도 함 : ```#undef```

## 예시
- 헤더 파일이 여러번 포함되는 것을 방지 (그런데 이건 헤더 파일에서 정의를 안쓰면 되는거 아닌가..??)
```c
#ifndef HDR
#define HDR
/* contents of hdr.h go here */
#endif
```

- 다른 파일에서 정의된 매크로를 특정 파일에서 해제하고 싶을 때, 혹은 매크로 아닌 함수임을 명확히 할 때
```c
#undef getchar

int getchar(void) { ... }
```

## 장점
- 자주 사용하는 함수 매크로를 통해 런타임에서 function call로 인한 오버헤드를 줄일 수 있다.
- 모드 별(디버깅 등) 다른 전략을 구사할 수 있다.
- 헤더 가드 사용으로 같은 헤더 파일 사용 시 발생할 수 있는 문제점들을 방지할 수 있다.

## 참고
- 문자열화 연산자 ```#```(Stringizing operator) : 변수명을 그대로 문자열 변환
```c
#define dprint(expr) printf(#expr " = %g\n", expr)

...

dprint(x + 2);
// printf("x + 2" " = %g\n", x + 2);
```