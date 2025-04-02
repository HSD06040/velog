<h1 id="❖-알고리즘이란">❖ 알고리즘이란?</h1>
<blockquote>
<p>연산이라는 뜻으로 문제를 해결하기 위한 과정을 논리적 절차에 따라 구성한 일련의 단계이다.</p>
</blockquote>
<p>입력값에 따라 다른 결과값이 출력이된다. 즉 함수이다.</p>
<p>컴퓨터는 우리가 절차를 정해주어야만 작동하기 때문에 알고리즘은 중요한 요소이다.</p>
<p>알고리즘의 조건</p>
<p>입력</p>
<ul>
<li>알고리즘은 0개 이상의 입력을 가져야한다.</li>
</ul>
<p>출력</p>
<ul>
<li>알고리즘은 최소 1개 이상의 결과를 가져야한다. (출력이 꼭 있어야한다.)</li>
</ul>
<p>명확성</p>
<ul>
<li>수행 과정은 정확한 수단을 제공해야한다.</li>
</ul>
<p>유한성</p>
<ul>
<li>수행과정은 무한하지 않고 유한한 작업 이후에 정지해야 한다. (끝이 있어야한다.)</li>
</ul>
<p>효과성</p>
<ul>
<li>모든 과정은 명백하게 실행 가능해야 한다.</li>
</ul>
<p>알고리즘의 설계단계</p>
<p>문제 이해 -&gt; 예시와 테스트 케이스 작성 -&gt; 알고리즘 설계 -&gt; 알고리즘 구현 및 검증 -&gt; 알고리즘 분석과 개선</p>
<p><strong>문제 이해</strong></p>
<ul>
<li>알고리즘은 풀어내기 위해 문제를 먼저 분석해야한다.</li>
</ul>
<p><strong>예시와 테스트 케이스 작성</strong></p>
<ul>
<li>해결하기 위해 입력값에 따른 결과값을 파악해 둘 필요가있다.</li>
</ul>
<p><strong>알고리즘 설계</strong></p>
<ul>
<li>문제를 해결하는 과정을 구상해 보는 과정이다.</li>
<li>최우선은 정확도이다. 다음이 효율, 그리고 BigO 값이다.</li>
<li>의사코드 혹은 흐름도를 먼저 그리는 것이 좋다.</li>
</ul>
<p><strong>알고리즘 구현 및 검증</strong></p>
<ul>
<li>구상한 설계과정으로 구현을 한 후 테스트 하여 검증한다.</li>
</ul>
<p><strong>알고리즘 분석과 개선</strong></p>
<ul>
<li>알고리즘에 대해 분석하여 개선해야할 부분(성능 최적화 등)을 찾아 수정한다.</li>
</ul>
<h2 id="✤-recursion-재귀">✤ Recursion (재귀)</h2>
<blockquote>
<p>어떠한 것을 정의할 때 자기 자신을 참조하는 것
 힘수를 정의할 때 자기자신을 호출하는 것이다.</p>
</blockquote>
<ul>
<li>재귀함수의 특징으로는 기저조건을 반드시 필요로한다.</li>
<li>장점으로는 코드로 표현하기 어려운 경우도 직관적인 처리가 가능하다.</li>
<li>분할정복으로 효율을 높일 수 있다.</li>
</ul>
<p><em>분할정복 - 큰 문제를 나눠서 풀고 나중에 합치는 것</em></p>
<p> 팩토리얼</p>
<pre><code class="language-cs"> int Factorial(int n)
 {
     if(n == 1) return n;

    return n * Factorial(n-1);
 }</code></pre>
<p> 상위폴더 삭제시 하위폴더 삭제</p>
<pre><code class="language-cs"> if(folders.folder.Count &lt;= 0)
     return;

 foreach(Folder folder in folders.folder)
 {
     Remove(folder);
 }</code></pre>
