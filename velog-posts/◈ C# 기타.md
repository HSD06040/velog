<h1 id="❖-c">❖ C#</h1>
<p>C#은 계속해서 업데이트 되고 있으며 새롭고 편한 기능을 꾸준히 제공하려 하고있다.
그래서 바뀌는 기능도 존재한다.</p>
<h1 id="❖-파라미터">❖ 파라미터</h1>
<p><strong>✧ Named Parameter</strong></p>
<ul>
<li>함수(level:3,name:&quot;이름&quot;) 처럼 매개변수의 순서를 신경쓰지 않고 써줄 수 있다.
(별로 안쓸듯)</li>
</ul>
<p><strong>✧ Default Parameter</strong></p>
<ul>
<li>함수의 매개변수를 입력하지 않아도 되는 기본 값을 입력할 수 있다.
함수(string name, level = 1)
처럼 선언하면 level에 아무것도 입력하지 않을 시에는 기본적으로 1을 넘긴다.</li>
</ul>
<p><strong>✧ params</strong></p>
<ul>
<li>함수가 매개변수로 배열을 받을때 사용해 주면 함수호출 시 값을 넘길때</li>
<li>배열을 넘기지 않고 (1,2,3) 이런 식으로 나열하여 넘겨주는 것이 가능하다.</li>
</ul>
<p><strong>✧ out Parameter</strong></p>
<ul>
<li>추가적인 반환값이 필요할 때에 사용이 가능하다.</li>
</ul>
<p><strong>✧ in Parameter</strong></p>
<ul>
<li>입력 전용으로 함수에서 변경이 되지않고 그저 입력값으로만 가능한 파라미터이다.</li>
<li>값을 보장한다는 것이다.</li>
<li>특이한 것은 값이 보장된다는 것 때문에 &quot;원본&quot;을 준다. 즉 &quot;참조&quot;이다.</li>
</ul>
<p><strong>✧ ref</strong></p>
<ul>
<li>원본 전달이 가능하도록 하는 파라미터이다.</li>
<li>한수로 원본의 값을 변경하도록 할때에 사용한다.</li>
</ul>
<p><strong>✧ partial</strong></p>
<ul>
<li>분할해서 구현하는 방법이다.</li>
<li>같이 협업하는 상황에서 작업이 겹치지않도록 분리해서 관리해주도록 해준다.</li>
<li>세이브 데이터같은 작업이 무조건 겹치는 경우에 쓰인다.</li>
</ul>
<p><strong>✧ Nullable</strong></p>
<ul>
<li>nullError를 방지하기 위해 != null 같이 nullCheck를 하는데</li>
<li>?.함수() 로 사용이 가능한다.<pre><code class="language-cs">if(Event != null)
{
  Event(); //있다면 실행
}
</code></pre>
</li>
</ul>
<p>Event?.Invoke(); // 있다면 실행</p>
<pre><code>만약 Event가 null이 아니라면 실행한다. 라는 코드를 축소한 것이다. (실행결과는 똑같다.)

**✧ 삼항 연산자**

```cs
private bool isAlive = hp &gt; 0? true : false;

private int i = value % 2 == 0? 1 : 0; // 짝수라면 1 아니라면 0</code></pre><ul>
<li>hp가 0보다 크다면 true 아니라면 false로 작성된다.
즉 조건 ? 참일때 : 거짓일때 로 작성한다.</li>
</ul>
<p><strong>✧ Operator Overoding</strong></p>
<pre><code class="language-cs">struct Position
{
    public int x;
    public int y;

    public static Position operator +(Position left,Position right)
    {
        int x = left.x + right.x;
        int y = left.y + right.y;
        return new Position() {x = x, y = y};
    }
}</code></pre>
<p>라고 재정의 해주어 구조체 안에서 구조체의 더하는 식을 만들어 주는 것이다.</p>
<ul>
<li>이런 구조체의 덧셈이 많이 사용될때 재활용성을 높이기 위해 사용된다.</li>
</ul>
<p><strong>✧ Indexer</strong></p>
<pre><code class="language-cs">class MyCollection
{
    private int[] data = new int[10];

    public int this[int index]
    {
        get
        {
            if(index &lt; 0 || index &gt;= data.Length)
                throw new IndexOutOfRangeException(&quot;인덱스 범위를 벗어났습니다.&quot;);
            return data[index];
        }
        set
        {
            if(index &lt; 0 || index &gt;= data.Length)
                throw new IndexOutOfRangeException(&quot;인덱스 범위를 벗어났습니다.&quot;);
            data[index] = value;
        }
    }
}

class Program
{
    static void Main()
    {
        MyCollection collection = new MyCollection();

        // 인덱서를 사용해 값을 설정 (set)
        collection[0] = 100;
        collection[1] = 200;

        // 인덱서를 사용해 값을 가져옴 (get)
        Console.WriteLine(collection[0]);  // 100 출력
        Console.WriteLine(collection[1]);  // 200 출력
    }
}</code></pre>
<p>클래스를 배열의 대괄호 처럼 사용이 가능하다.</p>
<ul>
<li>다양한 타입을 넣을 수 있기에 get 할때 대괄호를 추가하여 어떤 아이템을 가져올 것인지 정하기 같이 사용이 가능하다.</li>
<li>반대로 set으로 하면 그 값을 집어넣기도 가능하다.</li>
</ul>
<p>불변성</p>
<ul>
<li><p>불변객체는 이름처럼 변하지않는다는 객체이다.</p>
</li>
<li><p>불변객체들은 변하지 않기때문에 변경이 들어올때 기존의 데이터를 삭제하고 들어온 변경값으로 새롭게 객체를 생성한다.</p>
</li>
<li><p>이러한 동작들은 컴퓨터에 부하를 주게 되는데 그 이유는 삭제와 생성이 빈번하게 일어나게 되면 GC의 빈도도 같이 늘어나기 때문이다.</p>
<h1 id="❖-속성-property">❖ 속성 Property</h1>
<pre><code class="language-cs">public int hp { get; private set; }</code></pre>
<p>public으로 선언된 hp를 외부에서 바꿀 수 있다면 예상치 못한 상황이 일어날 수 있다.
그래서 private로 선언하기에는 이 값을 다른곳에서 읽고는 싶을때 사용하는 것이 property이다.</p>
</li>
</ul>
<p><strong>✧ get</strong></p>
<ul>
<li>가져오는 것에는 제한이 없다는 것이다.</li>
</ul>
<p><strong>✧ private set</strong></p>
<ul>
<li>이 변수를 바꾸는 것은 외부에서 막겠다는 것이다.</li>
</ul>
<p>만 있는 것이 아니라 다양하게 사용이 가능한데</p>
<pre><code class="language-cs">[SerializeField] int hp;

public int HP
{
    get =&gt; hp;
    set
    {
        hp = value;
        HPChanged?.Invoke(value);
        if(hp &gt; 1000)
        {
            ;             // 디버깅용
        }
    }
}</code></pre>
<ul>
<li>처럼 체력이 set 될때 이벤트 실행과 같이도 가능하고 여기에서 중간에 조건을 걸어 나중에 브레이크 포인트를 이용해 디버깅 할때에도 도움이 된다. (참고로 value는 들어온 값이다.)</li>
</ul>
<p>변수의 변경을 외부에서 제한하는 등으로 굉장히 많이 사용된다.</p>
<pre><code class="language-cs">public int damage =&gt; level * baseDamage;</code></pre>
<p>간단한 경우 위와 같이 프로퍼티를 선언 가능하다.</p>