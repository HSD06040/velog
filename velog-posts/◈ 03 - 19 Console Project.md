<h1 id="❖-프로젝트-기획">❖ 프로젝트 기획</h1>
<blockquote>
<p>상 하 좌 우 로 움직이는 콘솔프로젝트를 만드는 과제이다.</p>
</blockquote>
<p>일단 난 포켓몬을 매우 좋아하기 때문에 듣자마자 바로 포켓몬 빙판길 기믹이 떠올랐다.
그래서 일단 먼저 어떻게 구성하였냐면</p>
<ul>
<li><p>for문 사용
for 문으로 움직인 방향을 쭉 스캔해서 벽이 있다면 그 벽앞에 Player의 Position은 세팅하는 식으로 하였다.</p>
</li>
<li><p>리셋 기능
R을 눌러 리셋가능 하도록 하였다.
(여기서 map을 그냥 받아와서 원래맵에 깊은 복사하는 형식으로 하였는데 이러면 객체 자체가 생성되는 개념이고 이것으로 맵을 그려도 받아온 객체가 아닌 생성된 객체이기에 원래 초기맵을 그대로 복사하지 못한다고 해야하나? 일단 ref로 받아오니 생성개념이 아닌 값할당 개념으로 바뀌어서 맵 초기화가 잘 되었다.)</p>
</li>
<li><p>별 먹기
점수 시스템이 있으면 좋겠다고 생각해서 넣었다.</p>
</li>
</ul>
<h1 id="❖-코드">❖ 코드</h1>
<blockquote>
<p>객체지향이 아닌 절차지향이라 조금 길 수 있다.</p>
</blockquote>
<h3 id="✧-맵">✧ 맵</h3>
<pre><code class="language-cs">originalMaps = new int[,,]
{
    {
       // 1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16
/*1*/    {1, 1, 1, 1, 1, 1, 1, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8},
/*2*/    {1, 0, 0, 0, 1, 1, 1, 1, 8, 8, 8, 8, 8, 8, 8, 8, 8},
/*3*/    {1, 3, 4, 4, 1, 1, 1, 1, 8, 8, 8, 8, 8, 8, 8, 8, 8},
/*4*/    {1, 0, 0, 0, 4, 0, 0, 1, 1, 8, 8, 8, 8, 8, 8, 8, 8},
/*5*/    {1, 1, 0, 0, 0, 0, 0, 1, 1, 8, 8, 8, 8, 8, 8, 8, 8},
/*6*/    {1, 1, 0, 0, 0, 0, 0, 0, 1, 1, 1, 8, 8, 8, 8, 8, 8},
/*7*/    {1, 1, 1, 1, 0, 0, 0, 0, 0, 2, 1, 8, 8, 8, 8, 8, 8},
/*8*/    {8, 8, 8, 1, 0, 0, 0, 0, 0, 2, 1, 8, 8, 8, 8, 8, 8},
/*9*/    {8, 8, 8, 1, 0, 0, 0, 0, 4, 2, 1, 8, 8, 8, 8, 8, 8},
/*10*/   {8, 8, 8, 1, 1, 1, 1, 1, 1, 1, 1, 8, 8, 8, 8, 8, 8},
/*11*/   {8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8},
/*12*/   {8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8},
/*13*/   {8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8},
/*14*/   {8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8},
/*15*/   {8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8}
    },
};</code></pre>
<p>스테이지를 이런식으로 구현했다. 4개 더있다..</p>
<h3 id="✧-drawmap">✧ DrawMap</h3>
<pre><code class="language-cs">static void DrawMap(int[,,] mapTile)
{
    for (int i = 0; i &lt; mapTile.GetLength(1); i++)
    {
        for (int j = 0; j &lt; mapTile.GetLength(2); j++)
        {
            Console.BackgroundColor = ConsoleColor.Black;
            switch (mapTile[stageCount,i, j])
            {
                case 0:
                    Console.BackgroundColor = ConsoleColor.Cyan;
                    Console.Write(&quot;  &quot;);
                    break;
                case 1:
                    Console.Write(&quot;■&quot;);
                    break;
                case 2:
                    Console.BackgroundColor = ConsoleColor.Cyan;
                    Console.ForegroundColor = ConsoleColor.Black;
                    Console.Write(&quot;◎&quot;);
                    break;
                case 3:
                    Console.BackgroundColor = ConsoleColor.Cyan;
                    Console.ForegroundColor = ConsoleColor.Black;
                    Console.Write(&quot;◆&quot;);
                    break;
                case 4:
                    Console.BackgroundColor = ConsoleColor.Cyan;
                    Console.Write(&quot;★&quot;);
                    break;
                case 5:
                    Console.BackgroundColor = ConsoleColor.Cyan;
                    Console.ForegroundColor = ConsoleColor.Black;
                    Console.Write(&quot;●&quot;);
                    break;
                case 6:
                    Console.BackgroundColor = ConsoleColor.Cyan;
                    Console.ForegroundColor = ConsoleColor.Black;
                    Console.Write(&quot;○&quot;);
                    break;
                default:
                    Console.Write(&quot;  &quot;);
                    break;
            }
            Console.ResetColor();
        }
        Console.WriteLine();
    }
}</code></pre>
<p>이런 식으로 int 값에 맞는 그림을 그리도록 하였다.</p>
<h3 id="✧-movement">✧ Movement</h3>
<pre><code class="language-cs"> static void MoveUpDown(int vector , int[,,] map)
 {
     moveCount++;
     {
         for (int i = 0; i &lt; map.GetLength(1); i++)
         {
             if (vector &gt; 0)
             {
                 if (map[stageCount, playerPos.y - i, playerPos.x] == 1)
                 {
                     map[stageCount, playerPos.y, playerPos.x] = 0;
                     playerPos.y = playerPos.y - i + 1;
                     return;
                 }
                 else if (map[stageCount, playerPos.y - i, playerPos.x] == 4)
                 {
                     map[stageCount, playerPos.y - i, playerPos.x] = 0;
                     starCount++;
                 }
             } 
             else

             {
                 if (map[stageCount, playerPos.y + i, playerPos.x] == 1)
                 {
                     map[stageCount, playerPos.y, playerPos.x] = 0;
                     playerPos.y = playerPos.y + i - 1;
                     return;
                 }
                 else if (map[stageCount, playerPos.y + i, playerPos.x] == 4)
                 {
                     map[stageCount, playerPos.y + i, playerPos.x] = 0;
                     starCount++;
                 }
             }
         }
     }
 }
 static void MoveLeftRight(int vector ,int[,,] map)
 {
     moveCount++;

     for (int i = 0; i &lt; map.GetLength(1); i++)
     {
         if (vector &gt; 0)
         {
             if (map[stageCount, playerPos.y, playerPos.x + i] == 1)
             {
                 map[stageCount, playerPos.y, playerPos.x] = 0;
                 playerPos.x = playerPos.x + i - 1;
                 return;
             }
             else if (map[stageCount,playerPos.y,playerPos.x + i] == 4)
             {
                 map[stageCount, playerPos.y, playerPos.x + i] = 0;
                 starCount++;
             }
         }
         else
         {
             if (map[stageCount, playerPos.y, playerPos.x - i] == 1)
             {
                 map[stageCount, playerPos.y, playerPos.x] = 0;
                 playerPos.x = playerPos.x - i + 1;
                 return;
             }
             else if (map[stageCount, playerPos.y, playerPos.x - i] == 4)
             {
                 map[stageCount, playerPos.y, playerPos.x - i] = 0;
                 starCount++;
             }
         }
     }
 }</code></pre>
