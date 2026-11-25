# 휴대폰과 태블릿

휴대폰 화면을 먼저 만들고, 폭이 넓어지면 배치를 바꾼다. 데스크톱 화면을 줄여서 휴대폰을 만들지 않는다. 태블릿은 휴대폰을 늘린 것도, 데스크톱을 줄인 것도 아닌 따로 설계하는 구간이다.

## 1. 구간과 판단 기준

| 구간 | 폭 | 대표 기기 | 점검 폭 |
|---|---|---|---|
| 휴대폰 | 744px 미만 | 아이폰, 갤럭시 (360~430) | 390 (필요하면 320, 360도) |
| 태블릿 세로 | 744~1023px | 아이패드 미니 744, 아이패드 820, 갤럭시 탭 800 | 820 |
| 태블릿 가로, 작은 노트북 | 1024~1366px | 아이패드 1180, 갤럭시 탭 1280, 아이패드 Pro 1366 | 1180 |
| 데스크톱 | 1280px 이상 | | 1280 |

- **배치는 폭으로, 손가락 크기는 입력 장치로 정한다.** 폭은 `@media (min-width: …)`, 터치인지는 `@media (pointer: coarse)`, 마우스를 올릴 수 있는지는 `@media (hover: hover)`로 본다. 태블릿에 키보드와 마우스를 붙이기도 하고, 노트북에도 터치 화면이 있다.
- 기기 이름이나 브라우저 정보(user agent)로 휴대폰, 태블릿을 가르지 않는다. 아이패드 화면 나누기(Split View)에서는 태블릿도 폭이 320~500px이 된다.
- 가로 태블릿은 높이가 744~820px뿐이다. 폭만 보지 말고 높이도 본다.
- 브라우저 확대 200%면 1280px 화면도 640px처럼 그려진다. 구간 경계마다 깨지지 않아야 한다.
- 기준 화면 하나(휴대폰 세로 360~420px 폭)로 설계하고 늘이고 줄인다. 기종마다 따로 맞춘 배치를 만들지 않고, 물리 화소로 좌표를 계산하지 않는다.
- 휴대폰 글자 크기를 키운 사용자(설정에서 160%, 200%)도 쓸 수 있어야 한다. 글자 크기는 `rem`으로, 높이는 고정하지 말고 `min-height`로. 글자가 커지면 목록 줄의 제목 칸을 넓히고(오른쪽 값은 아래로 내려도 된다), 시트는 제목까지 함께 스크롤되게 한다.

## 2. 가로 넘침 막기 (휴대폰에서 가로 스크롤바가 생기지 않게)

화면 전체가 옆으로 밀리는 가로 스크롤은 오류다. 가로로 스크롤되는 곳은 일부러 만든 표와 가로 카드 목록뿐이고, 그것도 그 칸 안에서만 움직인다.

**기본으로 넣는 것** (`tokens.css`에 들어 있다)
- `<head>`에 `<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">`. `maximum-scale=1`, `user-scalable=no`로 확대를 막지 않는다(눈이 불편한 사람이 못 쓴다).
- 그림, 영상, canvas, iframe은 `max-width: 100%`, 높이 `auto`.
- 긴 낱말, 주소, 파일 이름이 칸을 뚫지 않게 `overflow-wrap: anywhere`. 한국어 줄바꿈은 `word-break: keep-all`.
- 가로 모드 아이폰이 글자를 멋대로 키우지 않게 `text-size-adjust: 100%`.

**넘침을 만드는 흔한 원인과 고치는 법**

| 원인 | 고치는 법 |
|---|---|
| `width: 400px`처럼 고정한 폭 | `width: 100%; max-width: 25rem` |
| `100vw` (스크롤바 폭까지 들어가 데스크톱에서 넘침) | `100%` |
| flex 안 글자나 입력 칸이 줄어들지 않음 | 자식에 `min-width: 0`, 글자에는 말줄임(`text-overflow: ellipsis`)이나 줄바꿈 |
| 그리드 칸 `1fr`이 내용에 밀려 늘어남 | `minmax(0, 1fr)`, 칸 수는 `repeat(auto-fill, minmax(min(10rem, 100%), 1fr))` |
| 칩, 버튼 줄이 한 줄로 길게 늘어남 | `flex-wrap: wrap`, 또는 그 줄만 `.scroll-x` |
| 표 | `.scroll-x` 상자로 감싼다. 휴대폰에서 열이 4개를 넘으면 목록 한 줄로 바꾼다 |
| 닫힌 서랍, 메뉴를 `transform`으로 화면 밖에 숨김 | 닫혔을 때 `display: none`이나 `<dialog>`(닫히면 안 그려짐) |
| 음수 여백, 화면 밖으로 나간 장식 | 장식을 지우거나 감싼 칸에 `overflow: clip` |
| `pre`, 코드, 긴 숫자 | `overflow-x: auto`나 `white-space: pre-wrap` |

