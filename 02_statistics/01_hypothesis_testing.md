# Hypothesis Testing (가설 검정)

## 개요

가설 검정(Hypothesis Testing)은 표본 데이터를 이용하여 모집단에 대한 주장을 검증하는 통계적 방법이다.

데이터 분석 및 A/B 테스트에서는 관측된 차이가 우연에 의한 것인지, 실제 효과에 의한 것인지를 판단하기 위해 사용한다.

예를 들어 다음과 같은 질문에 답하기 위해 가설 검정을 수행한다.

- 새로운 추천 알고리즘이 기존 알고리즘보다 더 좋은가?
- 새로운 CRM 캠페인이 전환율을 증가시키는가?
- 광고 노출 방식 변경이 매출에 영향을 주는가?

---

# 1. 가설 검정이란?

## 정의

표본(Sample)을 이용하여 모집단(Population)에 대한 가설(Hypothesis)을 검증하는 과정

---

## 왜 필요한가?

실험 결과에서 관측된 차이는 두 가지 원인으로 발생할 수 있다.

### Case 1. 우연(Random Noise)

```text
CTR

Control : 10.0%
Treatment : 10.2%

차이 = 0.2%p
```

실제 효과가 없어도 우연히 발생 가능

---

### Case 2. 실제 효과(True Effect)

```text
CTR

Control : 10.0%
Treatment : 12.0%

차이 = 2.0%p
```

실험의 영향으로 발생

---

가설 검정은 이 차이가 우연인지 실제 효과인지 판단하는 과정이다.

---

# 2. 기본 개념

## 모집단 (Population)

관심 있는 전체 대상

예시

```text
전체 음악 서비스 사용자
전체 CRM 대상자
전체 광고 노출 사용자
```

---

## 표본 (Sample)

실제로 관측 가능한 데이터

예시

```text
실험에 참여한 사용자
```

---

## Parameter (모수)

모집단의 특성

예시

```text
모집단 평균
모집단 CTR
모집단 전환율
```

---

## Statistic (통계량)

표본으로부터 계산한 값

예시

```text
표본 평균
표본 CTR
표본 전환율
```

---

# 3. 가설 설정

## 귀무가설 (Null Hypothesis, H0)

현재 상태 또는 차이가 없다는 가설

```text
효과 없음
차이 없음
```

---

## 대립가설 (Alternative Hypothesis, H1)

우리가 검증하고 싶은 가설

```text
효과 있음
차이 있음
```

---

## 예시

### CTR 비교

```text
H0 : CTR_A = CTR_B

H1 : CTR_A ≠ CTR_B
```

---

### 전환율 비교

```text
H0 : CVR_A = CVR_B

H1 : CVR_A ≠ CVR_B
```

---

### 매출 비교

```text
H0 : Revenue_A = Revenue_B

H1 : Revenue_A ≠ Revenue_B
```

---

# 4. 가설 검정 프로세스

```text
1. 가설 설정
      ↓
2. 유의수준(α) 설정
      ↓
3. 검정통계량 계산
      ↓
4. p-value 계산
      ↓
5. H0 기각 여부 판단
```

---

## Step 1. 가설 설정

```text
H0 : 효과 없음

H1 : 효과 있음
```

---

## Step 2. 유의수준 설정

보통

```text
α = 0.05
```

사용

---

## Step 3. 검정 통계량 계산

예시

```text
t-statistic
z-statistic
chi-square statistic
```

---

## Step 4. p-value 계산

현재 결과가 우연히 발생할 확률 계산

---

## Step 5. 의사결정

```text
p < α

→ H0 기각
```

---

```text
p ≥ α

→ H0 기각 실패
```

---

# 5. 검정 통계량 (Test Statistic)

## 정의

관측된 차이가 얼마나 큰지를 수치화한 값

---

## 대표 검정 통계량

| 검정 | 통계량 |
|--------|--------|
| Z-test | Z-statistic |
| T-test | T-statistic |
| Chi-square Test | χ² |
| ANOVA | F-statistic |

---

## 직관적 의미

```text
차이
────────
표준오차(SE)
```

값이 클수록

```text
우연히 발생하기 어려움
```

---

# 6. Type I Error / Type II Error

실제 진실과 검정 결과는 다를 수 있다.

| | H0 참 (효과 없음) | H1 참 (효과 있음) |
|----|----|----|
| H0 채택 | True Negative ✅ | False Negative ❌ (Type II Error, β) |
| H0 기각 | False Positive ❌ (Type I Error, α) | True Positive ✅ (Power = 1-β) |

---

## Type I Error (α)

실제로 효과가 없는데 있다고 판단

