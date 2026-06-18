# P-value and Confidence Interval

## 개요

가설 검정 결과를 해석할 때 가장 많이 사용하는 지표는 P-value와 Confidence Interval(CI)이다.

실무에서는 단순히 P-value만 확인하는 것이 아니라,

- Effect Size
- Confidence Interval
- MID (Minimum Detectable Impact)
- Business Impact

를 함께 고려하여 의사결정을 수행한다.

---

# 1. P-value

## 정의

귀무가설(H0)이 참이라고 가정했을 때,

현재 관측된 결과 또는 그보다 더 극단적인 결과가 발생할 확률

---

## 예시

```text
Control CTR = 10.0%
Treatment CTR = 10.5%

p-value = 0.03
```

의미

```text
실제로 차이가 없다면

현재 결과 이상이
우연히 발생할 확률 = 3%
```

---

## 의사결정

| 조건 | 해석 |
|--------|--------|
| p < α | H0 기각 |
| p ≥ α | H0 기각 실패 |

일반적으로

```text
α = 0.05
```

사용

---

## 흔한 오해

❌

```text
p-value = H0가 참일 확률
```

❌

```text
p-value = 효과가 있을 확률
```

---

✅

```text
H0가 참이라고 가정할 때

현재 결과 또는
그보다 더 극단적인 결과가
발생할 확률
```

---

# 2. Confidence Interval (신뢰구간)

## 정의

점추정값(Point Estimate)의 불확실성을 나타내는 범위

---

## 공식

```text
95% CI

= Point Estimate ± 1.96 × SE
```

여기서

```text
SE = Standard Error (표준오차)
```

이다.

---

## 예시

```text
CTR Lift = +2%

95% CI

[0.5%, 3.5%]
```

의미

```text
실제 효과가

0.5% ~ 3.5%

사이에 존재할 가능성이 높음
```

---

# 3. 신뢰구간 해석

## Case 1

```text
95% CI

[1%, 4%]
```

0 포함 X

→ 통계적으로 유의

---

## Case 2

```text
95% CI

[-1%, 3%]
```

0 포함

→ 통계적으로 유의하지 않음

### 왜 0이 포함되면 안 되는가?

A/B 테스트에서 효과는 일반적으로

```text
Treatment - Control
```

의 차이로 정의된다.

따라서

```text
효과 = 0
```

은

```text
두 그룹 간 차이가 없음
```

을 의미한다.

만약 신뢰구간에 0이 포함되어 있다면

```text
실제 효과가 없을 가능성
```

이 여전히 존재한다는 의미이다.

즉,

```text
효과가 존재한다
```

라고 결론 내릴 충분한 근거가 없다.

---

## Case 3

```text
95% CI

[0.1%, 0.2%]
```

0 포함 X

→ 통계적으로 유의

하지만

```text
효과 크기 자체는 매우 작음
```

따라서

```text
Statistical Significance
≠
Practical Significance
```

일 수 있다.

---

# 4. CI 폭의 의미

| CI 폭 | 의미 |
|--------|--------|
| 좁음 | 추정 정확도 높음 |
| 넓음 | 추정 불확실성 큼 |

예시

```text
95% CI

[1.9%, 2.1%]
```

↓

매우 정확한 추정

---

```text
95% CI

[-3%, 7%]
```

↓

효과 추정이 매우 불안정

---

# 5. CI 폭을 줄이는 방법

신뢰구간 폭은 표준오차(SE)에 의해 결정된다.

```text
95% CI

= Point Estimate ± 1.96 × SE
```

즉

```text
SE 감소
↓
CI 감소
↓
추정 정확도 증가
```

---

## 5.1 표본 수 증가

표준오차는

```text
SE ∝ 1 / √n
```

관계를 가진다.

따라서

```text
Sample Size 증가
↓
SE 감소
↓
CI 감소
```

---

예시

```text
1,000명 실험
↓

10,000명 실험
```

신뢰구간이 훨씬 좁아짐

---

## 5.2 분산 감소

표준오차는

