<p>외적과 내적의 개념은 게임개발시 반드시 쓰이게 되는 개념중 하나이다.</p>
<h1 id="dot">Dot</h1>
<blockquote>
<p>두 벡터가 얼마나 같은 방향을 향하고 있는지에 대한 값을 나타낸 연산이다.</p>
</blockquote>
<p>A * B = A벡터의 길이 * B벡터의 길이 * cos(두 벡터 사이의 각도) 이다.</p>
<p>즉 두 벡터의 길이와 방향의 유사성을 곱한 값이 된다는 것이다.</p>
<p>그리고 두 벡터가 정규화 상태라면 |A| * |B| = cos(0) 라는 값이 나온다.</p>
<h3 id="방향이-같을-때">방향이 같을 때</h3>
<p>방향이 같을때에는 세타는 0도</p>
<p>Cos(0) = 1</p>
<p>최종값은 1이 나온다.</p>
<h3 id="수직일-때">수직일 때</h3>
<p>세타는 90도</p>
<p>Cos(90) = 0</p>
<p>최종값은 0이 나온다.</p>
<h3 id="방향이-다를-때">방향이 다를 때</h3>
<p>세타는 180도</p>
<p>Cos(180) = -1</p>
<p>최종값은 -1이 나온다.</p>
<p>즉 방향이 같을 수록 1에 가까워지고 다를수록 -1에 가까워진다.</p>
<p>내적은 두 벡터의 원소끼리의 곱이기도 하다. (Vector3.Dot(a, b)가 내부에서 사용하는 식)</p>
<p>A * B = |A| * |B| * cos(0) = (Ax * Bx) + (Ay * By) + (Az * Bz) 이다.</p>
<h2 id="dot-활용">Dot 활용</h2>
<h3 id="앞뒤-판별">앞뒤 판별</h3>
<pre><code class="language-cs">Vector3 forward = transform.forward;
Vector3 toTarget = (target.position - transform.position).nomalized;

if(Vector3.Dot(forward,toTarget) &gt; 0)
    Debug.Log(&quot;앞에 있다.&quot;);
else
    Debug.Log(&quot;뒤에 있다.&quot;);</code></pre>
<p>내가 바로보는 앞의 벡터와 나에게서 상대에게로 가는 벡터의 내적을 구해서</p>
<p>0보다 크면 앞에 있고 아니면 뒤에 있는 식으로 판단이 가능하다.</p>
<p>예시로 앞에 있다면 방어막 전개시 데미지 감소나 캔슬 등 여러가지로 응용가능하다.</p>
<h3 id="시야-판별">시야 판별</h3>
<pre><code class="language-cs">float fieldOfView = 60f;
float halfFOV = fieldOfView / 2f;

float dot = Vector3.Dot(transform.forward,toTarget.nomalized);

if(dot &gt; Mathf.Cos(halfFOV * Mathf.Deg2Rad))
    Debug.Log(&quot;시야 안에 있다.&quot;);</code></pre>
