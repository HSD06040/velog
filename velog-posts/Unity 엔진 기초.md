<h1 id="게임-엔진">게임 엔진</h1>
<ul>
<li><p>예전에는 무조건 게임을 만들기위해 엔진을 만든 후 게임제작을 하였지만</p>
</li>
<li><p>사용화된 엔진(Unity, Unreal, Havok) 등의 엔진등의 등장으로 인해 게임개발이 훨씬 용이해졌다.</p>
</li>
</ul>
<h2 id="사용하는-이유">사용하는 이유</h2>
<ul>
<li><p>개발시간이 단축된다.
게임 개발에 필요한 물리, 시간, 그래픽, 렌더링 등의 작업을 이미 제공해주기 떄문에 시간이 단축된다.</p>
</li>
<li><p>비용이 상대적으로 낮다.
직접 게임엔진을 만들려고하면 큰 비용이 든다.그리고 인재구하기 측면에서도 비용이 낮아진다.</p>
</li>
<li><p>상대적으로 진입장벽이 낮아졌다.
정해진 틀이 이미 만들어져있기 때문에 개발자 입장에서의 진입장벽도 낮아졌고,
게임 회사에서도 직접 게임 엔진개발을 하지않고도 게임을 만들 수 있으니 게임 회사 입장에서도 진입장벽이 낮아졌다.</p>
</li>
<li><p>신기술 도입이 빠르다.
상용 엔진회사는 게임엔진만을 전문적으로 만들기 떄문에 신기술을 엔진에 적용하는 시간이 비교적 짧다.</p>
</li>
</ul>
<p>GUI</p>
<ul>
<li>직관적이며 조작하기 쉬운 </li>
</ul>
<p>UnityHub</p>
<p>Build</p>
<p>간편한 .Net의 통합 빌드 적용</p>
<p>사용화된 엔진이라 자료의 확보가 용이하다.</p>
<h2 id="unity-interface">Unity Interface</h2>
<p>씬</p>
<ul>
<li>게임오브젝트를 배치할 수 있는 실제 게임을 제작하는 화면이다,</li>
</ul>
<p>게임</p>
<ul>
<li>실제 게임화면이 보이는 창 (카메라가 바라보는 화면)</li>
</ul>
<p>하이어라키</p>
<ul>
<li>계층창으로 폴더의 계층구조처럼 부모 자식의 개념이 있는 창이다.</li>
<li>씬에 배치된 모든 오브젝트가 보인다.</li>
</ul>
<p>인스펙터</p>
<ul>
<li>게임오브젝트들의 설정을 변경할 수 있는 창이다. (컴포넌트 부착 과 값 변경 등)</li>
<li>직렬화된 데이터를 설정가능한데 pulbic은 기본 직렬화이고</li>
<li>만약 Private인데 직렬화를 하고 싶다면 [ SerializeField ] 어트리뷰트를 사용하면 직렬화가 가능하다.</li>
</ul>
<p>프로젝트</p>
<ul>
<li>프로젝트에 Assets과 Import한 Pakages의 폴더들을 볼 수 있거나 관리할 수 있는 창이다.</li>
</ul>
<p>콘솔</p>
<ul>
<li>Debug Log 출력등을 확인 할 수 있는 창이다.</li>
</ul>
<p>LayOut</p>
<ul>
<li>인터페이스의 구성을 저장 및 불러올 수 있는 기능이다. 편집하기 쉽게 구성한뒤 저장 후 사용한다,
(애니메이션, 디버그, 라이팅 등)</li>
</ul>
<h1 id="unity-lifecycle">Unity LifeCycle</h1>
<p><img alt="" src="https://velog.velcdn.com/images/hsd0604/post/b596169f-b660-47b9-8dff-c61422f10d12/image.png" /></p>
<p>유니티에는 정해진 LifeCycle이 존재한다. (메시지 함수)</p>
<p>여기서 Awake와 Start는 오브젝트의 생명주기 동안 단 한번만 실행된다.</p>
<p>Awake</p>
<ul>
<li>객체가 초기화 될때 실행되는 함수이다. 컴포넌트가 비활성화 되어있어도 동작한다.
(보통 컴포넌트의 초기화에 쓰인다.)</li>
</ul>
<p>OnEnable</p>
<ul>
<li>객체가 활성화 될때 호출된다.</li>
</ul>
<p>Start</p>
<ul>
<li>객체가 시작되기전에 실행되는 함수이다. 컴포넌트가 비활성화일 경우에는 실행되지않는다.</li>
</ul>
<p>FixedUpdate</p>
<ul>
<li>정해진 주기마다 호출된다. 물리연산에 적합하다. (기본 0.02초)</li>
</ul>
<p>OnTrigger</p>
<ul>
<li>객체가 충돌되었을때 호출된다 (isTrriger == true)</li>
</ul>
<p>OnCollision</p>
<ul>
<li>객체가 충돌되었을때 호출된다 (isTrriger == false)</li>
</ul>
<p>Update</p>
<ul>
<li>매 프레임마다 호출되는 함수이다.</li>
</ul>
<p>LateUpdate</p>
<ul>
<li>Update가 끝나고 한번 호출된다.</li>
</ul>
<p>OnDisable</p>
<ul>
<li>객체가 비활성화될때 호출된다.</li>
</ul>
<p>OnDestroy</p>
<ul>
<li>객체가 삭제될때 호출된다.</li>
</ul>