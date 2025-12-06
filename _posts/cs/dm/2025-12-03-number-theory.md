---
title: "Chapter 4. Number Theory and Cryptography"
excerpt: ""
categories: [cs, dm]
tags:
   - discrete math
   - mod

show_date: true
---

# 4.1 Divisibility and Modular Arithmetic

## Division algorithm

- 어떤 정수를 특정 값으로 나누었을 때 몫과 나머지를 구성하는 unique한 pair가 존재한다.
- 여기서 a는 dividend, q는 quotient, r은 remainder라고 한다.
$$
a = dq + r\\
q = a \text{ div } d\\
 r = a \text{ mod } d
$$

## Modular Arithmetic
- congruent(합동): 나머지가 같은 경우를 말한다. ex) $17≡2(mod\; 5)$
- modulus(법): 나눗셈의 기준이 되는 수를 말한다. ex)위의 예시에서 5
- 같은 의미로, 두 수의 차이가 modulus의 정수배이면 두 수는 congruent이다.

### 어디에씀?
- 컴퓨터의 나눗셈 연산은 빼기의 반복이므로, 나머지를 어떻게 처리하는가가 중요해지게 된다.

### some theorems
- 합과 곱에 대해서 일관성을 갖는다.
> Let m be a positive integer. If a ≡ b (mod m) and c ≡ d (mod m), then
> $$a + c ≡ b + d (\text{mod } m)\\
> ac ≡ bd (\text{mod } m).$$
- 자신을 modulo 연산한 결과는 자신과 합동이다. 이를 이용하면, 큰 수의 경우 합이나 곱으로 분해하여 각각을 modulo 연산하여 나머지를 구할 수 있다.
> Let m be a positive integer and let a and b be integers. Then
> $$(a + b) \text{mod }m = ((a \text{ mod } m) + (b \text{ mod } m)) modm$$
> and
> $$ab \text{ mod }m = ((a \text{ mod } m)(b \text{ mod } m)) \text{ mod } m.$$

## Arithmetic modulo m
- 나머지에 초점을 맞추어 연산하는 수 체계를 말한다.
- ex) $7+_{11}9$는 7과 9의 11 mod 덧셈으로, 두 수의 합을 11에 대한 나머지로 표현한다.

# 4.2 Integer Representations and Algorithms

## 2진수의 사칙연산
- 덧셈은.. 생략
- 곱셈은 초등학교 곱셈 연습하듯이 각 자릿수에 대해 곱한 값을 적고 이를 모두 더하기

$$
\begin{aligned}
  1\ 1\ 0 \\
\times\ 1\ 0\ 1 \\
\hline
  1\ 1\ 0 \\
  0\ 0\ 0 \ \ \ \\
  1\ 1\ 0 \ \ \ \ \ \  \\
\hline
  1\ 1\ 1\ 1\ 0
\end{aligned}
$$
- 곱셈의 complexity는 $O(n^2)$이다. 각 자릿수에 대해 n번 연산이 이루어지고 그것을 또 다시 n번 반복한다. 그리고 이와 별개로 각 자릿수를 모두 더한다(n번).
- 나눗셈의 경우 빼기를 수행하여 나머지가 나올 때 까지 반복한다. 하지만 이 방법은 $O(q \log a)$로 몫이 커지면 너무 비싸다.
- 따라서 곱셈과 비슷한 방식으로 계산하면 complexity는 $O(n^2)$이다. 가장 높은 자릿수부터하여 n번 곱하고 1번 빼고, 이 과정을 다시 n번 반복한다. (초등학교 나눗셈 하는 방식과 동일)

## Modular Exponentiation
- 아주 큰 지수를 포함한 숫자의 modulo 연산할 때
- 핵심 아이디어: 모듈러 곱, $(b^a)^2 \text{ mod } m = (b^a \text{ mod } m)(b^a \text{ mod } m)$. 이를 통해 지수적(^2)으로 커지는 지수(a)에 대해서 효과적으로 연산할 수 있다.
```
n은 a_i로 이루어진 이진수 표현
x = 1
power = b mod m (예시에서 b=3, m=645)
작은 digit부터 시작하여 반복
   만약 a_i가 1이면 x = (x*power) mod m (실제 모듈러 계산)
   power = (power*power) mod m (실제 계산과는 별개로 다음 자릿수에 대해 모듈러 곱 계산)
```

- ex) $3^{644} \text{ mod } 645$
$$
3^{(1010000100)_2} \text{ mod } 645\\
\Rightarrow 3^{512\cdot 128 \cdot 4} \text{ mod } 645\\
\Rightarrow (3^{512} \text{ mod } 645)\cdot (3^{128} \text{ mod } 645) \cdot (3^{4} \text{ mod } 645)\\
\Rightarrow (3^{512} \text{ mod } 645)\cdot (3^{128} \text{ mod } 645) \cdot 81
$$
- 
- complexity는 $O((\log m)^2 \log n)$이다. 지수의 n비트에 대해 각각을 돌고, 모듈러 곱을 사용하기 때문에(x * power 혹은 power * power에서 곱셈 연산에 m, 그리고 이어지는 나눗셈 연산에서 m) 저렇게 나온다.

