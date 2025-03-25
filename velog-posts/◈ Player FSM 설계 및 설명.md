<h1 id="❖-fsm이란">❖ FSM이란?</h1>
<blockquote>
<p>유한 상태 머신이라고도 불린다. 상태가 다른상태로 전이하는 형식이다.</p>
</blockquote>
<p>해당 객체의 상태를 세세하게 관리하여 상태의 전이조건이 만족되면 전이시킨다.
<img alt="" src="https://velog.velcdn.com/images/hsd0604/post/e5c705b1-67ce-4239-9743-e35ef649405c/image.png" /></p>
<h1 id="❖-코드로-알아보자">❖ 코드로 알아보자</h1>
<h2 id="✧-playerstate">✧ PlayerState</h2>
<blockquote>
<p>플레이어의 상태이다. 이 스크립트를 상속받아 상태를 만들 수 있도록 설계되었다.</p>
</blockquote>
<pre><code class="language-cs">public class PlayerState
{
    protected Player player;
    protected PlayerStateMachine stateMachine;

    private string animBoolName;

    protected float stateTimer;
    protected bool animFinished;

    public PlayerState(Player _player, PlayerStateMachine _stateMachine, string _animBoolName)
    {
        player = _player;
        stateMachine = _stateMachine;
        animBoolName = _animBoolName;
    }

    public virtual void Enter()
    {
        animFinished = false;
        player.anim.SetBool(animBoolName, true);
    }

    public virtual void Update()
    {
        stateTimer -= Time.deltaTime;
    }

    public virtual void Exit()
    {
        player.anim.SetBool(animBoolName, false);
    }

    public virtual void AnimationFinishTrigger()
    {
        animFinished = true;
    }
}</code></pre>
<p>하나하나 살펴보자면</p>
<ul>
<li><p>Enter()
상태가 시작될때 실행되는 함수이다.</p>
</li>
<li><p>Update
상태가 진행중 일때 매 프레임마다 실행되는 함수이다.</p>
</li>
<li><p>Exit()
현재상태가 다른상태로 전이할때 즉 빠져나갈때 실행되는 함수이다.</p>
</li>
<li><p>public PlayerState(인자)
상태가 생성될때 필요한 정보들을 가져오기 위해 인자값이 존재한다.
(Player의 상태이다 보니 Player를 필요로 하는 경우가 많다.)</p>
</li>
<li><p>animBoolName
상태에 따라 애니메이션이 변경되기 위해 필요하다.
Unity에서는 이름을 찾아 Bool을 true/false로 지정하기 때문에 string 타입이다.</p>
</li>
</ul>
<h2 id="✧-statemachine">✧ StateMachine</h2>
<blockquote>
<p>상태를 바꾸는 핵심적인 기능을 하는 객체이다.</p>
</blockquote>
<pre><code class="language-cs">public class PlayerStateMachine
{
    public PlayerState currentState {  get; private set; }

    public void Initialize(PlayerState _startState)
    {
        currentState = _startState;
        currentState.Enter();
    }

    public void ChangeState(PlayerState _newState)
    {
        currentState.Exit();
        currentState = _newState;
        currentState.Enter();
    }
}</code></pre>
<p>다음과 같이 간단하게 작성되어있다.</p>
<ul>
<li>Initialize
매개변수로 받아온 상태를 현재상태로 지정하고 현재상태의 Enter()을 실행한다.</li>
<li>ChangeState
현재상태의 Exit()를 실행하고 매개변수로 받아온 상태를 현재상태로 지정한 후 
현재상태의 Enter()을 실행한다.</li>
</ul>
<p>이런구조 덕분에 상태가 들어올때 Enter()실행 나갈때 Exit()실행과 같이 직관적으로 코드를 사용할 수있다.</p>
<h2 id="✧-player">✧ Player</h2>
<blockquote>
<p>상태를 생성하고 처음 상태를 지정하며 현재상태의 Update가 프레임마다 실행되도록 한다.</p>
</blockquote>
<pre><code class="language-cs">public class Player : Entity
{
    #region State

    public PlayerStateMachine stateMachine { get; private set; }
    public PlayerIdleState idleState { get; private set; }

    #endregion

    protected override void Awake()
    {
        base.Awake();

        stateMachine = new PlayerStateMachine();
        idleState = new PlayerIdleState(this, stateMachine, &quot;Idle&quot;);
    }
    private void Start()
    {
        stateMachine.Initialize(idleState);
    }

    void Update()
    {
        stateMachine.currentState.Update();
    }
}</code></pre>
<p>현재는 간단히 짜여져있다.</p>
<ul>
<li><p>플레이어에서 상태들을 전부 생성한 후 다른 상태로 전이할때에는
PlayerState코드에 있는 (PlayerIdleState(this, stateMachine, &quot;Idle&quot;)에서 this로 인자값을 받은)
player로 상태들을 불러와 전이하는 방식이다.</p>
</li>
<li><p>이제 설계는 거의 끝이 났다고 보면된다.
이제 점점 확장해가면서 필요한부분이 있으면 수정하는 것이 좋을 것 같다.</p>
</li>
</ul>
<h1 id="❖-마무리">❖ 마무리</h1>
<ul>
<li><p>FSM의 단점은 분명히 존재한다. 확장성이 좋은 방식은 아니다.</p>
</li>
<li><p>하지만 현재 프로젝트단계에서 판단하기에 엄청 복잡한 로직이 적고 FSM이 Input 기반에서는 유리하기 떄문에 FSM으로 하였다.</p>
</li>
<li><p>간단하고 이해하기에도 쉽기때문에 다루기에는 수월할 것이다.</p>
</li>
</ul>