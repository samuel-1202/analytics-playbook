# Sample Size and Power

## 개요

A/B 테스트에서 신뢰할 수 있는 결과를 얻기 위해서는 충분한 표본 수(Sample Size)가 필요하다.

표본 수가 부족하면 실제 효과가 있어도 발견하지 못할 수 있으며(Type II Error), 반대로 지나치게 크면 매우 작은 효과도 통계적으로 유의하게 나타날 수 있다.

---

# 1. 핵심 개념

| 개념 | 의미 |
|--------|--------|
| MID | 비즈니스적으로 의미 있는 최소 효과 |
| MDE | 통계적으로 탐지하고자 하는 최소 효과 |
| α | Type I Error |
| β | Type II Error |
| Power (=1-β) | 실제 효과를 발견할 확률 |
| Sample Size | 필요한 표본 수 |

---

# 2. 실험 설계 순서

```text
비즈니스 목표 정의
        ↓
MID 정의
        ↓
AA Test로 분산 추정
        ↓
MDE 설정
        ↓
α / Power 설정
        ↓
Sample Size 계산
        ↓
실험 기간 산출
```

---

# 3. MID vs MDE

## MID (Minimum Important Difference)

비즈니스적으로 의미 있는 최소 효과

예시

```text
CTR +0.05%

→ 의미 없음

CTR +1%

→ 의미 있음
```

MID는 통계가 아니라 비즈니스 의사결정이다.

---

## MDE (Minimum Detectable Effect)

실험이 탐지할 수 있는 최소 효과

예시

```text
MID = 1%

MDE = 0.8%
```

일반적으로

```text
MDE ≤ MID
```

로 설정한다.

---

# 4. Sample Size 공식

## 이진 변수

CTR, CVR, Retention 등

```text
N =
(Zα/2 + Zβ)² ×
[p₁(1-p₁)+p₂(1-p₂)]
/
(p₁-p₂)²
```

---

## 연속 변수

매출, 사용시간, 스트리밍 수 등

```text
N =
(Zα/2 + Zβ)² × 2σ²
/
Δ²
```

---

# 5. Sample Size에 영향을 주는 요인

| 요인 | 영향 |
|--------|--------|
| Variance ↑ | N 증가 |
| MDE ↓ | N 급증 |
| α ↓ | N 증가 |
| Power ↑ | N 증가 |
| Allocation 불균형 | N 증가 |

---

## Variance

```text
Variance ↑
↓
SE ↑
↓
N ↑
```

---

## MDE

```text
N ∝ 1 / MDE²
```

예시

```text
MDE 절반
↓
필요 Sample Size 4배
```

---

# 6. Power

## 정의

실제로 효과가 있을 때 이를 발견할 확률

```text
Power = 1 - β
```

---

## 일반적인 설정

| Power | 의미 |
|--------|--------|
| 0.8 | 실무 표준 |
| 0.9 | 중요한 실험 |
| 0.95 | 매우 보수적 |

---

## 해석

```text
Power = 0.8
```

↓

```text
실제 효과가 존재하면

80% 확률로 발견
```

---

# 7. Variance Reduction

표본 수를 늘리는 것 외에도 분산을 줄이면 동일한 Power를 더 적은 트래픽으로 확보할 수 있다.

---

## 대표 기법

| 기법 | 목적 |
|--------|--------|
| CUPED | 실험 전 정보 활용 |
| Stratification | 균형 확보 + 분산 감소 |
| Blocking | Nuisance 변수 제거 |
| Covariate Adjustment | 공변량 보정 |

---

## 직관

```text
Variance 감소
↓
SE 감소
↓
CI 감소
↓
Power 증가
```

---

# 8. 실험 기간 산출

```text
Required Sample Size
/
Daily Traffic
=
실험 기간
```

예시

```text
필요 표본 = 100,000

일일 트래픽 = 10,000

→ 10일
```

---

실무에서는

- 요일 효과
- Novelty Effect
- Learning Effect

등을 고려하여

```text
최소 2주 이상
```

운영하는 경우가 많다.

---

# Frequently Confused Concepts

## MID vs MDE

| 항목 | MID | MDE |
|--------|--------|--------|
| 의미 | 비즈니스 최소 효과 | 통계적 최소 탐지 효과 |
| 결정 주체 | 비즈니스 | 분석가 |
| 목적 | 의사결정 기준 | 실험 설계 기준 |

### 핵심

```text
MID = 사업 기준

MDE = 통계 기준
```

---

## Power가 높을수록 좋은가?

❌ 무조건 좋지 않음

```text
Power ↑
↓
Sample Size ↑
↓
실험 비용 ↑
```

일반적으로

```text
Power = 0.8
```

을 사용한다.

---

## MDE는 작을수록 좋은가?

❌ 무조건 좋지 않음

```text
MDE ↓
↓
Sample Size ↑↑
```

작은 효과를 탐지할수록 비용이 급격히 증가한다.

---

## Sample Size가 크면 좋은가?

❌ 무조건 좋지 않음

```text
0.01%
```

수준의 의미 없는 차이도 유의해질 수 있다.

```text
Statistical Significance
≠
Practical Significance
```

---

## p-value와 Power의 차이

| 항목 | p-value | Power |
|--------|--------|--------|
| 시점 | 실험 후 | 실험 전 |
| 목적 | 결과 해석 | 실험 설계 |

### 핵심

```text
p-value = 결과 해석

Power = 실험 설계
```

---

## Variance Reduction은 왜 중요한가?

표본을 늘리는 것보다 비용 효율적인 경우가 많다.

```text
Variance ↓
↓
SE ↓
↓
CI ↓
↓
Power ↑
```

대표 기법

- CUPED
- Stratification
- Blocking
- Covariate Adjustment

---

## AA Test와 Sample Size의 관계

AA Test는 Sample Size를 계산하는 과정이 아니다.

목적은

```text
분산 추정
+
실험 인프라 검증
```

이다.

AA Test 결과는

```text
Variance
MDE
Sample Size
```

를 현실적으로 설정하는 데 활용된다.

---

## 유의하지 않음 = 효과 없음?

❌ 아니다.

다음 두 경우 모두 가능하다.

1. 실제 효과 없음
2. 실제 효과는 있으나 Sample Size 부족

따라서

```text
p-value
+
Effect Size
+
Confidence Interval
```

을 함께 해석해야 한다.
