# redlasha's blog — 사이트 저장소

Jekyll + Minimal Mistakes(remote_theme). GitHub Pages가 `master` 브랜치를 자동 빌드한다.

**이 저장소는 결과물만 담는다.** 기획·조사·초안·지표는 비공개 워크스페이스에 있다.

| | 저장소 | 담는 것 |
|---|---|---|
| 작업 | [`redlasha/blog-ops`](https://github.com/redlasha/blog-ops) (private) | 전략, 조사, 칸반, 초안, 실측 |
| 결과 | 여기 (public) | 발행된 글, 테마, 조판 |

> ⚠️ **작업은 `~/work-hslee/blog-ops`에서 시작한다.** `/blog-*` 스킬이 거기 있다. 여기서 세션을 열면 스킬이 잡히지 않는다. 발행과 디자인 작업도 그쪽에서 실행하고 이 저장소를 대상으로 삼는다.

## 글 작성 규칙

### front matter (필수)

```yaml
---
title: "제목"
excerpt: "목록과 검색 결과에 노출되는 요약. 제목을 반복하지 말고 왜 읽어야 하는지를 쓴다."
date: YYYY-MM-DD
categories: software
tags: [claude-code, ai-coding, 개발환경]
---
```

`excerpt`는 **반드시** 쓴다. 생략하면 첫 문단이 발췌로 잡힌다.

### ⚠️ 본문에 H1(`#`)을 쓰지 않는다

테마가 front matter의 `title`을 이미 페이지 제목으로 렌더링한다. 본문 H1을 또 쓰면 ① 제목이 두 번 나오고 ② `excerpt_separator: "\n\n"` 때문에 **목록 발췌문이 제목 반복**이 된다.

본문은 H2(`##`)부터 시작한다. 과거 글들이 전부 H1을 갖고 있었고 그 상태로 한동안 발행됐다 — 관례처럼 보였지만 같은 버그의 반복이었다.

### 구조

- 제목 다음 첫 블록은 `>` 인용구 한 줄 — 글 전체를 압축한 후크
- 섹션은 H2, 하위는 H3. 긴 글은 번호를 붙인다
- 마지막에 `### 참고` — **공식 문서를 최우선으로 인용한다**

## 조판

한국어 줄바꿈(`word-break: keep-all`), 본문 폭, 제목 위계, 표 밀도는 `_includes/head/custom.html`의 `<style>` 블록에서 보정한다. Mermaid 렌더링도 거기 있다.

**SCSS를 건드리지 말 것.** Gemfile이 없어 로컬 빌드 검증이 안 되고, SCSS 오류는 GitHub Pages 빌드를 통째로 실패시킨다. `<style>` 블록은 빌드 리스크가 없다.

현재 값과 그 근거는 워크스페이스의 `ops/DESIGN.md`에 있다.

## 사이트 정보 구조

| 경로 | 무엇 | 진입점 |
|---|---|---|
| `/` | 최근 글 목록 | 사이트 제목 |
| `/tags/` | 태그 아카이브 — 시리즈 탐색의 실질 축 | 마스트헤드 |
| `/about/` | 소개, 시리즈 안내 | 마스트헤드 |
| `/categories/` | 카테고리 아카이브 | 현재 미연결 |

마스트헤드 메뉴는 `_data/navigation.yml`. **항목을 늘리지 않는다.** `/categories/`를 뺀 건 카테고리가 `software` 하나뿐이라 클릭해도 홈과 같은 목록이 나오기 때문이다.

**새 페이지를 만들면 진입점을 함께 만든다.** 아카이브 페이지를 만들어놓고 어디서도 링크하지 않아 죽은 페이지가 된 적이 있다.

## 빌드 제약

GitHub Pages 기본 빌드는 `--safe` 모드라 **화이트리스트 플러그인만** 동작한다: `jekyll-paginate`, `jekyll-sitemap`, `jekyll-gist`, `jekyll-feed`, `jekyll-include-cache`.

OG 이미지 자동 생성 등은 GitHub Actions 빌드로 전환해야 쓸 수 있다.

## 발행

`_posts/YYYY-MM-DD-slug.md` (slug은 영문 케밥케이스). 퍼머링크는 `/:categories/:title/` → `https://redlasha.github.io/software/<slug>/`

절차는 워크스페이스의 `/blog-publish` 스킬이 수행한다. 검증 명령:

```bash
SLUG=<slug>
URL=https://redlasha.github.io/software/$SLUG/

until curl -sf -o /dev/null "$URL"; do sleep 10; done
curl -s "$URL" | grep -c '<h1'                     # 1이어야 정상
curl -s "$URL" | grep -o 'href="/software/[^"]*"'  # 상호 링크
```

## 자동화

`.github/workflows/link-check.yml` — 주 1회(월요일 09:00 KST)와 글 발행 시 링크 검사. 깨진 링크는 이슈로 남는다.
