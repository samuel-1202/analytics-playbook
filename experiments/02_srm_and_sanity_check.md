# SRM and Sanity Check

## 개요

A/B 테스트 결과를 해석하기 전에 가장 먼저 확인해야 할 것은 실험 자체가 정상적으로 수행되었는지 여부이다.

아무리 통계적으로 유의한 결과가 나와도 실험군과 대조군이 올바르게 배정되지 않았거나, 데이터 수집 과정에 문제가 있다면 결과를 신뢰할 수 없다.

따라서 실험 전후로 다양한 검증 절차를 수행하며, 대표적으로 AA Test, SRM(Sample Ratio Mismatch), Baseline Parity, Guardrail Monitoring이 사용된다.

---

# 1. AA Test

## 정의

실험 시작 전 실험 인프라 및 랜덤화 로직이 정상적으로 동작하는지 검증하는 과정이다.

실험군과 대조군에 동일한 경험을 제공한 상태에서 두 그룹 간 차이가 발생하지 않는지 확인한다.

즉,

> "효과가 없는 상황에서 효과가 없는 결과가 나오는지 검증하는 과정"

이다.

---

## 목적

- 랜덤화 로직 검증
- Bucketing 정상 동작 여부 확인
- 데이터 수집 파이프라인 검증
- 그룹 분배 정상 여부 확인
- 측정 지표 계산 로직 검증

---

## 주요 확인 항목

### Group Allocation

예상 비율

- Control : 50%
- Treatment : 50%

실제 비율이 유사한지 확인

### Baseline Metrics

실험 시작 전 주요 지표 비교

예시

- DAU
- 구매율
- 매출
- 방문 빈도

유의미한 차이가 없어야 함

---

## AA Test와 Sanity Check의 관계

AA Test는 실험 시작 전에 수행하는 시스템 수준 검증이다.

Sanity Check는 실험 진행 중 또는 종료 후 수행하는 실험 수준 검증이다.

```text
AA Test
↓
실험 시작
↓
Sanity Check
↓
결과 해석
```

---

# 2. Sample Ratio Mismatch (SRM)

## 정의

실험군과 대조군의 실제 샘플 수가 설계된 비율과 유의하게 다른 현상

예시

설계

- Control : 50%
- Treatment : 50%

실제

- Control : 56%
- Treatment : 44%

---

## 왜 중요한가?

SRM이 발생했다는 것은 랜덤 배정 과정 또는 데이터 수집 과정에 문제가 있을 가능성이 높다는 의미이다.

SRM이 발견되면 실험 결과 자체를 신뢰하기 어렵다.

따라서

> SRM 발생 → 원인 분석 → 재실험

이 원칙이다.

---

## 탐지 방법

### Chi-Square Test

#### 귀무가설 (H0)

설계 비율과 실제 비율이 동일하다.

#### 대립가설 (H1)

설계 비율과 실제 비율이 다르다.

일반적으로

```text
p < 0.001
```

이면 SRM 발생으로 판단한다.

---

## 주요 원인

### Traffic Routing Issue

트래픽 전달 과정 문제

예시

- 특정 서버 오류
- 특정 플랫폼 누락
- API 호출 실패

---

### Tracking Loss

데이터 측정 누락

예시

- Android 이벤트 누락
- 특정 브라우저 추적 실패
- 로그 적재 실패

---

### Bucketing Bug

랜덤 배정 오류

예시

- Hash 충돌
- Assignment Logic 버그
- Sticky Assignment 실패

---

### Filtering Issue

예시

- Bot Filtering
- Fraud Detection
- Internal Traffic 제거

---

## 대응 방법

### 잘못된 대응

- 비율 맞춰서 샘플 제거
- 가중치 보정

SRM은 분석 단계에서 보정할 수 없다.

### 권장 대응

1. 원인 조사
2. 파이프라인 수정
3. 재실험 수행

---

