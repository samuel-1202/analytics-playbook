# Quasi Experiments

## 개요

A/B 테스트(RCT, Randomized Controlled Trial)는 인과 효과를 추정하는 가장 강력한 방법이다.

하지만 실제 서비스 환경에서는 다음과 같은 이유로 랜덤 실험이 불가능한 경우가 많다.

- 이미 전체 사용자에게 기능이 배포된 경우
- 정책 또는 법적 제약이 있는 경우
- 특정 지역 또는 사용자만 대상으로 기능이 적용된 경우
- 과거 데이터를 활용해 효과를 추정해야 하는 경우

이러한 상황에서는 준실험(Quasi Experiment) 방법론을 사용하여 인과 효과를 추정한다.

---

# RCT vs Quasi Experiment

| 구분 | RCT | Quasi Experiment |
|--------|--------|--------|
| 랜덤화 | 있음 | 없음 |
| 인과 추론 | 강함 | 상대적으로 약함 |
| 편향 위험 | 낮음 | 높음 |
| 가정 의존성 | 낮음 | 높음 |
| 대표 방법 | A/B Test | PSM, DID, ITS |

---

# 1. Propensity Score Matching (PSM)

## 정의

Treatment를 받을 확률(Propensity Score)이 유사한 사용자끼리 매칭하여 효과를 추정하는 방법

---

## 핵심 아이디어

관측 데이터에서

```text
Treatment User
↓
성향 점수 계산
↓
유사한 Control User 매칭
↓
효과 비교
```

---

## 예시

프로모션 노출 여부

```text
Treatment
- 여성
- 30대
- 구매력 높음

↓

유사한 Control 매칭
```

---

## 절차

### 1. Propensity Score 계산

일반적으로 Logistic Regression 사용

예시 변수

- 성별
- 연령
- 구매 이력
- 방문 빈도

---

### 2. Matching

방법

- Nearest Neighbor Matching
- Caliper Matching
- Radius Matching

---

### 3. 효과 추정

```text
ATE
ATT
ATC
```

---

## 가정

### Ignorability

모든 교란변수가 관측되어 있어야 함

즉,

```text
Unobserved Confounder 없음
```

---

## 한계

숨겨진 변수는 통제 불가능

예시

```text
구매 의도
관심도
동기
```

---

## 언제 사용하는가?

- 비교 그룹 존재
- 랜덤화 불가능
- 사용자 수준 데이터 존재

---

# 2. Difference-in-Differences (DID)

## 정의

Treatment 그룹과 Control 그룹의 전후 변화 차이를 비교하여 순수 효과를 추정하는 방법

---

## 핵심 아이디어

```text
Treatment 변화
-
Control 변화
=
순수 효과
```

---

## 공식

```text
DID

=
(Treatment After - Treatment Before)

-

(Control After - Control Before)
```

---

## 예시

신규 프로모션 적용

### Treatment

```text
Before : 100

After : 150

+50
```

### Control

```text
Before : 80

After : 100

+20
```

### DID

```text
50 - 20

=

+30
```

---

## 가정

### Parallel Trend

처치가 없었다면

Treatment와 Control은 동일한 추세를 가졌어야 함

---

## 검증 방법

### 시각화

처치 전 추세 비교

```text
Treatment
Control

기울기 유사
```

---

### Event Study

처치 전 기간별 효과 확인

---

## 한계

Parallel Trend가 깨지면 결과 왜곡

---

## 언제 사용하는가?

- 전후 데이터 존재
- 비교 그룹 존재
- 정책 효과 분석

---

# 3. Interrupted Time Series (ITS)

## 정의

비교 그룹 없이 시계열 데이터만으로 효과를 추정하는 방법

---

## 핵심 아이디어

처치 이전의 추세를 기반으로

```text
예상값(Counterfactual)
```

을 만든 후

실제값과 비교

---

## 예시

신규 기능 출시

```text
출시 전
↓

상승 추세

↓

예상값 생성

↓

실제값 비교
```

---

## 분석 포인트

### Level Change

즉시 효과

### Trend Change

장기 효과

---

## 가정

처치 시점에

다른 외부 변화가 없어야 함

---

## 한계

동시에 다른 이벤트가 발생하면 효과 분리 어려움

예시

