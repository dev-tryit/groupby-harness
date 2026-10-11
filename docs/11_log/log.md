# 로그 설계

무엇을 어떤 형태로 남길지를 적은 문서. 남기면 안 되는 것과 반드시 남겨야 하는 것은 `10_security/security.md` 4장을 따른다.

## 1. 로그로 답하려는 질문

로그는 아래 질문에 답할 수 있어야 한다. 3장의 목록은 여기서 나왔다.

| 질문 | 답하는 로그 |
| :- | :- |
| 이 공고는 어디까지 진행되었고 어디서 멈췄는가 | 공고의 trace id로 묶인 로그 전체 |
| OpenAI에 얼마를 썼는가 | `llm.called`의 비용, `run.finished`의 합계 |
| 이번 실행에서 LLM을 실제로 불렀는가, 저장된 결과를 썼는가 | `llm.called`, `stage.skipped` |
| LLM이 지어낸 것이 있었는가 | `item.not_in_reading`, `quote.not_in_resume` |
| 바깥으로 무엇을 요청했고 어떻게 답했는가 | `http.requested`, `llm.called`, `llm.failed` |
| 화면의 요청이 왜 실패했는가 | 요청의 trace id로 묶인 `api.*` |

## 2. 형태

모든 로그는 한 줄에 JSON 하나다. 고객사 프로그램, 서비스 제공사 프로그램, 화면이 같은 형태를 쓴다.

```json
{"ts":"2026-10-11T19:30:00.123+09:00","level":"INFO","service":"server","trace_id":"3f9a1c2e","event":"llm.called","posting_id":"55240017","resume_id":"resume-1","purpose":"match","model":"gpt-4.1-mini","input_tokens":1612,"output_tokens":637,"cost_usd":0.0017,"ms":4210,"openai_request_id":"req_…"}
```

| 필드 | 뜻 | 있어야 하는가 |
| :- | :- | :- |
| `ts` | 시각. 시간대가 붙은 ISO 8601 | 항상 |
| `level` | `INFO`, `WARN`, `ERROR` | 항상 |
| `service` | `client`(고객사 프로그램), `server`(서비스 제공사 프로그램), `frontend`(화면) | 항상 |
| `trace_id` | 3장 | 항상 |
| `event` | 무슨 일인지. `대상.일어난 일`의 모양이다. 4장의 목록에서만 고른다 | 항상 |
| `posting_id`, `resume_id`, `item_id` | 무엇에 대한 일인지 | 해당할 때 |
| `ms` | 걸린 시간(밀리초) | 바깥을 부른 일, 요청, 실행 |
| `error` | 실패한 이유. 예외의 종류와 짧은 글 | `ERROR`일 때 |

- 문장으로 된 메시지를 두지 않는다. 무슨 일인지는 `event`로, 값은 필드로 적는다. 그래야 형태가 일관되고 검색할 수 있다.
- 필드의 이름은 `06_data-model`과 `07_api`의 이름을 그대로 쓴다(`posting_id`, `resume_id`, `item_id`, `input_tokens`, `output_tokens`).
- 수준의 기준: 예상한 일은 `INFO`, 사람이 봐야 하지만 계속 진행한 일은 `WARN`, 그 일을 끝내지 못했으면 `ERROR`다.

## 3. trace id

하나의 흐름에서 나온 로그를 끝까지 잇는 값이다. 16진수 8자다.

| 흐름 | 만드는 곳 | 이어지는 길 |
| :- | :- | :- |
| 공고 하나의 처리 | 고객사 프로그램이 공고마다 하나 만든다 | 수집 → 읽기 → 넘긴 것의 `trace_id`(`07_api/background/background.md` 2장) → 서비스 제공사 프로그램의 특징 추출 → 그 공고의 모든 비교 |
| 화면의 요청 | 화면이 요청마다 하나 만든다 | `X-Trace-Id` 헤더 → 서버의 로그 → 응답의 `X-Trace-Id` 헤더(`07_api/request-response/request-response.md` 5장) |
| 프로그램의 실행 | 프로그램이 시작할 때 하나 만든다 | `run.started`와 `run.finished`만 이 값을 쓴다 |

- 바깥 서비스에는 trace id를 넘길 수 없다. 대신 OpenAI가 응답에 준 요청 번호를 `openai_request_id` 필드로 함께 남긴다.
- 서버가 `X-Trace-Id`가 없는 요청을 받으면 하나 만든다. 받은 값이 16진수 8자가 아니면 버리고 새로 만든다.
- 넘긴 것에 `trace_id`가 없으면(앞의 실행에서 넘긴 것) 서비스 제공사 프로그램이 그 공고에 하나 만든다.
- `item_id`는 API와 같은 값이다(`{공고 번호}-C1`).

공고 하나를 trace id로 따라간 모습이다.

