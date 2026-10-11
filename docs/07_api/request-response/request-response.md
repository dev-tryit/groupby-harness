# 요청-응답

화면이 서비스 제공사 프로그램에 물어보는 API. 근거는 `04_wireframe/wireframe.html`의 구역 ①~⑥과 `06_data-model/persistent/persistent.md`다.

요청마다 하는 일, 입력(`ReqParam`), 출력(`ResResult`)을 적는다.

## 1. 한눈에

| 요청 | 하는 일 | 입력 | 출력 | 쓰는 구역 |
| :- | :- | :- | :- | :- |
| `GET /api/postings` | 공고의 목록을 준다 | `GetPostingsReqParam` | `GetPostingsResResult` | ① |
| `GET /api/postings/{posting_id}` | 공고에서 뽑은 것과 랭킹을 준다 | `GetPostingReqParam` | `GetPostingResResult` | ② ③ |
| `GET /api/resumes/{resume_id}` | 지원자 한 명의 점수 계산, 판정과 근거, 이력서 원문을 준다 | `GetResumeMatchReqParam` | `GetResumeMatchResResult` | ④ ⑤ ⑥ |

- 모두 읽기다. LLM을 부르지 않는다. 권한은 "없음"이다(`03_prd/prd.md` 2장). 폴링으로 부르지 않는다.

## 2. `GET /api/postings`

**하는 일**: 넘겨받은 공고의 목록을 준다. 공고 번호가 큰 것(최근에 올라온 공고)이 먼저다. 화면은 목록의 첫 공고를 먼저 보여 준다.

**입력**

```
class GetPostingsReqParam {
}
```

**출력**

```
class GetPostingsResResult {
    postings: PostingSummary[]
}

class PostingSummary {
    posting_id: string      // 공고 번호
    title: string           // 공고의 제목
    job: string             // 뽑은 직무
}
```

## 3. `GET /api/postings/{posting_id}`

**하는 일**: 공고 하나에서 뽑은 것(직무, 담당업무, 조건)과, 그 공고 지원자들의 랭킹을 준다. 점수와 순위는 저장하지 않고 이때 계산한다. 조건이 읽은 내용 안에 있는지(`in_reading`)는 넘겨받은 읽은 내용이 있으면 이때 다시 계산하고, 없으면 뽑을 때 저장한 값을 준다(`06_data-model/persistent/persistent.md` 3장). `item_id`와 `resume_id`는 저장된 번호 앞에 공고 번호를 붙여 이때 만든다(`{공고 번호}-C1`, `{공고 번호}-resume-1`).

**입력**

```
class GetPostingReqParam {
    posting_id: string      // 경로. 공고 번호. 숫자만
}
```

**출력**

```
class GetPostingResResult {
    posting_id: string
    title: string
    job: string
    text: string | null             // 고객사가 넘긴, 공고를 읽은 내용 전체. 넘긴 것이 없는 실행에서는 null
    duties: Duty[]                  // 담당업무
    conditions: ConditionGroups
    ranking: RankedResume[]         // 점수가 높은 순서. 같은 순위 안에서는 이력서 번호 순서
}

class Duty {
    item_id: string
    no: string              // D1, D2, …
    text: string
}

class ConditionGroups {
    자격요건: Condition[]
    우대사항: Condition[]
    참고사항: Condition[]
}

class Condition {
    item_id: string
    no: string              // C1, C2, …. 화면에 보여 주는 번호
    text: string            // 공고의 글자 그대로
    in_reading: boolean     // 읽은 내용 안에 글자 그대로 있는가
}

class RankedResume {
    rank: number            // 점수가 같으면 같은 순위
    resume_id: string       // 전체에서 고유하다
    name: string | null     // 가운데를 가린 지원자의 이름. 예: 강*재. 이력서의 제목에서 얻지 못하면 null이고 화면은 이력서 번호를 보여 준다
    score: number           // 0~100. 반올림한 정수. 순서는 반올림하기 전의 값으로 정한다
    required: GroupScore | null     // 자격요건이 없는 공고는 null
    preferred: GroupScore | null    // 우대사항이 없는 공고는 null
    verdicts: VerdictMark[]         // 조건마다의 판정. 자격요건, 우대사항, 참고사항의 순서
}

class VerdictMark {
    item_id: string
    mark: string            // 4장의 mark와 같다
}

class GroupScore {
    points: number          // 판정값을 더한 것
    full: number            // 만점. 조건의 수 × 2
    matched: number         // 일치한 조건의 수
    count: number           // 조건의 수
}
```

