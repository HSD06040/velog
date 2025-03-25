<h1 id="❖-앞서">❖ 앞서</h1>
<ul>
<li><p>싱글톤은 &quot;안티패턴&quot;으로 분류되기는 하나 (SOLID의 D를 지키지 않기 때문)</p>
</li>
<li><p>디자인 패턴이 그러하듯 단점이 있는 것일뿐 어떻게 사용하느냐가 중요하다.</p>
</li>
</ul>
<h1 id="❖-싱글톤-패턴이란">❖ 싱글톤 패턴이란?</h1>
<blockquote>
<p>하나의 인스턴스만 보장된다는 디자인패턴이다.
즉 하나의 인스턴스로만 작동한다는 뜻이다.
<img alt="업로드중.." src="blob:https://velog.io/0fc79fff-92c5-4b04-988c-c8a324d005ae" /></p>
</blockquote>
<p>싱글톤 패턴의 기본 형식이다.
자기 자신을 static선언하여 참조한다.</p>
<pre><code class="language-cs">public static GameManager instance;


private void Awake()
{
    if (instance != null)
        Destroy(instance.gameObject);
    else
        instance = this;

    DontDestroyOnLoad(gameObject);
}</code></pre>
<p>이러면 어떤 현상이 일어나냐?</p>
<ul>
<li><p>다른 스크립트에서 GameManager.instance로 바로 접근이 가능하다.</p>
</li>
<li><p>따로 참조없이 바로 사용할 수 있다는 것이 싱글톤의 큰 장점이다.</p>
</li>
</ul>
<h3 id="어떻게-가능할까">어떻게 가능할까?</h3>
<blockquote>
<p>static키워드 때문이다</p>
</blockquote>
<ul>
<li><p>static은 클래스자체에 소속되어있다.
즉 static 멤버(변수, 메서드) 클래스처럼 단 하나만 존재한다.</p>
</li>
<li><p>static으로 선언된 것들은 인스턴스가 없어도 접근이 가능하다.
그래서 싱글톤 패턴을 사용 할 때에는 static을
인스턴스화 해줌으로서 그안의 매서드나 함수를 쓸 수 있는 것이다.</p>
</li>
</ul>
<h1 id="❖-싱글톤을-쓰는-이유">❖ 싱글톤을 쓰는 이유</h1>
<blockquote>
<p>편하다.</p>
</blockquote>
<ul>
<li>따로 참조 필요없이 Class.instance에서 가져와 사용할 수 있기에 필요한 데이터를 가져오기 편하다.</li>
</ul>
<blockquote>
<p>메모리 누수가 줄어든다.</p>
</blockquote>
<ul>
<li>만약 똑같은 요청을 여러번 보내면 이 요청을 처리하기 위해 생성 소멸을 반복하며 메모리 누수가 발생한다.</li>
<li>하지만 싱글톤은 단 하나의 인스턴스만 보장을 하고 하나의 인스턴스가 전부 처리하기에 메모리 누수가 줄어든다.</li>
</ul>
<h1 id="❖-싱글톤은-왜-안티패턴인가">❖ 싱글톤은 왜 안티패턴인가?</h1>
<ul>
<li><p>테스트
인스턴스 값이 변경되고 유지되는 경우  다음 테스트에 영향을 끼친다.</p>
</li>
<li><p>유지보수 및 디버깅
아무곳에서나 사용할 수 있다보니 어디서 어떤 오류가 났는지 찾기 힘들다.</p>
</li>
<li><p>커플링이 발생한다.
즉 의존성이 생긴다. 의존역전 원칙을 위배한다.</p>
</li>
</ul>
<p>이러한 이유때문에 싱글톤의 무분별한 사용은 권장하지않는다.</p>
<h1 id="❖-싱글톤의-구현">❖ 싱글톤의 구현</h1>
<p>싱글톤은 하이어라키에 올리는 것과 그러지 않은게 있다.
싱글톤은 단점을 극복하기위해 여러시도를 통해 여러 형태를 가지게 되었다.</p>
<h2 id="✧-lazy-싱글톤-lazy-initialization">✧ Lazy 싱글톤 (Lazy Initialization)</h2>
<blockquote>
<p>사용할 때 생성하는 방법</p>
</blockquote>
<pre><code class="language-cs">private static Singleton instance;

private Singleton() {} //외부 생성 막음

public static Singleton
{
    get{
           if(instance == null)
            instance = new Singleton();

         return instance;
        }
}</code></pre>
<ul>
<li><p>원래의 싱글톤은 그 싱글톤을 사용하지 않더라도 이미 생성되어있는
이른 초기화(Eager Initialization)으로 작성된다.</p>
</li>
<li><p>사용할 때에 생성하면 메모리를 절약할 수 있다는 장점이 있다.</p>
</li>
</ul>
<p>하지만 멀티스레드 환경에서는 안전하지 못하다.</p>
<ul>
<li><p>첫번째 스레드가 instance == null을 판단한 후 없다면
instance = new Singleton(); 를 실행한다.</p>
</li>
<li><p>정확히는 new Singleton();으로 생성하고 그 값을 instance에 넣는다는 순서이다.</p>
</li>
<li><p>하지만 첫번째 스레드가 instance를 만드는 중이고 아직 instance의 값에 넣지 않았을 때</p>
</li>
<li><p>두번째 스레드가 instance == null을 검사 했다면 결국 2개의 인스턴스가 생성 된다는 문제점이 있다.</p>
</li>
</ul>
<h2 id="✧-thread-safe-싱글톤">✧ Thread-Safe 싱글톤</h2>
<blockquote>
<p>멀티스레드 환경에서 안전한 싱글톤이다.</p>
</blockquote>
<pre><code class="language-cs">private static Singleton instance;
private static readonly object lockObject = new object();

