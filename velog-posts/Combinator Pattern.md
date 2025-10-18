<h2 id="combinator-pattern이란">Combinator Pattern이란?</h2>
<p>Combinator Pattern의 개념은 “작은 함수를 합성해서 더 큰 함수를 만든다” 이다.
if문을 여러개 쓰지 않고 함수들을 이어 조합함으로 써
코드를 간결히 사용할 수도 있는 패턴이다.</p>
<p>예를 들어 Target이 있다고 하면 </p>
<ol>
<li>해당 타겟은 범위내에 들어와 있는가?</li>
<li>해당 타겟은 살아 있는가?</li>
</ol>
<p>등을 if문이 아닌 and, or, not으로 연결해서 더 복작합 규칙을 만들 수 있다.</p>
<p>Combinator는 값을 변경하거나, 행동 순서를 정할수도 있고
나아가 B Tree 전체를 구성하는 것도 가능하다.</p>
<h3 id="leaves">Leaves</h3>
<p>조건들을 정의한다.</p>
<pre><code class="language-cs">// ---------- Leaves ----------
readonly struct InRange : IPred
{
    // Inlining 메서드
    // 메서드 호출 오버헤드 감소: 일반적인 메서드 호출은 스택 프레임을 설정하고, 인수를 전달하고, 점프하는 등의 추가적인 작업(오버헤드)을 발생시킨다.
    // 성능 향상: 메서드 본문을 호출 지점에 직접 삽입(인라인) 하면 이러한 호출 단계를 건너뛰어 실행 속도를 높이는 효과를 얻을 수 있다.
    // 이 메서드가 호출 될때마다
    // 함수 자체를 호출하는 것이 아니라
    // 메서드 본문을 호출자에 직접 붙혀넣도록(Inline 하도록) 컴파일러에게 지시한다.
    // 이는 Unreal의 Inline이랑 본질적으로 동일하다.
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public bool Test(Enemy e, in TargetCtx c) =&gt; (e.transform.position - c.origin).sqrMagnitude &lt;= c.r2;
}

readonly struct IsAlive : IPred
{
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public bool Test(Enemy e, in TargetCtx c) =&gt; e.health.healthAmount &gt; 0;    
}

readonly struct LineOfSight : IPred
{
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public bool Test(Enemy e, in TargetCtx c) =&gt; !Physics.Linecast(c.origin, e.transform.position, c.los);    
}</code></pre>
<p>함수를 Inline하여 사용한다. 이러한 함수들은 추후 Conbinator들이 받아 사용하게 될 것이다.</p>
<h3 id="combinators">Combinators</h3>
<pre><code class="language-cs">// ---------- Combinators ----------
readonly struct Not&lt;A&gt; : IPred
    where A : struct, IPred
{
    public readonly A a;    

    public Not(A a)
    {
        this.a = a;       
    }

    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public bool Test(Enemy e, in TargetCtx c) =&gt; !a.Test(e, c);
}

readonly struct Or&lt;A, B&gt; : IPred
    where A : struct, IPred
    where B : struct, IPred
{
    public readonly A a;
    public readonly B b;

    public Or(A a, B b)
    {
        this.a = a;
        this.b = b;
    }

    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public bool Test(Enemy e, in TargetCtx c) =&gt; a.Test(e, c) || b.Test(e, c);
}

readonly struct And&lt;A, B&gt; : IPred 
    where A : struct, IPred 
    where B : struct, IPred
{
    public readonly A a;
    public readonly B b;

    public And(A a, B b)
    {
        this.a = a;
        this.b = b;
    }

    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public bool Test(Enemy e, in TargetCtx c) =&gt; a.Test(e, c) &amp;&amp; b.Test(e, c);    
}</code></pre>
<p>조건들을 And, Or, Not로 판단한다.</p>
<h3 id="identities">Identities</h3>
<pre><code class="language-cs">// ---------- Identities (항등 원소) ----------

// 항상 true를 반환하는 기본 Predicate
// 논리식 시작 시 기본값으로 사용 (항등원 역할)
readonly struct TruePred : IPred
{
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public readonly bool Test(Enemy e, in TargetCtx c) =&gt; true;

    // Singleton 인스턴스 (불필요한 생성 방지)
    public static readonly TruePred Instance = default;
}

