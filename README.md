# 당토 디자인 (dangto-design)

AI 코딩 도구(Claude Code)가 화면을 만들 때 **당근과 토스처럼** 만들게 하는 디자인 스킬입니다.

목표는 "배우지 않아도 바로 쓰는 화면"입니다. 화려하게 꾸미는 대신 정보의 위계와 덜어내기로 완성도를 냅니다.

![가상의 도서관 자리 신청 앱: 자리 고르기, 맡기 전 확인, 내 자리(다크)](examples/library-seat.png)

가상의 도서관 자리 신청 앱에 이 스킬을 적용한 예시입니다. 한 화면에 질문 하나, 누르기 전에 결과 보여 주기, 핵심 숫자 하나, 다크 모드를 보여 줍니다.

## 무엇이 들어 있나

- **원칙 6개:** 배우지 않아도 이해된다, 한 화면에 목표 하나, 덜어낸다, 결과를 예측할 수 있게, 사용자 머릿속 개념대로 묶는다, 빈 화면과 오류도 설계한다
- **바로 쓰는 CSS 토큰:** 회색 10단계, 글자, 간격, 모서리, 그림자, 움직임, 다크 모드
- **강조색 프리셋 6개:** 숲, 바다, 제비꽃, 노을, 개나리, 먹. 색 하나만 바꾸면 연한 색, 누름 색, 다크 모드 색이 따라 바뀌고 대비 4.5:1을 넘깁니다
- **다이얼 두 개:** 밀도(편안, 촘촘), 개성(담백, 브랜드)
- **부품은 TDS, SEED에서:** 앱인토스면 TDS Mobile, React면 SEED React(브랜드색은 고른 색으로 바꿈). 그 밖에서는 TDS와 SEED 문서를 읽고 구조와 동작을 그대로 따라 새로 짬(코드는 복사하지 않음)
- **화면 패턴:** 목록 줄, 버튼 위계, 스위치, 대화상자, 토스트, 빈 화면, 오류, 로딩, 입력 양식, 관리 화면
- **문구 규칙:** 해요체, 누른 결과를 말하는 버튼, 쉬운 말, 바꿔 쓰기 예시
- **서체:** Pretendard 파일 4종 동봉(인터넷 없이도 사용), 브랜드 제목 서체 6종 목록, 디자인 요소용 리틱체 안내
- **AI 티 금지 목록:** 같은 크기 숫자 카드 늘어놓기, 모든 줄에 "보기" 링크, 모든 줄에 색 태그 같은 습관
- **완성 전 점검표:** 결과를 보여 주기 전에 스스로 확인합니다

![다이얼 조합 네 가지](examples/dials.png)

![같은 관리 화면을 스킬 없이(왼쪽), 스킬로(오른쪽) 만든 비교](examples/before-after-admin.png)

## 설치

### 방법 1. 플러그인으로 설치 (추천)

Claude Code에서 아래 두 줄을 차례로 입력합니다.

```
/plugin marketplace add yhk1m/dangto-design
/plugin install dangto-design@dangto-design
```

업데이트가 나오면 `/plugin marketplace update dangto-design`으로 받습니다.

### 방법 2. 폴더 복사

이 저장소의 `skills/dangto-design` 폴더를 통째로 아래 위치에 복사합니다.

- Windows: `C:\Users\<사용자이름>\.claude\skills\dangto-design`
- macOS, Linux: `~/.claude/skills/dangto-design`

한 프로젝트에서만 쓰려면 그 프로젝트의 `.claude/skills/dangto-design`에 넣습니다.

## 사용법

### 1. 그냥 요청하기

설치하면 화면을 만들어 달라는 요청에 알아서 쓰입니다.

```
학생들이 체험학습을 신청하는 휴대폰 화면 만들어 줘
```

### 2. 이름으로 부르기

확실히 이 스킬을 쓰게 하려면 이름을 붙입니다.

```
/dangto-design 도서실 대출 관리 화면 만들어 줘
```

### 3. 색과 다이얼 정해 주기

