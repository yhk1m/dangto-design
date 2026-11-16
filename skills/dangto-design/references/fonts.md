# 서체

## 1. 본문과 기본 제목: Pretendard

모든 화면의 기본 서체다. 당근(SEED)이 윈도우에서 쓰고, 원티드, 쏘카 등 여러 회사가 쓰는 한국어 UI 서체다. SIL Open Font License 1.1이라 상업적 사용과 파일 동봉 배포가 모두 된다.

**불러오는 방법 (둘 중 하나)**

1. 인터넷 주소(CDN). 대부분의 웹 프로젝트.
   ```html
   <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/orioncactus/pretendard@v1.3.9/dist/web/static/pretendard.min.css">
   ```
2. 스킬에 들어 있는 파일. 인터넷이 막힌 곳(학교 전산실, 오프라인 수업, 사내망), 외부 요청을 줄이고 싶을 때.
   - `assets/fonts/`의 `Pretendard-Regular/Medium/SemiBold/Bold.woff2`(400, 500, 600, 700)와 `OFL-Pretendard.txt`를 프로젝트의 `fonts/` 폴더로 복사한다. 라이선스 파일도 꼭 함께 복사한다.
   - `assets/fonts.css`를 복사해서 `tokens.css`보다 먼저 불러온다. 경로가 다르면 `url()`만 고친다.

서체가 안 불러와져도 `tokens.css`의 글꼴 목록이 Apple SD Gothic Neo, 맑은 고딕 순서로 대신한다.

## 2. 브랜드 개성의 제목 서체 (하나만 고른다)

개성 다이얼이 `브랜드`일 때만 쓴다. **큰 제목, 숫자, 홍보 띠에만** 쓰고 본문, 버튼, 표, 입력 칸은 Pretendard를 그대로 둔다. 모두 무료이고 Google Fonts에서 바로 불러올 수 있다(SIL OFL).

| 성격 | 서체 | 어울리는 곳 | 불러오기 (`family=` 값) |
|---|---|---|---|
| 단정한 명조 | 고운바탕 Gowun Batang | 잡지, 독서, 전시, 차분한 기록 | `Gowun+Batang:wght@700` |
| 고전적 명조 | 나눔명조 Nanum Myeongjo | 공식 안내, 상장, 역사 | `Nanum+Myeongjo:wght@800` |
| 굵은 고딕 | 검은고딕 Black Han Sans | 행사, 대회, 강한 제목 | `Black+Han+Sans` |
| 각진 고딕 | 도현 Do Hyeon | 게임, 순위, 숫자 강조 | `Do+Hyeon` |
| 둥근 고딕 | 고운돋움 Gowun Dodum | 어린이, 가족, 다정한 서비스 | `Gowun+Dodum` |
| 손글씨 | 개구 Gaegu | 동아리, 메모, 가벼운 홍보 | `Gaegu:wght@700` |

```html
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Gowun+Batang:wght@700&display=swap">
```
```css
[data-personality="brand"] { --font-display: "Gowun Batang", serif; }
```

고르는 기준: 서비스 성격 하나에 맞춰 하나만. 두 개 이상 섞지 않는다. 손글씨는 제목 한 줄까지만 쓴다(길면 읽기 어렵다).

## 3. 디자인 요소용 서체: 리틱체 (REATIC)

정사각형 칸에 자음, 모음, 받침을 블록처럼 쌓은 서체다. 획 두께가 일정하고 자간과 행간이 같아서 격자처럼 정렬된다. 글자라기보다 **그래픽 요소**로 쓴다.

- 만든 곳: 리틱인더스트리 (디자인 유준상), https://www.reaticindustry.com/reaticfont
- 종류: REATIC, REATIC Fixed 두 벌. 각각 너비 3종(Narrow, 기본, Wide) × 굵기 3종(Light, Regular, Bold)

**쓰는 곳 (디자인 요소가 중요할 때만)**
- 브랜드 개성에서 첫 화면의 큰 숫자나 짧은 제목 한두 단어 (예: D-4, 2027, 오픈)
- 홍보 배너, 포스터, 썸네일, 공유 이미지, 영상 자막처럼 이미지로 만드는 것
- 쓰지 않는 곳: 본문, 버튼, 입력 칸, 표, 긴 문장. 읽기용 서체가 아니다.

**라이선스 (배포 파일의 LICENSE.txt, 2026 기준)**
- 이미지 제작, 영상 제작, 인쇄물에 상업적, 비상업적으로 자유롭게 쓸 수 있다.
- **폰트 파일 유료 판매, 임의 수정과 가공, 무단 재배포는 금지.**

**그래서 이 스킬은 이렇게 다룬다**
- 스킬과 저장소에 리틱체 파일을 넣지 않는다. 사용자가 위 공식 주소에서 직접 내려받게 안내한다.
- 파일을 woff2로 바꾸거나 글자만 잘라 내는 일(서브셋)은 "가공"에 해당하므로 하지 않는다.
- **이미지, 영상, 인쇄물에는 쓴다.** 공식으로 허락된 용도다. 웹 화면의 그래픽은 가능하면 리틱체로 만든 이미지(PNG, SVG로 내보낸 글자 모양)로 넣는다.
- 웹사이트에 폰트 파일 자체를 올려 `@font-face`로 쓰는 것은 라이선스에 적혀 있지 않다. 파일이 방문자에게 전달되므로 재배포로 볼 수도 있다. 이렇게 쓰려면 먼저 리틱인더스트리에 문의하라고 사용자에게 알린다.
- 쓸 때는 화면 아래나 정보 화면에 "리틱체 by 리틱인더스트리"를 적어 출처를 밝힌다.

## 4. 공통 규칙

- 한 화면에 서체는 최대 2개(Pretendard + 제목 서체 하나).
- 토스 전용 서체(Toss Product Sans)는 공개되지 않았으므로 쓰지 않는다.
- 제목 서체에도 글자 크기 규칙(12px 이상)과 대비 4.5:1을 똑같이 적용한다.
- `font-display: swap`을 써서 서체를 받는 동안에도 글자가 보이게 한다.
