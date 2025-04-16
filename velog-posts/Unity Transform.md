<ul>
<li><p>Unity에서의 GameObject는 반드시 Transform 컴포넌트를 가지고 있다.(삭제 불가)</p>
</li>
<li><p>위치, 회전, 크기의 관리 또한 부모 자식을 연결시켜주는 역할을 한다.</p>
</li>
<li><p>Unity에서는 더 빠른 연산을 위해 연산을 4x4행렬로 구현이 된다.</p>
</li>
<li><p>위치 크기에 대한 변화는 간단한 행렬연산이지만 </p>
</li>
<li><p>회전은 조금 더 복잡한 연산이기 떄문에 행렬연사으로 한다는 이해는 필요하다.</p>
</li>
</ul>
<h2 id="position">Position</h2>
<ul>
<li><p>X, Y, Z 3축으로 구성 되어있다.</p>
</li>
<li><p>Vector3로 구현되어있다.</p>
</li>
</ul>
<pre><code class="language-cs">transform.position = new Vector3(x,y,z);

transform.Translate(Vecotor3.Forward * Time.deltaTime);

// targetPosition을 향하여 1초에 speed만큼의 속도로 이동.
transform.MoveTowards(currentPosition, targetPosition, speed * Time.deltaTime);

// Lerp = linear interpolation의 약자
// currentPosition에서 targetPosition까지 rate 비율만큼 이동한 지점을 반환한다.
// Update마다 실행되면 보간을 반복하면 부드러운 움직임을 보여준다.
transfrom.position = Vector3.Lerp(currentPosition, targetPosition, rate)
</code></pre>
<p>Position을 잘 다루기 위해서는 프레임에 대한 이해가 필요하다.</p>
<ul>
<li><p>Unity의 Update에는 프레임마다 실행된다. </p>
</li>
<li><p>즉 이동로직을 Update에 넣는다면 플레이어마다 다른 이동속도를 가지게 되는 문제를 겪을 수 있다.</p>
</li>
<li><p>이런 문제를 해결하기위해 Time.deltaTiem이 존재한다.</p>
</li>
<li><p>Time.deltaTime은 프레임마다 값이 달라지는데 그 값은 이전 프레임과 현재 프레임 사이에 실제로 걸린 시간이다.</p>
</li>
<li><p>즉 어떤 프레임 이든간에 1초에 1이라는 고정값이 나오게 되어 Update 이동속도 문제를 해결할 수 있게된다.</p>
</li>
</ul>
<h2 id="rotation">Rotation</h2>
<ul>
<li><p>X, Y, Z 축으로 보여지지만 내부적으로는 4원소인 Quaternion을 통해 이뤄진다.</p>
</li>
<li><p>연산속도가 빠르고 징벌락 현상을 방지하는데 효과적이다.</p>
</li>
<li><p>더 부드러운 회전구현이 가능해진다.</p>
</li>
<li><p>Nomalized 되어 있기 때문에 수치 오차에 강하다.</p>
</li>
</ul>
<p>짐벌락
<img alt="" src="https://velog.velcdn.com/images/hsd0604/post/b15f8982-6753-434d-9a2b-dc8bab82fb8d/image.png" /></p>
<ul>
<li><p>짐벌락 현상은 두개의 축이 겹쳐져 하나의 축의 기능이 상실하는 현상이다.
Euler각을 기준으로 연산한다면 생길 수 있는 문제이다.</p>
</li>
<li><p>그리고 Quaternion이 Euler보다 성능적으로 더 빠르다.</p>
</li>
</ul>
<h3 id="회전에-대한-이해">회전에 대한 이해</h3>
<p>X의 회전값을 돌리면 Y축이 돌아가고
Y의 회전값을 돌리면 </p>
<pre><code class="language-cs">// rotation에 값을 직접 지정해 회전하는 방법
transform.rotation = Quaternion.Euler(x, y, z);

// 방향을 Quaternion으로 변환
Quaternion.LookRotation(Vector);



// Rotate 함수를 이용한 회전
transform.Rotate(Vector3.up, Speed);

// RotateAround를 이용한 중심축 기준으로 회전
transform.RotateAround(Target.position, Vector3.up, Speed);

// LookAt을 이용한 대상을 향해 회전시키기 즉 바라보기
transform.LookAt(Target.position);</code></pre>
<p>회전의 보간 필요시 Quaternion쓰는 방법은 다음과 같다.</p>
<pre><code class="language-cs">float rotSpeed = 2f; 

Quaternion start   = transform.rotation;
Quaternion end     = targetRotation;

transform.rotation = Quaternion.Slerp(start,end, rotSpeed * Time.deltaTime); 
// 구면 선형 보간

// 실제 회전 구면 상의 최단 거리 경로로 함.
// 일정하게 느껴지는 자연스러운 속도

//하지만
// 느림 (삼각함수 연산을 포함하고 있음.)

// 부드럽고 자연스러운 회전이 필요할 경우 적합함.

transform.rotation = Quaternion.Lerp(start,end, rotSpeed * Time.deltaTime);  
// 선형 보간

// 직선처럼 보간
// 일정하지 않음

//하지만
// 빠름 (단순 계산)

// 속도가 더 중요할 경우 (작은 변화)에 적합함.</code></pre>
<p>Qauternion은 Euler보다 더 복잡해서 보통 Euler로 작업을 한 뒤
Qauternion으로 바꿔주어 적용하는 식으로 많이 사용한다.</p>
<h2 id="world-space와-local-space">World Space와 Local Space</h2>
<p>World Space</p>
<ul>
<li>세상을 기준에서 어디에 있는지를 말하는 절대 좌표이다.</li>
</ul>
<p>Local Space</p>
<ul>
<li>부모가 있을 경우 부모를 기준으로 위치한다. 즉 상대적이다.</li>
</ul>
<p>부모 자식 관계일때 자식의 회전을 부모의 회전에 영향받지않고 독립적으로 하고싶을때에는
자식의 월드 좌표계를 회전하도록 할 수 있다는 것이다.</p>
<pre><code>transform.rotation = Quaternion.Euler(position);</code></pre><p>만약 그러고 싶지 않고 부모의 회전도 영향을 받았으면 좋겠다면</p>
<pre><code class="language-cs">transform.localRotation = Quaternion.Euler(position);</code></pre>
<p>처럼 로컬좌표계를 쓰면 된다.</p>
<h1 id="위치이동">위치이동</h1>
<h2 id="입력">입력</h2>
<p>GetKey</p>
<ul>
<li>누르고 있는 동안 true이다.</li>
</ul>
<p>GetkeyDown</p>
<ul>
<li>키를 눌렀을때 한번만 true가 된다.</li>
</ul>
<p>GetKeyUp</p>
<ul>
<li>키를 떗을때 한번만 true가 된다.</li>
</ul>
<pre><code class="language-cs">if(Input.GetKeyDown(KeyCode.Space))
    Debug.Log(&quot;Input SpaceBar Down&quot;);

if(Input.GetKeyUp(KeyCode.Space))
    Debug.Log(&quot;Input SpaceBar Up&quot;);

if(Input.GetKey(KeyCode.Space))
    Debug.Log(&quot;Input SpcaeBar Running&quot;);</code></pre>