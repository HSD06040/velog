<p>개인 PC에 저장하는 것이 아닌 서버에서 유저의 정보를 저장해서 불러오는 방식을 사용할 수 있다.
개인에 맞춘 동일한 경험을 제공할 수 있다.</p>
<p>공공저장소인 데이터 베이스를 이용하면 동일한 데이터를 공유해서 사용할 수 있다.</p>
<p>서버는 많은 사람들이 사용하는 공공저장소인 만큼 빠른 읽기, 쓰기가 중요하다.</p>
<p>대규모의 데이터인 경우에는 SQL 데이터 베이스를 쓴다.
장점 : 관리에 좋다, 체계가 좋다, 빠름
단점 : 유연하지 못하다.</p>
<p>하지만 Firebase는 JSON 스냅샷으로 NoSQL방식인데
장점 : 유연하다.
단점 : 관리가 약하다, 체계가 약하다, 비교적 느리다.
(SQL은 거의 테이블 기반으로 되어있고, NoSQL은 Tree 기반이다.(폴더))</p>
<p>Firebase는 &quot;Realtime&quot; Database인데 PC에도 캐싱을 시켜두는 방식을 사용하기 때문에
다른 NoSQL 데이터베이스보다 빠르다.</p>
<p>JSON 기반으로 되어 있다.</p>
<h2 id="firebase는-폴더-형태이기-때문에-관리를-할-필요가-있다">Firebase는 폴더 형태이기 때문에 관리를 할 필요가 있다.</h2>
<h3 id="권장사항">권장사항</h3>
<blockquote>
<p>쉽게 데이터를 저장하고 검색하기 쉽게 만드는 방법이 중요하다.</p>
</blockquote>
<ul>
<li>JSON은 유의미한 키값을 사용하는 것을 권장한다.</li>
<li>폴더의 구조를 깊게 파지말고 User의 데이터는 User아래에 위치하는 것이 정렬하고 찾는데에 빠르다.
(최대 32개이지만 최대 5개정도 까지 쓰는게 좋다.)</li>
<li>위의 규칙이 더 먼저이기 때문에 접근 관리가 힘들어져</li>
<li>평면화된 데이터 구조로 관리하는 것을 권장한다.
(예 : 사용자만 보는구조, 채팅만 보는구조, 메시지만 보는구조 로 따로 권한 관리를 한다.)
(사용자에서 특정 사용자의 안에 있는 데이터를 수정할 수 있는 구조 (모든 사용자를 읽지 않음))</li>
</ul>
<p>즉 일정 수준의 규칙을 정할 필요가 있다는 것이다.</p>
<h2 id="데이터-저장">데이터 저장</h2>
<table>
<thead>
<tr>
<th align="left">메서드</th>
<th align="left">용도</th>
</tr>
</thead>
<tbody><tr>
<td align="left">SetValueAsync</td>
<td align="left">정의된 경로에 데이터를 쓰거나 대체합니다. (하나의 값으로 덮어쓰기)</td>
</tr>
<tr>
<td align="left">UpdateChildrenAsync</td>
<td align="left">정의된 경로에서 모든 데이터를 대체하지 않고 일부 키를 업데이트 합니다.</td>
</tr>
<tr>
<td align="left">RunTransaction</td>
<td align="left">동시 업데이트에 의해 손상될 수 있는 복잡한 데이터를 업데이트 합니다.</td>
</tr>
</tbody></table>
<h3 id="databasereference">DatabaseReference</h3>
<blockquote>
<p>저장하기 위한 폴더의 위치이다.</p>
</blockquote>
<h3 id="setvalueasync-빠름">SetValueAsync (빠름)</h3>
<h4 id="저장이-가능한-타입">저장이 가능한 타입</h4>
<ul>
<li>string</li>
<li>long</li>
<li>double</li>
<li>bool</li>
<li>Dictionary&lt;string, Object&gt; (k)</li>
<li>List&lt;&quot;object&quot;&gt; : 인덱스 0 부터 저장된다. (장착된 스킬 순서 기억 등)</li>
<li>JSON</li>
</ul>
<p>object를 사용해서 박싱, 언박싱이 일어나지만, 네트워크는 어쩔 수 없이 박싱, 언박싱이 일어날 수 밖에 없다.</p>
<h3 id="setrawjsonvalueasync">SetRawJsonValueAsync</h3>
<p>Json으로 변환하여 한번에 쓰기도 가능하다. (덮어 쓰기)</p>
<pre><code class="language-cs">public class DatabaseTester : MonoBehaviour
{
    [SerializeField] Button testButton;
    [SerializeField] PlayerData data;

    private void Awake()
    {
        testButton.onClick.AddListener(DatabaseTest);
    }   

    private void DatabaseTest()
    {
        DatabaseReference reference = FirebaseManager.Database.RootReference;
        //reference.SetValueAsync(text);
        reference.SetRawJsonValueAsync(JsonUtility.ToJson(data));
    }
}

