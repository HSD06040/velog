<h1 id="commit-규칙">Commit 규칙</h1>
<ul>
<li><p>제목과 본문은 구분하고</p>
</li>
<li><p>제목에서는 무엇을 만들었는지</p>
</li>
<li><p>본문에서는 왜 개발하였는지를 설명한다.</p>
</li>
</ul>
<table>
<thead>
<tr>
<th>유형</th>
<th>내용</th>
</tr>
</thead>
<tbody><tr>
<td>Feat</td>
<td>새로운 기능 추가</td>
</tr>
<tr>
<td>Fix</td>
<td>수정을 한 경우</td>
</tr>
<tr>
<td>Build</td>
<td>빌드 관련 수정</td>
</tr>
<tr>
<td>Test</td>
<td>테스트 코드 추가</td>
</tr>
<tr>
<td>refactor</td>
<td>코드 리펙토링</td>
</tr>
<tr>
<td>docs</td>
<td>주석 수정</td>
</tr>
<tr>
<td>release</td>
<td>버전 릴리즈</td>
</tr>
</tbody></table>
<h1 id="git-branch">Git Branch</h1>
<blockquote>
<p>분기를 나눠서 독립적으로 개발하는 방식이다.</p>
</blockquote>
<ul>
<li><p>분기를 나누게 되면 별도의 공간에서 제작이 가능하여 개인제작에만 집중해도 되며</p>
</li>
<li><p>Branch에 접근해서 현재 어떤 방식으로 제작되고 있는지 확인도 가능하다.</p>
</li>
<li><p>그리고 다 만들어지면 maseter에 merge 해줘도 되지만
더 안전하게 Develop Branch를 만들어서 실직적인 개발이 끝나고 완성 품을 버전으로 master에 붙이는 걸 추천한다.</p>
</li>
</ul>
<h2 id="conflict-충돌">CONFLICT (충돌)</h2>
<ul>
<li><p>같은 곳에서 분기를 나눠서 작업하였을때 같은 파일의 같은 위치에 상이한 내용이 작성되었을 때 </p>
</li>
<li><p>git은 무엇을 선택해야하는지 판단하지 못하여 충돌이 발생한다.</p>
</li>
<li><p>이 때에는 무엇이 맞는지 판단한 후 둘중 하나로 결정 해주어야한다.</p>
</li>
</ul>
<p>Scene은 상황이 좀 다르다.</p>
<ul>
<li><p>Scene은 코드가 아니라 파일이기 떄문에 한눈에 보기 힘들다. (거의 불가능)</p>
</li>
<li><p>그래서 그냥 Scene을 공동 작업할 때에는 복사한 후에 작업해야한다.</p>
</li>
</ul>
<h2 id="meta">.meta</h2>
<ul>
<li><p>Unity는 meta파일을 쓴다.</p>
</li>
<li><p>meta 파일은 오브젝트를 판단하기 위한 정보가 담긴 파일이다.</p>
</li>
<li><p>예를 들어 meta 파일은 반드시 고유한 guid가 존재한다.</p>
</li>
<li><p>이 아이디를 통해 Unity는 파일을 찾고 관리한다. (참조)</p>
</li>
<li><p>그래서 이 meta파일이 없으면 참조하지 못해 깨지게된다.</p>
</li>
</ul>
<h2 id="pull-request">Pull request</h2>
<ul>
<li><p>병합을 바로 해버리면 위험이 존재한다.</p>
</li>
<li><p>그래서 병합을 바로 하는게 아닌 병합을 요청하는 것이 Pull request이다.</p>
</li>
<li><p>만약 승인이 된다면 그제서야 merge가 되는 안전성을 위한 시스템이다.</p>
</li>
</ul>
<p>전에 먼저 충돌상황을 해결하고 싶을 때에는 </p>
<ul>
<li>자신의 branch에 master을 merge한 후 충돌을 해결한 후에 붙이면 충돌이 나지않는다.</li>
</ul>
<h2 id="issues">Issues</h2>
<blockquote>
<p>개발과 버그에 대한 이슈를 작성하는 공간이다.</p>
</blockquote>
<ul>
<li>Setting -&gt; SetupTemplate 에서 템플릿 설정이 가능하다.</li>
</ul>
<p>예를 들어</p>
<ul>
<li><p>플레이어가 2번째 공격에서 움직이지 않는 문제 발생.</p>
</li>
<li><p>스크린샷</p>
</li>
</ul>
<p>버그에 대한 내용과 스크린샷을 넣고 해결 방안을 제시한다거나 하는 등의</p>
<p>버그 리포트를 작성할 수 있다.</p>