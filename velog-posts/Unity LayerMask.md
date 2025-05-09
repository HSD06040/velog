<p>Layer는 객체를 구분하기 위해 쓰이고 비트를 사용해서 사용이 가능하다.</p>
<h1 id="layermask">LayerMask</h1>
<ul>
<li>직접 지정하고 쓸 수 있는 타입이라고 생각하면 편하다.</li>
<li>Layer의 특이점이라고 하면 특정 레이어를 제외하고 렌더링하는 등의 기능을 동작 가능하고</li>
<li>Wall Layer가 카메라의 앞을 가렸다면 카메라를 당기도록 할 수도 있다.</li>
</ul>
<p>LayerMask는 비트 마스크를 활용한 레이어 필터링 시스템이며, 
Tag보다 연산 효율이 높고, 충돌/렌더링/레이캐스트 필터링에 주로 사용된다.</p>
<h2 id="비트-시프트">비트 시프트</h2>
<blockquote>
<p>Unity의 레이어 마스크는 32자리의 이진법으로 표현된다.</p>
</blockquote>
<p>32자리의 이진법으로 표현이 가능해서 32가지의 레이어를 전부 1로 하여 사용이 가능하다.</p>
<pre><code class="language-cs">// 5가 Player라면 왼쪽으로 4칸 이동
private int player = (1 &lt;&lt; 4) 

// Player(레이어 4) + Enemy(레이어 6) 포함
LayerMask mask = (1 &lt;&lt; 4) | (1 &lt;&lt; 6);

// GameObject의 Layer가 mask와 일치한다면 true 아니면 false 처럼 사용 가능
bool IsInLayerMask(GameObject obj, LayerMask mask)
{
    return (mask.value &amp; (1 &lt;&lt; obj.layer)) != 0;
}</code></pre>
<p>로 사용이 가능하다.
Tag는 하나밖에 못 달지만, Layer는 마스크로 다중 체크가 가능하다.</p>