<p>재귀는 잘 사용하여야한다.
함수안의 함수 호출은 반복되다 보면 스텍 오버플로우를 일으킬 수 있기때문에
피보나치같은 깊은 재귀 O(log n^2) 는 피하는 것이 좋고 재귀 사용 시 깊이를 생각해 보는 것이 좋다.</p>
<p> 피보나치</p>
<pre><code class="language-cs"> public int Fibo(int n)
 {
     if(n == 2 || n == 1) return 1;
    return Fibo(n-1) + Fibo(n-2);
 }</code></pre>
<h1 id="❖-정렬-알고리즘">❖ 정렬 알고리즘</h1>
<h3 id="✧-선택-정렬selectionsort">✧ 선택 정렬(SelectionSort)</h3>
<blockquote>
<p>가장 작은 값부터 선택하여 정렬한다.</p>
</blockquote>
<p>시간 복잡도 - O(n^2)
공간 복잡도 - O(1)
불안정 정렬</p>
<pre><code class="language-cs">  public static void SelectionSort(int[] array)
 {
     for (int i = 0; i &lt; array.Length; i++)
     {
         int minIndex = i;                       // 최소 인덱스
         for (int j = i; j &lt; array.Length; j++)  // i번째 부터 확인 ( i번쨰는 이미 정렬됨 )
         {
             if (array[j] &lt; array[minIndex])     // array를 쭉 탐색하고 탐색하던 도중 작은수가 있다면 최소 
                 minIndex = j;                   // 그 index를 minIndex에 넣음.
         }
         Swap(array,i,minIndex);                 // 배열의 i번째 index를 minIndex번째 인덱스로 바꿈
     }                                           // 반복
 }</code></pre>
<h3 id="✧-삽입-정렬insertionsort">✧ 삽입 정렬(InsertionSort)</h3>
<blockquote>
<p>데이터를 하나씩 꺼내어 정렬된 자료 중 적합한 위치에 삽인하는 정렬</p>
</blockquote>
<p>시간 복잡도 - O(n^2)
공간 복잡도 - O(1)
안전정렬</p>
<pre><code class="language-cs"> public static void InsertionSort(int[] array)
{
    for (int i = 1; i &lt; array.Length; i++)
    {
        int key = array[i];                 // 비교할 키 설정
        int j = i - 1;                      // j = i의 앞에있는 index

        while (j &gt;= 0 &amp;&amp; array[j] &gt; key)    // j가 key 보다 작다면 ( key는 array[i] )
        {
            array[j + 1] = array[j];        // 한칸뒤로 밀어준다.
            j--;                            // j-- 하고 다시 while문에 확인
        }

        array[j + 1] = key;
    }
}</code></pre>
<h3 id="✧-버블-정렬bubblesort">✧ 버블 정렬(BubbleSort)</h3>
<blockquote>
<p>인접한 index를 비교해서 더 작으면 교체를 반복한다.</p>
</blockquote>
<ul>
<li>이러면 큰 것들이 점점 모이게된다.</li>
</ul>
<p>시간 복잡도 - O(n^2)
공간 복잡도 - O(1)
안전정렬</p>
<pre><code class="language-cs">public static void BubbleSort(int[] array)
{
    for(int i = 1; i &lt; array.Length; i++)
    {
        for(int j = 0; j &lt; array.Length - i; j++)   // 정렬된 것을 제외하여 반복
        {
            if (array[j] &gt; array[j+1])              // 인접한게 더 작다면
            {
                Swap(array,j,j+1);                  // 둘 교체
            }
        }
    }
}</code></pre>
<h3 id="✧-병합-정렬mergesort">✧ 병합 정렬(MergeSort)</h3>
<blockquote>
<ul>
<li>데이터를 2분할하여 정렬 후 병합한다.</li>
</ul>
</blockquote>
<ul>
<li>데이터는 나눠지면서 정렬이 되어</li>
<li>병합시 비교가 훨씬 간단해지고 빠르다. (이진 탐색)</li>
<li>많은 데이터의 정렬에 적합하다.</li>
<li>재귀하여도 피보나치처럼 엄청나게 깊은 재귀가 아니기 때문에 괜찮다.</li>
</ul>
<p>시간 복잡도 - O(nlogn)
공간 복잡도 - O(n)
안전정렬</p>
<pre><code class="language-cs"> public static void MergeSort(int[] array, int start, int end)
 {
     if (start == end) // 재귀 함수이니 기저조건이 필수
         return;

     int mid = (start + end) / 2;

     MergeSort(array, start, mid); // 1 ~ 중간까지 정렬
     MergeSort(array, mid+1, end); // 중간 +1 에서 끝까지 정렬 2개로 나눠서 정렬
     Merge(array, start, mid, end);
 }

 public static void Merge(int[] array) =&gt; MergeSort(array, 0, array.Length -1); // 처음부터 끝까지

 public static void Merge(int[] array, int start, int mid, int end)
 {
     List&lt;int&gt; sortedList = new List&lt;int&gt;();
     int leftIndex = start;
     int rightIndex = mid + 1;

     while (leftIndex &lt;= mid &amp;&amp; rightIndex &lt;= end)
     {
         if (array[leftIndex] &lt; array[rightIndex])
         {
             sortedList.Add(leftIndex);
             leftIndex++;
         }
         else
         {
             sortedList.Add(rightIndex);
             rightIndex++;
         }
     }

     if(leftIndex &lt;= mid)
     {
         while (leftIndex &lt;= mid)
         {
             sortedList.Add(leftIndex);
             leftIndex++;
         }
     }
     else
     {
         while (rightIndex &lt;= end)
         {
             sortedList.Add(array[rightIndex]);
             rightIndex++;
         }
     }

     for (int i = 0; i &lt; sortedList.Count; i++)
     {
         array[start + i] = sortedList[i];
     }

 }</code></pre>