<p>방향을 정해주면 그 방향으로 검사하고 벽이 있으면 벽앞에 가는 식으로 구현하였다.
사실 조금 더 생각했으면 간소화가 가능해 보이긴하나 맵 만드는 시간이 많이 걸려 포기했다.</p>
<h3 id="✧-input">✧ Input</h3>
<pre><code class="language-cs">static void Input(ref int[,,] maps)
{
    ConsoleKey input = Console.ReadKey().Key;

    switch (input)
    {
        case ConsoleKey.UpArrow:
        case ConsoleKey.W:
            MoveUpDown(1, maps);
            break;
        case ConsoleKey.DownArrow:
        case ConsoleKey.S:
            MoveUpDown(-1, maps);
            break;
        case ConsoleKey.RightArrow:
        case ConsoleKey.D:
            MoveLeftRight(1,maps);
            break;
        case ConsoleKey.LeftArrow:
        case ConsoleKey.A:
            MoveLeftRight(-1,maps);
            break;
        case ConsoleKey.R:
            ResetGame(ref maps);
            break;   
    }
}</code></pre>
<p>간단하다 Input을 받아 맞는 것을 실행시킨다.</p>
<h3 id="✧-게임루프">✧ 게임루프</h3>
<pre><code class="language-cs">while (!gameOver)
{
    if (!isPlaying)
    {
        Start(spawnPos[stageCount]);
        isPlaying = true;
    }

    Console.Clear();
    DrawUI();
    SetupPlayerPos(maps);
    DrawMap(maps);
    Input(ref maps);
    CheckGameClear(maps);
}</code></pre>
<p>gameOver가 true가 될때까지 계속 반복한다.</p>
<h3 id="✧-checkclear">✧ CheckClear</h3>
<pre><code class="language-cs">static void CheckGameClear(int[,,] maps)
{
    if (maps[stageCount,playerPos.y, playerPos.x] == 2)
    {
        Console.Clear();
        DrawUI();
        SetupPlayerPos(maps);
        DrawMap(maps);
        Console.WriteLine($&quot;{stageCount+1} 스테이지 클리어!!!!!!!!!&quot;);

        stageCount++;

        if (stageCount &gt;= maps.GetLength(0))
        {
            totalMoveCount += moveCount;

            Console.WriteLine(&quot;\n----------------------------------------------------------------------------------------------&quot;);
            Console.WriteLine(&quot;\n준비된 스테이지가 모두 끝났습니다!&quot;);
            Console.WriteLine(&quot;\n----------------결과----------------&quot;);
            Console.WriteLine($&quot;\n총 움직인 횟수 : {totalMoveCount}&quot;);
            Console.WriteLine($&quot;\n총 먹은 별 갯수 : {starCount}&quot;);
            Console.WriteLine($&quot;\n최종 점수 : {(totalStarCount * 100) / totalMoveCount}&quot;);
            Console.WriteLine(&quot;\n----------------------------------------------------------------------------------------------&quot;);
            Console.WriteLine(&quot;\n &quot;);

            gameOver = true;
        }
        else
        {
            totalStarCount += starCount;
            totalMoveCount += moveCount;
            Console.WriteLine($&quot;\n{stageCount + 1} 스테이지를 시작하시려면 아무키나 누르세요.&quot;);
            Console.ReadKey();

            ResetGame(ref maps);
        }
    }
}
</code></pre>
<p>클리어를 판단해서 클리어라면 다음 스테이지로 넘어가고 만약 스테이지가 끝났다면 게임을 종료 시킨다.</p>