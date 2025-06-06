# 什么是HTTP长连接和短连接？在NGINX中如何管理这些连接？

HTTP长连接和短连接是关于客户端（例如浏览器）和服务器之间的连接持续时间的概念。它们在网络通信中起着重要作用，尤其是在Web服务器和客户端之间进行通信时。下面是它们的定义和区别，以及在NGINX中如何管理这些连接：



**HTTP长连接**：

+ 长连接也称为持久连接，指的是在单个TCP连接上可以发送多个HTTP请求和响应。
+ 客户端和服务器之间的TCP连接在一段时间内保持打开状态，允许多次请求和响应在同一连接上进行。
+ 这减少了连接的建立和关闭开销，提高了性能和响应时间，特别是在需要加载多个资源的网页中。



**HTTP短连接**：

+ 短连接是指每个HTTP请求和响应都使用单独的TCP连接进行。
+ 每次请求都需要建立新的TCP连接，然后在响应后关闭连接，这增加了连接管理的开销。



在NGINX中，可以通过以下方式管理HTTP长连接和短连接：

1.  **HTTP长连接的管理**： 
    - NGINX默认支持HTTP长连接，因为它在处理客户端请求时会自动保持连接打开，直到达到超时时间或请求完成。
    - 可以通过调整`keepalive_timeout`指令来控制NGINX在保持连接打开时等待的时间。例如：`keepalive_timeout 60s;` 将连接保持60秒。
    - 你还可以使用`keepalive_requests`指令设置在同一连接上允许的最大请求数。例如：`keepalive_requests 100;` 将允许在同一连接上处理100个请求。
2.  **HTTP短连接的管理**： 
    - NGINX默认情况下也支持短连接，因为每个HTTP请求和响应都使用单独的TCP连接。
    - 如果要强制关闭连接，可以使用`Connection: close`头部或在响应中包含它，这会告知客户端在响应后关闭连接。



总之，NGINX可以自动管理HTTP长连接和短连接，但你可以通过调整相关的配置指令来影响连接的行为，以满足你的需求。长连接适用于减少连接开销和提高性能，而短连接适用于需要在每个请求之间保持隔离的情况。



> 更新: 2023-09-04 19:31:35  
> 原文: <https://www.yuque.com/tulingzhouyu/db22bv/vfl7we33wwirep1n>