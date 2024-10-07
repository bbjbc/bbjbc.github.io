---
title: "[패스트캠퍼스] 김민태의 프론트엔드 데브캠프 2기 두 번째 섹션: HTML & CSS"
categories: [dev-camp, react]
tags: [FE, 부트캠프, 패스트캠퍼스, 패스트캠퍼스데브캠프, 프론트엔드, 김민태]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 아흔 번째 포스팅

안녕하세요! 아흔 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥ 벌써 아흔💫

오늘의 포스팅 내용은 **[패스트캠퍼스] 김민태의 프론트엔드 데브캠프 2기 두 번째 섹션: HTML & CSS**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ HTML,CSS,JS의 다양한 관점

프론트엔드 개발을 시작하려는 분들에게 [**로드맵 사이트**](https://roadmap.sh/)는 가이드라인을 제공한다. 이 사이트에서는 프론트엔드 개발에 필요한 다양한 기술과 개념들을 보여준다.

프론트엔드의 핵심 기술인 HTML, CSS, JavaScript는 여러 관점에서 바라볼 수 있다. 이러한 다양한 시각은 기술을 어떻게 활용할지, 어떤 맥락에서 이해할지에 영향을 미친다.

> 🍩 **HTML/CSS/JS를 바라보는 4가지 관점**

HTML/CSS/JS는 4가지 관점으로 확인할 수 있다.

1. **운영 환경**: 웹 브라우저, 모바일 앱, 데스크롭 애플리케이션 등 다양한 플랫폼에서의 동작
2. **GUI - 그래픽**: 사용자와 상호작용하는 시각적 요소의 구현
3. **문서**: 구조화된 정보의 표현과 관리
4. **앱 개발**: 동적이고 인터랙티브한 웹 애플리케이션 구축

> 🍩 **그래픽의 기본 요소: 점과 색**

그래픽은 본질적으로 점과 색으로 구성된다. 이러한 기본 요소들은 아래와 같이 표현될 수 있다.

1. **2D (2차원)**: x축과 y축으로 이루어진 평면 좌표계
2. **3D (3차원)**: x, y축에 깊이를 나타내는 z축이 추가된 입체 좌표계
3. **벡터**: 크기를 조절해도 이미지 품질이 유지되는 수학적 描述 방식

> 🍩 **HTML의 강력한 기능**

HTML은 단순한 마크업 언어를 넘어서 위에서 언급한 모든 그래픽 표현 방식을 지원한다. HTML이 단순한 문서 구조화 도구를 넘어서 멀티미디어와 인터랙티브한 컨텐츠를 제공할 수 있는 강력한 플랫폼을 의미한다.

- 2D 그래픽: `<canvas>` 요소를 통한 2D 드로잉
- 3D 그래픽: WebGL을 이용한 3D 렌더링
- 벡터 그래픽: SVG(Scalable Vector Graphics) 지원

이러한 HTML의 다재다능함은 현대 웹 개발에서 무한한 가능성을 열어주며, 프론트엔드 개발자들이 다양한 시각적 경험을 만들어낼 수 있게 해준다.

---

# 2️⃣ HTML의 요소

HTML 요소는 다양한 기준에 따라 분류할 수 있다.

> 🍩 **표시 형식에 대한 기준**

- 블록 레벨 요소

  - 블록 레벨 요소는 새 줄에서 시작하며 inline 요소를 제외한 나머지를 말한다.
  - `<div>`, `<p>`, `<h1>`-`<h6>`, `<ul>`, `<ol>`, `<li>`

- 인라인 레벨 요소
  - 텍스트 흐름 내에서 사용되는 요소이며 내용물의 크기만큼 공간을 차지한다.
  - `<span>`, `<a>`, `<img>`, `<em>`, `<code>`, `<br>`, `<strong>`

> 🍩 **의미 기준(시맨틱)**

- 문서의 구조와 의미를 명확히 전달하기 위해 사용하며 검색엔진 최적화(SEO)에 도움이 된다.
- `<main>`, `<article>`, `<section>`, `<header>`, `<footer>`, `<nav>`, `<aside>`

> 🍩 **제목, 텍스트 꾸밈 태그**

- 문서의 제목 구조를 나타내거나 텍스트를 특별히 표시한다.
- `<h1>` ~ `<h6>`, `<p>`, `<blockquote>`, `<pre>`, `<code>`

> 🍩 **폼 관련 태그**

- 사용자로부터 데이터를 입력받거나 서버로 전송하는 데에 사용한다.
- `<form>`, `<input>`, `<select>`, `<option>`, `<textarea>`, `<button>`, `<submit>`

> 🍩 **미디어 관련 태그**

- 멀티미디어 콘텐츠를 웹 페이지에 삽입하는 데에 사용한다.
- `<img>`, `<video>`, `<picture>`, `<source>`

---

# 3️⃣ 메타 태그

HTML 문서의 `<head>` 섹션에는 다양한 메타 태그를 사용하게 된다. 그 중 문자 인코딩을 지정하는 메타 태그는 특히 중요하다.

## 🍰 여러 가지 메타 태그

> 🍩 **UTF-8 인코딩**

가장 널리 사용되는 문자 인코딩은 UTF-8이다. UTF-8은 유니코드 문자를 지원하고, 전 세계의 거의 모든 문자를 표현할 수 있다.

```html
<meta charset="UTF-8" />
```

이 태그는 `<head>` 태그의 가장 앞부분에 위치해야 한다.

> 🍩 **뷰포트 설정**

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
```

이 태그는 반응형 웹 사이트를 제작할 때 필수적인 태그이다.

> 🍩 **페이지 설명**

```html
<meta name="description" content="이 페이지에 대한 간단한 설명" />
```

검색 엔진 결과에 표시될 수 있는 페이지 설명을 쓰면 된다.

> 🍩 **작성자 정보**

```html
<meta name="author" content="작성자 이름" />
```

작성자 정보도 메타 태그를 사용해서 작성할 수 있다.

> 🍩 **Cache-Control**

```html
<meta
  http-equiv="Cache-Control"
  content="no-cache, no-store, must-revalidate"
/>
```

이 태그는 브라우저의 캐싱 동작을 제어한다. 위는 캐싱을 완전 비활성화하는 설정이다.

Cache-Control은 웹페이지의 캐싱 동작을 세밀하게 제어할 수 있게 해주고 성능과 보안 측면에서 중요 역할을 할 수 있다.

- no-cache: 캐시를 사용하기 전에 서버에 재검증을 요청함
- no-store: 어떤 종류의 캐시도 저장하지 않음
- must-revalidate: 캐시를 사용하기 전에 반드시 원본 서버에 재검증해야 함
- max-age=<seconds>: 리소스가 신선하다고 간주되는 최대 시간을 초 단위로 지정함

위는 주요 옵션이다.

---

# 4️⃣ CSS

CSS는 **C**ascading **S**tyle **S**heets의 약어이다. 공룡을 그렸다면 공룡에 색을 입히는 과정이라고 보면 된다.

자 그럼 CSS에 대해 짧막하게 알아보도록 하자.

## 🍬 CSS 선택자 (Selectors)

CSS 선택자는 스타일을 적용할 HTML 요소를 지정하는 패턴이다. 다양한 유형의 선택자가 있으며, 각각 특정 상황에 맞게 사용된다.

> 🍩 **Type Selector: HTML 태그 이름을 직접 사용**

```css
p {
  color: blue;
}
```

> 🍩 **Class Selector: 마침표로 시작하며, 같은 클래스를 가진 모든 요소에 적용**

```css
.highlight {
  background-color: yellow;
}
```

> 🍩 **ID Selector: 해시(#)로 시작하며, 고유 ID를 가진 단일 요소에 적용**

```css
#header {
  font-size: 24px;
}
```

> 🍩 **속성 Selector: 대괄호([])를 사용하여 특정 속성이나 속성값을 가진 요소 선택**

```css
input[type="text"] {
  border: 1px solid gray;
}
```

> 🍩 **Grouping Selector: 쉼표로 여러 선택자를 그룹화**

```css
h1, h2, h3 {
  font-family: font-family: Arial, sans-serif;
}
```

> 🍩 **Pseudo Selector: 요소의 특정 상태나 위치를 선택**

```css
a:hover {
  text-decoration: underline;
}

