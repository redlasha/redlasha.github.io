---
name: blog-design
description: 블로그 디자인·조판을 점검하고 개선한다. 실제 렌더링을 브라우저로 측정해서 판단한다. "디자인 봐줘", "조판", "레이아웃", "이미지", "다이어그램", "/blog-design" 요청에 사용.
---

# 디자인

`_ops/DESIGN.md`를 먼저 읽는다. 현재 값들은 **측정해서 정한 것**이므로 근거를 알고 바꾼다.

## 원칙: 눈대중으로 판단하지 않는다

HTML만 보거나 스크린샷만 보고 결론 내지 않는다. **브라우저로 실제 페이지를 열어 측정한다.**

### 측정 절차

브라우저 도구로 페이지를 열고 다음을 뽑는다.

```javascript
const c = document.querySelector('.page__content');
const body = c.querySelector('p');
const bs = parseFloat(getComputedStyle(body).fontSize);
({
  contentW: Math.round(c.getBoundingClientRect().width),
  charsPerLine: Math.round(c.getBoundingClientRect().width / bs),
  h1count: document.querySelectorAll('h1').length,
  h2ratio: (parseFloat(getComputedStyle(c.querySelector('h2')).fontSize) / bs).toFixed(2),
  h3ratio: (parseFloat(getComputedStyle(c.querySelector('h3')).fontSize) / bs).toFixed(2),
  wordBreak: getComputedStyle(c).wordBreak,
  toc: !!document.querySelector('.toc')
})
```

### 기준값

| 항목 | 목표 | 왜 |
|---|---|---|
| 한 줄당 글자 | 35~45자 | 한국어 가독 범위 |
| `<h1>` 개수 | **1** | 2면 본문 H1이 남아 있는 것 |
| H2 비율 | 1.5~1.7배 | 1.25배면 섹션 구분이 안 보인다 |
| H3 비율 | 1.25~1.35배 | |
| `word-break` | `keep-all` | 한국어 어절 단위 줄바꿈 |

**변경 전후를 반드시 함께 기록한다.** "좋아졌다"가 아니라 "29자 → 39자"로 말한다.

## 수정 위치

`_includes/head/custom.html`의 `<style>` 블록. **SCSS를 건드리지 않는다** — 로컬 빌드 검증이 불가능한 상태라 SCSS 오류는 사이트를 통째로 멈춘다 (`_ops/DECISIONS.md` 참조).

수정 후 `_ops/DESIGN.md`의 값 표를 갱신한다.

## 다이어그램

Mermaid가 활성화돼 있다. 마크다운에 ` ```mermaid ` 코드블록을 쓰면 렌더링된다.

**언제 쓰나**: 구조·순서·관계를 설명할 때만. 표로 충분한 걸 그림으로 만들지 않는다.

```
flowchart LR   순서·흐름
graph TD       계층·구조
sequenceDiagram  주고받음
```

다크 스킨에 맞춰 `theme: 'dark'`로 초기화돼 있다. 추가한 뒤에는 **실제 렌더링을 눈으로 확인한다** — 문법 오류가 나면 그 자리가 빈다.

## 이미지 규격

`_ops/DESIGN.md`의 3단계 계획 참조. Level 1(OG 카드)은 GitHub Actions 빌드 전환이 전제다.

## 체크리스트

디자인 변경 후 확인한다.

- [ ] 측정값이 기준 범위 안인가
- [ ] 데스크톱과 **모바일 폭**에서 모두 확인했나 (현재 모바일 미검증 상태)
- [ ] 표가 좁은 폭에서 깨지지 않나
- [ ] 코드블록이 가로로 넘치지 않나
- [ ] 다크 스킨에서 대비가 충분한가
- [ ] 목록 페이지(`/`)도 함께 확인했나

## 흔한 함정

- **기존 글이 다 그렇다고 관례인 건 아니다.** 본문 H1이 그랬다 — 관례로 보였지만 같은 버그의 반복이었다
- 한 곳을 고치면 다른 곳이 깨진다. 본문 폭을 바꾸면 표와 코드블록을 다시 봐야 한다
- 목차를 끄면 우측 레일이 통째로 빈다. 폭 설정과 목차는 함께 판단한다
