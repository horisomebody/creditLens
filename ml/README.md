# ml

CreditLens의 데이터 탐색·전처리·모델링(Python) 영역.

## 환경

이 디렉터리는 [uv](https://docs.astral.sh/uv/)로 관리한다.

```bash
cd ml
uv sync              # 의존성 설치
uv run jupyter lab   # 노트북 실행
```

## 구조

- `notebooks/` — 단계별 탐색·분석 노트북 (`01_eda.ipynb` 등)
- `src/creditlens/` — 재사용 가능한 Python 모듈 (전처리, 피처, 모델 등. Phase 2부터 채워짐)

## 데이터

원천 데이터는 `../data/raw/`에 위치하며 저장소에 커밋하지 않는다. `../data/raw/README.md` 참고.