<ul>
<li><p>앞 벡터와 나에게서 상대로 가는 벡터를 dot로 구한다.
(dot는 모두 정규화된 벡터이기 때문에 cos(0)과 같이 같다.)</p>
</li>
<li><p>그게 만약 시야각의 반을 라디안으로 변형한 값을 코사인으로 계산한 것보다 크다면
시야각안에 들어와 있다고 판단한다.</p>
</li>
<li><p>즉 시야각의 반으로 계산하는 것은 전체가 60이라고 하면 좌우 30씩안에 들어와 있어야 
시야각에 들어와있는 것이기 때문이다.</p>
</li>
</ul>
<h4 id="왜-cos을-사용하는가">왜 Cos을 사용하는가?</h4>
<p>cos의 성질로는 각도가 크면 값이 작아지고 작으면 커지는 성질이 있다.</p>
<p>그래서 dot = cos(0)를 가지고 있다면</p>
<pre><code class="language-cs">if(dot &gt; cos(시야각/2))</code></pre>
<p>라는 것이 성립하는 것이다.</p>
<h4 id="조명-각도계산-nomal-map">조명 각도계산 (Nomal map)</h4>
<pre><code class="language-cs">float intensity = Mathf.Max(Vector3.Dot(normal, lightDir), 0f);</code></pre>
<h2 id="dot-정리">Dot 정리</h2>
<ul>
<li><p>내적은 앞 뒤를 비교하는데 편리한 연산이다.</p>
</li>
<li><p>가까울 수록 1에 가까워지고 멀어질수록 -1에 가까워진다는 성질이 있다. (cos과 같은 성질)</p>
</li>
<li><p>Dot은 결국 cos(0)이고 (정규화 가정하에)</p>
</li>
<li><p>그러기 때문에 dot값과 cos값을 비교할 수 있는 것이다.</p>
</li>
</ul>
<h4 id="시야각-판단을-더-정리하자면">시야각 판단을 더 정리하자면</h4>
<ul>
<li><p>dot(정규화된 나에게서 상대로 가는 벡터와 내 앞방향의 내적,cos값) 과</p>
</li>
<li><p>cos(시야각의 반 (좌우 반반씩) 에 대한 각도) 를 똑같은 cos값으로 비교해서 </p>
</li>
</ul>
<p>dot이 cos의 값인 시야각의 판단 최댓값 보다 크다면 안에 들어와 있다고 판단한다.
(각도가 작으면 값이 커지는 성질과, 각도가 작으면 더 가까이 있기 때문.)</p>
<h1 id="cross">Cross</h1>
<blockquote>
<p>두 벡터가 이루는 평면에 수직인 벡터를 반환하는 연산이다.</p>
</blockquote>
<p>간단히 말하면 Dot은 앞뒤를 판별해 줬다면 Cross는 좌우를 판별해주는 것이다.</p>
<p>계산식은 다음과 같다.
<img alt="" src="https://velog.velcdn.com/images/hsd0604/post/c2fdeecb-cd7e-4e09-a668-737902d77492/image.png" />
X = Ay * Bz - Az * By;       ( 계산에 x없음 )
Y = Az * Bx - Ax * Bz;       ( 계산에 y없음 )
Z = Ax * By - Ay * Bx;         ( 계산에 z없음 )</p>
<ul>
<li>X성분은 y와 z의 차이</li>
<li>Y성분은 z와 x의 차이</li>
<li>Z성분은 x와 y의 차이
(정확히는 성분간의 회전 효과, 나머지 두 축의 회전 기여도를 뺀 것이다.)</li>
</ul>
<ul>
<li><p>이 계산식은 모든 방향과 축에 대한 정보가 한 번에 들어가 있다.</p>
</li>
<li><p>외적 벡터의 크기 자체는 두 벡터가 이루는 평행사변형의 넓이이다.</p>
</li>
</ul>
<h2 id="cross-활용">Cross 활용</h2>
<h3 id="normal-map">Normal Map</h3>
<pre><code class="language-cs">Vector3 normal = Vector3.Cross(edge1,edge2).normalized;</code></pre>
<ul>
<li>삼각형의 두 변을 외적하면 그 면에 수직인 법선(normal)이 나온다.</li>
<li>쉐이더, 물리, 라이팅에 쓰인다.</li>
</ul>
<h3 id="좌우-판단">좌우 판단</h3>
<pre><code class="language-cs">Vector3 toTarget //  그거
Vector3 cross = Vector3.Cross(transform.forward, toTarget);

if(cross.y &gt; 0)
    Debug.Log(&quot;왼쪽&quot;);
else
    Debug.Log(&quot;오른쪽&quot;);</code></pre>
<ul>
<li><p>결과는 결국 두 벡터가 이루는 평면에 수직인 벡터가 나온다.</p>
</li>
<li><p>cross.y는 전체 벡터에서 y축으로 분해 한것이다.
즉 회전축이 얼마나 Y 방향을 향하고 있는지를 나타낸다.</p>
</li>
<li><p>두 벡터가 이루는 평면은 대상의 상대적인 위치(왼쪽, 오른쪽) 에 따라
Y+ 방향 혹은 Y− 방향으로 회전축이 나뉜다.</p>
</li>
<li><p>우리는 보통 XZ 평면 상에서 좌우를 판단하고 그 평면에 수직인 축이 바로 Y축이므로
cross.y를 기준으로 왼쪽과 오른쪽을 구분할 수 있다.</p>
</li>
<li><p>그래서 forward와 toTarget의 외적이 y+면 왼쪽 y-면 오른쪽으로 판단한다.</p>
</li>
</ul>
<h3 id="회전-방향-판단">회전 방향 판단</h3>
<pre><code class="language-cs">float crossZ = a.x * b.y - a.y * b.x;

