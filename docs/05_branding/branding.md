---
version: alpha
name: groupby 매칭 화면
description: 토스의 design.md(TDS)의 시각 언어를 바탕으로 한 브랜딩. 채용 담당자가 지원자의 판정과 근거를 이력서 원문과 나란히 보고 판단하는 화면에 맞췄다.
colors:
  primary: "#3182f6"
  blue-500: "#3182f6"
  blue-600: "#1b64da"
  blue-50: "#e8f3ff"
  grey-900: "#191f28"
  grey-800: "#333d4b"
  grey-700: "#4e5968"
  grey-600: "#6b7684"
  grey-500: "#8b95a1"
  grey-400: "#b0b8c1"
  grey-300: "#d1d6db"
  grey-200: "#e5e8eb"
  grey-100: "#f2f4f6"
  grey-50: "#f9fafb"
  white: "#ffffff"
  green-500: "#007738"
  orange-500: "#ff8800"
  red-500: "#f04452"
  fill-brand: "{colors.blue-500}"
  fill-brand-weak: "{colors.blue-50}"
  fill-primary: "{colors.grey-900}"
  fill-secondary: "{colors.grey-100}"
  fill-weak: "{colors.grey-50}"
  text-primary: "{colors.grey-900}"
  text-secondary: "{colors.grey-700}"
  text-tertiary: "{colors.grey-600}"
  text-brand: "{colors.blue-600}"
  text-alt: "{colors.white}"
  border-secondary: "{colors.grey-200}"
  border-strong: "{colors.grey-400}"
  border-focus: "{colors.blue-500}"
  verdict-match: "{colors.green-500}"
  verdict-match-weak: "#dcf9e4"
  verdict-similar: "#9f5100"
  verdict-similar-weak: "#ffecd0"
  verdict-different: "#c2152f"
  verdict-different-weak: "#ffe1e1"
typography:
  display-2:
    fontFamily: Pretendard
    fontSize: 40px
    fontWeight: 700
    lineHeight: 1.2
    letterSpacing: -0.02em
  h4:
    fontFamily: Pretendard
    fontSize: 20px
    fontWeight: 700
    lineHeight: 1.35
    letterSpacing: -0.015em
  title-2:
    fontFamily: Pretendard
    fontSize: 17px
    fontWeight: 600
    lineHeight: 1.45
    letterSpacing: -0.01em
  body-1:
    fontFamily: Pretendard
    fontSize: 17px
    fontWeight: 400
    lineHeight: 1.5
    letterSpacing: -0.005em
  body-2:
    fontFamily: Pretendard
    fontSize: 15px
    fontWeight: 400
    lineHeight: 1.5
    letterSpacing: -0.005em
  body-3:
    fontFamily: Pretendard
    fontSize: 13px
    fontWeight: 400
    lineHeight: 1.5
  label-m:
    fontFamily: Pretendard
    fontSize: 15px
    fontWeight: 600
    lineHeight: 1.25
    letterSpacing: -0.005em
  label-s:
    fontFamily: Pretendard
    fontSize: 13px
    fontWeight: 600
    lineHeight: 1.25
  caption:
    fontFamily: Pretendard
    fontSize: 12px
    fontWeight: 500
    lineHeight: 1.4
  score:
    fontFamily: Pretendard
    fontSize: 28px
    fontWeight: 700
    lineHeight: 1.3
    letterSpacing: -0.02em
    fontFeature: "tnum"
rounded:
  radius-xs: 4px
  radius-s: 8px
  radius-m: 12px
  radius-xl: 16px
  radius-2xl: 20px
  radius-3xl: 24px
  radius-full: 999px
spacing:
  space-1: 4px
  space-2: 8px
  space-3: 12px
  space-4: 16px
  space-5: 20px
  space-6: 24px
  space-8: 32px
  space-10: 40px
  space-12: 48px
  space-16: 64px