[Serializable]
public class PlayerData
{
    public string name;
    public int level;
    public float speed;
    public bool clear;
    public List&lt;string&gt; equipSkills;
    public Dictionary&lt;string, bool&gt; skillUnlocked;
}</code></pre>
<h3 id="updatechildrenasync">UpdateChildrenAsync</h3>
<blockquote>
<p>덮어 쓰기가 아닌 해당 값만을 변경할 때 사용하고 만약 해당값이 없다면 데이터를 생성한다.</p>
</blockquote>
<pre><code class="language-cs">Dictionary&lt;string, object&gt; dictionary = new Dictionary&lt;string, object&gt;();
dictionary[&quot;level&quot;] = 10;
dictionary[&quot;speed&quot;] = 3.5;

user.SetValueAsync(dictionary); // 해당 경로를 이 데이터로 설정
user.UpdateChildrenAsync(dictionary); // 기존 데이터에서 특정 데이터만 수정 / 없다면 생성</code></pre>
<p>수정할 데이터만 수정하기 때문에 효율적이다.</p>
<h3 id="runtransaction-상대적으로-느림">RunTransaction (상대적으로 느림)</h3>
<blockquote>
<p>동시에 같은 데이터를 건드릴 때 (경매장 등)에서 변경할 것 이라는 보내고 만약 데이터가 남아 있다면 실행이 되고 같은 타이밍에 다른 곳에서 수정이 되었으면 실패해야한다.</p>
</blockquote>
<p>변화량을 지정해주어 사용된다.</p>
<p>즉 변화된 값을 덮어쓰기위해 넘겨주는 것이 아니라 함수를 보내는 방식이다.
(함수자체를 보내는 것은 네트워크 상 불가능 해서 다른 방식으로 동작한다.)</p>
<p>작업 변경사항의 함수를 보내면 실행을 하고 반환한다. 도중에 값이 변경되면 다시 실행된다. (반복)</p>
<p>이러면 멀티스레드 미스매칭 문제를 해결할 수 있다.</p>
<h4 id="조심할-점">조심할 점</h4>
<blockquote>
<p>추가 삭제에서 조심해야 한다.</p>
</blockquote>
<p>체력과 같은 값의 변화량은 문제가 없지만</p>
<p>예)</p>
<ul>
<li>경매장에서 유저1이 입찰가 +10을 한다고 해보자
그런데 다른 유저가 바로 즉시구입을 하면 데이터가 사라지게 된다.
그럼 유저1이 실패하게된다.</li>
</ul>
<p>이때는 예외처리를 해서 아이템 데이터가 있을 때만 올릴 수 있도록 하는 예외처리를 해야한다.(중요)</p>
<p>네트워크 환경은 다른 플레이어의 행동과 엮이는 곳에서 예외처리를 생각 해야한다.</p>
<pre><code class="language-cs">private void DatabaseTest()
{
    DatabaseReference root = FirebaseManager.Database.RootReference;
    DatabaseReference leaderBoardRef = root.Child(&quot;LeaderBoard&quot;);

    bool isComplated = leaderBoardRef.RunTransaction(data =&gt;
    {

        return TransactionResult.Abort(); // 갱신 안함

        return TransactionResult.Success(data); // 값 변경
    }).IsCompleted;
}</code></pre>
<h2 id="데이터-읽기">데이터 읽기</h2>
<p>JSON으로 읽어올 수 있다 (편하다)</p>
<pre><code class="language-cs">userInfo.GetValueAsync().ContinueWithOnMainThread(task =&gt;
{
    if (task.IsFaulted)
        return;

    DataSnapshot snapshot = task.Result;

    string json = snapshot.GetRawJsonValue();

    PlayerData playerData = JsonUtility.FromJson&lt;PlayerData&gt;(json);
});</code></pre>
<p>데이터를 JSON으로 쓰고 JSON으로 읽어 온다는게 큰 장점인 것 같다.</p>