# UI 컴포넌트 가져오기 (TDS, SEED)

버튼, 바텀시트, 스위치, 입력 칸 같은 부품은 직접 처음부터 그리지 말고, 아래 순서로 공개 디자인 시스템에서 가져온다. 가져온 부품도 이 스킬의 규칙(강조색, 문구, 금지 목록, 점검표)을 따른다.

## 0. 먼저: 공통 부품 `assets/controls.css`

버튼, 체크, 라디오, 스위치, 세그먼트, 칩은 프로젝트마다 다시 그리면 사이트마다 모양이 조금씩 달라진다. 그래서 이 여섯 가지는 SEED 레시피(`@seed-design/css` 3.4.1, Apache-2.0)의 치수와 상태를 옮긴 `assets/controls.css` 하나로 통일한다. 아래 1절의 어떤 경우든(React가 아닌 프로젝트, 이미 만든 사이트에 당토를 입힐 때 포함) 이 부품들은 이 파일이 먼저다.

| 부품 | 쓰는 법 | SEED 치수 |
|---|---|---|
| 체크 | `<input type="checkbox">` 그대로 | 20px 네모, 모서리 4, 손가락 기기 24. 고르면 주색 채움 + 체크, 일부는 가로줄(`indeterminate`) |
| 라디오 | `<input type="radio">` 그대로 | 20px 원, 고르면 주색 원 안에 8px 점 |
| 스위치 | `<input type="checkbox" role="switch">`, 큰 것 `class="dt-switch-l"` | 38x24(손잡이 20), 큰 것 52x32(손잡이 26). 꺼짐 손잡이는 80% 크기 |
| 버튼 | `<button class="dt-btn dt-btn--primary">` | 기본 40, `--l` 52, `--s` 36, `--xs` 32 알약. 종류 primary, brand-weak, neutral, weak, outline, ghost, critical |
| 세그먼트 | `.dt-seg` 안에 `label > input[type=radio] + span` | 알약 모양 회색 길, 고른 칸은 떠 있는 흰 면, 칸 높이 34, 2~4칸 |
| 칩 | `<button class="dt-chip" aria-pressed>` 또는 `label.dt-chip > input[type=checkbox]` | 36(`--s` 32, `--l` 40), 고른 칩은 진한 회색 채움 |

- 색은 모두 tokens.css 변수라 테마색, 다크 모드, 촘촘 다이얼을 그대로 따른다. 모서리는 `--r-control`이라 브랜드 다이얼(0 또는 크게)도 따른다.
- 당토 규칙 때문에 SEED와 다르게 둔 곳: 빈 체크와 라디오 테두리, 꺼진 스위치 길은 대비 3:1을 넘도록 `--icon-muted`. 고른 칩은 주요 버튼과 헷갈리지 않게 주색 대신 진한 회색.
- 프로젝트에 원래 있던 클래스(`.btn` 등)를 지우기 어려우면, 그 클래스의 높이, 모서리, 굵기, 색, 상태를 controls.css 값과 같게 맞춘다. 모양이 같으면 이름은 달라도 된다.
- 바텀시트, 대화상자, 목록 한 줄, 하단 고정 버튼처럼 이 파일에 없는 부품은 아래 1절 순서대로 SEED나 TDS 문서를 읽고 짠다.
- TDS 체크는 동그라미 모양이라 SEED 네모와 섞이면 일관성이 깨진다. 앱인토스 미니앱(TDS 패키지)이 아니면 체크, 라디오, 스위치, 버튼 모양은 controls.css(SEED)로 정한다.

## 1. 어느 것을 쓸까

| 프로젝트 | 쓰는 것 | 이유 |
|---|---|---|
| 앱인토스(토스 앱 안의 미니앱) | **TDS Mobile 패키지** `@toss/tds-mobile` | 앱인토스용으로 제공되는 공식 부품 |
| React 웹앱, 그 밖의 React 프로젝트 | **SEED React 패키지** `@seed-design/react` + `@seed-design/css`. SEED에 없는 부품(하단 고정 버튼, 큰 제목 머리 등)은 아래 **TDS 문서 보고 새로 짜기** | SEED는 Apache-2.0 공개 라이선스라 어디서나 쓸 수 있음 |
| React가 아닌 프로젝트(HTML 한 장, 바닐라 JS, Vue, Svelte) | **TDS 문서 보고 새로 짜기**(소비자 화면, 편안 밀도) 또는 **SEED 문서 보고 새로 짜기**(관리 화면, 촘촘 밀도, TDS에 없는 부품). 값은 `assets/tokens.css` | 두 패키지 모두 React 전용 |

