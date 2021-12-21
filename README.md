# 하이브리드 추천시스템

## 하이브리드 추천시스템이란?

- content-based, Collaborative Filtering의 장점을 합치면?
- 다양한 데이터/알고리즘을 조합
- 여러 추천 알고리즘의 장점만 모아서 고성능/뛰어난 시스템을 만드는 방식

- content-based, CF
  - 서로 잘 하는 특징들을 바탕으로 하나의 시스템을 만드는 방법

### 다양한 하이브리드 방법들

- Weighted Ensemble
  - 여러 모델의 추천 결과를 하나로 합쳐서 최종 추천 아이템을 정하는 방식
  - 어떻게 가중치를 주느냐?
    - 개별 모델을 testset으로 테스트 해 본 이후 그 성능을 기준으로 가중치를 주는 방법도 있을 수 있음
    - 그 이외 여러가지 방법이 있을 수 있음
- Mixed
  - 특징이 다른 여러 추천 알고리즘을 활용하고, 알고리즘의 추천 결과를 모두 보여주는 방식
  - CF, user-based, content-based 등을 모두 보여줌
  - 특징이 다른 알고리즘을 모두 사용하여 다양성 확보 가능
- Switch
  - 사용자/서비스 상태 등 특정 상황을 고려하여 추천 결과를 선택적으로 보여주는 방식
  - ex) 모바일일 경우 content-based, pc의 경우 CF
  - ODK의 경우 추천이 필요한 섹션별로 적절한 알고리즘을 사용하는 방식으로 사용 가능
- Feature Combination
  - 보유한 데이터로 얻을 수 있는 다양한 feature를 모두 조합하여 추천 알고리즘을 학습
- Meta-Level
  - 여러 추천 알고리즘을 활용
  - 첫 번째 모델이 다음 모델의 input이 되어 서로가 서로의 정보를 학습
  - 다음 모델은 조금 더 나은 방법이 될 수 있음

위 방법들 이외에 상황에 맞춰서 다양한 방식으로 하이브리드가 가능

### 하이브리드 모델 평가

- 서비스 관점

  - 비즈니스 KPI 달성 여부
  - online/offline 평가: 추천 적용 이후 좋아졌는지

- 모델 성능 관점
  - 수치화된 점수들이 기존 모델보다 하이브리드 이후에 더 좋아졌는지
  - 여러가지 다양한 방법을 가치로 평가를 하는게 중요

# Context-Aware 추천시스템

> "맥락 기반의 추천 시스템"

지금까지의 추천 시스템의 틀

- user, item이 제공한 explicit한 데이터 활용
- 위 데이터가 부족하여 유저가 흘리고 간 힌트 (implicit)을 찾으려고 애씀
- user-item 간의 관계, 특징을 활용
- 이 데이터로 유저/아이템 행렬을 만드는 것이 특징
- 행렬을 바탕으로 평점/랭킹을 추천

item/user와 직접적으로 관련된 위 틀을 벗어날 수는 없을까 하는 질문에서 나온 것이 `Context-Aware 추천 시스템`

---

- Context = 맥락
  - 맥락을 이해한다 = 유저의 상황을 이해한다
  - user, item과 관련은 있는 상황
  - interaction (상호관계)를 설명하지는 않음
  - ex) 시간정보, 위치정보
- Context-aware 추천 시스템
  - user, item의 상호관계 뿐 아니라 context (상황 정보)도 포괄한 추천시스템

### Context-aware 추천시스템 예시

- 시간 추천 추천
  - 기존 방법
    - 유저의 성향과 뉴스 컨텐츠의 유사도 등을 판단하여 추천
    - ex) "스포츠신문 - 야구" 를 보는 사람은 해당 내용을 바탕으로 추천
  - 맥락기반 방법
    - context(시간대)에 맞는 내용을 추천
    - ex) 월요일(시간)에 뉴스를 읽는다면 한 주를 시작할 때 날씨 등 관심갖을 내용을 추천
  - ODK에서도 활용할 여지가 높아 보임
    - 컨텐츠 소비 시간/형태에 따른 context를 사용할 수 있음
- 장소 추천
  - 기존 방법
    - 유저가 좋아하는 취향, 과거 선택한 아이템을 기반으로 추천
  - 맥락기반 방법
    - 가고자하는 시간 정보 등을 바탕으로 추천
    - ex) 유저가 술집, 이자까야 등을 좋아한다고 해도 점심시간에는 이에 맞는 다른 식당을 추천

