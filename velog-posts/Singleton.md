<blockquote>
<p>단 하나의 인스터스만 유지되며 지속적으로 존재 하도록하는 데이터 관리에 이점이 있는 패턴이다.</p>
</blockquote>
<ul>
<li>전역적인 접근이 가능하다.</li>
</ul>
<h1 id="구현">구현</h1>
<pre><code class="language-cs">public class Singleton : MonoBehaviour
{
    private static Singleton instance;
    public static Singleton Instance =&gt; instance;

    private void Awake()
    {
        if(instance == null)
            instance = this;
        else
            Destroy(instance);

        DontDestroyOnLoad(gameObject);
    }
}</code></pre>
<p>기본적으로는 이런식으로 구현된다.</p>
<h2 id="활용">활용</h2>
<p>GameManager</p>
<pre><code class="language-cs">public class GameManager : MonoBehaviour
{
    private static GameManager instance;
    public static GameManager Instance =&gt; instance;

    public int score {  get; private set; }

    private void Awake()
    {
        if (instance == null)
            instance = this;
        else
            Destroy(instance);

        DontDestroyOnLoad(gameObject);
    }

    public void AddScore(int amount) =&gt; score += amount;
}</code></pre>
<p>score을 관리하는 객체이고 </p>
<pre><code class="language-cs">GameManager.Instance.AddSocre(amount)</code></pre>
<p>의 형태로 호출하여 스코어를 변경한다.</p>
<h1 id="주의점">주의점</h1>
<ul>
<li>싱글톤이 로컬 오브젝트에 대해 종속성을 가지면 안된다.</li>
</ul>
<h1 id="정리">정리</h1>
<ul>
<li><p>싱글톤의 책임이 너무 많아지는 경우는 적합하지 않다. (단일 책임원칙 위반)</p>
</li>
<li><p>싱글톤은 코드의 결합도를 높인다.(커플링)</p>
</li>
<li><p>싱글톤은 단위 테스트를 하기 어렵게 한다.</p>
</li>
</ul>
<p>라는 단점들이 존재한다.</p>
<p>즉 너무 많은 싱글톤은 남용은 바람직하지 않고 하나만 존재하는 매니저 객체만을 판단해서 잘 사용하는 것이 중요한 패턴이다.</p>
<p>그 외 <a href="https://velog.io/@hsd0604/%EC%8B%B1%EA%B8%80%ED%86%A4Singleton-%ED%8C%A8%ED%84%B4">https://velog.io/@hsd0604/%EC%8B%B1%EA%B8%80%ED%86%A4Singleton-%ED%8C%A8%ED%84%B4</a></p>