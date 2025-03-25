<h1 id="✤-원래-시스템의-문제점">✤ 원래 시스템의 문제점</h1>
<p>UI와 게임로직이 직접적으로 연결 될 수 밖에 없는 구조였다.
그래서 이를 수정하고자 MVP패턴을 활용하여 수정하기로 하였다.</p>
<h1 id="✤-inventorymanager싱글톤">✤ InventoryManager(싱글톤)</h1>
<blockquote>
<p>하이어라키 제네릭 타입의 싱글톤을 상속받는 InventoryManager을 만들었다.</p>
</blockquote>
<h2 id="✦-싱글톤으로-만든이유">✦ 싱글톤으로 만든이유</h2>
<ol>
<li>✧ 사용빈도의 고려</li>
</ol>
<ul>
<li>Inventory는 아이템 추가, 제거 등이 이루어지는데 이는 게임 내에서 아주 다양한 방법으로 
이루어지고 그때마다 의존성을 신경쓰며 작업하면 코드가 어지러워질 것 이라고 생각하였기 때문에 싱글톤으로 관리하도록 하였다.</li>
</ul>
<ol start="2">
<li>✧ Inventory의 씬 전환시 유지방법</li>
</ol>
<ul>
<li>인벤토리가 직접 하이어라키에 올라가지않고 씬 전환시 유지되도록 하는 방법이 무엇일까 고민하였다.</li>
<li>그래서 결론이 Inventory를 싱글톤에서 생성하여 유지 되도록하는 것이다.
(그래서 Inventory는 ScriptableObject로 관리된다.)</li>
</ul>
<pre><code class="language-cs">using UnityEngine;

public class InventoryManager : Singleton&lt;InventoryManager&gt;
{
    public Inventory inventory { get; private set; }
    [SerializeField] private Transform itemSlotParent;
    private UI_ItemSlot[] inventorySlot;

    private void Awake()
    {
        inventory = ScriptableObject.CreateInstance&lt;Inventory&gt;();
        inventorySlot = itemSlotParent.GetComponentsInChildren&lt;UI_ItemSlot&gt;();
    }

    private void UpdateSlotUI()
    {
        for(int  i = 0; i &lt; inventorySlot.Length; i++)
        {
            inventorySlot[i].CleanUpSlot();
        }
        for (int i = 0; i &lt; inventory.inventoryList.Count; i++)
        {
            inventorySlot[i].UpdateSlot(inventory.inventoryList[i]);
        }
    }

    public void AddItem(ItemData _data)
    {
        if (_data.itemtype == ItemType.Equipment)
            inventory.AddToInventory(_data);

        UpdateSlotUI();
    }

    public void RemoveItem(ItemData _itemData)
    {
        if (inventory.inventoryDictionary.TryGetValue(_itemData, out InventoryItem item))
        {
            if (item.stack &gt; 1)
            {
                item.RemoveStack();
            }
            else
            {
                inventory.inventoryList.Remove(item);
                inventory.inventoryDictionary.Remove(_itemData);
            }
        }
    }
}</code></pre>
<blockquote>
<p>로직부분을 싱글톤에서 관리하고 UI를 업데이트 하기 때문에 실제 Inventory와 Slot의 의존성을 낮출 수 있었다.</p>
</blockquote>
<h1 id="✤-인벤토리-변경사항">✤ 인벤토리 변경사항</h1>
<blockquote>
<p>이제 인벤토리는 하이어라키가아닌 ScriptableObject로 관리된다.</p>
</blockquote>
<pre><code class="language-cs">public class Inventory : ScriptableObject
{
    public List&lt;InventoryItem&gt; inventoryList = new List&lt;InventoryItem&gt;();
    public Dictionary&lt;ItemData, InventoryItem&gt; inventoryDictionary = new Dictionary&lt;ItemData, InventoryItem&gt;();

    public void AddToInventory(ItemData _itemData)
    {
        if(inventoryDictionary.TryGetValue(_itemData,out InventoryItem item))
        {
            item.AddStack();
        }
        else
        {
            InventoryItem newItem = new InventoryItem(_itemData);
            inventoryList.Add(newItem);
            inventoryDictionary.Add(_itemData,newItem);
        }
    }
}</code></pre>
<ul>
<li>에셋형태로 저장하기 위해서 존재하는 ScriptableObject는 아니다.</li>
<li>런타임에서 싱글톤이 생성되었을 때 이 ScriptableObject가 함께 생성된다.</li>
</ul>
<h1 id="✤-아이템-슬롯">✤ 아이템 슬롯</h1>
<pre><code class="language-cs">public class UI_ItemSlot : MonoBehaviour
{
    [SerializeField] private Image itemIcon;
    [SerializeField] private TextMeshProUGUI itemStack;

    public InventoryItem item;

    public void UpdateSlot(InventoryItem _newItem)
    {
        item = _newItem;

        itemIcon.color = Color.white;

        if (item != null)
        {
            itemIcon.sprite = item.data.icon;

            if (item.stack &gt; 1)
            {
                itemStack.text = item.stack.ToString();
            }
            else itemStack.text = &quot;&quot;;
        }
    }

    public void CleanUpSlot()
    {
        item = null;

        itemIcon.sprite = null;
        itemIcon.color = Color.clear;
        itemStack.text = &quot;&quot;;
    }
}</code></pre>
<blockquote>
<p>실제 우리가 볼 UI를 세팅하는 부분이다.</p>
</blockquote>
<ul>
<li>실제로는 세팅할 함수를 만들고 InventoryManager에서 업데이트 해준다.</li>
</ul>
<h1 id="✤-마무리">✤ 마무리</h1>
<p>프로토타입의 모습이다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/hsd0604/post/df882a23-2b02-4344-9e10-b74031b93da5/image.png" /></p>
<ul>
<li>아이템의 아이콘 현재 갯수에 따라 표시된다.</li>
<li>인벤토리는 이제 거의 완성이다.</li>
</ul>