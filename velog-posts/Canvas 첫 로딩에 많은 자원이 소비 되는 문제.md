<p>UI Canvas가 첫 로딩 때만 프레임이 낮춰지는 문제가 있었다.</p>
<h2 id="해결-방법">해결 방법</h2>
<p>Canvas설정에 Pixel Perfect를 체크 해주어 해결 되었다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/hsd0604/post/bae990a7-8979-4ac2-a7b7-2590df5da297/image.png" /></p>
<p>이 옵션은 UI를 픽셀 단위로 깔끔하게 맞추도록 해주는 기능이다.</p>
<p>보통 2D UI나 Sprite 기반 게임에서 글자/선이 흐려지지 않게 하기 위해 사용되며</p>
<p>내부적으로는 Canvas가 Snap to Pixel Grid 식으로 동작하게 된다.</p>
<h2 id="해결된-예상-이유">해결된 예상 이유</h2>
<h3 id="1-rebuild-계산-단순화">1. Rebuild 계산 단순화</h3>
<ul>
<li><p>픽셀 퍼펙트를 켜면 UI 요소들의 배치와 렌더링을 픽셀 그리드에 맞춰 고정한다.</p>
</li>
<li><p>이로 인해 서브 픽셀 보간, 정렬, 모션 블러 관련 계산이 줄어든다.</p>
</li>
</ul>
<h3 id="2-canvas의-텍스처버퍼-세팅이-간단해짐">2. Canvas의 텍스처/버퍼 세팅이 간단해짐</h3>
<ul>
<li>일부 플랫폼이나 GPU에서는 픽셀 정렬 방식이 더 간단한 오프스크린 렌더링 패스를 유도한다.</li>
<li>초기 Canvas 생성 시 더 빠르게 최적화된 버퍼 구조가 적용될 수 있다.</li>
</ul>
<h3 id="3-레이아웃-컴포넌트-재계산량이-감소">3. 레이아웃 컴포넌트 재계산량이 감소</h3>
<ul>
<li>RectTransform들의 정렬이 픽셀 격자에 맞게 단순화됨 
→ Layout Group, Content Size Fitter 비용 약간 줄어듦.</li>
</ul>