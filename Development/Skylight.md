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



# Performance Tips

## Allocation Hogs

당신의 레일스 앱은 대부분의 시간에는 잘 동작할 것이다. 그러니 당신의 유저들은 때때로 조용히 매우 느린 요청을 랜덤하게 본다. 설명되지 않은 속도 저하가 많은 이유 때문에 일어날 수 있을지라도 대부분의 공통적인 근본 이유는 거대한 객체 할당들 때문이다.



### Garbage collection pauses

루비에서 당신이 객체를 생성하면 결국에는 정리해야 한다. 그 정리(GC)는 대게 처음 객체를 생성하는 코드로부터 멀리 떨어져있다.

심지어 GC가 완전히 다른 요청에서 발생하는 것은 드물지 않습니다. 각 요청은 작은 스케일과 GC 실행 트리거 발생 가능성이 적기 때문에 결과적으로 당신의 유저들은 무작위로 속도저하를 보게 됩니다.

이 미스터리를 설명하는 것은 문제를 해결하는데 도움이 되지 않습니다. 다행히 Skylight는 당신의 모든 요청들의 객체 할당들을 추척하여 당신이 가장 큰 손상을 입히는 부분을 제거하도록 도와줍니다.



### Identifying allocation hogs

할당이 많은 엔드포인트들은 Skylight에서 "heads up"이라는 파이 차트로 표시됩니다.