private Singleton() {} //외부 생성 막음

public static Singleton
{
    lock(lockObject)
    {
        if (instance == null)
            instance = new Singleton();
    }
    return instance;
}</code></pre>
<p>lock을 활용해서 스레드 세이프를 보장한다.</p>
<ul>
<li><p>lock
여러 스레드가 접근하지 못하도록 막는 키워드.</p>
</li>
<li><p>그래도 아직 불안하다. 왜 불안하냐면
CPU는 메모리에서 정보를 가져올때 캐시를 통한다.</p>
</li>
<li><p>캐시를 통할때 아주 재수없게 캐시미스가 일어나면 이 코드에서도
인스턴스가 2개 생성될 가능성이 있다.</p>
</li>
</ul>
<p>그래서</p>
<h2 id="✧-lazy-initializationthread-safe-싱글톤">✧ Lazy Initialization,Thread-Safe 싱글톤</h2>
<blockquote>
<p>사용될 때에 생성되면서 멀티스레드로 부터 안전한 싱글톤이다.</p>
</blockquote>
<pre><code class="language-cs">using UnityEngine;

public class SimpleSingleton
{
    private static volatile SimpleSingleton instance = null; //싱글톤을 저장할 instance
    private static readonly object padlock = new object(); // 스레드 간의 동기화를 위한 lock객체

    public static SimpleSingleton Instance
    {
        get
        {
            if(instance == null)
            {
                lock (padlock)
                {
                    if (instance == null)
                    {
                        instance = new SimpleSingleton();
                    }
                }
            }
            return instance;
        }
    }
}</code></pre>
<ul>
<li>volatile
멀티스레드를 사용한다. 라는 뜻이라 보면된다.
(모든 스레드가 변수를 MainMemory에 로드할 수 있도록 강제성을 부여하는 키워드)
CPU캐시 최적화로인해 발생할 수 있는 동기화 문제를 방지한다.</li>
</ul>
<p>싱글톤을 사용할때 생성됩니다.</p>
<h2 id="✧-제네릭-싱글톤-하이어라키">✧ 제네릭 싱글톤 (하이어라키)</h2>
<blockquote>
<p>코드가 좀 길다</p>
</blockquote>
<pre><code class="language-cs">using UnityEngine;

public class Singleton&lt;T&gt; : MonoBehaviour where T : Component
{
    private static T instance; //저장할 instance
    public static T Instance
    {
        get
        {
            if(instance == null) // 없다면
            {
                instance = (T)FindObjectOfType(typeof(T)); // 싱글톤을 찾음
            }
            if(instance == null) //그래도 없다면
            {
                SetupInstance(); // 생성
            }
            return instance; // 반환
        }
    }

    private void Awake()
    {
        RemoveDuplicates();
    }

      private void RemoveDuplicates()
    {
        if (instance == null)
        {
            instance = this as T; // 현재 instance T타입으로 변환
            DontDestroyOnLoad(this.gameObject);
        }
        else
        {
            Destroy(gameObject);
        }
    }

    private static void SetupInstance()
    {
        instance = (T)FindObjectOfType(typeof(T)); // T타입의 싱글톤을 찾음

        if(instance == null) // 없다면
        {
            GameObject gameObj = new GameObject(); //게임 오브젝트 생성
            gameObj.name = typeof(T).Name; // 이름을 설정
            instance = gameObj.AddComponent&lt;T&gt;(); //게임 오브젝트에 컴포넌트 추가
            DontDestroyOnLoad(gameObj);
        }
    }
}
</code></pre>
<p>제네릭을 사용해서 싱글톤 객체들이 상속받아 코드의 사용을 줄일 수 있다.</p>
<ul>
<li>하이어라키에 올려두어야 하는 설계이기에 SetupInstance를 했을때
오브젝트를 만들고 컴포넌트를 넣는다.</li>
</ul>
<ul>
<li>이것만으로도 기본적인 싱글톤 패턴은 갖춰 졌기에 상속받아 사용하면 된다. </li>
</ul>
<ul>
<li>갑자기 멀티스레드환경에 놓였을 때에도 제네릭에서만 수정을 하면 모든 싱글톤이 수정된다.</li>
</ul>
<h1 id="❖-마무리">❖ 마무리</h1>
<ul>
<li><p>안티패턴으로 분류되긴하지만 엄연한 디자인패턴으로 분류되기 때문에 사용해도 된다.</p>
</li>
<li><p>앞서 다양한 싱글톤이 있는 것처럼 자신의 상황에 맞는 싱글톤은 어떤 것인지 고민해 보는 것이 좋다.</p>
</li>
<li><p>무분별한 사용은 권장하지 않는다. 유지보수가 힘들어지고 의존성이 강해진다.</p>
</li>
</ul>
<p>싱글톤은 단점은 분명히 존재하지만 장점이 크다고 생각한다.
나는 싱글톤이 필요할 때 고민은 하겠지만 결국 싱글톤을 쓸 것 같다.</p>