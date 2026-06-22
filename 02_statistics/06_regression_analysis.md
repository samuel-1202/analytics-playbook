# Regression Analysis

## 개요

회귀분석(Regression Analysis)은 변수 간의 관계를 정량적으로 설명하고 특정 변수의 영향력을 추정하기 위한 방법이다.

통계 검정이

```text
차이가 있는가?
```

를 확인하는 것이라면,

회귀분석은

```text
얼마나 영향을 주는가?
```

를 설명하는 방법이다.

---

# 1. Simple Linear Regression

## 정의

독립변수(X) 하나가 종속변수(Y)에 미치는 영향을 분석

```text
Y = β0 + β1X + ε
```

예시

```text
광고비 증가
↓
매출 증가
```

---

## 계수 해석

```text
β1 = 100
```

이면

```text
광고비 1만원 증가 시

매출 100원 증가
```

로 해석

---

# 2. Multiple Linear Regression

## 정의

여러 독립변수가 종속변수에 미치는 영향을 동시에 분석

```text
Y = β0 + β1X1 + β2X2 + ...
```

예시

```text
매출

=
광고비
+
방문횟수
+
성별
+
연령
```

---

## 장점

다른 변수의 영향을 통제한 상태에서 효과 추정 가능

예시

```text
광고비 효과

vs

고객 특성 효과
```

분리 가능

---

# 3. OLS Assumptions (OLS 가정)

회귀분석 결과를 신뢰하기 위해서는 몇 가지 가정이 필요하다.

---

## 1. Linearity (선형성)

### 의미

X와 Y의 관계가 직선 형태라고 가정

```text
광고비 ↑
↓
매출 ↑
```

---

### 문제 상황

실제로는

```text
광고비 ↑
↓
초반에는 증가

후반에는 증가폭 감소
```

할 수 있음

---

### 대응 방법

- Log Transformation
- Polynomial Regression
- Tree Model

---

## 2. Independence (독립성)

### 의미

각 관측치가 서로 영향을 주지 않아야 함

---

### 문제 상황

예시

```text
같은 사용자의 반복 방문

같은 지역 사용자

같은 광고주의 캠페인
```

---

### 문제점

```text
실제보다 표준오차가 작아짐

↓

가짜 유의성(False Positive)
```

---

### 대응 방법

- Cluster Randomization
- Mixed Model
- Cluster Robust SE

---

## 3. Homoscedasticity (등분산성)

### 의미

오차의 분산이 일정해야 함

---

### 정상 예시

```text
저매출 고객

고매출 고객

오차 크기 비슷
```

---

### 문제 상황

```text
고매출 고객

↓

오차가 훨씬 큼
```

---

### 문제점

```text
회귀계수(β)는 맞음

하지만

표준오차(SE)가 왜곡됨
```

---

### 대응 방법

- Robust SE
- Log Transformation
- Weighted Regression

---

## 4. Normality (정규성)

### 의미

오차(Residual)가 정규분포를 따른다는 가정

---

### 문제 상황

```text
매출 데이터

사용시간 데이터
```

처럼

Right-skewed

Heavy-tailed

분포가 많음

---

### 문제점

```text
p-value

Confidence Interval

왜곡 가능
```

---

### 대응 방법

- Log Transformation
- Bootstrap
- Robust Statistics

---

## 5. No Multicollinearity (다중공선성)

### 의미

독립변수끼리 지나치게 높은 상관관계를 가지면 안 됨

---

### 예시

```text
스트리밍 건수

스트리밍 일수

총 이용시간
```

---

### 문제 상황

비슷한 정보를 가진 변수가 동시에 들어감

---

### 문제점

```text
회귀계수 해석 어려움

계수 방향 불안정

p-value 왜곡
```

---

예시

```text
모델 A

스트리밍 건수
+
스트리밍 일수

↓

건수 +
일수 -

```

같은 이상한 결과 발생

---

### 진단 방법

- Correlation Matrix
- VIF (Variance Inflation Factor)

---

### 일반 기준

```text
VIF > 5

주의

VIF > 10

심각
```

---

### 대응 방법

- 변수 제거
- Feature Selection
- PCA

---

# 4. Robust SE

## 정의

회귀계수는 그대로 두고

표준오차(SE)만 보정하는 방법

---

## 왜 필요한가?

OLS는

```text
등분산성
```

을 가정

하지만 실무에서는 자주 깨짐

---

### 일반 OLS

```text
β = 정상

SE = 왜곡
```

---

### Robust SE

```text
β = 정상

SE = 보정
```

---

## 언제 사용하는가?

- Heteroskedasticity
- Heavy-tailed Data
- 실무 로그 데이터

---

# 5. Cluster Robust SE

## 정의

같은 그룹 내부 상관관계를 고려하여 표준오차를 보정

---

## 예시

```text
같은 사용자

같은 광고주

같은 지역
```

---

## 일반 Robust SE 와 차이

| 방법 | 대응 |
|--------|--------|
| Robust SE | 이분산성 |
| Cluster Robust SE | 이분산성 + 비독립성 |

---

# Frequently Confused Concepts

## 회귀분석 vs 가설검정

가설검정

```text
차이가 있는가?
```

회귀분석

```text
얼마나 영향을 주는가?
```

---

## 상관관계 vs 인과관계

❌

```text
상관관계

=

인과관계
```

아님

---

## Robust SE가 계수를 바꾸는가?

❌ 아니다

```text
회귀계수(β)

그대로
```

```text
표준오차(SE)

보정
```

---

## 다중공선성이 있으면 예측력이 떨어지는가?

반드시 그렇지는 않다.

문제는

```text
해석 가능성
```

이다.

예측 모델에서는 큰 문제가 아닐 수 있지만

회귀계수 해석에는 치명적일 수 있다.
