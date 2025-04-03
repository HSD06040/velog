<h1 id="❖-앞서">❖ 앞서</h1>
<ul>
<li><ol>
<li>시작할 위치를 정하고 방문한 셀로 표시한다.</li>
</ol>
</li>
<li><ol start="2">
<li><p>현재 위치에서 인접한 4방향의 셀 중 방문하지 않은 셀을 랜덤하게 선택한다.</p>
<ul>
<li><p>2-1. 선택한 셀로 이동하고, 두 셀 사이의 벽을 허문다.</p>
</li>
<li><p>2-2. 새로운 셀을 현재 위치로 설정하고 방문한 셀로 표시한다.</p>
</li>
<li><p>2-3. 새로운 위치에서 다시 탐색을 진행한다.</p>
</li>
</ul>
</li>
</ol>
</li>
<li><ol start="3">
<li>이동할 수 있는 곳이 없으면 이전 위치(스택의 이전 값)로 되돌아간다.</li>
</ol>
</li>
<li><ol start="4">
<li>모든 셀이 방문될 때까지 반복한다.</li>
</ol>
</li>
<li><ol start="5">
<li>이렇게 모든 셀을 돌다 보면 미로가 완성된다.</li>
</ol>
</li>
</ul>
<h2 id="✤-unity-미로-생성-알고리즘">✤ Unity 미로 생성 알고리즘</h2>
<pre><code class="language-cs">using System.Collections;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using UnityEngine.UIElements;

public class MazeGenerator : MonoBehaviour
{
    [SerializeField] private MazeCell mazeCell;    // 프리팹

    [SerializeField] private int mazeWidth, mazeHeight;  // 미로의 크기 Inspector에서 설정가능

    private MazeCell[,] mazeGrid; // 미로

    IEnumerator Start()
    {
        mazeGrid = new MazeCell[mazeWidth, mazeHeight];

        // 전부 막혀져 있는 미로를 생성
        for (int x = 0; x &lt; mazeWidth; x++)
        {
            for (int z = 0; z &lt; mazeHeight; z++)
            {
                mazeGrid[x, z] = Instantiate(mazeCell, new Vector3(x, 0, z), Quaternion.identity);
            }
        }

        yield return GenerateMaze(mazeGrid[0,0]);

        SetEntranceAndExit();
    }

    /// &lt;summary&gt;
    /// 실제 미로를 생성(뚫어주는) 하는 코루틴. DFS,Backtracking 활용 알고리즘
    /// &lt;/summary&gt;
    IEnumerator GenerateMaze(MazeCell startCell)
    {
        Stack&lt;MazeCell&gt; cellStack = new Stack&lt;MazeCell&gt;(); 
        startCell.Visit();          // 시작셀은 방문처리
        cellStack.Push(startCell);  // 시작 셀을 스텍에 추가

        while (cellStack.Count &gt; 0)    // 셀 스텍이 없을때 까지 반복
        {
            MazeCell currentCell = cellStack.Peek();    // 스택의 가장 위에 있는 현재 셀을 가져옴
            List&lt;MazeCell&gt; unVisitedCells = GetUnvisitedCells(currentCell).ToList();

            if(unVisitedCells.Count &gt; 0)
            {
                MazeCell nextCell = unVisitedCells[Random.Range(0, unVisitedCells.Count)]; // 인접 랜덤 셀을 가져옴
                ClearWalls(currentCell, nextCell);  // 현재셀과 다음 셀을 비교해서 벽을 제거
                nextCell.Visit();   // 다음셀을 방문처리
                cellStack.Push(nextCell);   //다음 셀을 스텍에 추가
                yield return new WaitForSeconds(.03f);  // 미로 생성 주기
            }
            else // 방문할 곳이 없다면
            {
                cellStack.Pop();    // 스텍에서 제일 위를 제거하여 이전으로 돌아가서 다시 실행
            }
        }    
    }

    /// &lt;summary&gt;
    /// 주변셀중 랜덤하게 다음 셀을 지정한다.
    /// &lt;/summary&gt;
    private MazeCell GetNextUnvisitedCell(MazeCell currentCell)
    {
        IEnumerable&lt;MazeCell&gt; unVisitedCells = GetUnvisitedCells(currentCell);

        return unVisitedCells.OrderBy(_ =&gt; Random.value).FirstOrDefault();  // 랜덤한 값으로 정렬 후 첫번째 요소를 반환
    }


