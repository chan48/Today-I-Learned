[Skylight Document](https://www.skylight.io/support)

# Getting started

## Quick Start

당신의 앱이 Skylight가 동작할 수 있는 [조건](https://www.skylight.io/support/advanced-setup#requirements)들을 충족하는지 확인하세요. 다음 과정들은 당신의 로컬 개발 환경에서 수행되어야 합니다.

1. Gemfile에 Skylight를 추가하세요. 모든 환경에 포함시키세요.

```
# Gemfile
gem "skylight"
```

2. [이곳](https://www.skylight.io/app/setup)에서 토큰을 발급받으세요.
3. 터미널에서 아래의 명령어를 실행시키세요.

```
bundle install
bundle exec skylight setup <setup token>
```

이 명령은 자동으로 `config/skylight.yml` 파일을 생성해줍니다.

4. 프로덕트에 당신의 앱을 배포하세요.

환경 변수 설정이나 커스텀 환경 설정, 레일스가 아닌 다른 프레임워크에서 Skylight를 사용하는 것과 같이 더 자세한 설치 방법을 보고 싶으시면 [이곳](https://www.skylight.io/support/advanced-setup)을 클릭하세요.



## 특징 오버뷰

### 실용적인 인사이트

Skylight는 당신의 앱 퍼포먼스 데이터를 이해하고 활용하기 쉽게 만들어줍니다.

- True Response Times와 Request Aggregation은 당신이 삽질하는 것을 막아줍니다.
- Agony로 정렬하기는 당신의 리스트의 맨 위쪽에 가장 심각하게 개선이 필요한 엔드포인트를 위치시켜줍니다.
- Heads Ups은 당신의 앱을 느려지게 만들 가능성이 있는 코드를 알려줍니다.

### 앱 대쉬보드

![앱 대쉬보드](https://d1xmotl1g5cxcm.cloudfront.net/production/assets/skylight/docs/features/app-dashboard-d3a7698c1936566593b3af5782acdb0a1f05808c24d249ebb40380bab03202aa.png)

Skylight에 접속하면 처음으로 보게되는 앱 대쉬보드다. 앱 대쉬보드는 앱이 어떻게 수행되는지 자세히 보여주고 어떤 것을 알아봐야하는지를 알려준다.

[이곳](https://www.skylight.io/support/skylight-guides#navigating-your-app)에서 더 자세히 배울 수 있다.

### 엔드포인트 뷰

![엔드포인트 뷰](https://d1xmotl1g5cxcm.cloudfront.net/production/assets/skylight/docs/features/endpoint-view-35b9bf07a8e19dbeb542ce72d6ea5341b2075b59855a5d7fdb570c6a957b0024.png)

엔드포인트 뷰는 Skylight의 핵심이다. 이 페이지는 수천 개의 데이터 포인트를 실행 가능한 정보로 정리한 결과로 앱 속도를 높이는데 활용할 수 있다.

[이곳](https://www.skylight.io/support/skylight-guides#navigating-your-endpoint)에서 더 자세히 배울 수 있다.



### 트렌드 이메일

![트렌드 이메일](https://d1xmotl1g5cxcm.cloudfront.net/production/assets/skylight/docs/features/trends-email-071d421c5e4e7bf85584ad9138ae913303093bf4cb97d52e0b39841fe393d1c1.png)

우리의 주간 트렌드 이메일은 당신의 퍼포먼스 데이터를 아름답고 활용가능한 레포트로 만들어준다. 이것은 당신의 고객이 속도 저하를 경험하기 전에 캐치할 수 있게 도와준다.

[이곳](https://www.skylight.io/support/skylight-guides#staying-the-course)에서 더 자세히 배울 수 있다.



##Skylight의 철학 

### 데이터가 아닌 해답

엄청나게 많은 그래프와 차트로 당신을 괴롭히지 않도록 Skylight는 퍼포먼스 데이터를 실용적인 인사이트로 변환한다. 그래서 너는 분석하는 시간을 줄이고 개선시키는 데에 더 많은 시간을 쏟을 수 있다.

우리의 [UI](https://www.skylight.io/support/skylight-guides)는 사용하기 예쁘고 재밌다. 우리는 가능한 적은 지시로 최고의 인사이트를 제공하기 위해 최적화 했다. 우리는 정확하게 보여질 수 없는 많은 데이터를 제공하는 것은 나쁜 경험이라는 생각을 가지고 Skylight의 UI를 접근성있게 만들기 위해 노력했다.

우리는 실제로 당신이 당신의 앱을 개선시킬 수 있도록 하는 특징들에 집중했다. 예를 들어 개선점들의 우선순위를 정하기 위한 Agony와 잘못될 가능성이 있는 코드를 알려주는 heads up, 그리고 당신의 앱이 가장 많은 시간을 보내고 있는 곳이 어디인지를 결정하는 Event sequence들이 있다.

당신의 앱이 왜 느린지 배우기 위해 고군분투하지 마라. Skylight로 해답을 찾아라.

### True response times

![true response times](https://d1xmotl1g5cxcm.cloudfront.net/production/assets/skylight/docs/features/response-times-49621690cd1a29190c7d3931ce9fd906af024e9f82c3b330cb83e8d8cb1d3f8c.png)

당신의 앱의 응답 시간는 Skylight의 여러 위치에서 보여준다. 대부분의 경우 우리는 두 가지 방법 중 하나로 숫자를 말한다. "problem" 응답 시간은 95 백분위 그에 반해 "typical" 응답 시간은 절반이다.

대부분의 툴들은 단지 평균값만을 보여준다. 이는 계산하기 쉽지만 자체적으로 제한된 유용성을 제공한다. Skylight에서는 항상 95 백분위의 응답 시간을 보여준다. 이것이 백엔드에서 계산할 때 더 많은 계산량을 필요로 하지만 실제 성능을 나타내는 것에는 훨씬 더 나은 수치이다.

웹 퍼포먼스에 대해 고민할 때 평균은 대부분 유용하지 않다. 최악의 경우에는 잘못된 방향으로 갈 수 있다. 더 많은 정보를 위해  [DHH’s blog post, The Problem with Averages](http://signalvnoise.com/posts/1836-the-problem-with-averages) 글을 읽어보아라. 구글, 트위터, 깃헙은 모두 95 백분위 숫자의 성능을 추적한다.

Skylight의 백분위 접근에 대해 더 알아보고 싶으면 [이곳](http://blog.skylight.io/the-log-normal-reality/)을 보아라.

### 집계 VS 샘플링

대부분의 다른 프로파일링 툴들은 당신에게 엔드포인트에 대한 자세한 정보를 제공하기 위해 샘플링에 의존한다. 이는 몇 가지 가치를 제공하는 동안 모든 리퀘스트에 대한 정보를 제공하지 않는다. 샘플이 얼마나 자주 발생하는지에 따라 스파이크 및 비정상적인 동작을 놓칠 수 있다.

Skylight는 모든 리퀘스트 정보를 추적한다. 우리는 당신의 앱의 대표적인 피처를 제공하기 위해 데이터를 집계하고 나머지는 삭제한다. 이것은 무시되는 리퀘스트가 없다는 의미이고 우리는 당신에게 모든 정보를 주고 당신의 앱이 가속화할 수 있도록 만들어준다.