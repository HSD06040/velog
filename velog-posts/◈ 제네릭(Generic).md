<h1 id="❖-object">❖ object</h1>
<blockquote>
<p>모든 클래스의 기본이 되는 자료형이다. 즉 모든 객체는 object를 상속받고 있다.</p>
</blockquote>
<ul>
<li><p>object 형식을 사용하면 타입에 상관없이 할당이 가능하다. (상속의 개념)</p>
</li>
<li><p>object는 크기를 특정할 수 없다. 그래서 힙에만 저장 가능하다.</p>
</li>
</ul>
<h3 id="✧-박싱과-언박싱">✧ 박싱과 언박싱</h3>
<blockquote>
<p>값 형식과 참조 형식간의 변환이다.</p>
</blockquote>
<pre><code class="language-cs">int integer = 1;

//스텍에 있는 값타입 integer 값이 힙에 할당된다.(박싱)
object obj = integer;

// 힙에 있는 integer 값를 스텍으로 다시 가져온다.(언박싱)
int i = (int)obj;</code></pre>
<p><strong>✧ 박싱 (Boxing)</strong></p>
<ul>
<li><p>스텍에 있는 데이터를 힙에 할당하는 것을 박싱이라고 한다.</p>
</li>
<li><p>int는 값 형식인데 참조형식인 object에 넣는다.</p>
</li>
<li><p>그럼 즉 값을 힙에 생성하고 그 값을 가르키는 주소를 obj에 넣는다는 것이고 값 형식이 참조 형식이 되는 것이다.</p>
</li>
</ul>
<p><strong>✧ 언박싱 (Unboxing)</strong></p>
<ul>
<li>힙에 있는 데이터를 스텍에 다시 할당하는 것을 언박싱이라고 한다.</li>
<li>명시적 변환이 필요하다.</li>
<li>다운 캐스팅과 비슷하게 언박싱을 사용 시 에는 주의가 필요하다.</li>
</ul>
<h4 id="✧문제✧">!✧문제✧!</h4>
<p><strong>✧ 박싱</strong></p>
<ul>
<li>object 자체가 힙영역에 있어 object에 저장될 시에는 힙에 메모리를 할당하고 새로운 object를 생성한다. </li>
<li>즉 박싱할 때마다 힙 메모리를 게속 사용하게된다.</li>
<li>애초에 힙 할당속도 자체가 느리다.</li>
</ul>
<p><strong>✧ 언박싱</strong></p>
<ul>
<li>문제가 더 있다.</li>
<li>힙에 있는 데이터를 가져와서 스텍에 다시 복사하는 과정을 거친다.</li>
<li>이는 더 추가적인 CPU연산이 필요하다는 것이다.</li>
<li>그리고 잘못된 캐스팅으로 예외 발생 가능성이 있다.</li>
</ul>
<p>또한</p>
<ul>
<li><p>GC는 메모리 사용량이 특정 임계값을 초과할때 실행되는데
박싱을 사용하게 되면 힙 메모리 사용량이 증가하게되고 이는 GC의 실행 빈도 증가로 이어질 수 있다.</p>
</li>
<li><p>GC가 많이 실행되면 GC는 사용하지않는 객체를 스캔하는데, 이 과정에서 CPU 자원을 소모하여 앱 응답성 저하를 일으킬 수 있다.</p>
</li>
</ul>
<h1 id="❖-제네릭이란">❖ 제네릭이란?</h1>
<blockquote>
<p>일반화라는 뜻을 가지며 다양한 타입을 담을 수 있는 것이 제네릭이다.</p>
</blockquote>
<p>제네릭은 object를 피하기위해 사용되고 박싱 언박싱이 없다.</p>
<pre><code class="language-cs">public class Inventory&lt;T&gt;
{
    List&lt;T&gt; list;

    public void Test&lt;T1&gt;(T1 test)
    {
        T1 t = test;
    }
}</code></pre>
<ul>
<li>와 같이 뒤에 &lt; T &gt; 처럼 선언하면 T라는 타입을 쓸 수 있다.</li>
<li>이 T 타입은 무슨 타입이던지 들어올 수 있어 유연한 대처가 가능하다.</li>
</ul>
<p>하지만 우리가 T를 사용해 기능을 만들때에는 의도가 분명 존재할 것이다.</p>
<ul>
<li>예를 들어 이 기능은 class를 위한 기능인데 어떤 class든 들어오게 하고 싶다 같은거 말이다.</li>
</ul>
<pre><code class="language-cs">public class Inventory&lt;T&gt; where T : class
{
    List&lt;T&gt; list;
}</code></pre>
<pre><code class="language-cs">Test&lt; class타입 &gt;();
// 으로 메서드 실행가능하지만 &lt; class타입 &gt;가 없어도 자동으로 실행가능</code></pre>
<p>이때는 뒤에 where T : class 와 같이 붙혀 사용한다.</p>
<ul>
<li><p>where T : class 는 T는 class 타입이 여야 한다는 제약을 건다는 것이다.</p>
</li>
<li><p>이 처럼 기능의 의도에 따라 어떤 타입만을 받도록 제한할 수가 있다.
(특정 클래스로 제약을 거는 것 또한 가능하다.)</p>
</li>
</ul>
<p>인터페이스 또한 제한으로 가능한데 인터페이스는 너무 다양해서 한가지를 예시로 들자면</p>
<pre><code class="language-cs">public class Test
{
    public static T Bigger&lt;T&gt;(T t1, T t2) where T : IComparable&lt;T&gt;
    {
        T t3 = t1.CompareTo(t2) &gt; 0? t1 : t2;
        return t3;
    }
}</code></pre>
<p>Bigger 메서드는 둘중 더 큰 숫자를 리턴하는 식으로 가능하다.</p>
<ul>
<li><p>원래 같으면 비교는 불가능한데 T는 IComparable 에 충족하는 경우만을 받을 수 있기때문에 비교가 가능해진다.</p>
</li>
<li><p>IComparable은 비교가 가능한 자료형에 붙는 인터페이스인데</p>
</li>
<li><p>이 인터페이스가 있다면 무조건 비교를 하는 기능을 구현해야하기 때문에 T가 이 인스터페이스 만을 받으면 비교가 가능해지는 것이다.</p>
</li>
</ul>