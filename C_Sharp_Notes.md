---
tags: C#
---
# C# 綜合筆記
* [我的GitHub](https://github.com/Chihhao/C_Sharp_Advance)
* [Burt Zhang YouTube](https://www.youtube.com/channel/UCnA36j_KoX-Z0QsayrLCsOg/videos)
這個頻道搜集了很多個Eleven老師的教學影片，非常精彩。
* [什麼是事件(Event)？](https://www.tutorialsteacher.com/csharp/csharp-event)


## 資料結構 API
### 「陣列」Array, ArrayList, List
內存連續分配，可座標訪問，==讀取快，增刪慢==
* #### Array (固定類型)(固定長度)
```C#
int[] intArray = new int[10];
intArray[0] = 87;
string[] strArray = new string[3] { "1", "22", "333" };
for (int i = 0; i < strArray.Length; i++) {
    Console.WriteLine(strArray[i]);
}
```
* #### ArrayList (不固定類型)(不固定長度)
```C#
ArrayList arrayList = new ArrayList();  //宣告時不知道有多長
arrayList.Add("0-Eleven");     //添加string數據，會增加長度
arrayList.Add("1-Eric");
arrayList.Add(2);              //添加int數據，會增加長度
arrayList.Add(3);
arrayList.Add(4);
//arrayList[5] = 26;           //這裡會Runtime error: 索引賦值時，不能增加長度
//arrayList.RemoveAt(5);       //這裡會Runtime error: 超過範圍
arrayList.RemoveAt(3);         //刪除數據 by index
arrayList.Remove("0-Eleven");  //刪除數據 by 內容 (滿足的第一個)
for (int i = 0; i < arrayList.Count; i++) {
    Console.WriteLine(arrayList[i]);
}
```
* #### List (==泛型==)(不固定長度)(==最常用==)
```C#
List<int> intList = new List<int>();  //宣告時不知道有多長
List<string> stringList = new List<string>();  //可以換成各種常用型別
stringList.Add("0-Eleven");     //添加數據，會增加長度
stringList.Add("1-Eric");
stringList.Add("2-Jack");
stringList.Add("3-Kevin");
//stringList[4] = "4-Susan";    //這裡會Runtime error: 索引賦值時，不能增加長度
//stringList.RemoveAt(4);       //這裡會Runtime error: 超過範圍
stringList.RemoveAt(0);         //刪除數據 by index
stringList.Remove("2-Jack");    //刪除數據 by 內容 (滿足的第一個)
for (int i = 0; i < stringList.Count; i++) {
    Console.WriteLine(stringList[i]);
}
```
### 「鏈表」LinkedList, Queue, Stack
內存非連續分配，不可座標訪問，==增刪快，讀取慢==
* #### LinkedList (泛型)(不固定長度)
```C#
LinkedList<int> intLinkList = new LinkedList<int>();
intLinkList.AddFirst(123);
intLinkList.AddFirst(456);
intLinkList.AddLast(4);
intLinkList.AddLast(5);
intLinkList.AddLast(6);
intLinkList.AddLast(7);

bool isContain = intLinkList.Contains(123);  //是否包含某數據
LinkedListNode<int> node123 = intLinkList.Find(123);   //找到節點
intLinkList.AddBefore(node123, 1);     //在結點前添加數據    
intLinkList.AddAfter(node123, 3);      //在結點後添加數據
intLinkList.AddAfter(node123, 2);      //在結點後添加數據

intLinkList.Remove(456);
intLinkList.Remove(node123);
intLinkList.RemoveFirst();
intLinkList.RemoveLast();

//查找不方便，如果不用foreach，就要靠一個一個串接下去查
foreach (int item in intLinkList) {
    Console.WriteLine(item);
}
intLinkList.Clear();
```
* #### Queue (泛型)(不固定長度)(先進先出)
```C#
Queue<string> stringQueue = new Queue<string>();  //建立隊列
stringQueue.Enqueue("one");     //加入隊列
stringQueue.Enqueue("two");
stringQueue.Enqueue("three");
stringQueue.Enqueue("four");
stringQueue.Enqueue("five");

foreach (string item in stringQueue) {
    Console.WriteLine(item);
}
Console.WriteLine("離開隊列: " + stringQueue.Dequeue());  //先進先出，取出一個 (直接取出)
Console.WriteLine("下一個要離開隊列的是: " + stringQueue.Peek());  //先進先出，取出一個 (僅查詢)
Console.WriteLine("離開隊列: " + stringQueue.Dequeue());

//Queue可以轉成數組，也可利用數組建立Queue
Queue<string> copyQueue = new Queue<string>(stringQueue.ToArray());
foreach (string item in copyQueue) {
    Console.WriteLine(item);
}
Console.WriteLine("隊列中是否包含four: " + copyQueue.Contains("four"));
Console.WriteLine("隊列中有幾個元素: " + copyQueue.Count);
copyQueue.Clear();
```
* #### Stack (泛型)(不固定長度)(後進先出)
```C#
Stack<string> stringStack = new Stack<string>();  //建立堆疊
stringStack.Push("one");     //加入堆疊
stringStack.Push("two");
stringStack.Push("three");
stringStack.Push("four");
stringStack.Push("five");

foreach (string item in stringStack) {
    Console.WriteLine(item);
}
Console.WriteLine("離開堆疊: " + stringStack.Pop());  //後進先出，取出一個 (直接取出)
Console.WriteLine("下一個要離開堆疊的是: " + stringStack.Peek());  //後進先出，取出一個 (僅查詢)
Console.WriteLine("離開堆疊: " + stringStack.Pop());

//Stack可以轉成數組，也可利用數組建立Stack
Stack<string> copyStack = new Stack<string>(stringStack.ToArray());
foreach (string item in copyStack) {
    Console.WriteLine(item);
}
Console.WriteLine("隊伍中是否包含four: " + copyStack.Contains("four"));
Console.WriteLine("隊伍中有幾個元素: " + copyStack.Count);
copyStack.Clear();
```
### 「集合」HashSet, SortedSet
內存非連續分配，不可座標訪問，「**除去重複**」是集合最主要的價值
* #### HashSet (泛型)(不固定長度)(去重)(沒有順序)
```C#
HashSet<string> stringHashSet = new HashSet<string>();  //建立集合
stringHashSet.Add("zero");     // 加入集合
stringHashSet.Add("one");
stringHashSet.Add("two");
stringHashSet.Add("three");
stringHashSet.Add("three");    // 重複元素，不會加入
stringHashSet.Add("three");    // 重複元素，不會加入

foreach (string item in stringHashSet) {
    Console.WriteLine(item);
}
Console.WriteLine("集合中是否包含four: " + stringHashSet.Contains("four"));
Console.WriteLine("集合中有幾個元素: " + stringHashSet.Count);

//這是另一個集合
HashSet<string> hashSet_2 = new HashSet<string>();  //建立集合
hashSet_2.Add("one");
hashSet_2.Add("two");
hashSet_2.Add("three");
hashSet_2.Add("four");
hashSet_2.Add("five");

//兩個集合可以完成「交差併補」的操作: 交集/差集/聯集/補集 
stringHashSet.SymmetricExceptWith(hashSet_2); //補集
stringHashSet.UnionWith(hashSet_2);           //聯集 (並集)
stringHashSet.ExceptWith(hashSet_2);          //差集 
stringHashSet.IntersectWith(hashSet_2);       //交集
```
* #### SortedSet (泛型)(不固定長度)(去重)(==有順序==)
```C#
SortedSet<string> stringSortedSet = new SortedSet<string>();  //建立集合
stringSortedSet.Add("zero");     // 加入集合
stringSortedSet.Add("one");
stringSortedSet.Add("two");
stringSortedSet.Add("three");
stringSortedSet.Add("four");
stringSortedSet.Add("three");    // 重複元素，不會加入

foreach (string item in stringSortedSet) {
    Console.WriteLine(item);
}
Console.WriteLine("集合中是否包含four: " + stringSortedSet.Contains("four"));
Console.WriteLine("集合中有幾個元素: " + stringSortedSet.Count);
```
### 「Key-Value型式」Hashtable, Dictionary, SortedDictionary
內存非連續分配，不可座標訪問，不能重複；
查找數據時，利用Key一次定位，==增刪快，讀取也快！== (空間換時間)(數據多效率會下降)
* #### Hashtable (object類型)(不固定長度)(資料不按加入順序)(無排序)
object 類型 --> 需要一定的轉換時間 --> 效能損失
```C#
Hashtable hashtable = new Hashtable();  //建立哈希表
hashtable.Add("zero", "0000000000");    //利用Key與Value加入哈希表
hashtable.Add(1, "111111111");          //Key: 不固定類型
hashtable.Add("two", 2);                //Value: 不固定類型
hashtable.Add("3", DateTime.Now);
hashtable.Add("four", "444");
//hashtable.Add("four", "555");         //Runtime error: 相同Key不能重複加入
hashtable.Add("four-2", "444");         //不同Key，相同Value，是可以的

foreach (DictionaryEntry objDE in hashtable) {
    //一次拿到一對資料，包含一個Key一個Value
    Console.WriteLine(objDE.Key.ToString() + "\t-\t" + objDE.Value.ToString());
}
Console.WriteLine("哈希表中是否包含Key=\"four\"的資料: " + hashtable.Contains("four"));
Console.WriteLine("哈希表中有幾個元素: " + hashtable.Count);

//線程安全，保證只有一個線程寫數據，其他線程只能讀
Hashtable mySyncdHT = Hashtable.Synchronized(hashtable);
Console.WriteLine("hashtable 是不是線程安全的？ {0}", hashtable.IsSynchronized ? "是" : "否");
Console.WriteLine("mySyncdHT 是不是線程安全的？ {0}", mySyncdHT.IsSynchronized ? "是" : "否");
```
* #### Dictionary (泛型)(不固定長度)(資料按加入順序)(無排序)
```C#
Dictionary<int, string> dictionary = new Dictionary<int, string>();  //建立字典
dictionary.Add(1, "Jack");        //利用Key與Value加入字典
dictionary.Add(5, "Kevin");
dictionary.Add(3, "Bill");
dictionary.Add(2, "Pond");
dictionary.Add(4, "Eric");
//dictionary.Add(2, "Daniel");    //Runtime error: 相同Key不能重複加入

foreach (var item in dictionary) {
    Console.WriteLine(item.Key + "\t-\t" + item.Value);
}
Console.WriteLine("字典表中是否包含Key=4的資料: " + dictionary.ContainsKey(4));
Console.WriteLine("字典表中是否包含Value=\"Jimmy\"的資料: " + dictionary.ContainsValue("Jimmy"));
Console.WriteLine("字典中有幾個元素: " + dictionary.Count);

//线程安全的字典
ConcurrentDictionary<int, string> concurrentDictionary =
    new ConcurrentDictionary<int, string>(dictionary);    
```
* #### SortedDictionary (泛型)(不固定長度)(資料按加入順序)(有排序)
```C#
SortedDictionary<int, string> sortedDictionary = new SortedDictionary<int, string>();  //建立字典
sortedDictionary.Add(1, "Jack");        //利用Key與Value加入字典
sortedDictionary.Add(5, "Kevin");
sortedDictionary.Add(3, "Bill");
sortedDictionary.Add(2, "Pond");
sortedDictionary.Add(4, "Eric");
//sortedDictionary.Add(2, "Daniel");    //Runtime error: 相同Key不能重複加入

foreach (var item in sortedDictionary) {
    Console.WriteLine(item.Key + "\t-\t" + item.Value);
}
Console.WriteLine("字典表中是否包含Key=4的資料: " + sortedDictionary.ContainsKey(4));
Console.WriteLine("字典表中是否包含Value=\"Jimmy\"的資料: " + sortedDictionary.ContainsValue("Jimmy"));
Console.WriteLine("字典中有幾個元素: " + sortedDictionary.Count);  
```


## 多線程相關 API

請參考Elevan老師的教學影片([第1集](https://www.youtube.com/watch?v=bM7s2gmnNC0&t=3935s))([第2集](https://www.youtube.com/watch?v=gzt5zG-5HPE&t=2715s))([第3集](https://www.youtube.com/watch?v=PR-r7oWlV5Y&t=5349s))([第4集](https://www.youtube.com/watch?v=WGUb59-m_eM&t=3s))([第5集](https://www.youtube.com/watch?v=B-UUii_hNbM&t=2692s))([第6集](https://www.youtube.com/watch?v=5LcsC1ufRRo))
以下節錄我認為的重點。
* **進程(process)**：一個程序運行時，佔用的全部計算資源的總和
* **線程(thread)**：程序執行的最小單位，任何操作都是由線程完成的。線程是依託於進程存在的，一個進程可以包含多個線程，線程也可以有自己的運算資源。
* **多線程(multi-thread)**：多個執行流同時運行
    * 可以簡易理解為CPU太快了，分時間片-->需要上下文切換(加載-->計算-->保存環境)
    * 微觀：一個核同一時刻只能執行一個線程
    * 宏觀：多線程並發，多CPU、多核是可以獨立運作的
    * 選購CPU時講的四核八線程，是指有八個偽核，可以想像成有4*8=32個內核，32個內核可獨立運作；此線程非彼線程。
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
```c#
// 這是委託的宣告
public delegate void NoReturnNoPara(); 
// 這是準備好，要被委託的方法
static public void DoNothing_1() {
    Console.WriteLine("void DoNothing_1()");
}
```
```c#
// 委託實例化
NoReturnNoPara method1 = new NoReturnNoPara(DoNothing_1); 
method1.Invoke(); //呼叫委託，這三個是一樣意思的
method1();        //呼叫委託，這三個是一樣意思的
DoNothing_1();    //呼叫委託，這三個是一樣意思的
```
* Case2. 無返回+有參數
```C#
public delegate void NoReturnWithPara(int a, int b);
static public void DoNothing_2(int a, int b) {
    Console.WriteLine("void DoNothing_2(int a, int b)");
}
```
```c#
NoReturnWithPara method2 = new NoReturnWithPara(DoNothing_2);
method2.Invoke(3, 5);
```
* Case3. 有返回+無參數
```C#
public delegate int WithReturnNoPara();
static public int DoNothing_3() {
    Console.WriteLine("int DoNothing_3()");
    return 3;
}
```
```c#
WithReturnNoPara method3 = new WithReturnNoPara(DoNothing_3);
int i3 = method3.Invoke();
```
* Case4. 有返回+有參數
```C#
public delegate int WithReturnWithPara(int a, int b);
static public int DoNothing_4(int a, int b) {
    Console.WriteLine("int DoNothing_4(int a, int b)");
    return 4;
}
```
```c#
WithReturnWithPara method4 = new WithReturnWithPara(DoNothing_4);
int i4 = method4.Invoke(1, 2);
```
* Case5. 無返回+有泛型參數
```C#
public delegate void NoReturnWithPara<T>(T t);
static public void DoNothing_5(String s) {
    Console.WriteLine("void DoNothing_5(String s)");
}
```
```c#
NoReturnWithPara<string> method5 = new NoReturnWithPara<string>(DoNothing_5);
method5.Invoke("123");
```

### Lambda表達式的演進
Lambda表達式的精神就是==匿名函數==

* .net 1.0 時代，就是單純的委託
    ```c#
    public delegate void NoReturnWithPara(int a, int b);
    static public void DoNothing_2(int a, int b) {
        Console.WriteLine("void DoNothing_2(int a, int b)");
    }
    ```
    ```C#
    NoReturnWithPara method6 = new NoReturnWithPara(DoNothing_2);
    method6.Invoke(2, 3);
    ```
* .net 2.0 時代，直接把整個方法搬進來，去掉沒有意義的方法名稱，並加上 delegate 關鍵字。
    在以下這個例子中，==DoNothing_2 這個函數已經被**匿名**了==。
    ```C#
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
    ```C#
    NoReturnWithPara method8 = new NoReturnWithPara(
        (int a, int b) => { //lambda表達式的本質是匿名方法，也就是個方法(函數)
            Console.WriteLine("(int a, int b)=>{...}");
        }
    );
    ```
    ```C#
    //參數型別可以省略，編譯器會自動推算
    NoReturnWithPara method9 = new NoReturnWithPara(
        (a, b) => {
            Console.WriteLine("(a, b)=>{...}");
        }
    );
    ```   
    ```C#
    //如果方法體只有一行，大括號與分號也能去掉 (多行就不行了)
    NoReturnWithPara method10 = new NoReturnWithPara(
        (a, b) => Console.WriteLine("(a, b)=>...")
    );
    ```
    ```C#
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
    ```C#
    Action action1 = () => Console.WriteLine("hello");
    action1.Invoke();
    ```
* **Action\<int\>** 是『接受一個int，沒有返回值』的委託 
    ```C#
    Action<int> action2 = (a) => Console.WriteLine("(a)=>...");
    Action<int> action2_1 = a => Console.WriteLine("a=>..."); //如果只有一個參數，可以把小括號去掉
    action2_1.Invoke(2);
    ```
* **Action\<int,string\>** 是『接受一個int一個string，沒有返回值』的委託 
    ```C#
    Action<int, string> action3 = (a, b) => Console.WriteLine("(a,b)=>...");
    action3.Invoke(2, "3");
    ```
* **Func\<int\>** 是『沒有參數，返回int』的委託 
    ```C#
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
    ```C#
    Func<int, string> func5 = (i) => "this is string" + i.ToString();
    string s = func5(5);
    ```

### 同步方法與異步方法
```C#
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
```C#=1
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
```C#=+
    while (!ia1.IsCompleted || !ia2.IsCompleted || !ia3.IsCompleted || !ia4.IsCompleted) {
        Thread.Sleep(5);
    }
```
* 異步方法的控制(2) - 等待信號量
```C#=+
    ia1.AsyncWaitHandle.WaitOne();     //等待任務的完成，卡介面
    ia2.AsyncWaitHandle.WaitOne(5000); //等待任務的完成，但最多等5000毫秒
    ia3.AsyncWaitHandle.WaitOne();
    ia4.AsyncWaitHandle.WaitOne();
```
* 異步方法的控制(3) - 結束方法：
  對Func使用才有意義，可以得到結果。
  當然對Action也能使用，只是就沒有返回值，效果與前面一樣。
```C#=+
    action.EndInvoke(ia1); //EndInvoke可以主動釋放線程
    action.EndInvoke(ia2); //但其實不用考慮線程釋放的問題
    action.EndInvoke(ia3); //線程結束後，.net會協助釋放
    action.EndInvoke(ia4);

    Console.WriteLine("---->異步方法結束 [" + Thread.CurrentThread.ManagedThreadId.ToString("00") + "] " + DateTime.Now.TimeOfDay + "<----");
}
```

### .net 1.0 Thread 

### .net 2.0 Thread Pool

### .net 3.0 Task 與 .net 4.5 Task.Run()

### .net 4.0 Task Factory

### .net 4.5 Parallel

### 多線程的其他觀念
* 多線程的異常處理
* 線程取消
* 多線程的臨時變量
* 線程安全


## 設計模式
* [一秒看破 裝飾者模式 Decorator Pattern](http://weisnote.blogspot.com/2013/05/decorator-pattern.html)
* [設計模式教學影片](https://ke.qq.com/webcourse/index.html#cid=456617&term_id=100546373&taid=7199666563643305&vid=5285890796401082033)
* [微软MVP-Eleven](https://space.bilibili.com/486089130/video?tid=0&page=1&keyword=&order=pubdate)





## 網路
* [TCP-非常清新的C#网络通信](https://www.bilibili.com/video/BV1zx411u7bQ?p=4&spm_id_from=pageDriver)
* [C#|Socket基础编程实战教程详解|由浅入深带你理解socket(Suppersocket)](https://www.bilibili.com/video/BV1Vy4y117p4?from=search&seid=8860391722785697097)


## 資料庫
* [使用 DataSet 管理從 MySQL 查詢出來的資料](https://godstamps.blogspot.com/2012/02/c-dataset-mysql.html)

