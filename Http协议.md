### Http协议

#### HTTP协议及特点🌼

**Http协议**(Hyper Text Transfer Protocol) -- **超文本传输协议**，用于web服务器和本地浏览器之间的传输协议

- Http**跑在TCP/IP协议之上，支持B/S和C/S架构**
- 传输数据类型
  - **HTML(超文本标记语言)文件**
  - 图片、视频片段
  - 查询结果

- Http主要特点
  - 简单快速：Http协议简单，使得Http服务器的程序规模小
    - 客户向服务器请求时，**只需传送请求方法和路径**
    - 常用方法有GET、HEAD、POST
  - 灵活：Http**允许传输任意类型的数据对象**，当前**传输类型由Content-Type字段指明**
  - **无连接**：每次链接只处理一个请求，服务器处理完请求并收到客户端应答后，立即断开连接
  - **无状态**：Http对事物的处理没有记忆状态的能力，如果服务器后续处理需要之前的信息，则客户端需要重传该信息



#### URL组成🌼

Http与URL

- Http使用**统一资源标识符**(URI Uniform Resource Identifiers)来建立连接并传输数据
- **URL(统一资源定位符)**是一种特殊的URI，在互联网上**用来标识某处资源的地址**

```http
http://www.aspxfans.com:8080/news/index.asp?boardID=5&ID=24618&page=1#name
```

- URI是抽象的标准，URL是URI的一种具体实现
- URL组成
  - **协议部分**`http://`：表示网页使用http协议，Internet中可以使用多种协议，如HTTP、FTP、SSH
  - **域名部分**`www.aspxfans.com`：域名或IP均可使用
  - **端口部分**`:8080`：非必须，省略时**默认80端口**
  - **虚拟目录部分**`/news/`：非必须，用于向http服务器标识请求资源的虚拟位置
  - **文件名部分**`index.asp`
  - **参数部分**`boardID=5&ID=24618&page=1`：位于**?#**之间，用**&**分隔，以kv形式传递参数
  - **锚部分**`#name`：



#### Request格式🌼

**浏览器将用户输入的URL转化为Http请求**，请求方法为GET

Request格式 -- **`请求行` + `请求头部` + `空行` + `请求体`**

- **请求行**：说明**请求类型**`GET`  **请求资源**`/562f25980001b1b106000338.jpg`  **Http版本**`HTTP/1.1`

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220530114215185.png" alt="image-20220530114215185" style="zoom: 33%;" />

- **请求头部**：说明浏览器使用的附加信息，kv形式（主机域名 代理 接受文件类型...）
- **空行**
- **请求体**：本例为GET请求，故为空

Charlse抓取的一个GET request

```
GET /562f25980001b1b106000338.jpg HTTP/1.1
Host  img.mukewang.com
User-Agent  Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.106 Safari/537.36
Accept  image/webp,image/*,*/*;q=0.8
Referer  http://www.imooc.com/
Accept-Encoding  gzip, deflate, sdch
Accept-Language  zh-CN,zh;q=0.8


```

Charles抓取的一个POST request，注意`Content-Length`字段**指明请求体部分长度**

```
POST / HTTP1.1
Host:www.wrox.com
User-Agent:Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 2.0.50727; .NET CLR 3.0.04506.648; .NET CLR 3.5.21022)
Content-Type:application/x-www-form-urlencoded
Content-Length:40
Connection: Keep-Alive

name=Professional%20Ajax&publisher=Wiley
```



#### Response格式🌼

Response格式 -- **`状态行` + `消息报头` + `空行` + `响应正文`**

- **状态行**：HTTP**协议版本号**`HTTP/1.1`  **状态码**`200`  **状态消息**`OK`

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220530114544672.png" alt="image-20220530114544672" style="zoom: 33%;" />

- **消息报头**：说明客户端要使用的一些附加消息
- **空行**
- **消息正文**：服务器返回给客户端的请求资源，本例中为html文本

Charlse抓取的一个response

```
HTTP/1.1 200 OK
Date: Fri, 22 May 2009 06:07:21 GMT
Content-Type: text/html; charset=UTF-8

<html>
	<head></head>
	<body>
	<!--body goes here-->
	</body>
</html>
```

 

#### HTTP状态码🌼

- 1xx：指示信息，表示请求已接收，继续处理
  - **`100 Continue`**

- 2xx：成功，表示请求已被成功接收、理解、接受
  - **`200 OK`**  客户端**请求成功**