# 4.3 Primes and Greatest Common Divisors

## 소수 구하는 데 쓰이는 몇 가지 성질
- n이 소수가 아니면, $\sqrt{n}$보다 작거나 같은 소수인 약수가 존재한다. → 계산량 절약
- 에라토스테네스의 체: 2부터 시작하여 테이블에서 배수를 지워나가는 식으로 소수를 구할 수 있다.

## 이외의 특징들
- 1보다 큰 정수는 소수이거나, 둘 이상 소수들의 곱으로 이루어진다.
- 메르센 소수: $2^p-1$의 형태인 소수로, 큰 소수를 만들어내기 용이한 구조를 가진다. 다만 모든 $p$에 대해서 소수는 아니다.
- 소수의 비율은 $\frac{x}{\log x}$에 점근한다.

## 흥미로운 추측들
- 골드바흐: 2보다 큰 짝수는 두 소수의 합으로 표현된다.
- 쌍둥이 소수: 두 수의 차이가 2인 소수가 무한히 많이 존재한다.

## GCD, LCM
- relatively prime(서로소) : gcd가 1인 경우
- pairwise relatively prime : 여러 정수 set의 모든 요소끼리 서로소인 경우
## GCD algorithm
- 유클리드 알고리즘
> Let a = bq + r, where a, b, q, and r are integers. Then gcd(a, b) = gcd(b, r).
- 다음을 이용하여 재귀적으로 gcd를 구하는 알고리즘을 설계할 수 있다.

## Bézout’s identity
- gcd는 정수의 linear combination으로 표현할 수 있다.
$$
\gcd(a, b) = sa + tb
$$
- 유클리드 알고리즘을 통해 구한 식을 역방향으로 진행하면 위 식을 구할 수 있다.

## 모듈러도 약분할 수 있을까?

> Let m be a positive integer and let a, b, and c be integers. If ac ≡ bc (mod m) and
gcd(c,m) = 1, then a ≡ b (mod m).

- 모듈러 연산에서 약분을 위해서는 모듈러와 약분하는 수가 서로소 관계여야 함

# 4.4 Solving Congruences

## Linear congruences
$$
ax ≡ b (\text{mod } m)
$$
- 위의 선형 합동식을 풀기 위해 역원의 개념이 필요하다.
- a의 역원이란, $a\bar{a} ≡ 1 (\text{mod } m)$를 만족시키는 $\bar{a}$를 말한다. 이 때, 다음이 성립한다.
> If a and m are relatively prime integers and m > 1, then an inverse of a modulo m exists.
Furthermore, this inverse is unique modulo m. (That is, there is a unique positive integer a
less than m that is an inverse of a modulo m and every other inverse of a modulo m is
congruent to a modulo m.)
- 서로소인 경우(즉, gcd=1인 경우), modulo m의 역원이 유일하게 존재한다.
- Bézout’s identity를 이용하면 sa+tm=1인 s,t를 계산할 수 있어 역원(여기서는 s)를 구할 수 있다.

### 푸는 방법
- ax ≡ b (mod m)
- gcd(a, m)을 구한다.
- b가 gcd로 나누어떨어지는지 구한다. 안나누어떨어지면 해가 없음
- gcd로 식 전체를 나누어 단순화한다.
- 그러면 이제 역원을 구해 곱한다
- 곱의 분배법칙을 이용하여 x ≡ b*x_bar (mod m)의 형태를 만든다.

## Chinese remainder theorem
- x에 대해 n개의 서로소 set $m_1, ..., m_n$이 존재한다고 하자. 이에 대한 (mod m) 값이 주어졌을 때, x는 modulo $m=m_1\cdots m_n$에서 유일한 해를 갖는다.

### 푸는 방법
- 첫번째 식부터 푼다.
- 그러면 일반화된 식 x = m*t + c의 형태
- 이걸 두번째 식에 넣어 t에 대해 푼다.
- 그럼 t의 일반화된 식이 구해진다. x와의 관계식을 통해 x를 또다른 변수에 대해 표현하고 이걸 모든 식에 대해 반복한다.

### 왜 쓰나?
- 큰 수를 작은 모듈러 연산으로 분해 ex) a(아주 큼) mod 105를 mod 3, mod 5, mod 7의 세 가지 식으로 분리할 수 있다. 이렇게 하면 실제 비트 연산 개수가 작아진다.
- 병렬 처리가 가능하여 연산 속도를 높인다.

## 페르마의 소정리

> If p is prime and a is an integer not divisible by p, then
> $$a^{p−1} ≡ 1 (\text{mod } p).$$
> Furthermore, for every integer a we have
> $$a^p ≡ a (\text{mod } p).$$

### 언제씀?
- 아주 큰 power를 가진 수의 mod를 구할 때 쓰인다.
- ex) $7^{222} \text{ mod } 11$
- 역원 계산에도 활용할 수 있다.

