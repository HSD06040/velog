<h1 id="❖-hashtable">❖ HashTable</h1>
<blockquote>
<p>Key와 Value로 이루어진 자료구조이다.</p>
</blockquote>
<p>우리가 사전에서 첫 철자를 보고 정보를 찾는것과 같이 Key를 통해 Value를 찾는 개념이다.
그래서 빠르게 탐색이 가능하다.</p>
<h2 id="✤-key-value-hashdata개념">✤ Key Value HashData개념</h2>
<p><strong>key</strong></p>
<ul>
<li>value를 찾기위한 값</li>
<li>해싱이 적용된다.</li>
</ul>
<p><strong>value</strong></p>
<ul>
<li>실제 반환될 값</li>
</ul>
<p><strong>HashData</strong></p>
<ul>
<li>key값이 Hash함수로 변형된 값</li>
</ul>
<h2 id="✤-hash란">✤ Hash란?</h2>
<blockquote>
<p>뭉친다라는 의미이고 해시태그 처럼 찾기위한 수단의 의미를 가진다.</p>
</blockquote>
<ul>
<li>임의의 크기를 가진 데이터를 고정된 크기의 값으로 변환하는 과정이다.
이 변환 과정에서 사용되는 수학적 알고리즘을 해시 함수 라고한다.</li>
</ul>
<p><strong>단방향성</strong></p>
<ul>
<li>키 값으로 해시 값을 구할 수 있지만, 해시 값으로 원래값을 복원하는건 어렵다.</li>
</ul>
<p><strong>고유성</strong></p>
<ul>
<li>다른 키값은 일반적으론 다른 해쉬 값을 가지지만 같은 해시값이 나오는 충돌이 발생할 수도 있다.</li>
<li>다른값이 해시값으로 변환하는 과정(해시함수)에서 같은 해시테이블 주소를 반환할 수도 있다.</li>
</ul>
<p><strong>고정된 길이</strong></p>
<ul>
<li>키값의 크기와 관계없이 해시 값은 항상 일정한 크기를 가진다.</li>
</ul>
<h2 id="✤-hash-함수란">✤ Hash 함수란?</h2>
<ul>
<li>키값을 해싱해서 고유한 Index를 만드는 함수이다.</li>
<li>하나의 키값을 해싱하는 경우 반드시 항상 같은 Index를 반환해야한다.</li>
<li>대표적인 해시함수로 나눗셈법이 있다.</li>
</ul>
<p><strong>해쉬된 값은 Bucket의 Index를 결정하는 데에 사용된다.</strong></p>
<h2 id="✤-hash-충돌-해결법">✤ Hash 충돌 해결법</h2>
<blockquote>
<p>해시 테이블에서의 충돌은 피할 수 없기에 해결방법이 존재한다.</p>
</blockquote>
<h3 id="✧-체이닝">✧ 체이닝</h3>
<ul>
<li>같은 Index에 여러 개의 데이터를 저장할 수 있도록한다.</li>
<li>Linked List를 활용한다.</li>
<li>충돌이 많아도 효율적이다.</li>
<li>다만 추가적인 저장공간으로 인해 메모리의 사용량이 증가하고, 삽입삭제시 오버헤드가 발생한다.</li>
</ul>
<h3 id="✧-개방주소법">✧ 개방주소법</h3>
<ul>
<li>충돌 시 다른 빈 공간에 데이터를 저장한다.</li>
<li>선형탐색, 제곱탐색, 이중해시 등을 통하여 빈공간을 선정한다.</li>
<li>추가적인 저장공간을 필요로 하지않기에 메모리를 추가로 사용하지않고, 삽입 삭제에서 오버헤드가 적다.</li>
<li>하지만 자료사용률에 따라 성능저하가 발생한다.(클러스터링 문제가 발생한다.)</li>
</ul>
<p><strong>클러스터링</strong></p>
<ul>
<li>연속된 빈 공간이 줄어들어 충돌 가능성이 증가한다.</li>
</ul>
<h4 id="선형탐사">선형탐사</h4>
<ul>
<li>충돌이 발생하면 그 다음 빈 공간을 찾아서 저장한다.</li>
<li>간단하고 메모리를 추가로 사용하지는 않는다.</li>
<li>하지만 클러스터링 문제가 발생한다.</li>
</ul>
<h4 id="이중-해싱">이중 해싱</h4>
<ul>
<li>충돌 시, 새로운 해시 함수를 사용하여 다른 위치를 찾는다.</li>
<li>클러스터링 문제가 없다.</li>
<li>하지만 연산량이 증가한다.</li>
</ul>
<h4 id="제곱-탐색">제곱 탐색</h4>
<ul>
<li>만약 충돌 시 다른 공간을 탐색하는데</li>
<li>제곱단위로 위치를 선정한다.</li>
</ul>
<p>즉 개방주소법은 빠르지만 사용률이 높아지면 </p>
<h2 id="✤-hashtable의-효율">✤ HashTable의 효율</h2>
<ul>
<li>사용륭이 70퍼 이상되었을 때 급격한 성능저하가 발생한다.</li>
<li>재해싱을 통해 공간 사용률을 낮추어 다시 효율을 확보함</li>
<li>재해싱 : 해시테이블의 크기를 늘리고 테이블 내의 모든 데이터를 다시 해싱하여 보관</li>
<li>그냥 복사가 아니고 모든 데이터를 재해싱하여 위치를 배치해주어야한다.</li>
</ul>
<p>HashTable의 추가 삭제는 O(1)이라는 시간복잡도를 가지고있다. 하지만</p>
<ul>
<li><strong>HashTable의 제거는 그 자리를 못쓰도록 한다는 의미의 제거이다.</strong></li>
<li>추가 삭제가 여러번 일어나면 쓸 수 없는 공간들이 늘어나고 공간사용률이 낮아지게 된다.</li>
<li>그래서 재해싱을 하여 이 빈공간들을 정상화하는 과정이 필요하다.</li>
</ul>
<p>처럼 효율이 좋아보여도 이러한 과정때문에 </p>
<h2 id="✤-entry와-bucket">✤ Entry와 Bucket</h2>
<h3 id="✧-entry">✧ Entry</h3>
<ul>
<li>Key와 Value 저장되는 곳이다.</li>
<li>저장 및 검색이 가능하다.</li>
</ul>
<p><strong>저장</strong></p>
<ul>
<li>저장은 키를 해시 함수에 입력해서 Bucket Index를 찾는다.</li>
<li>해당 Bucket Index에 이미 데이터가 있다면 충돌 해결 방법에 따라 추가로 저장된다.</li>
</ul>
<p><strong>검색</strong></p>
<ul>
<li>키를 해시 함수에 입력해서 Bucket Index를 찾는다.</li>
<li>Bucket 내부에서 키와 매칭되는 데이터를 검색한다.</li>
</ul>
<p><strong>Bucket</strong></p>
<ul>
<li><p>해시 테이블의 특정 Index에 저장되는 Entry들의 집합이다.</p>
</li>
<li><p>하나의 버킷에 여러개의 Entry가 들어갈 수 있다.</p>
</li>
<li><p>해시 충돌이 발생하면 같은 인덱스에 여러 개의 </p>
</li>
</ul>
<p>데이터의 보관용도등 많은 곳에서 사용가능하다.</p>
<h1 id="❖-dictionary란">❖ Dictionary란?</h1>
<blockquote>
<p>사전이라는 뜻이고 HashTable 자료구조이다.</p>
</blockquote>
<p>같은 키를 입력하면 에러가 발생한다.
해싱이 될때 겹칠 가능성은 있지만 애초에 key값이 같다면 에러가 일어난다.</p>
<p><strong>Add</strong>
-Key에 해당하는 Value 추가</p>
<p><strong>Remove</strong>
-Key에 해당하는 Value 지우기</p>
<p><strong>TryAdd</strong>
-Key에 해당하는 Value추가 시도</p>
<p><strong>ContainsKey</strong></p>
<ul>
<li>Key에 해당하는 Value를 반환한다.</li>
</ul>
<p><strong>TryGetValue</strong></p>
<ul>
<li>키 값이 있는지 확인 후 있다면 Value를 반환한다.</li>
</ul>
<p>배열의 Index같은 기능을 지원을 해서 Key값을 넣는것을 지원한다.</p>