<h3 id="✧-퀵정렬">✧ 퀵정렬</h3>
<blockquote>
<ul>
<li>하나의 기준(피벗)으로 작은값과 큰값을 2분할하여 정렬한다.</li>
</ul>
</blockquote>
<ul>
<li>left는 피벗보다 큰값을 만날때까지 가고 right는 작은값을 만날때 까지 간다.</li>
<li>만약 둘다 멈췄다면 left와 right를 바꿔준다.</li>
<li>그리고 left와 right가 교차하면 피벗과 rigth를 바꿔준다.</li>
<li>이러면 작은값은 왼쪽에 모이고 큰값은 오른쪽에 모인다.</li>
<li>그 후 작은값을 정렬해주고 큰값을 정렬해준다.</li>
</ul>
<p>시간복잡도 - O(nlogn)     최악 : O(n^2)
공간복잡도 - O(1)
불안전 정렬</p>
<pre><code class="language-cs">public static void QuickSort(int[] array) =&gt; QuickSort(array, 0, array.Length - 1);

public static void QuickSort(int[] array,int start, int end)
{
    if (start &gt;= end)
        return;

    int pivot = start;
    int left  = pivot + 1;
    int right = end;

    while (left &lt;= right)
    {
        while (array[left] &lt;= array[pivot] &amp;&amp; left &lt; right)
        {
            left++;
        }

        while (array[right] &gt; array[pivot] &amp;&amp; left &lt;= right)
        {
            right--;
        }
        if(left &lt; right)
        {
            Swap(array, left, right);
        }
        else
        {
            Swap(array, pivot, right);
            break;
        }
    }

    QuickSort(array, start, right - 1);
    QuickSort(array, right+1, end);
}</code></pre>
<h3 id="✧-힙-정렬">✧ 힙 정렬</h3>
<blockquote>
<ul>
<li>최대 값이나 최소 값을 계속 뽑아주는 방식으로 정렬한다.</li>
</ul>
</blockquote>
<ul>
<li>반분할이 반드시 가능하다.</li>
<li>힙을 이용하여 우선순위가 가장 높은 요소가 가장 마지막 요소와 교체된 후 제거되는 방법을 이용한다.</li>
<li>토너먼트를 하기 위해서 배열의 위치를 불규칙적이게 사용하기 때문에</li>
<li>배열에서 연속적인 데이터를 사용하지않아 캐시 메모리를 효율적으로 사용할 수 없어 상대적으로 느리다.</li>
<li>컴퓨터는 캐시메모리 때문에 연속적인 데이터의 처리속도가 더 빠르다.</li>
</ul>
<p>CPU는 캐시 L1~L3 순으로 빠르게 가져올 수 있는데 이 캐시들의 용량이 크지 않아서 많이 들고오지못한다.
그래서 힙 정렬에 필요한 메모리를 전부 한번에 들고오지 못하기 때문에 느리다.
즉 캐시 적중률이 낮아 느려서 잘 쓰지않는다. (Linked List도 비슷한 구조)</p>
<ul>
<li>시간 복잡도 - O(nlogn)</li>
<li>공간 복잡도 - O(1)</li>
<li>불안전 정렬</li>
</ul>
<pre><code class="language-cs">public static void HeapSort(IList&lt;int&gt; list)
{
    MakeHeap(list);

    for (int i = list.Count - 1; i &gt; 0; i--)
    {
        Swap(list, 0, i);
        Heapify(list, 0, i);
    }
}

