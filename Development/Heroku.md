# [Heroku Architecture](https://devcenter.heroku.com/categories/heroku-architecture)

Heroku 플랫폼의 high-level 아키텍처 구성 요소. 이 문서는 구성 요소들이 어떻게 잘 어울려 동작할 수 있게 하는지에 대한 좋은 설명을 제공합니다.

## [Heroku 동작 방식](https://devcenter.heroku.com/articles/how-heroku-works)

이것은 Heroku가 어떻게 동작하는지에 대한 높은 수준의 기술적 설명입니다. Heroku 플랫폼에서 앱을 구현하고, 구성하고, 배포하고, 실행하는 동안 만날 수 있는 개념들을 하나로 묶어줍니다.

* [Getting Started](https://devcenter.heroku.com/start) 튜토리얼 중 하나를 수행하면 이 문서의 개념들을 보다 구체적으로 이해할 수 있습니다.

이 문서를 순차적으로 읽으세요. 일관된 이야기를 하기 위하여 플랫폼을 설명하는 개념을 점진적으로 소개하고 설명합니다.

마지막 섹션에서는 모든 정의들을 한 번에 묶어 Heroku의 [deploy-time](https://devcenter.heroku.com/articles/how-heroku-works#deploy)와 [runtime-view](https://devcenter.heroku.com/articles/how-heroku-works#runtime)를 제공합니다.

### Difining an application

Heroku는 Ruby와 Node.js, Java, Python, Clojure, Scala, Go, PHP로 쓰여진 앱들을 배포하고 동작하고 관리할 수 있게 해줍니다.

앱은 이 언어들 중 하나로 쓰여진 (아마도 프레임워크로 작성되었을) 소스 코드와 앱을 빌드하고 실행하기 위해 추가적인 종속성이 필요한 빌드 시스템에 지시하는 중속성 설명의 묶음입니다.

- 용어: 앱은 소스 코드와 종속성들의 설명으로 구성되어 있다.

종속적인 메커니즘은 언어에 따라 다릅니다. Ruby에서는 `Gemfile`, Python에서는 `requirements.txt`, Node.js에서는 `package.json`, Java에서는 `pom.xml` 등을 사용합니다.

종속 파일과 함께 있는 당신의 앱의 소스 코드는 Heroku 플랫폼이 앱을 빌드하고, 실행될 수 있는 것을 생성할 수 있도록 정보를 충분히 제공해야 합니다. 

### Knowing what to execute

Heroku에서 앱이 동작하기 위해서 앱에서 많은 것들이 바뀔 필요가 없습니다. 앱의 어느 부분이 동작가능지를 플랫폼에 알리는 것만이 필요합니다.

만약 어떤 프레임워크를 사용하고 있다면 Heroku는 알 수 있습니다. 예를 들어 Ruby on Rails에서는 일반적으로 `rails server`이고, Django에서는 `python <app>/manage.py runserver`, Node.js에서는 `package.json`에 있는 `main`필드입니다.

- 용어: [Procfiles](https://devcenter.heroku.com/articles/procfile)는 프로세스 유형을 나열합니다. 실행하고 싶을 수 있는 명령어들입니다.

다른 앱들을 위해 실행될 수 있는 것을 명시적으로 선언해야 할 수 있습니다. 이것을 소스 코드와 함께 제공되는 [Procfile](https://devcenter.heroku.com/articles/procfile) 텍스트 파일에서 작업할 수 있습니다. 각 줄에서 [프로세스 유형](https://devcenter.heroku.com/articles/process-model)을 선언합니다. 이는 내장된 앱을 실행할 수 있는 명령입니다. 예를 들어 Procfile은 이렇게 생겼을 겁니다:

```shell
web: java -jar lib/foobar.jar $PORT
queue: java -jar lib/queue-processor.jar
```

이 파일은 `web` 프로세스 유형을 선언하고 이를 동작시키기 위해 수행되어야할 명령을 제공합니다. 이 경우에는 `java -jar lib/foobar.jar $PORT`입니다. 또한 `queue` 프로세스 유형과 해당 명령을 선언합니다.

- 용어: 앱은 소스 코드와 어떤 종속에 대한 설명과 Procfile로 구성됩니다.

Heroku는 모든 언어에서 비슷한 방식으로 종속성들과 Procfile을 활용하여 앱을 빌드하고, 실행하고, 확장할 수 있게 해주는 다각형 플랫폼입니다. Procfile은 앱의 아키텍처 측면을 보여냅니다. (위의 예제에는 두 가지 엔트리 포인트가 있음) 그리고 이 아키텍처는 당신이 각 부분들을 독립적으로 확장하게 해줍니다. Heroku에서 잘 동작하는 앱을 위한 아키텍처 원리에 대한 훌륭한 가이드는 [Architecting Applications for Heroku](https://devcenter.heroku.com/articles/architecting-apps)에서 볼 수 있습니다.

### Deploying applications

[Git](https://git-scm.com/)은 많은 개발자들이 소스 코드의 버전을 관리하기 위해 사용하는 강력한 버전 관리 시스템입니다. Heroku 플랫폼은 앱 배포를 위한 기본 수단으로 Git을 사용합니다. (API를 통해 Heroku로 소스 코드를 전송하는 많은 방법들이 있습니다.)

Heroku에서 앱을 생성할 때, 일반적으로 `heroku`라는 새 Git 리모트를 로컬 Git 저장소와 연결합니다.

결과적으로 코드를 배포하는 것은 `git push`와 비슷해지지고 `heroku` 리모트인 점이 다릅니다.

```shell
$ git push heroku master
```

- 용어: 앱을 배포하는 것은 당신의 앱을 Git, GitHub, Dropbox 혹은 API 통한 방법들을 사용하여 Heroku에 보내는 것을 의미합니다.

앱을 배포하는 많은 방법들이 있습니다. 예를 들어 당신은 [GitHub integration](https://devcenter.heroku.com/articles/github-integration)을 사용하여 각각의 새로운 pull request를 당신의 새로운 앱에 연결시킬 수 있습니다. 이는 모든 종류의 것들을 지속적으로 통합하여 진행할 수 있게합니다. 다른 방법으로는 [Dropbox Sync](https://devcenter.heroku.com/articles/dropbox-sync)를 사용할 수 있습니다. 이 방법은 Dropbox의 폴더들의 컨텐츠를 Heroku에 배포할 수 있게 해줍니다. 마지막으로 앱을 빌드하고 배포하기 위해 [Heroku API](https://devcenter.heroku.com/articles/build-and-release-using-the-api)를 사용할 수도 있습니다. 

배포는 당신의 앱이 로컬 시스템에서 Heroku로 움직이는 것을 말합니다. 그리고 Heroku는 앱이 배포될 수 있는 몇 가지 방법을 제공합니다.











---





# [Ruby on Heroku](https://devcenter.heroku.com/categories/ruby)

Heroku는 클라우드에 Ruby 앱을 쉽게 배포하고 확장할 수볼게 해줍니다. 당신이 Si사용할atra나 Rails와 같은 프레임워크를 선호하든, Unicorn 혹은 원시 소켓으로 손을 더럽히든 Heroku는 상관하지 않으므로, 좋아하는 툴을 이용해서 자신의 방법으로 빌드할 수 있습니다.