```text
SE ∝ σ / √n
```

관계를 가진다.

따라서

```text
Variance 감소
↓
SE 감소
↓
CI 감소
```

---

### 대표적인 Variance Reduction 기법

| 기법 | 설명 |
|--------|--------|
| CUPED | 실험 전 데이터를 활용하여 분산 감소 |
| Stratification | 핵심 변수 기준 층화 |
| Blocking | 유사 단위끼리 그룹화 |
| Covariate Adjustment | 공변량 보정 |
| Reweighting | 표본 불균형 보정 |

---

실무에서는 단순히 트래픽을 늘리는 것보다

```text
Variance Reduction
```

기법을 활용하여

동일한 트래픽으로 더 좁은 신뢰구간을 확보하는 경우가 많다.

---

# 6. P-value vs Confidence Interval

| 항목 | P-value | Confidence Interval |
|--------|--------|--------|
| 목적 | 유의성 판단 | 효과 크기 추정 |
| 제공 정보 | 유의 여부 | 효과 범위 |
| 해석 | H0 기각 여부 | 효과의 불확실성 |
| 한계 | 효과 크기 설명 불가 | 없음 |

---

실무에서는

```text
P-value
+
Confidence Interval
+
Effect Size
```

를 함께 확인해야 한다.

---

# 7. Statistical vs Practical Significance

## Statistical Significance

```text
p-value < 0.05
```

차이가 우연일 가능성이 낮음

---

## Practical Significance

```text
Effect Size > MID
```

비즈니스적으로 의미 있는 효과

---

## 판단 매트릭스

| Statistical | Practical | 해석 |
|--------|--------|--------|
| O | O | 적용 |
| O | X | 효과는 있으나 실무 의미 부족 |
| X | O | 추가 검증 필요 |
| X | X | 효과 없음 |

---

## 예시

```text
CTR

10.00%
→
10.02%

p-value < 0.01
```

통계적으로는 유의

하지만

```text
실무적으로 의미 없는 수준
```

일 수 있다.

---

# 8. 실무 해석 순서

실무에서는 다음 순서로 결과를 해석하는 것이 일반적이다.

```text
1. Effect Size 확인
        ↓
2. Confidence Interval 확인
        ↓
3. P-value 확인
        ↓
4. MID 비교
        ↓
5. Business Impact 평가
```

---

# 9. 흔한 실수

| 실수 | 문제 |
|--------|--------|
| P-value만 확인 | 효과 크기 무시 |
| CI 확인 안 함 | 불확실성 무시 |
| MID 미정의 | 의사결정 기준 없음 |
| 유의성 = 성공 | 잘못된 결론 |
| Effect Size 미확인 | 실무적 가치 판단 불가 |

---

# Interview Questions

### Q. P-value란?

귀무가설이 참이라고 가정했을 때 현재 결과 또는 그보다 더 극단적인 결과가 발생할 확률이다.

---

### Q. 신뢰구간은 왜 보나요?

효과 크기의 불확실성을 확인하기 위해서이다.

단순히 효과가 있는지뿐만 아니라

```text
얼마나 큰 효과가 있는지
얼마나 정확하게 추정되었는지
```

를 판단할 수 있다.

---

### Q. 왜 CI에 0이 포함되면 유의하지 않나요?

0은

```text
효과 없음
```

을 의미한다.

신뢰구간에 0이 포함되어 있다는 것은

```text
실제 효과가 없을 가능성
```

이 여전히 존재한다는 의미이므로 효과가 있다고 결론 내릴 수 없다.

---

### Q. 신뢰구간을 좁히려면 어떻게 해야 하나요?

대표적으로

1. Sample Size 증가
2. Variance Reduction

방법이 있다.

Variance Reduction 기법으로는

- CUPED
- Stratification
- Blocking
- Covariate Adjustment

등이 사용된다.

---

### Q. p-value는 유의한데 효과가 작으면?

통계적 유의성은 있지만 실무적 유의성은 부족할 수 있다.

따라서 Effect Size와 MID를 함께 확인해야 한다.

