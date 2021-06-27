---
tags: C#
---

# C#-多線程API

* [Source Code](https://github.com/Chihhao/C_Sharp_Advance)
* [Burt Zhang](https://www.youtube.com/channel/UCnA36j_KoX-Z0QsayrLCsOg/videos) 這個頻道搜集了很多個Eleven老師的教學影片，非常精彩。

---

請參考Elevan老師的教學影片([第1集](https://www.youtube.com/watch?v=bM7s2gmnNC0&t=3935s))([第2集](https://www.youtube.com/watch?v=gzt5zG-5HPE&t=2715s))([第3集](https://www.youtube.com/watch?v=PR-r7oWlV5Y&t=5349s))([第4集](https://www.youtube.com/watch?v=WGUb59-m_eM&t=3s))([第5集](https://www.youtube.com/watch?v=B-UUii_hNbM&t=2692s))([第6集](https://www.youtube.com/watch?v=5LcsC1ufRRo))
以下節錄我認為的重點。
* **進程(process)**：一個程序運行時，佔用的全部計算資源的總和
* **線程(thread)**：程序執行的最小單位，任何操作都是由線程完成的。線程是依託於進程存在的，一個進程可以包含多個線程，線程也可以有自己的運算資源。
* **多線程(multi-thread)**：多個執行流同時運行
    * 可以簡易理解為CPU太快了，分時間片-->需要上下文切換(加載-->計算-->保存環境)
    * 微觀：一個核同一時刻只能執行一個線程
    * 宏觀：多線程並發，多CPU、多核是可以獨立運作的
    * 選購CPU時講的「4核8線程」，是指每個核有2個邏輯處理器，可以想像成有4*2=8個內核可獨立運作；此線程非彼線程。下圖是「6核12線程」的工作管理員截圖，可以特別注意到目前有3254個線程正在跑，這個線程才是這裡討論的。![](https://i.imgur.com/E2eeMik.jpg)
* **同步方法(sync)**：等待方法完成計算後，再進入下一行 (我們平常寫的程式) 
* **異步方法(async)**：不會等待方法完成，會直接進入下一行，非阻塞，不卡UI 
* 異步和多線程有什麼差別：
    * 多線程就是多個thread並發
    * 異步是硬體式的異步，但是在C#裡面寫不了
    * 統稱「**異步多線程**」，反正不用太糾結「異步」與「多線程」的差別！就是指 Thread, Pool, Task這些東西。

### 什麼是委託(delegate)？

委託是個類型(Class)，需要實例化(Instance)。
**委託的目的是讓方法變成變量(實例)**，如此便能傳遞。
(其實就是C/C++的函數指標)

* ex1. 無返回+無參數
```csharp
// 這是委託的宣告
public delegate void NoReturnNoPara(); 
// 這是準備好，要被委託的方法
static public void DoNothing_1() {
    Console.WriteLine("void DoNothing_1()");
}
```
```csharp
// 委託實例化
NoReturnNoPara method1 = new NoReturnNoPara(DoNothing_1); 
method1.Invoke(); //呼叫委託，這三個是一樣意思的
method1();        //呼叫委託，這三個是一樣意思的
DoNothing_1();    //呼叫委託，這三個是一樣意思的
```
* ex2. 無返回+有參數
```csharp
public delegate void NoReturnWithPara(int a, int b);
static public void DoNothing_2(int a, int b) {
    Console.WriteLine("void DoNothing_2(int a, int b)");
}
```
```csharp
NoReturnWithPara method2 = new NoReturnWithPara(DoNothing_2);
method2.Invoke(3, 5);
```
* ex3. 有返回+無參數
```csharp
public delegate int WithReturnNoPara();
static public int DoNothing_3() {
    Console.WriteLine("int DoNothing_3()");
    return 3;
}
```
```csharp
WithReturnNoPara method3 = new WithReturnNoPara(DoNothing_3);
int i3 = method3.Invoke();
```
* ex4. 有返回+有參數
```csharp
public delegate int WithReturnWithPara(int a, int b);
static public int DoNothing_4(int a, int b) {
    Console.WriteLine("int DoNothing_4(int a, int b)");
    return 4;
}
```
```csharp
WithReturnWithPara method4 = new WithReturnWithPara(DoNothing_4);
int i4 = method4.Invoke(1, 2);
```
* ex5. 無返回+有泛型參數
```csharp
public delegate void NoReturnWithPara<T>(T t);
static public void DoNothing_5(String s) {
    Console.WriteLine("void DoNothing_5(String s)");
}
```
```csharp
NoReturnWithPara<string> method5 = new NoReturnWithPara<string>(DoNothing_5);
method5.Invoke("123");
```

### Lambda表達式的演進
Lambda表達式的精神就是==匿名函數==

* .net 1.0 時代，就是單純的委託
    ```csharp
    public delegate void NoReturnWithPara(int a, int b);
    static public void DoNothing_2(int a, int b) {
        Console.WriteLine("void DoNothing_2(int a, int b)");
    }
    ```
    ```csharp
    NoReturnWithPara method6 = new NoReturnWithPara(DoNothing_2);
    method6.Invoke(2, 3);
    ```
* .net 2.0 時代，直接把整個方法搬進來，去掉沒有意義的方法名稱，並加上 delegate 關鍵字。
    在以下這個例子中，==DoNothing_2 這個函數已經被**匿名**了==。
    ```csharp
    NoReturnWithPara method7 = new NoReturnWithPara(
        delegate(int a, int b) { //匿名方法
            Console.WriteLine("void DoNothing_2(int a, int b)");
        }
    );
    method7.Invoke(2, 3);
    ``` 
* .net 3.0 時代，引進lambda表達式
    拿掉delegate關鍵字，並加上箭頭=>
    **中間是箭頭=\>，左邊()是參數列表，右邊{}是方法體** 
    ```csharp
    NoReturnWithPara method8 = new NoReturnWithPara(
        (int a, int b) => { //lambda表達式的本質是匿名方法，也就是個方法(函數)
            Console.WriteLine("(int a, int b)=>{...}");
        }
    );
    ```
    ```csharp
    //參數型別可以省略，編譯器會自動推算
    NoReturnWithPara method9 = new NoReturnWithPara(
        (a, b) => {
            Console.WriteLine("(a, b)=>{...}");
        }
    );
    ```   
    ```csharp
    //如果方法體只有一行，大括號與分號也能去掉 (多行就不行了)
    NoReturnWithPara method10 = new NoReturnWithPara(
        (a, b) => Console.WriteLine("(a, b)=>...")
    );
    ```
    ```csharp
    //實例化委託時，可以省略 new NoReturnWithPara()
    NoReturnWithPara method11 = (a, b) => Console.WriteLine("(a, b)=>...");
    NoReturnNoPara method12 = () => Console.WriteLine("()=>...");
    ```
    到這裡已經是Lambda表達是最常見的樣子了。

### Action / Func
.net 3.0 時，微軟統一了委託的寫法，==引進Action與Func，取代了Delegate==，
**避免讓大家寫出各式各樣的委託**
* **Action** 是.net提供的『沒有參數沒有返回值』的委託
  也就相當於我們自己寫的 ```delegate void NoReturnNoPara()``` 
    ```csharp
    Action action1 = () => Console.WriteLine("hello");
    action1.Invoke();
    ```
* **Action\<int\>** 是『接受一個int，沒有返回值』的委託 
    ```csharp
    Action<int> action2 = (a) => Console.WriteLine("(a)=>...");
    Action<int> action2_1 = a => Console.WriteLine("a=>..."); //如果只有一個參數，可以把小括號去掉
    action2_1.Invoke(2);
    ```
* **Action\<int,string\>** 是『接受一個int一個string，沒有返回值』的委託 
    ```csharp
    Action<int, string> action3 = (a, b) => Console.WriteLine("(a,b)=>...");
    action3.Invoke(2, "3");
    ```
* **Func\<int\>** 是『沒有參數，返回int』的委託 
    ```csharp
    Func<int> func2 = new Func<int>(() => {
        return 5 + 6;
    });
    
    //簡化
    Func<int> func3 = () => { return 5 + 6; };
    
    //再簡化，如果方法體只有一行，大括號可以去掉，return也要去掉
    Func<int> func4 = () => 5 + 6;
    int i1 = func4.Invoke(); //Func的調用
    ```
* **Func\<int, string\>** 是『參數int，返回string』的委託
    ```csharp
    Func<int, string> func5 = (i) => "this is string" + i.ToString();
    string s = func5(5);
    ```

### 同步方法與異步方法
```csharp
private void button1_Click(object sender, EventArgs e){
    Action<string> action = do_something_long;

    //同步方法：sync，等待方法完成計算後，再進入下一行 (我們平常寫的程式) 
    //卡UI，同步方法是走主線程(也就是UI線程)
    action.Invoke("User1"); //等他跑完，會卡UI
    action.Invoke("User2"); //等他跑完，會卡UI
    action.Invoke("User3"); //等他跑完，會卡UI
    action.Invoke("User4"); //等他跑完，會卡UI

    //異步方法：async，不會等待方法完成，會直接進入下一行，非阻塞，不卡UI 
    action.BeginInvoke("User1", null, null); //開子線程執行
    action.BeginInvoke("User2", null, null); //開子線程執行
    action.BeginInvoke("User3", null, null); //開子線程執行
    action.BeginInvoke("User4", null, null); //開子線程執行
}
```
* 提升用戶體驗：==**不卡UI，背景作業**==
* 速度快，但不是線性增長-->開4個線程一起跑，速度不會變4倍
  -->越複雜差越多，資源換時間，但資源不一定夠-->線程絕不是越多越好
* 多線程有管理成本：==**程式不好寫，要有想像力！**==
* 有多個任務可以同時運行，如此才有多線程的價值。
  例如有一個任務特別複雜，但可以切成多個子任務「獨立進行」，則適合多線程。若不能切，則不適合多線程
* ==**子線程順序不可控！啟動無序！執行時間不確定！結束也無序！**==
  線程是.net向OS申請的，OS會如何分配線程？不可控！
  為什麼結束也不可控？即使是同一個線程執行同一個任務，執行時間也不可控。
  因為CPU時間被切片了！OS會如何分配切片？不可控！
* ==**不可以用sleep的方式控制子線程順序**==，即使現在沒問題，但壞事一定會發生的。
  -->應該透過回調(callback)，等待狀態，等待信號量來控制順序。

### 異步方法的控制
* 異步方法的回調(Callback)
```csharp=1
private void button2_Click(object sender, EventArgs e) {
    Action<string> action = do_something_long;

    //定義回調方法的委託
    AsyncCallback callback = ia => {
        Console.WriteLine(" --> " + ia.AsyncState + "任務完成了");
    };

    Console.WriteLine("---->異步方法開始 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] " + DateTime.Now.TimeOfDay + "<----");
    IAsyncResult ia1 = action.BeginInvoke("User1", callback, "<User1>"); //第1個參數是Action的參數
    IAsyncResult ia2 = action.BeginInvoke("User2", callback, "<User2>"); //第2個參數是回調方法的委託
    IAsyncResult ia3 = action.BeginInvoke("User3", callback, "<User3>"); //第3個參數是要傳遞到回調方法的任意object
    IAsyncResult ia4 = action.BeginInvoke("User4", callback, "<User4>"); //就是 上面的ia.AsyncState

```
* 異步方法的控制(1) - 等待狀態
```csharp=+
    while (!ia1.IsCompleted || !ia2.IsCompleted || !ia3.IsCompleted || !ia4.IsCompleted) {
        Thread.Sleep(5);
    }
```
* 異步方法的控制(2) - 等待信號量
```csharp=+
    ia1.AsyncWaitHandle.WaitOne();     //等待任務的完成，卡介面
    ia2.AsyncWaitHandle.WaitOne(5000); //等待任務的完成，但最多等5000毫秒
    ia3.AsyncWaitHandle.WaitOne();
    ia4.AsyncWaitHandle.WaitOne();
```
* 異步方法的控制(3) - 結束方法：
  對Func使用才有意義，可以得到結果。
  當然對Action也能使用，只是就沒有返回值，效果與前面一樣。
```csharp=+
    action.EndInvoke(ia1); //EndInvoke可以主動釋放線程
    action.EndInvoke(ia2); //但其實不用考慮線程釋放的問題
    action.EndInvoke(ia3); //線程結束後，.net會協助釋放
    action.EndInvoke(ia4);

    Console.WriteLine("---->異步方法結束 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] " + DateTime.Now.TimeOfDay + "<----");
}
```


### .net 1.0 Thread
```csharp=
private void Thread_Click(object sender, EventArgs e) {
    Console.WriteLine("---->1.0 按鈕開始 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] " + DateTime.Now.TimeOfDay + "<----");

    Action           action = () => do_something_long("Jack ");
    ThreadStart threadStart = () => do_something_long("Jack ");

    Thread thread = new Thread(threadStart);
    //Thread thread_0 = new Thread(action);  
    //錯誤，明明 action 與 threadStart 是一樣的，但 action 就是傳不進去
    //這就是為什麼要發明Action的原因：避免創造出各式各樣的委託 
    //Thread在1.0就有，但Action/Func是3.0才出現的

    //Thread實例化時，可以簡化成這樣
    Thread thread_1 = new Thread(() => do_something_long("Jack "));
    Thread thread_2 = new Thread(() => do_something_long("Bill "));

    //指定後台線程 (默認是前台線程)
    //前台線程: 線程一定要完成任務，阻止進程退出 (比較少用)
    //後台線程: 線程會隨著進程結束
    //這個是使用thread唯一的價值，往後的task, await/async都沒有這個功能
    thread_1.IsBackground = true; //預設為false(前台線程)
    thread_2.IsBackground = true;            

    //線程 Priority
    thread_1.Priority = ThreadPriority.Highest; 
    thread_2.Priority = ThreadPriority.Lowest;
    //這個只保證同一時間若是 thread_1/thread_2 同時來申請，OS 會優先給高優先的
    //但到底誰先做完，還是不可控的，所以這個屬性也沒多大意義

    //啟動線程
    thread_1.Start();
    thread_2.Start();

    //等待方法 1 - Join()
    thread_1.Join(5000); //最多等待5000毫秒
    thread_2.Join();     //等到完成

    //等待方法 2 - 等待狀態
    while (thread_1.ThreadState != System.Threading.ThreadState.Stopped) {
        Thread.Sleep(5);
    }

    Console.WriteLine("---->1.0 按鈕結束 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] " + DateTime.Now.TimeOfDay + "<----");

    //Thread還有很多方法，後來發現有缺點，造成不預期的結果，官方已不建議使用。例如：
    //thread_1.Suspend();    //掛起 (不一定能馬上暫停)
    //thread_1.Resume();     //喚醒 (不一定能馬上喚醒)
    //thread_1.Abort();      //停止 (不一定能馬上停止) (並且有些事情是停不住的)
    //等等
}
```         

### .net 2.0 Thread Pool
```csharp=
private void Thread_Pool_Click(object sender, EventArgs e) {
    Console.WriteLine("---->2.0 按鈕開始 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] " + DateTime.Now.TimeOfDay + "<----");
    //1.0 Thread
    //    (1) thread提供了太多的API，像是給了三歲小孩一把槍，造成很多誤傷
    //    (2) 無限使用線程，資源不可控
    //2.0 Thread Pool (線程池) (「池」意味著可以反覆利用，節省創建與銷毀過程的資源浪費)
    //    (1) 只給出必要的API
    //    (2) 線程數量加以限制
    //    (3) 重用線程，避免重複的創建與銷毀

    #region 線程啟動方式
    {
        //這個版本的操作是最簡單的，但功能也是最少的
        //只要一行，就可以啟動線程
        //不需要操作線程(創建/啟動/銷毀)，交給.net處理
        ThreadPool.QueueUserWorkItem(t => do_something_long("Eric "));

        //在2.0的 ThreadPool API中，沒辦法操作 暫停/恢復/銷毀/... 等動作
        //ThreadPool 啥都沒有，沒有操作線程的方法
        //把槍收走，避免誤傷！
        //連刀都不給你，只給一把筷子，你就傷害不了別人了！
    }
    #endregion

    #region 線程池中線程數量的管理
    {
        int maxWorkerThreads;        //線程池中的背景工作線程數最大值 (老師說平時只用到這個)
        int maxCompletionPortThread; //線程池中的非同步 I/O 線程數最大值
        int minWorkerThreads;        //線程池中的背景工作線程數最小值 (老師說平時只用到這個)
        int minCompletionPortThread; //線程池中的非同步 I/O 線程數最小值
        ThreadPool.GetMaxThreads(out maxWorkerThreads, out maxCompletionPortThread); //call by ref
        ThreadPool.GetMinThreads(out minWorkerThreads, out minCompletionPortThread); //call by ref
        Console.WriteLine("maxWorkerThreads        = " + maxWorkerThreads);
        Console.WriteLine("maxCompletionPortThread = " + maxCompletionPortThread);
        Console.WriteLine("minWorkerThreads        = " + minWorkerThreads);
        Console.WriteLine("minCompletionPortThread = " + minCompletionPortThread);

        Console.WriteLine("可以自己設定線程池的大小");
        ThreadPool.SetMaxThreads(16, 16);
        ThreadPool.SetMinThreads(5, 5);
        ThreadPool.GetMaxThreads(out maxWorkerThreads, out maxCompletionPortThread);
        ThreadPool.GetMinThreads(out minWorkerThreads, out minCompletionPortThread);
        Console.WriteLine("maxWorkerThreads        = " + maxWorkerThreads);
        Console.WriteLine("maxCompletionPortThread = " + maxCompletionPortThread);
        Console.WriteLine("minWorkerThreads        = " + minWorkerThreads);
        Console.WriteLine("minCompletionPortThread = " + minCompletionPortThread);
    }
    #endregion

    #region ManualResetEvent - 一個寫多線程時，常用的工具
    {
        ManualResetEvent manualResetEvent = new ManualResetEvent(false);
        //這是一個類，包含了一個bool屬性，可初始化為false
        //可通過Set()，把屬性變true；可通過Reset()，把屬性變false
        //若屬性為false，則WaitOne()過不去，卡UI
        //若屬性為true，WaitOne()就可以過去了

        //藉由ManualResetEvent，可以做到等待ThreadPool的線程完成
        ThreadPool.QueueUserWorkItem(t => {
            do_something_long("Bill ");
            Console.WriteLine("Bill 完成了 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] " + DateTime.Now.TimeOfDay + "<----");
            manualResetEvent.Set();  //將屬性設置為true
        });
        manualResetEvent.WaitOne();

        //這雖然可以用，但不是一個好主意
    }
    #endregion

    #region 最好不要阻礙線程池的線程
    // 由於上面已經將最大線程數設定為16，
    // 而下面這段代碼又希望在開啟第18個線程前都在等待，
    // 造成死鎖！
    {
        ManualResetEvent manualResetEvent = new ManualResetEvent(false);

        for (int i = 0; i < 20; i++) {
            int k = i;
            ThreadPool.QueueUserWorkItem(t => {
                Console.WriteLine("k=" + k);
                if (k < 18) {
                    manualResetEvent.WaitOne(); 
                }
                else {
                    manualResetEvent.Set();  //將屬性設置為true
                }
            });
        }
        if (manualResetEvent.WaitOne()) {
            Console.WriteLine("沒有死鎖");
        }

    }
    #endregion

    Console.WriteLine("---->2.0 按鈕結束 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] " + DateTime.Now.TimeOfDay + "<----");
}
```

### .net 3.0 Task 與 .net 4.5 Task.Run() ==這是目前主流寫法==
```csharp=
private void task_Click(object sender, EventArgs e) {
    //3.0 Task
    //    (1) Task是基於ThreadPool封裝的
    //    (2) 2.0時，ThreadPool的API功能太少，3.0的Task增加了多個API，滿足各種需求

    Console.WriteLine("---->3.0 按鈕開始 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] " + DateTime.Now.TimeOfDay + "<----");

    #region Start() - 線程啟動方式
    {
        //Start() - 啟動方式 1，取得實例，然後啟動
        Task task = new Task(() => do_something_long("Jack-1 "));
        task.Start();

        //Start() - 啟動方式 2，可以一行啟動，但取不到實例
        new Task(() => do_something_long("Jack-2 ")).Start();

        //Run() - 啟動方式 3 - 這個方法在 .net 4.5 才出現
        Task.Run(() => do_something_long("Eric-1 "));               //可以一行啟動
        Task task_1 = Task.Run(() => do_something_long("Eric-2 ")); //可以一行啟動，並取得實例
    }
    #endregion

    #region 控制線程數量也是可以用的，不過最好不要設定，用預設的就好
    // ThreadPool.SetMaxThreads(16, 16);
    // ThreadPool.SetMinThreads(4, 4);
    #endregion

    #region ContinueWith() - 回調(Callback)
    {
        Task task = new Task(() => do_something_long("Bill "));
        task.ContinueWith(t => {
            Console.WriteLine("Bill 工作做完了   [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "]");
        });
        task.Start();
        
        // 也可以寫成一行
        //Task.Run(() => do_something_long("Bill ")).ContinueWith(t => {
        //    Console.WriteLine("Bill 工作做完了   [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "]");
        //});
        
    }
    #endregion

    #region WaitAll() / WaitAny() - 「全部/某個」工作完成後，才能往下走 (卡介面)
    {
        Console.WriteLine("五個工作並發運行");
        List<Task> taskList = new List<Task>();
        taskList.Add(Task.Run(() => do_something_long("Alex ")));
        taskList.Add(Task.Run(() => do_something_long("Bill ")));
        taskList.Add(Task.Run(() => do_something_long("Ceil ")));
        taskList.Add(Task.Run(() => do_something_long("Dogy ")));
        taskList.Add(Task.Run(() => do_something_long("Elee ")));

        Task.WaitAny(taskList.ToArray());       //等待任一工作完成，卡介面
        //Task.WaitAny(taskList.ToArray(), 2000); //等待任一工作完成，最多2000毫秒，卡UI
        Console.WriteLine("任一工作做完了    [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "]");

        Task.WaitAll(taskList.ToArray());       //等待全部工作完成，卡介面
        //Task.WaitAll(taskList.ToArray(), 2000); //等待全部工作完成，最多2000毫秒，卡UI
        Console.WriteLine("任一工作做完了    [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "]");
    }
    #endregion

    #region WhenAll() / WhenAny() - 「全部/某個」工作完成後，做某件事 (Callback)(不卡介面)
    {
        Console.WriteLine("五個工作並發運行");
        List<Task> taskList = new List<Task>();
        taskList.Add(Task.Run(() => do_something_long("Alex ")));
        taskList.Add(Task.Run(() => do_something_long("Bill ")));
        taskList.Add(Task.Run(() => do_something_long("Ceil ")));
        taskList.Add(Task.Run(() => do_something_long("Dogy ")));
        taskList.Add(Task.Run(() => do_something_long("Elee ")));

        Task.WhenAny(taskList.ToArray()).ContinueWith(t => { //不卡介面
            Console.WriteLine("任一工作做完了    [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "]");
        });

        Task.WhenAll(taskList.ToArray()).ContinueWith(t => { //不卡介面
            Console.WriteLine("五個工作都做完了  [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "]");
        });
    }
    #endregion

    #region Thread.Sleep() / Task.Delay() - 等待與延遲的差異
    {
        //Thread.Sleep() 會卡介面
        //Task.Delay() 不會卡介面

        Stopwatch stopwatch = new Stopwatch(); //碼表

        //Thread.Sleep()的用法
        stopwatch.Restart();
        Thread.Sleep(2000); //這裡會卡介面
        stopwatch.Stop();
        Console.WriteLine("Sleep(2000) finish, stopwatch=" + stopwatch.ElapsedMilliseconds);

        //Task.Delay()的錯誤用法
        stopwatch.Restart();
        Task.Delay(2000);  //這裡不卡介面，但是也不會等待2000毫秒，這是錯誤的用法
        stopwatch.Stop();
        Console.WriteLine("Delay(2000) finish, stopwatch=" + stopwatch.ElapsedMilliseconds);

        //Task.Delay()的正確用法
        stopwatch.Restart();
        Task.Delay(2000).ContinueWith(t => {  //不卡介面，但等了兩秒才執行ContinueWith()
            Console.WriteLine("Delay(2000) finish, callback start");
            stopwatch.Stop();
            Console.WriteLine("callback finish, stopwatch=" + stopwatch.ElapsedMilliseconds);
        });
    }
    #endregion

    Console.WriteLine("---->3.0 按鈕結束 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] " + DateTime.Now.TimeOfDay + "<----");
}
```

### .net 4.0 Task Factory
```csharp=
private void task_factory_Click(object sender, EventArgs e) {
    Console.WriteLine("---->4.0 按鈕開始 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] " + DateTime.Now.TimeOfDay + "<----");

    #region StartNew() - 啟動方式
    {
        //回顧3.0 Task.Start() 的啟動方式  --> 需要兩行才能完成取得實例與啟動
        Task task = new Task(() => do_something_long("Jack "));
        task.Start();

        //現下4.0 Task Factory 的啟動方式 --> 只要一行即可取得實例與啟動
        Task task_1_1 = Task.Factory.StartNew(() => do_something_long("Vicki"));
        Task task_1_2 = Task.Factory.StartNew(
            (obj) => do_something_long("Vicki"),
            "<vicki>"); //可以傳一個 object 型別的「AsyncResult」進去給委託，這是Task.Start()做不到的

        //未來4.5 Task.Run() 的啟動方式 --> 只要一行即可取得實例與啟動
        Task task_2 = Task.Run(() => do_something_long("Eric-1 "));
    }
    #endregion

    #region ContinueWhenAny() / ContinueWhenAll() 
    {
        Console.WriteLine("五個工作並發運行");

        List<Task> taskList = new List<Task>();
        taskList.Add(Task.Factory.StartNew((obj) => do_something_long("Alex "), "<Alex>"));
        taskList.Add(Task.Factory.StartNew((obj) => do_something_long("Bill "), "<Bill>"));
        taskList.Add(Task.Factory.StartNew((obj) => do_something_long("Ceil "), "<Ceil>"));
        taskList.Add(Task.Factory.StartNew((obj) => do_something_long("Dogy "), "<Dogy>"));
        taskList.Add(Task.Factory.StartNew((obj) => do_something_long("Elee "), "<Elee>"));

        Task.Factory.ContinueWhenAny(taskList.ToArray(), t => {
            Console.WriteLine(t.AsyncState + " 工作做完了 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "]");
        });

        Task.Factory.ContinueWhenAll(taskList.ToArray(), t => {
            Console.WriteLine("五個工作都做完了  [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "]");
        });
    }
    #endregion

    Console.WriteLine("---->4.0 按鈕結束 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] " + DateTime.Now.TimeOfDay + "<----");
}
```

### .net 4.5 Parallel
```csharp=
private void parallel_Click(object sender, EventArgs e) {
    //Parallel 稱做並行編程，是基於Task的封裝
    Console.WriteLine("---->4.5 Parallel 按鈕開始 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] " + DateTime.Now.TimeOfDay + "<----");

    #region Parallel.Invoke() - 啟動方式
    {
        // 這樣一行，相當於之前要宣告 5 個 Task.Run，然後還要WaitAll()
        // 特色：主線程會參與計算，相當於節省了一個線程，但主線程就會卡介面了
        Parallel.Invoke(
            () => do_something_long("Alex-0"),
            () => do_something_long("Alex-1"),
            () => do_something_long("Alex-2"),
            () => do_something_long("Alex-3"),
            () => do_something_long("Alex-4")
        );
    }
    #endregion

    #region Parallel.For()
    {
        Parallel.For(0, 5, (i) => do_something_long("Alex-" + i.ToString()));
    }
    #endregion

    #region Parallel.Foreach()
    {
        List<string> list = new List<string>() { "A", "B", "C", "D", "E" };
        Parallel.ForEach(list, (s) => do_something_long("Alex-" + s));
    }
    #endregion

    Console.WriteLine("---->4.5 Parallel 按鈕結束 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] " + DateTime.Now.TimeOfDay + "<----");
}
```



### .net 4.5 await/async
await / async 這兩個是成對的，要用一起用，單獨使用是沒有意義的
這個就是讓你寫的方便點，沒有什麼特殊的功能 --> 用寫同步的方式來寫異步
```csharp=
private async void await_async_Click(object sender, EventArgs e) {
    Console.WriteLine("[主線程]---->4.5 Await / Async 按鈕開始 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] " + DateTime.Now.TimeOfDay + "<----");

    // 所有的方法都可以用 async 修飾
    //修飾前 private void NoReturnNoAwait()
    //修飾後 private async void NoReturnNoAwait()
    //只要方法被 Async 修飾了，那麼裡面就要出現 Await，否則會有綠色底線，不過可以通過編譯
    //Async方法裡面沒有Await是沒有意義的                
    NoReturnNoAwait();

    // 基本使用方法 (1) 沒有回傳值
    {
        NoReturn();
        for (int i = 0; i < 10; i++) {
            Console.WriteLine("[主線程] 其他任務 - " + i + " [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "]");
            Thread.Sleep(500); //主線程執行，卡UI
        }
    }
    
    // 基本使用方法 (2) 回傳 Task
    {
        Task task = Return_Task();
        await task.ConfigureAwait(false); //主線程到這裡就返回了
        for (int i = 0; i < 10; i++) {
            Console.WriteLine("[子線程] 其他任務 - " + i + " [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "]");
            Thread.Sleep(500);  //子線程執行，不卡UI
        }
    }

    // 基本使用方法 (3) 回傳 帶返回值的 Task<string>: 一個string的返回值
    {
        Task<string> task = Return_Task_String();
        await task.ConfigureAwait(false); //主線程到這裡就返回了
        Console.WriteLine("[子線程] 取得結果: " + task.Result);
        for (int i = 0; i < 10; i++) {
            Console.WriteLine("[子線程] 其他任務 - " + i + " [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "]");
            Thread.Sleep(500);  //子線程執行，不卡UI
        }
    }

    // 以前遇到的痛點
    {
        // 假設一個需求情境如下
        // 需要異步執行一個任務，但這個任務中，有許多小任務，寫必須依序完成                
        Task.Run(() => { //這裡是子線程執行
            string pizza = MakePizza_NoAwaitAsync();
            Console.WriteLine("    [子線程] 完成比薩 - " + pizza);                    
        });
        // 這裡是主線程執行
    }

    // await/async 的新寫法解決痛點
    {
        //前面要加上 async 才能用 await
        string pizza = await MakePizza_AwaitAsync();
        //這行以下都是子線程執行  
        Console.WriteLine("    [子線程] 完成比薩 - " + pizza);
    }

    // 最後的結論：
    // await/async 是會傳染的，要嘛不用，要嘛全部都要用
    // await/async 就是讓你寫的方便點，沒有什麼特殊的功能 --> 用寫同步的方式來寫異步

    Console.WriteLine("[主線程]---->4.5 Await / Async 按鈕結束 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] " + DateTime.Now.TimeOfDay + "<----");
}

```
```csharp=+
private async void NoReturnNoAwait() {
    // 只用 async 修飾方法，但方法裡面沒有用到 await，是沒有意義的
    Console.WriteLine("[主線程]--> NoReturnNoAwait() start <-- [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
    Task task = Task.Run(() => {  //啟動子線程
        Console.WriteLine("    [子線程] 任務開始 start [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
        Thread.Sleep(2000);  //任務
        Console.WriteLine("    [子線程] 任務結束 end [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
    });
    Console.WriteLine("[主線程]--> NoReturnNoAwait() end   <-- [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
}

private async void NoReturn() {
    Console.WriteLine("[主線程]--> NoReturn() start <-- [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
    Task task = Task.Run(() => {  //啟動子線程
        Console.WriteLine("    [子線程] 任務開始 start [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
        Thread.Sleep(3000);
        Console.WriteLine("    [子線程] 任務結束 end [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
    });

    await task.ConfigureAwait(false);  //主線程到這裡就返回了，執行外面的主線程任務
    //多加了 ConfigureAwait(false) 表示後續的任務不一定要用主線程執行 --> 不卡UI

    Console.WriteLine();
    Console.WriteLine("    -------------------這裡相當於是Callback方法-------------------");
    Console.WriteLine("    [子線程] 這裡是 await 以後的程式碼 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
    Console.WriteLine("    [子線程] --> NoReturn() end   <-- [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
    Console.WriteLine("    -------------------這裡相當於是Callback方法-------------------");
    Console.WriteLine();
}

private async Task Return_Task() {
    Console.WriteLine("[主線程]--> Return_Task() start <-- [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
    Task task = Task.Run(() => {  //啟動子線程
        Console.WriteLine("    [子線程] 任務開始 start [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
        Thread.Sleep(3000);
        Console.WriteLine("    [子線程] 任務結束 end [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
    });

    await task.ConfigureAwait(false);  //主線程到這裡就返回了，執行外面的主線程任務
    //多加了 ConfigureAwait(false) 表示後續的任務不一定要用主線程執行 --> 不卡UI

    Console.WriteLine();
    Console.WriteLine("    -------------------這裡相當於是Callback方法-------------------");
    Console.WriteLine("    [子線程] 這裡是 await 以後的程式碼 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
    Console.WriteLine("    [子線程] --> Return_Task() end   <-- [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
    Console.WriteLine("    -------------------這裡相當於是Callback方法-------------------");
    Console.WriteLine();
}

private async Task<string> Return_Task_String() {
    Console.WriteLine("[主線程]--> Return_Task_String() start <-- [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
    Task task = Task.Run(() => {  //啟動子線程
        Console.WriteLine("    [子線程] 任務開始 start [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
        Thread.Sleep(3000);
        Console.WriteLine("    [子線程] 任務結束 end [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
    });

    await task.ConfigureAwait(false);  //主線程到這裡就返回了，執行外面的主線程任務
    //多加了 ConfigureAwait(false) 表示後續的任務不一定要用主線程執行 --> 不卡UI

    Console.WriteLine();
    Console.WriteLine("    -------------------這裡相當於是Callback方法-------------------");
    Console.WriteLine("    [子線程] 這裡是 await 以後的程式碼 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
    Console.WriteLine("    [子線程] --> Return_Task_String() end   <-- [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
    Console.WriteLine("    -------------------這裡相當於是Callback方法-------------------");
    Console.WriteLine();

    return "這是Return_Task_String()的返回值";
}

```
```csharp=+
private string MakePizza_NoAwaitAsync() {   
    Task<string> task = 
        Task.Run(() => 製作麵團("黃金麵粉"))
            .ContinueWith(t => 麵團發酵(t.Result))
            .ContinueWith(t => 做成餅皮(t.Result))
            .ContinueWith(t => 做成比薩(t.Result));
    return task.Result;
}

private async Task<string> MakePizza_AwaitAsync() {
    string result;
    result = await Task.Run(() => 製作麵團("黃金麵粉")).ConfigureAwait(false);
    result = await Task.Run(() => 麵團發酵(result)).ConfigureAwait(false);
    result = await Task.Run(() => 做成餅皮(result)).ConfigureAwait(false);
    result = await Task.Run(() => 做成比薩(result)).ConfigureAwait(false);
    return result;
}

private string 製作麵團(string name) {
    Console.WriteLine("    [子線程] 用<" + name + ">製作麵團 開始 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
    Thread.Sleep(1000);
    Console.WriteLine("    [子線程] 用<" + name + ">製作麵團 完成 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
    return "用" + name + "做好的麵團";
}

private string 麵團發酵(string name) {
    Console.WriteLine("    [子線程] <" + name + ">發酵 開始 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
    Thread.Sleep(1000);
    Console.WriteLine("    [子線程] <" + name + ">發酵 完成 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
    return "發酵的" + name;
}

private string 做成餅皮(string name) {
    Console.WriteLine("    [子線程] 用<" + name + ">做成餅皮 開始 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
    Thread.Sleep(1000);
    Console.WriteLine("    [子線程] 用<" + name + ">做成餅皮 完成 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
    return "用" + name + "做成的餅皮";
}

private string 做成比薩(string name) {
    Console.WriteLine("    [子線程] 用<" + name + ">做成比薩 開始 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
    Thread.Sleep(1000);
    Console.WriteLine("    [子線程] 用<" + name + ">做成比薩 完成 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] ");
    return "用" + name + "做成的比薩";
}
```

### 多線程的其他觀念
* 多線程的異常處理：**線程裡面的異常會遺失！**
  建議在Action裡面包一層try-catch，在線程裡，不要出現異常；如果有異常，在線程裡面處理好．
```csharp=
private void error_handling_Click(object sender, EventArgs e) {
    Console.WriteLine("---->多線程的異常處理開始 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] " + DateTime.Now.TimeOfDay + "<----");

    // 線程裡面的異常會遺失
    try {
        List<Task> taskList = new List<Task>();
        for (int i = 0; i < 20; i++) {
            string name = "Jack-" + i.ToString();

            Action<object> action = s => {
                Thread.Sleep(1000);
                if (s.ToString() == "Jack-11") throw new Exception("Jack-11 Fail!"); //這個異常會遺失
                if (s.ToString() == "Jack-12") throw new Exception("Jack-12 Fail!"); //這個異常會遺失
                Console.WriteLine(s + " Finish");
            };

            Task task = new Task(action, name);
            task.Start();
            taskList.Add(task);
        }
        //Task.WaitAll(taskList.ToArray());  //除非在這裡等，外面才能抓到異常，但是會卡介面
    }
    catch (AggregateException aex) {
        foreach (Exception ex in aex.InnerExceptions) {
            Console.WriteLine("Error: " + ex.Message);
        }
    }
    catch (Exception ex) {
        Console.WriteLine("Error: " + ex.Message);
    }

    // 建議在Action裡面包一層try-catch
    // 在線程裡，不要出現異常；如果有異常，在線程裡面處理好
    try {
        List<Task> taskList = new List<Task>();
        for (int i = 0; i < 20; i++) {
            string name = "Jack-" + i.ToString();

            Action<object> action = s => {
                try {
                    Thread.Sleep(1000);
                    if (s.ToString() == "Jack-11") throw new Exception("Jack-11 Fail!"); //異常不會遺失
                    if (s.ToString() == "Jack-12") throw new Exception("Jack-12 Fail!"); //異常不會遺失
                    Console.WriteLine(s + " Finish");
                }
                catch (Exception ex) { //這裡會抓到異常
                    Console.WriteLine("Error In Action: " + ex.Message);
                }
            };

            Task task = new Task(action, name);
            task.Start();
            taskList.Add(task);
        }
        //Task.WaitAll(taskList.ToArray());  //除非在這裡等，外面才能抓到異常，但是會卡介面，不好
    }
    catch (AggregateException aex) {
        foreach (Exception ex in aex.InnerExceptions) {
            Console.WriteLine("Error: " + ex.Message);
        }
    }
    catch (Exception ex) {
        Console.WriteLine("Error: " + ex.Message);
    }

    Console.WriteLine("---->多線程的異常處理結束 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] " + DateTime.Now.TimeOfDay + "<----");
}
```
* 線程取消：**線程是沒有辦法從外部停止的**，這麼做很不好！
  應該在線程內部，自己停止自己。通常的作法是定期檢查一個flag，判斷是否結束線程。
```csharp=
private void cancel_Click(object sender, EventArgs e) {
    Console.WriteLine("---->線程取消按鈕開始 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] " + DateTime.Now.TimeOfDay + "<----");

    // 定期檢查一個flag，判斷是否結束線程           
    CancellationTokenSource cts = new CancellationTokenSource();
    //也可以用bool，但CancellationTokenSource多了一些功能(自動拋異常/不啟動)
    //bool bCancelThread = false; 
    try {
        List<Task> taskList = new List<Task>();
        for (int i = 0; i < 30; i++) {
            string name = "Jack-" + i.ToString();

            Action<object> action = s => {
                try {
                    Thread.Sleep(1000);
                    if (s.ToString() == "Jack-11") throw new Exception("Jack-11 Fail!"); //異常不會遺失
                    if (s.ToString() == "Jack-12") throw new Exception("Jack-12 Fail!"); //異常不會遺失

                    if (cts.IsCancellationRequested) {
                    //if (bCancelThread) {
                        Console.WriteLine(s + " 放棄執行!");
                    }
                    else {
                        Console.WriteLine(s + " Finish");
                    }

                }
                catch (Exception ex) { //這裡會抓到異常
                    cts.Cancel();
                    //bCancelThread = true;
                    Console.WriteLine("Error In Action: " + ex.Message);
                }
            };

            //建立Task時，可以加入 CancellationTokenSource
            Task task = new Task(action, name, cts.Token);
            task.Start();
            taskList.Add(task);
        }
        Task.WaitAll(taskList.ToArray());
    }
    catch (AggregateException aex) {
        foreach (Exception ex in aex.InnerExceptions) {
            Console.WriteLine("Error: " + ex.Message);
        }
    }
    catch (Exception ex) {
        Console.WriteLine("Error: " + ex.Message);
    }

    Console.WriteLine("---->線程取消按鈕結束 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] " + DateTime.Now.TimeOfDay + "<----");
}
```
* 多線程的臨時變量 (==**這是一個坑！**==)
```csharp=
private void forLoop_Click(object sender, EventArgs e) {
    Console.WriteLine("---->多線程的臨時變量按鈕開始 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] " + DateTime.Now.TimeOfDay + "<----");

    for (int i = 0; i < 5; i++) {
        int k = i;   //這行一定要寫，非常重要
        Task.Run(() => Console.WriteLine("k=" + k + ", i=" + i) );
    }
    // 全程有5個k，分別是0,1,2,3,4
    // 全程只有1個i，而在打印時，i早就變成5了    

    Console.WriteLine("---->多線程的臨時變量按鈕結束 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] " + DateTime.Now.TimeOfDay + "<----");
}
```
* 線程安全，這是多線程程式裡，最麻煩的事情！
  同時有多個線程寫入同一個東西，就會發生==覆蓋==！
  (同一個變數)(同一個集合)(同一個文件)(同一個資料庫)
```csharp=
private static readonly object _lock = new object(); // 線程安全鎖
private void lock_Click(object sender, EventArgs e) {
    Console.WriteLine("---->線程安全開始 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] " + DateTime.Now.TimeOfDay + "<----");

    // 單線程的情況
    {
        int iTotalCount = 0;
        List<int> listInt = new List<int>();
        for (int i = 0; i < 10000; i++) {
           iTotalCount++;
           listInt.Add(i);
        }
        Console.WriteLine("iTotalCount=" + iTotalCount);
        Console.WriteLine("listInt.Count=" + listInt.Count);
    }

    // 多線程的情況 (線程不安全)
    {
        int iTotalCount = 0;
        List<int> listInt = new List<int>();
        List<Task> taskList = new List<Task>();
        for (int i = 0; i < 10000; i++) {
            int i_new = i;
            taskList.Add(Task.Run(() => {
                iTotalCount++;
                listInt.Add(i_new);
            }));
        }
        Task.WaitAll(taskList.ToArray());
        Console.WriteLine("iTotalCount=" + iTotalCount);
        Console.WriteLine("listInt.Count=" + listInt.Count);
        // 同時有多個線程寫入同一個東西，就會發生覆蓋！
        // (同一個變數)(同一個集合)(同一個文件)(同一個資料庫)
    }

    // region 線程安全鎖
    {
        int iTotalCount = 0;
        List<int> listInt = new List<int>();
        List<Task> taskList = new List<Task>();
        for (int i = 0; i < 10000; i++) {
            int i_new = i;
            taskList.Add(Task.Run(() => {
                lock (_lock) {  //線程安全鎖，保證在大括號中，只有一個線程運行
                    iTotalCount++;
                    listInt.Add(i_new);
                }
            }));
        }
        Task.WaitAll(taskList.ToArray());
        Console.WriteLine("iTotalCount=" + iTotalCount);
        Console.WriteLine("listInt.Count=" + listInt.Count);

    }

    Console.WriteLine("---->線程安全結束 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] " + DateTime.Now.TimeOfDay + "<----");
}
```
* **線程安全鎖**，這個方法解決了問題，但犧牲了性能，所以鎖起來的東西要越少越好。
  最好能避免線程衝突，例如利用「==數據拆分==」的方式，並發寫多個小數據，最後再合成一個大數據。(多個小數據)(多個小集合)(多個小文件)...
  另外，==資料庫是沒辦法拆分的==！


