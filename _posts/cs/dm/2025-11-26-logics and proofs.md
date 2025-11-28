---
title: "Chapter 1.The Foundations: Logic and Proofs"
excerpt: "논리학과 명제 관련 내용"
categories: [cs, dm]
tags:
   - discrete math
   - proposition
   - logic

show_date: true
---

# 1.1 Propositional Logic

## 명제(Proposition)
참과 거짓을 판단할 수 있는 문장이나 식을 말한다.


## 자연어와 다른 수학에서의 조건문

P → Q 형식의 hypothesis와 conclusion이 있는 경우 다음의 진리표를 가진다.

| P | Q | P → Q |
|:---:|:---:|:-------:|
| T | T | T     |
| T | F | F     |
| F | T | T     |
| F | F | T     |
{: style="margin-left: auto; margin-right: auto;"}

여기서 참고할 점은 hypothesis가 거짓이라면 conclusion의 결과와 관계없이 명제가 참이라는 점이다.

### 왜?

다음의 명제를 생각해 보자
- 모든 짝수는 2로 나누어떨어진다.

이 명제가 참이라는 것을 알고, 이걸 이렇게 조건부 명제로 바꿀 수 있다.
- 모든 자연수 n에 대해, n이 짝수이면 n은 2로 나누어떨어진다.

가정(n이 짝수)이 거짓(n이 홀수)일 때 명제의 참을 판별할 수 없거나 거짓이라고 하자. 그러면 첫번째 명제의 결과와 달라지는 상황이 발생한다. 정리하면 all, any 등의 quantifier와 함께 사용될 때 문제가 발생하는 것을 막기 위함이라고 할 수 있다.

### 자기지시 역설(self referential paradox)

Q1. Is the assertion “This statement is false” a proposition?

Q2. The nth statement in a list of 100 statements is “Exactly
n of the statements in this list are false.”

&ensp; a. What conclusions can you draw from these statements?

&ensp; b. Answer part (a) if the nth statement is “At least n of the statements in this list are false.”

&ensp; c. Answer part (b) assuming that the list contains 99 statements.

이렇게 문제가 될 수 있는 자기지시에 한정해서는 조심할 필요가 있다.

## 역(Converse), 대우(Contrapositive), 이(Inverse)

P → Q에서 나아가 역, 대우, 이를 통해 좀 더 명제가 명확해지는 경우가 있다.

converse: q → p
contrapositive: ¬q →¬p
inverse: ¬p →¬q

## 필요충분(Bidirectional)

| p | q | p ↔ q |
|---|---|-------|
| T | T | T     |
| T | F | F     |
| F | T | F     |
| F | F | T     |
{: style="margin-left: auto; margin-right: auto;"}

## Precedence of operators

| Operator | Precedence |
|----------|------------|
| ¬        | 1          |
| ∧        | 2          |
| ∨        | 3          |
| →        | 4          |
| ↔        | 5          |
{: style="margin-left: auto; margin-right: auto;"}

## Tautology / Contradiction / Contingency

항진명제 : 항상 참인 명제, p ∨ ¬p, 진리표가 모두 1인 경우

모순명제 : 항상 거짓인 명제, p ∧ ¬p, 진리표가 모두 0인 경우

우연명제 : 그 이외에 나머지

모든 명제에 대해 이 범주는 mutually exclusive하고 exhaustive하다.

# 1.3 Propositional Equivalences

## Logical Equivalence

Bidirectional + Tautology인 경우. 풀어서 쓰면 두 명제가 항상 동시에 참이거나 동시에 거짓인 경우를 말한다. 기호는 p ≡ q.

### 어떤 의미를 지니는가?

진리표 노가다 하지 않고도 Logical equivalence를 통해 복잡한 명제를 단순화할 수 있다.

### 예시
- 드모르간 법칙
- p → q and ¬p ∨ q

### Logical Equivalences Table

