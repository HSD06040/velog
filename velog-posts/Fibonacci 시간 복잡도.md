<p>Fibonacci의 현재값은 이전 두 항의 합이다.</p>
<h3 id="방법-1--재귀함수">방법 1 : 재귀함수</h3>
<pre><code class="language-cs">int Fibonacci(int n) {
    if (n == 0) return 0;
    if (n == 1) return 1;
    return Fibonacci(n - 1) + Fibonacci(n - 2);
}</code></pre>
<p>동작은 하지만 비효율적이다.</p>
<h4 id="왜-비효율적일까">왜 비효율적일까?</h4>
<p>매번 f(n-1)과 f(n-2)의 값을 또 구하기위해 다시 계산해야한다.</p>
<pre><code class="language-cs">F(5)
├── F(4)
│   ├── F(3)
│   │   ├── F(2)
│   │   └── F(1)
│   └── F(2)
└── F(3)
    ├── F(2)
    └── F(1)</code></pre>
<p>5만 되어도 3이 2개 중복, 2가 3개 중복이다.</p>
<h4 id="시간-복잡도는-o2n으로-아주-느리다">시간 복잡도는 O(2^n)으로 아주 느리다.</h4>
<h4 id="공간-복잡도는-on이다">공간 복잡도는 O(n)이다.</h4>
<h3 id="방법-2--단순-반복-캐싱">방법 2 : 단순 반복 캐싱</h3>
<pre><code class="language-cs">    if (n == 0 || n == 1) return n;

    int f0 = 0;
    int f1 = 1;
    int f2 = 0;

    // 처음 값으로 계속 재사용 하면서 계산
    for (int i = 2; i &lt;= n; i ++)
    {
        f2 = f0+f2;
        f0 = f1;    //순서 중요
        f1 = f2;
    }</code></pre>
<h4 id="시간-복잡도는-on으로-비교적-빠르다">시간 복잡도는 O(n)으로 비교적 빠르다.</h4>
<h4 id="공간-복잡도는-on이다-1">공간 복잡도는 O(n)이다.</h4>