- 3xx：**重定向**，要完成请求必须进行更进一步的操作
- 4xx：客户端错误，请求有语法错误或请求无法实现
  - **`400 Bad Request`**  客户端**请求语法有误**，不能被服务器理解
  - **`403 Forbidden`**  服务器收到请求，但是拒绝服务
  - **`404 Not Found`**  请求资源不存在（路由指定的资源）
- 5xx：**服务器端错误**，服务器未能实现合法的请求



#### HTTP请求类型🌼

**HTTP1.0定义了GET POST HEAD三种方法**

**HTTP1.1新增五种方法**OPTIONS PUT DELETE TRACE CONNECT

- **GET**： 请求指定的页面信息，并返回实体主体
- **POST**：向指定资源**提交数据进行处理请求**（例如提交表单或者上传文件）
  - 数据被包含在请求体中。POST请求可能会导致新的资源的建立和/或已有资源的修改
- **HEAD**：与GET请求类似，但Response中不含响应正文，**用于获取response头部**
  - 例如缓存服务器向源服务器查询缓存资源是否发生改变
- PUT 从客户端向服务器传送的数据取代指定的文档的内容
- DELETE  请求服务器删除指定的页面
- CONNECT HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器
- OPTIONS 允许客户端查看服务器的性能
- TRACE 回显服务器收到的请求，主要用于测试或诊断

在浏览器中输入网址发送的是GET请求



#### HTTP的典型工作过程🌼

Http采用**`req/resp`模型**（请求/响应），客户端发送请求，服务器给出响应

**在浏览器中键入URL**，回车后经历如下过程

- 浏览器向DNS服务器请求URL中域名对应的IP
- 根据IP:Port和服务器建立TCP连接（默认80端口）
- 浏览器发送请求消息Request，**该消息作为TCP三次握手的第3个报文数据发送给服务器**
- 服务器解析Request，生成响应消息Response，**把请求的资源（如html）发送给浏览器**
- Http服务器主动断开TCP连接（Http连接模式为`Connection: close`条件下）
- 浏览器**解析html内容并在页面上展示**



#### GET vs POST🌼

|                                                   |                             GET                              |                   POST                    |
| :-----------------------------------------------: | :----------------------------------------------------------: | :---------------------------------------: |
|                   **请求参数**                    |                   位于Requst的**请求行中**                   |         位于Request的**请求体中**         |
| **传输数据大小**<br />HTTP协议本身没有限制URL大小 | 特定浏览器和服务器对URL长度有限制，如IE对URL长度的限制是2083B<br />**由URL生成的GET请求**，其传输数据长度受URL长度限制 | **POST不由URL传值**，理论上数据长度不受限 |
|                    **安全性**                     |        url传值时，可在浏览器历史记录中查找到请求信息         |                  更安全                   |
|                 请求参数解析方式                  |                        Request.Query                         |               Request.Form                |



#### DNS的三级缓存🌼

第一级：浏览器缓存

第二级：本机hosts文件（**/etc/hosts**）

第三级：DNS服务器



#### HTTPS协议🌼

HTTPS在应用层与传输层之间**增加SSL安全套接字层**

**解决HTTP协议明文传输带来的安全问题**

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220530182309032.png" alt="image-20220530182309032" style="zoom:50%;" />

安全层协议：SSL/TLS

SSL：Secure Socket Layer，安全套接字层

TLS：Transport Layer Security 传输安全层



#### 加密算法🌼

**对称加密算法**：加密 解密**共用一个密钥**

- 对称加密的加解密效率较高
- 代表算法：AES RC4 DES

```
0001 1001 1001 1000	数据
1010 1001 0011 0101 密钥
1011 0000 1010 1101 xor对称加密数据
1010 1001 0011 0101 密钥
0001 1001 1001 1000 xor对称解密数据
```



**非对称加密算法**：拥有公钥 + 私钥

- 公钥加密的数据 → 私钥解密
- 私钥加密的数据 → 公钥解密
- 代表算法：DH RSA



#### TLS层采用混合加密方式🌼

**混合加密过程**

- 服务器拥有非对称加密的公钥A+私钥B，服务器将公钥A明文发送给浏览器
  - 服务器私钥B不进行网络网络传输
- 浏览器生成对称加密的密钥C，浏览器用公钥A给密钥C加密，发送给服务器
  - 浏览器密钥C即使被截获，第三方也无法解析
- 服务器用私钥B解密得到密钥C
- 服务器和浏览器之间用密钥C**进行对称加密通信**

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/%E6%97%A0%E6%A0%87%E9%A2%98-16539044499801.png" alt="无标题" style="zoom: 67%;" />



