### defer 和 async 的区别

`defer` 和 `async` 都是(外部脚本)异步加载, 如果没有这两个属性, 浏览器会立即加载并执行相应的脚本, 就不会等待后续加载的文档元素, 只要读取到就会开始加载和执行, 这样就阻塞了后续文档的加载.

`async`:
  1. 异步加载.
  2. 后续文档的加载和执行与js脚本的加载和执行是并行进行的.
  3. 适用于第三方的脚本.
  
`defer`:
  1. 异步加载
  2. 加载后续文档的过程和js脚本的加载(此时仅加载不执行)是并行进行的(异步)，js脚本需要等到文档所有元素解析完成之后，DOMContentLoaded事件触发执行之前, 才执行。
  3. 适用于与 `dom` 相关的脚本.

下图可以只管的看出三者之间的区别: 
![script标签的加载与执行](https://cdn.nlark.com/yuque/0/2020/png/1500604/1603547262709-5029c4e4-42f5-4fd4-bcbb-c0e0e3a40f5a.png)

绿色代表html解析.

蓝色代表js脚本网络加载时间.

红色代表js脚本执行时间