```text
기능 출시

+

대형 프로모션
```

---

## 대표 도구

- CausalImpact
- Bayesian Structural Time Series

---

## 언제 사용하는가?

- 비교 그룹 없음
- 전체 사용자 대상 롤아웃
- 정책 변경 분석

---

# 4. Synthetic Control

## 정의

여러 Control 그룹을 가중합하여

가상의 Control 그룹을 생성하는 방법

---

## 핵심 아이디어

```text
한국
↓
Treatment

일본
대만
싱가포르

↓

가중 평균

↓

Synthetic Korea
```

---

## 예시

정책 효과 분석

```text
한국 정책 시행
```

↓

가상의 한국 생성

↓

효과 비교

---

## 장점

DID보다 유연

---

## 한계

적절한 비교군 필요

---

## 언제 사용하는가?

- 국가 정책
- 지역 정책
- Geo Experiment 분석

---

# 5. 방법론 선택 가이드

| 상황 | 추천 방법 |
|--------|--------|
| 비교 그룹 존재 | PSM |
| 전후 데이터 + 비교 그룹 | DID |
| 전체 롤아웃 | ITS |
| 국가/지역 정책 | Synthetic Control |

---

# 6. Sensitivity Analysis

## 정의

가정이 변경되어도 결과가 유지되는지 확인하는 방법

---

## 왜 필요한가?

준실험은 RCT보다 가정에 크게 의존한다.

따라서

```text
가정이 바뀌어도 결과가 유지되는가?
```

를 검증해야 한다.

---

## DID

### Placebo Test

실제 처치 이전 시점에

가짜 처치를 적용

효과가 없어야 정상

---

### Parallel Trend 변경

추세 가정 변경

결과 비교

---

## PSM

### Hidden Bias Test

관측되지 않은 교란변수 존재 가정

---

### Rosenbaum Bounds

숨은 편향 영향 측정

---

## ITS

### Intervention Date 변경

처치 시점 변경

---

### Window 변경

```text
7일
14일
30일
```

비교

---

## 공통 점검

### Outlier 포함 / 제외

결과 비교

---

### Covariate 추가 / 제거

결과 비교

---

### Attribution Window 변경

결과 비교

---

## 목적

```text
Robust Result
```

즉,

가정이 바뀌어도 결론이 유지되는지 확인하는 것

---

# 준실험 방법론 비교

| 방법 | 핵심 아이디어 | 주요 가정 | 대표 한계 |
|--------|--------|--------|--------|
| PSM | 비슷한 사용자 매칭 | Ignorability | 숨은 교란변수 |
| DID | 전후 변화 비교 | Parallel Trend | 추세 위반 |
| ITS | 시계열 추세 연장 | 외부 변화 없음 | 동시 이벤트 |
| Synthetic Control | 가상 Control 생성 | 적절한 비교군 | 데이터 요구량 |

---

# 실무 체크리스트

## 준실험 수행 전

- [ ] RCT 가능 여부 확인
- [ ] 비교 그룹 존재 여부 확인
- [ ] 처치 전 데이터 존재 여부 확인

## 분석 수행

- [ ] 주요 가정 명시
- [ ] 가정 검증 수행
- [ ] 결과 해석 시 한계 명시

## 결과 검증

- [ ] Sensitivity Analysis 수행
- [ ] Placebo Test 수행
- [ ] 세그먼트별 결과 확인

---

# Interview Questions

### Q. RCT가 불가능하면 어떻게 하시겠습니까?

PSM, DID, ITS, Synthetic Control과 같은 준실험 방법론을 활용합니다.

단, 각 방법의 가정을 명확히 검증하고 결과 해석 시 한계를 함께 설명합니다.

---

### Q. DID의 가장 중요한 가정은 무엇인가요?

Parallel Trend 가정입니다.

처치가 없었다면 Treatment와 Control이 동일한 추세를 가졌어야 합니다.

---

### Q. PSM의 가장 큰 한계는 무엇인가요?

관측되지 않은 교란변수를 통제할 수 없다는 점입니다.

---

### Q. 준실험 결과를 신뢰할 수 있는지 어떻게 검증하나요?

Sensitivity Analysis, Placebo Test, Event Study 등을 통해 결과의 견고성(Robustness)을 검증합니다.
