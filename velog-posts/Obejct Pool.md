<ul>
<li>Unity에는 생성, 삭제와 같은 힙에서 이루어지는 연산은 많은 비용이 들어간다.
그리고 그 중심에는 가비지 컬렉터(GC)가 존재한다.</li>
</ul>
<h1 id="object-pooling-이란">Object Pooling 이란?</h1>
<p>오브젝트를 미리 Pool에 담아둔뒤 외부에서 오브젝트가 필요하다면 꺼내서 빌려가는 형식이다.</p>
<p>그 후 다쓴 오브젝트는 삭제되는것이 아닌 다시 Pool에 저장하는 식으로 구성한다.</p>
<p>만약 Pool에 충분한 객체가 없을때에는 객체를 생성한다.
또한 Pool의 용량을 초과한 객체는 삭제한다.</p>
<h1 id="사용이유">사용이유</h1>
<ul>
<li><p>게임을 하다보면 생성, 삭제가 많이 일어날 수 밖에 없는 기능들이 존재한다.
하지만 힙에 올리고 내리는 이런 행위들은 꽤나 무거운 작업이다.</p>
</li>
<li><p>객체가 생성 될때에는 메모리할당, 초기화, 리소스로드 등의 작업이 이루어지고
객체가 파괴될때에는 가비지 컬렉팅으로 인한 프레임드랍이 발생한다.</p>
</li>
</ul>
<h2 id="가비지-컬렉팅으로-인한-프레임드랍">가비지 컬렉팅으로 인한 프레임드랍</h2>
<ul>
<li><p>Destroy로 오브젝트를 파괴할때 곧바로 메모리에서 사라지는 것이 아니다.
게임 내에서 보이지 않을뿐 GC가 수거하여 파괴하기 전까지는 힙에 메모리가 남아있게된다.</p>
</li>
<li><p>GC는 일정 임계치가 넘어가거나, 주기마다 메모리확보를 위해 가비지컬렉팅을 하는데 이때 삭제될 오브젝트(Destory된)
메모리에서 삭제하게된다. 그리고 이 양이 많다면 당연히 시간이 오래걸리게 된다.</p>
<p>-컴퓨터는 이미 게임로직, 렌더링등의 연산을 하고있다. 이 상태에서 많은 양의 오브젝트의 생성,삭제가 이루어 진다면
많은 메모리들을 정리하기위해 프레임이 떨어지는 문제가 발생한다.
(GC가 동작할때 프레임이 떨어지거나 잠시 게임이 멈추는 현상이 있다.) // GC 스파이크</p>
</li>
</ul>
<h2 id="메모리의-파편화">메모리의 파편화</h2>
<ul>
<li><p>메모리는 수납공간이라고 생각하면 편하다.</p>
</li>
<li><p>ABCD의 크키가 각각 4이고 16이라는 수납공간에 넣어두었다고 쳐보자.</p>
</li>
<li><p>그리고 A와 C를 수납공간에서 빼보자. 그러면 이제 수납공간에 넣을 수 있는 크기는 8인가?</p>
</li>
<li><p>반은 맞고 반은 틀렸다. 정답은 4와 4이다. 즉 ABCD에서 AC가 빠지면  []B[]D라는 수납공간이 완성되고</p>
</li>
<li><p>8크기의 물건을 넣을려고하면 크기가 맞지않아 넣을 수 없다.</p>
</li>
<li><p>즉 공간은 8만큼 존재하지만 연결된 공간이 8만큼 존재하지않아 넣을 수 없는 역설적인 상황이 일어난다.</p>
</li>
<li><p>이는 오브젝트의 생성과도 연결지을 수 있다. 즉 오브젝트를 생성할 수 없는 상황이 일어날 수 있다.</p>
</li>
</ul>
<h1 id="사용방법">사용방법</h1>
<p>Unity에서 2021.1 버전부터 Pool을 지원한다.</p>
<pre><code class="language-cs">Dictionary&lt;string, ObjectPool&lt;GameObject&gt;&gt; poolDic;

private void CreatePool(string name, GameObject prefab, int maxSize)
{
    ObjectPool&lt;GameObject&gt; pool = new ObjectPool&lt;GameObject&gt;
        (
        createFunc: () =&gt;
        {
            GameObject obj = Instantiate(prefab);
            obj.name = name;
            return obj;
        },

        actionOnGet: (GameObject obj) =&gt;
        {
            obj.SetActive(true);
        },

        actionOnRelease: (GameObject obj) =&gt;
        {
            obj.SetActive(false);
        },

        actionOnDestroy: (GameObject obj) =&gt;
        {
            Destroy(obj);
        },
        maxSize: maxSize
        );
    poolDic.Add(name, pool);
}

public T Get&lt;T&gt;(T original, Vector3 position, Quaternion rotation, Transform parent) where T : Object
{
    string name = original.name;

    GameObject obj = poolDic[name].Get();

    return obj as T;
}</code></pre>
<p>오브젝트 풀을 여러개로 관리하기 위한 간단한 코드이다.</p>
<p>ObjectPool은 실제 Object를 보관할 Pool이고 CreatePool로 만들어준다.</p>
<h1 id="주의점">주의점</h1>
<p>미리 만들어 둔다는 방식이니 사용중이지 않을 때에도 메모리 공간을 차지한다.</p>
<p>GC가 공간이 적을때에 남은 작은 공간을 활용하기 위해 반복해서 오고가는 경우가 생길 수 있다.</p>
<p>즉 적당한 최대 용량을 정하는 것이 좋다.</p>