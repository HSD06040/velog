<p>Unity의 Object 구조 때문에 그렇다.</p>
<p>Unity는 Destroy된 객체를 바로 삭제하지 않는다.</p>
<p>예를 들어보자</p>
<pre><code class="language-cs">Destroy(Component)

if(Component == null)</code></pre>
<p>여기서는 당연히 조건이 true로 나온다. 하지만 이는 Unity가 처리하는 FakeNull이란 것 덕분이다.</p>
<p>Unity는 가비지 컬렉터가 힙을 관리하며 Destroy된 객체는 삭제 리스트에 올라가 가비지 컬렉팅이 되면 그때 삭제된다. </p>
<p>그래서 사실 Component는 Null이 아니지만 삭제된 것처럼 보이도록 내부적으로 되어있다.</p>
<h2 id="unityengineobject를-타고가-보면-그-답이-나온다">UnityEngine.Object를 타고가 보면 그 답이 나온다.</h2>
<p>Object (Mono, GameObject, Component 등)은 커스텀 연산자를 오버로딩하여 가지고 있다.</p>
<p>여기서 중요한건 C#의 연산자가 아닌 커스텀 연산자라는 것이다.</p>
<p>이 커스텀 연산자 덕분에 아직 객체를 남겨두어도 Null을 판별할 수 있는것이다.</p>
<p>Action.Invoke()는 Unity의 연산자를 쓰지않고 C#으로 실행된다.</p>
<p>즉 오버로딩된걸 사용하지않고 직접 MethodPointer를 타고 들어가서 함수를 실행한다.</p>
<p>그러다가 Unity에서 Destroy된 객체여서 예외가 발생하는 것이다.</p>