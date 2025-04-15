<h1 id="component와-gameobject">Component와 GameObject</h1>
<blockquote>
<p>Component는 부품이고 GameObject는 많은 Component로 이루어진 객체이다.</p>
</blockquote>
<h2 id="component">Component</h2>
<p>Unity에서는 기본적으로 MonoBehaviour을 사용하며 이는 Component를 상속받고 있다.</p>
<pre><code class="language-cs">go.GetComponent&lt;T&gt;(); // go의 컴포넌트를 순회해서 T 가져오기
go.GetComponentInParent&lt;T&gt;(); // go의 부모를 순회해서 T 컴포넌트 가져오기
go.GetComponentInChiled&lt;T&gt;(); // go의 자식을 순회해서 T 컴포넌트 가져오기</code></pre>
<p>와 같이 다양하게 가져올 수 있다.</p>
<h2 id="gameobject">GameObject</h2>
<p>기본적으로 씬에 존재하는 모든 오브젝트를 말한다.</p>
<p>반드시 Transform을 가지고있다.</p>
<h1 id="프리팹">프리팹</h1>
<blockquote>
<p>객체의 원본을 Assets 형태로 저장해서 씬에 언제든 넣을 수 있도록한다.</p>
</blockquote>
<ul>
<li><p>이러한 Assets형태로 저장된다는 특징때문에 쓸 수 있는 반경이 늘어난다.
예를 들어 Resources.Load를 프리팹이 있는 경로로 해서 생성할때 경로만으로 생성할 수 있다는 것이 있다.
즉 동적으로 생성이 가능하다는 것이다.</p>
</li>
<li><p>씬에 필요할 때에만 로드할 수 있어서 경량화에도 도움이된다.</p>
</li>
<li><p>원본을 복사한 객체들을 계속 생성할 수 있어 재사용성이 뛰어나다.</p>
</li>
<li><p>그리고 원본을 수정하면 씬에 있는 복사본들이 수정되기 때문에 변경에도 용이하다.</p>
</li>
<li><p>Unity의 특성상 meta파일이 생성되어서 수정이 가능하긴하나 충돌이 생기기때문에 협업시에는 하지않는 것이 바람직하다.</p>
</li>
<li><p>Unity에서는 프리팹을 직렬화로 저장이 되어있다. 프리팹 Instance라는 별도로 관리되며
프리팹 원본 ID를 가지고있어 ID로 형태를 만든다는 형식이다.</p>
</li>
</ul>
<h1 id="생성과-삭제">생성과 삭제</h1>
<h2 id="instantiate">Instantiate</h2>
<p>Unity에서 런타임에 객체를 생성하는 함수이다.</p>
<pre><code class="language-cs">Instantiate(original);
Instantiate(original, position, rotation);
Instantiate(original, parent);
Instantiate(original, position, rotation, parent);</code></pre>
<p>처럼 다양한 오버로딩이 있다.</p>
<h2 id="destroy">Destroy</h2>
<p>Unity에서 런타임에 객체를 삭제하는 함수이다.</p>
<pre><code class="language-cs">Destroy(object);
Destroy(object, time); // time(초) 후 삭제</code></pre>
<p>처럼 몇초 후 삭제등도 가능하다.</p>