---
title: "[Github Blog] jekyll 설치 시 ERROR: Error installing jekyll 오류 해결"
excerpt: "gem install jekyll 명령 실행 시 ERROR: Error installing jekyll, sass-embedded 오류 해결 방법"
categories:
  - Github_Blog
tags:
  - Github_Blog
  - ruby
toc: true
toc_sticky: false
toc_label: "목차"
toc_icon: "blog"
---

---

## 문제 상황

깃헙 블로그를 위해 jekyll gem을 설치할 때, 아래와 같은 에러가 발생했습니다.

```sh
ERROR:  Error installing jekyll:
        The last version of sass-embedded (~> 1.54) to support your Ruby & RubyGems was 1.63.6. Try installing it with `gem install sass-embedded -v 1.63.6` and then running the current command again
        sass-embedded requires Ruby version >= 3.0.0. The current ruby version is 2.7.0.0.
```

이를 해결하기 위해선 에러 메시지에 나온 내용대로 루비의 버전을 3.0.0 이상으로 업그레이드 해야합니다.

하지만 apt 패키지 관리자를 이용해 삭제, 업그레이드 등을 시도해봐도 로컬에 설치된 루비의 버전을 2.7.0에서 바꿀 수 없었습니다.  
( 이에 대해선 아래 System Ruby를 참고! )

## 해결 방법

결론부터 말하자면 `rbenv`와 같은 루비 버전 관리자를 사용해 필요한 버전의 루비를 설치하고, 새로 설치한 버전을 global 또는 local에서 활용할 버전으로 등록해 사용하면 됩니다.

:bulb: **info**  
rbenv의 버전에 따라 설치할 수 있는 루비 버전이 정해져있으므로, 직접 깃헙 리포지토리를 클론해서 사용하는 방법을 권장합니다.
{: .notice--info}

rbenv 깃헙 리포지토리를 클론해서 rbenv를 설치하는 방법은 다음과 같습니다.

```sh
$ git clone https://github.com/rbenv/rbenv.git ~/.rbenv
$ echo 'eval "$(~/.rbenv/bin/rbenv init - bash)"' >> ~/.bashrc
$ source ~/.bashrc # 또는 터미널을 껐다가 다시 켜도 된다.
```

rbenv를 설치했다면, 다음 명령어로 설치할 수 있는 루비 버전을 확인할 수 있습니다.

```sh
$ rbenv install -l # 설치할 수 있는 최신 안정화 버전을 모두 보여준다.
$ rbenv install -L # 설치할 수 있는 모든 루비 버전을 보여준다.
```

:bulb: **info**  
만약 `rbenv install` 명령을 찾을 수 없다는 오류가 발생한다면, `git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build` 명령을 실행해 ruby-build를 설치하면 됩니다.
{: .notice--info}

적절한 버전을 찾은 다음, 아래와 같이 해당 버전의 루비를 설치하고 설정해주면 됩니다.  
3.1.2 버전을 예를 들어보겠습니다.

```sh
$ rbenv install 3.1.2
$ rbenv global 3.1.2   # 시스템 전체에 걸쳐 3.1.2 버전의 루비를 기본값으로 설정하낟.
# 또는:
$ rbenv local 3.1.2    # 현재 디렉터리에 대해 3.1.2 루비를 사용한다.
```

## 시스템 루비

문제는 해결했지만, 궁금한 점이 하나 남아있습니다.

**_"왜 루비 버전 관리자를 사용하지 않고 로컬에 설치되어있는 루비를 활용하려 했을 때, 그 버전을 바꾸지 못했을까?"_**

이를 위해선 시스템 루비에 대해 먼저 알아야 합니다.

:bulb: **시스템 루비란?**  
운영체제에 포함되어 운영체제 설치 시 설치되는 루비 또는 패키지 관리자를 통해 사용할 수 있는 루비를 말합니다.
{: .notice--info}

즉, 앞에서 말한 `로컬에 설치되어있는 루비`가 바로 시스템 루비인 것입니다.

만약 현재 활용중인 운영체제가 `Ubuntu 20.04 LTS` 버전이라면 이 운영체제를 설치한 머신에는 `2.7.0` 버전의 루비가 시스템 루비로 설치되어 있을 것입니다.

이 시스템 루비는 운영체제의 일부이므로 이를 사용자가 직접 활용하는 건 일반적으로 권장되지 않습니다.

### 시스템 루비를 사용하지 말아야 할 이유

---

**1. 시스템 루비는 사용자를 위한 것이 아니다.**

시스템 루비는 운영체제와 이에 의존하는 패키지들을 위한 것이므로, 문제 발생 시 정상적으로 동작하는 상태로 돌아가기 위해 환경 변수 등을 포함해 사용자가 만든 변경 사항들을 무시하고 이전 상태로 복구할 수 있습니다.

**2. 시스템 루비는 오래된 버전일 수 있다.**

앞에서도 말했듯, 깃헙 블로그를 위한 패키지들을 설치하던 머신의 운영체제는 `Ubuntu 20.04 LTS` 버전이었고, 이 운영체제의 시스템 루비는 `2.7.0` 버전이었습니다.

애초에 jekyll을 설치할 수 없다는 오류 메시지 또한 필요한 패키지를 설치하기엔 루비의 버전이 너무 오래 되었다는 내용이었죠.

또한 최신 버전의 루비는 이전 버전의 루비에서 발생하던 버그나 보안 이슈 등을 해결하고 새로운 기능 등을 추가한 버전이기 때문에, 최신 버전의 루비를 사용하는 것이 더 좋습니다.

**3. 시스템 루비는 사용자가 제거, 업그레이드, 다운그레이드가 불가능하다.**

시스템 루비는 운영체제에서 정해놓은 버전을 벗어날 수 없습니다.

앞의 과정에서 `로컬에 설치된 루비`를 업그레이드 할 수 없는 이유 또한 `Ubuntu 20.04 LTS`가 정한 시스템 루비의 버전이 2.7.0이기 때문입니다.

따라서, 루비 버전 관리자를 이용한 non-system ruby를 사용하는 걸 권장합니다.
