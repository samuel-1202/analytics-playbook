# Bias and Variance Reduction

## 개요

A/B 테스트는 무작위 배정을 통해 인과관계를 추정하는 강력한 방법이지만, 실제 서비스 환경에서는 다양한 편향(Bias)과 데이터 불균형(Imbalance)이 발생할 수 있다.

이러한 문제는 실험 결과를 왜곡하거나 효과 크기를 과대·과소 추정하게 만들 수 있으며, 심한 경우 잘못된 의사결정으로 이어질 수 있다.

또한 실험이 정상적으로 수행되더라도 지표의 분산이 크면 효과를 탐지하기 어려워진다.

따라서 실험 설계 단계에서는 편향을 최소화하고, 분석 단계에서는 분산을 줄여 보다 정확한 효과 추정을 수행해야 한다.

---

# 1. Bias vs Imbalance

## Bias

특정 방향으로 결과가 왜곡되는 현상

예시

- Selection Bias
- Survivorship Bias
- MNAR
- Simpson's Paradox

---

## Imbalance

실험군과 대조군 비율은 정상(SRM 없음)이지만 그룹 특성이 다른 상황

예시

```text
실험군 평균 연령: 25세
대조군 평균 연령: 42세
```

SRM은 없지만 결과 해석에 영향을 줄 수 있다.

---

# 2. Selection Bias

## 정의

특정 특성을 가진 사용자만 분석에 포함되어 결과가 모집단을 대표하지 못하는 현상

---

## 예시

광고 실험에서

- 적극적 사용자만 분석

결과

```text
CTR 증가
구매 증가
```

하지만 전체 사용자에 적용하면 동일한 효과가 발생하지 않을 수 있음

---

## 진단 방법

- AA Test
- Baseline Parity
- 모집단 vs 실험 대상 비교

---

## 완화 방법

### 사전

- Random Sampling
- Random Assignment
- Stratification

### 사후

- Reweighting
- Propensity Score Matching (PSM)

---

# 3. Survivorship Bias

## 정의

잔존한 사용자만 분석에 포함되어 효과가 과대평가되는 현상

---

## 예시

광고 캠페인 분석 시

CTR이 낮아 노출되지 않은 캠페인을 제외하고 분석

↓

성과 과대평가

---

## 진단 방법

- Cohort Analysis
- Retention Curve
- 이탈자 / 잔존자 비교

---

## 완화 방법

- Intention-To-Treat (ITT)
- Cohort Analysis
- Survival Analysis

---

# 4. Missing Data

## 정의

데이터가 누락(Missing)된 상황

누락 원인에 따라 결과 해석과 대응 방법이 달라진다.

실무에서는 Missing Data를 크게 세 가지 유형으로 구분한다.

| 유형 | 의미 | 편향 위험 |
|--------|--------|--------|
| MCAR | Missing Completely At Random | 낮음 |
| MAR | Missing At Random | 중간 |
| MNAR | Missing Not At Random | 높음 |

---

## 4.1 MCAR (Missing Completely At Random)

### 정의

누락 여부가 어떤 변수와도 관련이 없는 경우

즉,

```text
누락이 완전히 랜덤하게 발생
```

하는 상황

---

### 예시

설문 응답 데이터 중

```text
서버 장애로 일부 데이터 유실
```

---

### 특징

- 가장 이상적인 누락 형태
- 편향(Bias)은 거의 발생하지 않음
- 표본 크기 감소 문제만 존재

---

### 진단 방법

- 누락 패턴 확인
- 누락 여부와 주요 변수 간 관계 분석

---

### 대응 방법

- Listwise Deletion (단순 제거)
- Mean/Median Imputation
- Multiple Imputation

---

## 4.2 MAR (Missing At Random)

### 정의

누락 여부가 관측 가능한 변수(Observed Variable)로 설명 가능한 경우

---

### 예시

설문 응답률

```text
20대 : 응답률 높음
50대 : 응답률 낮음
```

연령 정보를 알고 있다면 누락 원인을 설명할 수 있음

---

### 특징

```text
Missingness
↓
Observed Variable로 설명 가능
```

---

### 진단 방법

- Missing Pattern 분석
- Missing Indicator 생성
- 응답자 vs 비응답자 비교

예시

- 연령
- 성별
- 디바이스
- 지역

---

### 대응 방법

- Multiple Imputation
- Regression Imputation
- Reweighting
- Inverse Probability Weighting (IPW)

---

## 4.3 MNAR (Missing Not At Random)

### 정의

누락 여부 자체가 결과 변수와 관련된 경우

---

### 예시

만족도 설문

```text
만족한 사용자
→ 응답

불만족 사용자
→ 응답하지 않음
```

---

### 특징

```text
Missingness
↓
Outcome 자체와 관련
```

