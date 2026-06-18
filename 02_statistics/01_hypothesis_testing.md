# Hypothesis Testing (가설 검정)

## 개요

가설 검정(Hypothesis Testing)은 표본 데이터를 이용하여 모집단에 대한 주장을 검증하는 통계적 방법이다.

주요 목적은 관측된 차이가 우연(Random Noise)인지, 실제 효과(True Effect)인지 판단하는 것이다.

---

# 1. 기본 개념

| 개념 | 정의 | 예시 |
|--------|--------|--------|
| Population (모집단) | 관심 있는 전체 대상 | 전체 음악 서비스 사용자 |
| Sample (표본) | 모집단에서 추출한 일부 데이터 | 실험 참여 사용자 |
| Parameter (모수) | 모집단의 특성 | 전체 사용자 평균 스트리밍 수 |
| Statistic (통계량) | 표본으로 계산한 값 | 실험 참여자 평균 스트리밍 수 |

### 표본 대표성

통계 검정 결과가 신뢰되려면 표본이 모집단을 적절히 대표해야 한다.

대표성 확인 방법

| 방법 | 확인 내용 |
|--------|--------|
| 데이터 추출 로직 검토 | 특정 그룹 누락 여부 |
| 모집단 vs 표본 EDA | 주요 변수 분포 비교 |
| Covariate Balance Check | 성별, 연령, 사용량 균형 확인 |
| 통계 검정 | Chi-square, t-test, SMD |

대표성을 위협하는 요인

- Selection Bias
- Survivorship Bias
- Missing Data Bias
- Coverage Bias

---

# 2. 가설 설정

| 구분 | 의미 |
|--------|--------|
| H0 (귀무가설) | 효과 없음, 차이 없음 |
| H1 (대립가설) | 효과 있음, 차이 있음 |

예시

```text
H0 : CTR_A = CTR_B

H1 : CTR_A ≠ CTR_B
```

---

# 3. 가설 검정 프로세스

```text
가설 설정
↓
유의수준(α) 설정
↓
검정통계량 계산
↓
p-value 계산
↓
의사결정
```

---

# 4. 검정 통계량(Test Statistic)

관측된 차이가 얼마나 큰지를 수치화한 값

대표 검정 통계량

| 검정 | 통계량 |
|--------|--------|
| Z-test | Z-statistic |
| T-test | T-statistic |
| Chi-square Test | χ² |
| ANOVA | F-statistic |

직관적으로는

```text
차이
────────
표준오차(SE)
```

라고 생각하면 된다.

---

# 5. Type I Error / Type II Error

| | H0 참 (효과 없음) | H1 참 (효과 있음) |
|----|----|----|
| H0 채택 | True Negative ✅ | False Negative ❌ (β) |
| H0 기각 | False Positive ❌ (α) | True Positive ✅ (Power = 1-β) |

| 개념 | 의미 |
|--------|--------|
| α (Type I Error) | 효과 없는데 있다고 판단 |
| β (Type II Error) | 효과 있는데 없다고 판단 |
| Power | 효과 있을 때 발견할 확률 |

일반적으로

```text
α = 0.05
Power = 0.8
```

를 사용한다.

---

# 6. One-sided vs Two-sided Test

| 구분 | 대립가설 | 사용 예시 |
|--------|--------|--------|
| 양측 검정 | H1 : A ≠ B | 대부분의 A/B 테스트 |
| 단측 검정 | H1 : A > B | 특정 방향만 관심 |

예시

```text
양측:
H1 : CTR_A ≠ CTR_B

단측:
H1 : CTR_B > CTR_A
```

---

# 7. Parametric vs Non-parametric

| 구분 | Parametric | Non-parametric |
|--------|--------|--------|
| 가정 | 정규성, 등분산성 | 가정 적음 |
| 대표 기법 | t-test, ANOVA | Mann-Whitney, Kruskal-Wallis |
| 장점 | 검정력 높음 | Robust |
| 단점 | 가정 위반에 민감 | 검정력 낮음 |

---

# 8. 검정 선택 가이드

| 분석 목적 | 추천 검정 |
|--------|--------|
| 두 그룹 평균 비교 | t-test |
| 세 그룹 이상 평균 비교 | ANOVA |
| 비율 비교 | Z-test |
| 범주형 변수 비교 | Chi-square |
| 비모수 데이터 | Mann-Whitney U |
| 세 그룹 이상 비모수 | Kruskal-Wallis |

예시

```text
CTR 비교
→ Z-test

평균 매출 비교
→ t-test

성별 분포 비교
→ Chi-square
```

---

# 9. 실무 체크리스트

### 검정 전

- 가설 정의
- Primary Metric 정의
- Assumption 확인
- 표본 대표성 확인

### 검정 후

- p-value 확인
- Effect Size 확인
- Confidence Interval 확인
- Business Impact 확인

---

# 10. 흔한 실수

| 실수 | 문제 |
|--------|--------|
| p-value만 확인 | 효과 크기 무시 |
| Effect Size 미확인 | 실무 의미 해석 불가 |
| Multiple Testing 무시 | False Positive 증가 |
| Assumption Check 생략 | 잘못된 결론 가능 |

---

# Interview Questions

### Q. 가설 검정이란?

표본 데이터를 이용하여 모집단에 대한 주장을 검증하는 통계적 방법이다.

### Q. Type I Error와 Type II Error 차이는?

- Type I Error : 효과 없는데 있다고 판단
- Type II Error : 효과 있는데 없다고 판단

### Q. 왜 p-value만 보면 안 되나요?

통계적 유의성만 설명할 뿐 효과 크기와 비즈니스 임팩트는 설명하지 못하기 때문이다.

### Q. 언제 Mann-Whitney를 사용하나요?

Outlier가 많거나 정규성 가정이 충족되지 않을 때 사용한다.
