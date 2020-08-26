---
layout: post
title: 깃 블로그에서 코드블록 만들기 - liquid, html, markdown을 한 번에!
comments: true
categories: [blog]
---

### 깃 블로그에서 코드 부분을 볼 수 있게 해보자!

안녕하세요 한헌종입니다.
이번엔 블로그에서 코드를 설명할 때 특정 블록으로 처리하는 방법을 알아보겠습니다.

깃 블로그는 Jekyll로 이루어져 있고, 각 포스트는 .md 파일로 구성되어 있죠.
이 파일 안에는 그냥 텍스트를 쓸 수도 있지만, liquid, html, markdown 세가지 언어를 통해 특정 기능이나 표현을 할 수 있습니다.

그런데 어떤 코드를 썼는지를 그대로 보여주려면 어떻게 할까요?
예를 들어, 아래처럼 이탤릭체로 기울어진 텍스트는 'asterisk 별표 모양을 글 양옆에 붙이면 된다'는 설명을 어떻게 하면 될까요?
*기울임글자*
별표-텍스트-별표 라고 쓰세요! 이런식으로 설명할 수도 없고 말이죠.

이럴 때, 코드블록이 필요합니다.
깃 블로그에서 쓰는 liquid, html, markdown 세가지에 대해 어떻게 코드를 나타낼 수 있는지 알아보겠습니다.

---
### liquid 의 경우

{% raw %}
Jekyll 은 중괄호-퍼센트 글자 사이, 즉 {% 와 %} 모양 안에 특정 문구를 넣어서 원하는 명령을 실행할 수 있습니다.
{% endraw %}
이걸 liquid 언어라고 하는데요, 이처럼 작성하면 Jekyll이 이 부분을 인식하고 렌더링을 통해 특정 기능을 실행합니다.
사실 이전 포스트에서 '이 코드를 넣으세요' 라며 설명하는 부분이 있었는데,
그 코드를 실제로 쓰려니 코드가 렌더링 되어서 여러분께 보여드릴 수가 없었습니다.

그래서 찾아보았습니다. 역시 구글은 검색하면 다 나오는군요.
아래처럼 Jekyll liquid 언어를 블로그에 넣는 방법을 설명드리겠습니다.
두 가지 방법이 있습니다. 하나는 raw-endraw 구문, 다른 하나는 highlight-endhighlight 구문입니다.

{% raw %}
{%- if page.comments -%}
    {%- include disqus_comments.html -%}
{%- endif -%}
{% endraw %}

위 세줄을 .md 파일에 그냥 쓰면 이 글에서 보이지 않을 겁니다. 그럼 어떻게 한 걸까요?
방법은 정말 간단합니다.
아래 그림처럼 raw - endraw 구문을 넣으면 됩니다!

| ![-]({{"/assets/200822/image1.png"| relative_url}}) | 
|:--:| 
| *이렇게 말입니다* |

정말 쉽죠?
liquid 문구 중 raw-endraw 라는 문구를 쓰면, 그 안에서 Jekyll liquid 언어를 작성하더라도
실제로 렌더링 하는 대신 '그대로' 보여주더군요.

그런데, 너무 코드블록 느낌이 안 나는군요. 어떻게 하면 될까요?
이 때는 pre, code 문구를 추가로 위아래에 집어넣으면 됩니다. 아래 그림처럼요.

| ![-]({{"/assets/200822/image2.png"| relative_url}}) | 

그럼 아래처럼 표시할 코드부분만 코드블록 안에 나오게 됩니다.

<pre><code>{% raw %}{%- if page.comments -%}
    {%- include disqus_comments.html -%}
{%- endif -%}{% endraw %}</code></pre>

이 방법을 통해서 여러분께 liquid 언어 쓰는 방법을 더 자세하게 설명할 수 있게 되었습니다.

---
### html의 경우
html 을 코드블록에 표시하는 방법은 두가지입니다.
첫 번째는 물결 세 개를 쓰고 html이라고 쓰면 되는군요.

| ![-]({{"/assets/200822/image3.png"| relative_url}}) | 

그럼 이렇게 나옵니다.
~~~html
<a href="https://hhj6212.github.io/">
   블로그 링크
</a>
~~~

혹은 highlight-endhighlight 기능을 사용할 수 있습니다.
highlight는 특정 언어를 지정하면, 그 언어에 맞는 형식대로 코드블록에서 보여줄 수 있게 해줍니다.
{% raw %}여기서 표시해야 할 언어는 html 이니, {% highlight html %} 이라고 작성해야 합니다.{% endraw %}

| ![-]({{"/assets/200822/image4.png"| relative_url}}) | 

이것도 똑같은 효과를 가져옵니다.
{% highlight html %}
<a href="https://hhj6212.github.io/">
   블로그 링크
</a>
{% endhighlight %}

---
### markdown 언어의 경우
markdown 언어는 어떻게 할까요?
위에서 썼던 방법 중 pre, code 문구를 사용하면 됩니다.
이렇게 말이죠.

| ![-]({{"/assets/200822/image5.png"| relative_url}}) | 

그럼 아래처럼 나타나게 됩니다.
<pre><code>### 하하하</code></pre>

---
### 세 언어를 동시에 사용할 때는요?
만약 제가 liquid, html, markdown 세 가지 언어로 작성된 .md 파일을 여러분께 그대로 보여드리고 싶을 때는 어떻게 할까요?
물론 스크린샷을 찍어서 보여드릴 순 있겠죠.
그렇지만 여러분께서 따라서 연습하고 싶으실 땐 복사-붙여넣기를 하고 싶으실테니까, 좀 더 좋은 방법을 사용해봅시다.

위에서 사용한 세 가지 방법을 잘 조합하면 됩니다!
1. liquid: raw-endraw 문구
1. html: highlight 문구
1. markdown: pre/code 문구

코드를 감쌀 때, 위 세 개의 물고 물리는 관계를 잘 고려해야 합니다.
예를 들어, pre/code 문구는 html이니, 이건 highlight로 묶기 전에 선언해야 합니다.
또, highlight 문구는 liquid 언어이므로, raw-endraw로 묶기 전에 선언해야 하죠.

즉 다음과 같은 구조로 짜야 합니다.
가장 바깥에 pre-code, 그 다음에 highlight, 그리고 raw 로 묶으면 됩니다.
아래 그림처럼 말이죠.

| ![-]({{"/assets/200822/image6.png"| relative_url}}) | 

그럼 아래처럼 liquid, html, markdown 세가지 언어가 모두 '그대로' 여러분께 표시됩니다!

<pre><code>{% highlight html %}{% raw %}{%- if page.comments -%}
    <div id="post-disqus" class="container">
        ### 하하하
        {%- include disqus_comments.html -%}
    </div>
{%- endif -%}{% endraw %}{% endhighlight %}</code></pre>

지금까지 Jekyll 에서 liquid, html, markdown 을 코드블록으로 표시하는 방법을 배워보았습니다.
여러분의 블로그에서도 잘 활용하셨으면 좋겠습니다.

그럼 다음 시간에 만나요!
