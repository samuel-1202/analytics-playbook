# Robust Statistics

## 개요

실무 데이터는 통계 교과서의 가정을 만족하지 않는 경우가 많다.

대표적으로

- Outlier
- Heavy-tailed Distribution
- Non-independence

문제가 발생하며,

이러한 상황에서는 Robust Statistics와 표준 통계 기법의 보완 방법을 고려해야 한다.

---

# 1. Outlier (이상치)

## 정의

대부분의 데이터와 크게 떨어져 있는 극단값

---

## 예시

```text
일반 사용자 그룹 인당 매출 = 1만원

---

VIP 사용자 그룹 인당 매출 = 500만원
```

---

## 문제점

```text
평균 왜곡

분산 증가

신뢰구간 증가

검정력 감소
```

---

## 탐지 방법

### IQR Rule

```text
Q1 - 1.5 × IQR

Q3 + 1.5 × IQR
```

범위 밖 데이터

---

### Z-score

```text
|Z| > 3
```

---

### Box Plot

시각적 탐지

---

## 대응 방법

| 방법 | 설명 |
|--------|--------|
| 제거 | 데이터 오류인 경우 |
| Winsorization | 극단값 절단 |
| Log Transform | 분포 압축 |
| Robust Statistics | 영향 최소화 |

---

# 2. Heavy-tailed Distribution

## 정의

정규분포보다 극단값이 발생할 확률이 높은 분포

---

## 예시

```text
사용자 매출

사용 시간

스트리밍 횟수
```

---

### 일반 정규분포

```text
극단값 드묾
```

---

### Heavy-tail

```text
극단값 자주 발생
```

---

## 문제점

```text
평균 불안정

분산 증가

신뢰구간 확대
```

---

## 대응 방법

### Log Transformation

```text
log(x + 1)
```

---

### Median 사용

```text
Mean → Median
```

---

### Bootstrap

비모수 방식으로 신뢰구간 추정

---

# 3. Non-independence (비독립성)

## 정의

관측치가 서로 독립이 아닌 경우

---

## 예시

```text
같은 사용자의 반복 방문

같은 지역 사용자

같은 광고주 캠페인
```

---

## 문제점

```text
표준오차 과소추정

False Positive 증가
```

---

## 대응 방법

### Cluster Randomization

클러스터 단위 실험

---

### Mixed Effect Model

Random Effect 추가

---

# Cluster Robust Standard Error

## 정의

클러스터(Cluster) 내부의 관측치들이 서로 독립이 아닐 때 표준오차(Standard Error)를 보정하는 방법

---

## 왜 필요한가?

일반적인 OLS는

```text
모든 관측치가 서로 독립
```

이라고 가정한다.

하지만 실무에서는 이 가정이 자주 깨진다.

---

## 예시 1. 같은 사용자의 반복 방문

```text
user_id = A

1일차 방문
2일차 방문
3일차 방문
4일차 방문
```

---

일반 OLS는

```text
4개의 독립 샘플
```

로 간주

하지만 실제로는

```text
같은 사용자
```

라서 행동 패턴이 매우 유사할 수 있다.

---

## 예시 2. 같은 광고주 캠페인

```text
광고주 A

캠페인1
캠페인2
캠페인3
```

---

광고주의 특성이 공유되므로

관측치가 완전히 독립이라고 보기 어렵다.

---

## 예시 3. 지역 기반 데이터

```text
서울

경기

부산
```

---

같은 지역 사용자는

- 날씨
- 경제 상황
- 프로모션

등의 영향을 함께 받을 수 있다.

---

# 문제점

독립성 가정이 깨지면

```text
표준오차(SE)가 과소 추정됨
```

---

결과

```text
실제로는 유의하지 않은데

유의한 것처럼 보일 수 있음
```

---

즉

```text
False Positive 증가
```

---

# Cluster Robust SE의 아이디어

일반 Robust SE

```text
이분산성(Heteroskedasticity)
```

만 보정

---

Cluster Robust SE

```text
이분산성
+
클러스터 내부 상관관계
```

까지 보정

---

즉

```text
같은 클러스터 안에서는

상관관계 존재 허용
```

한다.

---

# 무엇이 바뀌는가?

회귀계수는 그대로

```text
β = 동일
```

---

표준오차만 수정

```text
SE = 증가
```

하는 경우가 많다.

---

결과

```text
t-stat 감소

p-value 증가

Confidence Interval 확대
```

---

즉

```text
더 보수적인 결과
```

