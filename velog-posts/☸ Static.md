<h1 id="✤-static이란">✤ Static이란?</h1>
<blockquote>
<p>메모리 Data 영역에 할당되며 프로그램이 실행될 때 동안 공간에 항상 존재한며 자동 할당해제 되지 않는 것이 static이다.</p>
</blockquote>
<ul>
<li><p>정적이라는 개념이다. 즉 움직이지 않고 고정 되어있는 것이 static이다.</p>
</li>
<li><p>static은 최초 호출 시점부터 존재한다. 최초 호출 시 static 생성자가 실행된다.</p>
</li>
<li><p>static 클래스는 인스턴스를 생성할 수 없다.</p>
</li>
<li><p>static은 변수와 함수를 저장하는 공간을 제공한다.</p>
</li>
<li><p>static은 실행 중 항상 존재한다고 했다. 즉 클래스의 변수나 기능을 저장하는 공간을 항상 제공한다.</p>
</li>
<li><p>이러한 이유로 static 키워드를 사용하면 &quot;어디에서나 전역적인 접근이 가능하다&quot; 라는 편리한 장점이 있다.</p>
</li>
<li><p>보통 프로그램에서 자주 다뤄지는 GameManager같은 것에 쓰인다.(싱글톤 패턴)</p>
</li>
</ul>
<h3 id="✧-이러한-장점들이-있는-static-이지만-주의가-필요하다">✧ 이러한 장점들이 있는 Static 이지만 주의가 필요하다.</h3>
<blockquote>
<ul>
<li>프로그램이 실행되는 동안 항상 존재한다는 것은 항상 고정된 메모리를 차지하고 있다는 것이다.(메모리 상주)</li>
</ul>
</blockquote>
<ul>
<li>그래서 너무 많은 static 사용으로 불필요한 고정 메모리가 늘어나고 프로그램 자체가 무거워질 우려가 있다.</li>
</ul>
<p><strong>static의 어디에서나 접근가능한 특징 때문에 유지보수 측면에서 좋지않다.</strong></p>
<blockquote>
<ul>
<li>어디에서나 쓴다는 것은 쓰는 구간이 정해져있지 않다는 것이고 그만큼 많이 쓴다는 것이다.</li>
</ul>
</blockquote>
<ul>
<li>그래서 이 static을 수정해야 한다면 static을 쓴 만큼 프로그램의 수정이 이루어져야 할 수도 있다.</li>
</ul>
<p><strong>디버깅의 어려움이 생길 수도 있다.</strong></p>
<blockquote>
<ul>
<li>static이 정확히 어디에서 사용되었는지 알기 어렵다.</li>
</ul>
</blockquote>
<p><strong>프로그램 자체가 static에 너무 의존하는 상황이 생길 수도 있다.</strong></p>
<h2 id="✦-static-함수">✦ static 함수</h2>
<blockquote>
<ul>
<li>static 함수는 static 변수와 다르게 Data영역에 할당되지 않는다.</li>
</ul>
</blockquote>
<ul>
<li>static 함수는 일반 함수와 같이 Code영역에 저장된다.</li>
</ul>
<p><strong>✧ 그럼 왜 함수와 다르게 전역적으로 접근이 가능해질까?</strong></p>
<ul>
<li><p>static 이라는 키워드 자체가 함수에 소속 된다는 것을 의미하기도 하기 때문이다.</p>
</li>
<li><p>일반 함수는 인스턴스와 연결되어있어 클래스의 인스턴스를 생성해 주어야하지만</p>
</li>
<li><p>static 함수는 클래스 자체에 소속되어 있기 때문에 전역적으로 접근이 가능하다.</p>
</li>
</ul>
<p>static 함수는 멤버변수 사용이 불가능하다.</p>
<ul>
<li><p>static은 어디서든 접근이 가능해야 하기 때문에 다른 클래스의 멤버변수가 초기화 되었는지의 유무를 판단 할 수 없기 때문이다.</p>
</li>
<li><p>즉 메모리 측면에서는 일반 함수와 차이가 없고 사용방식의 차이 이다.</p>
</li>
</ul>
<h3 id="✧-사용방식">✧ 사용방식</h3>
<blockquote>
<p>static 함수는 멤버 변수를 사용할 수 없다. 즉 멤버 변수가 필요한 함수에는 적합하지않다.</p>
</blockquote>
<pre><code class="language-cs"></code></pre>
<h2 id="✦-static-클래스">✦ static 클래스</h2>
<ul>
<li><p>static 클래스를 선언하는 이유는 그 안에 static으로만 선언 하겠다는 것이다.</p>
</li>
<li><p>static 클래스는 인스턴스 생성이 불가능하다.</p>
</li>
<li><p>일반 클래스로 만들어진 static 기능들을 모아둔 FunctuinLibrary 클래스가 있다고 한다면</p>
</li>
<li><p>다른 사람들을 이걸 보고 인스턴스 생성해서 사용할 가능성이 있다.</p>
</li>
<li><p>하지만 static의 집합이기 때문에 인스턴스를 생성할 필요가 없는데 생성하게 되면 Heap영역에 불필요한 메모리가 생기는 것과 같다.</p>
</li>
<li><p>그래서 static클래스로 선언해 두어 인스턴스의 생성을 막고 기능들을 쉽게 불러올 수 있도록 한다.</p>
</li>
</ul>
<h2 id="private-public-static">private public static</h2>
<p>private static</p>
<ul>
<li>외부에서 접근이 불가능한 static.</li>
<li>보통 내부데이터를 관리하거나 보조함수로 활용한다.</li>
</ul>
<p>public static</p>
<ul>
<li>외부에서 접근이 가능한 static.</li>
<li>보통 전역으로 많이쓰는 GameManager같은 객체에서 활용한다.</li>
</ul>