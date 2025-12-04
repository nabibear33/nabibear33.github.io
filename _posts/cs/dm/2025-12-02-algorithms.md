---
title: "Chapter 3. Algorithms"
excerpt: "Algorithm에 관련된 내용"
categories: [cs, dm]
tags:
   - discrete math
   - algorithm
   - logic

show_date: true
---

# 3.2 The Growth of Functions

## Big-O Notation

> Let f and g be functions from the set of integers or the set of real numbers to the set of real
numbers.We say that $f (x)$ is $O(g(x))$ if there are constants $C$ and $k$ such that
> $$|f (x)| ≤ C|g(x)|$$
> whenever $x > k$. [This is read as “$f (x)$ is big-oh of $g(x)$.”]

- 좀 더 직관적으로 이야기해보면, x를 계속 키우다 어느 순간부터는 항상 상한이 되는 다른 함수가 있는 경우를 말한다.
- 함숫값에 가장 큰 영향을 주는 term에 의해 좌우된다.
- 대응되는 위치로, 하한의 경우 big-omega가 있다.
- Big-theta의 경우, big-omega, big-o를 동시에 만족시키는 경우로 복잡도의 정확한 차수를 의미한다.
- 하지만 실무에서는 번거로움 등의 이유로 big-o를 많이 씀.

# 3.3 Complexity of Algorithms

## 자주 언급되는 complexity

| Complexity | Terminology |
|:---:|:---:|
| $\Theta(1)$ | Constant complexity |
| $\Theta(\log n)$ | Logarithmic complexity |
| $\Theta(n)$ | Linear complexity |
| $\Theta(n \log n)$ | Linearithmic complexity |
| $\Theta(n^b)$ | Polynomial complexity |
| $\Theta(b^n)$, where $b > 1$ | Exponential complexity |
| $\Theta(n!)$ | Factorial complexity |
{: style="margin-left: auto; margin-right: auto;"}

- 이 중 $b^n$, $n!$은 컴퓨터 연산으로도 n의 크기가 100만 넘어가도 불가능할 정도의 시간이 소요된다.