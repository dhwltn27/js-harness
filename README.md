# js-harness

Codex로 데이터 작업을 할 때 쓰는 작업 규칙과 Skill 묶음이다. 요구사항 확인, 결정 기록, 독립 검산, 보안 점검을 정해진 때에 하게 한다. 특정 분야를 가정하지 않는다.

AI가 일을 대신 끝내게 하는 것이 아니라 세 가지 빚을 남기지 않게 하는 것이 목적이다.

- 기술 부채: 요구를 채우는 가장 작은 구현, 실제로 돌린 테스트
- 의도 부채: 목표, 선택 기준, 결정마다 출처
- 인지 부채: 사용자가 직접 대조할 숫자와 설명 근거. 결과를 사용자가 설명하지 못하면 아직 끝나지 않은 것이다.

작업 환경의 규칙과 과제 README가 이 저장소보다 먼저다. 이 저장소는 제한을 바꾸거나 우회할 권한을 주지 않는다. 작업 데이터와 결과는 이 저장소나 외부로 보내지 않는다. 받기만 해서는 규칙이 적용되지 않으므로 아래 설치를 하거나, Codex에 이 저장소의 `AGENTS.md` 경로를 알려 읽게 한다.

## 설치 (Codex가 읽고 따라 한다)

이 저장소는 작업 폴더 밖(예: 홈 폴더 `~/js-harness`)에 받는다. 작업 폴더 안에 받으면 결과물에 섞인다. 받은 위치를 `HARNESS`, 작업 폴더를 `TASK`라고 한다. 설치는 여러 번 해도 결과가 같아야 한다.

1. `HARNESS/skills/` 아래 `SKILL.md`가 있는 폴더와 `HARNESS/third_party/ponytail/skills/ponytail-review` 폴더를 `~/.agents/skills/`로 복사한다. ponytail-review를 복사할 때는 `HARNESS/third_party/ponytail/LICENSE`도 고치지 않고 `~/.agents/skills/ponytail-review/LICENSE`로 함께 복사한다. 사용자가 Skill 이름을 골랐으면 그 Skill만, 아니면 전부 복사한다. 같은 이름 폴더가 이미 있으면 파일 내용을 비교해 같으면 건너뛰고, 다르면 덮어쓰지 말고 사용자에게 알린다. 과제 폴더 안에는 복사하지 않는다.
2. `TASK/AGENTS.md`가 없으면 `HARNESS/AGENTS.md`를 복사한다. 이미 있으면 바꾸지 말고, 두 문서의 겹치거나 충돌하는 줄을 보이고 사용자에게 어떻게 합칠지 묻는다. 이미 같은 내용이 들어 있으면 다시 덧붙이지 않는다.
3. 그 밖의 과제 파일과 Codex 설정은 바꾸지 않는다. `templates/DECISIONS.md`는 과제에 같은 역할의 문서가 없을 때만 나중에 쓴다.
4. 복사한 파일 목록을 보이고, 사용자에게 `/skills`로 목록을 확인하고 안 보이면 Codex를 다시 시작하라고 알린다.
5. 마지막으로 `TASK/AGENTS.md`를 읽고 이 과제에 맞지 않는 줄이 있으면 알려 준다.

Skill이 끝내 목록에 안 보이면 설치를 건너뛴다. `AGENTS.md` 규칙에 따라 Codex가 이 저장소의 `skills/<이름>/SKILL.md`를 직접 읽어 같은 순서로 진행한다.

## 구성

| 파일 | 언제 | 하는 일 |
|---|---|---|
| `AGENTS.md` | 항상 | 결정 출처, 범위, 데이터 분석 방법, 검증, 보안, 역할, 저장 규칙 |
| `$cp-start` | 시작 | 과제 유형, 요구사항, 입력 특이점, 함정 후보, 보안 위치, 요청자 질문과 보안 질문 후보 |
| `$cp-check` | 할 일이 끝날 때, 마감 전 | 실행·테스트·재실행·바뀐 입력 확인, 건수 보존과 숫자 대조. 마감 때는 새 검사관의 독립 검산, 요구 충족, 제출 형식, 설명 질문 (읽기 전용) |
| `$cp-secure` | 마감 전, 비밀 파일이나 개인정보가 보일 때 | 비밀값·개인정보·원본 행 노출 검토, 확인 후 추적 해제·환경변수·개인정보 제거 |
| `$ponytail-review` | 구현 뒤 원할 때 | 코드 리뷰: 버그, 보안·데이터 손실, 테스트 누락, 속도, 불필요한 코드. 번호 붙은 지적만 내고 고치지 않음 (외부 공개 Skill, `third_party/ponytail/`에 원본 그대로) |
| `templates/DECISIONS.md` | 질문이 끝난 뒤 | 결정 기록 틀(과제에 없을 때만) |

요청자 답 반영, 계획, 작업 진행, 오류 반복, 웹 서버 확인은 Skill 없이 `AGENTS.md` 규칙으로 처리한다. 목표와 제약, 작업 지시는 사용자가 직접 쓴다.

## 쓰는 순서

1. 시작: `$cp-start` → 요청자에게 질문 → 답을 "요청자 답: …"으로 전달 → 결정 기록과 할 일 목록
2. 구현: 할 일 하나씩. 끝날 때마다 `$cp-check`, 통과하면 commit
3. 코드 리뷰(선택): `$ponytail-review` → 고칠 번호를 골라 지시
4. 마감: "마감 모드" → `$cp-secure` → `$cp-check` 최종

## 직접 쓰는 짧은 요청

반복 규칙은 `AGENTS.md`에 있으니 목표, 선택, 이유만 쓴다.

- 방향: "목표는 ○○. 완료 조건은 ○○(확인할 값). 모르는 것은 ○○."
- 고르기: "A로 할게. 이유는 ○○. B는 이번엔 안 해."
- 맡기기: "이번엔 ○○만. 완료 조건은 ○○."
- 어긋남: "예상은 A인데 실제는 B야. 고치기 전에 원인 후보부터."
- 손 대조: "○○ 하나의 ○○ 하루를 분자·분모 계산표로 보여줘. 내가 맞춰 볼게."
- 확인: "확인함: ○○ = 숫자." / "내가 화면에서 ○○를 볼 때까지 완료로 치지 마."
- 마감: "마감 모드."

## 역할

- 메인: 사용자와 대화하며 분석, 계획, 구현, 확인, 저장을 맡는다.
- 일꾼(선택): 사용자가 맡기라고 할 때만 부른다.
- 검사관: 매번 새로, 대화 기록 없이 부르고 작업 파일을 고치지 않는다.
- 일꾼과 검사관의 모델과 추론 단계는 사용자가 그때그때 정한다.

## 출처와 라이선스

MIT. 아래 공개 자료(모두 MIT)를 참고해 짧게 다시 썼다.

- 검증과 디버깅: obra/superpowers의 verification-before-completion, systematic-debugging
- 질문 방식: mattpocock/skills의 grilling, obra/superpowers의 brainstorming
- 데이터 속 지시 무시: K-Dense-AI/scientific-agent-skills의 exploratory-data-analysis
- 원인 기여도: nimrodfisher/data-analytics-skills의 root-cause-investigation
- 건수 보존: magnus919/agent-skills의 data-cleaning
- 인코딩은 가린 것이 아님: facebookresearch/secpriv-skill

`third_party/ponytail/`의 `ponytail-review`는 DietrichGebert/ponytail(MIT)의 파일을 고치지 않고 그대로 둔 것이다. 출처와 커밋은 `third_party/ponytail/README.md`에 있다.
