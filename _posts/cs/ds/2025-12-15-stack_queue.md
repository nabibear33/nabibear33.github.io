---
title: "CHAPTER 3 STACKS AND QUEUES"
excerpt: ""
categories: [cs, ds]
tags:
   - data structure

show_date: true
---

# 3.1 The Stack Abstract Data Type

LIFO 구조로 되어 있다.

### 기본 연산들
- push: 값 추가
- pop: 값 제거 및 반환
- peek / top: 값 확인만
- isEmpty: 크기가 0인지
- isFull: 꽉 찼는지

### Array Stack
```cpp
#define MAX_SIZE 100
int stack[MAX_SIZE];
int ptr = -1;

void push(int n) {
    if(isfull()) {
        throw runtime_error("Stack overflow");
    }
    stack[++ptr] = n;
}

int pop() {
    if(isempty()) {
        throw runtime_error("Stack underflow");
    }
    return stack[ptr--];
}

int top() {
    if(isempty()) {
        throw runtime_error("Stack is Empty");
    }
    return stack[ptr];
}

bool isempty() {
    return ptr == -1;
}

bool isfull() {
    return ptr == (MAX_SIZE - 1);
}
```
### Linked list Stack
```cpp
struct Node {
    Node* next;
    int value;
};
Node* ptr = nullptr;
int size = 0;

void push(int n) {
    Node* new_ptr = new Node({ptr, n});
    ptr = new_ptr;
    size++;
}

int pop() {
    if(isempty()) {
        throw runtime_error("Stack underflow");
    }
    int value = ptr->value;
    Node* temp_ptr = ptr->next;
    delete ptr;
    ptr = temp_ptr;
    return value;
}

int top() {
    if(isempty()) {
        throw runtime_error("Stack is Empty");
    }
    return ptr->value;
}

bool isempty() {
    return (ptr == nullptr);
}

void clearStack() {
    while(ptr != nullptr) {
        Node* temp = ptr->next;
        delete ptr;
        ptr = temp;
    }
    size = 0;
}
```
### 복잡도
Array, linked list 모두 push, pop, top, isempty, isfull 연산에 $O(1)$

### Call stack

함수를 호출할 때 사용하는 call stack도 stack이다. 재귀 함수를 무한히 많이 호출할 때 발생하는 stack overflow는 이러한 call stack의 용량을 초과했을 때 발생하는 오류이다.

Stack frame은 함수 호출 1회 당 스택에 생성되는 메모리 블록을 말한다. 보통 함수의 인자, 반환되는 주소, 이전 함수 프레임 주소, 지역 변수 등이 들어간다.

# 3.2 The Queue Abstract Data Type

FIFO 구조로 되어 있다. 다음과 같은 연산을 정의할 수 있다.
- push: 값 삽입
- front: 가장 앞에 있는 값 반환
- back: 가장 뒤에 있는 값 반환
- pop: 가장 앞의 값 제거

### Array queue
front + 1 인덱스부터 저장이 시작되는 circular queue
```cpp
#define MAX_SIZE 101
int queue[MAX_SIZE];
int front = 0;
int rear = 0;

void push(int n) {
    if(isfull()) {
        throw runtime_error("Queue overflow");
    }
    rear = (rear + 1) % MAX_SIZE;
    queue[rear] = n;
}

int pop() {
    if(isempty()) {
        throw runtime_error("Queue underflow");
    }
    front = (front + 1) % MAX_SIZE;
    return queue[front];
}

int front() {
    if(isempty()) {
        throw runtime_error("Queue is Empty");
    }
    return queue[(front + 1) % MAX_SIZE];
}

int back() {
    if(isempty()) {
        throw runtime_error("Queue is Empty");
    }
    return queue[rear];
}

bool isempty() {
    return (rear - front) == 0;
}

bool isfull() {
    return ((rear + 1) % MAX_SIZE) == front;
}
```
### Linked list queue
스택과 다르게 push는 뒤, pop은 앞에서 이루어지기 때문에 두가지 정보를 필요로 한다.
```cpp
struct Node {
    Node* next;
    int value;
};
Node* head = nullptr;
Node* tail = nullptr;

void push(int n) {
    Node* new_ptr = new Node({nullptr, n});
    if(isempty()) {
        head = new_ptr;
    } else {
        tail->next = new_ptr;
    }
    tail = new_ptr;
}

int pop() {
    if(isempty()) {
        throw runtime_error("Queue underflow");
    }
    Node* temp_ptr = head;
    int value = temp_ptr->value;
    head = temp_ptr->next;
    delete temp_ptr;
    if(head == nullptr) tail = nullptr;
    return value;
}

int front() {
    if(isempty()) {
        throw runtime_error("Queue is Empty");
    }
    return head->value;
}

int back() {
    if(isempty()) {
        throw runtime_error("Queue is Empty");
    }
    return tail->value;
}

bool isempty() {
    return (head == nullptr);
}

void clearQueue() {
    while(head != nullptr) {
        Node* temp = head->next;
        delete head;
        head = temp;
    }
}
```

# 3.3 A Mazing Problem
Stack, queue를 이용하여 길찾기를 할 수 있다.

# 3.4 Multiple Stacks And Queues

2개의 stack을 사용하는 경우 반대 방향으로 자라는 stack을 구현하면 모든 메모리를 사용하면서도 실용적이다. 

여러개의 stack을 같은 메모리 상에서 사용할 경우 n등분하여 사용하거나 동적으로 경계를 조정하는 방식으로 메모리를 관리한다. 동적으로 경계를 조정하면 전체 배열이 찰 때까지 사용할 수 있지만 확장 과정에서 스택을 이동시키기 위한 cost가 발생한다.

스택을 링크드 리스트로 구현하지 않는 이유는 포인터를 추가적으로 저장하는데에 사용되는 오버헤드를 줄이기 위해서이다.

큐의 경우 방향성이 2가지이기 때문에 단일 array로는 더 관리하기가 어렵다. 그래서 보통 링크드 리스트로 각각 구현하거나 전체 메모리 풀 관리용 allocator를 사용한다고 한다.
