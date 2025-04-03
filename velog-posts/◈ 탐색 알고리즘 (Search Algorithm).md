<h1 id="❖-이진-탐색-알고리즘">❖ 이진 탐색 알고리즘</h1>
<ul>
<li>배열의 원소가 모두 정렬되있어야 성립할 수 있는 탐색 방법이다.</li>
<li>2분할 하여 데이터를 찾는다.</li>
<li>중간 값으로 가서 찾고하는 값이 더 크다면 오른쪽에 있다는 것이니 왼쪽을 무시한다.</li>
<li>그럼 시작값을 현재 중간 값으로 옮기고 또 중간값으로 가서 비교한다.</li>
<li>이걸 중간값이 찾는 값과 같을때 까지 반복한다.</li>
</ul>
<p>시간 복잡도 - O(logn)</p>
<pre><code class="language-cs">public static int BinarySearch(int[] array, int target)
{
    int min, mid , max;

    min = 0;
    max = array.Length - 1;

    while (min &lt;= max)
    {
        mid = (min + max) / 2;

        if (array[mid] == target)
        {
            Console.WriteLine(mid);
            return mid;
        }

        else if (array[mid] &lt; target) //만약 target이 더 크다면
            min = mid + 1;          // 시작 값을 중간값 으로 옮김
        else                        // target이 더 작다면
            max = mid - 1;          // 끝 값을 중간값으로 옮김
    }

    return -1;
}</code></pre>
<p>이진 탐색은 정렬이 되어있어야 한다는 조건이 있는 만큼 요소가 많을때 좋은 효율을 보인다.</p>
<h1 id="❖-비선형-자료구조-탐색">❖ 비선형 자료구조 탐색</h1>
<h2 id="✤-bfs">✤ BFS</h2>
<ul>
<li>Queue를 이용하여 구현이 가능하다.</li>
</ul>
<p>장점</p>
<ul>
<li>최단 경로를 보장한다.</li>
</ul>
<p>단점</p>
<ul>
<li>현재 탐색에 불필요한 데이터도 보관하고 있어야만한다.</li>
<li>최악의 경우 모든 정점을 메모리에 저장해야한다.</li>
</ul>
<p>사용예시</p>
<ul>
<li>최단경로를 필요로하는 그래프에 사용한다.</li>
</ul>
<pre><code class="language-cs"> // 너비 우선 탐색
 // 그래프의 분기를 만났을 때 모든 분기를 탐색한 뒤
 // 다음 분기로 넘어가는 형식이다.
 public static void BFS(bool[,] graph, int start, out bool[] visited, out int[] parents)
 {
     int size = graph.GetLength(0);
     visited = new bool[size];
     parents = new int[size];

     for (int i = 0; i &lt; size; i++)
     {
         visited[i] = false;
         parents[i] = -1;
     }

     Queue&lt;int&gt; queue = new Queue&lt;int&gt;();
     queue.Enqueue(start);
     visited[start] = true;

     while (queue.Count &gt; 0)
     {
         int current = queue.Dequeue();

         for (int j = 0; j &lt; size; j++) // 탐색할 정점을 기준으로 탐색할 수 있는 정점들을 찾는다.
         {
             if (graph[current, j] &amp;&amp; !visited[j] &amp;&amp; parents[j] == -1) // 연결되어있고 방문하지 않았던 정점일때
             {
                 queue.Enqueue(j);        // 탐색할 수 있는 정점을 큐에 추가한다.
                 visited[j] = true;       // 방문표시를 한다.
                 parents[j] = current;    // 부모가 누구인지를 저장한다.(누구에 의해 찾아졌는지)
             }
         }
     }
 }</code></pre>
