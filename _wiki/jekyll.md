---
published: false
---
# Jekyll

## Ruby, RubyGems, Bundler, Jekyll 관계
* Ruby = Javascript
* RubyGems(gem) = npm
* Bundler = 어플리케이션 의존성 관리 패키지
* Jekyll = Static 사이트 생성 패키지
* Gemfile = package.json
* Gemfile.lock = package.json.lock

## 사이트 빌드 방법
```shell
$jekyll build
```
사이트 생성시 build 옵션을 주지 않으면 현재 디렉토리에 있는 마크다운이나 HTML 파일들을 하나 또는 여러 겹의 레이아웃으로 포장해서 ./_site 디렉토리에 그 결과(HTML 파일)를 저장한다.

[빌드 명령어 옵션](https://jekyllrb-ko.github.io/docs/quickstart/#bundler-%EC%97%90-%EA%B4%80%ED%95%98%EC%97%AC)

참고로 Jekyll의 모든 명령어들(예를 들어 build, serve)의 옵션들은  뒤에서 설명할 Jekyll 환경 설정 파일인 **_config.yml** 파일에서 지정해 줄 수 있다.

### 주의사항
사이트 빌드시 ./_site 디렉토리에 있는 파일들은 모두 삭제되기 때문에 이미지나 다른 리소스 파일들을 ./_site 디렉토리에 저장하지 말아야 한다.

## 로컬에서 사이트 미리보기
Jekyll은 개발 서버를 내장하고 있어서 사이트 배포 전에 브라우저로 사이트 모습을 확인할 수 있다.
```shell
# 변경사항 발생시 자동으로 재빌드
$jekyll serve

# 변경사항 발생시 자동으로 브라우저 리프래쉬
$jekyll serve --livereload
```

[serve 명령어 옵션](https://jekyllrb-ko.github.io/docs/quickstart/#bundler-%EC%97%90-%EA%B4%80%ED%95%98%EC%97%AC)

### 주의사항
Jekyll의 주 환경설정 파일인 **_config.yml**에서 변경사항이 생기면 재빌드되지 않는다. 따라서 이 환경설정 파일을 수정했을 경우에는 미리보기를 다시 실행해주어야 한다.

### 페이지 접속이 안될 시 문제 해결
로컬 서버를 켜고 브라우저에서 특정 페이지를 접속 해보았는데 만약 접속이 안되면 다음과 같은 방법으로 원인을 찾을 수 있다. 먼저 _site 디렉토리에 들어가서 보면 **아마도** 그 페이지가 없을 확률이 높다. 이런 경우 jekyll build --verbose 명령어를 통해 출력 메시지를 확인해보면 왜 페이지가 안만들어졌는지 그 원인을 찾을 수 있다. 만약 Jekyll이 그 특정 페이지를 빌드 조차 안한다면 _config.yml 파일에서 빌드에 포함되도록 설정해준다.

## Jekyll 사이트 주요 파일 및 디렉토리 구조 설명
[파일/디렉토리 구조](https://jekyllrb-ko.github.io/docs/structure/)

## HTTP 응답 헤더 수정하는 방법
[WEBrick 커스텀 헤더](https://jekyllrb-ko.github.io/docs/configuration/#webrick-%EC%BB%A4%EC%8A%A4%ED%85%80-%ED%97%A4%EB%8D%94)

## 개발/배포 환경 설정
[빌드 시점에 Jekyll 환경변수 지정하기](https://jekyllrb-ko.github.io/docs/configuration/#specifying-a-jekyll-environment-at-build-time)

## 블로그 포스트 예제
### 블로그 포스트 작성
[포스트 폴더](https://jekyllrb-ko.github.io/docs/posts/#%ED%8F%AC%EC%8A%A4%ED%8A%B8-%ED%8F%B4%EB%8D%94)
_posts 디렉토리에 블로그 포스트를 작성한다. 이 때 포스트 파일명은 아래 형식을 반드시 따라야만 한다.
```
YEAR-MONTH-DAY-title.MARKUP
```
그리고 모든 포스트는 반드시 뒤에서 설명할 YAML 머리말을 포함해야 한다.

_posts/'2014-04-10-my first blog.md'
```
---
layout: default
---
# My first blog
```
### 블로그 포스트 목록 페이지 작성
[포스트 목록 표시하기](https://jekyllrb-ko.github.io/docs/posts/#%ED%8F%AC%EC%8A%A4%ED%8A%B8-%EB%AA%A9%EB%A1%9D-%ED%91%9C%EC%8B%9C%ED%95%98%EA%B8%B0)
위와 같은 상태로 빌드를 하면 _site 디렉토리 안에 2014 디렉토리가 만들어지고, 2014 디렉토리 안에 04 디렉토리가 만들어지고 04 디렉토리 안에 10 디렉토리가 만들어지고 10 디렉토리 안에 my first blog.md 파일이 존재하게 된다. 즉, 이 포스트에 접근하기 위한 URL 경로는 /2014/04/10/my-first-blog.html가 된다.
이 상태로는 블로그에 방문한 사람이 포스트에 접근하기 어렵기 때문에 포스트 목록을 보여주는 페이지를 만들어준다. 나 같은 경우 index.html에 포스트 목록을 보여주도록 하였다.

index.html
```
---
layout: default
---
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
```
빌드를 하고 로컬 서버를 실행한 후 브라우저로 사이트에 접속하면 포스트 목록이 보여지고 포스트 목록을 클릭하면 해당 포스트로 이동하는 것을 볼 수 있다.

### 카테고리 목록 페이지 작성
[포스트의 카테고리와 태그 표시하기](https://jekyllrb-ko.github.io/docs/posts/#%ED%8F%AC%EC%8A%A4%ED%8A%B8%EC%9D%98-%EC%B9%B4%ED%85%8C%EA%B3%A0%EB%A6%AC%EC%99%80-%ED%83%9C%EA%B7%B8-%ED%91%9C%EC%8B%9C%ED%95%98%EA%B8%B0)
포스트를 카테고리로 나누어서 블로그 방문자에게 제공하고 싶으면 다음과 같이 하면 된다.
먼저, _layouts 디렉토리에 category.html 파일을 작성한다.
_layouts/category.html
```
---
layout: default
---

{% for post in site.categories[page.category] %}
    <a href="{{ post.url | absolute_url }}">
      {{ post.title }}
    </a>
{% endfor %}
```

다음으로 프로젝트 루트 디렉토리에 category 디렉토리를 만들고 카테고리별로 파일을 만든다. 예를 들어 dev 라는 카테고리를 만들고 싶으면 dev.html 파일을 다음과 같이 작성한다.
category/dev.html
```
---
layout: category
title: Development
category: dev
---
```
이렇게 하면 /category/dev.html로 dev 카테고리 페이지에 접근할 수 있다. 위 코드에서 title 값은 브라우저 탭에 보여질 타이틀을 의미한다.

마지막으로 포스트를 작성할 때 YAML 머리말에 categories 변수 값을 dev 로 설정해주면 dev 카테고리 목록에 해당 포스트가 추가된다.
_posts/2014-04-10-my-first-blog.md
```
---
layout: default
categories: [ dev ]
---
# My first blog
```
이 포스트는 이제 /dev/2014/04/10/my-first-blog.html 로 접근할 수 있다.

#### 카테고리 계층화 하기
예를 들어 어떤 포스트가 dev 라는 카테고리 안에 nodejs 카테고리에 속하도록 하고 싶다면 포스트의 categories 변수 값을 다음과 같이 설정해 주면 된다.
_posts/2014-04-10-my-first-blog.md
```
---
layout: default
categories: [ dev, nodejs ]
---
# My first blog
```
그러면 이 포스트는 /dev/nodejs/2014/04/10/my-first-blog.html로 접근할 수 있다. 그리고 이 포스트는 dev 카테고리에 여전히 속하기 때문에 dev 카테고리 목록에 나타나게 된다. 만약 nodejs 카테고리를 만들면 이 포스트는 dev 카테고리 목록에도 나타나고, nodejs 카테고리에도 나타난다.
category/nodejs.html
```
---
layout: category
title: Node.js
category: nodejs
---
```

### 태그 목록 페이지 작성
태그 목록을 만드는 방법은 카테고리 목록을 만드는 것과 매우 유사하기 때문에 간단히 설명하자면 포스트 태그를 붙이는 방법은 포스트의 YAML 머리말에 tags 변수 값을 할당해주면 된다.
_posts/2014-04-10-my-first-blog.md
```
---
layout: default
tags: [ blog, nodejs ]
---
# My first blog
```
카테고리와 다른 점은 카테고리의 경우 카테고리 계층 구조를 따라 URL 패스가 만들어지지만, 태그의 경우 카테고리를 지정안했을 때와 같이 포스트 파일명을 파싱해서 URL 패스를 만든다. 

### 포스트 발췌
[포스트 발췌](https://jekyllrb-ko.github.io/docs/posts/#%ED%8F%AC%EC%8A%A4%ED%8A%B8-%EB%B0%9C%EC%B7%8C)

## 페이지 예제
### 페이지 작성
[홈페이지](https://jekyllrb-ko.github.io/docs/posts/#%ED%8F%AC%EC%8A%A4%ED%8A%B8-%EB%B0%9C%EC%B7%8C)
페이지와 블로그 포스트의 차이점은 페이지는 날짜와 무관하다. 따라서 페이지 파일명에 블로그 포스트처럼 날짜를 적어주지 않아도 된다. 페이지의 예로는 about.html, contact.html 등이 있다. 
페이지 작성 위치는 두 가지가 가능하다. 첫번째는 프로젝트 루트에 html 또는 md 파일을 작성하는 것이고 두번째는 페이지가 많을 경우 원하는 디렉토리를 만들어서 그 디렉토리에 페이지를 작성하는 것이다.
페이지 작성 위치에 따라 페이지의 URL은 달라진다.
만약 페이지를 어떤 디렉토리 안에서 작성했지만 페이지 경로를 다르게 하고 싶으면 페이지의 YAML 머리말의 permalink 변수값을 원하는 경로로 할당해주면 된다.

## 환경 설정 방법 1. _config.yml
프로젝트의 루트 디렉토리에 있는 **_config.yml** 파일에 **defaults** 라는 키를 사용하여 사이트에 전체에 사용되는 변수들의 기본값을 정의할 수 있다.
여기서 설정한 환경 변수는 마크다운 또는 HTML 파일에서 사용할 수 있다.
```
#_config.yml
defaults:
	-
		scope:
			path: ""
			type: "wiki"
		values:
			title: "제목 테스트"

#_wiki/test.md
...
{{ page.title }}
```

## 환경 설정 방법 2. YAML 머리말
[머리말](https://jekyllrb-ko.github.io/docs/frontmatter/)
Jekyll 은 YAML 머리말 블록을 가진 모든 파일을 특별한 파일로 인식하여 처리한다. 머리말은 반드시 파일의 맨 첫 부분에 위치해야 하고 시작과 끝을 대시문자 세 개로 감싸서 올바른 YAML 형식으로 작성해야 한다. 가장 기본적인 형태를 예로 들면 다음과 같다.
```
---
layout: post
title: Blogging Like a Hacker
---
```
실험 결과 마크다운 파일에 YAML 머리말이 없으면 Jekyll이 이 마크다운 파일을 HTML로 변환하지 않고 그냥 파일 그대로 ./_site/ 디렉토리에 복사한다. 그 결과 브라우저에서는 파일을 찾을 수 없다고 나온다. 이것을 방지하기 위해서는 반드시 마크다운 파일에 최소한 아래와 같이 파일의 맨 첫 부분에 YAML 머릿말 블럭을 써줘야 한다.
```
---
---
```
파일을 비공개로 하고 싶으면 published 변수를 false로 설정한다. 그러면 이 파일은 빌드시 제외하게 된다.
```
---
published: false
---
```
만약 개발 서버에서만 이 파일의 미리보기 모습을 보고 싶으면 다음과 같이 serve 명령어 옵션으로 --unpublished 옵션을 주면 개발 서버에서만 이 파일에 대한 접근이 가능해진다.
```shell
$jekyll serve --unpublished
```


