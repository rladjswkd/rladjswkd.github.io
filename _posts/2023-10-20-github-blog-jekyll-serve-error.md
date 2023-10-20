---
title: "[Github Blog] bundler: failed to load command: jekyll 오류 해결"
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

깃헙 블로그 포스트를 작성하고 로컬에서 작성 결과를 미리 확인해보기 위해 `bundle exec jekyll serve` 명령을 입력했는데, 아래와 같은 오류가 발생했습니다.

```sh
...
bundler: failed to load command: jekyll (/.../gems/bin/jekyll)
/.../gems/gems/jekyll-3.9.3/lib/jekyll/commands/serve/servlet.rb:3:in `require': cannot load such file -- webrick (LoadError)
...

```

## 해결 방법

이는 버전이 3.0 이상인 ruby에서 webrick gem을 기본으로 제공하지 않기 때문에 발생하는 문제입니다.

따라서 `bundle add webrick`으로 webrick을 Gemfile에 추가해 직접 설치해야 합니다.

## webrick

`bundle exec jekyll serve`는 로컬에서 웹 서버를 돌려 작성한 페이지들을 확인하기 위한 명령어입니다.

이때 사용하는 작은 웹 서버가 webrick입니다.

## 참고 링크

- [ruby 3.0 릴리즈 노트](https://www.ruby-lang.org/en/news/2020/12/25/ruby-3-0-0-released/){:target="\_blank"}
- [jekyll 이슈 #8523](https://github.com/jekyll/jekyll/issues/8523){:target="\_blank"}
- [webrick](https://github.com/ruby/webrick){:target="\_blank"}
