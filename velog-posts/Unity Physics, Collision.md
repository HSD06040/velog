<p>물리와 충돌은 가장 많이 다뤄지면서 기초적인 오브젝트 상호작용중 하나이다.</p>
<p>게임 엔진 Unity에서는 이를 지원하는데 그 만큼 주어진 기능을 잘 소화해내는 것은 개발자의 몫이다.</p>
<p>그리고 물리엔진은 많은 연산을 요구하는 기능들중 하나이니 신중하게 사용해야 한다.</p>
<h1 id="physics">Physics</h1>
<h2 id="rigidbody">Rigidbody</h2>
<ul>
<li><p>게임 오브젝트에 물리를 적용시키는 컴포넌트이다.</p>
</li>
<li><p>질량, 공기저항, 중력과 같은 값의 조절을 지원한다.</p>
</li>
</ul>
<h3 id="mass">Mass</h3>
<ul>
<li>질량 값이다.</li>
<li>관성, 충돌계산 등에 영향을 주며 크면 밀기힘들고 작으면 잘밀리는 것이다.</li>
</ul>
<h3 id="drag">Drag</h3>
<ul>
<li>공기저항 값이다.</li>
<li>이동 속도를 줄이는 마찰값이며 값이 높으면 더 빠르게 감속한다. (즉 높을 수록 느리다.)</li>
</ul>
<h3 id="angulardrag">AngularDrag</h3>
<ul>
<li>회전 저항값이다.</li>
<li>회전 운동의 감속값이며 높을수록 회전이 빨리 멈춘다.</li>
</ul>
<h3 id="automatic-center-of-mass-bool">Automatic Center of Mass bool</h3>
<ul>
<li>Unity에서 자동으로 중심을 계산해줌.</li>
</ul>
<h3 id="automatic-tensor-bool">Automatic Tensor bool</h3>
<ul>
<li>관성을 자동 계산한다.</li>
</ul>
<h3 id="use-gravity-bool">Use Gravity bool</h3>
<ul>
<li>중력을 적용한다.</li>
</ul>
<h3 id="is-kinematic-bool">is Kinematic bool</h3>
<ul>
<li>Rigidbody가 물리엔진의 영향을 받지 않는다.
하</li>
</ul>
<h3 id="interpolate">Interpolate</h3>
<ul>
<li><p>Unity의 물리엔진이 완벽한 것은 아니다.</p>
</li>
<li><p>물리엔진은 0.02초마다 갱신을 시켜주는데 이동속도가 너무 빠른경우
즉 0.02초 사이에 오브젝트를 넘는 경우에는 통과하는 문제가 발생한다.</p>
</li>
<li><p>(렌더링은 Update마다 갱신되기 때문에 끊겨 보일 수도 있음)</p>
</li>
<li><p>이 문제 때문에 Interpolate를 사용하면</p>
</li>
</ul>
<p>Interpolate</p>
<ul>
<li>(이전 프레임 위치를 기준으로 현재위치를 보간한다.)</li>
</ul>
<p>Extrapolate</p>
<ul>
<li>(다음 위치를 예측해서 이동시킨다.)</li>
</ul>
<p>그만큼 연산량이 많기 때문에 부담이 될 수도 있다.</p>
<h3 id="collision-detection">Collision Detection</h3>
<p>Discrete</p>
<ul>
<li>기본값이다.</li>
</ul>
<p>Continuous</p>
<ul>
<li>빠른 물체가 작은 물체를 뚫고 지나가지 않게함.</li>
</ul>
<p>Continuous Dynamic</p>
<ul>
<li>고속 물체에 적합하며 다른 Continuous 오브젝트와도 충돌을 계산함.</li>
</ul>
<p>Continuous Speculative</p>
<ul>
<li>예측 기반으로 충돌을 체크함. 성능자체는 좋지만 쓸데없는 충돌계산이 많아질 수 있음.</li>
</ul>
<h3 id="constraints">Constraints</h3>
<p>제약 조건이다.</p>
<ul>
<li>축을 기준으로 움직임을 못하고하고 회전을 못하게 하는등의 제약조건을 걸 수 있다.</li>
</ul>
<h3 id="layer-overrides">Layer Overrides</h3>
<p>Include layers / Exclude Layers</p>
<ul>
<li><p>Rigidbody가 충돌 계산을 포함하거나 제외할 Layer를 설정한다.</p>
</li>
<li><p>기본값은 Nothing으로 따로 설정되어 있지는 않다.</p>
</li>
<li><p>물체가 특정 레이어를 가지는 물체에만 충돌이 가능해야할때 사용한다.</p>
</li>
</ul>
<h2 id="project-setting">Project Setting</h2>
<ul>
<li><p>Unity 물리는 세팅은 2D와 3D가 구분되어있다.</p>
</li>
<li><p>프로젝트 세팅을 이용하면 물리적 세팅을 한번에 모두 적용이 가능하다.</p>
</li>
</ul>
<p>예를 들어</p>
<ul>
<li><p>Gravity는 자동으로 Y -9.81 로 설정되어 있는데 이걸 수정하면 일괄적으로 프로젝트에 적용이 가능하다.</p>
</li>
<li><p>Layer 충돌의 여부의 설정도 여기서 가능하다.</p>
</li>
</ul>
<p>Rigidbody 클래스에서 사용할 수 있는 기능으로는</p>
<h3 id="addforcevector-forcemode">AddForce(Vector, ForceMode.)</h3>
<ul>
<li><p>매개변수에 입력된 방향으로 힘을 가한다.</p>
</li>
<li><p>어떻게 가해질지 정하는 ForceMode도 존재한다.</p>
</li>
</ul>
<h3 id="addtorque">AddTorque</h3>
<ul>
<li>매개변수에 입력된 축의 회전력을 더한다.</li>
</ul>
<h3 id="velocity">velocity</h3>
<ul>
<li><p>현재 게임 오브젝트가 가지는 물리력 이다. (속도)</p>
</li>
<li><p>원하는대로 설정가능해서 비현실적인 움직임이 가능하다.</p>
</li>
<li><p>하지만 다른 물리값에 대한 충돌 가능성이 있다.</p>
</li>
</ul>
<h3 id="angularvelocity">angularVelocity</h3>
<p>현재 게임 오브젝트가 가지는 회전력이다.</p>
<h1 id="vector">Vector</h1>
<ul>
<li>특정위치로 부터 방향과 크기를 표현하는 방법이다.</li>
<li>움직임과 힘의 작용등을 표현할때 쓰이는 표현이다. 즉 Vector = 방향 + 크기이다.</li>
</ul>
<h2 id="vector의-덧셈">Vector의 덧셈</h2>
<p>Vector끼리 더하면 그 Vector가 모두 가해졌을때 어느방향으로 얼마의 힘을 주어야하는지에 대한 Vector가 나온다.</p>
<h2 id="vector의-뺄셈">Vector의 뺄셈</h2>
<p>Vector끼리 뺀다면 Vector에서 Vector까지 가는 방향과 거리 Vector가 값으로 나온다.</p>
<h2 id="vector의-크기">Vector의 크기</h2>
<p>magnatitude 벡터의 길이이다.</p>
<p> 루트 (x^2 + y^2 + z^2) 의 계산식으로 정확한 길이(크기)를 구하게 된다.</p>
<h2 id="vector의-방향">Vector의 방향</h2>
<p>하지만 게임의 움직임에서는 따로 속도를 지정하는 경우가 많기 떄문에 Vector의 방향만을 원하는 경우가 많다.</p>
<ul>
<li>그래서 Nomalized로 정규화를 시켜주어 길이는 영향이 없고 방향만이 영향이 갈 수 있도록 한다.</li>
</ul>
<h1 id="collision">Collision</h1>
<p>Unity의 충돌은 Rigidbody와 Collider을 통해 이루어진다.</p>
<h2 id="collision-1">Collision</h2>
<ul>
<li>Collider의 컴포넌트 isTrigger가 해제되어 있어야한다.(기본값)</li>
<li>자신이나 충돌체 둘중하나에는 Rigidbody가 있어야하고 isKinematic이 해제되어 있어야한다.</li>
<li>활성화 된 오브젝트 간의 충돌처리 (not overrlap) 이다.</li>
</ul>
<h2 id="trigger">Trigger</h2>
<p>trigger는 물리적인 충돌이 이뤄지지않고 통과된다. (overrlap)</p>
<ul>
<li>Collider의 컴포넌트 isTrigger가 해제되어 있어야한다.</li>
<li>자신이나 충돌체 둘중하나에는 Rigidbody가 있어야한다.</li>
</ul>
<h2 id="충돌체의-종류">충돌체의 종류</h2>
<p>정적 충돌체</p>
<ul>
<li>Rigidbody가 없는 충돌체</li>
</ul>
<p>정적끼리는 충돌과 메시지함수가 실행되지않는다.</p>
<p>리지드바디 충돌체</p>
<ul>
<li>Rigidbody를 가지는 충돌체</li>
</ul>
<p>리지드바디 충돌체는 모두 밀 수 있고 밀릴 수 있다.</p>
<p>키네마틱 충돌체</p>
<ul>
<li>Rigidbody를 가지고 있지만 isKinematic이 체크가 된 충돌체</li>
</ul>
<p>키네마틱은 밀 수는 있고 밀리지 않는다.</p>
<p>키네마틱은 런타임 중에 설정도 가능하다.</p>
<ul>
<li>키네마틱과 정적끼리는 밀리지 않는다.</li>
</ul>
<p>즉 충돌로 움직여지지 않는 (Rigidbody가 없거나 isKinematic) 출동체 면
밀리지 않는다.</p>
<h2 id="메서드">메서드</h2>
<p>Enter </p>
<ul>
<li>들어왔을때 1회 호출한다.</li>
</ul>
<p>Stay</p>
<ul>
<li>들어와 있을때 계속 호출한다.</li>
</ul>
<p>Exit</p>
<ul>
<li>나갔을때 1회 호출한다.</li>
</ul>
<pre><code class="language-cs">OnCollisionXXX(Collision);
OnTriggerXXX(Collider);</code></pre>
<h1 id="raycast">Raycast</h1>
<p>레이저를 쏴서 충돌을 확인하는 것이다.</p>
<pre><code class="language-cs">// target의 정보가 필요할 시 RaycastHit의 out param으로 받아오는 오버로딩
Physics.Raycast(transform.position, transform.forward, out RaycastHit target, 10)

// LayerMask를 이용하여 충돌판단.
Physics.Raycast(transform.position, transform.down, 10f, enemyMask);</code></pre>
<p>처럼 Raycast는 다양한 상황에서 쓸 수 있도록 설계해 두었다.</p>
<p>하지만 항상 판단하는 것은 부담이 되기때문에 적절할때에 사용하는 것이 중요하다.</p>
<h2 id="gizmos">Gizmos</h2>
<p>화면에 Gizmos를 그리는 메시지 함수이다.</p>
<pre><code class="language-cs">private void OnDrawGizmos()
{
    Gizmos.color = Color.green;
    Gizmos.DrawLine(transform.position, transform.position + transform.forward * 10);
}</code></pre>
<p>처럼 선을 그릴 수도 있고 다양한 도형을 그릴수도 있는 함수이다.</p>
<p>게임시작 전 씬 화면에서 보이기 떄문에 테스트에 유용하게 사용된다.</p>