components:
  top-app-bar:
    backgroundColor: "{colors.white}"
    textColor: "{colors.text-primary}"
    typography: "{typography.title-2}"
    height: 56px
  tab:
    backgroundColor: "{colors.white}"
    textColor: "{colors.text-secondary}"
    typography: "{typography.label-m}"
    rounded: "{rounded.radius-full}"
    height: 34px
    padding: 16px
  tab-selected:
    backgroundColor: "{colors.fill-primary}"
    textColor: "{colors.text-alt}"
    typography: "{typography.label-m}"
    rounded: "{rounded.radius-full}"
    height: 34px
    padding: 16px
  chip:
    backgroundColor: "{colors.white}"
    textColor: "{colors.text-secondary}"
    typography: "{typography.body-2}"
    rounded: "{rounded.radius-full}"
    height: 34px
    padding: 16px
  card:
    backgroundColor: "{colors.white}"
    rounded: "{rounded.radius-3xl}"
    padding: 24px
  board-row:
    backgroundColor: "{colors.white}"
    rounded: "{rounded.radius-xl}"
    padding: 20px
  badge-match:
    backgroundColor: "{colors.verdict-match-weak}"
    textColor: "{colors.verdict-match}"
    typography: "{typography.label-s}"
    rounded: 6px
    height: 22px
    padding: 6px
  badge-similar:
    backgroundColor: "{colors.verdict-similar-weak}"
    textColor: "{colors.verdict-similar}"
    typography: "{typography.label-s}"
    rounded: 6px
    height: 22px
    padding: 6px
  badge-different:
    backgroundColor: "{colors.verdict-different-weak}"
    textColor: "{colors.verdict-different}"
    typography: "{typography.label-s}"
    rounded: 6px
    height: 22px
    padding: 6px
  badge-neutral:
    backgroundColor: "{colors.fill-secondary}"
    textColor: "{colors.text-secondary}"
    typography: "{typography.label-s}"
    rounded: 6px
    height: 22px
    padding: 6px
  button-secondary:
    backgroundColor: "{colors.fill-secondary}"
    textColor: "{colors.text-primary}"
    typography: "{typography.label-s}"
    rounded: 10px
    height: 32px
    padding: 12px
  text-button:
    textColor: "{colors.text-brand}"
    typography: "{typography.label-s}"
  progress-bar:
    backgroundColor: "{colors.grey-200}"
    rounded: "{rounded.radius-full}"
    height: 6px
  quote-block:
    backgroundColor: "{colors.fill-secondary}"
    textColor: "{colors.text-primary}"
    typography: "{typography.body-2}"
    rounded: "{rounded.radius-m}"
    padding: 12px
  quote-highlight-match:
    backgroundColor: "{colors.verdict-match-weak}"
    textColor: "{colors.text-primary}"
  quote-highlight-similar:
    backgroundColor: "{colors.verdict-similar-weak}"
    textColor: "{colors.text-primary}"
  quote-highlight-different:
    backgroundColor: "{colors.verdict-different-weak}"
    textColor: "{colors.text-primary}"
---

# 브랜딩

구글 스티치의 `DESIGN.md` 형식으로 쓴 문서다(`02_research/branding/branding.md` 6장). 위의 값(토큰)이 기준이고, 아래의 글은 그 값을 어떻게 쓰는지를 설명한다. 눈으로 보는 것은 `branding.html`이다.

## Overview

사용자가 토스의 `design.md`를 브랜딩에 넣자고 했다(2026-10-11). 이 문서는 그 문서(토스 디자인 시스템 TDS를 정리한 것)의 시각 언어를 가져와 이 서비스에 맞춘 것이다. 색, 글자의 크기, 여백, 모서리, 움직임의 값은 그 문서의 것을 그대로 옮겼다. 이 서비스에만 있는 것(판정의 색, 근거와 원문을 보여 주는 방식)은 그 위에 더했고, 더한 값은 "이 서비스가 더한 것"이라고 적었다.

