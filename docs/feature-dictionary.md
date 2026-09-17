# 피처 딕셔너리

스파이크에서 쓴 피처들의 이름과 의미를 정리한 문서. GMC와 Home Credit 각각 원본 컬럼, Home Credit은 추가로
직접 만든 파생(조인) 피처까지 다룬다. 원-핫 인코딩으로 전개된 최종 234개/260개 컬럼은 여기 낱개로 나열하지
않는다(아래 "원-핫 전개" 절 참고 — 기계적으로 생성되는 것이라 정적 문서보다 코드에서 바로 뽑는 게 정확하다).

## 1. GMC — 원본 10개 (전부 수치형)

`docs/project-plan.md` 2-1절과 동일. 출처: Kaggle Give Me Some Credit.

| 컬럼명 | 의미 |
|---|---|
| `SeriousDlqin2yrs` | 타깃. 향후 2년 내 90일 이상 연체 여부 |
| `RevolvingUtilizationOfUnsecuredLines` | 무담보 리볼빙 한도 사용률 |
| `age` | 나이 |
| `NumberOfTime30-59DaysPastDueNotWorse` | 30~59일 연체 횟수 |
| `DebtRatio` | 월 부채 상환액 / 월소득 |
| `MonthlyIncome` | 월소득 |
| `NumberOfOpenCreditLinesAndLoans` | 보유 대출·신용한도 건수 |
| `NumberOfTimes90DaysLate` | 90일 이상 연체 횟수 |
| `NumberRealEstateLoansOrLines` | 부동산 담보 대출 건수 |
| `NumberOfTime60-89DaysPastDueNotWorse` | 60~89일 연체 횟수 |
| `NumberOfDependents` | 부양가족 수 |

## 2. Home Credit — `application_train.csv` 원본 120개

출처: `data/raw_home_credit/HomeCredit_columns_description.csv`(Kaggle 공식 데이터 사전). 전 컬럼 설명이 존재함(결측 0건, 2026-09-17 확인).
아래는 3개 그룹으로 나눠 정리했다 — 같은 패턴이 반복되는 두 그룹(건물 정보, 서류 제출 플래그)은 요약하고,
개별로 의미가 다른 53개는 전부 나열한다.

### 2-1. 건물/주거 정보 (47개) — 패턴 반복 그룹

`APARTMENTS_AVG/MODE/MEDI`, `BASEMENTAREA_*`, `COMMONAREA_*`, `ELEVATORS_*`, `ENTRANCES_*`, `FLOORSMAX_*`,
`FLOORSMIN_*`, `LANDAREA_*`, `LIVINGAPARTMENTS_*`, `LIVINGAREA_*`, `NONLIVINGAPARTMENTS_*`, `NONLIVINGAREA_*`,
`YEARS_BEGINEXPLUATATION_*`, `YEARS_BUILD_*`, `TOTALAREA_MODE`, 그리고 범주형 `FONDKAPREMONT_MODE`,
`HOUSETYPE_MODE`, `WALLSMATERIAL_MODE`, `EMERGENCYSTATE_MODE`.

공식 설명(공통): "대출자가 사는 건물에 대한 정규화된 정보 — 평균(`_AVG`), 최빈값(`_MODE`), 중앙값(`_MEDI`) 접미사로
아파트 크기, 공용면적, 거주면적, 건물 연식, 엘리베이터/출입구 수, 건물 상태, 층수 등을 나타냄."

**결측률이 50~70%대로 가장 높은 그룹**(1단계 스파이크 2절 참고) — Phase 5에서 결측 처리 방침을 따로 정해야 한다.

### 2-2. 서류 제출 플래그 (20개) — 패턴 반복 그룹

`FLAG_DOCUMENT_2` ~ `FLAG_DOCUMENT_21` (모두 수치형, 0/1)

