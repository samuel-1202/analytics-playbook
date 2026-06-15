# A/B Testing Framework

## 개요

A/B 테스트는 서비스, 제품, 마케팅 변경이 실제로 사용자 행동이나 비즈니스 성과에 영향을 주는지 검증하기 위한 가장 대표적인 실험 방법론이다.

실험의 목적은 단순히 통계적으로 유의미한 결과를 찾는 것이 아니라, 실제 비즈니스 관점에서 의미 있는 효과가 존재하는지 판단하는 것이다.

---

## 1. 문제 정의 (Problem Definition)

실험 설계 이전에 먼저 해결하고자 하는 비즈니스 문제를 명확히 정의해야 한다.

### 예시

**비즈니스 질문**

> 메인 화면 상단에 개인화된 추천 플레이리스트를 노출하면 스트리밍 사용량이 증가할까?

### 가설 설정

#### 귀무가설 (H₀)

- 개인화된 추천 플레이리스트 노출은 스트리밍 사용량에 영향을 주지 않는다.

#### 대립가설 (H₁)

- 개인화된 추천 플레이리스트 노출은 스트리밍 사용량을 증가시킨다.

---

## 2. 지표 설계 (Metric Design)

실험 시작 전 어떤 지표를 기준으로 의사결정할 것인지 명확히 정의해야 한다.

### Primary Metric (핵심 지표)

실험 성공 여부를 판단하는 핵심 지표

예시

- 전환율 (Conversion Rate)
- 구독 유지율 (Retention Rate)
- 결제 완료율
- 사용자당 매출 (Revenue per User)

### Guardrail Metric (사업 방어 지표)

Primary Metric은 좋아졌지만 서비스 전체 관점에서 문제가 발생하지 않는지 확인하기 위한 지표

예시

- DAU / WAU
- 스트리밍 건수
- 페이지 로딩 시간
- 앱 크래시율

예시

```text
광고 수익 증가
↑

하지만

스트리밍 건수 급감
↓

실험 실패
```

### Secondary Metric (보조 지표)

실험 결과를 해석하기 위한 참고 지표

예시

- 세션 길이
- CTR
- 광고 수익
- 기능 사용률

---

## 3. 랜덤화 단위 (Randomization Unit)

가설에 따라 적절한 랜덤화 단위를 선택해야 한다.

### User-level

사용자 단위 랜덤화

주로 사용하는 경우

- Retention
- Conversion
- Subscription

### Item-level

아이템 단위 랜덤화

주로 사용하는 경우

- 추천 시스템
- 광고 소재 비교
- 캠페인 비교

### Auction-level

입찰 단위 랜덤화

주로 사용하는 경우

- 광고 랭킹
- 입찰 알고리즘

### Cluster-level

사용자 간 간섭(Interference)이 예상되는 경우

예시

- 지역
- 학교
- 커뮤니티
- 소셜 네트워크

---

## 4. 샘플 사이즈 설계 (Sample Size Planning)

### Step 1. MID 정의

**MID (Minimum Important Difference)**

비즈니스적으로 의미 있는 최소 효과 크기

예시

- Retention +0.5%p
- 매출 +2%

### Step 2. 분산 추정

AA 테스트 또는 과거 데이터를 활용하여 지표의 자연 변동성을 추정한다.

### Step 3. MDE 정의

**MDE (Minimum Detectable Effect)**

통계적으로 탐지 가능한 최소 효과 크기

일반적으로

```text
MDE ≤ MID
```

가 되도록 설계한다.

### Step 4. 샘플 사이즈 계산

입력값

- α (유의수준)
- β (Type II Error)
- Power (1-β)
- 분산
- MDE

관계

```text
Sample Size ∝ Variance / MDE²
```

즉,

- 분산이 커질수록 필요한 샘플 증가
- 탐지하려는 효과가 작을수록 샘플 급증

---

## 5. 트래픽 배정 (Traffic Allocation)

### 기본

50 : 50

가장 통계적으로 효율적

### 위험도가 높은 실험

점진적 롤아웃

```text
1%
↓
5%
↓
10%
↓
50%
↓
100%
```

---

## 6. Hash Bucketing

대부분의 온라인 실험은 Hash 기반 랜덤화를 사용한다.

예시

```text
hash(user_id + experiment_name) % 10000
```

장점

- 항상 동일 그룹 배정
- 중복 배정 방지
- 트래픽 제어 용이

---

## 7. AA Test

실험 시작 전 실험 인프라를 검증하는 단계

### 목적

- 랜덤화 정상 동작 확인
- 데이터 수집 정상 확인
- 파이프라인 검증
- 그룹 균형 확인

일반적으로

```text
약 1주일 수행
```

---

## 8. Sanity Check

실험 진행 중 반드시 수행해야 하는 검증 절차

### Sample Ratio Mismatch (SRM)

#### 정의

실험군과 대조군 비율이 설계와 다르게 배정되는 현상

예시

```text
예상
50 : 50

실제
55 : 45
```

#### 주요 원인

- 트래픽 라우팅 오류
- 측정 누락
- Bot Filtering
- Bucketing 버그

#### 탐지

Chi-Square Test

```text
p < 0.001
```

이면 SRM 의심

#### 대응

SRM이 발생한 실험은 분석하지 않는다.

원인을 수정한 뒤 재실험 수행

### Baseline Parity

실험 시작 전 그룹 특성이 유사한지 확인

예시

- 성별
- 연령
- 디바이스
- 과거 이용 패턴

### Guardrail Monitoring

실험 중 사업 방어 지표를 지속적으로 모니터링

---

## 9. 실험에서 자주 발생하는 리스크

### Selection Bias

특정 사용자만 실험에 포함되는 경우

대응

- Randomization
- Stratification

### Survivorship Bias

잔존 사용자만 분석하는 경우

대응

- ITT 분석
- Cohort 분석

### Novelty Effect

새로운 기능이라 일시적으로 반응이 증가하는 현상

대응

- 최소 2주 이상 관찰

### Learning Effect

사용자가 기능에 적응하면서 행동이 변하는 현상

대응

- 충분한 관찰 기간 확보

### Interference / Network Effect

실험군 효과가 대조군에 영향을 주는 경우

대응

- Cluster Randomization
- Geo Experiment
- Switchback Experiment

---

## 10. 실험 결과 해석

실험 결과는 통계적 유의성과 실용적 유의성을 함께 고려해야 한다.

| 통계적 유의성 | 실용적 유의성 | 의사결정 |
|-------------|-------------|---------|
| O | O | 적용 |
| O | X | 추가 검토 |
| X | O | 추가 데이터 수집 |
| X | X | 미적용 |

---

## 관련 문서

- 02_srm_and_sanity_check.md
- 03_bias_and_variance_reduction.md
- 04_sutva_and_interference.md
- 05_quasi_experiment.md
- sample_size_calculator.ipynb
- cuped_simulation.ipynb