가져온 것은 시각 언어다. 토스의 금융 화면, 로고, 이름은 가져오지 않는다. 그 문서도 성격이 다른 제품에 금융 서비스의 개념을 그대로 옮기지 말라고 적고 있다.

주려는 인상은 그 문서의 표현을 빌리면 "조용히 일을 처리하고, 사용자의 시간을 낭비시키지 않는" 도구다. 이 서비스에서는 이렇게 된다.

- **근거가 보이는 도구.** 화면에서 가장 먼저 눈에 들어오는 것이 점수가 아니라 판정과 근거여야 한다.
- **정하는 사람은 채용 담당자다.** 서비스는 판정과 근거를 내놓고, 채용 담당자가 근거를 이력서 원문과 나란히 보고 판단한다. 동의와 수정을 받는 것은 이 과제에서 만들지 않는다(`03_prd/prd.md` 3.2).
- **필요한 정보가 먼저다.** 채용 담당자는 서류 한 건을 짧게 본다(`02_research/screening/screening.md` 2장). 판단에 필요한 것(누가 어느 조건에서 걸렸는가, 근거가 무엇인가)을 먼저 보이고, 그렇지 않은 것(점수의 식, 판정한 이유의 긴 글, 점수에 넣지 않는 항목)은 눌러야 보이게 한다.

전체의 분위기는 차갑고 절제된 바탕에, 선명한 파랑 하나다.

## Colors