# 3. Baseline Parity

## 정의

실험 시작 시점에 실험군과 대조군이 유사한 특성을 가지는지 확인하는 절차

SRM이 없더라도 발생할 수 있다.

즉,

```text
SRM 없음
≠
동질성 보장
```

---

## 주요 확인 항목

예시

- 성별
- 연령
- 지역
- Device
- 과거 구매 이력
- 과거 방문 빈도
- 기존 KPI

---

## 확인 방법

### 연속형 변수

- T-Test
- Mann-Whitney U Test

### 범주형 변수

- Chi-Square Test

---

## 대응 방법

### 실험 전

- Stratification
- Blocking

### 실험 후

- CUPED
- Reweighting
- Covariate Adjustment

---

# 4. Guardrail Monitoring

## 정의

Primary Metric 개선을 위해 서비스 전체 품질이 훼손되지 않는지 확인하는 절차

---

## 예시

광고 노출 증가 실험

### Primary Metric

- 광고 수익

### Guardrail Metric

- 스트리밍 건수
- 사용자 이탈률
- 페이지 로딩 시간

---

## 주요 원칙

Primary Metric이 좋아져도 Guardrail Metric이 크게 악화되면 실험은 실패로 간주한다.

예시

```text
광고 수익 +5%

하지만

사용자 이탈률 +20%

→ 실험 실패
```

---

# 5. Sanity Check

## 정의

실험 진행 중 또는 종료 후 결과를 해석하기 전에 수행하는 품질 검증 절차

---

## 주요 확인 항목

### 1. SRM 확인

- 실험군/대조군 비율 확인
- Chi-Square Test 수행

---

### 2. Baseline Parity 확인

- 실험 시작 시 그룹 간 특성 균형 여부 확인

---

### 3. Guardrail Metric 확인

- 사업 방어 지표 이상 여부 확인

---

### 4. 데이터 수집 상태 확인

- 이벤트 누락 여부
- 로그 적재 상태
- 플랫폼별 수집 정상 여부

---

### 5. 실험 기간 검증

- 최소 실험 기간 충족 여부
- 요일 효과 포함 여부
- Novelty Effect 완화 여부

---

# AA Test vs Sanity Check

| 구분 | AA Test | Sanity Check |
|--------|--------|--------|
| 수행 시점 | 실험 전 | 실험 중 / 후 |
| 목적 | 시스템 검증 | 실험 품질 검증 |
| 확인 대상 | 랜덤화, 데이터 수집, 파이프라인 | SRM, Baseline Parity, Guardrail |
| 문제 발견 시 | 실험 시작 보류 | 결과 해석 보류 |

---

# 실무 체크리스트

## 실험 전

- [ ] AA Test 수행
- [ ] Group Allocation 확인
- [ ] Baseline Metric 확인
- [ ] Tracking 검증

## 실험 중

- [ ] SRM 확인
- [ ] Guardrail Monitoring
- [ ] 로그 적재 상태 확인

## 실험 후

- [ ] Baseline Parity 확인
- [ ] Sanity Check 완료
- [ ] 결과 해석 수행
- [ ] 의사결정 진행


# Interview Questions

### Q. SRM이 발생하면 어떻게 하시겠습니까?

SRM은 무작위 배정 또는 데이터 수집 과정에 문제가 발생했다는 강한 신호입니다.
분석 단계에서 보정하기보다 원인을 파악하고 재실험하는 것이 원칙입니다.

### Q. SRM이 없으면 실험이 정상이라고 볼 수 있나요?

아닙니다.
SRM은 비율 이상 여부만 확인할 뿐이며,
Baseline Parity 검증을 통해 그룹 특성 균형도 추가 확인해야 합니다.

### Q. AA Test와 Sanity Check의 차이는 무엇인가요?

AA Test는 실험 전 시스템 검증,
Sanity Check는 실험 중/후 실험 품질 검증입니다.
