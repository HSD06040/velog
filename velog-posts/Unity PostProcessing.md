<h1 id="channel-mixer">Channel Mixer</h1>
<blockquote>
<p>이미지의 RGB 채널을 섞어 컬러를 조정할 수 있다.</p>
</blockquote>
<table>
<thead>
<tr>
<th>값</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>Rad Out Rad In</td>
<td>빨간색 채널에서 빨간색 기여도</td>
</tr>
<tr>
<td>Rad Out Blue In</td>
<td>빨간색 체널에서 파란색 기여도</td>
</tr>
<tr>
<td>Rad Out Green In</td>
<td>빨간색 채널에서 초록색 기여도</td>
</tr>
</tbody></table>
<p>Green과 Blue도 똑같이 있다.</p>
<h1 id="chromatic-aberration">Chromatic Aberration</h1>
<blockquote>
<p>화면 외곽에 색 분리(빨강/파랑 번짐) 효과를 준다.</p>
</blockquote>
<p>값 | 설명
|-|-|
Intensity | 색수차 강도
Spectral LUT | 색 분리에 사용할 LUT 텍스처</p>
<h1 id="color-adjustments">Color Adjustments</h1>
<blockquote>
<p>전체 색감, 채도, 노출 등을 조정할 수 있다.</p>
</blockquote>
<p>값 | 설명
|-|-|
Post Exposure | 전체 화면 밝기 조정
Contrast | 대비 조정
Color Filter | 전체 색상 필터
Hue Shift | 색상 회전
Saturation | 채도 조정</p>
<h1 id="color-curves">Color Curves</h1>
<blockquote>
<p>컬러 커브를 이용해 색상과 밝기를 정밀하게 조정한다.</p>
</blockquote>
<p>값 | 설명
|-|-|
Master Curve | 전체 밝기 커브
Red Curve | 빨간색 커브 조정
Green Curve | 초록색 커브 조정
Blue Curve | 파란색 커브 조정</p>
<h1 id="color-lookup-lut">Color Lookup (LUT)</h1>
<blockquote>
<p>LUT를 적용해 색감 스타일을 빠르게 변환한다.</p>
</blockquote>
<p>값 | 설명
|-|-|
Texture | LUT 텍스처 지정
Contribution | LUT 적용 강도 (0~1)</p>
<h1 id="depth-of-field">Depth Of Field</h1>
<blockquote>
<p>특정 거리만 선명하게 하고 나머지는 흐릿하게 만든다.</p>
</blockquote>
<p>값 | 설명
|-|-|
Mode | Gaussian 또는 Bokeh 선택
Focus Distance | 초점 맞출 거리
Aperture | 조리개 값, 심도 조절
Focal Length | 렌즈 초점 거리 (mm 단위)
Blade Count | 조리개 날 수 (Bokeh 모드)
Blade Curvature | 조리개 곡률 (Bokeh 모드)</p>
<h1 id="film-grain">Film Grain</h1>
<blockquote>
<p>필름처럼 입자 노이즈를 추가해 아날로그 느낌을 만든다.</p>
</blockquote>
<p>값 | 설명
|-|-|
Type | 그레인 종류 (Thin1, Thin2 등)
Intensity | 노이즈 강도
Response | 밝은 영역에서 노이즈 반응도</p>
<h1 id="lens-distortion">Lens Distortion</h1>
<blockquote>
<p>광각 렌즈 같은 왜곡 효과를 준다.</p>
</blockquote>
<p>값 | 설명
|-|-|
Intensity | 왜곡 강도
X Multiplier | 가로 방향 왜곡 정도
Y Multiplier | 세로 방향 왜곡 정도
Center | 왜곡 중심 위치
Scale | 왜곡된 화면 크기 조정</p>
<h1 id="lift-gamma-gain">Lift, Gamma, Gain</h1>
<blockquote>
<p>그림자, 중간톤, 하이라이트 각각의 밝기와 색을 조정한다.</p>
</blockquote>
<p>값 | 설명
|-|-|
Lift | 그림자 영역 밝기 및 색상 조정
Gamma | 중간 영역 밝기 및 색상 조정
Gain | 하이라이트 영역 밝기 및 색상 조정</p>
<h1 id="motion-blur">Motion Blur</h1>
<blockquote>
<p>빠른 움직임에 흐릿한 잔상을 추가한다.</p>
</blockquote>
<p>값 | 설명
|-|-|
Mode | 카메라 기반 또는 오브젝트 기반 블러
Intensity | 모션 블러 강도
Clamp | 모션 블러 강도 제한</p>
<h1 id="panini-projection">Panini Projection</h1>
<blockquote>
<p>광각 시 왜곡을 보정해주는 특수 효과.</p>
</blockquote>
<p>값 | 설명
|-|-|
Distance | 왜곡 보정 거리 (0~1)
Crop To Fit | 화면 잘리는 부분 리사이즈
Crop | 크롭 정도 조정</p>
<h1 id="screen-space-lens-flare">Screen Space Lens Flare</h1>
<blockquote>
<p>화면 안의 밝은 빛에 렌즈 플레어를 추가한다.</p>
</blockquote>
<p>값 | 설명
|-|-|
Intensity | 렌즈 플레어 강도
Threshold | 플레어 발생 기준 밝기
Ghosts | 고스트 플레어 개수
Halo Intensity | 후광 강도</p>
<h1 id="shadows-midtones-highlights">Shadows, Midtones, Highlights</h1>
<blockquote>
<p>각 밝기 영역별 색 보정이 가능하다.</p>
</blockquote>
<p>값 | 설명
|-|-|
Shadows | 그림자 색상 보정
Midtones | 중간톤 색상 보정
Highlights | 하이라이트 색상 보정
Shadow Limit | 그림자 적용 범위
Highlight Limit | 하이라이트 적용 범위</p>
<h1 id="split-toning">Split Toning</h1>
<blockquote>
<p>어두운 부분과 밝은 부분에 다른 색조를 부여한다.</p>
</blockquote>
<p>값 | 설명
|-|-|
Shadows Tint | 그림자 영역 색상
Highlights Tint | 하이라이트 영역 색상
Balance | 그림자/하이라이트 비율 조정</p>
<h1 id="tonemapping">Tonemapping</h1>
<blockquote>
<p>HDR 이미지를 자연스러운 밝기로 압축한다.</p>
</blockquote>
<p>값 | 설명
|-|-|
Mode | None, Neutral, ACES, Custom 중 선택</p>
<h1 id="vignette">Vignette</h1>
<blockquote>
<p>화면 가장자리를 어둡게 만들어 집중도를 높인다.</p>
</blockquote>
<p>값 | 설명
|-|-|
Intensity | 비네트 강도
Smoothness | 테두리 부드러움 정도
Roundness | 비네트 모양 조정
Rounded | 완전 원형으로 만들지 여부
Color | 비네트 색상
Center | 비네트 중심 위치</p>
<h1 id="white-balance">White Balance</h1>
<blockquote>
<p>화면 전체의 색 온도와 틴트를 조정한다.</p>
</blockquote>
<p>값 | 설명
|-|-|
Temperature | 따뜻함/차가움 조정
Tint | 초록/마젠타 색조 조정</p>