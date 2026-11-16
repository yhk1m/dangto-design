---
description: 빈 화면과 오류 화면을 짧고 행동 중심으로 만드는지
tags: [states]
runs: 3
max_turns: 25
timeout_seconds: 900
allowed_tools: [Read, Glob, Grep, Skill, Write]
---
동아리 신청 앱에서 두 가지 휴대폰 화면을 나란히 만들어 줘. 왼쪽은 학생이 아직 신청한 동아리가 없을 때의 빈 화면(신청은 3월 15일까지), 오른쪽은 동아리 목록을 불러오다 인터넷 연결이 끊겼을 때의 오류 화면(다크 모드)이야. HTML 파일 한 장으로 만들어서 out/index.html에 저장해 줘.
