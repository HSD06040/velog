<p>Unity의 EnventSystem에서 UI의 입력과 같은 이벤트를 3D 오브젝트에 전달하기 위한 컴포넌트이다.</p>
<p>카메라에 붙여서 UI 시스템이 Collider를 가진 3D 오브젝트와 상호작용 할 수 있도록 한다.</p>
<ul>
<li>즉 IDragHandler, IPointerXX 같은 인터페이스를 3D 오브젝트에 사용이 가능해진다.</li>
</ul>
<h1 id="사용-조건">사용 조건</h1>
<ul>
<li><p>UI EventSystem이 있어야한다.</p>
</li>
<li><p>3D 오브젝트에 Collider가 있어야한다.</p>
</li>
<li><p>카메라에 PhysicsRaycaster이 있어야한다.</p>
</li>
</ul>
<pre><code class="language-cs">using UnityEngine;
using UnityEngine.EventSystems;

public class CubeObject : MonoBehaviour, IBeginDragHandler, IDragHandler
{
    private Vector3 offset;
    private Plane dragPlane;
    private Camera cam;

    void Start()
    {
        cam = Camera.main;
    }

    public void OnBeginDrag(PointerEventData eventData)
    {
        dragPlane = new Plane(cam.transform.forward * -1f, transform.position);

        Ray ray = cam.ScreenPointToRay(eventData.position);
        if (dragPlane.Raycast(ray, out float enter))
        {
            // 마우스를 기준으로 중심이 어디 있어야하는지에 대한 값
            offset = transform.position - ray.GetPoint(enter); 
        }
    }

    public void OnDrag(PointerEventData eventData)
    {
        Ray ray = cam.ScreenPointToRay(eventData.position);
        if (dragPlane.Raycast(ray, out float enter))
        {
            // 드래그 마다의 마우스 위치
            Vector3 point = ray.GetPoint(enter); 

            // 마우스 위치 + 마우스를 기준으로 중심이 어디 있어야하는지에 대한 값
            transform.position = point + offset;
        }
    }
}</code></pre>
<p>Plane은 평면(기준)을 생성하는 것이다.</p>
<p>우리가 마우스로 오브젝트를 움직이려면 마우스의 좌표를 World좌표로 변환할 필요가 있다.</p>
<p>Plane.Raycast로 클릭한 좌표에서 레이를 쏴서 Plane과 어디서 만나는지를 계산한다.</p>
<p>그래서 그 값으로 자연스럽게 드래그가 가능하도록 하는 것이다.</p>
<p>offset은 내가 오른쪽 구석부터 드래그를 시작하면 계속 마우스가 오브젝트의 오른쪽 끝에 있는게
자연스럽기 떄문에 보간해주기 위한 값이다.</p>