- 한 프로젝트에 TDS 패키지와 SEED 패키지를 함께 설치하지 않는다. (문서를 보고 새로 짜는 것은 섞어도 된다. 겉모양은 모두 tokens.css로 통일되기 때문이다.)
- 프로젝트에 이미 쓰는 부품 라이브러리(shadcn/ui, MUI 등)가 있으면 그것을 유지하되, 버튼, 체크, 라디오, 스위치, 세그먼트, 칩의 겉모양(크기, 모서리, 색, 상태)은 0절의 controls.css 값에 맞춘다. 라이브러리 자체를 SEED로 옮기는 것은 사용자가 바꾸자고 할 때만.

## 2. TDS Mobile (앱인토스 전용)

- 문서: https://tossmini-docs.toss.im/tds-mobile/ (AI용 목록: https://tossmini-docs.toss.im/tds-mobile/llms.txt)
- 설치: `npm install @toss/tds-mobile @emotion/react` (React 16.8~18)
- **라이선스 주의:** npm 패키지에 공개 라이선스 표시가 없다. 앱인토스 미니앱을 만들 때만 설치한다. 그 밖의 프로젝트에서는 설치하지 않고, 아래 2-1 방식으로 문서를 보고 새로 짠다.
- 자주 쓰는 부품: Button, BottomCTA, ListRow, Top, TextField, Switch, BottomSheet, Toast, Dialog, Result, Skeleton
- 쓰기 전에 그 부품의 문서 쪽을 열어 속성 이름을 확인한다. 기억으로 속성을 지어내지 않는다.
- 대화상자, 토스트, 바텀시트는 문서의 Overlay Extension 훅(`useDialog`, `useToast`, `useBottomSheet`)으로 띄운다.