가장 위험한 Missing Data 유형

---

### 진단 방법

엄밀하게는 직접 검증이 불가능

간접적으로 추정

- Missing Pattern 분석
- Domain Knowledge 활용
- Sensitivity Analysis

---

### 대응 방법

- Multiple Imputation
- Sensitivity Analysis
- 추가 데이터 수집
- 설문 구조 개선

---

## MCAR vs MAR vs MNAR 비교

| 구분 | MCAR | MAR | MNAR |
|--------|--------|--------|--------|
| 누락 원인 | 완전 랜덤 | 관측 변수로 설명 가능 | 결과 자체와 관련 |
| 편향 위험 | 낮음 | 중간 | 높음 |
| 진단 가능성 | 쉬움 | 가능 | 어려움 |
| 분석 영향 | 적음 | 보정 가능 | 심각 |
| 대표 대응 | 제거 가능 | Imputation | Sensitivity Analysis |

---

## 실무 예시

### 광고 로그

#### MCAR

```text
서버 장애로 로그 일부 유실
```

---

### 회원 프로필

#### MAR

```text
고연령 사용자 응답률 낮음
```

연령 정보로 설명 가능

---

### 만족도 조사

#### MNAR

```text
불만족 사용자가 응답하지 않음
```

만족도 자체가 누락 원인

---

## Missing Data 분석 프로세스

### Step 1. 누락 패턴 확인

- Missing Rate 계산
- 변수별 Missing 확인

---

### Step 2. 누락 원인 추정

MCAR / MAR / MNAR 분류

---

### Step 3. 적절한 처리 방법 선택

| 상황 | 권장 방법 |
|--------|--------|
| MCAR | 제거 또는 단순 대체 |
| MAR | Multiple Imputation |
| MNAR | Sensitivity Analysis |

---

### Step 4. 결과 민감도 검증

- Missing 처리 방식 변경
- 결과 변화 확인

---

## Sensitivity Analysis

### 정의

누락 데이터에 대한 가정을 변경해도 결과가 유지되는지 검증하는 방법

---

### 예시

Case 1

```text
누락값 = 평균값
```

---

Case 2

```text
누락값 = 최악의 값
```

---

Case 3

```text
누락값 = 최선의 값
```

---

세 가지 경우 모두 결과가 유사하면

```text
Robust Result
```

로 판단 가능

---

## Interview Questions

### Q. MCAR, MAR, MNAR의 차이를 설명해주세요.

MCAR는 누락이 완전히 랜덤하게 발생한 경우이고,
MAR는 관측 가능한 변수로 누락을 설명할 수 있는 경우,
MNAR는 누락 여부 자체가 결과 변수와 관련된 경우입니다.

실무에서는 MNAR이 가장 위험하며, 관측 데이터만으로 완전히 보정하기 어렵습니다.

---

### Q. 가장 위험한 Missing Data 유형은 무엇인가요?

MNAR입니다.

누락 자체가 결과 변수와 관련되어 있기 때문에 관측 데이터만으로 편향을 제거하기 어렵습니다.

---

### Q. MNAR인지 어떻게 확인하나요?

엄밀하게는 직접 확인할 수 없습니다.

대신

- Missing Pattern 분석
- Domain Knowledge
- Sensitivity Analysis

를 통해 영향 범위를 추정합니다.

---

### Q. Missing Data가 많으면 어떻게 하시겠습니까?

1. 누락 패턴 확인
2. MCAR / MAR / MNAR 분류
3. 적절한 Imputation 수행
4. Sensitivity Analysis로 결과 견고성 검증

순서로 접근합니다.

---

# 5. Simpson's Paradox

## 정의

전체 데이터와 세그먼트 데이터의 결과 방향이 반대로 나타나는 현상

---

## 예시

전체 CTR

```text
A > B
```

하지만

20대 여성

```text
B > A
```

---

## 진단 방법

- 세그먼트별 분석
- Stratified Analysis

---

## 완화 방법

- Stratification
- Segment Analysis
- Hierarchical Modeling

---

# 6. Variance Reduction

## 왜 필요한가?

실험 결과의 분산이 크면

- 더 많은 샘플 필요
- 더 긴 실험 기간 필요
- 작은 효과 탐지 어려움

---

관계

```text
Sample Size ∝ Variance
```

즉

```text
분산 감소 = 샘플 감소
```

---

# 7. Stratification

## 정의

중요 변수별로 층(Stratum)을 나눈 뒤 각 층 내에서 무작위 배정

---

## 목적

- 그룹 균형 확보
- 분산 감소

---

## 예시

층화 변수

- 성별
- 연령대
- Device

```text
20대 여성
↓
랜덤 배정

30대 여성
↓
랜덤 배정
```

---

