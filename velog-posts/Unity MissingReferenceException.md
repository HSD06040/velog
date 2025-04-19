<h1 id="문제-상황">문제 상황</h1>
<p>오늘 Scene을 Additive 방식으로 비동기 로딩 언로딩 하고
오브젝트의 정보를 담아 전에 열었던 상자면 열어져있도록 하게 하는 과정에서 </p>
<p>MissingReferenceException 에러를 마주하였다.</p>
<h1 id="missingreferenceexception">MissingReferenceException</h1>
<p>Destroy된 객체에 접근하면 나오는 에러이다.</p>
<p>나는 이걸 Action 이벤트에서 접하였다.</p>
<h1 id="왜-action에서-발생했는가">왜 Action에서 발생했는가</h1>
<p>정확히는 Action에서만 발생한 것이 아닌 Unity의 Object 구조 때문에 그렇다.</p>
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
<h1 id="그럼-왜-nullcheck이-이상했는가">그럼 왜 NullCheck이 이상했는가?</h1>
<p>Action의 동작방식에 있다.</p>
<p>Action.Invoke()는 Unity의 연산자를 쓰지않고 C#으로 실행된다.</p>
<p>즉 오버로딩된걸 사용하지않고 직접 MethodPointer를 타고 들어가서 함수를 실행한다.</p>
<p>그러다가 Unity에서 Destroy된 객체여서 예외가 발생하는 것이다.</p>
<h1 id="해결방법">해결방법</h1>
<p>Action와 try catch 를 이용해서 예외가 발생하면 Action에서 제외하는 식으로 해결하였다.</p>
<pre><code class="language-cs">for (int i = onLoadedSceneActions.Count - 1; i &gt;= 0; i--)
{
    Action act = onLoadedSceneActions[i];
    try
    {
        act?.Invoke();
    }
    catch (MissingReferenceException e)    // 예외 발생
    {
        Debug.LogWarning($&quot;Removed destroyed listener: {e.Message}&quot;);
        onLoadedSceneActions.RemoveAt(i);    // 예외 삭제
    }
}</code></pre>
<p>다른 해결방법으로는 UnityEvent는 Unity에서 내부적으로 관리해서 해결이 가능할 것 같다. </p>
<p>UnityAction은 그냥 함수 참조이기 떄문에 똑같은 에러가 발생할 것이다. (같은 delegate)</p>