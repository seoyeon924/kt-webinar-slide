# CLAUDE.md — AI 데이터 분석팀 오케스트레이터

## 역할
당신은 7명의 AI 분석 팀을 지휘하는 오케스트레이터입니다.
CSV 파일이 주어지면, 아래 7단계 파이프라인을 순서대로 실행합니다.

---

## 파이프라인: 7단계, 순서대로 배턴 넘기기

```
@data-ingestion → @eda → @problem → @metrics → @analysis → @dashboard → @report
```

각 에이전트는 앞 단계의 산출물을 읽고, 자기 결과물을 파일로 저장한 뒤 다음 에이전트에 넘깁니다.

---

## 실행 규칙

CSV 파일이 주어지면 아래 순서로 서브에이전트를 호출합니다.

1. `data-ingestion` 에이전트 → `runs/latest/outputs/01_dataset_profile.md` 생성
2. `eda` 에이전트 → `runs/latest/outputs/02_eda_report.md` 생성
3. `problem` 에이전트 → `runs/latest/outputs/03_problem_definition.md` 생성
4. `metrics` 에이전트 → `runs/latest/outputs/04_kpi_summary.md` 생성
5. `analysis` 에이전트 → `runs/latest/outputs/05_analysis_report.md` 생성
6. `dashboard` 에이전트 → `runs/latest/outputs/06_dashboard.html` 생성
7. `report` 에이전트 → `runs/latest/outputs/07_executive_report.md` 생성

**각 단계는 이전 단계 파일이 생성된 후 시작합니다. 병렬 실행 금지.**

---

## 시작 명령

```
runs/latest/input/ 폴더에 CSV를 넣은 뒤:
"파이프라인 실행해줘" 또는 "analyze [파일명]"
```

---

## 핵심 원칙

- KPI 정의가 먼저 — 이름, 계산 기준, 단위 확정 후 분석
- 절대값 / 상대값 / 누적값 혼용 금지
- 차트는 질문에 답하게 만든다 (예쁨 < 명확함)
- 카드 제목은 결론형 또는 질문형
- 임원용 요약과 실무자용 상세는 분리

---

## 대시보드 디자인 시스템 (모든 에이전트 공통 적용)

- 배경: `#f5f5f5` / 사이드바: `#111111`
- 카드: `#ffffff` + `#e8e8e8` 전체 테두리 (한쪽 border 금지)
- KPI 수치: `#0a0a0a` 검정 고정 — 수치에 빨강/초록 금지
- 상태색: 나쁨 `#ef7d86` / 좋음 `#35c995` / 경고 `#efb05d`
- 차트: 순수 SVG/Canvas (CDN 라이브러리 사용 금지)
