---
tags: C#
---
# C#-資料結構API
* [Source Code](https://github.com/Chihhao/C_Sharp_Advance)
* [Burt Zhang](https://www.youtube.com/channel/UCnA36j_KoX-Z0QsayrLCsOg/videos) 這個頻道搜集了很多個Eleven老師的教學影片，非常精彩。

---

### 「陣列」Array, ArrayList, List
內存連續分配，可座標訪問，==讀取快，增刪慢==
* #### Array (固定類型)(固定長度)
```csharp
int[] intArray = new int[10];
intArray[0] = 87;
string[] strArray = new string[3] { "1", "22", "333" };
for (int i = 0; i < strArray.Length; i++) {
    Console.WriteLine(strArray[i]);
}
```
* #### ArrayList (不固定類型)(不固定長度)
```csharp
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
```csharp
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
```csharp
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
```csharp
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
```csharp
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
```csharp
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
```csharp
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
```csharp
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
```csharp
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
```csharp
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





