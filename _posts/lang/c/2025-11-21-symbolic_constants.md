---
title: "기호 상수(symbolic constants)"
excerpt: "symbolic constant가 나오게 된 배경, 쓰임 등을 정리"
categories: [lang, c]
tags:
   - c
   - "#define"
   - const
   - symbolic constant
show_date: true
---


## 왜 쓰게 됨?


```c
int buffer[100];
for (int i = 0; i < 100; i++) { ... }
if (count > 100) { ... }
```

다음과 같은 상황에서 버퍼의 크기를 바뀌는 경우 유지 보수하기 어려워진다. 이러한 상황을 해결하기 위해 사용된다.

## 언제 씀?

- 고정된 크기의 배열
- 물리, 수학 상수
- 포트 번호
- 에러 코드, 디버깅
- 기타 반복되어 사용되는 값

## 동작 방식

### 1. #define을 사용하는 경우
전처리기(preprocessor)가 컴파일 전에 텍스트 치환하게 된다.

전처리기 이전
```c
#define MAX_SIZE 100
#define PI 3.14159
#define COMPANY_NAME "TechCorp"

int main() {
    int array[MAX_SIZE];
    double area = PI * radius * radius;
    // ...
}
```

전처리기 이후
```c
int main() {
    int array[100];
    double area = 3.14159 * radius * radius;
}
```

세미콜론(;)이 붙지 않는 이유도 단순 텍스트 치환이기 때문이라 할 수 있다.

그 이외의 특징에 대해서는

- 배열의 크기를 선언할 때에도 사용할 수 있다
- 메모리를 차지하지 않는다. (전처리기가 컴파일 이전에 문자로 치환해버리기 때문)
- 조건부 컴파일이 가능하다.\\
   ```c
   #define DEBUG_MODE // 요걸 켜고 끄면서 컴파일 되는 내용을 조절할 수 있다

   int main() {
      int result = calculate();
      
   #ifdef DEBUG_MODE
      printf("Debug: result = %d\n", result);
      printf("Debug: memory usage = %d\n", get_memory());
   #endif
      
      return result;
   }
   ```


### 2. const를 사용하는 경우
이 키워드는 ANSI C(=C89=C90) 표준부터 사용되었다. 타입 안정성 향상을 위해 도입되었다고 한다. C++가 1985년부터 ```const```를 지원하였기 때문에 이 영향도 있어 보인다.

- 처음 나왔을 때에는 배열의 크기를 결정할 수 없다는 단점이 있었으나, C99부터는 VLA(variable length arrays)를 이용하여 런타임에서도 동작하도록 하였다.
- 디버깅할 때 심볼을 확인할 수 있어 편리하다.
- 컴파일 시에 정해지는 상수가 아닌 런타임에 정해지는 변수다.

---

### 참고

gcc 같은 컴파일러에 옵션으로 넣어 줄 수도 있다.

```bash
# 값 없이 정의만
gcc -DDEBUG program.c
# → #define DEBUG

# 값과 함께 정의
gcc -DMAX_SIZE=200 program.c
# → #define MAX_SIZE 200

# 문자열 정의
gcc -DVERSION=\"1.0.0\" program.c
# → #define VERSION "1.0.0"

# 여러 개 정의
gcc -DDEBUG -DMAX_SIZE=200 -DVERSION=\"1.0\" program.c
```