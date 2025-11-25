---
title: "유닉스 시스템 인터페이스"
excerpt: "시스템 콜 등의 저수준 인터페이스"
categories: [lang, c]
tags:
   - c
   - unix
   - system call
   - interface
show_date: true
---

운영체제 쪽을 잘 몰라서 일단 간단히만 정리

## File Descriptors
유닉스 계열 OS에서 사용하는 추상적인 값. 그냥 음이 아닌 정수(0, 1, 2, ...)

### 왜씀?
- 파일, 네트워크 소켓, 파이프 등 종류에 상관없이 동일한 명령어로 다룰 수 있다.
- 프로세스 입장에서 'N번 파일'의 느낌으로 접근하게 되므로 효율적이다.
- 프로세스가 커널 내부의 파일을 직접 건드리지 않고 파일 디스크립터를 통해서 접근하여 보안에도 좋다.

### 흐름
- 프로세스가 시스템 콜을 보냄
- 커널이 대상 파일을 열고 정보를 커널 메모리 안에 저장
- 메모리 인덱스 정보만 프로세스에 전달

### File에 뭐가 포함됨?
- 일반 파일 (텍스트, 바이너리 파일)
- 디렉토리
- 파이프 (프로세스 간 통신)
- 소켓 (네트워크 통신)
- 디바이스 파일 (/dev/null, /dev/random 등)
- 터미널 (키보드, 화면)

### 왜 fd 0, 1, 2는 표준임?
모든 프로세스는 시작할 때 3개의 파일 디스크립터를 가지고 태어난다.
- 0: stdin (표준 입력 - 보통 키보드)
- 1: stdout (표준 출력 - 보통 화면)
- 2: stderr (표준 에러 - 보통 화면)

모든 프로그램의 일관성을 위해서 그리고 리다이렉션 시 편의성을 위해서의 이유가 있다.
```bash
# Redirection
$ ./myprog < input.txt > output.txt 2> error.log

# FD 0 → input.txt
# FD 1 → output.txt
# FD 2 → error.log
```

## Read / Write

실제 데이터를 주고받음

### read

```c
char buffer[100];
ssize_t bytes_read = read(fd, buffer, 100);
// fd에서 최대 100바이트를 읽어서 buffer에 저장
// 실제로 읽은 바이트 수를 반환
```

```c
char *message = "Hello";
ssize_t bytes_written = write(fd, message, 5);
// fd에 message를 5바이트 쓰기
// 실제로 쓴 바이트 수를 반환
```

### write

## Open / Create / Close / Unlink

파일 디스크립터의 생명주기를 관리 

### open
```c
int fd = open("file.txt", O_RDONLY);  // 읽기 전용으로 열기
int fd = open("file.txt", O_WRONLY);  // 쓰기 전용으로 열기
int fd = open("file.txt", O_RDWR);    // 읽기+쓰기로 열기
```

### create

파일을 생성하는 것이나, 그냥 ```open```에 ```O_CREAT``` 플래그를 붙여 사용하는 편.
```c
int fd = open("new.txt", O_CREAT | O_WRONLY, 0644);
// O_CREAT: 파일이 없으면 만들어라
// 0644: 파일 권한 (rw-r--r--)
```

### close

```c
close(fd);  // 파일 디스크립터 닫기
```

### unlink

```c
unlink("file.txt");  // 파일 삭제
```

### Lseek
파일의 현재 읽기/쓰기 위치를 변경한다. 마치 배열 접근하는 것처럼 동작하고 fd의 위치는 커널 구조체에 저장된다.
```c
off_t lseek(int fd, off_t offset, int whence);

// candidates for whence
// SEEK_SET: 파일의 시작부터
// SEEK_CUR: 현재 위치부터
// SEEK_END: 파일의 끝부터
```

```off_t```는 파일 위치를 표현하기 위한 타입이라고 생각하면 된다. 32/64비트 시스템의 차이 등으로 일반적인 ```int```, ```long```과는 구분하려고 만듬.