- **파랑(`fill-brand` #3182f6)은 하나뿐인 강조색이다.** 지금 고른 것과 누를 수 있는 것에만 쓴다. 고른 탭의 밑줄, 펼친 지원자의 테두리, 초점의 테두리다. 글자로 쓸 때는 한 단계 어두운 `text-brand`(#1b64da)를 쓴다. 흰 바탕에서 #3182f6은 대비가 3.71이라 작은 글자의 기준(4.5)에 못 미치고, #1b64da는 5.41이다.
- **회색은 차가운 파랑 기운이 도는 중성색이다.** 글자는 `text-primary`(grey-900, 순수한 검정이 아니다), 보조 글자는 `text-secondary`(grey-700), 더 흐린 글자는 `text-tertiary`(grey-600)다. 보조 표면은 `fill-secondary`(grey-100), 기본 줄은 `border-secondary`(grey-200)다. 회색 표면 위의 흐린 글자는 `text-secondary`를 쓴다. grey-600은 회색 표면 위에서 대비가 4.19다.
- **판정의 색은 판정에만 쓴다.** 이 서비스가 더한 것이다.

| 판정 | 글자 | 옅은 바탕 | 옅은 바탕 위에서의 대비 |
| :- | :- | :- | :- |
| 일치 | `verdict-match` #007738 (토스의 green-500) | `verdict-match-weak` #dcf9e4 | 5.06 |
| 비슷 | `verdict-similar` #9f5100 | `verdict-similar-weak` #ffecd0 | 4.97 |
| 다름 | `verdict-different` #c2152f | `verdict-different-weak` #ffe1e1 | 4.97 |

- 비슷과 다름의 글자색은 토스의 orange-500(#ff8800), red-500(#f04452)을 그대로 쓰지 않았다. 옅은 바탕 위에서 대비가 2.07, 3.03이라 읽기 어렵다. 같은 색상에서 어둡게 한 값이다. 옅은 바탕은 토스 문서가 badge의 바탕을 "base hue에서 밝기를 끌어올린 값"으로 쓴다고 적은 것을 따랐다.
- 대비는 계산한 값이다.

## Typography

글꼴은 Pretendard 하나다. 토스의 글꼴(Toss Product Sans)은 바깥에서 쓸 수 없고, 토스 문서가 가장 가까운 무료 글꼴로 Pretendard를 든다. 글꼴 파일을 바깥에서 불러오지 않는다. 없는 컴퓨터에서는 시스템 글꼴로 보인다.

| 이름 | 쓰는 곳 |
| :- | :- |
| `display-2` | 직무의 이름. 화면에서 가장 큰 글자다 |
| `h4` | 구역의 이름 |
| `title-2` | 지원자, 카드의 제목, 탭 |
| `body-1` | 직무 아래의 한 줄 |
| `body-2` | 본문. 조건, 근거의 구절, 이력서 원문 |
| `body-3`, `caption` | 설명 |
| `label-m`, `label-s` | 버튼과 판정 표시의 글자 |
| `score` | 점수. 이 서비스가 더한 것이다 |

- 굵은 제목은 자간을 -1.5%~-2%로 좁힌다. 본문은 15px에 줄 높이 1.5다.
- 점수, 조건의 수, 식의 숫자는 폭이 같은 숫자(tabular)로 쓴다.

## Layout

- 기본 단위는 4px이다. 화면의 바깥 여백은 `space-6`(24px), 줄과 줄 사이는 `space-4`(16px), 붙어 있는 요소 사이는 `space-2`(8px), 구역 사이는 `space-12`~`space-16`이다.
- 화면은 하나이고 가운데 정렬이다. 최대 폭은 1120px다. 토스는 모바일이 기본이라 한 줄로 쌓는다. 이 화면은 책상에서 쓰는 도구라, 넓은 화면에서만 근거와 이력서 원문을 두 칸으로 나란히 둔다. 좁으면 한 줄로 쌓는다.
- 바탕은 `fill-secondary`(grey-100)이고 내용은 흰 카드 위에 둔다.

## Elevation & Depth

평면이 기본이다. 카드는 그림자가 아니라 바탕색의 차이(회색 바탕 위의 흰 카드)로 구분한다. 누른 상태는 그림자가 아니라 어두운 겹이다.

움직임은 세 가지 길이 안에서만 쓴다. 120ms(누름), 200ms(전환), 320ms(펼침). 튀어 오르는 움직임과 반짝이는 뼈대(skeleton shimmer)를 쓰지 않는다. 받는 중은 점 세 개로 보인다.

## Shapes

둥글지만 귀엽지 않게 한다. 큰 카드는 `radius-3xl`(24px), 랭킹의 줄은 `radius-xl`(16px), 인용 상자는 `radius-m`(12px), 판정 표시는 6px, 작은 버튼은 10px다. 기본 줄은 1px의 `border-secondary`다. 2px의 장식용 줄과 왼쪽에 색 띠를 두른 카드를 쓰지 않는다.

## Components

토스 문서의 컴포넌트 가운데 이 화면에 쓰는 것이다. 괄호 안은 토스 문서에서의 이름이다.

- **top-app-bar (top-app-bar):** 화면 맨 위에 고정된 56px 높이의 막대. 흰 바탕에 아래로 1px의 줄이다. 왼쪽에 아이콘, 맨 오른쪽에 메뉴의 이름을 둔다. 지금 있는 메뉴는 아래에 2.5px의 파란 줄로 표시한다. 스크롤해도 그 자리에 있다. 공고를 고르는 것은 이 막대에 두지 않는다. 그것은 메뉴 안의 일이다.
- **tab, tab-selected (chip):** 메뉴 안에서 공고를 고른다. 알약 모양이고, 고른 것은 `fill-primary` 바탕에 흰 글자로 바뀐다. 고른 탭은 글자가 진해지고 아래에 2.5px의 파란 줄이 생긴다.
- **chip (chip):** 높이 34px의 알약 모양. 흰 바탕에 1px의 줄이다. 직무 아래에 담당업무를 늘어놓는 데 쓴다. 누르는 것이 아니다.
- **card:** 흰 표면. 조건의 묶음(자격요건, 우대사항)을 담는다. 점수에 넣지 않는 참고사항은 바탕 없이 줄만 두른 카드로 구분한다.
- **board-row (board-row):** 랭킹의 줄. 순위의 숫자, 지원자, 조건마다의 판정, 오른쪽에 점수가 있다. 펼친 줄은 1.5px의 파란 테두리를 두른다.
- **badge-match, badge-similar, badge-different (badge):** 판정의 표시. 높이 22px, 모서리 6px, 옅은 바탕에 판정의 색 글자다. 표시 안에 판정의 이름을 글자로 적는다.
- **badge-neutral:** "이력서에 적혀 있지 않음"과 "근거를 확인하지 못함". 회색 바탕이다. "근거를 확인하지 못함"은 바탕 없이 점선 테두리를 둘러 구분한다. 랭킹의 줄에서는 "없음", "미확인"으로 줄여 적는다.
- **button-secondary (button S), text-button:** 누르는 것. 버튼의 글자는 일어날 일을 그대로 말한다("근거 보기", "판정한 이유 보기").
- **progress-bar:** 점수 옆의 막대. 길이가 점수다. 채운 부분은 `fill-primary`(grey-900)다. 파랑으로 칠하지 않는다. 점수에는 좋고 나쁨의 선이 없기 때문이다.
- **quote-block:** 근거의 구절. 회색 바탕의 둥근 상자에 이력서의 글자를 그대로 넣는다. 이 서비스가 더한 것이다.
- **quote-highlight-match, -similar, -different:** 이력서 원문에서 고른 구절에 칠하는 바탕. 색은 그 조건의 판정을 따른다. 이 서비스가 더한 것이다.

테두리와 점선은 형식의 값으로 적을 수 없어서 글로 적었다.

## Do's and Don'ts

색과 모양

- 파랑은 한 화면에서 지금 고른 것과 누를 수 있는 것에만 쓴다. 강조색을 둘 이상 쓰지 않는다.
- 판정을 색만으로 구분하지 않는다. 판정의 이름을 글자로 함께 적는다(`02_research/branding/branding.md` 2.6).
- "이력서에 적혀 있지 않음"을 다름과 같은 빨강으로 칠하지 않는다. 적혀 있지 않은 것이 결격처럼 보이기 때문이다.
- 점수를 색으로 칠하지 않는다. 점수는 공고 하나 안에서 지원자끼리 비교하는 값이어서, 몇 점부터 좋다는 선이 없다.
- 그라디언트, 안쪽 그림자, 2px의 장식용 줄, 왼쪽에 색 띠를 두른 카드를 쓰지 않는다.
- 근거의 구절은 원문 그대로 보여 준다.

정보

- 판단에 필요한 정보를 먼저 보인다. 누가 몇 위인가, 어느 조건이 일치이고 어느 조건이 다름인가, 근거가 무엇인가.
- 필요하지 않은 정보는 눌러야 보이게 한다. 점수의 식, 판정한 이유의 긴 글, 점수에 넣지 않는 항목(담당업무, 참고사항), 판정 표시의 설명.
- 시스템이 쓰는 값(키, trace id, 모델의 이름, 토큰 수)은 화면에 보이지 않는다.

말투

토스 문서를 따라 해요체로 쓴다. 한 문장은 무엇인지를 말한다. 과장과 권유를 쓰지 않는다.

| 원칙 | 쓴다 | 쓰지 않는다 |
| :- | :- | :- |
| 해요체로 쓴다 | "이력서에서 그 구절을 보여드려요" | "표시합니다", "표시한다" |
| 사실을 적는다 | "자격요건 3개 중 2개가 일치해요" | "우수한 지원자예요" |
| 확인하지 못한 것은 그렇게 적는다 | "이력서에 적혀 있지 않음", "근거를 확인하지 못함" | "미달", "부적합" |
| 서비스가 정한 것처럼 말하지 않는다 | "비슷으로 판정했어요" | "합격권", "추천" |
| 점수의 뜻을 숨기지 않는다 | "이 공고가 적은 조건을 이력서에서 확인한 정도예요" | "적합도", "매칭률" |
| 버튼은 일어날 일을 말한다 | "근거 보기", "다시 불러오기" | "여기를 눌러주세요", "확인" |
| 실패는 다음에 할 일을 알린다 | "불러오지 못했어요. 다시 불러올 수 있어요" | "오류가 발생했습니다" |
