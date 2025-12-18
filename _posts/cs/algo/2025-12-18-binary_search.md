---
title: "이분 탐색의 헷갈리는 경계조건 잘 세우는 법"
excerpt: ""
categories: [cs, algo]
tags:
   - algorithm
   - binary search
show_date: true
---

## 요약
1. 조건을 잘 세워서 COND(lo) != COND(hi)가 되도록 IC 설정 (T/F or F/T 되도록)
2. while lo + 1 < high (항상 끝났을 때 lo + 1 = hi를 만족하게됨)
3. lo, hi는 1 차이나며 COND값이 바뀌는 경계가 된다. 문제 조건에 따라 둘 중 하나 선택


## 아이디어
이분 탐색은 decision problem에서 답이 TRUE, FALSE로 나뉘는 문제들을 말한다. 보통 1개의 파라미터로 문제를 풀게 된다(lo, mid, hi에 들어가는 그거)

우선 변수 x에 대한 조건을 설정한다.
```
count가 k보다 작거나 같은
var가 n보다 큰
```
그 다음에는 보통 lb, ub를 설정하게 되는데 예시로는

```
조건 A를 만족하는 x의 최솟값
조건 B를 만족하는 x의 최댓값
```
이런 식이다.

이제 lo, hi 인덱스를 잡고 while문을 돌린다. 여기서 COND 값에 따라 lo, hi 중 어떤 값을 mid로 바꿔줄지에 대해서는 맨 처음 IC의 COND(lo), COND(hi)와 비교하여 COND(mid)와 같은 녀석에 그대로 대응해주면 된다. 그렇게 해서 경계조건을 계속 유지시키는 것이 핵심 (1 차이날때까지)


## 복잡도
$O(\log n)$

## 의사코드
```cpp
// first, set condition COND(x)
// do binary search
// while lo + 1 < high:
//    mid = (lo + hi) / 2
//    if(COND(mid) == COND(hi)) hi = mid
//    if(COND(mid) == CONd(lo)) lo = mid
// take appropriate bound(lo or hi)
```
