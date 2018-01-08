[Next.js](https://github.com/zeit/next.js/)

Next.js는 서버 렌더링 된 React 애플리케이션 위한 가장 단순한 프레임워크



# Getting Started

요즘에 싱글 페이지 자바스크립트 어플리케이션을 개발하는 것이 꽤 어려울 수 있다는 걸 모두가 알고 있습니다. 다행히도 간단하고 빠르게 애플리케이션을 빌드할 수 있게 도와주는 몇 가지 프로젝트가 있습니다.

[Create React App](https://github.com/facebookincubator/create-react-app)은 이에 대한 좋은 예제입니다.

하지만 여전히 적당한 애플리케이션을 빌드하기까지 높은 러닝 커브가 있습니다. 클라이언트 사이드 라우팅과 페이지 레이아웃 등등을 배워야하기 때문입니다. 만약 더 빠른 페이지 로드를 위해 서버 사이드 랜더링을 수행하기 원한다면 이는 더 어려워질 수 있습니다.

**그래서 우리는 간단하지만 자유롭게 맞춤으로 설정할 수 있는 것이 필요합니다.**

웹 애플리케이션이 어떻게 PHP로 만들어지는지 생각해봅시다. 몇 가지 파일들을 만들고, PHP 코드를 작성한 다음 간단하게 이를 배포합니다. 라우팅에 대해 걱정하지 않아도 되고 이 애플리케이션은 기본적으로 서버에 랜더링됩니다.

**NEXT.js**

이것이 바로 Next.js에서 우리가 하는 일입니다. PHP 대신에 우리는 JavaScript와 React를 사용하여 애플리케이션을 생성합니다. Next.js가 제공하는 몇 가지 유용한 기능들은 다음과 같습니다.

- 기본적으로 서버 랜더링이 됨
- 자동으로 더 빠르게 페이지 로드를 하기 위해 코드를 쪼갬
- 간단한 클라이언트 사이드 라우팅 (페이지 기반)
- [How Module Replacement(HMR)[https://webpack.js.org/concepts/hot-module-replacement/]을 지원하는 Webpack 기반의 개발 환경
- Express나 다른 Node.js HTTP 서버를 실행할 수 있음
- 사용하고 있는 Babel과 Webpack 환경 설정을 맞춤으로 설정할 수 있음

