# data/raw

Kaggle **Give Me Some Credit** 대회 원천 데이터를 저장하는 위치다.
Kaggle 대회 규칙상 재배포 제한 가능성이 있어 이 폴더의 CSV/XLS 파일은 저장소에 커밋하지 않는다(`.gitignore` 참고).

## 받는 방법

1. https://www.kaggle.com/c/GiveMeSomeCredit/data 접속 (Kaggle 계정 필요)
2. 아래 파일을 내려받아 이 폴더에 그대로 배치한다.

| 파일 | 설명 |
|---|---|
| `cs-training.csv` | 학습용. 타깃(`SeriousDlqin2yrs`) 포함, 약 15만 행 |
| `cs-test.csv` | 채점용. 타깃 없음 — 이 프로젝트에서는 성능 평가에 쓰지 않는다(`cs-training.csv`를 분할해서 평가) |
| `sampleEntry.csv` | Kaggle 제출 형식 예시 |
| `Data Dictionary.xls` | 컬럼 설명 |

## 주의사항

- 첫 번째 컬럼은 이름 없는 인덱스 컬럼이다.
- 상세 데이터 주의사항은 `CLAUDE.md` 및 `docs/project-plan.md`, Phase 1 EDA 노트북(`ml/notebooks/01_eda.ipynb`) 참고.
