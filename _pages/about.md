---
title: "소개"
excerpt: "AI 코딩 에이전트를 실제로 굴리면서 겪은 문제와 그 구조적 이유를 기록합니다."
permalink: /about/
layout: single
author_profile: true
toc: true
toc_label: "목차"
---

## 이 블로그

**AI 코딩 에이전트를 실제로 굴리면서 겪은 문제와, 그게 왜 그렇게 되는지를 기록합니다.**

설치법이나 "이렇게 쓰면 좋아요" 같은 글은 이미 충분히 많습니다. 여기서 다루는 건 그다음입니다 — 매일 쓰다 보면 마주치는 한도, 비용, 계정, 캐시 같은 운영 문제와 그 밑에 깔린 구조.

글을 쓸 때 지키는 것 세 가지입니다.

- **공식 문서를 1차 출처로 인용합니다.** 경험담만으로 단정하지 않습니다
- **직접 겪은 문제에서 출발합니다.** 도구를 만들다 막힌 지점이 대부분의 글감입니다
- **발행 전에 사실 검증을 거칩니다.** 근거가 없는 주장은 근거가 없다고 씁니다

## 시리즈

### Claude Code 운영

계정, 토큰, 캐시. 각 편이 앞 편의 결론을 재료로 씁니다.

1. [클로드 계정 두 개 쓰다가 발견한, 거짓말하는 statusline](/software/claude-two-accounts/) — 관측 도구가 조용히 틀릴 때
2. [Claude Code 토큰, 어디서 새고 있나](/software/claude-code-token-saving/) — 캐시가 깨지는 지점
3. *계정 경계는 캐시 경계와 일치시켜라* — 준비 중

전체 목록은 [`claude-code` 태그](/tags/#claude-code)에서 볼 수 있습니다.

## 만든 것

| 프로젝트 | 무엇 |
|---|---|
| [usage-statusbar](https://github.com/redlasha/usage-statusbar) | Claude Code statusline. 컨텍스트 사용률, 5시간 한도, 리셋 시각, worktree 브랜치를 한 줄로 |

## 글쓴이

**redlasha** — 서울에서 일하는 개발자.

- [GitHub](https://github.com/redlasha)

---

글에 오류를 발견하셨다면 [GitHub 이슈](https://github.com/redlasha/redlasha.github.io/issues)로 알려주세요. 정정하고 무엇을 고쳤는지 남깁니다.