| Equivalence | Name |
|-------------|------|
| p ∧ T ≡ p<br>p ∨ F ≡ p | Identity laws |
| p ∨ T ≡ T<br>p ∧ F ≡ F | Domination laws |
| p ∨ p ≡ p<br>p ∧ p ≡ p | Idempotent laws |
| ¬(¬p) ≡ p | Double negation law |
| p ∨ q ≡ q ∨ p<br>p ∧ q ≡ q ∧ p | Commutative laws |
| (p ∨ q) ∨ r ≡ p ∨ (q ∨ r)<br>(p ∧ q) ∧ r ≡ p ∧ (q ∧ r) | Associative laws |
| p ∨ (q ∧ r) ≡ (p ∨ q) ∧ (p ∨ r)<br>p ∧ (q ∨ r) ≡ (p ∧ q) ∨ (p ∧ r) | Distributive laws |
| ¬(p ∧ q) ≡ ¬p ∨ ¬q<br>¬(p ∨ q) ≡ ¬p ∧ ¬q | De Morgan's laws |
| p ∨ (p ∧ q) ≡ p<br>p ∧ (p ∨ q) ≡ p | Absorption laws |
| p ∨ ¬p ≡ T<br>p ∧ ¬p ≡ F | Negation laws |
{: style="margin-left: auto; margin-right: auto;"}

### Logical Equivalences Involving Conditional Statements

| Equivalence |
|-------------|
| p → q ≡ ¬p ∨ q |
| p → q ≡ ¬q → ¬p |
| p ∨ q ≡ ¬p → q |
| p ∧ q ≡ ¬(p → ¬q) |
| ¬(p → q) ≡ p ∧ ¬q |
| (p → q) ∧ (p → r) ≡ p → (q ∧ r) |
| (p → r) ∧ (q → r) ≡ (p ∨ q) → r |
| (p → q) ∨ (p → r) ≡ p → (q ∨ r) |
| (p → r) ∨ (q → r) ≡ (p ∧ q) → r |
{: style="margin-left: auto; margin-right: auto;"}

### Logical Equivalences Involving Biconditional Statements

| Equivalence |
|-------------|
| p ↔ q ≡ (p → q) ∧ (q → p) |
| p ↔ q ≡ ¬p ↔ ¬q |
| p ↔ q ≡ (p ∧ q) ∨ (¬p ∧ ¬q) |
| ¬(p ↔ q) ≡ p ↔ ¬q |
{: style="margin-left: auto; margin-right: auto;"}

## Satisfiable

어떤 진리값이 명제를 참으로 만드는 경우를 말한다.

## Sudoku example

스도쿠에는 세 가지 규칙이 있다.
- 각 행마다 1~9까지의 숫자가 모두 들어간다.
- 각 열마다 1~9까지의 숫자가 모두 들어간다.
- 각 3x3 블록마다 1~9까지의 숫자가 모두 들어간다.

이 규칙을 명제로 표현하려고 한다. 식을 조금 명제로 표현하지 좋게 다듬어보기로 했다. 

- 모든 행에 대해, 임의의 숫자는, 어떤(아무) 열 중 하나에 들어가있다.
- 모든 열에 대해, 임의의 숫자는, 어떤(아무) 행 중 하나에 들어가있다.
- 모든 블록에 대해, 임의의 숫자는 어떤 블록의 영역에 들어가 있다.

모든(임의)을 교집합, 어떤을 합집합으로 생각할 수 있다. 이제 기본 명제 p(i, j, n) : (i, j) 칸에 숫자 n이 들어가있다. 로 명제식을 구성할 수 있다.

$$\bigwedge_{n=1}^{9} \bigwedge_{i=1}^{9} \bigvee_{j=1}^{9} p(i, j, n)$$

$$\bigwedge_{n=1}^{9} \bigwedge_{j=1}^{9} \bigvee_{i=1}^{9} p(i, j, n)$$

$$\bigwedge_{r=0}^{2} \bigwedge_{s=0}^{2} \bigwedge_{n=1}^{9} \bigvee_{i=1}^{3} \bigvee_{j=1}^{3} p(3r + i, 3s + j, n)$$

이 세 조건 외에도 한 가지를 더 고려해야 하는데 특정 칸에 숫자가 들어가 있으면 이 이외에 다른 숫자는 들어가 있으면 안된다.