- **`body { overflow-x: hidden }`으로 덮어 가리지 않는다.** 넘친 내용이 잘려 안 보이고, `position: sticky`도 망가진다. 넘친 요소를 찾아서 고친다. 점검기가 넘친 요소 이름을 알려 준다.
- 화면 높이는 `100vh` 대신 `100dvh`(주소창이 늘었다 줄었다 해도 맞음). 처음부터 화면을 채우는 바탕은 `min-height: 100dvh`.
- 노치와 홈 막대: 화면 아래 고정한 것(탭 막대, 하단 버튼)은 아래 여백에 `env(safe-area-inset-bottom)`을 더한다. 위에 고정한 머리 막대는 `env(safe-area-inset-top)`. `viewport-fit=cover`가 있어야 값이 들어온다.
- 입력 칸 글자는 휴대폰에서 16px 이상. 16px보다 작으면 아이폰이 누를 때마다 화면을 확대한다. 촘촘 화면도 휴대폰 폭에서는 16px(`tokens.css`가 처리).

**일부러 만드는 가로 스크롤 (`.scroll-x`)**
- 표, 가로 카드 목록, 칩 줄에만. 그 칸 안에서만 움직이고 화면 전체는 움직이지 않는다.
- 넘친다는 것을 알 수 있게 마지막 항목이 반쯤 보이게 한다(항목 폭을 화면의 80~85%로). 좌우 여백은 `scroll-padding-inline`으로 맞춘다.
- `overscroll-behavior-x: contain`으로 끝까지 밀었을 때 브라우저 뒤로 가기가 걸리지 않게 한다.
- 가로 스크롤 안에 또 가로 스크롤을 넣지 않는다.

## 3. 휴대폰 메뉴 양식

**고르는 기준**

| 상황 | 양식 |
|---|---|
| 주요 목적지 2~5개 (홈, 기록, 내 정보처럼 자주 오감) | 아래 탭 막대 |
| 한 화면 안에서 같은 종류를 나누기 (전체, 안 읽음) | 위쪽 탭이나 세그먼트 |
| 자주 안 가는 보조 메뉴 (설정, 도움말, 로그아웃) | 머리 막대 오른쪽 아이콘이나 "내 정보" 탭 안 목록 |
| 메뉴가 6개 이상인 웹 도구, 관리 화면, 문서 사이트 | 햄버거 버튼 + 서랍 |
| 메뉴가 1~2개 | 메뉴 없이 머리 막대 버튼 |

- **탭 막대가 먼저다.** 주요 목적지를 햄버거 안에 숨기면 사람들이 거기 있는 줄 모른다. 당근과 토스 앱은 아래 탭 막대를 쓴다.
- 탭 막대와 햄버거를 함께 쓰지 않는다. 같은 메뉴를 두 곳에 두지 않는다.
- 한 화면의 목적지 메뉴는 하나다. 아래 탭 막대와 위쪽 탭이 둘 다 화면 전환이면 하나로 합친다.

**머리 막대 (위)**
```
[뒤로 또는 글자 로고]      화면 제목      [아이콘] [아이콘 또는 햄버거]
```
- 높이 56px(`--header-h`) + 위쪽 안전 영역. 바탕은 `--bg`나 `--surface`, 그림자 대신 스크롤했을 때만 아래 옅은 선.
- 왼쪽은 뒤로 가기(첫 화면이면 글자 로고), 가운데는 짧은 제목, 오른쪽은 아이콘 버튼 2개까지. 아이콘 버튼은 24px 선 아이콘, 누르는 영역 44px, `aria-label` 필수.
- 첫 화면에서는 가운데 제목 대신 왼쪽에 큰 제목을 둘 수 있다.