// 항상 false를 반환하는 Predicate
// Or 체인 시작 시 기본값으로 사용
readonly struct FalsePred : IPred
{
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public readonly bool Test(Enemy e, in TargetCtx c) =&gt; false;
}</code></pre>
<h3 id="bulider-chain">Bulider Chain</h3>
<pre><code class="language-cs">// ---------- Chain ----------
// 빌더에서 사용되는 체인 래퍼 구조체.
// .And(), .Or(), .Not() 등을 통해 논리식을 조합함.
readonly struct Chain&lt;TPred&gt; where TPred : struct, IPred
{
    public readonly TPred p;

    public Chain(TPred p)
    {
        this.p = p;
    }

    // &amp;&amp;
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public Chain&lt;And&lt;TPred, TNext&gt;&gt; And&lt;TNext&gt;(TNext n) where TNext : struct, IPred =&gt; new(new And&lt;TPred, TNext&gt;(p, n));

    // ||
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public Chain&lt;Or&lt;TPred, TNext&gt;&gt; Or&lt;TNext&gt;(TNext n) where TNext : struct, IPred =&gt; new(new Or&lt;TPred, TNext&gt;(p, n));

    // !
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public Chain&lt;Not&lt;TPred&gt;&gt; Not() =&gt; new(new Not&lt;TPred&gt;(p));

    // (a &amp;&amp; b)
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public Chain&lt;And&lt;TPred, TSub&gt;&gt; AndGroup&lt;TSub&gt;(Func&lt;Chain&lt;TruePred&gt;, Chain&lt;TSub&gt;&gt; g) where TSub : struct, IPred
        =&gt; new(new And&lt;TPred, TSub&gt;(p, g(PredChain.All()).Build()));

    // (a || b)
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public Chain&lt;Or&lt;TPred, TSub&gt;&gt; OrGroup&lt;TSub&gt;(Func&lt;Chain&lt;TruePred&gt;, Chain&lt;TSub&gt;&gt; g) where TSub : struct, IPred
        =&gt; new (new Or&lt;TPred, TSub&gt;(p, g(PredChain.All()).Build()));

    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public TPred Build() =&gt; p;
}</code></pre>
<p>And, Or, Not을 사용하여 실제 조건들을 파악하고 조합하는 구조체</p>
<h3 id="builder-static-class">Builder static class</h3>
<pre><code class="language-cs">// ---------- Fluent builder ----------
// 체이닝으로 논리식을 조합할 수 있게 하는 빌더 클래스
static class PredChain
{
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public static Chain&lt;TLeaf&gt; Start&lt;TLeaf&gt;(TLeaf leaf) where TLeaf : struct, IPred =&gt; new (leaf);

    // 항상 true로 시작하는 체인 (기본값)
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public static Chain&lt;TruePred&gt; All() =&gt; new(TruePred.Instance);

    // 항상 false로 시작하는 체인 (기본값)
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public static Chain&lt;FalsePred&gt; Any() =&gt; new(new FalsePred());

    // 편의용 오버로드들
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public static Chain&lt;T1&gt; All&lt;T1&gt;(T1 a) where T1 : struct, IPred =&gt; new (a);

    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public static Chain&lt;And&lt;T1, T2&gt;&gt; All&lt;T1, T2&gt;(T1 a, T2 b) 
        where T1 : struct, IPred 
        where T2 : struct, IPred
        =&gt; new(new And&lt;T1, T2&gt;(a, b));

    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public static Chain&lt;And&lt;And&lt;T1, T2&gt;, T3&gt;&gt; All&lt;T1, T2, T3&gt;(T1 a, T2 b, T3 c)
        where T1 : struct, IPred
        where T2 : struct, IPred
        where T3 : struct, IPred
        =&gt; new(new And&lt;And&lt;T1, T2&gt;, T3&gt;(new And&lt;T1, T2&gt;(a,b), c));

    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    public static Chain&lt;Or&lt;T1, T2&gt;&gt; Any&lt;T1, T2&gt;(T1 a, T2 b)
        where T1 : struct, IPred
        where T2 : struct, IPred
        =&gt; new(new Or&lt;T1, T2&gt;(a, b));
}</code></pre>
<h2 id="example">Example</h2>
<pre><code class="language-cs">using CanTarget = And&lt;And&lt;InRange, IsAlive&gt;, LineOfSight&gt;; // 미리 고정되어 있다면 using문으로 정의

class Targeting : MonoBehaviour
{
    public Transform origin;
    public float range = 10;
    public LayerMask occluders;
    public List&lt;Enemy&gt; enemies = new();
    CanTarget pred;

    private void Awake()
    {
        pred = PredChain.
            Start(new InRange()).
            And(new IsAlive()).
            And(new LineOfSight()).
            Build();

        var filterA = PredChain.Start(new IsAlive()).And(new InRange()).And(new LineOfSight()).Build();

        // InRange &amp;&amp; !(IsAlive &amp;&amp; LOS)
        var filterB = PredChain.All(new InRange()).AndGroup(g =&gt; g.And(new IsAlive()).And(new LineOfSight()).Not()).Build();

        // (InRange || LOS) &amp;&amp; IsAlive
        var filterC = PredChain.Start(new InRange()).Or(new LineOfSight()).And(new IsAlive()).Build(); 
    }

    void Update()
    {        
        var ctx = new TargetCtx(origin.position, range, occluders);

        for (int i = 0; i &lt; enemies.Count; i++)
        {
            var e = enemies[i];

            if (e == null) continue;

            if (pred.Test(e, ctx))
                Debug.Log(&quot;조건 충족&quot;);
            else
                Debug.Log(&quot;조건 미충족&quot;);
        }
    }
}</code></pre>
<h2 id="정리">정리</h2>
<p>결국 현재 구조는 구조체들을 조합하는 구조체를 Tree형태로 가져가 최종적으로
And&lt;And&lt;InRange, IsAlive&gt;, LineOfSight&gt; 와 같은 복잡할 수 있는 구조체를 만들고
해당 구조체를 판단하여 true false를 return 할수 있는 구조이다.</p>
<p>이러면 사용할때 더욱 가독성 좋게 사용가능하고 구조체를 사용하기 때문에 GC이슈도 없다.</p>
<p>참고영상 : <a href="https://www.youtube.com/watch?v=pPHdF2r6TmU&amp;list=WL&amp;index=5">https://www.youtube.com/watch?v=pPHdF2r6TmU&amp;list=WL&amp;index=5</a></p>