![image](https://user-images.githubusercontent.com/23415251/146983372-b0fc5d8f-ca24-4d3a-93d2-12a99711ce14.png)

- 왜 추천을 했는지에 대한 설명을 context를 통해 할 수 있음

## Model Structure

- 기존 방법
  - R: user x item -> rating
- context-aware 추천 시스템
  - R: user x item x context -> rating
  - 3D의 행렬을 만들어야 함
  - 다른 context가 들어오면 Dimension이 추가되어 M dimensional한 다차원의 데이터

## Context-aware 추천시스템 정리

- 다양한 상황의 많은 context 정보 활용 가능
  - user, item의 정보는 한정적임
  - 시간/장소 정보 등의 메타정보 (판매자, 대표키워드, 태그 등) 활용 가능
  - 다른 많은 것을 cover 할 수 있는 추천 시스템 생성 가능
- Context 정보를 얻는 방법이 다양
  - explicit, implicit 한 방법 모두 사용 가능 (ex. 평점, log 등)
  - 접속한 기기, 이벤트 정보, 날씨 등
- 적절한 context 정보로 초기 filtering 가능
  - 이미 알고있는 context를 바탕으로 더 정확한 추천이 가능
- context 정보를 활용하여 A/B test 등 실험 가능
- 도메인 지식을 더욱 잘 활용할 수 있는 방법
  - 데이터의 중요한 상황적인 특징을 알고있다면 더 좋은 시스템을 만들 수 있음
  - ODK의 경우 어떤 context가 잘 활용될 수 것인지에 대한 논의가 있으면 좋을것 같음

# Contextual Pre-filtering & Contextual Post-filtering

- context를 활용한 여러 방법들 중에 대표적인 방법들을 소개
- 다양한 방법으로 다양한 context를 사용할 수 있겠구나 하고 이해하는 정도로

## Spotify 예시

![image](https://user-images.githubusercontent.com/23415251/146984585-ba3e6cec-f009-4050-b2bc-b6121e3534f5.png)

- daily, weekly 등 상황에 따라 다양한 추천을 함
  - weekday, weekend 등에 따라, 상황에 따라 추천

## Contextual factors

- explicit하거나 implicit 할 수 있음
  - fully observable
  - partially observable
  - unobservable
- 시간의 흐름에 따라 변하거나 그대로일 수 있음
  - static
  - dynamic

factor들의 특징에 따라서 참고

## Contextual information 적용

1. Contextual Pre-filtering

- context 정보를 활용하여 처음 데이터를 filtering
- 추천 시스템을 적용하기 이전에 데이터셋을 필터링 한 후 적용

2. Contextual Post-filtering

- 모델링을 먼저 진행 한 이후 context 정보를 활용하여 filtering

3. Contextual Modeling

- context 정보 자체를 모델링에 활용
- user, item 뿐 아니라 context도 데이터로 활용
- 데이터가 커지고 복잡도가 높아짐

![image](https://user-images.githubusercontent.com/23415251/146985318-6a7eb858-19fa-4b4d-ada0-be185a415666.png)

## Contextual Pre-filtering

- Main method

1. Context 정보를 활용하여 가장 관련있는 2D (users X Item) 데이터 생성
2. 이 후 다양한 추천 알고리즘을 사용

- Context는 query의 역할
  - 가장 관련있는 데이터를 선택하는 역할
- Context generalization
  - 너무 specific한 context 데이터는 충분하지 않기 때문에 sparsity 문제가 발생할 수 있음
  - ex) girlfriend, theater, saturday -> friend, weekday 등
  - context를 활용한 split 할 때 generalized 할 필요가 있음
- 많은 computation이 필요할 수 있음

## Contextual Post-filtering

- main method

1. Context 정보를 무시하고 User, item 정보로 2D 추천시스템 모델을 학습
2. 추천 결과를 context 정보를 활용하여 filter/adjust

- 유저의 특정 취향/패턴을 context를 통해 찾을 수 있음
- Heuristic approch
  - 주어진 context로 특정 user가 관심있는 공통 item의 특징을 활용
- model-based approch
  - 주어진 context로 user가 item을 선호할 확률을 예측

## Contextual modeling

- main method

1. 모든 정보를 전부 활용
2. Predictive model 또는 Heuristic approach를 사용

- 주로 Predictive model을 contextual modeling이라고 함

  - 기존 2D에서 N-dimension 형태로 확장

- 예시
  - Context-aware SVM
  - Tensor Factorization (차원수가 다른 데이터로 모델링)
  - Factorization Machine: part 4에서 다룰 예정

# LARS: Location-aware 위치기반 추천알고리즘 소개

- location보다 context에 초점을 맞춰서 봄
- 이후에 추천 시스템을 만들 때 location context를 사용하고자 하면 해당 논문을 깊이있게 보면 좋을듯

## Abstract & Instruction

- location-based rating을 써서 추천
- 3가지 종류의 location-based ratings 사용: 이후에 다룸

## Motivation

- 미국의 주 별로 선호하는 영화 장르가 다름
  - ex) 멀리 떨어져 있는 미네소타, 위스콘신 / 플로리다

![image](https://user-images.githubusercontent.com/23415251/146990425-059dea88-2490-4171-80e6-7ae12a90d4fc.png)

## Contribution

저자가 얘기하고자 하는 것들

1. 3가지 종류의 location-based ratings 제공

- Spatial ratings for non-spatial items
- Non-spatial rating for spatial items
- Spatial ratings for spatials items

2. 위 3가지 종류에 사용한 LARS 제안 (기법)
3. 다양한 실험으로 더 나은 성능을 냈다

## LARS overview

1. LARS Query model

- input: User id `U`, numeric limit `K`, location `L`
- output: `K`개의 추천되는 아이템
- Snapshot & continuous query
  - context의 특성에 따른 쿼리
  - location이 바뀌면 continuous 한 쿼리
  - user는 위치가 변함에 따라 업데이트된 추천을 받음

2. Item-based Collaborative Filtering

- 저자들은 이유를 popularity하기 때문이라고 헀다고 함
  - context를 최대한 드러내기 위해 다른 부분은 simple하게 갔다고 추측 (강사피셜)

## Spatial user ratings for non-spatial items

1. user의 출신/지역에 따라 선호도가 다르다
2. 3가지 requirement

- Locality
  - user와 가까이 있는 추천을 해야함
  - local한 정보를 바탕으로
- Scalability
  - 추천 과정에서 데이터 구조가 더 많은 user로 확장 가능해야함
  - 데이터 확장 및 user 추가를 위해
- Influence
  - spatial neighborhood의 크기를 조절해서 추천해야함
  - user size가 커지거나 적어져도 해당 추천 알고리즘을 적용

위 방법들을 달성하기 위한 방법으로 저자들이 사용한게 user partitioning 기법

3. User partitioning 기법 (피라미드 구조)

## Non-spatial user ratings for spatial items

아이템이 spatial한 정보를

1. user, rating, item, ilocation
2. Travel Locality

- 이동 거리가 user의 선택에 영향이 있다는 가정
- 추천 점수에 travel penalty를 부여
- ex) 똑같은 4점이라도 거리에 따라서 가까운 곳에 가중치를 부여

3. Travel Penalty

travel penalty를 주기 위해 계산을 할 때 계산량이 많아짐

## Experiments

저자들이 돌린 실험 결과를 바탕으로 얻은 인사이트

![image](https://user-images.githubusercontent.com/23415251/146992415-612f6e56-5784-401f-b564-54a5aea092c1.png)

1. (a)

- 기본보다는 travel penalty를 준 게 좋다
- user partitioning, location-based ratings가 효과가 있음
- 저자들이 만든 LARS가 제일 좋음k

2. (b)

- MovieLens 데이터 (user-item에 집중된 데이터)에 진행해도 효과가 있었음
- LARS-U가 효과가 있음

## Experiments 2

한개만 추천하는건 의미가 적기 때문에 K값이 증가해도 동일한 효과가 있는지를 실험

![image](https://user-images.githubusercontent.com/23415251/146992575-323d0362-d470-45fe-b7f1-0664e545e4ee.png)

- LARS, LARS-U 모두 품질 증가
- 대부분의 k에서 CF에 비해 2배 이상 나은 효과
- MovieLens 데이터셋도 동일한 지표

## Conclusion

1. location을 활용한 추천 시스템을 제안
2. location-based rating을 3가지 형태로 구분해서 제안
3. 2가지 기법을 도입: user partitioning, travel penalty
4. 2가지 기법을 함께/따로 사용 가능
5. 여러 실험을 통해 LARS가 efficient, scalable, better quality임을 증명
