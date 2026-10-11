# 13_code 수정 기록

이 폴더의 문서를 수정할 때마다 아래에 기록한다. 형식은 `docs/AGENTS.md`의 "수정 기록"을 따른다.

## 2026-10-10 21:45 — `frontend/`, `backend/`, `infra/`를 git 서브모듈로 바꿈

**무엇을 바꿨는지**

- 세 폴더를 각각 별도 레포(`dev-tryit/groupby-frontend`, `groupby-backend`, `groupby-infra`, 모두 public)로 만들고, 이 레포에 서브모듈로 연결했다. 서브모듈은 `v1.0.0` 브랜치를 따른다.
- `infra/local/.gitkeep`과 `infra/server/README.md`는 `groupby-infra` 레포로 옮겼다.
- `AGENTS.md`에 "서브모듈" 장을 넣었다.

**왜 바꿨는지**

- 사용자가 정했다. 이유는 두 가지다. 코드 레포를 각각 관리하기 위해서이고, 나중에 GitHub Actions를 환경마다 다르게 설정할 일이 많기 때문이다. 워크플로는 레포 단위로 설정되므로, 레포가 나뉘어 있어야 프론트엔드·백엔드·인프라가 서로 다른 테스트와 배포를 가질 수 있다.
- 서브모듈을 `v1.0.0` 브랜치에 둔 것은 지금 개발 중인 버전이 `v1.0.0`이기 때문이다. 하네스 레포의 `v1.0.0`이 세 서브모듈의 `v1.0.0`을 가리킨다.
- 세 레포를 public으로 만든 것은 제출물이 public 레포이기 때문이다. 서브모듈이 private이면 평가자가 코드를 받지 못한다.
- 잃는 것도 있다. 평가자가 `git clone --recursive`로 받아야 하고, 그냥 받으면 코드 폴더가 비어 보인다. 그래서 README의 "실행 방법" 맨 위에 그 명령을 적었다. 또 코드를 고칠 때마다 커밋이 두 번(서브모듈, 하네스) 필요하다.

**영향을 받는 다른 단계**

- `09_infra`: GitHub Actions를 레포마다 따로 설계한다.
- `13_test-scenario`: 골든셋은 하네스 레포에 있고 백엔드는 다른 레포에 있다. 백엔드가 골든셋을 읽는 경로를 `14_code`에서 정해야 한다.

## 2026-10-11 19:04 — 단계 번호를 `14_code`에서 `13_code`로 바꿈

**무엇을 바꿨는지**

- 폴더 이름을 `14_code`에서 `13_code`로 바꿨다. 이 폴더의 `AGENTS.md`가 가리키는 다른 단계의 이름도 새 번호로 고쳤다. 지난 기록은 고치지 않았다.
- 서브모듈 세 개(`frontend`, `backend`, `infra`)의 경로를 `docs/13_code/` 아래로 옮기고, `.gitmodules`의 이름과 경로를 함께 고쳤다.
- 서브모듈 세 개의 README에 적힌 경로를 고쳤다. 서브모듈에 커밋하고 푸시해야 한다.

**왜 바꿨는지**

- 핵심 알고리즘 단계(`08_algorithm`)를 `03_prd`에 합치면서 `08`번이 비었다(`03_prd/DECISIONS.md`).
- 사용자가 순서에 맞게 번호를 매겨야 한다고 했다. `docs/AGENTS.md`의 번호 규칙도 단계를 바꾸면 번호를 다시 매기라고 정한다.

**영향을 받는 다른 단계**

- 단계의 이름을 가리키는 모든 문서(README, `CHECKLIST.md`, 각 `AGENTS.md`, 설계 문서)를 함께 고쳤다.

## 2026-10-11 20:08 — `AGENTS.md`에 "주석과 수정 기록", "수정의 범위"를 더함

**무엇을 바꿨는지**

- 규칙 넷을 더했다. 코드의 주석은 최소화한다. 무엇을 왜 바꿨는지는 이 파일에 되돌릴 수 있을 만큼 적는다. 코드의 수정은 최소화하되 원하는 효과는 나와야 한다. 수정안이 여럿이면 사이드 이펙트가 적은 쪽을 고른다.

**왜 바꿨는지**

- 사용자가 정했다. 코드 자체가 주석과 같은 역할을 하므로 주석을 줄이고, 그 대신 수정 기록을 잘 써서 나중에 그것만 보고도 롤백할 수 있게 하라고 했다. 커밋도 함께 참고한다.
- 이 규칙은 `13_code`의 코드에 대한 것이다(사용자).

