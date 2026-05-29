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

---

## Step 7: JSON Model (데이터 관리)
* **핵심 내용**: 
  * 드디어 MVC 패턴의 마지막 퍼즐인 **Model(데이터 관리)** 을 도입했습니다. 앱에서 사용할 데이터(변수)를 보관하고 화면(View)과 연결하는 역할을 합니다.
  * `webapp/controller/App.controller.js` 수정: 
    * `onInit()` 이라는 함수를 추가했습니다. 이 함수는 앱이 처음 켜질 때 딱 한 번 자동으로 실행되는 초기화 함수입니다.
    * `JSONModel` 모듈을 불러온 뒤, `new JSONModel({ recipient: { name: "World" } })` 형식으로 데이터를 담은 모델 객체를 만들었습니다.
    * `this.getView().setModel(dataModel);` 코드를 통해 생성한 모델을 화면(View)에 장착했습니다.
  * `webapp/view/App.view.xml` 수정:
    * `<Input value="{/recipient/name}" />` 텍스트 입력창 컨트롤을 추가했습니다.
    * 여기서 중괄호 `{}` 문법이 바로 **데이터 바인딩(Data Binding)** 입니다. 모델이 가진 `name` 변수의 값("World")을 화면의 입력창 값으로 실시간 연결해 줍니다.

---

## Step 8: Translatable Texts (다국어 텍스트 분리)
* **핵심 내용**: 
  * 화면에 보이는 글씨("Say Hello", "Hello World")를 코드 안에 하드코딩(직접 입력)하지 않고, 별도의 텍스트 전용 파일(Properties)로 분리했습니다. 이렇게 하면 나중에 한국어, 영어 등 다국어(i18n) 번역을 쉽게 지원할 수 있습니다.
  * `webapp/i18n/i18n.properties` 파일 생성:
    * `showHelloButtonText=Say Hello`, `helloMsg=Hello {0}` 처럼 `변수명=텍스트` 형태로 앱에서 사용할 모든 글귀를 모아둡니다. (`{0}`은 나중에 이름이 들어갈 빈칸입니다.)
  * `webapp/controller/App.controller.js` 수정: 
    * `ResourceModel` 모듈을 가져와서 방금 만든 `i18n.properties` 파일을 읽어들이는 텍스트 전용 모델(`i18nModel`)을 만들고 화면에 세팅했습니다.
    * 버튼을 누를 때 텍스트를 직접 치지 않고, `resourceBundle.getText("helloMsg", [recipient])`를 호출하여 텍스트 파일에서 글귀를 동적으로 꺼내오도록 변경했습니다.
  * `webapp/view/App.view.xml` 수정:
    * `<Button text="Say Hello" />` 대신 `<Button text="{i18n>showHelloButtonText}" />`로 변경했습니다. 여기서 `i18n>`은 "i18n이라는 이름의 모델에서 해당 텍스트를 가져와라"라는 뜻입니다.

---

## Step 9: Component Configuration (컴포넌트 구성)
* **핵심 내용**: 
  * 지금까지는 `index.js`와 `App.controller.js`가 앱의 초기화와 모델(데이터/언어) 세팅을 직접 담당했습니다. 하지만 실제 앱은 여러 화면을 가질 수 있으므로, 앱 전체를 관리하는 독립적인 대장 부품(Component)으로 캡슐화(포장)하는 것이 표준입니다.
  * `webapp/Component.js` 파일 신규 생성:
    * 앱 전체를 관리하는 핵심 컴포넌트 파일입니다.
    * `init()` 함수 안에서 앱 전체가 쓸 데이터 모델(`JSONModel`)과 언어 모델(`ResourceModel`)을 세팅합니다. (기존 컨트롤러에 있던 초기화 코드를 이쪽으로 이사 왔습니다.)
  * `webapp/index.js` 수정: 
    * 기존처럼 화면(XML 뷰)을 직접 불러오지 않고, 대신 `ComponentContainer`라는 상자를 만들어 방금 만든 컴포넌트 전체를 화면에 얹어주는 역할만 하도록 수정했습니다.
  * `webapp/controller/App.controller.js` 수정:
    * 초기화 로직이 컴포넌트로 넘어갔으므로, 데이터를 세팅하던 `onInit` 함수가 통째로 삭제되었습니다. 오직 버튼 클릭 이벤트 로직만 남게 되어 컨트롤러 본연의 역할에 집중하게 되었습니다!

---

## Step 10: Descriptor for Applications (앱 설정 파일)
* **핵심 내용**: 
  * 하드코딩된 자바스크립트 코드에서 앱의 메타데이터(버전, 앱 이름, 사용할 모델, 루트 뷰 등)를 분리하여 `manifest.json` 이라는 하나의 설정 파일(Descriptor)로 통합했습니다.
  * `webapp/manifest.json` 신규 생성:
    * `sap.app`: 앱의 ID, 버전, 다국어 파일(i18n), 제목 등을 정의합니다.
    * `sap.ui`: 앱이 실행될 디바이스 환경(데스크톱, 모바일 등)을 정의합니다.
    * `sap.ui5`: 진입점이 되는 첫 화면(Root View)과 앱 전체에서 사용할 데이터/언어 모델(Models)을 정의합니다.
  * `webapp/Component.js` 수정:
    * 기존에 직접 코딩했던 `JSONModel`과 `ResourceModel` 생성 코드를 싹 지웠습니다.
    * 대신 `metadata` 속성에 `manifest: "json"` 한 줄만 추가했습니다. 이렇게 하면 UI5 프레임워크가 알아서 `manifest.json` 파일을 읽고, 거기에 적힌 대로 모델과 화면을 자동으로 세팅해 줍니다!

