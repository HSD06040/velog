<h1 id="ui">UI</h1>
<p>원래의 Unity는 UI도 코딩으로 제작하였다.</p>
<p>하지만 이는 디자이너가 다루기에는 어려움이 있었고 Unity는 UGUI를 개발하였다.</p>
<p>그래서 오늘 날의 Unity UI는 UGUI로 많이 만들어지고 있다.</p>
<h2 id="ui-편집">UI 편집</h2>
<ul>
<li><p>UI는 씬창에서 편집하기 위해서 씬에 존재하기는 하지만</p>
</li>
<li><p>3D 게임화면에 영향을 주지는 못한다.</p>
</li>
<li><p>즉 Canvas에서의 작업과 게임 화면은 별도의 다른 화면이다.</p>
</li>
<li><p>하지만 UI자체는 씬창에 보이기 때문에 씬을 편집하기 힙들 수도 있다.</p>
</li>
<li><p>이럴떄에는 Layers에서 UI를 보지 않도록 하고 작업한다.</p>
</li>
</ul>
<h2 id="canvas-rendermodes">Canvas RenderModes</h2>
<p>Overlay</p>
<ul>
<li>별도의 공간으로 분리하여 제작하는 방식이다.</li>
</ul>
<p>Camera</p>
<ul>
<li>카메라 앞에 그리는 방식이다. 보통 2D에서 많이 쓰인다.</li>
</ul>
<p>WorldSpace</p>
<ul>
<li>World에 그리는 방식으로 실제 게임창에 보이는 UI이다.</li>
</ul>
<h2 id="canvas-scaler">Canvas Scaler</h2>
<p>Scale With Screen Size</p>
<ul>
<li>Canvas에 맞는 크기로 UI를 자동구성 해준다.</li>
<li>Match에서 넓이 높이를 어느정도의 기준으로 자동구성 해줄지를 정할 수 있다.
(보통 PC는 넓이가 커지고 모바일은 높이까지 고려해야 한다.)</li>
</ul>
<p>Constant Pixel Size</p>
<ul>
<li>UI의 절대 크기를 지정한다.</li>
</ul>
<h2 id="text와-tmp">Text와 TMP</h2>
<p>Text</p>
<ul>
<li>픽셀로 표현되어 있는 글씨이다. 옛날 유니티에서 사용되었다.</li>
<li>픽셀로 구현되어 있어서 글자의 해상도가 낮다.</li>
</ul>
<p>TextMeshPro</p>
<ul>
<li>최근 유니티에서 많이 쓰는 방식으로 Text를 Mesh로 구현한 방식이다.</li>
<li>해상도가 높다.</li>
</ul>
<p>Warpping</p>
<p>TextMeshPro에서 일정 영역만 색깔 바꾸기</p>
<pre><code>&lt;color=red&gt;Button&lt;/color&gt;</code></pre><h2 id="rect-transform">Rect Transform</h2>
<ul>
<li>픽셀 기준으로 되어있는 Transform이다.</li>
</ul>
<h3 id="앵커">앵커</h3>
<p>Canvas 기준</p>
<ul>
<li>화면 해상도가 늘어나거나 줄더라도 기준을 정해서 위치를 설정하는 것이다. (크기, 위치)
(왼쪽 위 오른쪽 아래 등, 크기에 따른 비율 설정 등)</li>
</ul>
<p>앵커 프리셋</p>
<ul>
<li>앵커를 많이쓰는 것들을 프리셋화 해두었다.</li>
<li>크기까지 비율을 설정해야 할 경우에도 프리셋이 존재한다.</li>
</ul>
<h3 id="pivot">Pivot</h3>
<p>현재 UI 기준</p>
<ul>
<li>현재 UI를 기준으로 위치를 변경한다.</li>
</ul>
<h2 id="image">Image</h2>
<ul>
<li>Canvnas Renderer와 Image 컴포넌트가 있는 객체이다.</li>
</ul>
<p>SourceImage</p>
<ul>
<li>표시될 이미지 소스를 선택할 수 있다.</li>
</ul>
<p>Color</p>
<ul>
<li>원래 이미지의 색을 변경하는 것이 아닌 섞을 색을 정하는 것이다.</li>
</ul>
<p>Metarial</p>
<ul>
<li>이미지의 재질을 결정한다.</li>
</ul>
<p>Raycast Target</p>
<ul>
<li>이미지가 클릭할 수 있는지 아닌지를 정한다.</li>
</ul>
<h2 id="rawimage">RawImage</h2>
<ul>
<li><p>RenderTexture같은 동영상 재생기 같은 역할을 하는 컴포넌트이다.</p>
</li>
<li><p>미니맵, 다른 사람의 화면들을 게임내에 출력할때에 활용한다.</p>
</li>
</ul>
<h2 id="togle">Togle</h2>
<p>Bool 타입 같이 체크 언체크 형식으로 사용가능한 UI이다.</p>
<h2 id="slider">Slider</h2>
<p>게이지 형식으로 생긴 UI이다.</p>
<p>체력바 같은 것에 활용한다.</p>
<h2 id="slider-bar">Slider Bar</h2>
<p>상호작용을 통해 조절이 가능한 바이다.</p>
<p>SFX Volum 조절과 같은곳에서 활용한다.</p>
<h2 id="button">Button</h2>
<p>누를 수 있는 버튼 UI이다.</p>
<h3 id="transition">Transition</h3>
<p>SpriteSwap</p>
<ul>
<li>상호작용 시 이미지 변경이 되는 방식으로 사용이 가능하다.</li>
</ul>
<p>Animation</p>
<ul>
<li>상호작용 시 애니메이션이 실행되도록 사용이 가능하다.</li>
</ul>
<h2 id="dropdown">DropDown</h2>
<p>옵션이 여러개 있고 설정할 수 있는 UI이다.</p>
<h2 id="inputfield">InputField</h2>
<p>Text를 쓸 수 있는 칸이다.</p>
<p>타입이 있고 안보이게 한다거나 하는 등으로 사용가능하다.</p>