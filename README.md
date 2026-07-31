# DailyBrief

임원용 기술 인텔리전스 에이전트 팀. Claude Code 서브 에이전트로 구성된 정기 브리핑 파이프라인이다.

관심 도메인은 **On-Device LLM**과 이를 임베디드 디바이스에서 실행하는 **Runtime · Compiler ·
경량화** 기술이다. 각 에이전트는 임원이 반복적으로 하던 조사·판단 작업을 하나씩 대신하고,
마크다운 리포트와 웹 대시보드(Artifact)를 산출한다.

## 에이전트 구성

| 에이전트 | 역할 | 수동 호출 | 작업 공간 | 주기 |
|---|---|---|---|---|
| `ondevice-llm-monitor` | On-Device LLM · Runtime · Compiler · 경량화 **기술 동향** | `/ondevice-brief` | `monitor/` | 매일 |
| `ai-capital-monitor` | LLM · NPU · System Software 분야 **투자·펀딩·M&A** | `/invest-brief` | `capital/` | 매주 |
| `dev-activity-monitor` | 지정 개발자 **GitHub 활동** 요약 및 산출 지표 | `/dev-activity` | `devteam/` | 월간 |
| `dailybrief-hub` | 위 세 개를 **탭형 메인 페이지**로 통합 | `/dailybrief` | `hub/` | 수동 |

여기에 `agentify` 스킬이 있다. 프롬프트를 붙여넣고 "에이전트화" 를 요청하면 저장소 규약대로
새 에이전트 정의·작업 공간·수동 호출 스킬·레지스트리 등록까지 한 번에 만든다.

## 디렉터리 구조

```
.claude/agents/<name>.md          에이전트 정의 (서브 에이전트 본체)
.claude/agents/TEAM.md            팀 로스터 - 역할 경계와 상태 파일 목록
.claude/skills/<name>/SKILL.md    수동 호출용 슬래시 커맨드
<domain>/config/                  감시 대상·판단 기준 (사람이 손으로 고치는 설정)
<domain>/state/                   실행 간 유지되는 상태 (중복 제거 원장, 이력)
<domain>/reports/                 날짜별 산출 리포트
<domain>/artifact/                Artifact 로 발행하는 HTML 소스
```

설정과 에이전트 정의를 분리한 것이 핵심이다. 감시 대상이나 판단 기준이 바뀌면 `config/` 만
고치고 에이전트 정의는 건드리지 않는다.

## 설계 원칙

1. **결론이 맨 앞** - 5분 내 읽고 의사결정에 쓸 수 있어야 한다. 과정 설명은 뒤로.
2. **근거 없는 주장 금지** - 성능 수치와 투자 금액은 1차 출처 확인 후 인용한다.
   확인하지 못한 값은 "미확인"으로 쓴다.
3. **상태는 파괴하지 않는다** - `state/` 의 원장은 초기화하지 않는다. 같은 내용을 두 번
   보고하는 것이 가장 큰 실패다. 각 에이전트가 잃으면 복구 불가한 데이터는
   `.claude/agents/TEAM.md` 에 정리돼 있다.
4. **자동 + 수동 양방향** - 정기 실행 에이전트는 짝이 되는 슬래시 커맨드를 함께 갖는다.
5. **도구는 최소 권한** - 각 정의의 `tools:` 에 실제로 필요한 것만 나열한다.

## 중복 제거 설계

도메인마다 "같은 것을 다시 보고하지 않는" 규칙이 다르다.

- **기술 동향**: 최근 5일 내 기보고 항목은 억제. 단 급상승(스타 급증, HN 노출, 상용 SDK 채택)
  시 `[급상승 재보고]`로 재노출.
- **투자 동향**: 하나의 라운드는 원칙적으로 1회만 보고. 금액·밸류 정정, 확대 클로징,
  전략적 투자자 신규 합류, 인수 대상 전환 시에만 `[갱신 재보고]`.
- **개발 활동**: 월별 스냅샷을 누적해 전월 대비 추이를 만든다. GitHub API 는 과거 시점의
  집계를 되돌려주지 않으므로 스냅샷이 유일한 기준선이다.

## 측정에 관한 주의

`dev-activity-monitor` 는 개발 활동 지표를 다룬다. 아래를 명시적으로 금지하고 있다
(`devteam/config/metrics.md` §2).

- 코드 줄 수(LOC)를 생산성으로 환산
- 커밋 수로 인원 순위 매기기
- 역할이 다른 인원을 같은 숫자로 비교
- 단일 종합 점수 산출
- 연속 활동일수를 미덕으로 취급
- 비공개 기여를 0으로 취급

용도는 자원 배치 판단·병목 발견·과부하 확인이고, 인사 평가가 아니다.
`reports/` 의 산출물에는 관측 범위의 한계(비공개 기여, 조회 호스트 범위)가 매 회차 명시된다.

## 요구사항

- [Claude Code](https://claude.com/claude-code)
- `gh` CLI (`dev-activity-monitor` 전용). GitHub 인증 필요:
  `gh auth login`, 사내 호스트는 `gh auth login --hostname <host>`
- Windows 환경 기준. 리포트는 UTF-8 with BOM 으로 저장하고 CP949 안전성을 검사한다
  (BOM 이 없으면 일부 Windows 뷰어가 파일 전체를 CP949 로 오판해 한글이 깨진다)

## 사용

```
/ondevice-brief          기술 동향 브리핑 생성
/invest-brief            투자 동향 브리핑 생성
/dev-activity            개발 활동 브리핑 생성
/dailybrief              메인 페이지 갱신
```

각 커맨드는 인자를 받는다. `--focus`, `--since`, `--deep` 등 도메인별 인자 표는
해당 `SKILL.md` 에 있다.

새 에이전트를 추가할 때는 프롬프트를 붙여넣고 에이전트화를 요청한 뒤,
`hub/config/agents.md` 레지스트리에 항목을 추가하면 메인 페이지에 탭이 생긴다.
