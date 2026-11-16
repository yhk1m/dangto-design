---
type: llm
weight: 3
focus: {source: file, path: out/index.html}
---
HTML 코드를 읽고 판정한다.
PASS 조건을 모두 만족하면 PASS:
- 빈 화면과 오류 화면이 각각 "무엇이 없는지(무엇이 안 됐는지) 한 문장 + 짧은 도움말 + 다음 행동 버튼 하나"로 되어 있다.
- 오류 화면 본문에 오류 번호나 기술 용어(예: 500, NET-408, timeout, network error)가 보이지 않는다.
- 버튼이 화면마다 하나다.
- 다크 화면의 바탕이 순수 검정(#000)이 아니다.
하나라도 어기면 FAIL.
