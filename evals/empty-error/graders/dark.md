---
type: regex
pattern: 'prefers-color-scheme:\s*dark|data-theme=["'']dark'
match: contains
target: {source: file, path: out/index.html}
---
