# UI5 Walkthrough 학습 노트

이 파일은 튜토리얼을 진행하며 학습한 핵심 내용과 발생했던 이슈/질문들을 기록하는 문서입니다.

---

## Step 1: Hello World
* **핵심 내용**: 
  * UI5 프로젝트의 가장 기본적인 뼈대(`webapp` 폴더 및 `index.html`)를 구성했습니다.
  * 아직 UI5 엔진을 쓰지 않고 순수한 HTML 텍스트 태그(`<div>`)로만 화면을 출력했습니다.
* **Q&A 및 이슈**:
  * **Q: BAS(Business Application Studio) 환경에서 어떻게 테스트할까?**
    * A: 로컬 환경과 BAS를 연결하기 위해 GitHub에 프라이빗 저장소(`cap-ui5-study`)를 생성하고, `git push`를 통해 코드를 연동하는 방식으로 해결했습니다. (Node.js를 무시하는 폴더 처리를 위해 `.gitignore` 파일도 세팅)

---

## Step 2: Bootstrap (UI5 엔진 장착)
* **핵심 내용**: 
  * 평범한 HTML 문서에 SAPUI5 프레임워크를 연결하고 초기화(Bootstrap)했습니다.
  * `index.html`의 `<head>` 태그 내에 부트스트랩 스크립트 추가:
    * `sap-ui-core.js` 엔진 로드
    * `data-sap-ui-async="true"`: 비동기 로딩을 통한 성능 최적화
    * `data-sap-ui-on-init`: 엔진 로딩 완료 직후 `index.js` 파일을 실행하도록 지시
  * `index.js`: 엔진 구동 완료를 확인하기 위해 `alert("UI5 is ready");` 경고창 띄우기 수행.
* **Q&A 및 이슈**:
  * **Q: 테스트 실행 명령어는 무엇인가?**
    * A: 표준 웹 개발 환경과 동일하게 `npm install` (최초 1회) 후 `npm start` 를 입력하여 로컬 개발 서버를 띄웁니다.
  * **Q: 예전 SAP(ABAP, GUI 등) 개발 방식과 다른 것 같은데?**
    * A: 최신 SAPUI5 및 BAS 환경은 React, Vue 등과 같은 현대적인 웹 개발 표준(Node.js 생태계)을 따르도록 발전했습니다.

---

## Step 3: Controls (UI5 컨트롤 사용)
* **핵심 내용**: 
  * 순수 HTML 태그 대신 본격적으로 SAPUI5가 제공하는 컴포넌트(Control)로 화면을 그리기 시작했습니다.
  * `index.html`: `class="sapUiBody"`를 추가하여 SAP 테마를 화면 전체에 적용하고, 그림을 그릴 도화지 영역인 `id="content"` 지정.
  * `index.js`: `sap.ui.define`을 사용하여 UI5 텍스트 부품(`sap/m/Text`)을 가져옴. 이를 조립(`new Text(...)`)한 뒤 도화지 영역에 부착(`.placeAt("content")`).
* **Q&A 및 이슈**:
  * **Q: 테스트 화면을 띄웠는데 앱이 안나오고 파일/폴더 구조가 뜬다.**
    * A: 프로젝트 최상단 폴더 위치에서 단순 파일 서버가 열렸을 때 발생하는 현상입니다. 화면 목록에서 `webapp` 폴더를 클릭한 뒤 `index.html`을 클릭해 들어가거나, 터미널에서 `npm start` 명령어로 정확히 실행하여 해결합니다.

---

## Step 4: XML Views (화면을 파일로 분리)
* **핵심 내용**: 
  * 자바스크립트(`index.js`) 안에 UI 요소를 직접 작성하면 화면이 복잡해질수록 관리가 어려워집니다. 따라서 화면(View) 구성만 담당하는 전용 파일(`XML`)을 만들어 완벽히 분리했습니다.
  * `webapp/view/App.view.xml` 생성: 화면을 구성할 UI5 컨트롤을 HTML과 유사한 XML 태그 형식(`<Text text="Hello World"/>`)으로 선언.
  * `index.js` 수정: 텍스트를 직접 생성하는 대신, 방금 만든 XML 뷰 파일(`ui5.walkthrough.view.App`) 전체를 불러온 뒤 도화지(`id="content"`) 영역에 부착하도록 역할을 변경.

---

## Step 5: Controllers (이벤트 로직 분리)
* **핵심 내용**: 
  * 화면(View)에서 발생하는 클릭 등의 동작(이벤트)을 처리하는 로직만 따로 모아두는 **Controller(컨트롤러)** 파일을 분리했습니다. MVC 패턴의 **C**에 해당합니다.
  * `webapp/controller/App.controller.js` 생성: `onShowHello`라는 함수를 정의하고, 실행 시 "Hello World" 경고창을 띄우는 로직 작성.
  * `webapp/view/App.view.xml` 수정: `<mvc:View>` 태그에 `controllerName="ui5.walkthrough.controller.App"`을 선언하여 방금 만든 컨트롤러와 화면을 연결함. 단순 텍스트 대신 `<Button text="Say Hello" press=".onShowHello"/>` 버튼을 배치하여 버튼 클릭 시 컨트롤러의 `onShowHello` 함수가 실행되도록 연결함.

---

## Step 6: Modules (UI5 모듈 활용)
* **핵심 내용**: 
  * 자바스크립트 내장 경고창(`alert`) 대신, SAPUI5에서 제공하는 세련된 메시지 팝업 모듈(`sap/m/MessageToast`)을 사용하도록 코드를 개선했습니다.
  * `webapp/controller/App.controller.js` 수정: 상단의 `sap.ui.define` 배열 안에 `"sap/m/MessageToast"` 모듈을 추가로 선언하고, 콜백 함수 파라미터로 `MessageToast`를 받아옵니다.
  * 기존 `alert("Hello World");` 코드를 지우고 `MessageToast.show("Hello World");`로 변경.
  * 결과적으로 버튼을 누르면 브라우저의 투박한 경고창 대신, 화면 하단에 부드럽게 나타났다가 사라지는 토스트(Toast) 팝업이 뜹니다.
