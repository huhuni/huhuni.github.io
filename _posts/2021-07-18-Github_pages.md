---
title: Github pages 10분만에 만들기
categories: [Github, pages]
tags: [git, pages]     # TAG names should always be lowercase
---
> 해당 글은 Github가 무엇인지 안다는 가정하에 설명하고 있습니다.  
> 만약 해당 내용을 모른다면 다른 자료를 통해 기본적인 정보만이라도 알고 보시길 바랍니다.  

<br/>


> **필요 준비물**  
> Github 계정, 윈도우 PC


# Github pages?
<br/>
Github pages는 Github 저장소에 저장된 내용을 웹페이지로 만들어주는 서비스입니다.
<br/>
<br/>

즉, 별도의 서버가 없어도 자신만의 웹페이지를 무료로 구축할 수 있다는 것입니다.
<br/>
<br/>
일반적으로 Github pages는 다음과 같은 URL 형태로 이루어져있습니다.
<br/>
<br/>

```markdown
https://username.github.io
```
물론 이것과 다른 형태의 URL도 존재할 수 있다는 점 알아두세요!  
<br/>
<br/>

# Github pages 구축하기
## 1. Github 계정 생성 
앞서 필요 준비물에 github 계정이 있었습니다.  

혹시 만들지 않으신 분들은 만드는 것이 어렵지 않으므로 [Github](https://github.com)로 접속해서 가입해주세요!
<br/>
<br/>

## 2. Pages 관련 repository 만들기
<br/>
아래의 그림과 같이 Github에 가입 후 접속하면 repositories를 생성하는 버튼을 누릅니다.

<br/>
<br/>

![github_page_repo](/assets/img/favicons/github_page_repo1.png)

이 후 아래와 같은 창이 뜨게 되는데 repository name에 ***자신의 github 아이디 + .github.io***를 적으면 끝이납니다.  

![github_page_name](/assets/img/favicons/github_page_repo2.png)

저의 경우에는 아이디가 huhuni이기 때문에 huhuni.github.io를 넣었습니다.

나머지 설정은 화면과 같이 해주셔도 되고 아니면 자신의 취향에 맞게 변경해주셔도 됩니다.

이렇게 해주시고 `Create repository`를 눌러주시면 자신만의 빈 블로그가 완성되게 됩니다.
<br/>
<br/>

## 3. Jekyll 테마 이용하기

github pages에는 다양한 테마를 제공하는 사이트가 존재합니다.

대표적으로 제 홈페이지와 같은 테마를 제공해주는 곳이 `Jekyll 테마`입니다.

- [Jekyll theme](https://jekyllthemes.org/)

이 곳에서 제가 사용한 템플릿은 `chirpy`이기 때문에 본 블로그의 설명도 이것을 기반으로 합니다.

jekyll 사이트에서 chirpy를 찾아 github로 접속합니다.

이 후 우측 위 `Code` 버튼을 눌러 Download ZIP을 이용하여 다운 받아주세요.

MAC에서는 해당 방법이 제대로 동작하지 않아 저는 window pc를 이용하여 만들었습니다.

다운로드 된 압축파일을 풀어주시고 자신의 repository에 직접 업로드해줍니다.

참고로 업로드 파일이 100개가 넘으면 올라가지 않으므로 적당히 나누어서 업로드 해줍니다.

이렇게 업로드가 완료되면 자신의 블로그 기본 틀이 완성된 것입니다.

```
https://username.github.io
```
에 접속해보시면 자신의 블로그가 완성된 것을 확인할 수 있습니다.

이 후 자신의 포스트를 업데이트 하거나 자신의 마음대로 바꾸기 위해서는 `chirpy`에 접속하여 wiki를 참조해주세요!