**앱인토스 미니앱이면 함께 지킬 것** (출처: https://developers-apps-in-toss.toss.im/design/consumer-ux-guide, 검수에서 떨어지는 항목 포함)
- 화면 맨 위 내비게이션 바는 앱인토스가 주는 부품을 쓴다. 직접 그리지 않는다.
- 아래 탭 막대는 필수가 아니다. 쓰면 토스가 주는 떠 있는 모양(floating) 탭 막대를 그대로 쓰고 탭은 2~5개. 토스 앱 아래 탭처럼 보이는 탭 막대를 직접 만들지 않는다(사용자가 지금 어디 있는지 헷갈린다).
- 로고는 600x600px, 모서리 없는 정사각형, 라이트와 다크 양쪽에서 보이는 바탕. 서비스 이름은 한글로.
- 다크 패턴은 검수 탈락이다: 들어오자마자 뜨는 광고나 알림 동의 시트, 뒤로 가기에 뜨는 시트, 거절할 수 없는 선택, 메뉴를 눌렀는데 뜨는 화면 전체 광고, 결과를 알 수 없는 하단 버튼. (이 스킬 금지 목록과 같다.)
- 토스가 주는 아이콘과 그래픽은 화면 안에서만 쓴다. 로고, 썸네일에는 쓰지 않고, 앱인토스 밖으로 가져가지 않는다.
- 피그마 UI Kit도 앱인토스 미니앱을 만들 때만 쓸 수 있다(라이선스: https://developers-apps-in-toss.toss.im/design/prepare/figma-ui-license). 시안 폭은 375px.
- 앱인토스는 아직 다크 모드를 지원하지 않는다고 안내한다. 미니앱 시안은 라이트 모드로 만들고, 코드의 다크 모드 토큰은 그대로 둔다.
- 게임 미니앱은 화면 전체를 채운다. 웹뷰 여백, 반투명 영역, 회전 시 비율 깨짐이나 위아래 검은 띠가 없어야 하고, 노치와 카메라 구멍은 안전 영역으로 피한다.

## 2-1. TDS 문서 보고 새로 짜기 (앱인토스가 아닌 프로젝트)

TDS 문서에는 AI가 읽기 좋은 부품 설명이 있다. 부품의 **구조와 동작은 문서대로, 코드는 직접 새로** 짠다.

**문서 주소** (앞에 `https://tossmini-docs.toss.im`을 붙인다)
| 부품 | 문서 |
|---|---|
| 전체 목록 | `/tds-mobile/llms.txt` |
| 버튼 | `/tds-mobile/components/button/llm.txt` |
| 글자 버튼 | `/tds-mobile/components/text-button/llm.txt` |
| 하단 고정 버튼 | `/tds-mobile/components/BottomCTA/check-first/llm.txt`, `.../BottomCTA/Single/llm.txt`, `.../BottomCTA/Double/llm.txt`, `.../BottomCTA/fixed-bottom-cta/llm.txt` |
| 목록 한 줄 | `/tds-mobile/components/ListRow/list-row-overview/llm.txt`, `.../ListRow/list-row-components/llm.txt` |
| 묶음 제목 | `/tds-mobile/components/list-header/llm.txt` |
| 화면 제목 머리 | `/tds-mobile/components/top/llm.txt` |
| 입력 칸 | `/tds-mobile/components/TextField/text-field/llm.txt`, `.../TextField/text-area/llm.txt` |
| 검색 칸 | `/tds-mobile/components/search-field/llm.txt` |
| 스위치, 체크 | `/tds-mobile/components/switch/llm.txt`, `/tds-mobile/components/checkbox/llm.txt` |
| 탭, 세그먼트 | `/tds-mobile/components/tab/llm.txt`, `/tds-mobile/components/segmented-control/llm.txt` |
| 바텀시트 | `/tds-mobile/components/bottom-sheet/llm.txt` |
| 대화상자 | `/tds-mobile/components/Dialog/dialog/llm.txt`, `.../Dialog/confirm-dialog/llm.txt`, `.../Dialog/alert-dialog/llm.txt` |
| 토스트 | `/tds-mobile/components/toast/llm.txt` |
| 결과, 빈 화면 | `/tds-mobile/components/result/llm.txt` |
| 로딩 | `/tds-mobile/components/skeleton/llm.txt` |
| 진행 표시 | `/tds-mobile/components/progress-bar/llm.txt`, `/tds-mobile/components/progress-stepper/llm.txt` |
| 표 한 줄 | `/tds-mobile/components/table-row/llm.txt` |
| 대화상자 고르기(하나 버튼과 두 버튼) | `/tds-mobile/components/Dialog/dialog/llm.txt` |
| 목록 아래 더 보기 | `/tds-mobile/components/list-footer/llm.txt` |
| 화면 아래 안내 글 | `/tds-mobile/components/bottom-info/llm.txt` |
| 메뉴 | `/tds-mobile/components/menu/llm.txt` |
| 말풍선 도움말 | `/tds-mobile/components/tooltip/llm.txt` |
| 슬라이더 | `/tds-mobile/components/slider/llm.txt` |
| 수량 고르기(+, -) | `/tds-mobile/components/numeric-spinner/llm.txt` |
| 별점 | `/tds-mobile/components/rating/llm.txt` |
| 약관 동의 | `/tds-mobile/components/Agreement/v4/llm.txt` |
| 숫자 자판 | `/tds-mobile/components/Keypad/number-keypad/llm.txt` |
| 띄우는 방법(토스트, 시트, 대화상자) | `/tds-mobile/hooks/OverlayExtension/use-toast/llm.txt` 등 |

목록에 없는 부품은 전체 목록(`llms.txt`)에서 찾는다.

**순서**
1. 만들 부품의 문서를 읽는다. 기억으로 짐작하지 않는다.
2. 문서에서 가져올 것: 구성(어떤 영역이 어디에 오는지), 크기 단계, 종류(채움과 연한 채움 등), 상태(누름, 비활성, 로딩), 동작(키보드 위로 붙기, 스크롤 시 그림자 등), 쓰는 때와 쓰지 않는 때.
3. 값(색, 글자, 간격, 모서리)은 TDS 값이 아니라 `tokens.css`의 변수로 채운다.
4. 코드는 프로젝트의 방식(HTML, React, Vue)대로 새로 짠다.
5. 부품 위에 출처 주석을 한 줄 단다. 예: `/* 구조 참고: TDS Mobile BottomCTA 문서 */`

**하지 않는 것**
- TDS 패키지의 소스 코드, 이미지, 아이콘, 에셋을 복사하지 않는다.
- 토스 전용 서체(Toss Product Sans)를 쓰지 않는다. Pretendard나 시스템 글꼴을 쓴다.
- 토스 파랑(#3182f6)을 그대로 쓰지 않는다. 고른 테마색을 쓴다.
- 클래스 이름, 화면 글, 파일 이름에 "toss", "tds"를 넣지 않는다. 토스 공식 화면처럼 보이게 하지 않기 위해서다.

## 2-2. SEED 문서 보고 새로 짜기

React가 아니거나, 관리 화면처럼 TDS에 맞는 부품이 없을 때는 SEED 문서를 같은 순서로 읽고 새로 짠다.
- 디자인 규칙: `https://seed-design.io/components/llms.txt` (부품마다 `https://seed-design.io/llms/components/<이름>.txt`에 구성, 속성, 가이드라인)
- 상호작용 상태: `https://seed-design.io/llms/react/components/concepts/interaction-states.txt`
- SEED는 Apache-2.0이라 CSS 레시피(`@seed-design/css/recipes/*.css`)를 참고하거나 일부 가져와도 된다. 가져오면 파일 위에 출처와 라이선스(Apache-2.0)를 주석으로 남긴다.

## 3. SEED React (당근, Apache-2.0)

**문서 (AI가 읽기 좋은 형식)**
- 전체 목록: https://seed-design.io/llms.txt
- React 부품 API: https://seed-design.io/react/llms.txt (부품마다 `https://seed-design.io/llms/react/components/<이름>.txt`)
- 디자인 규칙(Anatomy, Guidelines): https://seed-design.io/components/llms.txt

부품을 쓰기 전에 그 부품의 `.txt` 문서를 읽고, 속성과 사용 예를 확인한 뒤 쓴다.

**설치 (Vite 기준)**
```sh
npm install @seed-design/react @seed-design/css
npx @seed-design/cli@latest init          # seed-design.json 만들기 (Node 22 이상)
npx @seed-design/cli@latest add ui:action-button   # 필요한 부품만 하나씩 추가
```
```ts
// main.tsx 맨 위
import "@seed-design/css/base.css";
```
```html
<!-- 다크 모드는 시스템 설정을 따른다 -->
<html data-seed-color-mode="system">
```

**자주 쓰는 부품과 이 스킬의 패턴 짝**
| 이 스킬 패턴 | SEED 부품 |
|---|---|
| 버튼 위계 | Action Button (`brandSolid`, `neutralWeak` 등 variant) |
| 하단 고정 버튼 | Action Button을 화면 아래에 고정 (Safe Area와 함께) |
| 대화상자 | Alert Dialog |
| 바텀시트 | Bottom Sheet |
| 토스트, 스낵바 | Snackbar |
| 켜고 끄기 | Switch |
| 선택 칩, 필터 | Chip, Chip Tabs |
| 탭 | Tabs |
| 목록 한 줄 | List |
| 빈 화면, 오류 | Result Section |
| 누르면 줄어드는 반응 | Scale Feedback |
| 로딩 | Skeleton |
| 상태 표시 | Badge, Callout |
| 입력 칸 | Text Field Input, Text Field Textarea |

**강조색을 당근 주황에서 사용자가 고른 색으로 바꾸기 (필수)**

SEED의 기본 브랜드색은 당근 주황이다. 그대로 두면 다른 회사 브랜드처럼 보이므로, `base.css` 다음에 아래를 넣어 이 스킬의 테마색으로 바꾼다. `tokens.css`를 함께 불러온 상태여야 한다.

```css
:root, [data-seed-color-mode] {
  --seed-color-bg-brand-solid: var(--accent);
  --seed-color-bg-brand-solid-pressed: var(--accent-press);
  --seed-color-bg-brand-weak: var(--accent-weak);
  --seed-color-bg-brand-weak-pressed: var(--accent-weak-press);
  --seed-color-fg-brand: var(--accent-text);
  --seed-color-fg-brand-contrast: var(--accent-text);
  --seed-color-fg-on-brand-solid: var(--on-accent);
  --seed-color-stroke-brand-solid: var(--accent);
  --seed-color-stroke-brand-weak: var(--accent-weak-press);
}
```
바꾼 뒤 브랜드 버튼 하나를 그려서 색이 실제로 바뀌었는지 확인한다.

**아이콘:** SEED 아이콘(`@karrotmarket/react-monochrome-icon`, Apache-2.0)은 선 아이콘이라 써도 된다. 여러 색 아이콘(multicolor)은 쓰지 않는다.

**배포할 때:** Apache-2.0은 저작권 고지를 지키면 된다. npm으로 설치해서 쓰면 패키지 안의 LICENSE, NOTICE가 함께 들어간다. 부품 코드를 CLI로 복사해 고쳐 쓰는 경우에도 파일 맨 위 주석을 지우지 않는다.

## 4. 라이브러리 기본값과 이 스킬 규칙이 부딪힐 때

이 스킬 규칙이 이긴다.
- 브랜드색: 위처럼 테마색으로 바꾼다.
- 문구: 라이브러리 예시 문구("확인", "취소")를 그대로 쓰지 않는다. `references/writing.md`를 따른다.
- 강조 버튼 수: 라이브러리 예시에 채운 버튼이 여러 개 있어도 화면마다 하나만.
- 이모지, 가운데 점, 긴 대시 금지는 그대로.