private static void MakeHeap(IList&lt;int&gt; list)
{
    for (int i = list.Count / 2 - 1; i &gt;= 0; i--)
    {
        Heapify(list, i, list.Count);
    }
}

private static void Heapify(IList&lt;int&gt; list, int index, int size)
{
    int left = index * 2 + 1;
    int right = index * 2 + 2;
    int max = index;
    if (left &lt; size &amp;&amp; list[left] &gt; list[max])
    {
        max = left;
    }
    if (right &lt; size &amp;&amp; list[right] &gt; list[max])
    {
        max = right;
    }

    if (max != index)
    {
        Swap(list, index, max);
        Heapify(list, max, size);
    }
}</code></pre>
<h1 id="❖-정리">❖ 정리</h1>
<p>그럼 뭘써야할까? 당연히 상황마다 다르겠지만...</p>
<ul>
<li>C#에서는 Sort라는 정렬이 존재한다. 보통 이걸쓴다.</li>
<li>C# Sort는 퀵 정렬, 힙 정렬, 삽입 정렬이 하이브리드된 정렬인 인트로 정렬이다.</li>
</ul>
<h2 id="✤-sort의-행동">✤ Sort의 행동</h2>
<ul>
<li>기본적으로는 퀵 정렬을 수행한다.</li>
<li>크기 16보다 작아지면 삽입 정렬을 한다.</li>
<li>n^2 (최악) 으로 점점 시간복잡도가 늘어나면 힙 정렬도 바꾼다.</li>
</ul>
<p>처럼 Sort는 상황에 필요한 정렬을 자동으로 사용한다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/hsd0604/post/e16a8017-b557-4123-9fe3-975b22955c85/image.png" /></p>
<ul>
<li>크기가 작으면 nlogn 보다 n^2이 빠르다.</li>
<li>nlogn은 분할하는 사전작업이 필요하기 때문에 작은 수에서는 불필요한 분할이 일어난다.</li>
<li>그래서 작은 수에는 오히료 n^2가 더빠른 상황이 일어난다.</li>
</ul>
<ul>
<li>삽입 정렬은 배열(Index)만 정렬이 가능하다.</li>
<li>버블 정렬은 인접한 것들로 가능해서 LinkedList는 버블정렬을 쓴다.</li>
<li>두 정렬은 이미 정렬되어 있다면 BigO(n)으로 정렬이 가능하다. ( 최적화 )</li>
</ul>
<h2 id="✤-안전-정렬--불안정-정렬">✤ 안전 정렬 / 불안정 정렬</h2>
<ul>
<li><p>이전 정렬이 유지가 되는경우 이다.</p>
</li>
<li><p>이는 정렬의 방식으로 인해 달라지는데</p>
</li>
<li><p>만약 앞 뒤가 섞는다면 불안정 정렬이고, 차례대로 정렬만 된다면 안정정렬이 된다.</p>
</li>
<li><p>상대적으로 안전정렬이 불안정 정렬보다 오래걸린다.</p>
</li>
</ul>
<p>결국 UI (유저한테 보이는 데이터들)은 안정 정렬이 되어야 할 것이고
보이지 않고 빠르게 데이터를 정렬해야할 떄에는 불안정 정렬을 써야할 것이다.</p>