$$
p(i, j, n) → ¬p(i, j, n'),\quad 1\leq n,n'\leq 9, n \neq n'
$$

## Functional Completeness

### disjunctive normal form

진리표에 대해 T인 것들을 모두 AND 처리하면 다음과 같은 형태가 나온다.

$$
(p ∧ q) ∨ (¬p ∧ r) ∨ ... ∨ (q ∧ ¬r)
$$

이것을 disjunctive normal form이라 한다.

### Functional Completeness

한편 모든 진리표를 표현할 수 있는 연산자 집합을 functional completeness 라고한다.
- {∧, ∨, ¬}은 functional complete하다.
- {→, ¬}도 드모르간을 이용하면 functional complete하다.
- {↑} 하나만으로도 모든 진리표를 표현할 수 있다.
- {↓} 하나만으로도 모든 진리표를 표현할 수 있다.

### 어디에 이 개념이 쓰일까?
- 회로 설계
- 소프트웨어 검증
- AI / 자동 추론
- DB 쿼리 최적화
- SAT Solver(컴파일러 최적화, 하드웨어 검증, 스케줄링 문제, 암호 해독)

## Dual proposition
명제의 dual을 만드는 방법은 다음과 같다.
- ∧를 ∨로 바꾸고
- ∨를 ∧로 바꾸고
- T(참)를 F(거짓)으로 바꾸고
- F(거짓)를 T(참)으로 바꿔요
- ¬는 그대로 둬요

이 형태는 진리값 할당(TF)을 뒤집는 것과 dual을 만드는 것이 서로 상쇄시킴을 확인할 수 있다.

### 예시
- 드모르간 변환식
- equivalent의 dual도 equivalent하다
<details markdown="1">
<summary>pf)</summary>

어떤 명제 p와 진리값 할당 α가 있을 때:

$$p(α) = ¬p^*(ᾱ)$$

여기서 ᾱ는 α를 뒤집은 할당이에요 (모든 변수의 진리값을 반대로).

이제 원래 문제로:

p ≡ q라고 하면, 모든 α에 대해 p(α) = q(α)죠.

그럼:

$$p^*(α) = ¬p(ᾱ)$$

$$q^*(α) = ¬q(ᾱ)$$

p(ᾱ) = q(ᾱ)이므로 (p ≡ q니까 모든 할당에서 같아요, ᾱ도 포함)

$$¬p(ᾱ) = ¬q(ᾱ)$$

따라서 $$p^*(α) = q^*(α)$$

모든 α에 대해 성립하므로 $$p^* ≡ q^*$$!

</details>


### 왜 나온 개념임?
만약 dual이 더 쉬운 형태이면 더 쉽게 해결할 수 있고, 대칭적인 상황에서 절반의 값을 바로 얻을 수 있다.

## 변수와 진리표

N개의 변수에 대해 진리표의 가짓수는 $2^{N^N}$. 따라서 유한한 변수에 대해 만들 수 있는 boolean function도 유한하다(AND, OR, NOT 등의 연산자를 통해). 하지만 매우 빠른 기울기로 증가하기 때문에 효율적인 설계가 중요함을 알 수 있다.

# 1.4 Predicates and Quantifiers

## 술어논리(Predicate logic)와 명제함수(propositional function)

앞서 본 명제는 참/거짓이 확정된 문장이다. 하지만 다음 명제의 경우

$$x > 3$$

변수의 값에 따라 참/거짓이 나뉜다.

여기서
$$P = “> 3”$$
이라는 속성을 나타내는 predicate이고,
$$x = variable$$
이다. $P(x)$는 propositional function이라 한다.

## 입력조건(Preconditions), 출력조건(Postconditions)

프로그램의 정확성을 표현하는 방법으로 predicate를 이용하여 표현할 수 있다. 

Precondtion이란 프로그램 실행 전 입력이 만족해야 하는 조건을 말한다. 가령 프로그램이 “정수 x를 입력받아 결과 y = x² 를 출력한다.”라고 하자.

Precondition은

$$P(x):x∈Z$$

Postcondition은

$$Q(x,y):y=x^2$$

따라서 프로그램의 작동을 predicate로 표현할 수 있다.

## 양화사(Qunatifiers)

Predicate는 어떤 값이 정해져야만 참/거짓을 판별할 수 있다. 따라서 이 범위에 대한 논의가 필요한데 이를 위해 제시되는 개념이 quantifier이다.


### universal quantifier / existential quantifier / uniqueness quntifier

