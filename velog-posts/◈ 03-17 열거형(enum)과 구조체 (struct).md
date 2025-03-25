<h1 id="❖-열거형이란">❖ 열거형이란?</h1>
<blockquote>
<p>숫자에 별명을 붙혀 더 쉽게 구분하기 위해 쓰인다.</p>
</blockquote>
<ul>
<li>우리가 1 = 한손 2 = 두손 이라고 정했다고 하자. 본인은 이걸알고 용할 수 있다.
하지만 다른 사람이 코드를 보았을 때에는 명확하지않아서 혼동을 일으킬 수 있다.
여기서 문제점을 해결 하는 것이 바로 열거혐 (enum)이다.</li>
</ul>
<p>enum은 1,2처럼 쓸 필요 없이 별명을 쓸 수 있도록한다.</p>
<h1 id="❖-열거형의-사용방법">❖ 열거형의 사용방법</h1>
<blockquote>
<p>포켓몬의 타입으로 예시를 들겠다.</p>
</blockquote>
<pre><code class="language-cs">enum Type
{
    Fire,
    Water
}
static void Main(string[] args)
{
    Type[] type = { Type.Fire, Type.Water };

    Console.WriteLine(TypeDamage(type[1], type[0]));

    float TypeDamage(Type myType,Type enemyType)
    {
        float totalDamageMultiply = 1;

        if(myType == Type.Water)
        {
            if(enemyType == Type.Fire)
            {
                totalDamageMultiply *= 2;
            }
        }
        if (myType == Type.Fire)
        {
            if (enemyType == Type.Water)
            {
                totalDamageMultiply *= .5f;
            }
        }
        return totalDamageMultiply;
    }
}</code></pre>
<ul>
<li><p>이런식으로 불 물 타입을 열거형으로 선언하여 약점데미지 계산식을 짜면 가독성이 좋아지고 실수가 나오지않는다.</p>
</li>
<li><p>물론 불 = 1, 물 = 2 이런식으로 작성하여도 작동은 하나 가독성이 심하게 떨어지고 실수가 나오기 쉽다.</p>
</li>
<li><p>숫자로 이루어져 있어 열겨형에서 숫자를 변경 가능하다.</p>
</li>
</ul>
<h1 id="❖-형-변환">❖ 형 변환</h1>
<pre><code class="language-cs">enum Type
{
    Fire,
    Water
    Size
}

Type type = (Type)1; //물
Type type = (Type)0; //불</code></pre>
<p>원래 숫자이기에 코드 처럼 형 변환이 가능하다.</p>
<ul>
<li><p>하지만 지금은 1번까지 밖에없는데 갑자기 5번이 나오면 예산치 못한 상황을 마주할 수 있다.</p>
<pre><code class="language-cs">Enum.IsDefined(typeof(Type),5); // Type에 7번이 있는지 검사하고 bool 타입으로 반환</code></pre>
</li>
<li><p>IsDefined를 사용하면 미리 검사 한 후 있다면 사용하는 식으로 안전하게 사용이 가능하다.</p>
</li>
<li><p>지금은 타입이 2개인데 0 1 이다. 그래서 Size를 추가해서
Size를 정수로 형 변환 해서 타입의 갯수를 얻을 수도 있다.</p>
</li>
</ul>
<h1 id="❖-구조체란">❖ 구조체란?</h1>
<blockquote>
<p>여러 자료형들을 모두 묶어서 하나를 만들 수 있다.</p>
</blockquote>
<p>예를 들어 포켓몬이 있다면 그 포켓몬이 가져야할 것들을 만들 수 있다.</p>
<pre><code class="language-cs">struct Pokemon
{
    public string name;
    public int number;
    public Type[] types;

    public float attack;
    public float defense;
    public float speed;
}

static void Main(string[] args)
{
    Pokemon pikachu = new Pokemon();

    pikachu.name = &quot;피카츄&quot;;
    pikachu.number = 25;
    pikachu.types = new Type[] { Type.Thunder };
    pikachu.attack = 55f;
    pikachu.defense = 40f;
    pikachu.speed = 90f;
}</code></pre>
<p>처럼 포켓몬이 가질 수 있는 정보들을 한번에 담을 수 있다.</p>
<ul>
<li><p>이런 형태로 제작한 후 필요한 객체에 사용한다면 재활용성이 높고 코드가 간략해지며 확장하기에도 용이하다.</p>
</li>
<li><p>가져와서 구조체를 생성하고 생성한 구조체안에서 정보들을 꺼내와 수정하거나 사용하면 된다.</p>
</li>
</ul>