공식 설명(공통): "대출자가 서류 N을 제출했는지 여부." 어떤 서류가 무엇인지 구체적 항목은 공개 안 됨(번호로만 구분).
1단계 스파이크에서 `FLAG_DOCUMENT_3`가 중요도 상위권에 있었다.

### 2-3. 나머지 53개 — 개별 의미

| 컬럼명 | 타입 | 의미 |
|---|---|---|
| `CODE_GENDER` | 범주형 | 성별 |
| `FLAG_OWN_CAR` | 범주형 | 자가용 소유 여부 |
| `FLAG_OWN_REALTY` | 범주형 | 주택/부동산 소유 여부 |
| `NAME_CONTRACT_TYPE` | 범주형 | 대출 유형(일시상환 현금대출 / 리볼빙) |
| `NAME_EDUCATION_TYPE` | 범주형 | 최종 학력 |
| `NAME_FAMILY_STATUS` | 범주형 | 결혼 상태 |
| `NAME_HOUSING_TYPE` | 범주형 | 주거 형태(임차/자가/부모님과 거주 등) |
| `NAME_INCOME_TYPE` | 범주형 | 소득 유형(사업가/근로자/육아휴직 등) |
| `NAME_TYPE_SUITE` | 범주형 | 신청 시 동행인 |
| `OCCUPATION_TYPE` | 범주형 | 직업 |
| `ORGANIZATION_TYPE` | 범주형 | 근무처 업종(58개 범주 — 원-핫 전개 시 컬럼 수 급증의 주범) |
| `WEEKDAY_APPR_PROCESS_START` | 범주형 | 신청 요일 |
| `AMT_ANNUITY` | 수치형 | 대출 연금(월 상환액) |
| `AMT_CREDIT` | 수치형 | 대출 금액 |
| `AMT_GOODS_PRICE` | 수치형 | 소비재 대출 시 구매 물품 가격 |
| `AMT_INCOME_TOTAL` | 수치형 | 총소득 |
| `AMT_REQ_CREDIT_BUREAU_HOUR` | 수치형 | 신청 1시간 전까지의 신용조회 건수 |
| `AMT_REQ_CREDIT_BUREAU_DAY` | 수치형 | 신청 1일 전까지의 신용조회 건수 |
| `AMT_REQ_CREDIT_BUREAU_WEEK` | 수치형 | 신청 1주 전까지의 신용조회 건수 |
| `AMT_REQ_CREDIT_BUREAU_MON` | 수치형 | 신청 1개월 전까지의 신용조회 건수 |
| `AMT_REQ_CREDIT_BUREAU_QRT` | 수치형 | 신청 3개월 전까지의 신용조회 건수 |
| `AMT_REQ_CREDIT_BUREAU_YEAR` | 수치형 | 신청 1년 전까지의 신용조회 건수 |
| `CNT_CHILDREN` | 수치형 | 자녀 수 |
| `CNT_FAM_MEMBERS` | 수치형 | 가족 구성원 수 |
| `DAYS_BIRTH` | 수치형 | 신청 시점 기준 나이(일 단위, 음수) |
| `DAYS_EMPLOYED` | 수치형 | 현 직장 근속 시작일(일 단위, 음수). **365243은 무직/연금생활자 placeholder 코드값**(1단계 스파이크 2절) |
| `DAYS_ID_PUBLISH` | 수치형 | 신분증 갱신 후 경과일 |
| `DAYS_LAST_PHONE_CHANGE` | 수치형 | 전화번호 변경 후 경과일 |
| `DAYS_REGISTRATION` | 수치형 | 주소 등록 변경 후 경과일 |
| `DEF_30_CNT_SOCIAL_CIRCLE` | 수치형 | 대출자 지인 중 30일 연체(DPD) 경험자 수 |
| `DEF_60_CNT_SOCIAL_CIRCLE` | 수치형 | 대출자 지인 중 60일 연체(DPD) 경험자 수 |
| `OBS_30_CNT_SOCIAL_CIRCLE` | 수치형 | 대출자 지인 중 30일 연체 관측 대상 수 |
| `OBS_60_CNT_SOCIAL_CIRCLE` | 수치형 | 대출자 지인 중 60일 연체 관측 대상 수 |
| `EXT_SOURCE_1` | 수치형 | **외부 출처 정규화 점수 (블랙박스 — 출처/산출방식 비공개)** |
| `EXT_SOURCE_2` | 수치형 | **외부 출처 정규화 점수 (블랙박스). XGBoost 중요도 1위** |
| `EXT_SOURCE_3` | 수치형 | **외부 출처 정규화 점수 (블랙박스). XGBoost 중요도 2위** |
| `FLAG_CONT_MOBILE` | 수치형 | 휴대폰 연락 가능 여부 |
| `FLAG_EMAIL` | 수치형 | 이메일 제공 여부 |
| `FLAG_EMP_PHONE` | 수치형 | 직장 전화 제공 여부 |
| `FLAG_MOBIL` | 수치형 | 휴대폰 제공 여부 |
| `FLAG_PHONE` | 수치형 | 집 전화 제공 여부 |
| `FLAG_WORK_PHONE` | 수치형 | 직장 유선전화 제공 여부 |
| `HOUR_APPR_PROCESS_START` | 수치형 | 신청 시각(시 단위) |
| `LIVE_CITY_NOT_WORK_CITY` | 수치형 | 거주지-근무지 도시 불일치 플래그 |
| `LIVE_REGION_NOT_WORK_REGION` | 수치형 | 거주지-근무지 지역 불일치 플래그 |
| `OWN_CAR_AGE` | 수치형 | 소유 차량 연식 |
| `REGION_POPULATION_RELATIVE` | 수치형 | 거주 지역의 정규화된 인구 밀도 |
| `REGION_RATING_CLIENT` | 수치형 | 거주 지역 등급(1~3) |
| `REGION_RATING_CLIENT_W_CITY` | 수치형 | 거주 지역 등급(도시 반영, 1~3) |
| `REG_CITY_NOT_LIVE_CITY` | 수치형 | 등록주소-실거주지 도시 불일치 플래그 |
| `REG_CITY_NOT_WORK_CITY` | 수치형 | 등록주소-근무지 도시 불일치 플래그 |
| `REG_REGION_NOT_LIVE_REGION` | 수치형 | 등록주소-실거주지 지역 불일치 플래그 |
| `REG_REGION_NOT_WORK_REGION` | 수치형 | 등록주소-근무지 지역 불일치 플래그 |