```text
False Positive
```

예시

```text
광고 효과 없음

↓

효과 있다고 출시
```

---

## Type II Error (β)

실제로 효과가 있는데 없다고 판단

```text
False Negative
```

예시

```text
좋은 기능

↓

효과 없다고 폐기
```

---

## Power

정말 효과가 있을 때 발견할 확률

```text
Power = 1 - β
```

보통

```text
80%
```

이상 사용

---

# 7. One-sided vs Two-sided Test

## Two-sided Test (양측 검정)

양 방향 모두 고려

```text
좋아질 수도 있다
나빠질 수도 있다
```

---

예시

```text
H1 : CTR_A ≠ CTR_B
```

---

실무 대부분의 A/B 테스트

---

## One-sided Test (단측 검정)

한 방향만 관심

```text
좋아지는 것만 관심
```

---

예시

```text
H1 : CTR_B > CTR_A
```

---

장점

```text
Sample Size 감소
Power 증가
```

---

주의

사전에 정의해야 함

실험 결과 보고 변경 금지

---

# 8. Parametric vs Non-parametric Test

## Parametric Test

특정 분포 가정 존재

### 주요 가정

- 정규성
- 등분산성
- 독립성

---

대표 기법

```text
t-test
ANOVA
Linear Regression
```

---

## Non-parametric Test

분포 가정이 적음

---

대표 기법

```text
Mann-Whitney U
Kruskal-Wallis
Wilcoxon Signed Rank
```

---

## 언제 사용하는가?

| 상황 | 추천 |
|--------|--------|
| 정규분포 근사 가능 | Parametric |
| Outlier 많음 | Non-parametric |
| Heavy-tailed | Non-parametric |
| 표본 적음 | Non-parametric 고려 |

---

# 9. 검정 선택 가이드

## 평균 비교

### 두 그룹

```text
t-test
```

예시

```text
평균 매출 비교
평균 스트리밍 수 비교
```

---

### 세 그룹 이상

```text
ANOVA
```

---

## 비율 비교

```text
Z-test
Chi-square Test
```

---

예시

```text
CTR
CVR
Retention
```

---

## 범주형 데이터

```text
Chi-square Test
```

---

예시

```text
성별
디바이스
지역
```

---

## 비모수 데이터

```text
Mann-Whitney U

Kruskal-Wallis
```

---

# 10. 실무 체크리스트

## 검정 전

### 가설 정의

```text
무엇을 검증할 것인가?
```

---

### Metric 정의

```text
Primary Metric

Guardrail Metric

Secondary Metric
```

---

### Assumption 확인

- Independence
- Normality
- Equal Variance

---

## 검정 후

### p-value 확인

---

### Effect Size 확인

---

### Confidence Interval 확인

---

### Business Impact 확인

---

# 11. 흔한 실수

## p-value만 보는 경우

```text
p < 0.05

→ 무조건 성공
```

❌

---

## Effect Size 무시

```text
0.01% 개선

p < 0.05
```

통계적으로 유의하지만 실무적으로 의미 없을 수 있음

---

## Multiple Testing 무시

지표를 많이 볼수록

```text
False Positive 증가
```

---

## Assumption Check 생략

```text
Outlier

Heavy-tailed

Non-independence
```

무시 시 잘못된 결론 가능

---

# 실무 예시

## 음악 서비스 추천 실험

### 가설

```text
H0:
신규 추천 알고리즘과 기존 알고리즘의
스트리밍 수 차이는 없다

H1:
신규 추천 알고리즘이
스트리밍 수를 증가시킨다
```

---

### Primary Metric

```text
스트리밍 수
```

---

### Guardrail Metric

```text
이탈률
```

---

### Secondary Metric

```text
세션 시간
재방문율
```

---

# Interview Questions

### Q. 가설 검정이란 무엇인가?

표본 데이터를 이용해 모집단에 대한 가설을 검증하는 통계적 방법입니다.

---

### Q. Type I Error와 Type II Error 차이는?

Type I Error는 효과가 없는데 있다고 판단하는 오류이며,

Type II Error는 효과가 있는데 없다고 판단하는 오류입니다.

---

### Q. 왜 p-value만 보면 안 되나요?

p-value는 통계적 유의성만 설명하며 효과 크기와 비즈니스 임팩트는 설명하지 못하기 때문입니다.

따라서 Effect Size와 Confidence Interval을 함께 확인해야 합니다.

---

### Q. 언제 Mann-Whitney를 사용하나요?

Outlier가 많거나 Heavy-tailed Distribution으로 인해 t-test 가정이 충족되지 않을 때 사용합니다.
