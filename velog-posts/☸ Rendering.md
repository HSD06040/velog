<h1 id="❖-앞서">❖ 앞서</h1>
<blockquote>
<p>렌더링이란 큰 카테고리 안에서 렌더링의 과정을 설명할때에 컴퓨터 구조를 간단히 알고 가면좋다.</p>
</blockquote>
<h1 id="❖-렌더링-파이프라인">❖ 렌더링 파이프라인</h1>
<blockquote>
<p>컴퓨터가 모니터에 화면을 출력할때 까지의 단계와 과정이다.</p>
</blockquote>
<p>렌더링 파이프라인의 순서는 다음과 같다.
<img alt="" src="https://velog.velcdn.com/images/hsd0604/post/2aa9b98b-a05f-46da-84eb-241d240236aa/image.png" /></p>
<h2 id="✤-1-입력-어셈블리input-assembly">✤ 1. 입력 어셈블리(Input Assembly)</h2>
<blockquote>
<p>CPU가 정점버퍼(Vertex Buffer)를 GPU에게 전달하는 과정이다. </p>
</blockquote>
<ul>
<li>이 정점버퍼는 위치,노말,UV,색상을 직렬화된 형태로 담고있다.</li>
<li>GPU는 이 정점버퍼로 삼각형과 같은 도형을 조립한다. (그렇기에 입력 조립 이라고도 한다.)</li>
<li>GPU가 CPU에게 직접적인 접근이 불가능하기에 이루어지는 과정이다.</li>
</ul>
<h2 id="✤-2-정점-셰이더vertexshader">✤ 2. 정점 셰이더(VertexShader)</h2>
<p>정점 셰이더는 모든 정점이 변환되어 화면 공간으로 이동되는 과정이다.
이 단계에서 정점의 위치를 변환하고, 조명 효과를 적용하며, 텍스처 좌표를 계산하는 등의 작업을 수행한다.
정점 셰이더의 출력은 다음 단계에서 사용될 정점 데이터(변환된 위치, 색상 등)이다.</p>
<h2 id="✤-3-레스터라이저rasterizer">✤ 3. 레스터라이저(Rasterizer)</h2>
<blockquote>
<ul>
<li>레스터라이저는 정점 셰이더의 출력을 기반으로 화면에 표시할 픽셀로 변환하는 단계이다. 즉 삼각형이 픽셀로 변환된다. </li>
</ul>
</blockquote>
<ul>
<li><p>시야에 필요없는 오브젝트의 정리, 화면에 출력될 데이터 보간 등이 이루어지는 단계이기도 하다.</p>
</li>
<li><p>이 과정에서 삼각형과 같은 도형이 화면의 픽셀 좌표로 변환되고, 각 픽셀에 대한 색상 및 깊이 정보가 계산된다.</p>
</li>
<li><p>도형의 경계와 내부를 결정하여 어떤 픽셀이 도형의 일부인지 판단한다.</p>
</li>
</ul>
<h4 id="✦-클리핑">✦ 클리핑</h4>
<blockquote>
<p>화면에 보이지않는 객체는 우리가 렌더링 할 필요가 없다. 이를 실행하는 것이 클리핑이다.</p>
</blockquote>
<ul>
<li>참고로 멀티플레이 환경에서는 보통은 각 클라이언트에서 처리하기 때문에 최적화에 도움을 많이 준다.</li>
</ul>
<h4 id="✦-후면컬링">✦ 후면컬링</h4>
<blockquote>
<p>말그대로 보이지 않아도 되는 객체 후면을 렌더링과정에서 제외 시키는 것이다.</p>
</blockquote>
<ul>
<li>우리의 카메라는 상대와 마주보고 있을 시에는 상대의 뒷 부분은 보이지 않는다.</li>
<li>이는 렌더링할 이유가 없다는 것과 같다. 클리핑과 마찬가지로 최적화에 도움을 많이준다.</li>
<li>참고로 클리핑 이후 후면컬링이 이루어진다.</li>
<li>즉 화면에 보이는 객체를 정하고 그 객체의 후면(보이않는 부분)을 렌더링과정에서 제외시킨다.</li>
</ul>
<h4 id="✦-뷰포트-변환">✦ 뷰포트 변환</h4>
<blockquote>
<p>우리가 모니터에서 3D&quot;처럼&quot; 볼 수 있게되는 과정이다.</p>
</blockquote>
<p>그럼 이과정은 어떻게 실행될까?</p>
<ul>
<li>먼저 우리의 모니터는 2D이다. 즉 2차원이다. 그래서 우리가 만든 3D를 2D로 변환시켜 주어야 한다.</li>
<li>투영 변환 후 클립 공간 좌표를 얻는다. 투영 행렬을 적용한 후 좌표는 클립 공간(Clip Space)에 존재한다. 이 과정에서 오브젝트가 카메라 시야에 있는지 판별된다.</li>
</ul>
<p>이어서</p>
<h4 id="✦-정규화-장치-좌표-ndc">✦ 정규화 장치 좌표 (NDC)</h4>
<ul>
<li><p>후 NDC로 변환을 해야한다. 클립 공간의 좌표를 동차(W) 좌표로 나누어 -1 ~ 1 범위로 정규화한다. ( X/W , Y/W , Z/W ) 투영변환의 마지막 과정이다.</p>
</li>
<li><p>이제 3D좌표들은 카메라 기준으로 정규환된 범위내에 배치되게된다.</p>
</li>
<li><p>하지만 모니터는 -1<del>1의 좌표계가 아닌 0</del>넓이 , 0~ 길이 와 같은 좌표계를 사용한다.</p>
</li>
<li><p>이제 계산식을 이용하여 NDC 좌표를 화면 픽셀 좌표로 변환한다. 식은 간단하다 X와 Y 각각의 NDC값에 1을 더하고 그걸 2로 나눈다. </p>
</li>
<li><p>그리고 X라면 넓이 Y라면 길이를 곱한다. 그러면 이제 3D좌표가 픽셀좌표로 변환된다.</p>
</li>
<li><p>참고로 NDC에서는 (0,0)은 중심이지만 화면 좌표에서는 좌측하단이다.</p>
</li>
</ul>
<h4 id="✦-스캔-변환">✦ 스캔 변환</h4>
<blockquote>
<p>기본도형을 실제 화면에서 표시될 픽셀로 변환하는 과정이다.</p>
</blockquote>
<h4 id="✦-기본도형이란">✦ 기본도형이란?</h4>
<ul>
<li>엔진마다 기본도형이 다를 수는 있지만 대부분 삼각형을 기본도형으로 사용한다.</li>
</ul>
<h5 id="그럼-왜-삼각형을-많이-사용할까">그럼 왜 삼각형을 많이 사용할까?</h5>
<ul>
<li><p>GPU가 삼각형 렌더링에 최적화되어있고 DirectX, OpenGL, Vulkan같은 그래픽 API 들도 기본도형을 삼각형으로 사용한다.</p>
</li>
<li><p>또한 삼각형은 항상 하나의 평면을 형성할 수 있기 때문이다.(또한 가장 단순한 폴리곤) 점이 3개이기 때문에 어떠한 형태로도 평면을 이룬다.</p>
</li>
<li><p>이 때문에 사각형처럼 비대칭 변형이 발생할 문제가 없고 어떤 형태라도 정확히 표현 가능하다.</p>
</li>
<li><p>그리고 3D모델은 렌더링 과정에서 반드시 삼각형으로 변형되기 때문에 복잡한 3D모델도 
삼각형으로 변환할 수 있다.</p>
</li>
<li><p>참고로 기본도형에는 점,선,삼각형,사각형이 존재하는데 삼각형을 제외한 이들도 이점을 가지는 분야가 있다.
(점 : 파티클, 선 : 와이어프레임 렌더링 등) 그래서 Unity에서는 모든 기본도형을 지원한다.</p>
</li>
</ul>
<h4 id="✦-프래그먼트-내부-픽셀-판별">✦ 프래그먼트 내부 픽셀 판별</h4>
<blockquote>
<ul>
<li>기본도형의 경계 박스를 계산한다. </li>
</ul>
</blockquote>
<ul>
<li><p>그 내부에 포함되는 픽셀을 판별한다.</p>
</li>
<li><p>포함된 픽셀을 기반으로 프래그먼트를 생성한다.</p>
</li>
<li><p>프래그먼트 픽셀 후보이며 깊이 테스트를 거쳐 최종 픽셀로 결정된다.</p>
</li>
<li><p>그 후 각픽셀이 기본도형의 내부에 있는지 검사한다. 바리센트릭 좌표 또는 엣지 함수를 쓴다.</p>
</li>
<li><p>즉 &quot;어떤 픽셀이 기본도형의 일부인가?&quot; 를 판별한다.
이 과정에서 기본도형의 내부에 있는 픽셀만을 남기고 나머지는 삭제한다.</p>
</li>
</ul>
<h4 id="✦-정점-데이터-보간">✦ 정점 데이터 보간</h4>
<blockquote>
<p>각 프래그먼트 마다의 정점 데이터를 보간하여 최종값을 결정한다.</p>
</blockquote>
<p>여기서 보간될 데이터로는</p>
<ul>
<li><p>UV맵핑</p>
</li>
<li><p>색상</p>
</li>
<li><p>Z값 (깊이버퍼)</p>
</li>
<li><p>노멀벡터 (조명계산용) 
가 있다.</p>
</li>
<li><p>이제 픽셀마다 필요한 데이터를 보간하여 화면을 생성할 수 있다.
(정점데이터를 프래그먼트로 저장하는 과정이다보니 프래그먼트 데이터 보간이라고도 한다.)</p>
</li>
</ul>
<h2 id="✤-4픽셀셰이더pixel-shader">✤ 4.픽셀셰이더(Pixel Shader)</h2>
<blockquote>
<p>픽셀 셰이더는 레스터라이저에서 생성된 픽셀에 대한 최종 색상을 계산하는 단계이다.</p>
</blockquote>
<ul>
<li><p>각 픽셀에 대해 조명, 텍스처, 혼합 등의 작업을 수행하여 최종 색상을 결정한다.</p>
</li>
<li><p>깊이 테스트 되기 전 픽셀들은 프래그먼트 상태이기 때문에 프래그먼트 셰이더 (Fragment Shader) 라고도한다.</p>
</li>
</ul>
<h2 id="✤-5출력-병합output-merger">✤ 5.출력 병합(Output-Merger)</h2>
<blockquote>
<p>출력 병합 단계에서는 완성된 프레임이 프레임 버퍼에 저장되고 화면에 출력되는 과정이다.</p>
</blockquote>
<ul>
<li><p>이 단계에서 각 픽셀의 색상 정보가 프레임 버퍼에 저장되고, 깊이 테스트 가장 앞의 픽셀만 남긴다.
(즉 화면 픽셀에 가장 앞의 물체가 뒤의 물체를 가린다는 것이다.)</p>
</li>
<li><p>블랜딩 반투명 오브젝트의 처리, 안티 앨리어싱 계단현상 제거 와 같은 
후처리 작업이 수행된다.</p>
</li>
<li><p>최종적으로 이 데이터는 화면에 출력될 수 있도록 준비된다.</p>
</li>
</ul>
<h2 id="✤-정리">✤ 정리</h2>
<p>간단히 정리해보면 </p>
<ul>
<li><p>정점셰이더에서 위치를 변환하고</p>
</li>
<li><p>mesh데이터가 넘어가고</p>
</li>
<li><p>데이터를 가지고 래스터라이저에서 출력될 픽셀을 계산한다.</p>
</li>
<li><p><strong>이 과정에서 (클리핑, 후면컬링, 뷰포트 변환, 스캔변환, 프래그먼트 내부 픽셀 판별)
등이 이루어진다.</strong></p>
</li>
<li><p>픽셀 셰이더에서 최종 픽셀 색상을 결정한다.</p>
</li>
<li><p><strong>(이 과정에서 텍스처 매핑(Texture Mapping), 조명 계산(Lighting), 블렌딩(Blending)을 수행한다.) 그리고 프레임버퍼를 출력한다.</strong></p>
</li>
<li><p>(해상도가 커지면 픽셀 수가 많아져 연산할 것이 많아진다.)</p>
</li>
<li><p>이과정을 프레임마다 실행한다.</p>
</li>
</ul>
<h1 id="❖-렌더링-기법">❖ 렌더링 기법</h1>
<blockquote>
<p>렌더링 기법에는 최적화와 더 높은 그래픽을 위한 기법들이 여러 개 존재한다.\</p>
</blockquote>
<h2 id="✤-깊이-버퍼depth-buffer">✤ 깊이 버퍼(Depth Buffer)</h2>
<blockquote>
<p>렌더링을 할 때, 깊이 버퍼는 물체의 깊이 정보를 저장하여 올바른 그리기 순서를 결정하는 데 사용한다.</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/hsd0604/post/cfef5afb-8be2-400e-9c80-8f9bd3b42255/image.png" /></p>
<ul>
<li><p>일반적으로 앞에 있는 물체를 먼저 그리며, 깊이 버퍼를 통해 각 픽셀의 깊이 값을 비교한다.</p>
</li>
<li><p>만약 물체가 겹친 경우 깊이 값을 비교하여 뒤에 있는 물체의 그리기를 건너뛴다.</p>
</li>
<li><p>그리고 픽셀하나에 하나의 값만이 저장되기 때문에 불투명 오브젝트만 저장된다.</p>
</li>
</ul>
<p>*<em>(참고로 Unity2D에서 제공하는 Order in Layer는 렌더링 순서를 정하는 것 일 뿐 깊이 버퍼는 아니다.)
*</em></p>
<h2 id="✤-더블버퍼링double-buffering-or-triple">✤ 더블버퍼링(Double Buffering or Triple)</h2>
<blockquote>
<p>Frame Buffer는 더블 버퍼링 개념이 적용된다.
<img alt="" src="https://velog.velcdn.com/images/hsd0604/post/e98eb6a6-9673-4513-9345-154c1c540e76/image.jpg" /></p>
</blockquote>
<h3 id="✦-더블버퍼링이란">✦ 더블버퍼링이란?</h3>
<ul>
<li>우리가 화면을 볼때에는 그리는 과정이 아닌 결과만을 보게된다.</li>
</ul>
<h3 id="✦-왜-결과만을-볼-수-있을까">✦ 왜 결과만을 볼 수 있을까?</h3>
<ul>
<li><p>렌더링 파이프라인을 거쳐 BackBuffer란 곳에 저장을 한다.</p>
</li>
<li><p>이 BackBuffer는 화면에 보이지않고 메모리로만 가지고있는 버퍼이다.</p>
</li>
<li><p>이제 그림을 다 그리면 FrontBuffer로 바꾼다. 그리고 이 FrontBuffer가 Display에 출력된다.</p>
</li>
</ul>
<h4 id="이처럼-backbuffer와-frontbuffer가-서로-프레임마다-바뀌며-하나는-그림을-그리고-하나는-그림을-보여주는-것이다">이처럼 BackBuffer와 FrontBuffer가 서로 프레임마다 바뀌며 하나는 그림을 그리고 하나는 그림을 보여주는 것이다.</h4>
<p><strong>(요즘은 모니터의 Hz를 고려하여 2개가아닌 3개를 쓰는 Triple Buffering을 쓰기도한다.)</strong></p>
<h2 id="✱-deferred지연">✱ Deferred(지연)</h2>
<ul>
<li><p>Forward는 오브젝트마다 드로우콜과 라이팅 작업을 거친다. 하지만 많은 라이트를 사용하기에는 Forward는 적합하지않아 등장한 것이 Deferred이다.</p>
</li>
<li><p>Deferred는 그때그때 라이팅연산을 하지않고 지버퍼(Geometry Buffer)에 데이터를 저장해두고 라이팅을 한번에처리한다. 이처럼 라이팅을 지연시켜서한다고해서 Deferred 렌더링이다.</p>
</li>
</ul>
<h1 id="✤-unity의-렌더링-루프">✤ Unity의 렌더링 루프</h1>
<p>● 렌더링 루프</p>
<ul>
<li>Update에서 Physics,Logic 연산등... 후</li>
<li>Culling 등 렌더링 작업 후 </li>
<li>SceneRender에서 DrawCall-&gt; 정점셰이더-&gt; 픽셀셰이더 후 </li>
<li>Post Processing 후더블 버퍼링 후  </li>
<li>Update에서 Physics,Logic 연산등... 후</li>
</ul>
<p>이걸 매프레임 마다 반복한다.</p>
<h1 id="✤-urpunityrenderingpipeline-light-최적화">✤ URP(UnityRenderingPipeline) Light 최적화</h1>
<ul>
<li><p>원래 광원의 갯수 X 오브젝트의 수 만큼의 드로우 콜이 발생한다. </p>
</li>
<li><p>URP에서는 라이트를 처리할 때마다 Frame Buffer을 읽지않고 셰이더 안에서 라이트의 갯수만큼 for문으로 연산하여 이루어지기 때문에 한번의 드로우콜로 처리를 할 수 있고 오버드로우 이슈도 깔끔하다.</p>
</li>
</ul>