## 장점

- Baseline 균형
- Sample Size 감소
- Variance 감소

---

## 언제 사용하는가?

다음과 같이 KPI에 영향을 많이 주는 변수가 존재할 때

- 성별
- 연령
- Device
- 기존 구매 여부

---

# 8. Blocking

## 정의

유사한 단위를 Block으로 묶고 Block 내에서 랜덤 배정

---

## 목적

Nuisance Variable(외부 교란 요인) 통제

---

## 예시

- 지역
- 국가
- 시간대

```text
서울
↓
랜덤 배정

부산
↓
랜덤 배정
```

---

## Stratification vs Blocking

| 구분 | Stratification | Blocking |
|--------|--------|--------|
| 목적 | 대표성 확보 | 외부 영향 제거 |
| 대상 | 사용자 특성 | 환경 변수 |
| 예시 | 성별, 연령 | 지역, 시간대 |

실무에서는 거의 유사하게 사용되는 경우도 많다.

---

# 9. CUPED

## 정의

실험 전 데이터를 Covariate로 활용하여 분산을 감소시키는 기법

**Controlled Experiments Using Pre-Experiment Data**

---

## 아이디어

실험 KPI와 높은 상관관계를 가지는 사전 데이터를 활용

예시

```text
실험 KPI
= 구매 금액

사전 데이터
= 지난달 구매 금액
```

---

## 효과

일반적으로

```text
20% ~ 50%
```

수준의 분산 감소 가능

---

## 장점

- 추가 트래픽 불필요
- 실험 기간 단축
- Power 증가
- Sample Size 감소

---

## 언제 사용하는가?

- 사용자별 과거 행동 데이터가 존재할 때
- KPI와 상관관계가 높은 사전 지표가 존재할 때

---

# 10. Reweighting

## 정의

실험 데이터가 모집단을 대표하도록 가중치를 조정하는 방법

---

## 아이디어

과대표집

↓

가중치 감소

과소표집

↓

가중치 증가

---

## 예시

모집단

```text
남성 50%
여성 50%
```

실험 데이터

```text
남성 80%
여성 20%
```

↓

가중치 조정

---

## 활용 사례

- Selection Bias 완화
- Survey Data 보정
- Observational Study 분석

---

# 11. Sensitivity Analysis

## 정의

분석 가정이 달라져도 결론이 유지되는지 검증하는 방법

---

## 예시

### Missing Data

- MCAR
- MAR
- MNAR

가정 변경 후 결과 비교

---

### Outlier

- 포함
- 제거

결과 비교

---

### Attribution Window

- 7일
- 14일
- 28일

결과 비교

---

## 목적

```text
결과가 얼마나 견고한가?
```

를 검증하는 것

---

# Bias 완화 전략

## 사전 설계

- Randomization
- Stratification
- Blocking

---

## 사후 분석

- CUPED
- Reweighting
- Propensity Score Matching (PSM)

---

## 견고성 검증

- Sensitivity Analysis

---

# 실무 체크리스트

## 실험 설계

- [ ] 주요 Bias 식별
- [ ] Stratification 필요 여부 검토
- [ ] Blocking 필요 여부 검토

## 실험 분석

- [ ] Baseline Imbalance 확인
- [ ] CUPED 적용 가능 여부 검토
- [ ] Reweighting 필요 여부 검토

## 결과 검증

- [ ] Sensitivity Analysis 수행
- [ ] 세그먼트별 결과 확인
- [ ] Simpson's Paradox 여부 확인
- [ ] 결론의 견고성 검증

---

# Interview Questions

### Q. SRM이 없으면 실험군과 대조군은 동일하다고 볼 수 있나요?

아닙니다.

SRM은 샘플 비율만 확인하는 절차이며, Baseline Parity를 통해 그룹 특성의 균형 여부를 추가 검증해야 합니다.

---

### Q. Stratification과 Blocking의 차이는 무엇인가요?

둘 다 그룹 균형을 맞추는 방법이지만,

- Stratification은 대표성 확보와 분산 감소 목적
- Blocking은 외부 교란 변수 제거 목적

이라는 차이가 있습니다.

---

### Q. CUPED를 사용하는 이유는 무엇인가요?

실험 전 데이터를 활용하여 KPI 분산을 줄이기 위함입니다.

분산이 감소하면 동일한 효과를 더 적은 샘플로 탐지할 수 있으며, 실험 기간 단축 효과도 기대할 수 있습니다.

---

### Q. Sensitivity Analysis는 언제 수행하나요?

분석 결과가 특정 가정에 지나치게 의존하지 않는지 검증할 때 수행합니다.

Missing Data 처리 방식, Outlier 처리 방식, Attribution Window 등을 변경해도 결론이 유지되는지 확인합니다.
