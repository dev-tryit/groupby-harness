# 백그라운드 작업

사용자의 요청 없이 시작되는 작업. 근거는 `03_prd/prd.md`의 7장과 F10이다.

작업마다 하는 일, 입력(`ReqParam`), 출력(`ResResult`)을 적는다.

## 1. 한눈에

| 작업 | 하는 일 | 시작하는 것 | 하는 프로그램 |
| :- | :- | :- | :- |
| 공고 넘기기 | 공고를 수집하고 읽어서 넘긴다 | 실행 명령 | 고객사 프로그램 |
| 공고 비교하기 | 넘겨받은 공고의 특징을 뽑고 이력서와 비교한다 | 고객사 프로그램이 끝난 것 | 서비스 제공사 프로그램 |

- 화면에는 공고 주소를 넣는 자리가 없다(`04_wireframe/wireframe.html` 5장). 그래서 전체 과정은 화면의 요청이 아니라 실행할 때 한 번 돈다.
- 받는 이벤트는 없다(`06_data-model/event/event.md`).
- 두 작업 모두 저장된 것이 있는 단계는 건너뛴다.

## 2. 공고 넘기기

**하는 일**: 공고마다 수집 → 읽기 → 넘기기를 순서대로 한다. 공고 하나가 실패해도 다음 공고로 넘어간다. 실패가 있어도 0으로 끝난다. 그래야 서비스 제공사 프로그램이 이어서 시작한다.

**입력**

```
class SubmitPostingsReqParam {
    postings: string[]      // 사람인 공고 주소 또는 공고 번호. 이 과제에서는 고른 공고 2개
}
```

**출력**

```
class SubmitPostingsResResult {
    submitted: Submission[]         // 넘긴 것. 06_data-model의 SUBMISSION
    failed: { posting: string, reason: string }[]
}

class Submission {
    posting_id: string
    title: string
    text: string            // 읽은 내용 전체
    trace_id: string        // 공고마다 하나 만든다
}
```

- 코드에는 위의 출력을 묶은 값이 없다. 넘긴 것은 파일이고, 넘긴 것과 실패한 것은 로그로 남는다(`posting.submitted`, `posting.collect_failed`, `llm.failed`, `run.finished`의 성공한 수와 실패한 수).

## 3. 공고 비교하기

**하는 일**: 넘겨받은 공고마다 특징 추출을 한 번, 이력서마다 비교를 한 번 한다. 끝나면 요청-응답 API를 연다(`request-response/request-response.md`).

**입력**

```
class MatchPostingsReqParam {
    submissions: Submission[]       // 넘겨받은 것 전부
    resumes: { posting_id: string, resume_id: string, text: string }[]
}
```

**출력**

```
class MatchPostingsResResult {
    features: PostingFeatures[]     // 06_data-model의 FEATURES, ITEM
    matches: ResumeMatch[]          // 06_data-model의 RESUME_MATCH, JUDGMENT
    failed: { posting_id: string, resume_id: string | null, reason: string }[]
}
```

- 넘겨받은 trace id를 그 공고의 특징 추출과 모든 비교의 로그에 그대로 쓴다. 넘긴 것에 trace id가 없으면(앞의 실행에서 넘긴 것) 하나 만든다.
- 코드에는 위의 출력을 묶은 값이 없다. 결과는 저장된 파일이고, 실패는 로그(`llm.failed`)와 `run.finished`의 실패한 수로 남는다.
- 넘겨받은 것이 하나도 없어도 API를 연다. 화면은 공고가 없다고 보여 준다.
- 점수와 순위는 이 작업의 출력이 아니다. API가 응답을 만들 때 계산한다.

## 4. 걸리는 시간

재지 않았다. 속도의 목표는 아직 없다(`03_prd/prd.md` 5장). LLM 호출은 처음 실행할 때 32번이고, 저장된 결과가 다 있으면 0번이다(`external/external.md` 4장).
