# SUTVA and Interference

## 개요

A/B 테스트는 일반적으로 각 사용자가 독립적으로 행동한다는 가정 하에 수행된다.

하지만 실제 서비스 환경에서는 사용자 간 상호작용, 네트워크 효과, 학습 효과 등으로 인해 이러한 가정이 깨질 수 있다.

이를 이해하지 못하면 효과를 과대 또는 과소 추정할 수 있으며, 실험 결과의 인과적 해석이 어려워진다.

---

# 1. SUTVA

## 정의

SUTVA

**Stable Unit Treatment Value Assumption**

실험 단위(Unit)의 결과는

- 자신의 Treatment 상태에만 영향을 받고
- 다른 Unit의 Treatment 상태에는 영향을 받지 않는다는 가정

---

## 의미

사용자 A가 Treatment를 받았다고 해서

사용자 B의 행동이 변하면 안 된다.

즉,

```text
User A
↓
Treatment

User B
↓
영향 없음
```

이어야 한다.

---

## 왜 중요한가?

대부분의 A/B 테스트는

```text
Treatment 효과
=
Treatment 그룹 평균
-
Control 그룹 평균
```

으로 계산된다.

하지만 사용자 간 영향이 존재하면

순수한 Treatment 효과를 추정할 수 없게 된다.

---

# 2. Interference

## 정의

한 사용자의 Treatment가 다른 사용자의 결과에 영향을 주는 현상

즉,

SUTVA 위반 사례

---

## 예시

SNS 공유 기능 실험

```text
Treatment User
↓
게시물 공유
↓
Control User 유입
```

Control User도 간접적으로 영향을 받게 됨

---

## 문제점

Treatment 효과가

- 과대 추정
- 과소 추정

될 수 있음

---

# 3. Network Effect

## 정의

사용자 간 연결(Network)을 통해 효과가 전파되는 현상

---

## 예시

### SNS

친구 초대 기능

### 메신저

새 기능 공유

### 커뮤니티

추천 게시글 확산

---

## 특징

네트워크가 강할수록

SUTVA 위반 가능성 증가

---

# 4. Cluster Randomization

## 정의

상호작용 가능성이 높은 사용자들을 하나의 Cluster로 묶고

Cluster 단위로 랜덤 배정하는 방법

---

## 목적

Interference 최소화

---

## 예시

### 학교

```text
학교 A
↓
Treatment

학교 B
↓
Control
```

---

### 지역

```text
서울
↓
Treatment

부산
↓
Control
```

---

## 장점

- SUTVA 위반 감소
- Spillover 감소

---

## 단점

- 필요한 샘플 증가
- 통계적 Power 감소

---

# 5. Geo Experiment

## 정의

지역(Geo)을 Cluster로 사용하여 실험하는 방법

---

## 활용 사례

광고 플랫폼

배달 서비스

오프라인 프로모션

---

## 예시

```text
서울
→ Treatment

부산
→ Control
```

---

## 장점

현실적인 운영 가능

---

## 단점

지역별 특성 차이 존재

---

# 6. Switchback Experiment

## 정의

사용자가 아닌 시간 단위로 Treatment를 교차 적용하는 방법

---

## 활용 사례

Marketplace

배달 플랫폼

실시간 광고 시스템

---

## 예시

```text
10:00~11:00
Treatment

11:00~12:00
Control

12:00~13:00
Treatment
```

---

## 장점

사용자 간 간섭 감소

---

## 단점

시간 효과(Time Effect) 존재

---

# 7. Ego Cluster

## 정의

소셜 네트워크 중심 사용자(Ego)를 기준으로 연결된 사용자들을 함께 묶어 배정하는 방법

---

## 활용 사례

SNS

친구 추천

팔로우 추천

---

## 예시

```text
User A
├─ Friend B
├─ Friend C
└─ Friend D

전체를 하나의 Cluster로 배정
```

---

# 8. Learning Effect

## 정의

사용자가 새로운 기능에 적응하면서 행동이 변화하는 현상

---

## 예시

새 UI 배포

```text
Day 1
불편

Day 14
적응 완료
```

---

## 문제점

초기 성과가 실제 성과보다 낮게 측정될 수 있음

---

# 9. Novelty Effect

## 정의

새로운 기능이라는 이유만으로 일시적인 반응 증가가 발생하는 현상

---

## 예시

새 추천 기능 출시

```text
1주차
CTR 급증

4주차
원래 수준으로 회귀
```

---

## 문제점

초기 성과 과대평가

---

# 10. Learning Effect vs Novelty Effect

| 구분 | Learning Effect | Novelty Effect |
|--------|--------|--------|
| 원인 | 사용자의 적응 | 신기함 |
| 초기 반응 | 낮음 | 높음 |
| 장기 추세 | 증가 | 감소 |
| 위험 | 효과 과소평가 | 효과 과대평가 |

---

# 11. 대응 방법

## 충분한 실험 기간 확보

권장

```text
최소 2주 ~ 4주
```

---

이유

- Weekly Cycle 반영
- Learning Effect 반영
- Novelty Effect 완화

---

## 시간 추세 분석

예시

```text
Day별 CTR
Day별 Conversion
Day별 Retention
```

추적

---

## 안정화 구간 확인

예시

```text
1주차 제외
2~4주차만 분석
```

---

# 실무 체크리스트

## 실험 설계

- [ ] 사용자 간 간섭 가능성 존재?
- [ ] 네트워크 효과 존재?
- [ ] Cluster Randomization 필요?
- [ ] Geo Experiment 가능?

---

## 실험 운영

- [ ] 최소 2주 이상 운영
- [ ] 주 단위 주기 반영
- [ ] Novelty Effect 모니터링
- [ ] Learning Curve 모니터링

---

## 결과 해석

- [ ] SUTVA 위반 가능성 검토
- [ ] Spillover 영향 검토
- [ ] 시간 추세 분석 수행

---

# Interview Questions

### Q. SUTVA란 무엇인가요?

각 실험 단위의 결과가 자신의 Treatment에만 영향을 받고, 다른 사용자의 Treatment 상태에는 영향을 받지 않는다는 가정입니다.

---

### Q. SNS 추천 기능 실험에서 어떤 문제가 발생할 수 있나요?

사용자 간 공유와 확산으로 인해 Control 사용자가 Treatment의 영향을 받을 수 있습니다.

즉, SUTVA가 위반될 수 있습니다.

---

### Q. 이런 문제를 어떻게 해결하나요?

Cluster Randomization, Geo Experiment, Ego Cluster 등을 사용하여 상호작용이 높은 사용자들을 동일한 그룹으로 배정합니다.

---

### Q. 왜 실험 기간을 최소 2주 이상 가져가나요?

요일 효과를 반영하고,

Learning Effect와 Novelty Effect가 안정화될 시간을 확보하기 위해서입니다.