    /// &lt;summary&gt;
    /// 현재 셀의 주변 셀 중 방문하지 않은 셀 확인
    /// &lt;/summary&gt;
    private IEnumerable&lt;MazeCell&gt; GetUnvisitedCells(MazeCell currentCell)
    {
        int x = (int)currentCell.transform.position.x;      // 현재 x
        int z = (int)currentCell.transform.position.z;      // 현재 z

        if(x+1 &lt; mazeWidth)             // 오른쪽이 맵 끝이 아니라면
        {
            MazeCell cellToRight = mazeGrid[x+1,z];

            if(!cellToRight.isVisited)
                yield return cellToRight;
        }

        if(x-1 &gt;= 0)                    // 왼쪽이 맵 끝이 아니라면
        {
            MazeCell cellToLeft = mazeGrid[x-1,z];

            if(!cellToLeft.isVisited)
                yield return cellToLeft;
        }

        if(z+1 &lt; mazeHeight)            // 위쪽이 맵 끝이 아니라면
        {
            MazeCell frontCell = mazeGrid[x, z + 1];

            if(!frontCell.isVisited)
                yield return frontCell;
        }

        if(z-1 &gt;= 0)                    // 아래쪽이 맵 끝이 아니라면
        {
            MazeCell backCell = mazeGrid[x, z - 1];

            if(!backCell.isVisited)
                yield return backCell;
        }
    }

    private void ClearWalls(MazeCell previousCell, MazeCell currentCell)
    {
        if (previousCell == null)
            return;

        // 이전 셀의 위치가 현재 셀의 왼쪽에 있는지 확인
        if(previousCell.transform.position.x &lt; currentCell.transform.position.x)
        {
            // 그럼 왼쪽에서 오른쪽으로 진행 되기때문에
            previousCell.ClearWalls(WallType.Right); // 이전의 오른쪽벽 삭제
            currentCell.ClearWalls(WallType.Left);   // 현재의 왼쪽벽 삭제로 연결해줌
            return;
        }

        if(previousCell.transform.position.x &gt; currentCell.transform.position.x)
        {
            previousCell.ClearWalls(WallType.Left);
            currentCell.ClearWalls(WallType.Right);
            return;
        }

        if(previousCell.transform.position.z &gt; currentCell.transform.position.z)
        {
            previousCell.ClearWalls(WallType.Back);
            currentCell.ClearWalls(WallType.Front);
            return;
        }

        if(previousCell.transform.position.z &lt; currentCell.transform.position.z)
        {
            previousCell.ClearWalls(WallType.Front);
            currentCell.ClearWalls(WallType.Back);
            return;
        }
    }
    private void SetEntranceAndExit()
    {
        MazeCell entrance = mazeGrid[0, Random.Range(0, mazeHeight)];
        entrance.ClearWalls(WallType.Left);                         // 왼쪽 벽 삭제.

        MazeCell exit = mazeGrid[mazeWidth - 1, Random.Range(0, mazeHeight)];
        exit.ClearWalls(WallType.Right);                            // 오른쪽 벽 삭제.
    }
}</code></pre>
<h2 id="✤-셀">✤ 셀</h2>
<pre><code class="language-cs">using UnityEngine;

public enum WallType
{
    Left, Right, Front, Back
}

public class MazeCell : MonoBehaviour
{
    [SerializeField] private GameObject leftWall;
    [SerializeField] private GameObject rightWall;
    [SerializeField] private GameObject frontWall;
    [SerializeField] private GameObject backmWall;
    [SerializeField] private GameObject unVisitedBlock;
    public bool isVisited { get; private set; }

    public void Visit()
    {
        isVisited = true;
        unVisitedBlock.SetActive(false);
    }

    public void ClearWalls(WallType walltype)
    {
        switch (walltype)
        {
            case WallType.Left:
                leftWall.SetActive(false); break;
            case WallType.Right:
                rightWall.SetActive(false); break;
            case WallType.Front:
                frontWall.SetActive(false); break;
            case WallType.Back:
                backmWall.SetActive(false); break;
        }
    }
}</code></pre>
<p><a href="https://www.youtube.com/watch?v=_aeYq5BmDMg&amp;t=27s">참고 영상</a></p>
<ul>
<li>재귀로 구현되어있어서 스텍플로우가 발생할 가능성이 있었기 떄문에</li>
<li>Stack으로 바꾸어 DFS + BackTracking 구현.</li>
</ul>