말하지 않으면 스킬이 알아서 정합니다. 원하는 게 있으면 요청에 함께 적습니다.

| 정할 것 | 고를 수 있는 값 | 정하지 않으면 |
|---|---|---|
| 강조색 | 프리셋 이름(숲, 바다, 제비꽃, 노을, 개나리, 먹) 또는 색 값(`#2b63d9`) | 숲 |
| 보조색, 셋째 색 | 색 값. 머리 띠, 배지, 차트에만 쓰고 버튼에는 쓰지 않음 | 없음 |
| 밀도 | 편안, 촘촘 | 사용자 화면은 편안, 관리 화면은 촘촘 |
| 개성 | 담백, 브랜드 | 담백 |

```
/dangto-design 급식 알림 앱 설정 화면. 강조색은 노을, 다크 모드도 같이
/dangto-design 우리 학원 관리자 화면. 강조색 #6847d6, 촘촘하게
/dangto-design 동아리 홍보 앱 첫 화면. 개성은 브랜드, 보조색 #1f2a44
```

### 4. 있는 화면 다듬기

```
/dangto-design 이 화면 점검하고 고쳐 줘
```

스킬의 점검표와 "AI 티" 목록으로 검사하고, 어긋난 곳을 고칩니다.

## 쓰기 좋은 곳, 맞지 않는 곳

- **좋은 곳:** 앱 화면, 웹 도구, 관리자 화면, 설정, 입력 양식, 목록, 빈 화면과 오류 화면
- **맞지 않는 곳:** 랜딩 페이지, 포트폴리오, 슬라이드처럼 "보여 주는" 화면

## 어디서 왔나

당근(SEED Design)과 토스(TDS, 제품 원칙, UX 글쓰기)의 공개 자료가 중심입니다. 관리 화면과 브랜드 표현은 LINE, 원티드, 채널톡, 쏘카, 뱅크샐러드, 배민, 29CM, 카카오페이의 공개 자료에서 보탰습니다. 규칙마다 근거 회사와 출처 주소는 [`references/sources.md`](skills/dangto-design/references/sources.md)에 있습니다.

**알림:** 이 스킬은 각 회사의 공식 자료가 아닙니다. 공개된 디자인 시스템 문서와 글을 읽고 개인이 정리한 것입니다. 회사 이름과 브랜드는 각 회사의 것이며, 프리셋 색은 특정 회사의 대표색을 그대로 쓰지 않았습니다.

## 파일 구성

```
skills/dangto-design/
  SKILL.md               시작 절차, 원칙, 핵심 규칙, 금지 목록, 점검표
  references/tokens.md   색, 글자, 간격, 모서리, 그림자, 움직임 값
  references/accent.md   강조색 프리셋과 직접 넣은 색 검사법
  references/components.md TDS, SEED 부품 가져오기
  references/fonts.md    서체 고르기와 불러오기
  references/patterns.md 화면 패턴
  references/writing.md  문구 규칙과 바꿔 쓰기 예시
  references/sources.md  근거와 출처
  assets/tokens.css      바로 복사해 쓰는 CSS
  assets/fonts.css       Pretendard를 파일로 불러오는 CSS
  assets/fonts/          Pretendard 4종과 라이선스(SIL OFL 1.1)
  assets/examples/       적용 예시 그림과 코드
```

## 서체 라이선스

- **Pretendard:** Copyright (c) 2021 Kil Hyung-jin, SIL Open Font License 1.1. 파일과 라이선스를 `assets/fonts/`에 함께 넣었습니다.
- **리틱체(REATIC):** 리틱인더스트리의 서체로, 라이선스상 재배포가 금지되어 이 저장소에는 넣지 않았습니다. 필요하면 [공식 페이지](https://www.reaticindustry.com/reaticfont)에서 직접 내려받으세요.
- 브랜드 제목 서체 6종은 Google Fonts에서 불러옵니다(SIL OFL).

## 만든 사람

양정고등학교 지리교사 김용현  
문의: bgnlkim@gmail.com

## 라이선스

MIT
