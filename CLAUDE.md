# redlasha's blog — 운영 규약

Jekyll + Minimal Mistakes(remote_theme) 기반 개인 기술 블로그. GitHub Pages가 `master` 브랜치를 자동 빌드한다.

## 블로그의 축

**AI 코딩 에이전트를 프로덕션처럼 운영하면서 관측·최적화한 기록.**

"이렇게 쓰면 좋아요" 수준의 사용법 소개가 아니라, 실제로 굴리다 겪은 문제 → 원인 규명 → 구조적 이해로 이어지는 글을 쓴다. 도구를 만들다 문제를 겪고, 그 원인을 파다 보면 글이 된다 — 이게 이 블로그의 주제 발굴 방식이다.

## 글 작성 규칙

### front matter (필수)

```yaml
---
title: "제목"
excerpt: "목록 페이지와 검색 결과에 노출되는 요약. 제목을 반복하지 말고 왜 읽어야 하는지를 쓴다."
date: YYYY-MM-DD
categories: software
tags: [claude-code, ai-coding, 개발환경]
---
```

- `excerpt`는 **반드시** 쓴다. 생략하면 첫 문단이 발췌로 잡힌다.
- `tags`는 시리즈를 잇는 장치다. Claude Code 운영 시리즈는 `claude-code` 태그로 묶인다.

### ⚠️ 본문에 H1(`#`)을 쓰지 않는다

테마가 front matter의 `title`을 이미 페이지 제목으로 렌더링한다. 본문에 H1을 또 쓰면:

1. 화면에 제목이 두 번 나온다
2. `excerpt_separator: "\n\n"` 때문에 **목록 발췌문이 제목 반복이 된다**

본문은 H2(`##`)부터 시작한다. 과거 글들이 H1을 갖고 있었고, 그 상태로 한동안 발행됐다.

### 구조

- 제목 다음 첫 블록은 `>` 인용구 한 줄 — 글 전체를 한 문장으로 압축한 후크
- 섹션은 H2, 하위는 H3. 긴 글은 번호를 붙인다(`## 1. …`)
- 마지막에 `### 참고` — 인용한 문서 링크. **공식 문서를 최우선으로 인용한다**

### 문장

- 한 문장을 길게 쓰지 않는다
- 단정할 때는 근거가 있어야 한다. 공식 문서에 "can be"라고 적힌 걸 "이다"로 쓰지 않는다
- 출처가 불분명한 주장은 "커뮤니티에서는" 대신 1인칭 경험으로 쓴다
- 표는 열거 가능한 짧은 사실에만 쓰고, 설명은 표 바깥 산문으로

## 조판

한국어 줄바꿈(`word-break: keep-all`), 본문 폭, 제목 위계, 표 밀도는 `_includes/head/custom.html`의 `<style>` 블록에서 보정한다.

**SCSS를 건드리지 말 것.** Gemfile이 없어 로컬 빌드 검증이 안 되는 상태이고, SCSS 오류는 GitHub Pages 빌드를 통째로 실패시킨다. `head/custom.html`의 스타일 블록은 빌드 리스크가 없다.

## 빌드 제약

GitHub Pages 기본 빌드는 `--safe` 모드라 **화이트리스트 플러그인만** 동작한다. 현재 사용 가능:

`jekyll-paginate`, `jekyll-sitemap`, `jekyll-gist`, `jekyll-feed`, `jekyll-include-cache`

OG 이미지 자동 생성(`jekyll-og-image`) 등은 GitHub Actions 빌드로 전환해야 쓸 수 있다. 전환은 사이트가 잠시 안 뜰 수 있으므로 별도 작업으로 진행한다.

## 발행 절차

1. `_posts/YYYY-MM-DD-slug.md` 작성 (slug은 영문 케밥케이스)
2. front matter 검증 — 특히 **본문 H1 없음**, `excerpt` 있음
3. 커밋 → `master` 푸시 (GitHub Pages가 자동 빌드, 보통 40초~2분)
4. 배포 검증 — URL 200, 상호 링크 동작, 제목 중복 없음

퍼머링크는 `/:categories/:title/` → `https://redlasha.github.io/software/<slug>/`

## 검증 명령

