# 프로젝트 프롬프트

## 원본 요청

> https://careerfoundry.com/en/blog/ui-design/ui-element-glossary/
>
> 32가지 프론트엔드 UI 엘리먼트 사전집 사이트입니다. 해당 요소를 각각 UI로 표현해서 미리보기와 함께 소스를 확인하는 프론트엔드 포트폴리오 및 데모 사이트를 구축해야 합니다. 다음의 사항을 기준으로 html 데모 사이트를 만들어주세요.
>
> 1. 좌측은 32가지 요소 리스트업 메뉴
> 2. 우측은 좌측메뉴 클릭시 보여지는 메인 컨텐츠로 해당 클릭한 element 의 미리보기를 제공해야 하며, 탭으로 html, css, javascript 소스를 확인할 수 있어야 함
> 3. Anti AI slop 기법을 적용하여 디자인, 폰트, 아이콘 등의 요소에 AI 전형적인 디자인이나 컬러를 제외
>
> 사이트 배경 컬러는 밝은 배경 색으로 하며 폰트는 블랙 계열로 화면에 선명하게 보여야 합니다.

## 참고 자료

- 출처: [32 UI Elements Designers Need To Know (CareerFoundry)](https://careerfoundry.com/en/blog/ui-design/ui-element-glossary/)
- 글에서 소개된 32개 UI 엘리먼트 전체를 다룸: Accordion, Bento Menu, Breadcrumb, Button, Card, Carousel, Checkbox, Comment, Döner Menu, Dropdown, Feed, Form, Hamburger Menu, Icon, Input Field, Kebab Menu, Loader, Meatballs Menu, Modal, Notification, Pagination, Picker, Progress Bar, Radio Buttons, Search Field, Sidebar, Slider Controls, Stepper, Tag, Tab Bar, Tooltip, Toggle

## 구현 사양

### 레이아웃
- **좌측 고정 색인(Index)**: 32개 엘리먼트 목록, 검색창, 4개 분류(입력 / 내비게이션 / 정보 전달 / 컨테이너) 필터
- **우측 스펙 시트(메인 영역)**:
  - 선택한 엘리먼트의 실제 동작 미리보기(iframe, 직접 상호작용 가능)
  - 미리보기 너비 전환(전체 너비 / 모바일 390px)
  - HTML · CSS · JavaScript 탭으로 구분된 소스 코드 뷰어 (syntax highlight, 코드 복사 버튼)
  - 한국어로 작성된 엘리먼트 설명 및 접근성(a11y) 구현 포인트
  - 이전 / 다음 엘리먼트 이동

### 디자인 방향 (Anti AI-slop)
- 밝은 배경(off-white) + 블랙 계열 텍스트로 선명한 대비
- 전형적인 AI 생성 디자인(크림색 + 세리프 + 테라코타, 보라-파랑 그라디언트, Inter/Space Grotesk 남용, 이모지 장식, 전부 중앙 정렬 등)을 의도적으로 피함
- 타이포그래피: Archivo(제목, 압축형 디스플레이 서체) + IBM Plex Sans KR(본문, 한글 가독성) + IBM Plex Mono(코드/숫자, tabular figures)
- "표본 카탈로그(specimen catalogue)" 컨셉 — 번호가 매겨진 스펙 시트 형식으로 각 엘리먼트를 도감처럼 정리

### 접근성
- 각 컴포넌트마다 실제 동작하는 네이티브 시맨틱(button, fieldset/legend, dialog, role 속성 등)을 사용
- 키보드 조작(방향키, Esc, Tab 포커스 트랩 등) 지원
- 스크린리더를 위한 aria-label, aria-live, aria-expanded 등 상태 속성 적용

### 기술 스택
- 순수 HTML / CSS / JavaScript (프레임워크 없음), 단일 `index.html` 파일로 배포
- 코드 하이라이팅: highlight.js (CDN)
- 폰트: Google Fonts (Archivo, IBM Plex Sans KR, IBM Plex Mono)

## 배포

- Claude 아티팩트로 1차 배포
- GitHub 리포지토리(`h01071461022-collab/UI-COMEPONENT`)에 `index.html`로 푸시
- GitHub Pages로 공개: `https://h01071461022-collab.github.io/UI-COMEPONENT/`
