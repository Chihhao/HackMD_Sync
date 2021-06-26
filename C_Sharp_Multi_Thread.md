---
tags: C#
---

# C#-多執行緒API

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
* ex2. 無返回+有參數
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
* ex3. 有返回+無參數
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
* ex4. 有返回+有參數
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
* ex5. 無返回+有泛型參數
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