**아래 탭 막대**
- 항목 3~5개. 아이콘(24px 선) 위, 이름(12~13px) 아래. **이름은 항상 보인다.** 아이콘만 두지 않는다.
- 높이 56px(`--tabbar-h`) + `env(safe-area-inset-bottom)`. 바탕 `--surface`, 위쪽 옅은 선 하나. 그림자, 떠 있는 둥근 막대, 가운데 튀어나온 큰 버튼은 쓰지 않는다.
- 고른 탭은 `--text-strong` 글자 + 채운 아이콘이나 굵은 선, 나머지는 `--text-muted`. 강조색은 쓰지 않는다(화면의 주요 버튼 몫으로 남긴다).
- 항목은 각자 같은 폭으로 화면 전체를 나눈다. 누르는 영역이 칸 전체다.
- 맨 위 화면(탭 화면)에만 둔다. 상세 화면, 신청 흐름, 하단 고정 버튼이 있는 화면에서는 숨긴다. 탭 막대와 하단 고정 버튼을 위아래로 쌓지 않는다.
- 이미 고른 탭을 다시 누르면 그 화면 맨 위로 올라간다.
- 알림 수는 아이콘 오른쪽 위 작은 점이나 숫자 배지 하나. 숫자는 99까지, 넘으면 "99+".
- 마크업: `<nav aria-label="주요 메뉴">` 안에 링크, 지금 화면은 `aria-current="page"`.
- 본문 맨 아래가 탭 막대에 가리지 않게 본문 아래 여백에 탭 막대 높이를 더한다.

**햄버거 버튼과 서랍**
- 버튼: 세 줄 선 아이콘(24px, 굵기 2px, 끝 둥글게), 누르는 영역 44px, 머리 막대 오른쪽 끝. 왼쪽은 뒤로 가기와 로고 자리라서다. 한 제품 안에서는 위치를 바꾸지 않는다.
- 버튼 속성: `aria-label="메뉴 열기"`, `aria-expanded`, `aria-controls="서랍 id"`. 아이콘을 X로 바꾸는 애니메이션은 넣지 않는다. 닫기 버튼은 서랍 안에 따로 둔다.
- 서랍은 버튼이 있는 쪽(오른쪽)에서 들어온다. 폭 `min(20rem, 85vw)`, 높이 `100dvh`, 바탕 `--surface-raised`, 바깥쪽 모서리만 `--r-sheet`, 뒤 막은 `dialog::backdrop`.
- `<dialog>`와 `showModal()`로 만든다. 초점 가두기, Esc로 닫기, 닫혔을 때 안 그려짐(가로 넘침 없음)이 따라온다. 닫히면 초점을 햄버거 버튼으로 돌린다.
- 뒤 막을 누르거나, 메뉴 항목을 고르거나, Esc를 누르면 닫힌다. 열려 있는 동안 뒤 화면은 스크롤되지 않는다(`html:has(dialog[open]) { overflow: hidden }`).
- 서랍 안 구성: 위에 닫기 버튼과 (있으면) 사용자 이름, 그 아래 목록 한 줄 모양의 메뉴(높이 `--row-h`), 묶음이 있으면 작은 묶음 제목, 맨 아래 로그아웃 같은 보조 행동. 서랍 안에서도 목록만 스크롤된다.
- 지금 있는 메뉴는 `--accent-weak` 바탕 + `--accent-text` 글자 + 600 굵기, `aria-current="page"`. 왼쪽 강조 줄을 쓰면 모서리 없는 일자 막대.
- 들어올 때 250ms 감속, 나갈 때 150ms. 동작 줄이기 설정이면 바로 나타난다.
- 폭이 1024px 이상이면 햄버거를 없애고 메뉴를 머리 막대에 펼치거나 왼쪽 막대로 둔다. 넓은 화면에서 메뉴를 숨기지 않는다.

