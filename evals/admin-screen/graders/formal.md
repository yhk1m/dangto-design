---
type: regex
pattern: '(습니다|십시오|하시겠|하십시|되었습)'
match: not_contains
target: {source: file, path: out/index.html}
---
