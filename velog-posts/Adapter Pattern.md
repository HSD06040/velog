<h1 id="adapter-pattern">Adapter pattern</h1>
<ul>
<li><p>코드가 너무 오래되어 레거시 코드가 된 경우, 코드의 의도를 알아보기 힘들고 섣불리 수정을 할 수 없다.</p>
</li>
<li><p>이때 어댑터패턴을 사용하면 기존의 코드를 수정없이 다형성을 확보할 수 있다.
(즉 개방폐쇠 원칙을 지킨다.)</p>
</li>
</ul>
<p>하지만</p>
<ul>
<li>중간객체를 거쳐야해서 약간의 성능저하가 발생한다.</li>
</ul>
<h2 id="사용-사례">사용 사례</h2>
<ul>
<li><p>코드의 변경이 불가능하거나 업데이트를 통해 기존 코드의 내용 변경이 우려될 때</p>
</li>
<li><p>레거시 코드임에도 그 코드가 효율적일때 쓰인다.</p>
</li>
</ul>
<h1 id="사용-예시">사용 예시</h1>
<h2 id="setting">Setting</h2>
<p>인터페이스와 제네릭 어뎁터</p>
<pre><code class="language-cs">public interface IAdapter
{
    public void Attack();
}

public abstract class Adapter&lt;T&gt; : IAdapter
{
    protected T Adaptee;

    public Adapter(T t)
    {
        Adaptee = t;
    }

    public abstract void Attack();
}</code></pre>
<h2 id="adaptee">Adaptee</h2>
<p>호환할 객체</p>
<pre><code class="language-cs">public class X
{
    public int damage;
    public float moveSpeed;

    public void x(int damage)
    {

    }
}

public class Y
{
    public int damage;
    public float moveSpeed;

    public void y(float moveSpeed)
    {

    }
}</code></pre>
<p>x y의 매개변수가 달라도 호환이 가능하다.</p>
<h2 id="adapter">Adapter</h2>
<p>호환시켜주는 객체</p>
<pre><code class="language-cs">public class XApdater : Adapter&lt;X&gt;
{
    public XApdater(X t) : base(t)
    {
    }

    public override void Attack()
    {
        Adaptee.x(Adaptee.damage);
    }
}

public class YApdater : Adapter&lt;Y&gt;
{
    public YApdater(Y t) : base(t)
    {
    }

    public override void Attack()
    {
        Adaptee.y(Adaptee.moveSpeed);
    }
}</code></pre>
<ul>
<li><p>Attack은 IApdater이고 </p>
</li>
<li><p>x와 y 어댑터는 인터페이스 구현에 호환할 자신의 기능을 넣어</p>
</li>
<li><p>결국 IAdapter로 두 기능이 호환되었다.</p>
</li>
</ul>
<h2 id="사용">사용</h2>
<pre><code class="language-cs">public class AdapterManager
{
    List&lt;IAdapter&gt; adapters = new List&lt;IAdapter&gt;();

    XApdater x = new XApdater(new X());
    YApdater y = new YApdater(new Y());

    public void AllAdapters()
    {
        adapters.Add(x); adapters.Add(y);

        foreach (IAdapter adapter in adapters)
        {
            adapter.Attack();
        }
    }
}</code></pre>
<p>그 객체가 아닌 어댑터로 호환해서 어댑터를 써야한다.
(어댑터에만 인터페이스가 존재하기 떄문)</p>
<pre><code class="language-cs">public class WeaponManager : MonoBehaviour
{
    IWeaponAttack currentWeapon;

    private void Start()
    {
        SetupWeapon(new XApdater(new X()));
    }

    private void SetupWeapon(IWeaponAttack weapon)
    {
        currentWeapon = weapon;
    }

    public void Attack()
    {
        currentWeapon.Attack();
    }
}</code></pre>
<p>무기의 Attack이 모두 다르고 수정이 불가능한 상황일때에</p>
<p>어댑터를 만들고 인터페이스(Attck)을 현재무기로 가지게한다.</p>
<p>그리고 Attack을 하면 그  무기의 Attck(어댑터된)이 실행되도록 하게 할 수 있다.</p>
<ul>
<li><p>즉 Attack 기능들을 IWeaponAttack으로 묶고</p>
</li>
<li><p>WeaponManager는 간단히 Attack만 실행하면 되도록 구조를 만들 수 있다.</p>
</li>
</ul>
<h1 id="unityevent-활용">UnityEvent 활용</h1>
<pre><code class="language-cs">public interface IDamagable
{
    public void TakeDamage(GameObject dealer, int damage);
}

public abstract class DamageAdapter : MonoBehaviour, IDamagable
{
    public UnityEvent&lt;GameObject, int&gt; OnDamaged;

    public void TakeDamage(GameObject dealer, int damage)
    {
        OnDamaged?.Invoke(dealer, damage);
    }
}</code></pre>
<p>UnityEvent를 활용하면 Editor내에서 손쉽게 수정이 가능하다.</p>
<p>예를 들어 맞았을때에는 무조건 TakeDamage만 실행하게 하고</p>
<p>Adapter를 만들고 UnityEvent를 Editor에 설정해주면 그 기능은 무슨 기능이든 맞았을때 호출된다.
(즉 TakeDamage하나로 객체마다 다른 행동 가능 (다형성 확보))</p>
<ul>
<li><p>그냥 Editor에서만 수정하면 되고 어댑터의 재활용성도 좋고</p>
</li>
<li><p>기존 코드의 수정도 없어서 정말 좋은 구조인것 같다.</p>
<h1 id="정리">정리</h1>
</li>
<li><p>어댑터 패턴은 유지보수적으로 매우 우수하다.</p>
</li>
<li><p>SOLID의 개방 폐쇠 원칙을 지키는 패턴이다.</p>
</li>
<li><p>원래의 코드의 수정없이 재활용이 가능하다.</p>
</li>
<li><p>원래의 코드들을 같은 카테고리로 호환시켜 사용할때에 사용한다.</p>
</li>
<li><p>하나의 객체만 그 인터페이스를 호환하도록 할 수 있다.</p>
</li>
</ul>
<p>하지만</p>
<ul>
<li><p>어댑터가 많아질수록 복잡성과 스크립트 관리의 어려움이 증가하고</p>
</li>
<li><p>중간객체를 거쳐야해서 성능상으로는 조금의 손해가 발생한다.</p>
</li>
<li><p>중간객체를 꼭 거쳐야하는데 해당 스크립트만을 보고 어떻게 동작하는지 확인하기 힘들다.
인스펙터를 보아야만 확인이 가능하다.</p>
</li>
</ul>
<p>그래서 사실 보통처럼 Interface를 포함시켜서 사용하는것이 가장 좋은 설계이다.</p>
<ul>
<li><p>즉 어쩔 수 없이 코드수정이 불가능할 경우에 사용하는 것이 좋은 패턴이다.</p>
</li>
<li><p>그래서 보통 플레이어의 상호작용과 같은 곳에서 인터페이스를 넘겨주고 
받는쪽 ( 몬스터, Npc )에서 어댑터를 사용하는 식으로 한다.</p>
</li>
</ul>