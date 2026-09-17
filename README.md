# CreditLens

설명 가능한 개인 대출 연체 위험 예측 시스템. 금융권(카드사·증권사) 취업 포트폴리오용 1년 캡스톤 프로젝트입니다.

> 🚧 진행 중인 프로젝트입니다. 현재 Phase 0(골격 구성)와 데이터셋 전략을 검증하는 스파이크 단계까지 진행했고,
> 정식 EDA·전처리·모델링(Phase 1 이후)은 진행 예정입니다.

## 이 프로젝트가 답하려는 질문

> 데이터의 복잡도(변수 수, 테이블 구조)에 따라 선형 모델(로지스틱 회귀)과 비선형 모델(XGBoost)의 성능 격차가
> 어떻게 달라지는가? 그리고 비선형 모델을 선택해야 하는 조건에서, SHAP 기반 설명 체계가 스코어카드에 준하는
> 설명력을 확보할 수 있는가?

단순한 정형 데이터(Kaggle *Give Me Some Credit*)로는 XGBoost가 로지스틱 회귀보다 크게 나을 게 없다는 걸
스파이크로 먼저 확인했고, 이 결과를 근거로 다중 테이블·고결측·범주형 변수를 가진 더 복잡한 데이터
(Kaggle *Home Credit Default Risk*)를 추가해 같은 비교를 반복하며 답을 찾아가고 있습니다.

## 지금까지 확인한 것 (스파이크 결과 요약)

| 데이터셋 | AUC 격차(XGBoost−로지스틱) | 상위 5% 위험군 정밀도 격차 |
|---|---|---|
| GMC (변수 10개, 단일 테이블) | +0.0093 | +0.5%p |
| Home Credit 1단계 (변수 121개, 단일 테이블) | +0.0126 | +3.1%p |
| Home Credit 2단계 (다중 테이블 조인) | +0.0131 | +3.8%p |

데이터가 복잡해질수록 XGBoost의 우위가 뚜렷해집니다. 다만 Home Credit에서 가장 중요한 변수(`EXT_SOURCE_2/3`)는
정의가 공개되지 않은 외부 신용평가 점수(블랙박스)였고, 이를 제외하면 두 모델 다 AUC가 약 3%p 떨어지지만
XGBoost의 우위는 사라지지 않았습니다 — 즉 "성능과 설명 가능성" 사이의 트레이드오프를 실측으로 확인할 수 있었습니다.

상세 결과: [`docs/spike-feasibility.md`](docs/spike-feasibility.md) (GMC),
[`docs/spike-home-credit.md`](docs/spike-home-credit.md) (Home Credit)

## 데이터

- **Give Me Some Credit** (비교군): [Kaggle](https://www.kaggle.com/c/GiveMeSomeCredit) · 대출자 15만 명, 변수 10개, 단일 테이블
- **Home Credit Default Risk** (주 데이터셋): [Kaggle](https://www.kaggle.com/c/home-credit-default-risk) · 대출자 30만 명, 다중 테이블(7개), 범주형·고결측 변수 다수

두 데이터셋 모두 원천 파일은 재배포 제한 때문에 저장소에 커밋하지 않습니다. 받는 방법은 각각
[`data/raw/README.md`](data/raw/README.md), [`data/raw_home_credit/README.md`](data/raw_home_credit/README.md) 참고.

## 왜 데이터셋을 두 개 쓰는가, 왜 XGBoost인가

두 질문 모두 이 저장소 안에서 수치로 답합니다. 요약하면 — 단순한 데이터에서 XGBoost의 이득이 거의 없다는 걸
발견했고, 그걸 하나의 데이터셋만으로 결론짓지 않으려고 복잡도가 다른 데이터셋을 하나 더 추가해 비교했습니다.
자세한 배경과 의사결정 과정은 [`CLAUDE.md`](CLAUDE.md), [`docs/project-plan.md`](docs/project-plan.md)에
**확정/제안/미결정**으로 구분해 기록해뒀습니다.

## 프로젝트 구조

```
creditLens/
├── ml/                     # 데이터 탐색·전처리·모델링 (Python, uv로 관리)
│   ├── notebooks/          # EDA·스파이크 노트북
│   └── src/creditlens/     # 재사용 모듈 (Phase 2부터 채워짐)
├── backend/                # Spring Boot / Spring Batch (예정)
├── infra/                  # Docker Compose 등 인프라 (예정)
├── data/
│   ├── raw/                # Give Me Some Credit 원천 (커밋 안 됨)
│   ├── raw_home_credit/    # Home Credit Default Risk 원천 (커밋 안 됨)
│   ├── interim/, processed/
├── docs/
│   ├── project-plan.md     # Phase별 계획, 데이터 설명, 미결정 사항
│   ├── spike-feasibility.md    # GMC 스파이크 결과
│   ├── spike-home-credit.md    # Home Credit 스파이크 결과
│   └── feature-dictionary.md   # 전체 피처 이름·의미 정리
└── CLAUDE.md                # 확정/제안 결정 사항, 데이터 주의사항, 진행 상황
```

## 실행 방법

```bash
cd ml
uv sync                    # 의존성 설치
uv run jupyter lab          # 노트북 실행
```

데이터를 `data/raw/`, `data/raw_home_credit/`에 각 폴더 안내에 따라 받아둔 뒤 노트북을 실행하세요.

## 기술 스택

- **ML**: Python, pandas, scikit-learn, XGBoost, SHAP
- **백엔드**: Java / Spring Boot, Spring Batch, MyBatis (예정)
- **프론트엔드**: Vue.js (채택 시)
- **인프라**: Redis, Docker Compose, nginx, AWS EC2, GitHub Actions CI/CD (예정)

## 로드맵

`docs/project-plan.md` 기준 Phase 0~10 + 선택 확장으로 구성됩니다. 현재 위치는 [`CLAUDE.md`](CLAUDE.md)의
"진행 상황"에서 확인할 수 있습니다.

## 알려진 한계

- 두 데이터셋 모두 시간(날짜) 정보가 없어 실제 시간 순 드리프트 검증은 불가능합니다.
- Home Credit의 `EXT_SOURCE_1/2/3`는 정의가 공개되지 않은 외부 신용평가 점수로, 실제 운영 시 제3자 신용평가사
  계약이 전제되어야 합니다.
- 두 데이터셋 모두 해외(미국/유럽) 데이터로, 한국 시장 데이터가 아닙니다.

(상세 한계는 각 데이터셋 문서 및 `docs/project-plan.md` 참고)
