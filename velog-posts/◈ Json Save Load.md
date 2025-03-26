<h1 id="❖-구조-설계">❖ 구조 설계</h1>
<p><img alt="" src="https://velog.velcdn.com/images/hsd0604/post/81fa731b-78b4-4d76-b7c9-d7de067a304b/image.png" /></p>
<ol>
<li>먼저 저장할 데이터가 있는 객체에 세이브 인터페이스를 구현한다.</li>
<li>세이브 로드 시 모든객체의 세이브 인터페이스를 가져와 순회하며 저장 / 로드 한다.</li>
<li>GameData C# 클래스를 만들어서 게임데이터를 저장하도록 한다.</li>
<li>저장 시 하나의 GameData에 모든 데이터가 저장되길 원하여 ref 키워드를 사용해 GameData 원본을 수정하도록 한다.</li>
<li>파일 생성 클래스를 따로 만들어 Json파일을 정해진 경로에 저장한다.</li>
<li>로드 시 이 경로의 파일을 GameData에 Load하고 세이브 인터페이스를 전부 순회하며 로드시킨다.</li>
<li>세이브는 OnApplicationQuit 함수가 호출 될때 즉 종료될때 실행되고</li>
<li>로드는 게임이 시작되었을 때 로드 되도록한다.</li>
</ol>
<h1 id="❖-isavemanager">❖ ISaveManager</h1>
<pre><code class="language-cs">using UnityEngine;

public interface ISaveManager
{
    public void SaveGame(ref GameData _data);

    public void LoadGame(GameData _data);
}</code></pre>
<ul>
<li>SaveGame LoadGame을 구현해야 하도록하고 GameData를 매개변수가 가지고 있다.</li>
<li>SaveGame일 때에는 원본을 수정해야 하는 경우 이므로 ref키워드를 붙였다.</li>
</ul>
<h1 id="❖-savemanager">❖ SaveManager</h1>
<pre><code class="language-cs">using System.Collections.Generic;
using System.Linq;
using UnityEngine;

public class SaveManager : MonoBehaviour
{
    [SerializeField] private string fileName;
    [SerializeField] private bool encryptData;

    private GameData gameData;
    private FileDataHandler fileHandler;


    private void Awake()
    {
        DontDestroyOnLoad(gameObject);

        if (instance != null)
        {
            Destroy(instance.gameObject);
        }
        else
                instance = this;
       }

    private void Start()
    {
        fileHandler = new FileDataHandler(Application.persistentDataPath, fileName, encryptData);

        gameData = new GameData();

        Load();
    }

    private void Save()
    {
        foreach (var saveManager in GetISaveManagers())
        {
            saveManager.SaveGame(ref gameData);
        }

        fileHandler.Save(gameData);
    }

    private void Load()
    {
        gameData = fileHandler.Load();

        if(this.gameData == null)
            gameData = new GameData();

        foreach (var saveManager in GetISaveManagers())
        {
            saveManager.LoadGame(gameData);
        }
    }

    private void OnApplicationQuit()
    {
        Save();
    }

    [ContextMenu(&quot;Delete Save Data&quot;)]
    private void DeleteSaveData()
    {
        fileHandler.DeleteFile();
    }

    private List&lt;ISaveManager&gt; GetISaveManagers()
    {
        IEnumerable&lt;ISaveManager&gt; saveManagers = FindObjectsByType&lt;MonoBehaviour&gt;(FindObjectsSortMode.None).OfType&lt;ISaveManager&gt;();

        return new List&lt;ISaveManager&gt;(saveManagers);
    }
}
</code></pre>
<p>하이어라키에 존재하는 세이브를 하는 객체이다.</p>
<ul>
<li><p>FileDataHandler를 생성하며 가지고있다. 즉 세이브/로드를 하고 파일을 생성/로드 같은 작업들이 이 객체에서 이루어진다.</p>
</li>
<li><p>FileDataHandler생성자로 인자값으로 기본경로, 파일이름, 암호화 여부를 넘긴다.</p>
</li>
</ul>
<p>*<em>✧ Save *</em></p>
<ul>
<li>모든 ISaveManager를 순회하며 SaveManager에 있는 gameData에 저장함.</li>
</ul>
<p>*<em>✧ Load *</em></p>
<ul>
<li>파일정보를 gameData로 가져오고 모든 ISaveManager를 순회하며 데이터를 로드함.</li>
</ul>
<p><strong>✧ GetISaveManagers</strong></p>
<ul>
<li>지연 실행으로 ISaveManager을 다 가져와서 값을 가지고 있다가 List로 반환함.</li>
</ul>
<h1 id="❖-filedatahandler">❖ FileDataHandler</h1>
<pre><code class="language-cs">using System;
using System.IO;
using TMPro;
using UnityEngine;

