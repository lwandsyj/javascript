1.IndexedDB 是一个用于在浏览器中储存较大数据结构的 Web API
   它是使用 JavaScript 对象而非列数固定的表格来储存数据的.
   用于客户端存储大量结构化数据(包括, 文件/ blobs)
   该API使用索引来实现对该数据的高性能搜索

2.IndexedDB是一个事务型数据库系统，类似于基于SQL的RDBMS。 然而，
  不像RDBMS使用固定列表，IndexedDB是一个基于JavaScript的面向对象的数据库
  IndexedDB 是 WebSQL 数据库的取代品, W3C组织在2010年11月18日废弃了webSql.  
  IndexedDB 和WebSQL的不同点在于WebSQL 是关系型数据库（复杂）IndexedDB 是key-value型数据库（简单好使）.
   
      (1) IndexedDB 数据库使用 key-value 键值对储存数据.
      (2) IndexedDB 是事务模式的数据库
      (3) The IndexedDB API 基本上是异步的
          IndexedDB的API不通过return语句返回数据，而是需要你提供一个回调函数来接受数据
          当操作完成时，数据库会以DOM事件的方式通知你，同时事件的类型会告诉你这个操作是否成功完成。
          这个和XMLHttpRequest请求是类似的
      (4) IndexedDB数据库“请求”无处不在 
          每一个“请求”都包含onsuccess和onerror事件属性，同时你还对“事件”调用addEventListener()和
          removeEventListener()。
          “请求”还包括readyState，result和errorCode属性， 用来表示“请求”的状态。
          result属性尤其神奇，他可以根据“请求”生成的方式变成不同的东西，
          例如：IDBCursor实例、刚插入数据库的数值对应的键值（key）等。
      (5) IndexedDB是面向对象的
          indexedDB不是用二维表来表示集合的关系型数据库
          数据是一个JavaScript对象
      (6) indexedDB不使用结构化查询语言（SQL）
          它通过索引(index)所产生的指针(cursor)来完成查询操作，从而使你可以迭代遍历到结果集合 
      (7) IndexedDB遵循同源（same-origin）策略 
          IndexedDB的安全机制避免应用访问非同“源”的数据。
          例如:
           http://www.example.com/app/的应用或页面可以访问http://www.example.com/dir/的数据，
            因为他们同“源”。
            但是它们不能访问http://www.example.com:8080/dir/（不同端口）
            或https://www.example.com/dir/（不同协议）的数据，因为他们不同“源”。
3.IndexedDB也遵守同源策略。 
  因此当你在某个域名下操作储存数据的时候，
  你不能操作其他域名下的数据。

4.使用IndexedDB执行的操作是异步执行的，以免阻塞应用程序

5.名词解释
  
    (1） 数据库（database)
        
        [1] 名字(Name)。
            它标识了一个特定源中的数据库，
            并且在数据库的整个生命周期内保持不变。  
            此名字可以为任意字符串值（包括空字符串）。
        
        [2] 当前版本(version)。当一个数据库首次创建时，
            它的 version 为1，除非另外指定. 每个数据库在任意时刻只能有一个 version。
    
    (2) 对象仓库(object store)
        
         数据在数据库中存储的方式, 数据以键值对形式被对象仓库永久持有
         对象仓库中的的数据以 keys 升序排列。
         
         每一个对象仓库在同一个数据库中必须有**唯一**的名字
    (3) 数据库连接（database connection）
        通过打开数据库创建的操作。一个给定的数据库可以同时拥有多个连接。
    
    (4) 请求（request）
        在数据库上进行读写操作完成后的操作。每一个请求代表一个读或写操作
    
    (5) 索引（index）
    
6. indexedDB 使用基本模式
   
                      
     (1) 打开数据库
     (2) 在数据库中创建一个对象仓库（object store）
     (3) 启动一个事务，并发送一个请求来执行一些数据库操作，像增加或提取数据等。
     (4) 通过监听正确类型的 DOM 事件以等待操作完成。
     (5) 在操作结果上进行一些操作（可以在 request 对象中找到）

7. 判断浏览器是否支持indexedDB
      'indexedDB' in window
      window.indexedDB
8.事务：


    window.indexedDB = window.indexedDB || window.mozIndexedDB || window.webkitIndexedDB || window.msIndexedDB;
    // DON'T use "var indexedDB = ..." if you're not in a function.
    // Moreover, you may need references to some window.IDB* objects:
    window.IDBTransaction = window.IDBTransaction || window.webkitIDBTransaction || window.msIDBTransaction;
    window.IDBKeyRange = window.IDBKeyRange || window.webkitIDBKeyRange || window.msIDBKeyRange

9.打开数据库
   
    
    var request = window.indexedDB.open("MyTestDatabase");
 
open 请求不会立即打开数据库或者开始一个事务。 对 open() 函数的调用会
返回一个我们可以作为事件来处理的包含 result（成功的话）或者错误值的 IDBOpenDBRequest 对象
 
 open 函数的结果是一个 IDBDatabase 对象的实例。

该open方法接受第二个参数，就是数据库的版本号
 
如果数据库不存在，open 操作会创建该数据库，
然后 onupgradeneeded 事件被触发，你需要在该事件的处理函数中创建数据库模式

如果数据库已经存在，但你指定了一个更高的数据库版本，
会直接触发 onupgradeneeded 事件，允许你在处理函数中更新数据库模式

**版本号是一个 unsigned long long 数字，这意味着它可以是一个特别大的数字，但不能使用浮点数，否则它将会被转变成离它最近的整数，这可能导致 upgradeneeded 事件不会被触发。例如，不要使用 2.4 作为版本号。
var request = indexedDB.open("MyTestDatabase", 2.4); // 不要这么做，因为版本会被置为 2。