![heads up](https://d1xmotl1g5cxcm.cloudfront.net/production/assets/skylight/docs/features/heads-up-allocation-hog-d3918c4bc46ca197d8768cfbd98b0226a707e21cce43fca244ca2c04e0412924.png)

기본적으로 Skylight는 얼마나 오랜 시간 동안 당신의 엔드포인드들과 이벤트들이 동작하는지에 집중합니다. 당신이 엔드포인트들을 파고 들때 완료까지 오랜 시간이 걸리는 이벤트들이 나타나는 엔트포인트들을 볼 것입니다. 너가 allocation mode로 전환할 때 같은 추적이 각 이벤트가 동작하는 동안의 allocation의 수에 기초하여 재배치될 것입니다. 이것은 당신이 더 빠르게 문제 가능성이 있는 이벤트들을 볼 수 있게 해줍니다.

![](https://d1xmotl1g5cxcm.cloudfront.net/production/assets/skylight/docs/features/allocations-mode-9542a40e1df04e5f09fd65fd576b12128ab2e62ee16c0f2fdf194fa70dfaf5cb.gif)

### Fixing the problem

이제 당신은 작업해야할 부분을 알았으니 할당을 줄일 가장 효과적인 방법에 대해 얘기하자.

Pro tip: 당신이 이 섹션에 있는 팁들을 모든 코드에 적용하기 몇 주 전에 부인해야 한다. 할당을 줄이는 것은 극소의 최적화이란 것을. 이것은 그들이 핫 패스에 매우 적은 이익을 생산하고 당신의 미래의 생산성을 줄일 것이다. 어떤 것은 아마도 캐시같은 적은 최적화를 중요하게 하는 능력을 줄일 수도 있다. 할당하는 핫 스팟을 확인하고 당신의 에너지를 이 곳에 집중하기 위해 Skylight를 사용해라

우리가 핫 패스로부터 의미는 무엇이냐? 짧게 말하면 루프다. 200개의 객체들이 생산된느 코드의 조각은 가끔씩만 불린다면 잘 동작할 것이다. 하지만 만약 루프에서 두천번 동안 그 코드가 불린다면 이것은 다 합쳐질 것이다. 심지어 이것은 너가 다른 루프로 그 루프를 감싸면 작은 코드 조각이 우연하게 수천번의 수백만번이 실행될 것이다.

모든 것이 명백하게 보일지 모르겠지만 루비같은 고수준 프로그래밍 언어는 루프가 기본적인 시야에서 사라지기 쉽다. 

여기에 당신의 앱에서 메모리 핫스팟을 확인하기 위해 Skylight가 사용했던 최근의 코드 예시가 있다.

```
def sync_organization(organization)
	Mixpanel.sync_organization(organization)
	
	organization.users.each do |user|
		Mixpanel.sync_user(user)
	end
end
```

처음 보았을 때 당신은 이 코드가 루프를 가진 것을 볼 수 있지만 그렇게 나빠보이지는 않을 것이다. 그러나 `sync_user` 메서드는 아래처럼 보이는 많은 라인을 포함한 30라인의 메서드를 불린다.

```
user.apps.includes(:organization).map(&:organization)
```

여기 우리는 organization에서 각 유저들을 통해 루프를 돌리고 있다. 그러면 각 유저는 모든 앱들을 통해 루프를 돈다. 지금 상상해라 `sync_organization` 각각은 각 요청에서 여러번 불린다. 당신은 어떻게 이것이 빠르게 합쳐지는지 볼 수 있다.

이 짧은 코드는 여러 숨겨진 루프들을 포함하고 이 루프들은 수 많은 객체들을 할당한다. 할당 핫스팟을 파헤칠때 첫번째 스탭은 루프를 확인하는 것이다. 그리고 저는 특별히 이 숨겨진 루프들을 경계해야한다. 쉽게 놓칠 수 있기 때문에.

숨겨진 루프들의 몇 가지 예시:

- `each`나 `map`(특히 프레첼 연산자라고 부르는 &:를 사용할 때) 같은  `Enumerable`에 있는 메서드들 혹은 다른 콜렉션들
- Active Record association들과 다른 관계(위의 예시에 있는 `user.apps`나 관계에서 `destroy_all`을 호출하는 것과 같은 것들)
- 파일을 가지고 동작하는 API들과 다른 IO 스트림들
- 다른 라이브러리들에 있는 API들, 특히 and/or 블록 콜렉션들을 가져오는 API들

문제는 루프 자체가 아니라는 점을 명심해라. 하지만 어떤 반복되는 작업의 효과를 확장하는 루프는 좋다. 그러므로 문제를 해결하는 두 가지 기본적인 전략은 루프를 줄이거나 루프의 각 반복에서의 작업을 줄이는 것이다.

### 반복 줄이기

루비에서 당신이 수행하는 반복의 수를 줄이는 가장 일반적인 방법 중 하나는 작업을 조합하기 위해 Active Record 메서드들을 사용하고 무거운 작업을 위해 당신의 데이터베이스의 장점을 취하는 것이다.

예를 들어 만약 당신이 모든 유저들을 거치는 국가들의 유니크 리스트를 얻으려고 한다면 이렇게 쓰려는 마음이 들 것이다.

``` ruby
User.all.map(&:country).uniq
```

이 코드는 각 로우의 Activce Record 객체들을 생성하기 위해 그들을 거치는 반복문으로 당신의 데이터베이스에서 모든 유저들을 가져온다. 그 다음 그것은 다시 한 번 각 국가를 포함하는 새로운 배열을 만들이 위해 전체를 반복한다. 마지막으로 이것은 루비에서 각 나라들을 속이기 위해 배열 전체를 반복한다.

너가 볼 수 있듯 이 방법은 결국 많은 낭비를 초래한다. 따라서 이 낭비는 많은 불필요한 객체를 할당한다. 대신 당신은 데이터베이스가 이 작업을 수행하도록 만들 수 있다.

```
User.distinct.pluck(:country)
```

이것은 보이는 것과 같은 SQL 쿼리를 실행한다.

```
SELECT DISTINCT "country" FROM "users";
```

모든 유저를 위해 루비 객체를 만들고 그 객체들을 여러번 루프 도는 것 대신에 디것은 직접 데이터베이스 자체에서 나라들의 배열을 생성한다. 당신의 앱에서 동작을 줄여줄 뿐만 아니다 당신의 데이터베이스가 수행해야할 일을 줄여주기까지 한다.

이 기술은 업데이트와 삭제에서도 똑같이 동작한다. 루비에서 당신이 바꾸길 원하는 객체마다 반복하는 대신에

```
User.where("last_seen_at < ?", 1.year.ago).each(&:destory)
```

당신은 직접 데이터베이스에서 같은 동작을 수행시킬 수 있다.

```
User.where("last_seen_at < ?", 1.year.ago).delete_all
```

이것은 아래와 같은 SQL 쿼리를 실행시킨다.

```sql
DELETE FROM "users" WHERE last_seen_at < ...;	
```

데이터베이스가 삭제를 담당하기 때문에 Activce Record의 밸리데이션과 콜백이 동작하지 않을 것이다. 그래서 당신은 항상 디 이 기술을 사용할 수는 없을 것이다. 이것은 `update_all`도 마찬가지다.

만약 당신이 당신 스스로 루비에서 Active Record 객체를 도는 루프를 발견한다면 몇몇 동작들을 데이터베이스로 전환할 방법이 있을 것이다. [Active Record Query Interface guide](http://guides.rubyonrails.org/active_record_querying.html)는 시작하기 좋은 문서다.

같은 줄들을 따라서 많은 숫자의 Active Record 객체들을 루프할 때 `find_each`와 같은 [batching API](http://api.rubyonrails.org/classes/ActiveRecord/Batches.html) 사용을 고려해라. 그들이 마침대 전체 할당 숫자를 줄이지 않을 동안 그들은 같은 시간 동안 더 적은 객체들이 홀딩되도록 할 것이다. 이것은 GC가 더 효율적으로 동작하도록 해준다.

### Allocate Fewer Objects Per Iteration

만약 당신이 절대적으로 핫패스에서 루프가 있어야만 한다면 각 루트에서 더 적은 객체가 할당되는 방법을 찾아야 한다. 

가장 빠른 방법은 루프 밖의 움직이는 공유된 작업을 여기에 포함하는 것이다. 그리고 겉으로 좋은 구조를 찾는 것이다. 객체 할당이 필요한. 이 가설의 예시를 보자.

```ruby
module Intercom
	def self.sync_customers
		Intercom.customers.each do |customer|
			if customer.last_seen < 1.year.ago
				cutstomer.deactivate!
			end
			
			if blacklisted?(domain: customer.email.domain)
				custom.blacklist!
			end
			
			log "Processed customer"
		end
	end
	
	def self.blacklisted?(options)
		["hacked.com", "l33t.com"].include?(options[:domain])
	end
end
```

겉보기에 간단한 이 예제에는 각 반복문에 우리가 할당한 불필요한 객체들이 있는 장소가 여러군데 있다.

- `1.year.ago` 호출은 매 반복에서 여러 새로운 객체들을 생성한다.
- `blacklisted?` 함수 호출은 hash(`{domain: customer.email.domain)}`)을 할당한다.
- `log` 호출은 `"Processed customer"` 문자열의 새 복제본을 할당한다.
- `blacklisted?` 메서드는 이 것의 안에 있는 문자열을 가지고 불릴때마다 하나의 배열을 할당한다.

이 루프의 약간 다른 버전은 많이 적은 할당을 가진다.

```Ruby
module Intercom
	def self.sync_customers
      	inactivity_threshold = 1.year.ago
      
		Intercom.customers.each do |customer|
			if customer.last_seen < inactivity_threshold
				cutstomer.deactivate!
			end
			
			if blacklisted?(domain: customer.email.domain)
				custom.blacklist!
			end
			
			log "Processed customer".freeze
		end
	end
	
  	BLACKLIST = ["hacked.com", "l33t.com"]
  
	def self.blacklisted?(domain:)
		BLACKLIST.include?(domain)
	end
end
```

얼마나 많은 고객들이 이 코드를 반복하는지에 따라 우리는 많은 할당 수를 절약할 수 있게 됩니다.

- 우리는 `1.year.ago`를  루프 밖으로 빼내어 전체 루프에서 하나의 객체만 공유하여 사용하여 단 한 번만 할당하게 됩니다.
- 우리는  `blacklisted?`의 키워드 파라미터를 바꿨습니다. 이는 Ruby 2.2 버전부터 해쉬를 위해 필요한 것입니다.
- 우리는 블랙리스트 배열을 상수로 옮겼습니다. 이것은 전체 함수에서 공유할 수 있습니다.
- 우리는 `"strong".freeze`를 사용하여 하나의 문자열만 만들어지고 재사용됩니다.

마지막 포인트는 특별이 언급할 만하다. 당신의 문자열을 선언할 때 변하지 않을 것이라고 선언함으로써 Ruby는 여러 함수를 거치는 같은 문자열 객체의 인스턴스를 재사용할 수 있다. 이것은 기본적으로 읽기 쉽게 문자열을 즉시 처리하면서 Ruby가 문자열을 가져오도록 허용한다. (비슷하게 블랙리스트 상수를 가져오는 방법)

`freeze` 함수는 보기에 좋지 않지만 곧 지나갈 것이다. Matz는 오랜 기간 동안 이를 기본값으로 만들어가로 발표해왔다. 정규 표현식, 정수(`Fixnum`들) 그리고 대부분의 실수 값들은 현재의 루비 버전들에서 이미 비슷한 최적화를 했다. 그래서 당신은 일반적으로 이것들을 신경쓰지 않아도 된다.