public class FileDataHandler
{
    private string dataPath;
    private string dataFileName;

    private bool encryptData = false;
    private string codeWord = &quot;hsd&quot;;

    public FileDataHandler(string _dataPath, string _dataFileName, bool _encryptData)
    {
        dataFileName = _dataFileName;
        dataPath = _dataPath;
        encryptData = _encryptData;
    }

    public void Save(GameData _data)
    {
        string fullPath = Path.Combine(dataPath, dataFileName);

        try
        {
            Directory.CreateDirectory(Path.GetDirectoryName(fullPath));

            string dataToStore = JsonUtility.ToJson(_data, true);

            using (FileStream stream = new FileStream(fullPath, FileMode.Create))
            {
                using (StreamWriter writer = new StreamWriter(stream))
                {
                    writer.Write(dataToStore);
                }
            }

            if (encryptData)
                dataToStore = EncryptDecrypt(dataToStore);
        }
        catch (Exception e)
        {
            Debug.LogError($&quot;Error : {fullPath}에 세이브 되지 않았습니다. {e}&quot;);
        }
    }

    public GameData Load()
    {
        string fullPath = Path.Combine(dataPath, dataFileName);
        GameData loadData = null;

        if (File.Exists(fullPath))
        {
            try
            {
                string dataToLoad = &quot;&quot;;

                using (FileStream stream = new FileStream(fullPath, FileMode.Open))
                {
                    using (StreamReader reader = new StreamReader(stream))
                    {
                        dataToLoad = reader.ReadToEnd();
                    }
                }
                if(encryptData)
                    dataToLoad = EncryptDecrypt(dataToLoad);

                loadData = JsonUtility.FromJson&lt;GameData&gt;(dataToLoad);
            }

            catch (Exception e)
            {
                Debug.LogError($&quot;Error : {fullPath}에 로드 되지 않았습니다. {e}&quot;);
            }
        }
        return loadData;
    }

    public void DeleteFile()
    {
        string fullPath = Path.Combine(dataPath, dataFileName);

        if (File.Exists(dataPath))
            File.Delete(dataPath);
    }

    private string EncryptDecrypt(string _data)
    {
        string modifiedData = &quot;&quot;;

        for (int i = 0; i &lt; _data.Length; i++)
        {
            modifiedData += (char)(_data[i] &amp; codeWord[i % codeWord.Length]);
        }

        return modifiedData;
    }
}</code></pre>
<p>fullPath</p>
<ul>
<li>Path.Combine함수를 이용해서 기본경로 + 파일이름으로 경로를 설정한다.</li>
</ul>
<p>그 후 fullPath의 파일 경로만 가져온다.</p>
<p><strong>✧ dataToStore</strong></p>
<ul>
<li>_data를 Json으로 변환 후 dataToStore에 넣는다.</li>
</ul>
<p><strong>✧ FileStream</strong></p>
<ul>
<li>FileStream을 생성해서 파일을 생성할 준비를 한다.</li>
<li>FileMode.Create는 기존파일이 있다면 덮어쓰고 없다면 생성한다.</li>
</ul>
<p><strong>✧ StreamWriter</strong></p>
<ul>
<li>파일에 데이터를 쓸 수 있는 객체이다.</li>
<li>writer.Write(dataToStore)은 Json 데이터를 파일에 저장한다.(FileMode에 따라 달라짐)</li>
</ul>
<p><strong>✧ try</strong></p>
<ul>
<li>코드에서 예외가 발생할 가능성이 있는 코드를 감싸는 역할이다.</li>
<li>즉 파일을 저장하는 동안 문제가 생기더라도 게임을 강제 종료하지 않고 예외를 처리하도록 돕는다.</li>
</ul>
<p><strong>✧ catch</strong></p>
<ul>
<li>파일 저장 중 오류가 발생하면 예외를 잡아 출력한다.</li>
<li>파일 권한, 디스크 부족, 경로 오류 등.</li>
</ul>