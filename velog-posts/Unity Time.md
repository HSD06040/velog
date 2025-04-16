<h3 id="time">time</h3>
<ul>
<li>실행 후 경과시간을 초 단위로 반환</li>
</ul>
<h3 id="deltatiem">deltaTiem</h3>
<ul>
<li>전 프레임과 현재 프레임의 간격을 초 단위로 반환</li>
</ul>
<h3 id="timescale">timeScale</h3>
<ul>
<li>시간이 흐르는 속도를 제어함. (기본적으로 1.0)
슬로우모션 등에서 활용가능</li>
</ul>
<h3 id="fixeddeltatime">fixedDeltaTime</h3>
<ul>
<li>Unity의 고정 타임스템 루프 간격을 제어함.</li>
</ul>
<h3 id="maximumdeltatime">maximumDeltaTime</h3>
<ul>
<li>엔진에서 deltaTime 프로퍼티를 통과하는 것을 보고하는 시간의 상한을 설정함. (기본 1/3초)
즉 deltaTime의 최대값을 설정해줌 (갑작스러운 프레임 드랍으로 물리연산이 튀지 않게)
<img alt="" src="https://velog.velcdn.com/images/hsd0604/post/d4eca1e7-22f9-4a8b-926e-c60d770f4c65/image.png" />
maximunDeltaTime은 deltaTime에만 영향을 주므로 unscaledDeltaTime의 반환값과 다른걸 볼 수 있다.</li>
</ul>
<h3 id="unscaleddeltatime">unscaledDeltaTime</h3>
<ul>
<li>Unity의 TimeScale에 영향을 받지않는 deltaTime이다.
UI 애니메이션, 타이머 등 활용
<img alt="" src="https://velog.velcdn.com/images/hsd0604/post/42b271fa-53c1-49bd-9754-e593b68eee62/image.png" /></li>
</ul>
<ul>
<li>그래프를 보면 알 수 있듯 time은 timeScale과 같은 것에 영향을 받는 가짜 시간이라고 말할 수 있고</li>
<li>다른 곳에 영향을 받지 않는 unscaledTime은 진짜 게임을 실행한 뒤 흐른 시간이라고 말할 수 있다.<h3 id="smoothdeltatime">smoothDeltaTime</h3>
</li>
<li>더 부드러운 deltaTime이다. 프레임의 변화가 커도 부드럽게 보간된다.
부드럽게 보여야하는 카메라 등에 활용
<img alt="" src="https://velog.velcdn.com/images/hsd0604/post/26f72702-3f7c-495b-9f38-1ff0d2e6b94e/image.png" /></li>
</ul>
<ul>
<li><p>정확히 말하자면 모든 배리에이션이 평활화된 최근 deltaTime 값의 근사치를 반환한다.
(최근 몇프레임간의 평균치)</p>
</li>
<li><p>특히 maximumDeltaTime으로 설정된 임계값 아래로 내려가는 경우 사용된다.
(너무 큰 deltaTime을 무시하고 부드럽게 처리한다.)</p>
</li>
</ul>
<h3 id="unscaledtime">unscaledTime</h3>
<ul>
<li>TimeScale을 무시한 실행후 경과시간 즉 TimeScale을 무시한 time이다.
실제 게임 플레이타입 확인 등에 활용</li>
</ul>
<h3 id="captureframerate">captureFramerate</h3>
<ul>
<li><p>게임 내에서 동영상으로 녹화 시 일정한 프레임 간격으로 강제로 흐름을 제어하는 기능이다.</p>
</li>
<li><p>스크린샷을 찍거나 저장하는 작업이 느리다.
또한 그냥 녹화하게 되면 프레임이 들쭉날쭉해지고 느려진다.</p>
</li>
<li><p>그래서 Unity는 녹화할 시간을 벌어주는 느낌으로 쓰는것이다.
rate에 지정된 값은 Unity가 1초에 몇 프레임으로 읽을지를 결정한다.</p>
</li>
<li><p>실제로 성능적으로 따라잡지 못해서 녹화는 Unity가 일정프레임으로 하고 있기때문에</p>
</li>
</ul>
<p>동영상은 부드럽고 일정프레임으로 녹화할 수 있게된다.</p>
<h1 id="unity-시간-로직">Unity 시간 로직</h1>
<p><img alt="" src="https://velog.velcdn.com/images/hsd0604/post/0984550c-8a04-4f4a-9bbd-6e0b3dfd84fe/image.png" /></p>
<h3 id="compute-time-elapsed-since-last-frame">Compute time elapsed since last frame</h3>
<ul>
<li>이전 프레임부터 지금까지 걸린 시간을 측정한다. (deltaTime)</li>
</ul>
<h3 id="deltatime--maximumdeltatime">deltaTime &gt; maximumDeltaTime?</h3>
<ul>
<li>걸린 시간이 상한보다 크다면 상한치로 바꾼다.</li>
</ul>
<h3 id="deltatime-is-add-to-time">deltaTime is add to time</h3>
<ul>
<li>deltaTime이 time에 추가된다.</li>
<li>그래서 time은 게임 실행 후 경과한 시간이 된다.</li>
</ul>
<h3 id="fixedtime">fixedTime</h3>
<ul>
<li><p>만약 fixedupdate가 실행되어야 한다면? (fiexd의 실행주기가 지났다면?)</p>
</li>
<li><p>time - fiexdTime &gt;= fiexdDeltaTime (실제시간 - 마지막으로 fiexdUpdate가 실행된 시간 &gt;= 주기 (0.02초))
(fiexdTime은 FiexedUpdate가 실행될때마다 업데이트됨.)</p>
</li>
<li><p>fiexedUpdate를 실행하고 Update를 실행함. 아니면 바로 Update를 실행함</p>
</li>
</ul>
<h3 id="update">Update</h3>
<ul>
<li>Update가 실행됨</li>
</ul>