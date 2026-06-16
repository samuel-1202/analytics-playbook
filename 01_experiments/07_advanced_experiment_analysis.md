# Advanced Experiment Analysis

## 개요

A/B 테스트에서 사용하는 t-test, 회귀분석 등 대부분의 통계 기법은 다음과 같은 가정을 전제로 한다.

- Independence (독립성)
- Normality (정규성)
- Homogeneity of Variance (등분산성)

하지만 실제 서비스 데이터는 Outlier, Heavy-tailed Distribution, 반복 측정(Repeated Measurement), 사용자 간 상호작용 등으로 인해 이러한 가정이 자주 위반된다.

이 문서에서는 대표적인 가정 위반 사례와 이를 완화하기 위한 분석 기법들을 정리한다.

---

# 1. Non-independence

## 정의

관측치 간 독립성(Independence) 가정이 깨지는 경우

---

## 발생 사례

### 동일 사용자의 반복 측정

```text
User A
├── Session 1
├── Session 2
└── Session 3
```

---

### 동일 광고주의 여러 캠페인

```text
Advertiser A
├── Campaign 1
├── Campaign 2
└── Campaign 3
```

---

### 지역 단위 데이터

```text
서울 사용자
부산 사용자
```

지역 내부 상관관계 존재

---

## 문제점

```text
표준오차(SE) 과소추정
↓
p-value 과소추정
↓
False Positive 증가
```

---

## 해결 방법

### Cluster Robust Standard Error

클러스터 내부 상관관계를 고려하여 표준오차를 보정

---

### Mixed Effect Model

고정효과(Fixed Effect) + 랜덤효과(Random Effect)를 동시에 모델링

---

### GEE (Generalized Estimating Equation)

상관된 데이터를 위한 Population Average 추정

---

# 2. Outlier

## 정의

데이터 분포에서 극단적으로 벗어난 관측치

---

## 예시

```text
매출

99명 → 1만원

1명 → 1억원
```

---

## 문제점

### 평균 왜곡

```text
Mean ↑
```

---

### 분산 증가

```text
Variance ↑
```

---

### Sample Size 증가

```text
N ∝ Variance
```

---

## 탐지 방법

### IQR Rule(Box Plot)

```text
Q1 - 1.5 × IQR

Q3 + 1.5 × IQR
```

범위 밖 데이터 탐지

---

### Z-score

```text
|Z| > 3
```

---

### Modified Z-score

MAD 기반 Robust 방법

---

## 해결 방법

### Winsorization

상·하위 극단값 절단

예시

```text
1% ~ 99%
```

---

### Trimmed Mean

상·하위 일정 비율 제거 후 평균 계산

---

### Log Transform

왜도(Skewness) 완화

---

### Median 사용

평균보다 이상치 영향이 적음

---

# 3. Heavy-tailed Distribution

## 정의

정규분포보다 극단값이 자주 발생하는 분포

---

## 대표 사례

### 매출

```text
대부분 소액
일부 초고액
```

---

### 광고비

---

### 세션 길이

---

### 구매 금액

---

## 문제점

### 평균 불안정

---

### 신뢰구간 증가

---

### 검정력(Power) 감소

---

### Sample Size 증가

---

## 해결 방법

### Log Transformation

가장 일반적인 방법

---

### Bootstrap

분포 가정 없이 신뢰구간 추정

---

### Quantile Analysis

평균 대신 분위수 사용

---

### Robust Statistics

이상치 영향을 줄이는 통계 기법 적용

---

# 4. Robust Statistics

## 정의

Outlier 또는 가정 위반 상황에서 덜 민감한 통계 방법

---

## 중심값

### 기존

```text
Mean
```

---

### Robust

```text
Median
Trimmed Mean
```

---

## 분산

### 기존

```text
Standard Deviation
```

---

### Robust

```text
MAD
IQR
```

---

## 검정

### 기존

```text
T-test
```

---

### Robust

```text
Mann-Whitney U Test
```

---

### 기존

```text
ANOVA
```

---

### Robust

```text
Kruskal-Wallis Test
```

---

## 회귀

### 기존

```text
OLS Regression
```

---

### Robust

```text
Huber Regression
Quantile Regression
```

---

# 5. Bootstrap

## 정의

데이터를 반복 재추출하여 통계량 분포를 추정하는 방법

---

## 왜 필요한가?

다음 상황에서 활용

- 비정규 분포
- Heavy-tailed Distribution
- Median 추정
- Quantile 추정

---

## 절차

```text
원본 데이터

↓

복원추출

↓

1000~10000회 반복

↓

통계량 계산

↓

신뢰구간 생성
```

---

## 장점

- 분포 가정 불필요
- 구현 간단

---

## 단점

- 계산 비용 증가

---

# 6. Cluster Robust Standard Error

## 정의

클러스터 내부 상관관계를 고려하여 표준오차를 보정하는 방법

---

## 목적

```text
계수(β)는 유지

SE만 보정
```

---

## 대표 클러스터