li:first-child {
  font-weight: bold;
}
```

> 🍩 **Combination Selector: 요소 간의 관계를 기반으로 선택**

```css
div > p {
  margin-left: 20px;
}

h1 + p {
  font-style: italic;
}
```

## 🍬 CSS 속성 (Properties)

CSS 속성은 선택된 요소에 적용할 스타일을 정의한다.

> 🍩 **Typography: 텍스트 관련 스타일**

```css
body {
  font-family: Arial, sans-serif;
  font-size: 16px;
  line-height: 1.5;
}
```

> 🍩 **Background: 배경 관련 속성**

```css
.hero {
  background-color: #f0f0f0;
  background-image: url("background.jpg");
  background-size: cover;
}
```

> 🍩 **Border & Shadow: 테두리와 그림자 효과**

```css
.card {
  border: 1px solid #ddd;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
```

> 🍩 **Animation & Transition: 동적 효과**

```css
.button {
  transition: all 0.3s ease;
}
.button:hover {
  transform: scale(1.1);
}
```

> 🍩 **Color & Effect: 색상과 시각적 효과**

```css
.alert {
  color: #721c24;
  background-color: #f8d7da;
  opacity: 0.9;
}
```

> 🍩 **Layout & Position: 요소의 배치와 위치 지정**

아래는 `position`에 대한 예시이다.

```css
.tooltip {
  position: absolute;
  top: 100%;
  left: 0;
}
```

아래는 `flexbox`에 대한 예시이다.

```css
.container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
```

아래는 `grid`에 대한 예시이다.

```css
.gallery {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
}
```

> 🍩 **Box Model: 요소의 크기와 여백 관리**

```css
.box {
  width: 300px;
  height: 200px;
  padding: 20px;
  margin: 10px;
  border: 1px solid #000;
}
```

## 🍬 CSS 값 (Values)

CSS 값은 속성에 할당되는 구체적인 설정이다.

> 🍩 **절대적 단위**

```css
.absolute-units {
  width: 100px;
  height: 50pt;
  margin: 1cm;
  padding: 10mm;
  border: 0.1in solid black;
}
```

> 🍩 **상대적 단위**

```css
.relative-units {
  font-size: 1.2em;
  width: 50%;
  height: 100vh;
  padding: 1rem;
}

.grid-container {
  display: grid;
  grid-template-columns: 1fr 2fr 1fr;
}

.responsive-font {
  font-size: calc(16px + 1vw);
}
```

## 🍬 인라인 스타일

인라인 스타일은 HTML 요소의 `style` 속성을 통해 직접 스타일을 적용하는 방식이다.

```html
<p style="color: red; font-size: 18px;">이것은 인라인 스타일의 문단입니다.</p>
```

이렇게 간단하게 CSS에 대해서 알아보았다.

---

# 5️⃣ 마무리

두 번째 섹션을 마무리하면서 HTML과 CSS에 대해 포괄적으로 학습할 수 있었다.

이미 알고 있던 내용이었지만, 항상 TailwindCSS를 사용하다 보니 순수 CSS파일로 작업하는 것이 꽤 어색하게 느껴졌다.

앞으로 진행할 두 번째 프로젝트에서 HTML, CSS, Vanilla JavaScript만으로 구현해야 한다는 점에서 어려움이 예상된다.

하지만, 이렇게 하면서 웹 개발의 근본적인 원리를 이해할 수 있을 것이라고 생각한다. 순수한 형태의 웹 기술로 작업하는 것은 더 탄탄한 기초를 쌓는 과정이 될 것이라고 생각한다❗️💥