```html
<header class="app-header">
  <a class="logo" href="/">도서관</a>
  <button class="icon-btn" aria-label="메뉴 열기" aria-expanded="false" aria-controls="drawer">
    <svg viewBox="0 0 24 24" width="24" height="24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><path d="M4 6h16M4 12h16M4 18h16"/></svg>
  </button>
</header>
<dialog id="drawer" class="drawer" aria-label="메뉴">
  <button class="icon-btn" data-close aria-label="메뉴 닫기">
    <svg viewBox="0 0 24 24" width="24" height="24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><path d="M6 6l12 12M18 6 6 18"/></svg>
  </button>
  <nav><a href="/" aria-current="page">자리 신청</a><a href="/me">내 기록</a></nav>
</dialog>
```
```css
.icon-btn { width: var(--touch); height: var(--touch); display: grid; place-items: center; border: 0; background: none; color: var(--text-strong); border-radius: var(--r-control); }
.drawer {
  margin: 0 0 0 auto; inset: 0 0 0 auto; width: min(20rem, 85vw); max-width: none;
  height: 100dvh; max-height: none; padding: env(safe-area-inset-top) 0 env(safe-area-inset-bottom);
  border-radius: var(--r-sheet) 0 0 var(--r-sheet); overscroll-behavior: contain;
  translate: 100% 0;   /* 닫힐 때 나가는 자리 */
  transition: translate var(--dur-fast) var(--ease-exit), overlay var(--dur-fast) allow-discrete, display var(--dur-fast) allow-discrete;
}
.drawer[open] { translate: 0; transition-duration: var(--dur); transition-timing-function: var(--ease-enter); }
@starting-style { .drawer[open] { translate: 100% 0; } }
.drawer [data-close] { margin: var(--sp-2) var(--sp-2) var(--sp-2) auto; }   /* 햄버거와 같은 오른쪽 위 자리 */
html:has(.drawer[open]) { overflow: hidden; }
@media (min-width: 1024px) { .app-header [aria-controls="drawer"] { display: none; } }
```
```js
const btn = document.querySelector('[aria-controls="drawer"]'), d = document.getElementById('drawer');
btn.onclick = () => { d.showModal(); btn.setAttribute('aria-expanded', 'true'); };
d.addEventListener('close', () => { btn.setAttribute('aria-expanded', 'false'); btn.focus(); });
d.addEventListener('click', e => { if (e.target === d || e.target.closest('[data-close], nav a')) d.close(); });
```

## 4. 휴대폰 배치

- 화면 좌우 여백 `--page-x`(편안 20px, 촘촘 16px). 카드와 목록도 이 선에 맞춘다.
- 한 칸으로 쌓는다. 두 칸 나란히는 사진, 상품처럼 같은 종류의 작은 카드뿐.
- 주요 버튼은 하단 고정(`patterns.md`). 아래 안전 영역을 더한다.
- 대화상자는 바텀시트로 바꿀 수 있다. 시트 높이는 최대 `90dvh`, 넘치면 시트 안에서 스크롤.
- 가로로 눕힌 휴대폰은 높이가 360~430px뿐이다. 하단 고정 버튼, 머리 막대, 시트가 화면을 다 덮지 않는지 본다.
- 가장 좁은 320px 폭에서도 버튼 글자가 두 줄로 깨지거나 넘치지 않는지 본다.

## 5. 태블릿

**세로 태블릿 (744~1023px)**
- 한 칸이지만 휴대폰보다 넓게 쓴다. 본문과 양식은 `--content-max`(40rem, 640px)에서 멈추고 가운데. 글줄이 너무 길면 읽기 어렵고, 화면 폭 꽉 찬 입력 칸은 쓰기 불편하다.
- 같은 종류 카드, 짧은 선택지(4개 이하)는 2열.
- 좌우 여백 24px. 하단 고정 버튼은 화면 폭 전체가 아니라 본문 칸 폭(최대 `--content-max`)에 맞춘다.
- 아래 탭 막대는 그대로 써도 된다. 항목이 칸 가운데로 몰리게 막대 안 항목 묶음의 폭을 `--content-max`로 제한한다.
- 대화상자는 바텀시트 대신 가운데 대화상자(최대 폭 32rem). 바텀시트는 휴대폰에서만.

**가로 태블릿 (1024px 이상)**
- **두 칸이 기본이다.** 목록 화면은 왼쪽 목록(약 40%, 최소 20rem) + 오른쪽 상세. 휴대폰에서 "목록 → 상세"로 넘어가던 흐름이 한 화면에 나란히 놓인다. 칸은 두 개까지.
- 두 칸은 각자 스크롤된다. 칸 높이 `calc(100dvh - 머리 높이)`, 칸 안 스크롤에 `overscroll-behavior: contain`.
- 아래 탭 막대는 왼쪽 세로 막대(`--rail-w` 80px, 아이콘 위 이름 아래)로 옮긴다. 엄지가 닿는 화면 왼쪽 가장자리다.
- 햄버거는 없애고 메뉴를 펼쳐 둔다.
- 주요 버튼은 그 버튼이 속한 칸 아래에 두고 폭은 칸을 따른다(최대 28rem). 1180px 폭으로 늘린 버튼은 누르기도 읽기도 어렵다.
- 좌우 여백 32px, 전체 폭은 `--wide-max`(72rem)에서 멈추고 가운데.
- 높이가 744~820px뿐이므로 첫 화면 핵심(목록 첫 몇 줄, 상세의 주요 버튼)이 스크롤 없이 보여야 한다.