## 4. `GET /api/resumes/{resume_id}`

**하는 일**: 지원자 한 명의 점수가 어떤 숫자에서 나왔는지, 조건마다 판정과 근거가 무엇인지, 이력서 원문이 무엇인지를 준다. 화면의 표시(`mark`)는 저장하지 않고 이때 계산한다. 근거가 이력서 안에 있는지(`quotes_in_resume`)는 비교할 때 저장한 값을 준다.

**입력**

```
class GetResumeMatchReqParam {
    resume_id: string       // 경로. 이력서 번호. 영문, 숫자, -, _ 만
}
```

**출력**

```
class GetResumeMatchResResult {
    posting_id: string
    resume_id: string
    name: string | null     // RankedResume의 name과 같다
    score: number
    required: GroupScore | null
    preferred: GroupScore | null
    judgments: JudgmentView[]       // 자격요건, 우대사항, 참고사항의 순서
    resume_text: string             // 이력서의 글자 전체
}

class JudgmentView {
    item_id: string         // Condition의 item_id. 조건의 글자와 번호는 공고의 응답에서 찾는다
    group: string           // 자격요건, 우대사항, 참고사항
    mark: string            // 화면의 판정 칸에 나오는 표시. 아래 표
    points: number | null   // 참고사항은 null. 점수에 넣지 않는다
    quotes: string[]        // 근거. LLM이 이력서의 구절이라고 낸 것들
    quotes_in_resume: boolean   // 구절이 모두 이력서 안에 글자 그대로 있는가
    thinking: string        // 그렇게 판정한 이유
}
```

| `mark` | `points` | `quotes` | `quotes_in_resume` |
| :- | :- | :- | :- |
| `일치` | 2 | 있다 | 참 |
| `비슷` | 1 | 있다 | 참 |
| `다름` | 0 | 있다 | 참이거나 거짓. 거짓이어도 판정은 다름이다 |
| `이력서에 적혀 있지 않음` | 0 | 비어 있다 | 거짓 |
| `근거를 확인하지 못함` | 0 | 비어 있거나, 이력서 안에 없는 구절 | 거짓 |

- `이력서에 적혀 있지 않음`은 LLM이 다름으로 판정하고 구절을 내지 않은 것이다.
- `근거를 확인하지 못함`은 LLM이 일치나 비슷으로 판정했는데 구절이 이력서 안에 없는 것이다. 다름으로 친다(`03_prd/prd.md` 10.1).

## 5. 오류

모든 요청의 오류 출력은 같다.

```
class ErrorResResult {
    error: string           // not_found, bad_request, internal
}
```

| 경우 | 상태 | `error` |
| :- | :- | :- |
| 없는 공고 번호나 이력서 번호 | 404 | `not_found` |
| 입력의 모양이 어긋난다 | 400 | `bad_request` |
| 그 밖의 실패 | 500 | `internal` |

- 오류 출력에 내부의 경로나 예외의 글자를 담지 않는다.
- 모든 응답의 헤더에 `X-Trace-Id`를 넣는다. 요청에 `X-Trace-Id`가 있고 16진수 8자이면 그 값을 쓴다. 없거나 모양이 어긋나면 버리고 새로 만든다. 이 값은 로그에 그대로 들어가기 때문이다.