if(crossZ &gt; 0)
    Debug.Log(&quot;반시계 방향&quot;)'
else
    Debug.Log(&quot;시계 방향&quot;)</code></pre>
<p>위와 같은 맥락이다. (회전 기여도)</p>
<h3 id="회전-축-구하기">회전 축 구하기</h3>
<pre><code class="language-cs">Vector3 from = transform.forward;
Vector3 to // 그거

Vector3 axis = Vector3.Cross(from,to).normalized;
float angle = Vector3.Angle(from.to);

Quaternion rotation = Quaternion.AngleAxis(angle, axis);</code></pre>
<ul>
<li>from에서 to 로 회전하려면 그 사이의 외적이 회전축이 된다.</li>
<li>이걸 Quaternion.AngleAxis에 넣으면 정확하게 회전시킬 수 있다.</li>
</ul>
<h2 id="cross-정리">Cross 정리</h2>
<blockquote>
<p>외적은 좌우 판단에 유리하다.</p>
</blockquote>
<p>외적의 결과값은 회전 축이자 면적 방향의 벡터이다.</p>
<p>내적이 cos이라면 외적은 sin이다.</p>
<p>실제로 정규화를 시킨 값으로 계산식을 짜면 sin(0)와 값이 같다. (sin의 직각일수록 1이라는 성질)</p>
<h2 id="projection">Projection</h2>
<blockquote>
<p>A가 B 방향으로 얼마나 늘어나는지 (투영되었는지)를 구할때 쓰인다.</p>
</blockquote>
<ul>
<li><p>기준점이 되는 방향의 벡터만 정규화를 한 상태로 계산하면</p>
</li>
<li><p>Projection Length = |A| * |B|.normalized;</p>
</li>
<li><p>A(투영하려는 벡터) B(정규화된 기준 벡터)로 계산시 결과값은 길이인 스칼라값이 된다.
즉 A가 B 방향으로 얼마만큼의 영향을 주는가에 대한 것이고 </p>
</li>
<li><p>길이 관점에서 본다면 |A| * cos(0)이다.</p>
</li>
<li><p>이런식으로 순수한 투영길이를 알아낼 수 있다.</p>
</li>
</ul>
<p>Cross도 사용해서 미끄러짐 방향계산 등을 할 수 있다.</p>
<pre><code class="language-cs">Vector3 slideDir = Vector3.Cross(groundNormal, Vector3.Cross(moveDir, groundNormal));</code></pre>
<p>Cross안에 Cross를 써서 MoveDir의 평면 투영 벡터를 만들어낸 것이다.</p>
<h1 id="최종-정리">최종 정리</h1>
<h2 id="dot-내적---cosθ">Dot (내적) -&gt; cos(θ)</h2>
<ul>
<li><p>두 벡터가 얼마나 같은 방향을 향하고 있는지</p>
</li>
<li><p>방향성 확인 (앞/뒤, 시야, 유사도 등)</p>
</li>
<li><p>최대: 방향 같을 때 → cos(0°) = 1</p>
</li>
<li><p>0이면 수직, 음수면 반대 방향</p>
</li>
</ul>
<h2 id="cross-외적---sinθ">Cross (외적) -&gt; sin(θ)</h2>
<ul>
<li><p>두 벡터가 얼마나 서로 회전 관계에 있는지</p>
</li>
<li><p>회전 축, 좌우, 법선, 면적 확인</p>
</li>
<li><p>최대: 수직일 때 → sin(90°) = 1</p>
</li>
<li><p>0이면 평행(같거나 반대), 결과 벡터는 없음 (크기 0)</p>
</li>
</ul>
<p>그래서</p>
<ul>
<li><p>결국 내적과 외적은 정규화를 하여 쓰면 계산이 아주 편해진다는 특징이 있다.</p>
</li>
<li><p>상황에 따라 다르겠지만 보통 외적보다는 내적의 사용률이 높고 
외적을 쓸때에도 내적과 함께 사용하는 경우가 많다.</p>
</li>
<li><p>그리고 둘다 쉐이더를 이해하기 위해서는 꼭 알아야 할 중요한 개념이다.</p>
</li>
</ul>