## pseudoprime
- 페르마의 소정리는 소수인 것이 충분조건이다. 하지만 페르마의 소정리를 이용하여 소수를 판별하려 하면(역이 참이라 가정하면) 반례가 생긴다.
- 합성수이지만 페르마의 소정리에서 마치 소수인 것 처럼 보인다.
> Let b be a positive integer. If n is a composite positive integer, and $b^{n−1} ≡ 1 (\text{mod } n)$, then
n is called a pseudoprime to the base b.

### Carmichael number
- pseudoprime 중 서로소인 모든 b에 대해 성립하는 경우를 말한다.
- 무한히 많다는 것이 증명되어 있다.
- 하지만 보다 더 강력한 판정 알고리즘을 통해 잘못 판정될 가능성은 close to 0이다.

## Primitive roots / discrete logarithms
- 3의 배수의 1의 자리를 관찰하면, 0~9까지 모든 수가 관찰되는 걸 확인할 수 있다.
- 이처럼 모듈러에서도 소수 p 미만의 정수를 표현할 수 있는 숫자 r(여기서는 배수가 아닌 지수의 mod)들이 있는데 이를 primitive root라고 한다.
- 그러면 1부터 p-1의 수 중에서 아무 a나 골라도 $r^e$ mod p = a인 e가 존재하게 된다.
- 여기서 e를 discrete logarithm of a modulo p to the base r이라고 한다.

### 왜 중요한가?
- 일반적으로 $r^e$ mod p는 구하기 쉽다.
- 하지만 $r^e≡a$ mod p를 만족하는 e를 구하는 것은 매우 어렵다. 모듈러 연산이 $r^e$의 정보를 거의 지워버리고 일반적인 로그함수와는 다르게 모듈러 연산 때문에 $r^e$≡ mod p는 e 값 변화에 따른 어떤 패턴도 보이지 않는다. 따라서 역연산이 매우 어려운 구조
- e를 구하는 다항 함수 알고리즘이 존재하지 않는다.
- 그렇기 때문에 암호학에서 중요한 성질임

# 4.5 Applications of Congruences
## 어떤 경우에 사용될까?
- 순방향은 어렵지 않되, 역방향 해는 구하는 것이 불가능해야함
- 대체로 균일한 분포를 출력함
- 조금의 입력 변화가 큰 출력 변화를 가져오고자 할 때

## Hashing functions

## Pseudorandom Numbers

## Check Digits

# 4.6 Cryptography

## Public Key Cryptography

## RSA Encryption

메시지를 전달하는데 암호화를 하고 싶다. 정수가 나열된 메시지 $M$을 단위마다 자르고 특정 키($n,e$)를 준비한다. 보통 여기서 n=pq은 아주 큰 두 소수의 곱으로 이루어진다(200자리 이상). 그리고 오일러 파이 함수를 이용하여 gcd를 확인한다. 오일러 파이 함수는 n과 서로소인 n 이하의 자연수의 개수이다. 함수 자체의 성질(φ(ab)=φ(a)φ(b))에 의해 $\phi(n)=(p − 1)(q − 1)$을 만족한다. 이제 이 수와 서로소인 e를 선택한다. 보통 소수 65537을 많이 쓰는 듯 하다. 서로소를 판별하는 방법은 유클리드 알고리즘을 쓰면 된다. 그리고 왠만하면 서로소라는 듯하다.
$$
\gcd (e,(p-1)(q-1))=1
$$
이제 다음과 같이 암호화한다.
$$
C=M^e \text{ mod } n
$$
나머지 값들을 다 아니까 C를 구하는 것은 크게 어렵지 않다.

### 왜 오일러 Phi 함수를 사용하는지?

###


## RSA Decryption

앞서 gcd(e, (p − 1)(q − 1)) = 1인 것을 알고 있기 때문에 e modulo (p − 1)(q − 1)의 역원이 존재한다는 사실 또한 알 수 있다. 이 역원을 d라고 하자. 그러면 de ≡ 1 (mod (p − 1)(q − 1))이므로 어떤 정수 k에 대해 de = 1+k(p−1)(q−1)라고 할 수 있다
$$
C^d ≡ (M^e)^d = M^{de} = M^{1+k(p−1)(q−1)} (\text{mod } n).
$$
gcd(M,p)=gcd(M,q)=1이라고 가정하면(p,q가 소수이기 때문에 대부분 참이 된다) 페르마의 소정리에 의해
$$
M^{p−1} ≡ 1 (\text{mod } p) \\
M^{q−1} ≡ 1 (\text{mod } q)
$$
위의 식에 대입하면
$$
C^d≡M(\text{mod } p)\\
C^d≡M(\text{mod } q)
$$
Chinese remainder theorem에 의해
$$
C^d≡M(\text{mod } n)
$$
C, d를 알고 있기 때문에 모듈러 연산과 동일해진다.

## Cryptographic Protocols