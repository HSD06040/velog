<p>크게 흐름을 제어하는 Flow와
행동을 실행하는 Action노드로 분리할 수 있습니다.</p>
<p>BT에는 Success, Running, Failure 이 있다.</p>
<ul>
<li><p>Success는 성공을 반환한다.</p>
</li>
<li><p>Failure은 실패를 반환한다.</p>
</li>
<li><p>Running은 계속 실행한다.</p>
</li>
</ul>
<pre><code class="language-cs">using System;
using Unity.Behavior;
using UnityEngine;
using Action = Unity.Behavior.Action;
using Unity.Properties;

[Serializable, GeneratePropertyBag]
[NodeDescription(name: &quot;SetOnlyAnimBool&quot;, story: &quot;[Self] Set Only Anim True [AnimBoolName]&quot;, category: &quot;Action&quot;, id: &quot;9d70ae7a46ad93ec0c9ca600bce8db2f&quot;)]
public partial class SetOnlyAnimBoolAction : Action
{
    [SerializeReference] public BlackboardVariable&lt;GameObject&gt; Self;
    [SerializeReference] public BlackboardVariable&lt;string&gt; AnimBoolName;

    private Animator anim;

    protected override Status OnStart()
    {
        anim = Self.Value.GetComponent&lt;Animator&gt;();

        if(anim == null)
            return Status.Running;

        foreach (AnimatorControllerParameter param in anim.parameters)
        {
            if(param.type == AnimatorControllerParameterType.Bool)
            {
                anim.SetBool(param.name, false);
            }
        }

        anim.SetBool(AnimBoolName, true);

        return Status.Running;
    }
}</code></pre>
<p>만든 Action 노드이다. 원하는 Anim Bool 하나만을 true로 하기위해 만든 노드이다.
이 경우는 설정하고 계속 실행되어야하기 때문에 Running으로 계속 실행한다. (Status는 열겨형이다.) </p>
<h1 id="action-노드">Action 노드</h1>
<table>
<thead>
<tr>
<th>노드 이름</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>Create new Action</td>
<td>기본적으로 지원하는 Action노드 외에도 새로운 Action노드를 C#으로 작성할 수 있습니다.</td>
</tr>
<tr>
<td>Set Animator ~~</td>
<td>애니메이터의 다양한 타입을 변경할 수 있습니다.</td>
</tr>
<tr>
<td>Set Variable Value</td>
<td>BlackBoard 안에 있는 기능으로 주어진 변수의 값을 설정합니다.</td>
</tr>
<tr>
<td>Conditional Guard</td>
<td>조건이 true로 평가되면 성공을 반환하고, false로 평가되면 실패를 반환합니다.</td>
</tr>
<tr>
<td>Log Message</td>
<td>콘솔에 메시지를 출력합니다.</td>
</tr>
<tr>
<td>Log Variable</td>
<td>변수의 값을 콘솔에 출력합니다.</td>
</tr>
<tr>
<td>Log Variable Change</td>
<td>변수의 값이 변경될 때 콘솔에 출력합니다.</td>
</tr>
<tr>
<td>Wait</td>
<td>지정된 시간(초) 동안 대기합니다.</td>
</tr>
<tr>
<td>Wait (Range)</td>
<td>최소값과 최대값 사이의 지정된 시간 동안 대기합니다.</td>
</tr>
<tr>
<td>Find Closest With Tag</td>
<td>지정된 태그를 가진 가장 가까운 GameObject를 찾습니다.</td>
</tr>
<tr>
<td>Find With Tag</td>
<td>지정된 태그를 가진 GameObject를 찾습니다.</td>
</tr>
<tr>
<td>Attach Object</td>
<td>GameObject의 부모를 다른 GameObject로 설정하고 오프셋을 적용합니다.</td>
</tr>
<tr>
<td>Destroy Object</td>
<td>GameObject를 삭제합니다.</td>
</tr>
<tr>
<td>Don't Destroy On Load</td>
<td>GameObject가 씬 로드 시 삭제되지 않도록 방지합니다.</td>
</tr>
<tr>
<td>Instantiate Object</td>
<td>GameObject를 그래프 소유자 씬에 생성합니다.</td>
</tr>
<tr>
<td>Set Object Active State</td>
<td>GameObject의 활성 상태를 설정합니다.</td>
</tr>
<tr>
<td>Navigate To Location</td>
<td>NavMeshAgent를 사용하여 GameObject를 지정된 위치로 이동시킵니다. NavMeshAgent가 없으면 Transform을 사용합니다.</td>
</tr>
<tr>
<td>Navigate To Target</td>
<td>NavMeshAgent를 사용하여 GameObject를 다른 GameObject 방향으로 이동시킵니다. NavMeshAgent가 없으면 Transform을 사용합니다.</td>
</tr>
<tr>
<td>Patrol</td>
<td>NavMeshAgent를 사용하여 GameObject를 웨이포인트를 따라 이동시킵니다. NavMeshAgent가 없으면 Transform을 사용합니다.</td>
</tr>
<tr>
<td>Add Force</td>
<td>대상 Rigidbody에 물리적인 힘을 가합니다.</td>
</tr>
<tr>
<td>Add Torque</td>
<td>대상 Rigidbody에 토크를 가합니다.</td>
</tr>
<tr>
<td>Check Collisions In Radius</td>
<td>에이전트를 기준으로 지정된 반경 내 충돌을 확인합니다. 충돌이 감지되면 해당 객체가  CollidedObject 에 저장됩니다.</td>
</tr>
<tr>
<td>Set Velocity</td>
<td>대상 Rigidbody의 선형 속도를 특정 값으로 설정합니다.</td>
</tr>
<tr>
<td>Wait For Collision</td>
<td>지정된 에이전트에서 OnCollision 이벤트를 대기합니다.</td>
</tr>
<tr>
<td>Wait For Collision 2D</td>
<td>지정된 에이전트에서 2D OnCollision 이벤트를 대기합니다.</td>
</tr>
<tr>
<td>Wait For Trigger</td>
<td>지정된 에이전트에서 OnTrigger 이벤트를 대기합니다.</td>
</tr>
<tr>
<td>Wait for Trigger 2D</td>
<td>지정된 에이전트에서 2D OnTrigger 이벤트를 대기합니다.</td>
</tr>
<tr>
<td>Resource</td>
<td>Resource 옵션을 사용하려면 Add &gt; Action &gt; Resource를 선택하세요.</td>
</tr>
<tr>
<td>Play Audio</td>
<td>대상 위치에서 AudioResource를 재생합니다.</td>
</tr>
<tr>
<td>Play Particle System</td>
<td>대상 위치에서 ParticleSystem을 재생합니다.</td>
</tr>
<tr>
<td>Load Scene</td>
<td>Unity 씬을 로드합니다.</td>
</tr>
<tr>
<td>Unload Scene</td>
<td>Unity 씬을 언로드합니다.</td>
</tr>
<tr>
<td>Look At</td>
<td>대상 방향을 향하도록 회전시킵니다.</td>
</tr>
<tr>
<td>Rotate</td>
<td>오일러 회전을 통해 회전시킵니다.</td>
</tr>
<tr>
<td>Scale</td>
<td>특정 값만큼 Transform을 스케일링합니다.</td>
</tr>
<tr>
<td>Set Position</td>
<td>대상의 위치를 특정 위치로 설정합니다.</td>
</tr>
<tr>
<td>Set Position To Target</td>
<td>Transform의 위치를 특정 대상 위치로 설정합니다.</td>
</tr>
<tr>
<td>Set Rotation</td>
<td>Transform의 회전을 특정 오일러 회전 값으로 설정합니다.</td>
</tr>
<tr>
<td>Set Scale</td>
<td>Transform의 스케일을 특정 값으로 설정합니다.</td>
</tr>
<tr>
<td>Translate</td>
<td>대상 위치를 특정 값만큼 이동시킵니다.</td>
</tr>
</tbody></table>
<h1 id="event-노드">Event 노드</h1>
<table>
<thead>
<tr>
<th>노드 이름</th>
<th>설명 (한글 번역)</th>
</tr>
</thead>
<tbody><tr>
<td>Start On Event Message</td>
<td>이벤트 메시지를 수신한 후 서브그래프를 시작합니다. 노드가 이벤트에 반응하는 방식은 다음 모드로 변경할 수 있습니다:<br />• Default: 노드가 유휴 상태이고 자식 노드가 실행 중이지 않을 때만 트리거됩니다.<br />• Restart: 모든 자식 노드를 종료한 후 노드를 다시 시작합니다.<br />• Once: 이벤트 채널 수신 후 한 번만 트리거되고, 이후 수신을 중지합니다.</td>
</tr>
<tr>
<td>On Start</td>
<td>동작 그래프의 루트입니다. 그래프에서 여러 개의 On Start 노드를 사용할 수 있습니다.</td>
</tr>
<tr>
<td>Send Event Message</td>
<td>지정된 채널에 이벤트 메시지를 보냅니다.</td>
</tr>
<tr>
<td>Wait for Event Message</td>
<td>게임 캐릭터가 지정된 채널에서 이벤트 메시지를 수신할 때까지 대기하도록 할 때 이 노드를 사용합니다.</td>
</tr>
</tbody></table>
<h1 id="flow-노드">Flow 노드</h1>
<table>
<thead>
<tr>
<th>노드 이름</th>
<th>설명 (한글 번역)</th>
</tr>
</thead>
<tbody><tr>
<td>Create new Modifier</td>
<td>새 수정자를 생성합니다. 이 노드를 사용하려면 Add &gt; Flow &gt; Create new Modifier를 선택하세요.<br />수정자는 연결된 서브그래프의 실행 방식에 영향을 줍니다. 예를 들어, 반복 수정자는 해당 브랜치가 작업을 여러 번 수행하게 합니다.<br />자세한 내용은 사용자 지정 노드 만들기를 참조하세요.</td>
</tr>
<tr>
<td>Create new Sequencing</td>
<td>새로운 시퀀싱 노드를 생성할 때 사용합니다. 이 노드를 사용하려면 Add &gt; Flow &gt; Create New Sequencing을 선택하세요.<br />시퀀싱 노드는 연결된 브랜치의 흐름을 제어합니다. 시퀀스가 어떻게 실행되고 언제 완료되는지를 정의합니다.<br />자세한 내용은 사용자 지정 노드 만들기를 참조하세요.</td>
</tr>
<tr>
<td>Abort</td>
<td>지정된 조건이 true일 때 브랜치를 중단합니다.</td>
</tr>
<tr>
<td>Restart</td>
<td>지정된 조건이 true일 때 브랜치를 다시 시작합니다.</td>
</tr>
<tr>
<td>Conditional Branch</td>
<td>조건이 true인지 false인지에 따라 브랜치를 선택합니다.</td>
</tr>
<tr>
<td>Switch</td>
<td>열거형(Enumeration) 값에 따라 브랜치를 나눕니다.</td>
</tr>
<tr>
<td><strong>Run In Parallel</strong></td>
<td>모든 브랜치를 동시에 실행합니다. 병렬 브랜치를 처리하기 위한 다양한 실행 모드를 설정할 수 있습니다.</td>
</tr>
<tr>
<td>Wait For All</td>
<td>모든 부모가 이 노드를 시작했을 때 자식을 활성화합니다. 자식의 서브그래프가 종료될 때까지 다시 시작할 수 없습니다.</td>
</tr>
<tr>
<td>Wait For Any</td>
<td>부모 중 하나가 이 노드를 시작했을 때 자식을 활성화합니다. 자식의 서브그래프가 종료될 때까지 다시 시작할 수 없습니다.</td>
</tr>
<tr>
<td>Cooldown</td>
<td>작업 반복 사이에 필수 대기 시간을 적용하여 실행 빈도를 조절합니다.</td>
</tr>
<tr>
<td>Inverter</td>
<td>자식 작업의 결과를 반대로 만듭니다. 성공은 실패로, 실패는 성공으로 바뀝니다.</td>
</tr>
<tr>
<td>Random</td>
<td>무작위로 브랜치를 실행합니다.</td>
</tr>
<tr>
<td>Repeat</td>
<td>노드의 작업을 반복 실행합니다. 반복 작업이 언제, 어떻게 실행되는지를 정의하는 다양한 실행 모드를 설정할 수 있습니다.</td>
</tr>
<tr>
<td>Sequence</td>
<td>브랜치를 순서대로 실행하며, 하나라도 실패하면 중단되고, 모두 성공하면 완료됩니다.</td>
</tr>
<tr>
<td>Succeeder</td>
<td>자식 노드의 결과를 강제로 성공으로 만듭니다. 실패가 예상되더라도 시퀀스를 방해하지 않고 트리의 브랜치를 처리하고 싶을 때 유용합니다.</td>
</tr>
<tr>
<td>Time Out</td>
<td>지정된 시간(초)이 지나면 해당 브랜치의 실행을 종료합니다.</td>
</tr>
<tr>
<td>Try In Order</td>
<td>브랜치를 순서대로 실행하며, 하나라도 성공하면 중단됩니다.</td>
</tr>
</tbody></table>
<h1 id="subgraphs-노드">Subgraphs 노드</h1>
<table>
<thead>
<tr>
<th>노드 이름</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>Run Subgraph</td>
<td>할당된 서브그래프를 실행하고 해당 그래프의 최종 상태를 반환합니다. 이 노드는 서브그래프를 현재 그래프에 <strong>정적으로 포함</strong>하기 때문에, 서브그래프 내의 Blackboard 변수(BBV)를 오버라이드할 수 있습니다. 항상 동일한 데이터를 사용하는 기능을 모듈화하고 싶을 때 이 노드를 사용하세요.</td>
</tr>
<tr>
<td>Run Subgraph Dynamically</td>
<td><code>BlackboardVariable&lt;Subgraph&gt;</code>를 사용하여 서브그래프를 <strong>동적으로 할당</strong>하는 노드입니다. 정적인 Run Subgraph 노드와 달리, 이 노드는 서브그래프를 현재 그래프에 포함하지 않으며, 할당된 BlackboardVariable는 런타임 중 변경될 수 있습니다. 이로 인해 서브그래프의 Blackboard에 직접 접근할 수 없습니다. 이를 해결하려면 <strong>Blackboard 에셋을 인터페이스로 할당</strong>하여 현재 그래프와 동적으로 할당된 서브그래프 간에 데이터를 주고받을 수 있도록 해야 합니다.</td>
</tr>
</tbody></table>
<h2 id="sticky-노트">Sticky 노트</h2>
<p>이 노드의 역할등을 적을 수 있는 메모장 같은 기능입니다.</p>
<p>  <img alt="" src="https://velog.velcdn.com/images/hsd0604/post/2e47a519-c63a-401d-a7ac-a7fc168e62d5/image.png" /></p>
<ul>
<li><p>처럼 다양한 기능을 하는 노드들을 조합하고 행동흐름을 제어해서 AI를 만드는 것이다.</p>
</li>
<li><p>그래서 노드들의 행동을 알아야 수월하며 만약 원하는 노드가 없을 시에는 Create New로 원하는 기능을 만들면 된다.</p>
<p>참고로 사진의 Self Set Only Anim True도 원래는 없는 Action노드를 C#으로 작성하여 원하는 Anim Bool타입만 true를 하는 기능을 만든 것 이다.</p>
</li>
</ul>
<h1 id="마무리">마무리</h1>
<p>  아직 많이 파보지도 못한 영역이고 NavMesh와 병합하여 간단한 AI밖에 구현해보지 못했다.
  (그 마저도 왜 그런지 모르는 버그가 있다.)</p>
<p>  일단 지금까지의 느낀점은 좀 느리고 최적화가 필요하다고 느꼈다.
  (모르는 부분에서 프레임드랍이 심하게 일어난다.)</p>
<p>결론은 더 많이 파봐야 할 것 같다. 실제로 파는 재미도 있어서 지속적으로 공부할 것 같고 
  이어서 Unity6의 개편되거나 다른기능 (DOTS ECS)와 같은 기능들도 한번 해보면 좋을 것 같다.</p>