## 3. Home Credit — 2단계에서 직접 만든 파생(조인) 피처 26개

대출자(`SK_ID_CURR`) 단위로 보조 테이블을 집계해 만든 피처. 원본 공식 사전에는 없음 —
정의는 `ml/notebooks/spike_home_credit_stage2.ipynb` 2절 코드가 원본이다.

### `bureau.csv` + `bureau_balance.csv` 출처 (7개)

| 피처명 | 정의 |
|---|---|
| `BUREAU_COUNT` | 타 금융기관 신용조회 건수 |
| `BUREAU_ACTIVE_COUNT` | 그중 `CREDIT_ACTIVE == "Active"`(진행 중) 건수 |
| `BUREAU_CREDIT_SUM_TOTAL` | `AMT_CREDIT_SUM`(신용 총액) 합계 |
| `BUREAU_CREDIT_SUM_DEBT_TOTAL` | `AMT_CREDIT_SUM_DEBT`(현재 채무액) 합계 |
| `BUREAU_DAYS_CREDIT_MEAN` | `DAYS_CREDIT`(신용 개설 시점) 평균 |
| `BUREAU_CREDIT_DAY_OVERDUE_MAX` | `CREDIT_DAY_OVERDUE`(연체일수) 최댓값 |
| `BUREAU_BALANCE_DPD_RATIO_MEAN` | `bureau_balance.STATUS`가 연체 구간(1~5)인 월의 비율 평균 |

