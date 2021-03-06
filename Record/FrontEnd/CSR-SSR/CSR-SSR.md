# CSR(Client Side Rendering) vs SSR(Server Side Rendering)

### 배경

웹 어플리케이션은 두 가지 측면이 존재한다.<br>
한쪽은 Client측, 다른 한쪽은 Server측면이 있다.<br>
여기서 클라이언트가 어떤 웹페이지 주소를 접속하게 되면 서버에게 여러가지를 요청(Request)하게 되고 서버는 그 요청에 맞는 데이터를 응답(Response)하게 된다.<br>
그리고 클라이언트는 서버가 응답한 정보를 토대로 HTML과 DOM을 렌더링하는 작업을 거쳐 사용자의 시각으로 보여주게 되는 것이다.<br>

시간이 흐르면서 웹 어플리케이션에서 발생하는 사용자 인터랙션이 증가하게 되고 그로 인해 요청을 보내고 응답을 받는 과정이 많아지게 됨으로써<br>
서버 입장에서 수많은 요청을 처리하게 되므로 부하가 커지게 됩니다.<br>
이 때 AJAX라는 기술이 등장하게 되는데 이는 유저가 웹사이트에서 인터랙션을 발생시키면 일부분만 정보를 다르게 표현해주는 방식으로<br>
HTML은 건들지 않고 특정부분에 대한 정보만 바꿔서 보여주는 컨셉이 자리잡게 된다.<br>
이로 인해 웹에서는 비동기 처리가 가능해지게 되었고 여기서 만족하지 않고 SPA(Single Page Application)라고 불리는 개념을 탄생시킵니다.<br>
이것은 라우트를 이동하게 될 때마다 웹페이지가 새로고침 되고 새로운 HTML을 다시 클라이언트에게 내려주는 MPA(Multiple Page Application)와 달리<br>
URL을 이동해도 새로고침이 발생않으며 HTML을 다시 내려주지 않는 방식을 의미합니다.<br>
SPA를 지원하는 대표적인 라이브러리에는 React / Vue / Angular 등이 있고 이들은 CSR(Client Side Rendering)으로 동작하게 됩니다.<br>

### CSR(Client Side Rendering)

- 특징

  CSR은 Server에서 가장 기본적인 HTML 파일만을 받아오고 클라이언트 측에서 script태그로 불러오는 자바스크립트를 통해 렌더링을 하게 됩니다.

  때문에 웹에서 발생하는 모든 인터랙션은 더 이상 새로고침을 발생하지 않습니다.

- 장점

  - 사용자 경험을 좋게 만들어 준다.

  - URL이 변경되어도 Server에 요청하지 않기 때문에 Server의 과부하를 줄여준다.

- 단점

  - 사용자가 첫 화면을 보기까지 오랜 시간이 걸린다.

    하나의 번들된 자바스크립트 파일을 클라이언트 측에서 다운로드 받아 실행하는데까지 시간이 걸리기 때문이다.

  - SEO(Search Engine Optimization) 관점에서 불리함을 보여준다.

    SEO는 HTML Meta 태그와 여러가지 정보를 바탕으로 적용되는데 CSR에서 서버로부터 받은 HTML에 아무 정보가 없기 때문에 SEO가 제대로 적용이 되지 않는다.

### SSR(Server Side Rendering)

- 특징

  SSR은 Client가 페이지를 요청할 때마다 Server로부터 해당 페이지에 관련된 HTML / CSS / JavaScript 및 데이터를 받아와서 렌더링합니다.

  때문에 페이지를 이동하면 그에 따른 HTML / CSS / JavaScript를 새로 받아야 하기 때문에 새로고침이 발생합니다.

- 장점

  - CSR과 달리 초기 렌더링의 속도가 빠르기 때문에 사용자가 첫 화면을 빠르게 볼 수 있다.

  - 모든 데이터가 HTML에 담겨 있기 때문에에 SEO 관점에서 유리함을 보여준다.

- 단점

  - 매번 페이지를 요청할 때마다 새로고침 되기 때문에 사용자 경험이 SPA에 비해서 좋지 않다.

  - 사용자가 많을수록 서버에 들어오는 요청이 많아 과부하가 걸리기 쉽다.

  - 사용자가 첫 컨텐츠를 보게 되는 시점 TTV(Time To View)와 사용자가 컨텐츠를 인터랙션하는 시점 TTI(Time To Interaction)의 공백기간이 꽤 길다.

    이는 사용자가 첫 컨텐츠를 보고 인터랙션을 시도해도 자바스크립트를 다운받지 못해 웹에서 반응을 하지 못하는 상황을 발생시킵니다.

### CSR + SSR

위와 같이 CSR과 SSR은 서로 장점도 존재하지만 단점도 존재하기 때문에 단점을 매울 수 있는 방법을 찾으려고 노력해야 합니다.<br>
CSR에서는 어떻게 SEO를 좋게 만들 수 있는지, SSR에서는 어떻게 TTV와 TTV의 격차를 줄여 사용자 경험을 좋게 만들 수 있는지를 생각할 수 있습니다.<br>
또한 요즘은 CSR | SSR을 분리하기 보단 서로의 장점을 살려 CSR + SSR을 합친 웹 어플리케이션도 존재한다.<br>
대표적인 예로 SPA를 사용하는 React에 SSR을 도입한 GatsbyJS | NextJS와 Vue에 SSR을 도입한 NuxtJS등이 있습니다.<br>
이들은 CSR에서 단점으로 작용한 좋지 않은 SEO 효율을 높이기 위해 첫 렌더링을 할 때만 서버에서 렌더링을 하고 그 이후로부터는 CSR을 사용하여 사용자 경험을 높이는 방식으로 많은 개발자들이 최근 선호하는 방식이라고 할 수 있습니다.<br>
