# 브라우저 렌더링 방식

[이전 글로 이동하기 -> CSR(Client Side Rendering) vs SSR(Server Side Rendering)](../CSR-SSR/CSR-SSR.md)

브라우저가 화면에 나타나는 모든 것을 렌더링 할 때, 렌더링 엔진과 자바스크립트 엔진을 사용해 다음과 같은 단계를 거칩니다.<br>

![FrontEnd 01](../../../Image/frontend-01.png)<br>

먼저 클라이언트가 구글에 접속하게 되면 브라우저가 구글 서버에 요청(Request)를 보내게 된다.<br>

브라우저는 구글 서버로부터 요청에 맞는 응답(Response)을 받게 되며 HTML / CSS / JavaScript / 이미지 등을 응답받는다. 그리고 아래와 같은 단계를 거칩니다.<br>

1. 렌더링 엔진이 HTML 파싱 후, DOM Tree 구축<br>

2. 렌더링 엔진이 CSS 파싱 후, CSSDOM Tree 구축<br>

3. HTML 파서가 script태그를 만나면 자바스크립트 코드 실행을 위해 DOM Tree 구축을 정지하고 자바스크립트 엔진으로 제어 권한을 넘긴다. 그 후 자바스크립트 엔진이 자바스크립트를 로드하고 파싱하여 실행한다.<br>

4. 자바스크립트 실행이 완료되면 다시 렌더링 엔진에게 제어 권한을 넘겨 나머지 HTML 파싱을 실행한다.<br>

5. DOM 및 CSSOM을 결합하여 렌더링 트리를 형성합니다.<br>

6. 뷰포트 기반으로 렌더트리의 각 노드가 가지는 정확한 위치와 크기 계산(Layout/Reflow 단계라고 불리움)<br>

7. 개별 노드를 화면에 페인트합니다.(Paint 단계)<br>

위와 같이 브라우저는 동기적으로 HTML / CSS / JavaScript를 처리하기 때문에 script태그의 위치가 중요한 역할을 한다.<br>

body태그 맨 아래에 script를 위치하는 이유도 DOM 생성이 완료되지 않은 시점에 자바스크립트를 조작하면 에러가 발생할 수 있기 때문이다.<br>

현재는 head 태그 안에서 script 태그를 위치하고 defer 속성을 가져 비동기적으로 파싱하는 방법도 좋다고 알고 있다.<br>

[다음 글로 이동하기 -> 브라우저 동작 방식](../Browser-Action/Browser-Action.md)