Universal quantifier는 임의의, 모든을 의미하는 $∀$를 말한다. Counterexample을 통해 거짓임을 보일 수 있다.

Existential quantifier는 어떤, 존재함을 의미하는 $∃$을 말한다. 참이 되는 example을 통해 참임을 보일 수 있다.

Uniqueness quantifier는 유일하게 존재함을 의미하는 $∃!$ or $∃_1$를 말한다.

| Statement | When True? | When False? |
|:---------:|:-----------|:------------|
| ∀xP(x) | P(x) is true for every x. | There is an x for which P(x) is false. |
| ∃xP(x) | There is an x for which P(x) is true. | P(x) is false for every x. |
{: style="margin-left: auto; margin-right: auto;"}

### Logical equivalence involving quantifiers

마찬가지로 quantifiers가 포함된 명제도 logical equivalnce를 정의할 수 있다.

### Qunatified expression의 부정

Universal, existential 간 도치가 일어난다. (드모르간)

$$
¬∀xP(x) ≡ ∃x ¬P(x)\\
¬∃xQ(x) ≡ ∀x ¬Q(x)
$$

### 주의할 부분

Every student in this class has studied calculus

$∀x(S(x) → C(x))$ 이렇게 표현되며, $∀x(S(x) ∧
C(x))$이 아님에 주의한다. x의 도메인이 사람이라고 하자. 만약 x에 학생인데 미적분을 공부한 사람과 학생이 아닌 사람이 동시에 있다고 하자. 그러면 후자는 모든 사람은 학생이면서 미적분을 공부했다라는 의미가 된다. 도메인이지만 관심영역(S)에 속하지 않는 요소들을 배제하기 위해 directional을 이용한다고 생각하면 좋을 것 같다.

Some student in this class has visited Mexico

$∃x(S(x) ∧ M(x))$ 이렇게 표현되며, $∃x(S(x) → M(x))$이 아님에 주의한다. x의 도메인이 사람이라고 하자. 만약 x에 학생이 아닌 사람과, 학생인데 멕시코에 가지 않은 사람밖에 없다고 하자. 그러면 식은 거짓이어야 하는데 후자는 참값을 뱉는다. 존재성을 보이는 것은 참인 명제를 하나만 보이면 되다 보니 도메인을 제한하는 S가 타이트하게 들어갔다고 해석하면 좋을 것 같다.

다르게 정리하면, '가정이 거짓이면 명제는 참'이라는 속성에 기반하여, '모든 ... 명제'의 형태에서는 가정이 거짓인 경우를 T로 받아야 하는 상황, '어떤 ... 명제'의 형태에서는 가정이 거짓인 경우를 제외해야 하는 상황에 각기 적용된다고 보면 될 것 같다.

## Nested Quantifiers

드모르간 법칙을 순차적으로 이용한다.

### Convergence

$$
∀\epsilon>0, ∃δ>0, ∀x(0 < |x − a| < δ → |f (x) − L| < \epsilon)
$$

수렴의 부정은 다음과 같이 드모르간 법칙을 이용하여 나타낼 수 있다.

$$
\begin{align*}
&¬∀\epsilon > 0 \exists\delta > 0 \forall x(0 < |x - a| < \delta \to |f(x) - L| < \epsilon) \\
&\equiv ∃\epsilon > 0 ¬∃\delta > 0 \forall x(0 < |x - a| < \delta \to |f(x) - L| < \epsilon) \\
&\equiv ∃\epsilon > 0 \forall\delta > 0 ¬\forall x(0 < |x - a| < \delta \to |f(x) - L| < \epsilon) \\
&\equiv ∃\epsilon > 0 \forall\delta > 0 ∃x ¬(0 < |x - a| < \delta \to |f(x) - L| < \epsilon) \\
&\equiv ∃\epsilon > 0 \forall\delta > 0 ∃x(0 < |x - a| < \delta \land |f(x) - L| \geq \epsilon).
\end{align*}
$$

## Rules of Inference

### Argument, premise, conclusion, valid

명제들의 나열을 argument라 한다. 여기서 맨 마지막을 제외한 앞부분을 premises라 하고, 마지막을 conclusion이라 한다. Argument form이 valid란 것은 전제가 모두 참이면 결론도 참인 경우이다. 다르게 표현하면

