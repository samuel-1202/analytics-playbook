# Statistical Tests

## 개요

통계 검정은 데이터 특성과 가정에 따라 적절한 방법을 선택해야 한다.

실무에서는 검정 자체보다

- 데이터 타입
- 가정 충족 여부
- Outlier 존재 여부

를 먼저 확인하는 것이 중요하다.

---

# 검정 선택 가이드

| 목적 | Parametric | Robust / Non-parametric |
|--------|--------|--------|
| 평균 비교 (2그룹) | t-test | Mann-Whitney U |
| 평균 비교 (3그룹 이상) | ANOVA | Kruskal-Wallis |
| 비율 비교 | Z-test | Fisher Exact |
| 범주형 분포 비교 | Chi-square | Fisher Exact |
| 상관관계 | Pearson | Spearman |

---

# 1. T-test

## 목적

두 그룹 평균 비교

---

## 가정

- 독립성
- 정규성
- 등분산성

---

## 예시

```text
A/B Test

평균 구매금액 비교
```

---

## 한계

- Outlier 영향 큼
- Heavy Tail 취약

---

## 대안

### Mann-Whitney U

정규성 가정이 어려울 때 사용

비모수 검정

---

# 2. ANOVA

## 목적

3개 이상 그룹 평균 비교

---

## 예시

```text
광고 A
광고 B
광고 C
```

CTR 비교

---

## 한계

정규성

등분산성

---

## 대안

### Kruskal-Wallis

비모수 ANOVA

---

# 3. Z-test

## 목적

비율 비교

---

## 예시

```text
CTR
CVR
Retention
```

---

## 조건

충분히 큰 Sample Size

---

# 4. Chi-square Test

## 목적

범주형 데이터 비교

---

## 예시

```text
성별 분포

A/B 그룹 분포
```

---

## 실무 활용

- SRM 탐지
- 그룹 분포 비교

---

# 5. Correlation

## Pearson Correlation

가정

- 선형 관계
- 정규성

---

## Spearman Correlation

순위 기반

Outlier에 강함

---

# Robust Statistics

## 왜 필요한가?

실무 데이터는

- Outlier
- Heavy-tailed
- Skewed

인 경우가 많다.

---

## 대표 대체 방법

| 기존 방법 | Robust 방법 |
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
정규성 위반

Outlier 다수

Heavy-tail
```

인 경우

---

# Frequently Confused Concepts

## T-test vs Mann-Whitney

T-test

```text
평균 비교
```

Mann-Whitney

```text
분포 비교
순위 비교
```

---

## ANOVA vs Multiple T-test

❌

```text
A-B

A-C

B-C
```

반복 수행

↓

Type I Error 증가

---

✅

```text
ANOVA
```

먼저 수행

---

## Z-test vs Chi-square

Z-test

```text
비율 차이
```

Chi-square

```text
분포 차이
```

---

## Parametric vs Non-parametric

Parametric

```text
가정 강함

Power 높음
```

Non-parametric

```text
가정 약함

Robust
```
