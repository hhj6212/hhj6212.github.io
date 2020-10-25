---
layout: post
title: 깃블로그에 댓글창 달기
comments: true
categories: [blog,tech,programming]
---

### 깃허브 블로그에 댓글창을 달아보자
안녕하세요. 한헌종입니다.

이번엔 Jekyll 로 구축된 깃허브 블로그에 댓글창을 넣는 방법을 설명해 보겠습니다.
블로그에는 당연하게 댓글창이 달려있을 거라 생각하실 겁니다.
그러나 깃허브 블로그는 그렇지 않습니다...
오로지 글만 올리는 용도의 블로그로만 써야하나? 하는 생각이 들었습니다.

| ![-]({{"/assets/200820/image1.png"| relative_url}}) | 
|:--:| 
| *소통 없는 삭막한 블로그의 현장* |

그래서 찾아봤습니다. 어딘가 당연히 방법이 있겠지.
구글링을 해보니 이미 많은 분들이 Jekyll 에 댓글창을 추가해 사용하고 계시더군요.
그 방법은 바로 Disqus 라는 사이트의 서비스를 이용하는 것이었습니다.
자, 일단 [https://disqus.com/](https://disqus.com/) 에 접속해서 계정을 만들어 봅시다.

| ![-]({{"/assets/200820/image2.png"| relative_url}}) | 
|:--:| 
| *계정을 만든 다음에는 이메일 인증을 꼭 해주세요* |

계정을 만들고 Get started 를 클릭하면 다음과 같은 선택창이 뜹니다.
저는 제 사이트에 댓글을 만들고 싶으므로 아래의 "I want to install Disqus on my site" 를 클릭합니다.

| ![-]({{"/assets/200820/image3.png"| relative_url}}) | 
|:--:| 
| *선택지가 둘 뿐이군요. 아래꺼 선택!* |

그 후 Disqus를 사용하기 위한 설정들을 해야 합니다.
먼저 사이트를 나타낼 이름을 지어봅니다. (저는 간단하게 hhangitblog로 했습니다.)
Category 도 아무거나 선택해주시구요.

| ![-]({{"/assets/200820/image4.png"| relative_url}}) | 
|:--:| 
| *간단하고 기억하기 쉬운 걸로 지어봅시다* |

그런 다음엔 서비스를 선택해야 합니다.
돈 내고 굉장히 좋은 서비스를 사용할 수도 있군요.

| ![-]({{"/assets/200820/image5.png"| relative_url}}) | 

돈 안내는 서비스는 없나 찾아보니, 아래쪽에 숨겨뒀네요.
Basic을 선택하시면 무료로 이용이 가능합니다.
물론 선택은 여러분에게 달려있습니다.

| ![-]({{"/assets/200820/image6.png"| relative_url}}) | 

그 다음엔 어떤 플랫폼을 사용하고 있는지 고르게 되어있습니다.
이중 Jekyll을 선택해줍시다.

| ![-]({{"/assets/200820/image7.png"| relative_url}}) | 
|:--:| 
| *Jekyll 말고도 웹사이트 만드는 선택지가 굉장히 많군요* |

자, 이러면 disqus 사이트에서 설정할 부분은 모두 끝났습니다.

---
다음 장면에서는 Jekyll 코드에서 어떤 부분을 만져야 댓글창이 나오는지 알려줍니다.
Post를 작성할 때 _post/ 디렉터리에 yyyy-mm-dd-name.md 문서를 작성하실 겁니다.
이 때, 각 .md 파일의 맨 윗부분에 변수로 "comments" 라는 것을 만들고, 이를 "true"로 설정해줘야 합니다.
그림에서처럼 말이죠.
여기서, layout: default 처럼 여러분이 포스트 글에서 사용하고 계신 _layout/[레이아웃이름].html 을 기억하고 계세요!

| ![-]({{"/assets/200820/image8.png"| relative_url}}) | 

이후, 위 그림의 아래 부분에 나와있는 파란 글씨의 "Universal Embed Code"라는 것을 클릭해봅시다.
그럼 다음과 같은 긴 코드가 나오는데요.
여러분의 Jekyll 디렉토리에 _includes/disqus_comments.html 이라는 파일을 만들고, 이 코드를 모두 복사해 붙여넣어 주세요.
코드 중간에 보시면 제가 위에서 지은 블로그이름 'hhangitblog' 가 사용되고 있는 걸 확인하실 수 있어요.

| ![-]({{"/assets/200820/image9.png"| relative_url}}) | 
|:--:| 
| ![-]({{"/assets/200820/image10.png"| relative_url}}) | 
|:--:| 
| *위 코드를 _includes/disqus_comments.html 에 붙여넣기 한 모습* |

거의 다 됐습니다!
이제 _layouts 만 바꾸면 됩니다.
저는 각 포스트의 레이아웃을 _layouts/post.html 을 참조하게 해놨는데요.
그럼 이 코드의 밑 부분에 아래 코드들을 추가하면 댓글창 사용이 가능합니다.

{% raw %}
~~~html
  {%- if page.comments -%}
  <div id="post-disqus" class="container">
    {%- include disqus_comments.html -%}
  </div>
  {%- endif -%}
~~~
{% endraw %}

| ![-]({{"/assets/200820/image11.png"| relative_url}}) | 

자, 끝났습니다!
이제 포스트를 확인하시면 댓글창이 들어와 있는 걸 보실 수 있습니다.
간단한 것 같으면서도 여러 과정을 거쳐야 하는군요.
그래도 블로그에 댓글창 정도는 있어야겠죠.
배워보길 잘했다는 생각이 듭니다.

| ![-]({{"/assets/200820/image12.png"| relative_url}}) | 
|:--:| 
| *드디어 댓글로 소통이 가능해졌군요. 편-안* |

이제 여러분도 댓글창을 만드실 수 있습니다.
긴 글 읽어주셔서 감사합니다.
