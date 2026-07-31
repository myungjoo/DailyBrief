---
name: ondevice-brief
description: On-Device LLM · Runtime · Compiler · 경량화 기술 브리핑을 지금 즉시 생성한다(Manual Refresh). 매일 아침 자동 실행분과 동일한 파이프라인을 쓰며, 최근 5일 중복 제거와 급상승 재노출 규칙을 그대로 적용한다. "온디바이스 브리핑", "기술 동향 지금 확인", "브리핑 갱신" 요청 시 사용.
---

# On-Device LLM 브리핑 - 수동 실행

`ondevice-llm-monitor` 서브 에이전트를 즉시 호출해 브리핑을 갱신한다.

## 실행

Agent 도구로 `subagent_type: "ondevice-llm-monitor"` 를 호출한다.
백그라운드로 두지 말고 `run_in_background: false` 로 실행해 결과를 바로 받는다.

프롬프트에 넘길 내용:

```
오늘자 On-Device LLM 기술 브리핑을 생성해라.
- 에이전트 정의의 STEP 0~7을 순서대로 모두 수행할 것.
- monitor/config/watchlist.md 를 먼저 읽고 감시 축을 확정할 것.
- monitor/state/seen-index.json 의 5일 중복 제거 및 급상승 재노출 규칙을 반드시 적용할 것.
- 리포트 파일과 Artifact 대시보드를 모두 갱신할 것.
{추가 지시사항}
```

## 인자 처리

사용자가 인자를 넘긴 경우 `{추가 지시사항}` 자리에 반영한다.

| 인자 예시 | 해석 |
|---|---|
| `--focus quantization` / `양자화만` | 해당 기술 트랙에 가중해 수집. 다른 축은 표만 유지 |
| `--vendor qualcomm` | 특정 기업 집중. 경쟁 구도 분석을 강화 |
| `--since 2w` / `2주` | 수집 기간 확장(기본 7~10일) |
| `--deep` / `자세히` | 상위 3개 항목을 원문까지 읽어 기술 상세를 확장 |
| `--all` / `중복포함` | 5일 중복 제거를 이번 회차만 해제(전체 재확인용). `seen-index.json`은 정상 갱신 |
| 인자 없음 | 표준 일일 브리핑 |

## 완료 후

에이전트가 반환한 요약(한 줄 요약 · 핫이슈 3~5개 · 파일 경로 · Artifact URL · 집계)을 사용자에게
그대로 전달한다. 리포트 전문을 터미널에 다시 출력하지 않는다 - 파일과 웹 대시보드가 있다.