**모든 태블릿**
- 글자는 키우지 않는다. 본문 16px 그대로, 폭을 제한해서 읽기 좋게 만든다. 위계는 회색 단계와 굵기로.
- 손가락으로 쓰면(`pointer: coarse`) 누르는 영역 48px. 촘촘 화면도 터치 기기에서는 버튼과 줄 높이를 40~44px로 올린다(`tokens.css`가 처리).
- 마우스를 올려야만 보이는 정보(hover 메뉴, `title` 말풍선)를 두지 않는다. hover 효과는 `@media (hover: hover)` 안에서만.
- 끌기, 밀기, 집어 확대는 늘 버튼 대안(앞뒤 버튼, 확대 버튼)과 함께.
- 세로와 가로를 바꿔도 입력한 값, 고른 항목, 스크롤 위치가 남는다. 방향이 바뀔 때 화면을 다시 만들지 말고 CSS로 배치만 바꾼다.
- 화면 키보드가 아래 절반을 가린다. 입력 칸과 그 확인 버튼이 키보드 위에 보이게 한다.

## 6. CSS 틀

```css
/* 휴대폰 (기본): 한 칸 */
.page { padding-inline: var(--page-x); padding-bottom: calc(var(--sp-6) + env(safe-area-inset-bottom)); }
.has-tabbar .page { padding-bottom: calc(var(--tabbar-h) + var(--sp-6) + env(safe-area-inset-bottom)); }
.tabbar {
  position: fixed; inset: auto 0 0 0; z-index: 10;
  display: grid; grid-auto-flow: column; grid-auto-columns: 1fr;
  height: calc(var(--tabbar-h) + env(safe-area-inset-bottom)); padding-bottom: env(safe-area-inset-bottom);
  background: var(--surface); border-top: 1px solid var(--line);
}
.tabbar a { display: grid; place-items: center; align-content: center; gap: 2px; font-size: var(--fs-caption); color: var(--text-muted); text-decoration: none; }
.tabbar a[aria-current="page"] { color: var(--text-strong); font-weight: var(--fw-semibold); }

/* 태블릿 세로: 넓은 한 칸 */
@media (min-width: 744px) {
  .page > * { max-width: var(--content-max); margin-inline: auto; }
  .choices { grid-template-columns: repeat(2, minmax(0, 1fr)); }
  .tabbar { padding-inline: max(var(--page-x), (100% - var(--content-max)) / 2); }
}

/* 태블릿 가로 이상: 왼쪽 막대 + 두 칸 */
@media (min-width: 1024px) {
  .page > * { max-width: var(--wide-max); }   /* 제목과 두 칸의 왼쪽 선을 맞춘다 */
  .has-tabbar .page { padding-bottom: var(--sp-6); padding-left: calc(var(--rail-w) + var(--page-x)); }
  .tabbar { inset: 0 auto 0 0; width: var(--rail-w); height: auto; grid-auto-flow: row; grid-auto-rows: 4.5rem; align-content: start; padding: var(--sp-4) 0; border-top: 0; border-right: 1px solid var(--line); }
  .split { display: grid; grid-template-columns: minmax(20rem, 2fr) minmax(0, 3fr); gap: var(--sp-6); }
  .split > * { max-height: calc(100dvh - var(--header-h)); overflow: auto; overscroll-behavior: contain; }
}
```

## 7. 점검

- `check.mjs`는 기본으로 390(휴대폰), 820(태블릿 세로), 1180(태블릿 가로), 1280(데스크톱) 폭을 본다. 넘치면 넘친 요소 이름까지 알려 준다.
- 좁은 휴대폰이 걱정되면 `--widths 320,360,390`으로 한 번 더 본다.
- 가로 태블릿은 높이 820에서 첫 화면 핵심이 보이는지 눈으로 확인한다.
- 메뉴를 열고 닫는 화면이면 서랍을 연 상태도 한 번 확인한다(점검기는 첫 화면만 본다).
- 실제 기기로 볼 수 있다면 3~5대: 화면 비율이 다른 휴대폰 2~3대, 노치나 홈 막대가 큰 기기 1대, 작은 화면 1대.
- 휴대폰 글자 크기를 크게(200%) 바꾼 상태에서 글자가 잘리거나 겹치지 않는지 본다. 개발자 도구에서 html 글자 크기를 32px로 바꿔 흉내 낼 수 있다.