$$
(p_1 ∧ p_2 ∧ · · · ∧ p_n) → q
$$

가 tautology일 때를 의미한다.

### Rules of inference

valid임을 보이기 위해 모든 변수에 대해 진리표를 만들면 되지만 rules of inference를 통해 빠르게 이를 확인할 수 있다. premises를 잘 조합하여 rules of inference의 chain으로 conclusion을 이끌어 내면 argument 또한 tautology가 되어 valid함을 보일 수 있다.

| Rule of Inference | Tautology | Name |
|:-----------------|:----------|:-----|
| $p$<br>$p \to q$<br>$\therefore q$ | $(p \land (p \to q)) \to q$ | Modus ponens |
| $\neg q$<br>$p \to q$<br>$\therefore \neg p$ | $(\neg q \land (p \to q)) \to \neg p$ | Modus tollens |
| $p \to q$<br>$q \to r$<br>$\therefore p \to r$ | $((p \to q) \land (q \to r)) \to (p \to r)$ | Hypothetical syllogism |
| $p \lor q$<br>$\neg p$<br>$\therefore q$ | $((p \lor q) \land \neg p) \to q$ | Disjunctive syllogism |
| $p$<br>$\therefore p \lor q$ | $p \to (p \lor q)$ | Addition |
| $p \land q$<br>$\therefore p$ | $(p \land q) \to p$ | Simplification |
| $p$<br>$q$<br>$\therefore p \land q$ | $((p) \land (q)) \to (p \land q)$ | Conjunction |
| $p \lor q$<br>$\neg p \lor r$<br>$\therefore q \lor r$ | $((p \lor q) \land (\neg p \lor r)) \to (q \lor r)$ | Resolution |
{: style="margin-left: auto; margin-right: auto;"}


### Resolution이 프로그래밍에서 중요한 이유

resolution의 경우 계산 대상이 cluase(literal들의 disjunction)의 형태를 띄고 있다. 프로그램의 입장에서 complementary($p,¬p$)의 set만 찾고 제거하면 된다.

### Fallacies

### Instantiation, generalization

## Introduction to Proofs

### 용어

theorem: 참임을 보일 수 있는 statement
propositions(=facts, results): 좀 덜 중요한 theorem
proof: theorem이 참임을 보이는 과정
axioms(postulates): 공리, 참이라고 assume하는 것들
lemma: proof하는데 도움을 주는 덜 중요한 theorem
corollary: theorem의 결과로부터 바로 얻을 수 있는 theorem
conjecture: 추측. true statement일 것이라 어림짐작하는 statement. 이후에 증명 여부에 따라 theorem이 될 수도 안될 수도 있다.

### Proof의 종류

- direct proof: $p → q$에서 $p$가 참임을 가정하고 증명.
- proof by contraposition: $p → q$에서 대우를 증명.
- vacuous proof: $p → q$에서 $p$가 거짓임을 보여 증명.
- trivial proof: $p → q$에서 $q$가 참임을 보여 증명.
- proof by contradiction
   - $p$가 참임을 보일 때, $¬p → (r ∧¬r)$가 참임을 보이면 $¬p$가 거짓이므로 증명.
   - $p → q$가 참임을 보일 때, $(p ∧¬q) → F$가 equivalent하기 때문에 이를 이용하여 증명.
- proof of equivalence: $(p ↔ q) ↔ (p → q) ∧ (q → p)$
- counterexample: $∀xP(x)$이 거짓임을 보일 때, $P(x)=F$인 $x$를 찾아주면 된다.

### 도움되는 것들
- exhaustive proof: $[(p_1 ∨ p_2 ∨ · · · ∨ p_n) → q] ↔ [(p_1 → q) ∧ (p_2 → q) ∧ · · · ∧ (p_n → q)]$ 가 tautology임을 이용하여 케이스별로 확인
- existance proof: $∃xP(x)$를 보이는 방법
   - constructive: 실제 만족하는 $x$를 찾기
   - nonconstructive: proof by contradiction으로 $∀x¬P(x)$를 보이기. 실제 만족하는 $x$를 찾을 필요가 없다!
- uniqueness proof: $∃x(P(x) ∧ ∀y(y \neq x →¬P(y)))$