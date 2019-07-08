---
title: robots.txt는 뭘까?
description: 크롤링을 통해 프로그램 개발 시 꼭 확인해야하는 robots.txt에 대해 알아봅니다.
categories:
 - TIL
tags: [robots.txt, 크롤링 불법]
---

크롤링을 배우게 되면 여기 저기 사이트를 크롤링해서 데이터를 가져온 후 유용한 프로그램을 만들어 보고 싶을 것이다. 나도 크롤링을 배운 후 어디다 써보고 싶어서 이런 저런 아이디어를 생각해냈다. 그러다 문득 크롤링을 하는 것은 그 사이트의 데이터를 허가 없이 가져다 사용하는 것인데.. 크롤링을 통해서 수익을 내지 않더라도 웹 사이트나 어플을 런칭해도 될까?라는 생각이 들었다. 그래서 인터넷을 뒤져보기 시작했는데 지나가다 많이 보던 `robots.txt` 의 정체를 이제서야 알게되었다.

## robots.txt ? 

robots.txt 파일은 서버  root 디렉토리에 위치하는 파일이다. 이 파일을 통해 해당 사이트가 크롤링을 허용하는지 알 수 있다. google 의 robots.txt 파일을 보자. 

[http://google.com/robots.txt](http://google.com/robots.txt)

```
User-agent: *
Disallow: /search
Allow: /search/about
Allow: /search/static
Allow: /search/howsearchworks
Disallow: /sdch
Disallow: /groups
Disallow: /index.html?
Disallow: /?
Allow: /?hl=
...
```

user-agent : * 는 모든 유저에 대한 크롤링 권한을 뜻한다. Disallow에 해당하는 페이지는 크롤링을 허용하지 않는다는 것이고, Allow는 허용한다는 것이다. 구글은 검색 페이지에 대한 크롤링은 허용하지 않는 것 같다. 네이버도 알아보자

[https://www.naver.com/robots.txt](https://www.naver.com/robots.txt)

```
User-agent: *
Disallow: /
Allow : /$ 
```

네이버는 모든 페이지에 대한 크롤링을 허용하지 않는다. 

이렇게 크롤링을 할 때 해당 사이트의 robots.txt을 통해 크롤링 허용 여부를 확인하고 프로그램을 개발하는 것이 좋을 것 같다!  또한 자신의 사이트를 만들 때에도 robots.txt 파일을 생성해 둠으로써 크롤링 허용에 대한 정의를 해두는 것이 좋을 것 같다. 