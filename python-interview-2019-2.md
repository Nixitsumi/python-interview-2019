## Python开发工程师笔试题

> 答题要求：将该项目从[地址1](<https://github.com/jackfrued/python-interview-2019>)或[地址2](<https://gitee.com/jackfrued/python-interview-2019>)fork到自己的[github]()或[gitee]()仓库并填写答案，完成之后将自己的仓库（public）地址发给面试官即可，答题时间120分钟。

1. 下面的Python代码会输出什么。

   ```Python
   print([(x, y) for x, y in zip('abcd', (1, 2, 3, 4, 5))])
   print({x: f'item{x ** 2}' for x in range(5) if x % 2})
   print(len({x for x in 'hello world' if x not in 'abcdefg'}))
   ```

   答案：

   ```
   [('a',1),('b',2),('c',3),('d',4)]
   {1:'item1',3:'item9'}
   6
   ```

2. 下面的Python代码会输出什么。

   ```Python
   from functools import reduce
   
   items = [11, 12, 13, 14] 
   print(reduce(int.__mul__, map(lambda x: x // 2, filter(lambda x: x ** 2 > 150, items))))
   ```

   答案：

   ```
   42
   ```

3. 有一个通过网络获取数据的Python函数（可能会因为网络或其他原因出现异常），写一个装饰器让这个函数在出现异常时可以重新执行，但尝试重新执行的次数不得超过指定的最大次数。

   答案：

   ```Python
   from functools import wraps
   def outer(count=1):
      def inner(func):
         @wraps(func)
         def wrapper(*args, **kwargs):
            times = 0
            while times < count:
               try:
                  return func(*args, **kwargs)
               except:
                  continue
         return wrapper
      return inner
   ```

4. 下面的字典中保存了某些公司今日的股票代码及价格，用一句Python代码从中找出价格最高的股票对应的股票代码，用一句Python代码创建股票价格大于100的股票组成的新字典。

   > 说明：美股的股票代码是指英文字母代码，如：AAPL、GOOG。

   ```Python
   prices = {
       'AAPL': 191.88,
       'GOOG': 1186.96,
       'IBM': 149.24,
       'ORCL': 48.44,
       'ACN': 166.89,
       'FB': 208.09,
       'SYMC': 21.29
   }
   ```

   答案：

   ```Python
   from collections import Counter
   res = Counter(pricecs).most_common(1)
   new_dict = {key:value for key,value in prices.items() if value>100}
   ```

5. 用生成式实现矩阵的转置操作。例如，用`[[1, 2], [3, 4], [5, 6]`表示矩阵$\begin{bmatrix}1 & 2\\\\3 &4\\\\5 & 6\end{bmatrix}$，写一个生成式将其转换成`[[1, 3, 5], [2, 4, 6]]`即$\begin{bmatrix}1 & 3 & 5\\\\2 & 4 & 6\end{bmatrix}$。

   答案：

   ```Python
   res = [list(item) for item in list(zip(*[[1, 2], [3, 4], [5, 6]]))]
   ```

6. 写一个函数，传入的参数是一个列表（列表中的元素可能也是一个列表），返回该列表最大的嵌套深度，例如：

   > 参数：`[1, 2, 3]`
   >
   > 返回：`1`
   >
   > 参数：`[[1], [2, [3]]]`
   >
   > 返回：`3`

   答案：

   ```Python
   def max_depth(lst):
      count_list = []
      count = 0
      lst_str = str(lst)
      for char in lst_str:
         if char == '[':
            count += 1
            count_list.append(count)
         elif char == ']':
            count -= 1
      return max(count_list)
      
      def depth_of_list(items):
         if isinstance(items, list):
         max_depth = 1
         for item in items:
            curr_depth = depth_of_list(item)
            if curr_depth + 1 > max_depth:
               max_depth = curr_depth + 1
            return max_depth
         return 0
   ```

7. 写一个函数，实现将输入的长链接转换成短链接，每个长链接对应的短链接必须是独一无二的且每个长链接只应该对应到一个短链接，假设短链接统一以`http://t.cn/`开头。

   > 参数：`http://jackfrued.xyz/api/users/10001`
   >
   > 返回：`http://t.cn/E6MUth1`

   答案：

   ```Python
   seq_num = 10000000
   url_maps = {}


   def to_base62(num):
      chars = '0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz'
      result = []
      while num > 0:
         result.append(chars[num % 62])
         num //= 62
      return ''.join(reversed(result))

   def to_short(url):
      if url in url_maps:
         return url_maps[url]
      global seq_num
      seq_num += 1
      short_url = f'http://t.cn/{to_base62(seq_num)}'
      url_maps[url] = short_url
      return short_url

   while True:
      url = input('输入url(q退出):')
      if url == 'q':
         break
      print(to_short(url))
      print(url_maps)
   ```

8. 用5个线程，将1~100的整数累加到一个初始值为0的变量上，每次累加时将线程ID和本次累加后的结果打印出来。

    答案：

    ```Python
   from threading import Thread
   count = 0
   def func(nums):
      while True:
         try:
            num = next(nums)
            global count
            count += num
            print(count)
         except StopIteration:
            return
            
   nums = (i for i in range(1,101))
   t_list = [Thread(target=func, args=(nums,)) for _ in range(5)]
   for t in t_list:
      t.start()
   for t in t_list:
      t.join()
      
      
      
      from threading import get_ident, Thread, Lock
      from concurrent.futures import ThreadPoolExecutor
      result, i = 0, 1
      locker = Lock()
      def calc():
         global result, i
         while True:
            with locker:
               if i > 100:
                  break
               result, i = result + i, i + 1
               print(get_ident(), result)
      with ThreadPoolExecutor(max_workers=5) as pool:
      pool.submit(calc)

    ```

9. 请阐述Python是如何进行内存管理的。

    答案：

    ```
    在堆空间中数据对象会有一个引用计数的属性表示当前有多少变量指向它或者说被引用多少次，若引用计数为0，python自动销毁该对象并回收内存空间
    ```

10. 在MySQL数据库中有名为`tb_result`的表如下所示，请写出能查询出如下所示结果的SQL。

  `tb_result`表：

  | rq         | shengfu |
  | ---------- | ------- |
  | 2017-04-09 | 胜      |
  | 2017-04-09 | 胜      |
  | 2017-04-09 | 负      |
  | 2017-04-09 | 负      |
  | 2017-04-10 | 胜      |
  | 2017-04-10 | 负      |
  | 2017-04-10 | 负      |

  查询结果：

  | rq         | 胜   | 负   |
  | ---------- | ---- | ---- |
  | 2017-04-09 | 2    | 2    |
  | 2017-04-10 | 1    | 2    |

  答案：

  ```SQL
  
  ```

11. 列举出你知道的HTTP请求头选项并说明其作用。

    答案：

    ```
    accept：客户端可接收数据类型
    user-agent：浏览器标识
    referer：表示上一个链接的url
    host：指定访问主机名
    ```

12. 阐述JSON Web Token的工作原理和优点。

    答案：

    ```
    
    ```

13. 请阐述访问一个用Django或Flask开发的Web应用，从用户在浏览器中输入网址回车到浏览器收到Web页面的整个过程中，到底发生了哪些事情，越详细越好。

    答案：

    ```
    本地进行域名解析，或者通过dns服务器进行域名解析获取目标网址ip地址，建立可靠的tcp连接，http请求经过应用层、传输层、网络层、数据链路层封装，经由默认网关或指定网关向外传输，经由多跳路由，到达目标服务器的web服务器如nginx、apache，由web服务器处理网络请求并将请求分发至其反向代理映射的多台(可以通过uwsgi)web应用服务器，及django或者flask处，在django中，http请求会通过设置文件中指定的中间件并按顺序通过，首先通过的是所有中间件的proccess_request方法，接着会重新按顺序执行所有中间件的process_view方法，接着会走到路由分发的地方，进行路由匹配，再进入到对应的业务代码视图函数，在这里可能会通过模型层通过orm对数据库数据进行操作，并返回指定数据，或者调用模板层，返回html文件，返回时再按配置文件中的中间件顺序逆序执行所有的process_response方法，再对返回数据进行逐层封装，通过web服务器返回至客户端，传输过程应该是基于tcp协议可靠传输，若数据过多会进行ip分片，客户端收到数据对分片数据组装，逐层解包，获取html页面，在浏览器上渲染页面。
    ```

14. 请阐述HTTPS的工作原理，并说明该协议与HTTP之间的区别。

    答案：

    ```
    https协议在开始客户端与服务端会进行一系列的确认过程，包括确定通信加密套件，密钥协商过程使用非对称加密算法，数据通信过程使用对称加密算法，因为对称加密的加密解密过程比非对称加密方式的效率要高。https比起http多了一个ssl/tls协议层，用于加密通信，而http传输过程是明文传输。
    ```

15. 简述如何检查数据库是不是系统的性能瓶颈以及你在工作中是如何优化数据库操作性能的。

    答案：

    ```
    
    ```

16. 在Linux系统中，假设Nginx的访问日志位于`/var/log/nginx/access.log`，该文件的每一行代表一条访问记录，每一行都由若干列（以制表键分隔）构成，其中第1列记录了访问者的IP地址。请用一条命令找出最近的100000次访问中，访问频率最高的IP地址及访问次数。

    答案：

    ```Shell
    tail -n 100000 /var/log/nginx/access.log | awk '{print $1}' | sort | uniq -c | sort -nr | head -n 1 
    ```

