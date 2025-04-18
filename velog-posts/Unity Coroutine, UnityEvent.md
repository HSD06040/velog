<ul>
<li><p>Unity에서는 함수를 일시정지하고 나중에 다시 이어서 실행할 수 있는 유용하고 범용적인 기능이다.</p>
</li>
<li><p>yield 키워드를 사용한다. yield 키워드 사용시 사용이 일시 정지되고 다른 코드가 실행된다.</p>
</li>
<li><p>또는 Unity에게 제어권을 반납하고 정지가 해제되고 난 뒤 중단점부터 다시 실행된다.</p>
</li>
<li><p>비동기적인 처리같지만 코루틴은 라이프 사이클 내에서 직렬적으로 일을 처리한다. (동기)</p>
</li>
</ul>
<ul>
<li><p>코루틴은 클래스로 이루어진 객체이다. 즉 인스턴스가 메모리에 할당된다. (스레드 아님)
(WaitForSecond도 인스턴스이다.)</p>
</li>
<li><p>그럼 코루틴을 실행할때마다 새로운 코루틴을 실행하여 여러개의 같은 루틴이 돌아가는 문제가 생긴다.</p>
</li>
<li><p>그리고 계속 실행되면 그만큼 가비지가 쌓이게 되고 부담이 걸린다는 뜻이다.</p>
</li>
<li><p>해결방법은 필드에서 선언 후 사용하는 것이다. 그럼 새로운 객체를 사용할 필요가 없다.</p>
</li>
<li><p>또한 Coroutine에 실행중에 코루틴을 넣어 실행, 미실행에 대한 판단이 가능해진다.</p>
</li>
</ul>
<h1 id="coroutine의-행동">Coroutine의 행동</h1>
<ul>
<li><p>Coroutine과 Update는 동시에 돌아가는 것처럼 보이지만 동시에 돌아가지 않는다. (병렬성이 없음.)</p>
</li>
<li><p>유니티는 기본적으로 싱글 스레드 방식으로 작동하기 때문에 당연한 결과이다.</p>
</li>
</ul>
<h2 id="yield-키워드">yield 키워드</h2>
<ul>
<li><p>코루틴이 등록된 코루틴을 확인한 후 있다면 Update후 yield의 delay를 보고 대기시간이 지났다면 다음이 실행되는 식으로 동작한다.</p>
</li>
<li><p>이 처럼 실제로는 분할된 순차 실행하는 것을 시분할 시스템이라고 한다.</p>
</li>
</ul>
<h2 id="왜-싱글스레드로-쓰냐">왜 싱글스레드로 쓰냐</h2>
<ul>
<li><p>멀티스레드로 사용 시 공유자원 문제가 발생한다.</p>
</li>
<li><p>즉 같은자원을 쓰는 스레드들이 시간차이로 인해 예상외의 결과가 나타난다는 것이다.</p>
</li>
<li><p>불러오기 -&gt; 바꾸기 -&gt; 저장 등을 멀티스레드로 동시에 처리하게되면 충돌의 가능성이 있다.</p>
</li>
</ul>
<p>이런 문제때문에 게임엔진은 기본적으로는 싱글스레드로 작동한다.(Unity LifeCycle)</p>
<h2 id="wait">Wait</h2>
<p>ForSeconds</p>
<ul>
<li>매개변수 초간 대기</li>
</ul>
<p>ForSecondsRealtime</p>
<ul>
<li>tiemScale등에 영향을 받지않는 초 대기</li>
</ul>
<p>ForFixedUpdate</p>
<ul>
<li>FixedUpdate가 끝날때 까지 대기</li>
</ul>
<p>ForEndOfFrame</p>
<ul>
<li>프레임이 끝날때까지 대기 (LateUpdate 다음)</li>
</ul>
<p>Until</p>
<pre><code class="language-cs">yield return new WaitUntil(() =&gt; Input.GetKeyDown(KeyCode.XXX));</code></pre>
<ul>
<li>입력까지 대기</li>
</ul>
<h1 id="unityevent">UnityEvent</h1>
<p>Unity에서 지원하는 이벤트이다.</p>
<p>실제 Unity의 UI 버튼도 Unity이벤트을 사용하고있다.</p>
<ul>
<li><p>인스펙터 창에서 손쉽게 수정이 가능하다.</p>
</li>
<li><p>인스펙터 창에서는 매개변수가 일치하지않아도 등록이 가능하다.</p>
</li>
<li><p>+= 와는 다르게 AddListener로 추가한다.</p>
</li>
<li><p>비 개발자와의 협업이거나 따로 확장성을 구사할때 사용한다.</p>
</li>
<li><p>일반 Action보다는 느리다.</p>
</li>
<li><p>이벤트 테스트 에도 먼저 UnityEvent로 간단히 테스트 후 코딩을 하는것도 좋은 방안인 것 같다.</p>
</li>
</ul>
<pre><code class="language-cs">public UnityEvent uEvent = new(); // 생성

uEvnet.AddListener(); // 등록
uEvnet.RemoveListener(); // 해제

uEvent.Invoke(); // 실행</code></pre>
<h3 id="dynamic과-static">Dynamic과 Static</h3>
<p>Dynamic은 값을 자동으로 할당한다.</p>
<p>Static은 직접 값을 할당할 수 있다.</p>