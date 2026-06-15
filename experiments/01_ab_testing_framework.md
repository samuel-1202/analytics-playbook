# A/B Testing Framework

## 개요

A/B 테스트는 서비스, 제품, 마케팅 변경이 실제로 사용자 행동이나 비즈니스 성과에 영향을 주는지 검증하기 위한 가장 대표적인 실험 방법론이다.

실험의 목적은 단순히 통계적으로 유의미한 결과를 찾는 것이 아니라, 실제 비즈니스 관점에서 의미 있는 효과가 존재하는지 판단하는 것이다.

---

## 1. 문제 정의 (Problem Definition)

실험 설계 이전에 먼저 해결하고자 하는 비즈니스 문제를 명확히 정의해야 한다.

**가설 검정 프레임**

귀무가설 H0: 실험군과 대조군 평균 차이가 없다 (μ₁ = μ₂)
대립가설 H1: 실험군과 대조군 평균 차이가 있다 (μ₁ ≠ μ₂, 양측)
예: 광고 노출 영역에 따른 매출 변화 검증

α (Type 1 Error): 
  효과 없는데 있다고 잘못 판단할 확률 (보통 0.05)

β (Type 2 Error):
  효과 있는데 없다고 잘못 판단할 확률 (보통 0.2)

Power = 1 - β:
  효과 있을 때 탐지할 확률 (보통 0.8)
  
|  | 실제 효과 없음 (H0 참) | 실제 효과 있음 (H1 참) |

|---|---|---|

| 효과 없음이라고 판단 (H0 채택) | True Negative | False Negative (β) |

| 효과 있다고 판단 (H0 기각) | False Positive (α) | True Positive (Power = 1-β) |

### 해석

- Type I Error (α): 효과가 없는데 있다고 판단

- Type II Error (β): 효과가 있는데 없다고 판단

- Power (1-β): 효과가 있을 때 이를 탐지할 확률

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

주로 사용하는 경우: 누적 지표 (retention, 사용 건수), 1 user = 1 obs

- Retention
- Conversion
- Subscription

### Item-level

아이템 단위 랜덤화

주로 사용하는 경우: 광고/캠페인 단위 CTR 비교, 1 item = 1 obs

- 추천 시스템
- 광고 소재 비교
- 캠페인 비교

### Auction-level

입찰 단위 랜덤화: 입찰 과정 개선, 1 경매 = 1 obs

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
**공식**: 두 그룹 비율 차이의 감지 한계

```
δ = √(p(1-p) × (1/n_A + 1/n_B)) × (Z_α/2 + Z_β)
```

- `p`: 합쳐진 베이스라인 해지율 (예: 8.94%)
- `n_A`, `n_B`: 두 그룹의 표본 수 (그룹간 동일 크기면 대칭, 다르면 비대칭)
- `Z_α/2`: 유의수준 (α=0.05 양측 → 1.96)
- `Z_β`: 검정력 (β=0.20 → 0.84)
- 합성 상수 `(Z_α/2 + Z_β)² ≈ 7.84`

**비대칭 비교의 감지력 향상 원인**: 큰 그룹 쪽의 `1/n` 항이 작아져 합산 분산이 줄어듦.
```
각 부분의 의미:
  - Z_α/2 = 1.96: 검정 통계량의 H0 분포에서 양 끝 2.5%
  - Z_β = 0.84: 검정 통계량의 H1 분포에서 한쪽 20%
  - 두 그룹 분산의 합
  - MDE의 제곱

MDE-N 관계:
  N ∝ 1/MDE² 
  → MDE가 감소할 때마다 필요한 샘플은 제곱으로 폭증
  → 작은 효과 탐지가 비싼 이유

[Sample Size에 영향을 주는 요소]

1. 분산 (σ²)
   - 분산 ↑ → N ↑ (선형)
   - 지표의 자연 변동성

2. 효과 크기 (MDE)
   - MDE ↓ → N ↑↑ (제곱)
   - 예: MDE 절반 → N 4배
   - 작은 효과 탐지가 가장 비쌈

3. Baseline Rate (이진 변수)
   - p(1-p) 분산 결정
   - 50% 근처에서 분산 최대
   - 상대 MDE 사용 시 baseline 작을수록 N 증가

4. α (유의수준)
   - α ↓ (엄격) → Z_α/2 ↑ → 【N 증가】
   - 0.05 → 0.01: N 약 49% 증가
   - 0.05 → 0.001: N 약 2배

5. Power (1-β)
   - Power ↑ → Z_β ↑ → 【N 증가】
   - 0.8 → 0.9: N 약 34% 증가

6. 단측 vs 양측 검정
   - 양측 (표준): Z_α/2 = 1.96
   - 단측: Z_α = 1.645 (약 16% 감소)
   - N 약 20% 감소 (절반 X)
   - 단, 강한 이론적 근거 필요

7. 다중 비교 (Multiple Testing)
   - 검정 m개 시 false positive 폭증
   - 보정 방법:
     a) Bonferroni: α_각각 = α/m (엄격, 단순)
     b) FDR: Benjamini-Hochberg (덜 보수적, 대량 비교에 적합)
   - 보정 시 α 작아져 N 증가

8. Allocation Ratio
   - 50:50이 가장 효율적
   - 불균등 배정 시 N 증가

9. Clustering / Non-independence
   - 같은 user 반복 측정, 지역 클러스터 등
   - Design Effect (DEFF) = 1 + (m-1)×ICC
   - N_adjusted = N × DEFF

10. Drop-out / Attrition
    - 이탈률 r% 예상 시
    - N_inflated = N / (1-r)

11. Variance Reduction 기법
    - CUPED: 분산 20~50% 감소
    - Stratification: 사전 균형으로 분산 감소
    - N 감소 효과

12. Heterogeneous Treatment Effect (HTE)
    - 세그먼트별 분석 필요 시
    - 세그먼트당 충분 N 확보
    - 전체 N 증가
즉,

- 분산이 커질수록 필요한 샘플 증가
- 탐지하려는 효과가 작을수록 샘플 급증

---

## 5. 트래픽 배정 (Traffic Allocation)

### 기본

50 : 50

가장 통계적으로 효율적

### 위험도가 높은 실험

- 점진적 롤아웃

```text
1% > 5% > 10% > 50% > 100%
```

---

## 6. Hash Bucketing

- 대부분의 온라인 실험은 Hash 기반 랜덤화를 사용한다.
- 같은 사용자 항상 같은 그룹 (결정적)
- user_id + experiment_name으로 salt
- MD5: 16진수 → 10진수 → % 10000 → bucket (0~9999)
- 10000 bucket으로 세밀한 분할 가능

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
Chi-square test
  H0: 분포 같음 (SRM 없음)
  H1: 분포 다름 (SRM 있음)
  p < 0.001 (일반 0.05보다 엄격) → H0 기각 → SRM
```
원인:
  - 트래픽 라우팅 이슈 (배정/전달 파이프라인)
  - 측정 누락 (MAR/MNAR, 그룹별 누락률 차이)
  - 봇 필터링, bucketing 버그 등


#### 대응

SRM이 발생한 실험은 분석하지 않는다. 원인을 수정한 뒤 재실험 수행

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
