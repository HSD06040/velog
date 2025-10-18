<p>PhotonView</p>
<ul>
<li>가장 기본적인 동기화를 지원해주는 컴포넌트이다.</li>
<li>ViewID(네트워크 식별자)</li>
<li>소유자</li>
<li>네트워크 변경사항을 읽고 쓰는 컴포넌트이다.</li>
<li><blockquote>
<p>Synchronization : 동기화 방법을 선택한다.</p>
</blockquote>
</li>
<li><blockquote>
<p>Observalble Search : 동기화 될 내용을 찾는다.</p>
</blockquote>
</li>
</ul>
<p>MonobehaviourPun</p>
<ul>
<li>동기화의 기준이 되며 포톤 뷰를 쓰는 오브젝트는 상속받아 사용하기를 권장된다.</li>
</ul>
<p>PhotonNetwork.Instantiate</p>
<ul>
<li>모든 클라이언트에서 오브젝트를 생성하기 위해 사용된다.</li>
<li>매개변수로 string을 받는다.</li>
<li>string 을 받는 이유는 Resouecs폴더에서 받아오기 때문이다.</li>
<li>해당 함수로 생성 시 자동으로 PvID가 할당된다.</li>
</ul>
<p>PhotonNetwork.Destroy</p>
<ul>
<li>네트워크 객체를 삭제할 때에 사용된다.</li>
<li>룸 안에 있는 모든 클라이언트는 같은 게임 오브젝트를 삭제하기 위한
포톤 뷰를 기준으로 삭제를 실행한다.</li>
</ul>
<p>IPunObservable</p>
<ul>
<li><p>동기화를 위한 Interface이며 포톤의 Transform 동기화 스크립트가 있기는 하지만
포톤에서도 직접 인터페이스를 구현하거나, classic을 쓰는 것을 권장하고 있다.</p>
</li>
<li><p>stream.SendNet로 보내고, stream.ReceiveNext로 보낸 순서대로 받는다. (순서 중요)</p>
</li>
</ul>
<p>참조 형식 데이터</p>
<ul>
<li>참조 형식의 데이터도 전달이 가능하다. (직접적이지는 않음)</li>
<li>PhotonView.Find(PvID)로 해당 오브젝트를 받아 올 수 있다. (캐싱이라 빠름)</li>
</ul>
<p>함수 동기화 (PunRPC)</p>
<ul>
<li>어트리뷰트를 통해 사용가능하다.</li>
<li>원격 클라이언트에 있는 함수를 호출할 수 있다.</li>
<li>매개변수로는 직렬화를 받을 수 없다.(각 클라가 모를 수 있음)</li>
<li>PhotonMessageInfo를 매개변수로 받을 수 있다. (보낸 플레이어 정보, 뷰 정보, 보낸 서버 시간을 받을 수 있다.)</li>
</ul>
<p>지연보상</p>
<ul>
<li><p>동기화 차이가 클수록 경험 저하와 오류로 이어질 수 있다.</p>
<pre><code class="language-cs">[PunRPC]
public void GameStart(PhotonMessageInfo info)
{
// 서버에서 RPC를 보낸 시간과 현재 시간의 격차를 계산
float lag = Mathf.Abs((float)(PhotonNetwork.Time - info.SentServerTime));

rigidbody.velocity = transform.forward * speed;
rigidbody.position += rigidbody.velocity * lag;
}</code></pre>
</li>
<li><p>그래서 다음과 같이 적용시켜 지연이 생겨도 동기화 될 수 있도록 할 수 있다.</p>
</li>
</ul>