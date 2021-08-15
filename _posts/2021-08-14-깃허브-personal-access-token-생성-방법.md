---
layout: post
title:  "깃허브 personal access token 생성 및 에러 해결 방법"
date:   2021-08-14
excerpt: ""

tag:
- markdown 
- syntax
- sample
- test
- jekyll
---
{% include toc.html %}


## push 할 때 에러가 났다
 어제까지만 해도 잘 되던 git push가 갑자기 remote 에러와 fatal 에러를 띄웠다. 내가 받은 에러는 다음의 두 가지
~~~
remote: Support for password authentication was removed on August 13, 2021. Please use a personal access token instead.
remote: Please see https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/ for more information.
fatal: unable to access 'https://github.com/healthykim/healthykim.github.io.git/': The requested URL returned error: 403
~~~

remote 에러는 2021년 8월 13일부터 비밀번호 인증 방식을 없앴기 때문에, personal access token(PAT)을 사용하라는 것이고, 그것을 어떻게 발급받는지에 대한 정보를 제시된 Url에서 받으라는 것이다. 

fatal 에러는 내 깃에 접근 실패했다는 것인데, 이것은 내가 깃허브 패스워드를 로컬에 저장해 두고 레포에 접근할 때마다 자동 로그인하게 해 두었기 때문이다. 토큰을 물어봤는데 패스워드를 넘겨서 인증에 실패하니까 이 에러를 준 듯

  

## remote 에러 해결 : personal access token(PAT) 생성
이건 쉽다. 알려주는 사이트로 들어가 보면 PAT생성법을 알려주는 곳으로 이동할 수 있다. 아래 사이트로 이동.

[https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token)

대략
  
> 세팅 $$\rightarrow$$ 개발자 세팅 $$\rightarrow$$ personal access token $$\rightarrow$$ generate new token $$\rightarrow$$ 유효기간, 범위 설정(난 모두 최대로 설정했다) $$\rightarrow$$ generate token

의 순서를 따르면 된다고 한다. <markPink>그리고 마지막에 주는 토큰 번호는 다시 볼 수 없으니 반드시 다른 곳에 저장해야 한다.</markPink>


  

## fatal 에러 해결 : keyChain 정보수정
### 첫 번째 시도(실패) : git config --global 변경
  비밀번호를 바꿨을 때와 동일하게 global config에 저장된 정보를 없애고, 자동 로그인을 하지 않는 방식(접근할 때마다 password입력하는 방식)으로 바꾼 뒤 다시 비밀번호를 입력 및 저장하면 된다고 생각했다. 그런데 왜인지 실패했다.
  git config --global credential.helper osxkeychain과 git config --global --unset user.password 을 모두 시도했지만 모두 실패해서 계속 자동 로그인되었다.
  
### 두 번째 시도 : keychain에서 직접 변경
 맥북 [기타] > [키체인 설정] 에서 github에 해당하는 암호를 직접 변경했다. (기존에 저장되어있던 암호 $$\rightarrow$$ 토큰 번호)
 <figure>
     <a href="../../assets/keychainimage.png"><img src="../../assets/keychainimage.png"></a>
 </figure>
 
 
 
 사진에도 보이듯이 저장된 암호의 종류가 두가지 있었는데(인터넷 암호, 웹 양식 암호), 아래 것만 변경했을 때는 에러가 해결되지 않았고, 위의 것만 변경했을 때는 에러가 해결되었다. 이유를 모르겠다. 뭐 그럴 수도 있지. 아무튼 간에 이런 식으로 토큰을 발급받고 자동 로그인을 유지하게 되었다. 끝