```bash
# 배포 대기
until curl -sf -o /dev/null https://redlasha.github.io/software/<slug>/; do sleep 10; done

# H1 중복 검사 (1이어야 정상)
curl -s https://redlasha.github.io/software/<slug>/ | grep -c '<h1'

# 내부 링크 렌더링 확인
curl -s https://redlasha.github.io/software/<slug>/ | grep -o 'href="/software/[^"]*"'
```

## 운영 하네스

이 블로그는 제품처럼 운영한다. 구조 전체는 **[`_ops/HARNESS.md`](_ops/HARNESS.md)**에 정의돼 있다. 관찰 → 이해 → 행동 루프이고, 관찰 쪽에 무게를 둔다.

### 상태 파일 — 판단의 근거는 파일로 남는다

| 파일 | 담는 것 |
|---|---|
| [`_ops/STRATEGY.md`](_ops/STRATEGY.md) | 목적, 포지셔닝, 독자, 성공 기준 |
| [`_ops/EDITORIAL.md`](_ops/EDITORIAL.md) | 시리즈 계획, 발행 리듬, 다음에 쓸 것 |
| [`_ops/ANALYTICS.md`](_ops/ANALYTICS.md) | 측정 기준, 관찰 로그 |
| [`_ops/DESIGN.md`](_ops/DESIGN.md) | 디자인 시스템, 조판 값과 근거 |
| [`_ops/DECISIONS.md`](_ops/DECISIONS.md) | 결정과 그 이유 |
| [`_ops/EXPERIMENTS.md`](_ops/EXPERIMENTS.md) | 실측 프로토콜과 결과 |
| `_ops/research/` | 조사 산출물 (날짜별, append-only) |
| `IDEAS.md` | 글감 인박스 |

`_ops/`와 `IDEAS.md`, `CLAUDE.md`는 사이트에 게시되지 않는다.

### 스킬 7종

| 단계 | 스킬 | 용도 |
|---|---|---|
| 관찰 | `/blog-research` | 시장·경쟁·주제 조사 → `_ops/research/`에 기록 |
| 관찰 | `/blog-analytics` | 지표 확인 → 관찰·해석·액션 기록 |
| 이해 | `/blog-plan` | 목적 점검, 다음 글 결정 |
| 행동 | `/blog-draft` | 공식 문서 리서치 → 개요 확인 → 초안 |
| 행동 | `/blog-review` | 서브에이전트 팩트체크 + 구성 리뷰 |
| 행동 | `/blog-publish` | 조판 검증 → 커밋 → 배포 확인 |
| 행동 | `/blog-design` | 디자인 시스템 관리, 렌더링 측정 |

**작업을 시작하기 전에 해당 단계의 상태 파일을 먼저 읽는다.** 없는 상태에서 새로 만들지 말고 기존 판단을 이어받는다.

> ⚠️ **스킬은 이 저장소 디렉터리에서 세션을 열어야 잡힌다.** `.claude/skills/`는 프로젝트 스코프라 다른 디렉터리에서 시작한 세션에서는 `/blog-*`가 목록에 뜨지 않는다. 그 경우 스킬 파일(`.claude/skills/<name>/SKILL.md`)을 직접 읽어 따르면 동작은 같다.

## 사이트 정보 구조

| 경로 | 무엇 | 진입점 |
|---|---|---|
| `/` | 최근 글 목록 | 사이트 제목 |
| `/tags/` | 태그 아카이브 — 시리즈 탐색의 실질 축 | 마스트헤드 |
| `/about/` | 블로그 소개, 시리즈 안내, 만든 것 | 마스트헤드 |
| `/categories/` | 카테고리 아카이브 | 현재 미연결 |

마스트헤드 메뉴는 [`_data/navigation.yml`](_data/navigation.yml)에서 관리한다. **항목을 늘리지 않는다** — 글이 몇 편 없는 블로그에 메뉴가 많으면 비어 보인다. `/categories/`를 넣지 않은 건 카테고리가 `software` 하나뿐이라 클릭해도 홈과 같은 목록이 나오기 때문이다. 2개 이상 되면 추가한다.

**새 페이지를 만들면 진입점을 함께 만든다.** 아카이브 페이지를 만들어놓고 어디서도 링크하지 않아 죽은 페이지가 된 적이 있다.
