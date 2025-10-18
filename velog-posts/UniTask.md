<p>UniTask는 코루틴과 많이 비교된다. 그도 그럴게 UniTask가 코루틴을 대체할 수 있고 성능적으로도 우수 하기 때문이다.</p>
<h1 id="코루틴과-unitask">코루틴과 UniTask</h1>
<p>코루틴은</p>
<ul>
<li>서버로부터 받은 값을 IEnumrator Funtion()에서 리턴할 수 없다.</li>
<li>StartCoroutine() 실행 시 GC 할당이 높다, 그리고 yield return wait을 할때에도
빈번한 할당이 일어난다.(그래서 사용할 경우 미리 캐싱해둔다.)</li>
</ul>
<p>UniTask는</p>
<ul>
<li>Struct 기반으로 제작되어 있어 Zero Allocation(힙 영역 할당 줄임)이 특징이다.</li>
<li>서버로 부터 받은 값을 Return할 수 있다.</li>
<li>UniTask.Yield, UniTask.Delay, UniTask.DelayFrame기능으로 코루틴을 대체한다.</li>
<li>메모루 누수를 방지라기 위한 TaskTracker창을 지원한다.</li>
<li>Task,await을 더 효율적이게 사용할 수 있도록 한다.</li>
</ul>
<h2 id="코루틴-대체">코루틴 대체</h2>
<p><img alt="" src="https://velog.velcdn.com/images/hsd0604/post/171fa665-657c-4599-8ae5-00533b2fa2af/image.png" />
사진처럼 기다리는 방식도 사용이 가능하다.</p>
<pre><code class="language-cs">public class UniTaskTest : MonoBehaviour
{
    private void Start()
    {
        WaitSecondAsync().Forget(); // Void 반환형인 경우 Forget을 추가작성
    }

    private async UniTaskVoid WaitSecondAsync()
    {
        await UniTask.Delay(TimeSpan.FromSeconds(1f));  // 1초
        Debug.Log(&quot;1초가 지났습니다.&quot;);
    }
}</code></pre>
<p>반환형이 Void라면 Forget을 달아주어야한다.(UniTask공식 문서 참고),(던지는 예외를 무시)
<img alt="" src="https://velog.velcdn.com/images/hsd0604/post/a59998f1-9b12-4a52-8532-4c9ce4c2c1e0/image.png" />
정상적으로 실행되는걸 알 수 있다.</p>
<h1 id="unitask">UniTask</h1>
<h2 id="delaytype">DelayType</h2>
<p>타임 스케일로 게임이 멈췄을때 특정 비동기, 코루틴은 진행이 되었으면 할때</p>
<pre><code class="language-cs">private async UniTaskVoid WaitSecondAsync()
{
    await UniTask.Delay(TimeSpan.FromSeconds(1f), true);  // 1초 (UnScaleDeltaTime)
    Debug.Log(&quot;1초가 지났습니다.&quot;);
}</code></pre>
<p>처럼 사용한다.</p>
<h2 id="특정조건-기다리기">특정조건 기다리기</h2>
<pre><code class="language-cs">private async UniTaskVoid AttackAsync()
{
    await UniTask.WaitUntil(() =&gt; isAttack);
}</code></pre>
<p>await UniTask.WaitUntil을 사용해 isAttack이 true가 될때까지 기다린다.</p>
<h2 id="web-request">Web Request</h2>
<pre><code class="language-cs">// 코루틴 방식
private IEnumerator WaitGetWeb(UnityAction&lt;Texture2D&gt; action)
{
    UnityWebRequest www = UnityWebRequestTexture.GetTexture(ImagePath);
    yield return www.SendWebRequest();

    Texture2D texture = ((DownloadHandlerTexture)www.downloadHandler).texture;

    action.Invoke(texture);
}

// UniTask 방식
private async UniTask&lt;Texture2D&gt; GetWebImageAsync()
{
    UnityWebRequest www = UnityWebRequestTexture.GetTexture(ImagePath);
    await www.SendWebRequest();

    Texture2D texture = ((DownloadHandlerTexture)www.downloadHandler).texture;
    return texture;
}

