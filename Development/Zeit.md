[Zeit 홈페이지](https://zeit.co/)



# 소개

```shell
~/my-app $ now
> Deploying...
> Ready!
```

Instant Global Deployments

단 한 줄의 명령어로 전 세계에서 가장 강력한 서버없는 플랫폼에 배포하세요.



## 서버가 아닌 프로덕트에만 집중하세요.

```js
~/my-app $ ls
package.json	index.js	lib		static
~/my-app $ now
> Ready! https://my-proj-hj1v2m.now.sh
(copied to clipboard) [440ms]
> Upload [=====================] 100% 5.7s
> Sync complete (1.38MB) [4759ms]
```

우리의 애플리케이션을 설치하고 모든 플랫폼에서 쉽게 배포하세요.

- 원하는 프로그래밍 언어나 프레임워크에서 코드를 작성하세요.
- 노력없이 무한히 확장하세요.
- 마이크로 서비스로 API를 구성하고 나중에 [합치세요](https://zeit.co/docs/features/path-aliases).



## 전체 구성원들을 하나의 워크플로우로 모으세요.

![./images/zeit_1.png](./images/zeit_1.png)

실시간으로 협업하여 배포하세요.

- 팀의 활동들을 즉시 시각화하세요.
- 배포, DNS 기록, 인증 - 모든 것을 한 곳에서
- 플랜을 구매하거나 온디맨드를 결제하세요.



## 계속 반복할 수 있습니다. 제한 없이. 끊김없이.

Now Alias를 만나세요. 서비스 중단 없이 계속 업그레이드 할 수 있습니다.

```shell
~/my-app $ now alias my-app-h3o2mc.now.sh my-app.com
> my-app.com is a custom domain
> Verifying the DNS settings for my-app.com
> Detected zeit.world nameservers! Configuring records...
> DNS Configured! Verifying propagation...
> Verification OK!
> Provisioning certificate for my-app.com
> Success! my-app.com now points to my-app-hei2m.now.sh!
```



## 자동으로 보안을 지킵니다.

**SSL 인증서**를 자동으로 제공하고 갱신합니다.

[Let's Encrypt](https://letsencrypt.org/) 덕분에 원하는 만큼 여러 도메인에 무료로 보안을 지킬 수 있습니다.



## 완전히 프로그래밍적입니다.

우리의 가장 단순하고 강력한 [API](https://zeit.co/api)는 이미 사용하는 스택이나 클라우드에 통합될 수 있습니다.

CI 파이프라인을 통합하세요. UI와 비즈니스에 필요한 워크플로우를 구현하세요.



## 한계가 없습니다.

모든 변경 사항을 코드에 배치할 수 있습니다. 그런 다음 유저가 사용하는 도메인과 API가 어느 것과 연견되는지 결정하세요. 무언가 잘못되었나요? **같은 명령어로 롤백하세요!**



## zeit.world - 자유로운 글로벌 DNS

DNS와 기록 수정, CNAME 혹은 ALIAS에 대해 걱정할 필요가 없습니다. `now alias`를 실행하여 여러분의 배포를 기억할 수 있는 도메인이나 서브 도메인에 연결하세요. 배포, 스테이징, 프로덕션 모두 한 곳에서.