### `previous_application.csv` 출처 (6개)

| 피처명 | 정의 |
|---|---|
| `PREV_APP_COUNT` | Home Credit 내 과거 대출 신청 건수 |
| `PREV_APPROVED_RATIO` | `NAME_CONTRACT_STATUS == "Approved"` 비율 |
| `PREV_REFUSED_RATIO` | `NAME_CONTRACT_STATUS == "Refused"` 비율 |
| `PREV_AMT_APPLICATION_MEAN` | 신청 금액(`AMT_APPLICATION`) 평균 |
| `PREV_AMT_CREDIT_MEAN` | 승인 금액(`AMT_CREDIT`) 평균 |
| `PREV_DAYS_DECISION_MEAN` | 심사 결정 시점(`DAYS_DECISION`) 평균 |

### `POS_CASH_balance.csv` 출처 (4개)

| 피처명 | 정의 |
|---|---|
| `POS_COUNT` | POS/현금 대출 월별 기록 건수 |
| `POS_SK_DPD_MEAN` | 연체일수(`SK_DPD`) 평균 |
| `POS_SK_DPD_MAX` | 연체일수 최댓값 |
| `POS_SK_DPD_DEF_MAX` | 유예기간 초과 연체일수(`SK_DPD_DEF`) 최댓값 |

### `credit_card_balance.csv` 출처 (5개)

| 피처명 | 정의 |
|---|---|
| `CC_COUNT` | 신용카드 월별 기록 건수 |
| `CC_AMT_BALANCE_MEAN` | 카드 잔액(`AMT_BALANCE`) 평균 |
| `CC_AMT_BALANCE_MAX` | 카드 잔액 최댓값 |
| `CC_SK_DPD_MAX` | 연체일수 최댓값 |
| `CC_UTILIZATION_MEAN` | 한도 대비 사용률(`AMT_BALANCE / AMT_CREDIT_LIMIT_ACTUAL`) 평균 — GMC `RevolvingUtilization`과 동일 개념 |

### `installments_payments.csv` 출처 (4개)

| 피처명 | 정의 |
|---|---|
| `INSTALL_COUNT` | 상환 이력 건수 |
| `INSTALL_LATE_DAYS_MEAN` | 실제 납부일 - 약정 납부일(음수는 0으로 절단) 평균 — 연체 일수 |
| `INSTALL_LATE_RATIO` | 연체(`LATE_DAYS > 0`) 비율. XGBoost 중요도 4위(2단계) |
| `INSTALL_PAYMENT_RATIO_MEAN` | 실 납부액/약정 납부액(`AMT_PAYMENT / AMT_INSTALMENT`) 평균 — 과소납부 신호 |

## 4. 원-핫 전개 (234개 / 260개)

위 2절의 범주형 12개(2-3절) + 4개(2-1절의 `FONDKAPREMONT_MODE` 등)를 `pd.get_dummies(..., drop_first=True)`로
전개한 결과다. 컬럼명은 `원본컬럼명_범주값` 패턴(예: `NAME_EDUCATION_TYPE_Higher education`,
`ORGANIZATION_TYPE_Business Entity Type 3`)이며, 어떤 범주가 실제로 나타나는지에 따라 결정되므로 정적 문서로
고정하지 않는다. 필요하면 아래처럼 바로 뽑을 수 있다:

```python
# ml/notebooks/spike_home_credit_stage1.ipynb 또는 stage2.ipynb의 전처리 셀 실행 후
print(feature_cols)
```

## 5. 출처

- GMC: `docs/project-plan.md` 2-1절
- Home Credit 원본: `data/raw_home_credit/HomeCredit_columns_description.csv` (Kaggle 공식)
- Home Credit 파생 피처: `ml/notebooks/spike_home_credit_stage2.ipynb`
- 관련 분석 결과: `docs/spike-home-credit.md`
