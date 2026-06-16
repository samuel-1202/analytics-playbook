# Metric Design

## 개요

데이터 분석 및 실험의 품질은 통계 기법보다 어떤 지표(Metric)를 정의했는가에 의해 결정되는 경우가 많다.

잘못된 지표를 선택하면 실험 결과가 유의하더라도 비즈니스 가치가 없는 의사결정으로 이어질 수 있다.

따라서 실험 설계 단계에서는

- 무엇을 개선할 것인가?
- 어떤 위험을 방어할 것인가?
- 어떤 부작용을 모니터링할 것인가?

를 명확히 정의해야 한다.

---

# 1. Metric Hierarchy

실험 지표는 일반적으로 다음과 같이 구성된다.

```text
Primary Metric
↓
Guardrail Metric
↓
Secondary Metric
```

---

# 2. Primary Metric

## 정의

실험의 성공 여부를 판단하는 핵심 지표

---

## 특징

- 하나 또는 소수
- 실험 목표와 직접 연결
- 의사결정 기준

---

## 예시

### CRM

```text
결제 전환율
```

---

### 추천 서비스

```text
스트리밍 전환율
```

---

### 광고

```text
CTR
ROAS
```

---

## 좋은 Primary Metric 조건

- 비즈니스 목표와 연결
- 측정 가능
- 조작하기 어려움
- 이해하기 쉬움

---

# 3. Guardrail Metric

## 정의

Primary Metric 개선 과정에서

서비스 품질이 악화되지 않도록 보호하는 지표

---

## 목적

```text
"좋아지면 안 되는 것이 나빠지는지 확인"
```

---

## 예시

### 광고 노출 증가

Primary

```text
광고 수익
```

Guardrail

```text
사용자 이탈률
스트리밍 건수
페이지 로딩 시간
```

---

### CRM 프로모션

Primary

```text
구매 전환율
```

Guardrail

```text
신규 상품 구매 건수
전체 매출
환불률
```

---

## 특징

Primary보다 중요할 수도 있음

---

## 판단 예시

```text
전환율 +5%

하지만

이탈률 +30%

→ 실패
```

---

# 4. Secondary Metric

## 정의

실험 효과를 이해하기 위한 보조 지표

---

## 목적

효과 발생 원인 파악

---

## 예시

Primary

```text
결제 전환율
```

Secondary

```text
방문일수
페이지뷰
클릭률
```

---

## 특징

성공 여부 판단 기준은 아님

---

# 5. Leading Metric

## 정의

최종 성과보다 먼저 반응하는 선행 지표

---

## 예시

최종 목표

```text
구독 유지율
```

Leading Metric

```text
방문 빈도
콘텐츠 소비량
즐겨찾기 수
```

---

## 장점

빠른 피드백 가능

---

## 단점

최종 성과와 연결성 검증 필요

---

# 6. Lagging Metric

## 정의

최종 비즈니스 성과를 나타내는 후행 지표

---

## 예시

```text
LTV
Retention
매출
구독 유지율
```

---

## 특징

의미는 크지만 반응이 느림

---

# 7. North Star Metric

## 정의

서비스의 장기적인 성장을 가장 잘 설명하는 핵심 지표

---

## 예시

### Spotify

```text
Listening Hours
```

---

### Netflix

```text
Watch Time
```

---

### Airbnb

```text
Booked Nights
```

---

## 특징

서비스 전체 방향성을 설명

---

# 8. Composite Metric

## 정의

여러 지표를 결합하여 만든 지표

---

## 예시

추천 서비스

```text
0.4 × CTR

+

0.3 × Stream

+

0.3 × Retention
```

---

## 장점

다차원 성과 반영

---

## 단점

설명력이 떨어질 수 있음

---

# 9. Metric Trade-off

## 정의

한 지표가 좋아질 때 다른 지표가 나빠지는 현상

---

## 예시

광고 증가

```text
광고 수익 증가

↓

사용자 경험 감소
```

---

### CRM

```text
전환율 증가

↓

마진 감소
```

---

## 대응

Guardrail Metric 설정

---

# 10. Metric Selection Framework

## Step 1

비즈니스 목표 정의

```text
무엇을 개선할 것인가?
```

---

## Step 2

Primary Metric 선정

```text
성공 기준
```

---

## Step 3

Guardrail Metric 선정

```text
사업 방어 지표
```

---

## Step 4

Secondary Metric 선정

```text
원인 분석 지표
```

---

## Step 5

Leading / Lagging 연결

```text
장기 성과 연결성 검증
```

---

# CRM 사례

## Primary Metric

```text
구매 전환율
```

---

## Guardrail Metric

```text
신규 상품 구매 건수
전체 매출
환불률
```

---

## Secondary Metric

```text
서비스 방문일
스트리밍 사용자 수
로그인율
```

---

# 추천 서비스 사례

## Primary Metric

```text
스트리밍 전환율
```

---

## Guardrail Metric

```text
사용자 이탈률
재방문율
```

---

## Secondary Metric

```text
홈탭 클릭률
플레이리스트 진입률
```

---

# 뮤직웨이브 사례

## 문제

기존 KPI

```text
방문자 수
스트리밍 수
```

만으로 성과 설명 어려움

---

## 신규 지표 정의

```text
댓글 참여율
채팅 참여율
이벤트 참여율
팬덤 활성도
```

---

## 결과

참여형 지표가

스트리밍 및 재방문을 설명하는

Leading Metric으로 확인

---

# Interview Questions

### Q. Primary Metric과 Guardrail Metric 차이는?

Primary는 성공 여부 판단 지표이고,

Guardrail은 실험 과정에서 서비스 품질 악화를 방어하기 위한 지표입니다.

---

### Q. Leading Metric과 Lagging Metric 차이는?

Leading Metric은 빠르게 반응하는 선행 지표,

Lagging Metric은 최종 비즈니스 성과를 나타내는 후행 지표입니다.

---

### Q. Guardrail Metric은 어떻게 선정하나요?

실험이 성공하더라도 절대 악화되면 안 되는 지표를 선택합니다.

예를 들어 광고 실험이라면 광고 수익보다 사용자 이탈률이 더 중요할 수 있습니다.

---

### Q. 좋은 KPI의 조건은?

- 비즈니스 목표와 연결
- 측정 가능
- 이해하기 쉬움
- 조작하기 어려움
- 장기 성과와 연결
