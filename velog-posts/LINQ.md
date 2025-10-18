<p>코딩테스트를 하다가 LINQ를 알면 훨씬 간단하고 쉽게 풀수 있을 것 같아 사용해보려고 한다.</p>
<h3 id="groupby">GroupBy</h3>
<blockquote>
<p>어떠 한 기준으로 묶어서 그룹화를 넣으면 그룹화를 진행한다.</p>
</blockquote>
<pre><code class="language-cs">Array.GroupBy(b =&gt; b)// 값은 값끼리 그룹화</code></pre>
<h3 id="orderby">OrderBy</h3>
<blockquote>
<p>기준을 넣어주면 그 기준대로 정렬한다.</p>
</blockquote>
<pre><code class="language-cs">Array.OrderBy(o =&gt; o.Count()) // 요소수가 적은 순서대로 정렬

// 반대 정렬도 가능하다.

Array.OrderByDescending(o =&gt; o.Count()) // 요소수가 많은 순서대로 정렬</code></pre>
<p>OrderBy =&gt; 오름차순
OrderByDescending =&gt; 내림차순</p>
<h3 id="selectmany">SelectMany</h3>
<blockquote>
<p>배열안의 배열같은 구조를 1차원으로 펼칠때 사용한다.</p>
</blockquote>
<p>SelectMany = Select + Flatten (펼치기)</p>
<p>Select는: 각 항목을 변환</p>
<p>SelectMany는: 각 항목을 여러 개로 변환한 후, 그것들을 펼쳐서 하나의 시퀀스로 합침</p>
<pre><code class="language-cs">string[] words = { &quot;cat&quot;, &quot;dog&quot;, &quot;bat&quot; };

// 각 단어의 문자(char)를 하나씩 펼치고 싶다
var chars = words.SelectMany(word =&gt; word.ToCharArray());

['c', 'a', 't', 'd', 'o', 'g', 'b', 'a', 't'] // SelectMany 상황
[[&quot;c&quot;,&quot;a&quot;,&quot;t&quot;], [&quot;d&quot;,&quot;o&quot;,&quot;g&quot;], [&quot;b&quot;,&quot;a&quot;,&quot;t&quot;]] // Select 상황</code></pre>
<p>추가 예시</p>
<pre><code class="language-cs">int[][] numbers = new int[][]
{
    new int[] { 1, 2 },
    new int[] { 3, 4 },
    new int[] { 5 }
};

var flat = numbers.SelectMany(n =&gt; n); // 결과: {1, 2, 3, 4, 5}</code></pre>
<p>추가로</p>
<h3 id="distinct">Distinct</h3>
<blockquote>
<p>배열에서 쓸 수 있는 것</p>
</blockquote>
<pre><code class="language-cs">a.Distinct(); // 중복 제거</code></pre>
<p>훨씬 간단하게 쓸 수 있다.</p>