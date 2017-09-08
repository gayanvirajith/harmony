---
published: true
title: facebook connect 설치(워프 가지신 분은 그냥 디스커스 쓰시기를..)
layout: post
date: '2009-05-04 10:51:00 +0900'
tags: disqus dqus facebook connect wordpress
author: siku
---
** 17년 지금 보면..이러한 것들이 많이 의미가 없어졌습니다.

facebook의 인터페이스가 익숙하지 못해서 못찾는 것인가? 혹은 원래 그런 기능이 없는지...

* <http://wiki.developers.facebook.com/index.php/Trying_Out_Facebook_Connect> ( fb co~에 대한 위키)

* <http://www.facebook.com/video/video.php?v=630563174283> (  fb co~동영상)

* <http://www.chris-wallace.com/2009/01/07/five-easy-steps-to-integrate-facebook-connect-with-your-blog/> (워프 플러긴 설치하는곳

* <http://business2press.com/2009/02/20/how-to-install-new-facebook-connect-comments-to-wordpress/>

위의 사이트를 통해서 facebook connect 을 달게 되었습니다.

이를 달고 느낀 문제중 하나고

디스커스와 인텐스디베이트 처럼 다른 커맨트를 통합으로 처리하지 못한다는 것입니다.(제가 못찾은 건지. 찾으신 분을 알려주세요)
facebook connect을 달고 느낀 장점은 커멘트를 적을 때 로그인을 할 수 있다는 것... 오픈아이디가 더 활용성이 클 것 같지만 facebook의 저력이 워낙 대단하다보니.. 사용자 입장에서 더 활용성이 클 수 있을 것 같습니다. 답변을 단사람의 자세한 프로필은 못보더라도 약간은 볼 수 있으니..

 facebook connect은 크게 두가지로 활용할 수 있는 것 같습니다.

답글용(너무 간단한가)
블로그에 접속한 사람의 정보 및 커뮤니티(여기에 대한 글은 생각보다 적더군요. http://staynalive.com/이를 보시면 사이드바에 facebook connect가 설치된 것을  볼 수 있습니다.
여하튼 많은 삽질이 있었지만 facebook connect을 활용하기 위하연 여러방법 중 공통적 단계는 에플리케이션의 등록입니다.  이 단계는 다음과 같습니다.

페이스부가기능 만들기: 여기서 새로운 부가기능의 이름을 입력하고(동의를 누룹니다.)
기본정보 탭을 보면( api와 비밀번호가 형성된 것을 볼 수 있습니다.) 여기서 callback url을 입력합니다. 저 같은 경우는 이 블로그의 주소 http://siku.name를 입력하였습니다.
여기가 공통적인 단계고 커멘트로서 활용할 수 있는 방법은 크게 2가지 입니다.

코드를 직접 만지기
제공된 플러그인을 활용하기
여기까지 쓰니까 갑지가 쓰기가 싫어지군요. 페이스북에서 제공되는 방법으로 코드를 직접 만져서 코멘트 폼에 페이스북 로그인 버튼을 설치하는 것은 http://www.facebook.com/video/video.php?v=630563174283에서 확인할 수가 있습니다. 그리고 페이스북에서 제공한 위젯형식의 커맨트박스(구글 프렌드 커넥트와 비슷함.)는 http://business2press.com/2009/02/20/how-to-install-new-facebook-connect-comments-to-wordpress/서 확인할 수 있고요..

이렇게 써보니까 별로 도움이 안되네요. 이제 결론인데. 결론적으로 플러그인을 쓰는게 더 쉽습니다. 방금 텍스트큐브에서 실행해봤는데.. 포기하였고, 다른 뛰어난 분이 개발해주었으면 좋겠네요.  워프 플러그인 홈페이지에서 wp-facebookconnect은 굉장히 쉽게 설치할 수 있습니다. 이를 설치하고 기존의 플러그인과 마찬가지로 activate 만 하면 되고, theme의 코멘트와 관련되 파일(예: comments.php) 에 다음과 같은 코드를 원하는 위치에 삽입하면 됩니다.

```
<?php do_action('fbc_display_login_button') ?> ```
이 때 주의 할점으로 파일 안에 다음과 같은 코드가 꼭 있어야 합니다.

```
<?php if ( $user_ID ) : ?> ```
저는 아무리 설치해도 되지를 않아서 꽤 고생했는데, Intensedebate, disqus, typepad와 같이 커맨트를 따로 끌어다 쓰는 경우는 죽었다 깨어나도 볼 수 파란색의 페이스북 버튼을 볼 수 없습니다.


위와 같이 생겼죠.. 여차저차 시간을 많이 낭비하였는데, 이 wp-facebookconnect의 이점은 단순히 커맨트 뿐만 아니라 다른 데에도 이점이 있습니다. 바로 facebook 아이디로 블로그에 로근인을 할 수 있죠. 공동 블로그로서 저자들을 끌어모을 때, 새로 아이디를 만든 다는 것이 상당한 부담으로 다가 오는데, 이런 오픈아이디와 같은 기능은 큰 장점이라 할 수 있죠(다른 사람들은 성공하던데, 저는 아무리 해도 워프에서 오픈아이디를 돌리지 못하겠습니다. 하실 줄 아는 분은 좀 도와주시기를...)

다른 플러그인으로는 디스커스에서 제공하는 플러긴으로 http://wordpress.org/extend/plugins/disqus-comment-system/서 다운 받을 수 있는데, 진작에 이것을 다운받으면 될 것을 괞히 시간만 버린 것 같은 느낌이 듭니다. 제 것과 같은 코멘트 폼으로서 본래 워프, 디스커스, 페이스북 모든 계정으로 글을 쓸 수 있고, 저는 못 찾았지만, 페이스북에서 찾을 수 없던 코멘트 종합관리?? 를 디스커스에서 할 수 있으니까 좋죠. 게다가 오픈아이디, 구글, 야후, msn 등 한국인들이 쓸만한 계정으로 왠만큼 다 로그인이 됩니다. 예전에 다음사인이 폐쇠된것이 안타깝네요. 다음은 텍스트큐브에서 다는 방법을 연구좀 하고, 위젯으로 어떻게 다는지도 연구해봐야 겠습니다.
