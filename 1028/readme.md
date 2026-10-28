# 📘 Chapter 08. HTML DOM과 Document

이 문서는 **명품 웹 프로그래밍 Chapter 08 - HTML DOM과 Document**의 핵심 내용을 정리한 **GitHub README**용 마크다운 서식 문서입니다.

---

## 📌 목차 (Table of Contents)

1. [HTML DOM의 개념 및 구조](#1-html-dom의-개념-및-구조)
2. [DOM 객체의 구성 요소](#2-dom-객체의-구성-요소)
3. [DOM 객체 접근 및 스타일 제어](#3-dom-객체-접근-및-스타일-제어)
4. [Document 객체](#4-document-객체)
5. [DOM 객체의 동적 생성 및 삭제](#5-dom-객체의-동적-생성-및-삭제)

---

## 1. HTML DOM의 개념 및 구조

### 1.1 HTML DOM (Document Object Model)이란?
* **웹 페이지 내의 모든 HTML 태그를 1:1로 대응되는 객체(DOM 객체)로 변환**하여 관리하는 모델입니다.
* **목적**: 자바스크립트를 이용해 HTML 태그의 출력 모양(CSS3 스타일), 콘텐츠(텍스트/이미지 등), 구조를 동적으로 제어하기 위함입니다.

### 1.2 DOM 트리 (DOM Tree)
* HTML 태그 간의 부모-자식 포함 관계를 바탕으로 구성된 **트리 구조**입니다.
* **특징**:
  * 최상위 노드(Root)는 `document` 객체입니다.
  * HTML 태그 하나당 하나의 DOM 객체(DOM Node / DOM Element)가 생성됩니다.
  * 브라우저는 HTML 문서를 위에서 아래로 읽으면서 DOM 객체를 생성하고 완성된 DOM 트리를 화면에 렌더링합니다.
  * **동적 반영**: 자바스크립트로 DOM 객체를 수정하면 브라우저 화면의 HTML 출력 모양이 즉시 갱신됩니다.

---

## 2. DOM 객체의 구성 요소

DOM 객체는 HTML 태그의 정보를 포함하는 **5가지 주요 요소**로 구성됩니다.

| 구성 요소 | 설명 및 특징 | 예시 |
| :--- | :--- | :--- |
| **프로퍼티 (Property)** | HTML 태그의 속성(Attribute) 및 상태 정보를 내포 | `id`, `tagName`, `innerHTML`, `value` 등 |
| **메소드 (Method)** | DOM 객체를 제어하기 위한 멤버 함수 | `click()`, `focus()`, `setAttribute()` 등 |
| **컬렉션 (Collection)** | 자식 DOM 객체들의 주소를 가지는 배열 형태의 집합 | `children` |
| **이벤트 리스너 (Event Listener)** | HTML 태그에 등록된 이벤트 처리기 | `onclick`, `onkeydown`, `onload` 등 |
| **CSS3 스타일 (Style)** | HTML 태그에 설정된 CSS3 스타일 시트 정보 | `style.color`, `style.backgroundColor` 등 |

---

## 3. DOM 객체 접근 및 스타일 제어

### 3.1 DOM 객체 접근 (검색)
HTML 태그의 `id` 속성을 활용해 단일 DOM 객체를 찾을 수 있습니다.

```javascript
// id가 "firstP"인 DOM 객체 가져오기
let p = document.getElementById("firstP");
```

### 3.2 innerHTML 프로퍼티
* 태그 내부의 **HTML 콘텐츠(텍스트 및 자식 태그)**를 읽거나 변경할 수 있습니다.

```javascript
let p = document.getElementById("firstP");

// 콘텐츠 변경 (HTML 태그 해석 포함)
p.innerHTML = "나의 <img src='puppy.png'> 강아지";
```

### 3.3 `this` 키워드 활용
* 이벤트 리스너 함수 내에서 **이벤트가 발생한 객체 자신**을 가리킬 때 사용합니다.

```html
<!-- 버튼 자신을 클릭 시 배경색 변경 -->
<button onclick="this.style.backgroundColor='orange'">버튼</button>

<!-- 함수 파라미터로 this 전달 -->
<div onclick="change(this, '25px', 'orange')">클릭시 변경</div>
```

---

## 4. Document 객체

### 4.1 Document 객체의 개요
* HTML 문서 전체를 대표하는 객체이자 **DOM 트리의 최상위 노드**입니다.
* 브라우저가 HTML 문서를 로드하기 전 가장 먼저 생성합니다.

### 4.2 주요 프로퍼티
* `document.location` / `document.URL`: 현재 문서의 URL 정보
* `document.title`: 문서의 제목 (`<title>` 태그 내용)
* `document.head` / `document.body`: `<head>`, `<body>` 태그의 DOM 객체
* `document.readyState`: 문서의 로딩 상태 (`loading`, `complete`)
* `document.activeElement`: 현재 포커스를 가지는 DOM 객체

### 4.3 DOM 객체 탐색 메소드 및 관계 프로퍼티
1. **태그 이름으로 찾기**:
   ```javascript
   let divList = document.getElementsByTagName("div"); // HTMLCollection 리턴
   ```
2. **클래스 이름으로 찾기**:
   ```javascript
   let plainList = document.getElementsByClassName("plain"); // HTMLCollection 리턴
   ```
3. **노드 간의 관계 프로퍼티**:
   * `parentElement`: 부모 객체
   * `children`: 자식 객체 컬렉션
   * `firstElementChild` / `lastElementChild`: 첫 번째 / 마지막 자식 객체
   * `previousElementSibling` / `nextElementSibling`: 이전 / 다음 형제 객체

### 4.4 `document.write()` 및 `open()` / `close()`
* `document.write()` / `writeln()`: 현재 document에 HTML 텍스트 동적 출력.
  * ⚠️ **주의**: 문서 로드가 완료된 시점 이후(예: 이벤트 함수 내부)에 `document.write()`를 호출하면 **기존 문서 내용을 전부 지우고 새로 작성**하게 됩니다.
* `document.open()` / `document.close()`: 새로운 문서 작성 스트림을 열거나 닫을 때 사용.

---

## 5. DOM 객체의 동적 생성 및 삭제

자바스크립트를 이용해 실행 중에 새로운 HTML 태그를 생성하거나 기존 태그를 동적으로 삭제할 수 있습니다.

### 5.1 DOM 객체 생성 및 트리에 추가
1. **객체 생성**: `document.createElement("태그이름")`
2. **속성 및 내용 설정**: `innerHTML`, `setAttribute()`, `style` 등 조작
3. **DOM 트리에 추가**:
   * `parent.appendChild(child)`: 부모 노드의 마지막 자식으로 추가
   * `parent.insertBefore(newNode, refNode)`: 기준 자식 노드 앞에 추가

```javascript
// 1. DIV 객체 동적 생성
let newDIV = document.createElement("div");

// 2. 속성 및 스타일 설정
newDIV.innerHTML = "새로 생성된 DIV입니다.";
newDIV.setAttribute("id", "myDiv");
newDIV.style.backgroundColor = "yellow";

// 3. 부모 객체에 추가
let parent = document.getElementById("parent");
parent.appendChild(newDIV);
```

### 5.2 DOM 객체 삭제
* `parent.removeChild(child)`: 부모 객체로부터 지정된 자식 객체를 제거.

```javascript
let myDiv = document.getElementById("myDiv");
let parent = myDiv.parentElement; // 부모 객체 찾기

// 부모로부터 자기 자신 제거
parent.removeChild(myDiv);
```

---

## 💡 요약 정리 (Summary)

* **DOM**: HTML 문서를 객체화하여 자바스크립트로 동적 제어할 수 있게 하는 표준 모델.
* **DOM 탐색**: `getElementById`, `getElementsByTagName`, `getElementsByClassName` 사용.
* **DOM 동적 변경**: `innerHTML`, `style` 프로퍼티 조작 및 `createElement()`, `appendChild()`, `removeChild()`를 활용한 실시간 노드 제어.
