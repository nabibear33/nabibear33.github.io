---
title: "Register Variables"
excerpt: "register 변수를 언제 쓰는지"
categories: [lang, c]
tags:
   - c
   - register
show_date: true
---

## 특징
- 지역 변수에만 사용
- 주소 접근 불가
- 빠름
- 현재는 잘 안씀 (컴파일러가 알아서 최적화하여 자주 사용하는 변수는 레지스터로 보냄)

## 왜 썼나?
1970년대 초기 C 컴파일러들은 레지스터 최적화하는 알고리즘도 없었고, 메모리는 KB 수준이었으며, 프로그래머들은 그 당시 컴파일러보다 더 어느 곳에서 컴파일 시간이 많이 들지 꿰고 있었기 때문에 직접 컨트롤하는 것이 가능했다.

하지만 지금 modern 컴파일러들은 이미 register allocation algorithms를 가지고 있고, 당연히 우리보다 잘하겠지... 그래서 현재 개발자들이 ```register``` 키워드를 사용해도 그냥 컴파일러가 무시하는 경우도 많다고 한다.

하지만 comaptibility를 위해 아직 키워드 자체는 남아있다.