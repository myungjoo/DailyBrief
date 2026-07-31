# 허브 통합 대상 레지스트리

> `dailybrief-hub` 가 매 실행 시 읽는다. **에이전트를 새로 만들면 이 파일에 항목을 추가한다.**
> 그러면 허브 정의를 수정하지 않고도 새 탭이 생긴다.
> 여기에 없는 에이전트는 통합되지 않고 `미등록 에이전트` 알림으로만 표시된다.

## §0 탭 규칙

- 첫 탭은 항상 `전체`(허브가 생성. 레지스트리에 항목으로 넣지 않는다)
- 이후 탭은 아래 `tab_order` 오름차순
- 탭 라벨은 짧게. 좁은 화면에서 탭 바가 가로 스크롤된다

---

## §1 에이전트 항목

### 1. ondevice-llm-monitor

| 필드 | 값 |
|---|---|
| `name` | `ondevice-llm-monitor` |
| `label` | 기술 동향 |
| `role` | On-Device LLM · Runtime · Compiler · 경량화 기술 동향 |
| `tab_order` | 1 |
| `command` | `/ondevice-brief` |
| `cadence` | `daily` |
| `artifact_state` | `monitor/state/artifact.json` |
| `data_state` | `monitor/state/seen-index.json` |
| `reports_glob` | `monitor/reports/*-ondevice-llm-brief.md` |

`summary_fields` (보유 데이터로 표시할 항목):
- `추적 항목` = `items` 배열 길이
- `재보고 항목` = `items` 중 `report_count >= 2` 개수
- `마지막 수집` = `last_run`

### 2. ai-capital-monitor

| 필드 | 값 |
|---|---|
| `name` | `ai-capital-monitor` |
| `label` | 투자 동향 |
| `role` | LLM · NPU · System Software 분야 투자·펀딩·M&A |
| `tab_order` | 2 |
| `command` | `/invest-brief` |
| `cadence` | `weekly` |
| `artifact_state` | `capital/state/artifact.json` |
| `data_state` | `capital/state/deal-ledger.json` |
| `reports_glob` | `capital/reports/*-ai-capital-brief.md` |

`summary_fields`:
- `추적 기업` = `companies` 배열 길이
- `누적 딜` = 모든 `companies[].rounds` 개수 합
- `누적 조달액` = 모든 `companies[].cumulative_usd_m` 합 (단위 $M)
- `거대자본 항목` = `macro` 배열 길이
- `루머` = `rumors` 중 `status != "died"` 개수
- `마지막 수집` = `last_run`

### 3. dev-activity-monitor

| 필드 | 값 |
|---|---|
| `name` | `dev-activity-monitor` |
| `label` | 개발 활동 |
| `role` | 부서원 GitHub 활동 - 작업 주제와 산출 지표 |
| `tab_order` | 3 |
| `command` | `/dev-activity` |
| `cadence` | `monthly` |
| `artifact_state` | `devteam/state/artifact.json` |
| `data_state` | `devteam/state/activity-history.json` |
| `reports_glob` | `devteam/reports/*-devteam-activity.md` |
| `contains_personal_data` | `true` |

`summary_fields`:
- `대상 인원` = 최신 `snapshots[].team_totals.roster_members`
- `활동 관측 인원` = 최신 `snapshots[].team_totals.active_members`
- `스냅샷` = `snapshots` 배열 길이
- `정체 항목` = 최신 `snapshots[].signals.stalled_count`
- `마지막 수집` = `last_run`

> **`contains_personal_data: true`** 인 항목은 실명과 개인별 지표를 포함한다.
> 허브는 발행 후 공개 범위 주의를 반드시 고지한다.

---

## §2 미등록 에이전트 처리

`.claude/agents/` 에는 있으나 이 레지스트리에 없는 에이전트는 탭으로 만들지 않는다.
아래는 **의도적으로 등록하지 않은** 항목이다.

| 에이전트 | 등록하지 않는 이유 |
|---|---|
| `dailybrief-hub` | 허브 자신. 자기를 탭으로 만들 수 없다 |

`agentify` 는 에이전트가 아니라 스킬이므로 대상이 아니다.

---

## §3 신선도 판정 기준

허브 정의의 STEP 2 표를 따른다. 요약:

| `cadence` | 최신 | 주의 | 정체 |
|---|---|---|---|
| `daily` | 1일 이내 | 2-3일 | 4일 이상 |
| `weekly` | 7일 이내 | 8-14일 | 15일 이상 |
| `monthly` | 31일 이내 | 32-45일 | 46일 이상 |
| `manual` | 판정 안 함 - 경과 일수만 사실로 표시 | | |

주기를 바꾸려면 해당 항목의 `cadence` 값을 고친다. 실제 자동 실행 등록(크론)과
이 값이 어긋나면 잘못된 경보가 나므로, 크론을 변경할 때 이 파일도 같이 고친다.

---

## §4 신규 에이전트 등록 체크리스트

새 에이전트를 허브에 붙일 때 확인할 것.

1. 그 에이전트가 `state/artifact.json` 을 갖고 있는가 (URL·`last_published`)
2. 원장/이력 파일이 JSON이고 파싱 가능한가
3. 리포트 파일명에 날짜가 들어 있는가 (`YYYY-MM-DD-*.md` 또는 `YYYY-MM-*.md`)
4. 리포트에 `**한 줄 요약**:` 줄이 있는가 (허브가 헤드라인을 여기서 뽑는다)
5. `summary_fields` 를 그 원장의 실제 스키마에 맞게 정의했는가
6. 개인정보 포함 여부(`contains_personal_data`)를 표시했는가
