---
published: true
layout: post
tags: lifestreaming sns feed data-flow dqus
date: '2010-05-07 01:36:00 +0900'
comment: false
comments: false
---
- sns.jsiku.com(폐쇄)에서 옮김

http://www.steverubel.com/a-lifestreaming-workflow몇 일전  누군가의 Facebook 링크를 통해서 본 글이 있습니다. 모범이 될만한 life-streaming data flow 5개를 보여준 글인데 [링크](http://lifestreamblog.com/5-good-examples-for-managing-your-lifestreaming-data-flow/), 너무나도 훌륭했습니다. 이러한 도식들로 데이타의 흐름을 개념적으로 정리하고 실제 여러 SNS에 대하여 설정한다면, 최소의 노력으로 여러 SNS를 사용할 수 있게 됩니다. 그럼 더 많은 사람들하고 여러 좋은 정보들을 공유할 수 있겠죠.

그래서 나도 이와 같이 정리해야겠다는 결심해서 다음과 그리게 되었습니다.
![siku's lifestreaming](https://images-blogger-opensocial.googleusercontent.com/gadgets/proxy?url=http%3A%2F%2Ffarm5.static.flickr.com%2F4020%2F4584878418_35c9a06100_o.jpg&container=blogger&gadget=a&rewriteMime=image%2F*)
11가지의 SNS 서비스(블로그 제외)와 몇가지 툴간의 데이터 흐름을 표시하였습니다. 크게 3단계(생산, 종합, 공유, *위에 제시된 Steve Rubel의 흐름도를 참고하였습니다.)로 설정하였고, 각각의 SNS들을 그 용도에 맞게 배치하였습니다. 그럼 자세히 알아보도록 하죠.

- Write and Capture: 컨텐츠를 생산한다는 의미에서 Write가 主가 되어야 하지만, 가진 꺼리가 너무 적고 글 솜씨도 그리 좋지 못하기 때문에 몇몇 좋은 글들에 대한 감상 혹은 즐겨찾기 등이 주가 되어 Capture라고도 썼습니다. [Delicious](http://delicious.com/)와 [Google reader](http://reader.google.com/)도 SNS로서의 기능을 갖추고 있지만, 이를 통해서 사람들과 연결되는 경우 적기 때문에 컨텐츠를 생산하는 도구로서 분류를 하였습니다.

- Aggregate: 본인이 둘러보는 수많은 웹자료와 이에 대한 리액션 그리고, 글들을 1회성으로 놓치는 것이 아까워서 이를 한 곳으로 모을 생각으로 [Friendfeed](http://friendfeed.com/)를 사용하였습니다. 이 서비스 역시 사람들과 연결되는 경우가 적어 어느정도의 컨텐츠를 보여줄지에 대해 전혀 신경을 쓰지하고 가능한 모든 자료들을 모으는 데에 초점을 두었습니다. 단 사진 및 동영상에 대해서는 공적으로 유용성이 떨어지고, 중복된 자료로 등록될 수 있기 때문에 Friend feed에 모으지 않았습니다

- Share and Connect: 사람들이 주로 많이 들르는 SNS를 대상으로 삼았습니다. 국내에서 사용자가 급증하는 [Facebook](http://facebook.com/), [Twitter](http://twitter.com/) 그리고 최근 Google에서 서비스하는 [Buzz](https://mail.google.com/mail/?shva=1#buzz)를 대상으로 삼았으며, 추가된 친구가 많지는 않지만 사진 및 동영상에 특화된 [Picasa](http://picasaweb.google.co.kr/home), [Flickr](http://flickr.com/), [Youtube](http://youtube.com/)도 최종적으로 사람들에게 보여지는 것들로 분류하였고, 인터페이스가 깔끔한 [Meme](http://meme.yahoo.com/)도 이러한 분류에 포함시켰습니다. Facebook 같은 경우는 다른 서비스에 비해 친구, 가족, 동료등의 사용비율이 높기 때문에 그들에게 사진 및 동영상에 관계된 서비스를 직접 연결하였습니다. (위에서 Friendfeed에 이러한 자료를 모으지 않는다고 하였는데, Facebook 담벼락을 통해 결국 Friendfeed로 흘러들어가군요.) 사실 제가 적은 3단계는 완벽하게 순서적으로 진행되는 것이 아닙니다. Aggregate와 Share and Connect가 서로 feedback을 주고 받는 형태이지요. 그래서 양쪽 화살표로서 표시하였습니다.

흐름도를 작성하면서 좋았던 점은 여러 서비스를 재정리할 수 있다는 것, 자료의 중복을 피하게 된 것(잘 못 연결하면 거의 스팸수준으로 다수의 중복된 feed들이 기록됩니다. 본인도 짜증나고, 이를 보는 follower들도 짜증이 날 것입니다.)입니다.
여려웠던 점은 수많은 서비스에 대하여 이해해야 한다는 것입니다. 단일의 서비스라면 별 어려움이 없겠지만, 다수의 서비스들이 효율적으로 맞물리게 하려면 사용하는 모든 서비스들 중 몇가지는 포기해야 할 것입니다. ([Ping.fm](http://ping.fm/)의 경우 feed를 모으는 것 뿐만 아니라, 뿌리는 데 특화된 서비스이지만, 자료의 중복때문에 제외시켰고, [Linkedin](http://linkedin.com/) 같은 경우는 사용하고 싶지만, 이를 어떠한 목적으로 사용할 지 아직 정리하지 않았기 때문에 제외시켰습니다.)
마지막으로 실제 어떻게 돌아가는지 확인하기에 많은 시간이 걸립니다. 서비스간에 feed를 즉각적으로 주고받는 경우도 있지만, 이를 확인하는 데에 여러 시간이 걸리는 경우도 있습니다. 그리고 에러가 날 수도 있습니다. 제 페이스북 같은 경우는 로그아웃 상태에서 찾아볼 수 없습니다. 위의 흐름도에서는 연결시킨 것으로 나오지만 Friendfeed와 Facebook이 서로 연결된 상태는 아닙니다. (이를 고치기 위해 Facebook에 요청을 했는데, 빨리 이루어졌으면 좋겠군요. Ping.fm에 대해서도 Delicious에 대해 완전히 대응하지 못하고 있고요.)
이렇게 정리하면서 현재 스마트폰 시장과 웹서비스 시장의 형태가 비슷함을 느낄 수 있었습니다. 몇몇의 공급자들에 의해 어느정도 성격이 정해진 Local시장들이 보다 큰 Global시장과 다른 특성을 보이게 되고, 기존의 수익을 무시할 수 없기 때문에 수익구조를 바꾸지 못하다가(뭐 나름 열심히 노력은 하고 있죠.) 결국 소비자들의 요구와 개방할 수 밖에 없는 시기가 때문에 지켜왔던 위상이 무너지는 것이 요세 종종 보이죠. 피쳐폰에서는 2,3위를 달리다가 스마트폰에서는 맥을 못추는 국내기업과 국내 대형사이트안에서만 도는 엄청난 트래픽을 포기할 수 없기 때문에, Facebook, Twitter와 같은 서비스를 개시하기 힘든 몇몇 포탈업체들을 보면, 가진 것이 없기에 새로운 시장환경에 도전할 수 있는 중소기업이 얼마나 중요한지를 알 수 있습니다. 음. 실리콘벨리를 잘알고 그들의 문화에 완전히 편입된 기업을 만들던가, 한국어를 사용하고 한국문화에 지배되는 사람들이 10억 이상 정도된다면 국내기업들도 크게 활약할 수 있을텐데요. 어려운 일이네요.
