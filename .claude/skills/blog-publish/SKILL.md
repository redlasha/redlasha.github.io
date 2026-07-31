---
name: blog-publish
description: 블로그 글을 발행한다. 조판 규칙을 검증하고 _posts로 옮긴 뒤 커밋·푸시하고 배포까지 확인한다. "발행", "올려줘", "배포", "/blog-publish" 요청에 사용.
---

# 블로그 발행

`CLAUDE.md`의 발행 절차를 따른다. **검증 → 배치 → 발행 → 배포 확인** 순서다.

## 1. 발행 전 검증 (필수)

아래를 모두 통과해야 발행한다. 하나라도 실패하면 고치고 다시 검증한다.

```bash
POST=_posts/YYYY-MM-DD-slug.md

# ① 본문 H1 금지 — 결과가 있으면 실패
grep -n '^# ' "$POST"

# ② front matter 필수 필드
head -10 "$POST" | grep -E '^(title|excerpt|date|categories|tags):'

# ③ Liquid 충돌 — 코드블록 안에 {{ 나 {% 가 있으면 빌드가 깨진다
grep -n '{{\|{%' "$POST"
```

③에서 `{{ site.baseurl }}` 같은 의도된 Liquid만 남아야 한다. JSON/템플릿 코드 예제에 중괄호가 인접해 있으면 `{% raw %}`로 감싼다.

**추가 확인**

- 파일명이 `YYYY-MM-DD-영문-케밥케이스.md`인가
- `excerpt`가 제목의 반복이 아닌가
- `tags`가 기존 태그 체계와 맞는가 (`/tags/` 페이지 참고, 새 태그를 남발하지 않는다)
- 내부 링크가 `{{ site.baseurl }}/software/<slug>/` 형식인가

## 2. 상호 링크

시리즈로 이어지는 글이면 **양방향으로** 건다. 새 글에서 이전 글로만 걸면 이전 글 독자는 새 글을 못 본다.

이전 글에서 자연스럽게 이어지는 대목(대개 "남은 것", "한계" 같은 절)을 찾아 링크를 추가한다.

## 3. 커밋

```bash
cd ~/work-hslee/redlasha.github.io
git add -A && git commit -F - <<'EOF'
<한국어 한 줄 요약>

- 변경 내용
EOF
git push origin master
```

커밋 메시지는 기존 이력의 스타일을 따른다 (한국어, "~ 추가/개선").

## 4. 배포 확인 (생략 금지)

푸시만 하고 끝내지 않는다. GitHub Pages 빌드는 보통 40초~2분 걸린다.

```bash
SLUG=<slug>
URL=https://redlasha.github.io/software/$SLUG/

# 배포 대기 (백그라운드로 폴링)
for i in $(seq 1 30); do
  [ "$(curl -s -o /dev/null -w '%{http_code}' "$URL")" = "200" ] && echo "LIVE" && break
  sleep 10
done

# H1 개수 — 1이어야 정상 (2면 본문 H1이 남아 있는 것)
curl -s "$URL" | grep -c '<h1'

# 목차 렌더링
curl -s "$URL" | grep -c 'class="toc"'

# 상호 링크
curl -s "$URL" | grep -o 'href="/software/[^"]*"'
```

## 5. 발행 후

- 목록 페이지(`/`)에서 **발췌문이 제목 반복이 아닌지** 눈으로 확인
- `/tags/`에 새 태그가 잡혔는지 확인
- 사용자에게 URL과 검증 결과를 보고

## 실패 시

빌드가 깨지면 사이트는 직전 정상 버전을 계속 서빙한다. 당황하지 말고:

```bash
gh api repos/redlasha/redlasha.github.io/pages/builds/latest --jq '.status, .error.message'
```

원인이 SCSS나 Liquid면 해당 커밋을 되돌리고(`git revert`) 다시 시도한다.
