# data/raw_home_credit

**Home Credit Default Risk** (Kaggle) 원천 파일을 저장하는 위치다. 데이터는 이미 받아서 배치됨(2026-09-17).

2026-09-17 결정으로 데이터셋 전략이 바뀌었다: GMC(`data/raw/`)는 비교군으로 유지하고, 이 폴더에는
주 데이터셋인 Home Credit Default Risk를 둔다. 변경 사유와 근거 수치는 `CLAUDE.md`, `docs/project-plan.md`,
`docs/spike-feasibility.md` 참고.

## 파일 목록

| 파일 | 설명 | 용량 |
|---|---|---|
| `application_train.csv` | 주 테이블. 대출 신청 시점 신청자 정보 + 타깃(`TARGET`) | 158M |
| `application_test.csv` | 주 테이블(채점용, 타깃 없음) | 25M |
| `bureau.csv` | 다른 금융기관에 보고된 신용 정보 | 162M |
| `bureau_balance.csv` | `bureau`의 월별 잔액 이력 | 358M |
| `previous_application.csv` | Home Credit 내 과거 대출 신청 이력 | 386M |
| `POS_CASH_balance.csv` | 과거 POS/현금 대출 월별 잔액 | 375M |
| `credit_card_balance.csv` | 과거 신용카드 월별 잔액 | 405M |
| `installments_payments.csv` | 과거 대출 상환 이력 | 690M |
| `HomeCredit_columns_description.csv` | 컬럼 설명 | 40K |
| `sample_submission.csv` | Kaggle 제출 형식 예시 | 524K |

전체 약 2.5GB.

## 주의사항

- 이 폴더의 파일은 저장소에 커밋하지 않는다(`.gitignore` 참고). Kaggle 대회 규칙상 재배포 제한 가능성이 있다.
- 정확한 행 수·결측률·타깃 비율 등은 아직 실측하지 않았다 — `docs/project-plan.md` Phase 5에서 EDA로 검증한다.