private async UniTaskVoid GetIamgeAsync()
{
    Texture2D texture = await GetWebImageAsync();
    profileImage.texture = texture;
}</code></pre>
<p>처럼 코루틴은 함수를 넘겨야하는데 
UniTask는 반환형이 있어 받아오는 함수를 만든 후 GetImageAsync를 실행하면 된다.</p>
<p>이제</p>
<pre><code class="language-cs">private void Start()
{
    GetIamgeAsync().Forget();
}</code></pre>
<p>이렇게 사용하면 코루틴과 동일하게 동작한다.</p>
<h2 id="도중에-멈추기">도중에 멈추기</h2>
<p>코루틴에는 Stop코루틴이 있는 것처럼 UniTask도 지원을 한다.
크게 2가지로 나뉘는데</p>
<ol>
<li>CancellationTokenSource</li>
</ol>
<ul>
<li>Cancel 타이밍을 원할 때 처리 가능, 직접적인 Dispose호출 필요<pre><code class="language-cs">private CancellationTokenSource _source = new CancellationTokenSource();
</code></pre>
</li>
</ul>
<p>private void Update()
{
    if(Input.GetKeyDown(KeyCode.A))
        _source.Cancel();           // A키 누르면 캔슬
}</p>
<p>private async UniTask WaitAsync()
{
    await UniTask.Delay(TimeSpan.FromSeconds(3), cancellationToken: _source.Token);
    Debug.Log(&quot;진행됨&quot;);
}</p>
<pre><code>코루틴이랑 비슷한 방식이다.

하지만 _source가 자동으로 메모리가 해제되지 않는걸 볼 수 있다.
즉 Dispose를 직접 해주어야 한다는 것이다.
```cs
private void OnDestroy()
{
    _source.Cancel();
    _source.Dispose();
}

private void OnEnable()
{
    if(_source != null)
        _source.Dispose();

    _source = new CancellationTokenSource();
}

private void OnDisable()
{
    _source.Cancel();    
}</code></pre><p>귀찮지만 더 디테일하게 처리가 가능하다.</p>
<p>하지만 이렇게 모두 작성하는 것은 상당히 귀찮다. 그래서 제공하는 것이</p>
<ol start="2">
<li>this.GetCancellationTokenOnDestroy</li>
</ol>
<ul>
<li>Destroy될때 Cancel 및 Dispose를 수행함.<pre><code class="language-cs">private async UniTask WaitAsync()
{
  await UniTask.Delay(TimeSpan.FromSeconds(3), cancellationToken: this.GetCancellationTokenOnDestroy());
  Debug.Log(&quot;진행됨&quot;);
}</code></pre>
이렇게 작성하면 삭제되면 자동으로 종료가 된다.</li>
</ul>
<p>하지만 OnDestroy이기 떄문에 비활성화 될때에는 따로 설정을 해주어야한다.</p>
<h2 id="tasktraker">TaskTraker</h2>
<p>코루탄, UniTask의 실행을 시각정인 정보로 제공한다.</p>
<pre><code class="language-cs">private void Start()
{
    Wait3Second().Forget();
}

private async UniTaskVoid Wait3Second()
{
    await UniTask.Delay(TimeSpan.FromSeconds(3f));
    Debug.Log(&quot;3초 지남&quot;);
}</code></pre>
<p>간단한 3초 딜레이 함수를 작성하고 보면</p>
<p><img alt="" src="https://velog.velcdn.com/images/hsd0604/post/61788a03-1a9e-46d3-b8ba-a63d2b15f7ea/image.png" />
보는 바와 같이 현재 진행되고 있는 것들이 전부 보이는걸 알 수 있다.</p>
<p>이는 메모리 누수를 방지하기 위해서는 꼭알아야하는 중요한 기능이다.</p>
<h2 id="dotween과-호환">DoTween과 호환</h2>
<p>호환하기 위해서는 define으로 UNITASK_DOTWEEN_SUPPORT라는 내용을 추가시켜야 한다.</p>
<p>Player탭에서 추가가 가능하다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/hsd0604/post/d367a3e4-10db-4a5d-a646-19184405d921/image.png" /></p>
<pre><code class="language-cs">private async UniTaskVoid WaitMove()
{
    await transform.DOMove(new Vector3(0,3,0), 3f); // 목표에 도다할때 까지 기다린다.
}</code></pre>
<h2 id="그-외">그 외</h2>
<pre><code class="language-cs">await UniTask.Yield(PlayerLoopTiming.FixedUpdate);</code></pre>
<p>처럼 플레이어 루프 타이밍을 이용해서 지정한 루프타이밍으로 기다릴 수 있다.</p>
<pre><code class="language-cs">private async UniTask 병렬로드()
{
    var taskA = LoadA();
    var taskB = LoadB();
    await UniTask.WhenAll(taskA, taskB);
}</code></pre>
<p>처럼 동시에 병렬로 로드할 수 있다.</p>
<h3 id="멀티스레딩-오해-금지">멀티스레딩 오해 금지</h3>
<p>UniTask는 멀티스레딩이 아니라 논블로킹 기반.</p>
<p>Unity API는 여전히 메인스레드에서만 호출 가능.
→ await 이후에도 Unity 객체 접근할 땐 반드시 메인 루프 보장돼야 함</p>