를 얻게 된다.

---

# Robust SE vs Cluster Robust SE

| 방법 | 대응 문제 |
|--------|--------|
| Robust SE | 이분산성 |
| Cluster Robust SE | 이분산성 + 비독립성 |

---

## 예시

### Robust SE

```text
고소득 사용자

↓

분산 증가
```

---

### Cluster Robust SE

```text
같은 사용자 반복 관측

같은 광고주

같은 지역
```

---

# 언제 사용하는가?

## User Level Repeated Measure

```text
한 사용자가 여러 번 등장
```

---

## Geo Experiment

```text
지역 단위 실험
```

---

## DID

```text
지역

매장

학교

기업
```

단위 분석

---

## Panel Data

```text
User × Time

Store × Time
```

데이터

---

# 실무 예시

## 잘못된 분석

```text
CTR 개선 효과 분석

관측치

100만 건
```

---

하지만

```text
실제 사용자 수

10만 명
```

---

동일 사용자가 여러 번 등장

↓

독립성 위반

↓

SE 과소추정

↓

가짜 유의성

---

## 개선

```text
Cluster = user_id
```

로 설정

↓

Cluster Robust SE 적용

↓

보다 현실적인 p-value 계산

---

# Frequently Confused Concepts

## Cluster Randomization vs Cluster Robust SE

### Cluster Randomization

실험 설계 단계

```text
클러스터 단위 배정
```

---

### Cluster Robust SE

분석 단계 **표준오차 보정**

---

## Cluster Robust SE가 Bias를 제거하는가?

❌ 아니다. Cluster Robust SE는 **표준오차 보정**만 수행.

---

Bias, Confounding, Selection Bias 문제는 해결하지 못한다.

---

## Cluster 수가 적어도 괜찮은가?

**주의 필요**

Cluster 수 < 30 이면 추정이 불안정할 수 있다. 실무에서는 30~50개 이상 권장

---

# 한 줄 정리

Cluster Robust SE 정의

```text
같은 클러스터 내부의 상관관계를 허용하여 왜곡된 표준오차를 보정하는 방법
```

**회귀계수는 그대로 두고 표준오차만 수정하여 보다 현실적인 p-value와 신뢰구간을 제공한다.**
---

# 4. Robust Statistics

## 정의

가정 위반에 덜 민감한 통계 방법

---

## 비교

| 일반 방법 | Robust 방법 |
|--------|--------|
| Mean | Median |
| Std | MAD |
| t-test | Mann-Whitney |
| ANOVA | Kruskal-Wallis |
| OLS | Quantile Regression |
| Parametric CI | Bootstrap CI |

---

## 언제 사용하는가?

```text
Outlier 존재

Heavy-tail

정규성 위반
```

---

# 5. Bootstrap

## 정의

데이터를 반복 재추출(Resampling)하여 분포를 추정하는 방법

---

## 장점

```text
정규성 가정 불필요
```

---

## 활용

- Confidence Interval
- Effect Size
- A/B Test

---

## 예시

```text
원본 데이터

↓

1,000번 재추출

↓

분포 생성

↓

CI 계산
```

---

# 6. Cluster Robust SE

## 정의

클러스터 내부 상관관계를 고려하여 표준오차를 보정하는 방법

---

## 예시

```text
user_id

region

advertiser_id
```

---

## 일반 Robust SE와 차이

| 방법 | 목적 |
|--------|--------|
| Robust SE | 이분산성 대응 |
| Cluster Robust SE | 이분산성 + 비독립성 대응 |

---

## 핵심

```text
회귀계수(β)

변화 없음

표준오차(SE)

보정
```

---

# Frequently Confused Concepts

## Outlier vs Heavy-tail

Outlier

```text
소수의 극단값
```

---

Heavy-tail

```text
극단값이 자주 발생하는 분포
```

---

## Robust Statistics = Outlier 제거?

❌ 아니다

Outlier를 제거하는 것이 아니라

영향을 줄이는 방법이다.

---

## Robust SE가 계수를 바꾸는가?

❌ 아니다

```text
β

그대로
```

```text
SE

보정
```

---

## Mean vs Median

Mean

```text
효율적

하지만 Outlier 취약
```

---

Median

```text
Robust

하지만 정보 일부 손실
```

---

## Bootstrap은 언제 사용할까?

- 정규성 가정이 어려울 때
- Sample Size가 작을 때
- Heavy-tailed 데이터
- 실험 결과 신뢰구간 추정
