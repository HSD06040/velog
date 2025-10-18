<h2 id="플레이어와-무기로-알아보는-zenject-di">플레이어와 무기로 알아보는 Zenject (DI)</h2>
<p>WeaponFactory 코드</p>
<pre><code class="language-cs">public class WeaponFactory : IWeaponDataFactory
{
    private Dictionary&lt;string, WeaponData&gt; _weaponDataMap;

    public WeaponFactory(List&lt;WeaponData&gt; allWeaponData)
    {
        _weaponDataMap = new Dictionary&lt;string, WeaponData&gt;();
        foreach (var data in allWeaponData)
        {
            _weaponDataMap[data.weaponName] = data;
        }
    }

    public WeaponData GetWeaponData(string weaponId)
    {
        return _weaponDataMap.TryGetValue(weaponId, out var data) ? data : null;
    }
}</code></pre>
<p>MonoInstaller 코드</p>
<pre><code class="language-cs">public class GameInstaller : MonoInstaller
{
    public List&lt;WeaponData&gt; allWeaponData;

    public override void InstallBindings()
    {
        // IWeaponDataFactory 타입을 WeaponFactory 구현체로 바인딩
        // .AsSingle() : 컨테이너에서 하나의 싱글톤 인스턴스로 관리됨 (같은 컨테이너에서 Resolve 시 동일 인스턴스 반환)
        // .WithArguments(allWeaponData) : 생성자 인자로 allWeaponData 리스트를 전달하여 WeaponFactory를 생성
        Container.Bind&lt;IWeaponDataFactory&gt;()
            .To&lt;WeaponFactory&gt;()
            .AsSingle()
            .WithArguments(allWeaponData);
    }
}</code></pre>
<p>Player 코드</p>
<pre><code class="language-cs">public class Player : MonoBehaviour
{
    [Inject] // DI
    private IWeaponDataFactory _weaponDataFactory; // 대상
    private WeaponData currentWeapon;
    [SerializeField] string _weaponId; // 테스트 코드

    private void Update()
    {
        if (Input.GetKeyDown(KeyCode.A))
            EquipWeapon(_weaponId);
    }

    public void EquipWeapon(string weaponId)
    {
        currentWeapon = _weaponDataFactory.GetWeaponData(weaponId);
        Debug.Log($&quot;장착한 무기 : {currentWeapon.weaponName} 무기 데미지 : {currentWeapon.damage}&quot;);
    }
}</code></pre>
<h3 id="결과">결과 <img alt="" src="https://velog.velcdn.com/images/hsd0604/post/bd9eecfc-78f5-4075-8644-8570c66d1a29/image.png" /></h3>
<ul>
<li>정상적인 주입확인 Null안뜸</li>
</ul>
<p>ProjectContext : Context가 알고 있거나 관리하는 객체에게 주입가능
SceneContext : 씬에서 자동으로 찾아 주입시킴</p>
<p>이여서 ProjectContext는 Inject가 안되었는데 이쪽에 대한  이해가 더 필요해보임</p>