<h2 id="✤-dfs">✤ DFS</h2>
<ul>
<li>Stack과 재귀를 이용하여 구현이 가능하다.(함수가 곧 스텍의 행동이기 떄문)</li>
</ul>
<p>장점</p>
<ul>
<li>현재 탐색중인 경로 메모리만을 담을 수 있어서 메모리 효율이 좋다.</li>
<li>함수 호출 스텍이라 속도가 빠르다.</li>
</ul>
<p>단점</p>
<ul>
<li>최단 경로를 보장해주지 않는다.</li>
</ul>
<p>사용예시</p>
<ul>
<li>트리의 구조상 최단경로가 하나 밖에 없어 깊이를 우선하여도 최단경로를 보장할 수 있고</li>
<li>빠른 속도라는 이점만을 가져올 수 있기때문이다.</li>
<li>Behavior Tree (행동 트리)</li>
</ul>
<pre><code class="language-cs">public static void DFS(bool[,] graph, int start, out bool[] visited, out int[] parents )
{
    int size = graph.GetLength(0);
    visited = new bool[size];
    parents = new int[size];

    for(int i = 0;i &lt; size;i++)
    {
        visited[i] = false;
        parents[i] = -1;
    }

    SearchNode(graph, start, visited, parents); // 시작노드에서 인접한 노드 검색
}

public static void SearchNode(bool[,] graph, int vertex, bool[] visited, int[] parents)
{
    int size = graph.GetLength(0);    // graph size
    visited[vertex] = true;

    for (int i = 0; i &lt; size; i++)
    {
        if (graph[vertex,i] &amp;&amp; !visited[i]) // 정점에서 i번째 노드와 연결이 되어있다면 (기저 조건)
        {
            parents[i] = vertex;
            SearchNode(graph, i, visited, parents);        // i번째 노드의 연결노드를 찾음.(재귀)
        }
    }
}</code></pre>
<h2 id="✤-다익스트라-알고리즘">✤ 다익스트라 알고리즘</h2>
<blockquote>
<p>가중치를 부여하여 가장 가중치가 낮은 경로를 탐색하는 알고리즘이다.</p>
</blockquote>
<ul>
<li>인접한 노드들 중 가장 짧은 최단경로를 찾는다.</li>
<li>직접가는 가중치로 거쳐가는 가중치로 바꾼다.</li>
<li>경로중에서 가장 가중치가 낮은 노드부터 확인한다.</li>
<li>경로가 더 짧은 경우에는 경로를 대체한다.</li>
<li>그 노드의 최단경로 가중치를 확인하면 그 노드와 연결된 다른노드와의 최단경로를 확인할때</li>
<li>이 노드의 최단경로 + 그 노드로가는 가중치를 하여 계산하는 방식으로 반복한다.</li>
</ul>
<pre><code class="language-cs">public static void Dijkstra(int[,] graph, int start,out bool[] visited, out int[] parents, out int[] cost)
{
    int size = graph.GetLength(0);
    visited = new bool[size];
    parents = new int[size];
    cost = new int[size];

    for (int i = 0; i &lt; size; i++)
    {
        visited[i] = false;
        parents[i] = -1;
        cost[i] = int.MaxValue;
    }

    cost[start] = 0;

    for(int i = 0; i &lt; size; i++)
    {
        int minIndex = -1;          
        int minCost = int.MaxValue;

        for(int j = 0; j &lt; size; j++)
        {
            if (!visited[j] &amp;&amp; cost[j] &lt; minCost) // 방문한적 없는 정점 중 가장 가까운 정점선택
            {
                minCost = cost[j];
                minIndex = j;
            }
        }
        if (minIndex &lt; 0)
            break;

        visited[minIndex] = true;

        for(int j = 0;j &lt; size; j++) // 만약 선택한 정점이 다른 정점보다 짧으면 대체
        {
            // cost[j] :        &quot;목적지&quot; 까지 직접 연결된 거리 (AB)
            // cost[minIndex] : &quot;중간점&quot; 까지 직접연결된 거리 (AC)
            // graph[minIndex,j]: 중간점 부터 목적지 까지의 거리 (CB)
            if (cost[j] &gt; cost[minIndex] + graph[minIndex, j])
            {
                cost[j] = cost[minIndex] + graph[minIndex, j];
                parents[j] = minIndex;
            }
        }
    }
}</code></pre>