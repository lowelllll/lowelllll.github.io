---
title: Jeykll Systax highlight 적용하기
description: 지킬 블로그에 코드를 이쁘게 올릴 수 있습니다.
categories:
 - Jeykll
tags: [jeykll highlighting, 지킬 하이라이트, jeykll syntax highlighting]

---

 Jeykll 블로그에서 코드를 올릴 때 syntax highlighting을 해준다면 독자가 글을 읽을 때 직관적이고 편하게 읽을 수 있을 것이다. syntax highlighting을 적용하는 것은  `rouge`를 통해 쉽게 할 수 있다.

### 1.  gem을 통한 rouge 설치

```bash
$ gem install kramdown rouge
```

코드 하이라이팅을 위한 rouge를 설치해준다. kramdown은 지킬 마크다운 엔진이다. 

github.io는 기본으로 kramdown 엔진을 사용한다.

### 2. _config.yml 설정

```yml
# _config.yml

...생략

markdown: kramdown
highlighter: rouge
```

### 3.  테마 다운

```bash
$ rougify help style
...
available themes:
  base16, base16.dark, base16.light, base16.monokai, base16.monokai.dark, base16.monokai.light, base16.solarized, base16.solarized.dark, base16.solarized.light, colorful, github, gruvbox, gruvbox.dark, gruvbox.light, igorpro, molokai, monokai, monokai.sublime, pastie, thankful_eyes, tulip
```

위 명령어로 지원하는 테마를 확인 한 후 맘에 드는 테마를 다운받으면 된다.

```bash
$ rougify style monokai > /assets/css/syntax.css
```

### 4. 적용

 Html head 부분에 다운 받은 css 파일을 적용해준다.

```html
...
  <link rel="stylesheet" href="{{ "/assets/css/syntax.css" | prepend: site.baseurl }}"">

```

href 안의 링크는 다운 받은 css 파일의 경로를 잘 설정해줘야한다. 



그럼 끗 ! 💁‍♀️