1.会话Storage 和 本地Storage
  sessionStorage:会话storage,关闭浏览器会话storage丢失。浏览器不关闭会一直存在
  localStorage:浏览器关闭在打开同样会存在，直到清空浏览器缓存

2.Storage
  (1) 同源策略：只能访问同域名下的（域名完全系统），子域名不能访问
      sessionStorage : 由网站打开的同域名下的页面，在新标签页展示可以获取到sessionStorage
      但是手动打开一个标签，输入同域名网站，新标签页获取不到sessionStorage
  (2) Storage 只能存储字符串类型，如果要存储对象（数组）或JSON，只能先转成字符串在存储。
  (3) 大小限制：5M（不同浏览器会有不同）
3.使用：
  Storage 可以看成一个特殊的对象，他是以key/value 形式存储的。
  （1）取值：
        Storage.getItem(key)
        Storage.key
  （2）赋值：
        Storage.key=value;
        Storage.setItem(key,value)
  (3) 移除
        Storage.removeItem(key)
        delete Storage.key
  (4) length 获取长度
        Storage.length
 （5）clear():清空
        Storage.clear()