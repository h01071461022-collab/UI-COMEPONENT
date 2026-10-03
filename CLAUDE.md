# CLAUDE.md

이 파일은 이 저장소에서 작업하는 Claude Code(claude.ai/code)에게 제공하는 안내 문서입니다.

## 이 프로젝트는 무엇인가

CareerFoundry의 [UI 엘리먼트 사전집](https://careerfoundry.com/en/blog/ui-design/ui-element-glossary/)에 소개된 32가지 프론트엔드 UI 엘리먼트(Accordion, Button, Modal, Toggle 등)를 번호가 매겨진 "표본 카탈로그(specimen catalogue)" 형식으로 정리한 단일 파일 데모/포트폴리오 사이트입니다. 좌측 컬럼은 32개 엘리먼트를 검색·필터링할 수 있는 색인이고, 우측 컬럼은 선택한 엘리먼트의 실시간 동작 미리보기와 HTML·CSS·JS 소스 탭을 보여 주는 스펙 시트입니다. UI 문구, 설명, 접근성(a11y) 노트 모두 한국어로 작성되어 있습니다.

전체 원본 요구사항/사양은 [prompt.md](prompt.md)에 있으니, 의도한 디자인 방향(명시적으로 "anti AI-slop" — 밝은 배경, 블랙 계열 텍스트, Archivo/IBM Plex Sans KR/IBM Plex Mono 폰트, 보라색 그라디언트·크림색 세리프·이모지 같은 전형적인 AI 생성 디자인 요소 배제)과 배포 계획을 확인할 때는 그 문서를 참고하세요.

## 명령어

빌드, 패키지 매니저, 테스트 스위트가 없습니다 — 모든 것이 인라인된 정적 `index.html` 하나뿐입니다 (CSS는 `<style>` 블록, JS는 `<script>` 블록에 있고, `highlight.js`만 외부 CDN 스크립트로 불러옵니다). 개발 시에는:

- `index.html`을 브라우저에서 직접 열거나, 이 디렉터리를 임의의 정적 파일 서버로 서빙하세요.
- 변경 사항은 페이지를 로드하고 좌측 색인에서 엘리먼트들을 직접 클릭해 보며 확인합니다 — 실행할 수 있는 자동화 테스트는 없습니다.

배포는 두 곳에 병행하며, 둘 다 "파일을 올리는" 작업입니다:
- GitHub Pages — `main` 브랜치 루트에서 서빙, `https://h01071461022-collab.github.io/UI-COMEPONENT/` — `index.html`을 커밋·푸시하면 됩니다.
- 같은 파일로 퍼블리시한 Claude 아티팩트 — 같은 아티팩트 URL로 다시 퍼블리시해서 동기화합니다.

## 아키텍처

모든 내용이 `index.html` 하나에 세 부분으로 들어 있습니다: `<style>` 블록, 마크업 셸(`.index` 좌측 컬럼 + `.main` 스펙 시트 컬럼), 그리고 모든 데이터와 동작을 담은 `<script>` 블록.

**엘리먼트 데이터.** 32개 엘리먼트는 각각 `E({...})` 호출(`index.html:219`)로 등록되며, 이 함수는 모듈 수준의 `ITEMS` 배열에 push합니다. 각 항목은 `id`, `en`/`ko` 이름, `cat`(`input`/`nav`/`info`/`box` 중 하나 — `index.html:3820` 근처의 `CATS`/`CAT_ORDER` 참고), 한국어 `desc`, 접근성 노트 문자열 `a11y`, 그리고 원본 소스 문자열 세 개(`html`, `css`, `js`)를 가집니다. 이 세 문자열이 유일한 소스(single source of truth)로, 실시간 미리보기와 소스 탭 표시를 모두 이 값으로부터 만들어 냅니다. 따라서 엘리먼트를 수정한다는 것은 해당 템플릿 리터럴을 직접 고치는 것을 의미합니다(`id:'<element-id>'`로 검색).

**실시간 미리보기.** `buildPreviewDoc(item)`(`index.html:3836`)이 `item.html`/`css`/`js`와 공용 `RESET_CSS`를 조합해 완전한 HTML 문서 문자열을 만들고, 이를 `iframe.srcdoc`에 주입합니다(`allow-scripts`로 샌드박싱). 이 방식 덕분에 각 엘리먼트의 미리보기는 완전히 격리되어 있어 — 부모 페이지에 영향을 줄 수 없고, 부모 페이지도 엘리먼트별 DOM/CSS를 알 필요가 없습니다. "모바일 390" 토글(`setWidth`, `index.html:3953`)은 CSS 클래스로 iframe 크기만 바꿀 뿐 srcdoc은 그대로입니다.

**선택 흐름.** 하나의 `state` 객체(`index.html:3834`)가 현재 선택된 엘리먼트 id, 선택된 소스 탭(`lang`), 카테고리 필터, 검색어, 미리보기 너비를 추적합니다. `select(id)`(`index.html:3929`)가 중앙 재렌더링 진입점으로, `ITEMS`에서 해당 항목을 찾아 스펙 시트의 모든 텍스트 필드를 갱신하고, iframe `srcdoc`을 다시 만들고, 소스 탭을 다시 렌더링하고, 이전/다음 링크를 갱신합니다 — 또한 URL 해시(`location.hash`)에도 반응하므로 각 엘리먼트는 딥링크가 가능합니다.

**소스 탭.** `renderTabs(item)`(`index.html:3966`)이 현재 `state.lang`에 해당하는 원본 `html`/`css`/`js` 문자열을 `highlight.js`에 넘겨 하이라이팅하고, 복사 버튼(`index.html:4000`)은 현재 표시 중인 탭의 원본 텍스트를 `navigator.clipboard`로 복사합니다.

**좌측 색인.** `renderCatFilters()`와 `renderList()`(`index.html:3848`, `3866`)가 `ITEMS`를 필터링/검색해서 `<li>` 목록을 다시 만들고, `markCurrent()`가 현재 활성 항목에 `aria-current="page"`를 설정합니다. 작은 화면에서는 색인이 슬라이드 아웃 드로어로 바뀝니다(`#index.open`, `index.html:4028` 근처에서 토글).

33번째 엘리먼트를 추가하거나 기존 엘리먼트를 수정할 때 필요한 모든 작업은 해당 `E({...})` 블록 안에서만 이루어집니다 — 새 카테고리를 추가하는 경우가 아니라면 다른 파일이나 섹션은 건드릴 필요가 없습니다(새 카테고리라면 `CATS`/`CAT_ORDER`/`CAT_DESC`).