- user_id
- advertiser_id
- region
- school
- store

---

## 언제 사용하는가?

- Repeated Measures
- Marketplace
- 광고 플랫폼
- 지역 단위 실험

---

# 7. Mixed Effect Model

## 정의

고정효과와 랜덤효과를 동시에 고려하는 회귀모형

---

## 예시

```text
CTR

=

광고효과

+

광고주효과

+

오차
```

---

## 장점

클러스터 효과를 직접 모델링 가능

---

## 활용

- Longitudinal Data
- User Repeated Data
- Hierarchical Data

---

# 8. Generalized Estimating Equation (GEE)

## 정의

상관된 데이터를 위한 회귀분석 기법

---

## 특징

Population Average 추정

---

## 비교

| 방법 | 목적 |
|--------|--------|
| Cluster Robust SE | 표준오차 보정 |
| Mixed Effect Model | 개별 효과 모델링 |
| GEE | 평균 효과 추정 |

---

# 9. Multiple Testing Problem

## 정의

동시에 많은 검정을 수행할 경우 False Positive가 증가하는 문제

---

## 예시

```text
100개 검정

α = 0.05
```

↓

```text
평균 5개는 우연히 유의
```

---

# 10. Bonferroni Correction

## 정의

검정 횟수만큼 α를 나누어 보정

---

## 공식

```text
α_adj = α / m
```

---

## 예시

```text
0.05 / 10

=

0.005
```

---

## 장점

간단

---

## 단점

매우 보수적

---

# 11. False Discovery Rate (FDR)

## 정의

False Positive 비율 자체를 제어하는 방법

---

## 대표 방법

### Benjamini-Hochberg

---

## 장점

Bonferroni보다 현실적

---

## 활용

- 추천 실험
- Feature Selection
- 대규모 실험 플랫폼

---

# 12. Sequential Testing

## 정의

실험 진행 중 결과를 반복적으로 확인할 때 발생하는 문제와 해결 방법

---

## 문제점

```text
매일 p-value 확인

↓

유의해 보이면 종료

↓

False Positive 증가
```

---

## 해결 방법

### Group Sequential Test

사전에 확인 시점 정의

```text
25%
50%
75%
100%
```

---

### Alpha Spending

전체 α를 분할 사용

---

## 대표 기법

- Pocock
- O'Brien-Fleming

---

# 분석 기법 선택 가이드

| 상황 | 추천 방법 |
|--------|--------|
| 반복 측정 데이터 | Cluster Robust SE |
| 사용자 효과 모델링 | Mixed Effect Model |
| 평균 효과 추정 | GEE |
| 비정규 분포 | Bootstrap |
| 다중 비교 | FDR |
| 중간 결과 모니터링 | Sequential Testing |
| Heavy-tailed Distribution | Log Transform, Bootstrap |
| Outlier 다수 | Robust Statistics |

---

# 실무 체크리스트

## 데이터 확인

- [ ] Outlier 존재 여부 확인
- [ ] Heavy-tailed Distribution 확인
- [ ] 반복 측정 데이터 존재 여부 확인
- [ ] 클러스터 구조 존재 여부 확인

---

## 분석 설계

- [ ] 독립성 가정 충족 여부 확인
- [ ] Cluster Robust SE 필요 여부 검토
- [ ] Bootstrap 적용 가능 여부 검토
- [ ] Multiple Testing 발생 여부 확인

---

## 결과 해석

- [ ] False Positive 가능성 검토
- [ ] Robustness 확인
- [ ] Sensitivity Analysis 수행
- [ ] 주요 가정 및 한계 명시

---

# Interview Questions

### Q. Cluster Randomization과 Cluster Robust SE의 차이는?

Cluster Randomization은 실험 설계 단계에서 클러스터 단위로 Treatment를 배정하는 방법입니다.

Cluster Robust SE는 분석 단계에서 클러스터 내부 상관관계를 반영하여 표준오차를 보정하는 방법입니다.

---

### Q. Bootstrap은 언제 사용하나요?

비정규 분포, Heavy-tailed Distribution, Median 및 Quantile 기반 분석에서 신뢰구간을 추정할 때 사용합니다.

---

### Q. Multiple Testing 문제는 어떻게 해결하나요?

Bonferroni Correction 또는 FDR(Benjamini-Hochberg)을 사용합니다.

실무에서는 일반적으로 FDR을 더 많이 사용합니다.

---

### Q. Heavy-tailed 데이터는 어떻게 처리하나요?

Log Transform, Winsorization, Bootstrap, Robust Statistics 등을 활용하여 분포 왜곡과 이상치 영향을 줄입니다.

---

### Q. 왜 Cluster Robust SE가 필요한가요?

반복 측정 데이터나 동일 사용자 데이터는 독립성 가정을 위반하기 때문에 표준오차가 과소추정될 수 있습니다. Cluster Robust SE는 이를 보정하여 보다 신뢰할 수 있는 통계적 추론을 가능하게 합니다.