**영향을 받는 다른 단계**

- 없다. 지금 백엔드 코드에는 주석이 많다. 코드를 고칠 때 맞춘다.

## 2026-10-11 20:55 — API와 HTTP 서버, Docker Compose, 프론트엔드, `result/`

**무엇을 바꿨는지**

- 백엔드에 보여 주기(`groupby/server/viewing/`)를 더했다. 저장된 특징과 비교에서 `07_api/request-response`의 응답 3개를 만든다. 점수와 순위는 이때 계산한다. LLM을 부르지 않는다.
- HTTP 진입점(`server/adapters/inbound/http.py`)을 더했다. 표준 라이브러리의 HTTP 서버다. API 3개와 화면의 정적 파일을 준다. 오류는 `not_found`, `bad_request`, `internal`이고 모든 응답에 `X-Trace-Id`를 넣는다.
- 서버 CLI에 `serve` 명령을 더했다. 넘겨받은 공고를 비교한 뒤 서버를 연다. 공고나 이력서 하나가 실패해도 다음으로 넘어간다. 기존의 `extract`, `rank` 명령은 그대로다.
- 로그 도우미(`shared/log.py`)를 더했다. 한 줄에 JSON 하나다. `serve`와 API가 쓴다.
- 공고 목록의 어댑터(`file_posting_catalog.py`)를 더했다. 특징이 저장된 공고가 목록이고, 제목은 넘긴 것에서 얻는다. 넘긴 것이 없으면 직무를 제목 자리에 쓴다.
- 두 CLI가 키가 없어도 시작하게 했다(`os.environ.get`). 고객사 CLI는 공고 하나가 실패해도 다음 공고로 넘어간다.
- `Dockerfile`과 `.dockerignore`를 더했다. root가 아닌 사용자로 실행하고, `.env`는 이미지에 넣지 않는다. 테스트는 이미지에 넣어 컨테이너에서 돌릴 수 있게 했다.
- `infra/local/docker-compose.yml`을 더했다. 컨테이너는 `client`와 `server` 둘이고 포트는 `127.0.0.1:8000`이다.
- `frontend/index.html`을 더했다. `09_screen-design/screen-design.html`을 그대로 복사한 것이다. 서버에서 열면 API를 부르고, 파일로 열면 미리보기 데이터를 쓴다.
- 테스트 `tests/test_api.py`(9개)를 더했다.
- 루트에 `result/`를 만들고 위의 것을 복사했다. `result/docker-compose.yml`은 경로만 `result/`의 모양에 맞췄다.

**왜 바꿨는지**

- 과제의 요구사항 6(명령 하나로 실행)과 7(UI)이 충족되지 않았다. 사용자가 다 충족하게 만들라고 했다. 설계는 `07_api`, `08_infra`, `09_screen-design`에 이미 있었고 그대로 따랐다.
- `result/`는 사용자가 정했다. 평가자가 결과로서 볼 것만 한 곳에 두고 거기서 실행하게 하려는 것이다. 서브모듈을 받지 않아도 실행된다.
- 이력서 번호는 API에서 `{공고 번호}-{파일 이름}`으로 만든다. 골든셋의 파일 이름이 공고마다 겹치기 때문이다. 파일 이름을 바꾸면 저장된 비교 결과의 경로가 함께 바뀌어야 해서, 사이드 이펙트가 적은 쪽을 골랐다.
- `server`는 `client`가 끝난 뒤 시작한다. `client`는 실패해도 0으로 끝나므로, 수집이 실패해도 `server`가 저장된 결과로 화면을 연다(`08_infra/infra.md` 4장).

**되돌리려면**

- 백엔드에서 `viewing/`, `adapters/inbound/http.py`, `adapters/outbound/file_posting_catalog.py`, `shared/log.py`, `tests/test_api.py`, `Dockerfile`, `.dockerignore`를 지우고, 두 `cli.py`를 앞 커밋으로 돌린다. `infra/local/docker-compose.yml`, `frontend/index.html`, `result/`를 지운다.

**설계와 아직 어긋난 곳**

- 로그는 `11_log`의 사건 가운데 `run.*`, `resume.matched`, `llm.failed`, `api.*`만 남긴다. 고객사 프로그램은 아직 사람이 읽는 글을 출력한다. 넘긴 것에 `trace_id`가 없다.
- 저장된 파일의 필드 이름(`id`, `condition_id`)은 `06_data-model`의 이름(`item_id`, `no`)과 다르다. API의 응답에서 맞춘다.