```
client  3f9a1c2e  posting.collected   posting_id=55240017 images=1
client  3f9a1c2e  llm.called          purpose=read
client  3f9a1c2e  posting.submitted
server  3f9a1c2e  llm.called          purpose=extract
server  3f9a1c2e  features.extracted
server  3f9a1c2e  llm.called          purpose=match resume_id=resume-1
server  3f9a1c2e  quote.not_in_resume resume_id=resume-1 item_id=…
server  3f9a1c2e  resume.matched      resume_id=resume-1
```

## 4. 남기는 일

| `event` | 수준 | 쓰는 쪽 | 함께 남기는 것 |
| :- | :- | :- | :- |
| `run.started` | INFO | client, server | 공고의 수 |
| `run.finished` | INFO | client, server | 성공한 수(`succeeded`), 실패한 수(`failed`), LLM 호출 수(`llm_calls`), 비용의 합(`cost_usd`), 걸린 시간(`ms`). client는 공고를, server는 이력서를 센다 |
| `http.requested` | INFO | client | 대상의 호스트, 무엇을 가져왔는지(`purpose`: `page`, `image`), 상태, `Content-Type`, 걸린 시간 |
| `posting.collected` | INFO | client | 이미지의 수 |
| `posting.collect_failed` | ERROR | client | 실패한 이유 |
| `posting.read` | INFO | client | 조각의 수, 글자의 수 |
| `posting.submitted` | INFO | client | |
| `stage.skipped` | INFO | client, server | 건너뛴 단계(`stage`: `collect`, `read`, `extract`, `match`). 저장된 결과가 있다 |
| `llm.called` | INFO | client, server | 무엇을 위한 호출인지(`purpose`: `read`, `extract`, `match`), 모델, 토큰 수, 비용, 걸린 시간, `openai_request_id` |
| `llm.failed` | ERROR | client, server | `purpose`, 실패한 이유 |
| `features.extracted` | INFO | server | 그룹마다 항목의 수(`groups`). 새로 뽑았을 때만 남긴다 |
| `item.not_in_reading` | WARN | server | `item_id`. 실행할 때마다 넘겨받은 읽은 내용으로 다시 견준다 |
| `resume.matched` | INFO | server | 판정마다 개수(`verdicts`). 점수에 쓰는 판정으로 센다. 저장된 결과를 쓴 경우에도 남긴다 |
| `quote.not_in_resume` | WARN | server | `item_id`, LLM이 낸 판정(`verdict`). 저장된 결과를 쓴 경우에도 남긴다 |
| `api.requested` | INFO | server | 메서드, 경로, 상태, 걸린 시간 |
| `api.rejected` | WARN | server | 메서드, 경로, 상태(400, 404) |
| `api.failed` | ERROR | server, frontend | 경로, 실패한 이유 |

- 요청 하나에는 `api.requested`, `api.rejected`, `api.failed` 가운데 하나만 남긴다.
- 화면은 `api.failed`만 남긴다. 성공한 요청은 서버의 `api.requested`에 같은 trace id로 남는다.
- 점수와 순위의 계산은 로그로 남기지 않는다. 판정에서 늘 같은 값이 나오고, 화면이 식을 보여 준다.

`10_security/security.md` 4장이 반드시 남기라고 한 것과의 대응이다.

| 반드시 남겨야 하는 것 | 남기는 로그 |
| :- | :- |
| LLM 호출마다 모델, 토큰 수, 비용 | `llm.called` |
| 대조에 실패한 것의 번호 | `item.not_in_reading`, `quote.not_in_resume` |
| 바깥으로 보낸 요청의 대상과 결과 | `http.requested`, `llm.called`, `llm.failed`, `posting.collect_failed` |
| 400과 404로 답한 요청 | `api.rejected` |

## 5. 남기지 않는 것

`10_security/security.md` 4장과 같다. OpenAI 키와 `Authorization` 헤더, 이력서의 글자와 근거의 구절, 읽은 내용 전체, 프롬프트와 LLM 응답의 본문이다.

- 그래서 대조에 실패한 로그에는 구절이 아니라 번호(공고 번호, 이력서 번호, 항목 번호)를 남긴다. 구절은 저장된 결과에서 그 번호로 찾는다.
- 실패한 이유(`error`)에 바깥에서 받은 본문을 그대로 넣지 않는다.
- `http.requested`에는 호스트만 남긴다. 주소 전체를 남기지 않는다.

## 6. 내보내는 곳

- 백엔드의 두 프로그램은 표준 출력으로 낸다. `docker compose logs`로 본다(`08_infra/infra.md` 4장).
- 두 컨테이너의 표준 출력에는 로그만 나온다. 사람이 읽는 결과(랭킹과 근거)는 화면이 보여 준다. 서비스 제공사 프로그램의 `extract`, `rank` 명령은 컨테이너가 쓰지 않는 명령이고 사람이 읽는 글을 출력한다.
- 화면은 브라우저의 콘솔로 낸다. 서버로 보내지 않는다.
- 로그를 모으는 저장소는 두지 않는다. 평가자 한 명이 자기 컴퓨터에서 실행한다(`03_prd/prd.md` 5장).