---

## Step 11: Pages and Panels (페이지와 패널 레이아웃)
* **핵심 내용**: 
  * 허허벌판이던 흰 화면 대신, 화면의 레이아웃을 모바일 앱처럼 깔끔하게 잡아주는 기본 틀(`Page`, `Panel`)을 적용했습니다.
  * `webapp/view/App.view.xml` 수정:
    * 기존의 버튼과 입력창을 바로 나열하지 않고, `<App>` ➡️ `<Page>` ➡️ `<Panel>` 이라는 3겹의 레이아웃 컨트롤로 감쌌습니다.
    * `<App>`: 앱 전체를 감싸는 최상위 컨테이너로, 향후 화면 전환(네비게이션) 같은 모바일 기능을 지원합니다.
    * `<Page>`: 화면 상단에 파란색 헤더 바(제목 표시줄)를 만들어주고 전체 여백을 예쁘게 잡아줍니다.
    * `<Panel>`: 버튼과 입력창 주변에 테두리와 제목을 만들어 구역을 깔끔하게 나눠줍니다.
  * `webapp/i18n/i18n.properties` 수정:
    * 페이지 제목용(`homePageTitle=Walkthrough`)과 패널 제목용(`helloPanelTitle=Hello World`) 텍스트를 추가하고 XML 뷰에서 바인딩했습니다.

---

## Step 12: Shell Control as Container (쉘 컨테이너)
* **핵심 내용**: 
  * 앱 전체를 감싸는 가장 바깥쪽 레이아웃인 `<Shell>` 컨트롤을 도입했습니다.
  * `webapp/view/App.view.xml` 수정:
    * 기존 최상위 태그였던 `<App>`을 `<Shell>` 태그로 한 번 더 묶어주었습니다.
    * 모바일 환경에서는 큰 차이가 없지만, 데스크톱(PC)과 같이 큰 화면에서 앱을 열 때, 화면이 양옆으로 무한정 늘어나는 것을 방지해 줍니다. 
    * 앱을 화면 중앙에 적절한 너비로 예쁘게 배치(레터박스 효과)하여 가독성을 높여주는 역할을 합니다.

---

## Step 13: Margins and Paddings (여백 조정)
* **핵심 내용**: 
  * UI 요소들이 너무 딱딱하게 붙어 있는 것을 개선하기 위해, SAPUI5에서 기본 제공하는 표준 CSS 여백 클래스를 사용했습니다.
  * `webapp/view/App.view.xml` 수정:
    * `<Panel>`에 `class="sapUiResponsiveMargin"`을 추가하여 기기 화면 크기(PC, 폰)에 따라 바깥 여백이 알아서 조절되게 했습니다.
    * `<Button>`에 `class="sapUiSmallMarginEnd"`를 추가하여 오른쪽 입력창과의 간격을 살짝 띄워주었습니다.
    * 기존에 입력창 안에 있던 `description` 속성(Hello World 표시용)을 지우고, 대신 독립적인 `<Text>` 컨트롤을 새로 추가한 뒤 `class="sapUiSmallMargin"`을 주어 전체적인 모양새를 다듬었습니다.

---

## Step 14: Custom CSS and Theme Colors (커스텀 CSS)
* **핵심 내용**: 
  * SAPUI5가 제공하는 기본 디자인만 쓰지 않고, 나만의 독창적인 디자인(CSS)을 직접 추가하는 방법을 배웠습니다.
  * `webapp/css/style.css` 파일 신규 생성:
    * 나만의 스타일을 작성할 순수 CSS 파일을 만들었습니다.
    * 글씨를 굵게 만드는 `.myCustomText { font-weight: bold; }` 등의 클래스를 정의했습니다.
  * `webapp/manifest.json` 수정:
    * 앱이 켜질 때 이 CSS 파일도 자동으로 읽어오도록 설정 파일의 `sap.ui5` 항목 안에 `resources` 블록을 추가하고 CSS 경로(`css/style.css`)를 등록했습니다.
  * `webapp/view/App.view.xml` 수정:
    * 다른 앱과 CSS 이름표가 겹치지 않게(캡슐화) 하려고 최상위 `<App>` 태그에 `class="myAppDemoWT"`를 달아주었습니다.
    * 인사를 띄우던 기존 `<Text>`를 `<FormattedText>`로 바꾸고, `class="myCustomText"`를 붙여 우리가 만든 굵은 글씨 효과가 들어가게 했습니다. (추가로 `sapThemeHighlight-asColor`라는 기본 클래스도 달아 글씨가 테마 고유의 파란색으로 나오게 했습니다!)
