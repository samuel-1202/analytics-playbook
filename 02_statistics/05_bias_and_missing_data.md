# Bias and Missing Data

## 개요

데이터 분석 결과가 왜곡되는 주요 원인은 Bias(편향)와 Missing Data(데이터 누락)이다.

충분한 표본 수와 적절한 통계 검정을 수행하더라도 데이터 수집 과정에서 편향이 존재하면 잘못된 결론에 도달할 수 있다.

---

# 1. Bias vs Imbalance

## Bias

데이터 수집 또는 분석 과정에서 특정 방향으로 결과가 왜곡되는 현상

예시

```text
활동성이 높은 사용자만 분석
```

↓

실제 효과 과대 추정

---

## Imbalance

실험군과 대조군의 특성이 균형을 이루지 못한 상태

예시

```text
Treatment
20대 비율 70%

Control
20대 비율 30%
```

---

### 핵심

```text
Bias
= 대표성 문제

Imbalance
= 균형 문제
```

---

# 2. Selection Bias

## 정의

특정 특성을 가진 사용자만 분석에 포함되어 결과가 모집단을 대표하지 못하는 현상

---

## 예시

```text
헤비 유저만 대상으로 실험
```

↓

실제 효과 과대 추정

---

## 진단 방법

- 모집단 vs 분석 대상 비교
- Baseline Parity 확인
- AA Test 수행

---

## 완화 방법

- Random Sampling
- Stratification
- Reweighting
- Propensity Score Matching (PSM)

---

# 3. Survivorship Bias

## 정의

생존한 사례만 분석하여 결과를 과대평가하는 현상

---

## 예시

```text
잔존 사용자만 분석
```

↓

서비스 효과 과대평가

---

## 진단 방법

- 이탈자 추적
- Cohort 분석
- 이탈률 확인

---

## 완화 방법

- ITT (Intent-to-Treat) 분석
- Survival Analysis
- Cohort Tracking

---

# 4. Simpson's Paradox

## 정의

전체 데이터에서는 A가 좋아 보이지만,

세그먼트별로 보면 B가 더 좋은 현상

---

## 예시

```text
전체 CTR

A > B

하지만

20대
A < B

30대
A < B
```

---

## 원인

- 세그먼트 구성 차이
- Confounding Variable

---

## 완화 방법

- Stratified Analysis
- Segment Analysis
- Randomization

---

# 5. Missing Data

## 개요

누락 데이터는 발생 원인에 따라 해석과 대응 방법이 달라진다.

---

## MCAR (Missing Completely At Random)

### 정의

누락이 완전히 무작위로 발생

---

### 예시

```text
서버 오류로 일부 로그 손실
```

---

### 특징

```text
누락 여부와
어떤 변수도 관련 없음
```

---

### 대응

- Complete Case Analysis
- 단순 제거 가능

---

## MAR (Missing At Random)

### 정의

누락이 관측된 변수로 설명 가능한 경우

---

### 예시

```text
고령층 사용자가
설문 응답을 덜 하는 경우
```

---

### 특징

```text
누락 자체는 결과와 관련 없지만

관측된 변수와 관련 있음
```

---

### 대응

- Multiple Imputation
- Regression Imputation

---

## MNAR (Missing Not At Random)

### 정의

누락 자체가 결과와 직접 관련된 경우

---

### 예시

```text
불만족 사용자가
설문에 응답하지 않음
```

---

### 특징

```text
가장 위험한 누락 유형
```

---

### 대응

- Sensitivity Analysis
- Missing Indicator
- 데이터 수집 개선

---

# 6. Sensitivity Analysis

## 정의

가정을 변경했을 때 결과가 얼마나 달라지는지 확인하는 방법

---

## 목적

분석 결과의 견고성(Robustness) 검증

---

## 예시

### Missing Data

```text
MAR 가정

vs

MNAR 가정
```

---

### Outlier

```text
포함

vs

제외
```

---

### Attribution Window

```text
7일

vs

14일

vs

30일
```

---

## 해석

```text
결과 동일

→ Robust
```

```text
결과 크게 변화

→ 결과 해석 주의
```

---

# Bias 완화 기법

| 기법 | 목적 |
|--------|--------|
| Randomization | 선택편향 제거 |
| Stratification | 균형 확보 |
| Blocking | Nuisance 변수 통제 |
| Reweighting | 모집단 보정 |
| PSM | 관측 변수 보정 |
| CUPED | 분산 감소 |
| Sensitivity Analysis | 견고성 검증 |

---

# Frequently Confused Concepts

## Bias vs Variance

| 항목 | 의미 |
|--------|--------|
| Bias | 결과가 특정 방향으로 치우침 |
| Variance | 결과의 변동성이 큼 |

---

## Selection Bias vs SRM

### Selection Bias

```text
대표성 문제
```

예)

```text
헤비 유저만 분석
```

---

### SRM

```text
실험 배정 문제
```

예)

```text
50:50 배정 계획

실제
60:40 배정
```

---

## Missing Data 제거하면 해결될까?

❌ 항상 그렇지 않다.

| 유형 | 제거 가능 여부 |
|--------|--------|
| MCAR | 가능 |
| MAR | 주의 필요 |
| MNAR | 제거 시 추가 편향 위험 |

---

## Simpson's Paradox는 왜 발생할까?

주요 원인

```text
Confounding Variable
```

즉, 숨겨진 세그먼트 효과 때문이다.

---

## Sensitivity Analysis는 언제 사용할까?

- Missing Data 분석
- 준실험 (PSM, DID)
- Outlier 처리
- Attribution 분석
- 모델링 결과 검증

목적은

```text
결과가 특정 가정에
지나치게 의존하지 않는지 확인
```

하는 것이다.
