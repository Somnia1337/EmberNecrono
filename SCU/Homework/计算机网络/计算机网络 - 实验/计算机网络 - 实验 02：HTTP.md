@2024.03.19 | Week 04

### 1. The Basic HTTP GET/response interaction

> 1\. Is your browser running HTTP version 1.0 or 1.1? What version of HTTP is the server running?

![[Snipaste_240319_105613.png|500]]

它们都在运行 HTTP 1.1。

> 2\. What languages (if any) does your browser indicate that it can accept to the server?

![[Snipaste_240319_105832.png|500]]

> 3\. What is the IP address of your computer? Of the gaia.cs.umass.edu server?

![[Snipaste_240319_110056.png|500]]

- 我的 IP：`192.168.110.198`。
- 服务器的 IP：`128.119.245.12`。

> 4\. What is the status code returned from the server to your browser?

![[Snipaste_240319_110207.png|500]]

服务器返回状态码 `200`。

> 5\. When was the HTML file that you are retrieving last modified at the server?

![[Snipaste_240319_110326.png|500]]

最后修改时间：`Mon, 18 Mar 2024 05:59:01 GMT`。

> 6\. How many bytes of content are being returned to your browser?

![[Snipaste_240319_110459.png|500]]

内容长度：`128` Bytes。

> 7\. By inspecting the raw data in the packet content window, do you see any headers within the data that are not displayed in the packet-listing window? If so, name one.

![[Snipaste_240319_110918.png|500]]

没有找到未列出的 header。

### 2. The HTTP CONDITIONAL GET/response interaction

> 8\. Inspect the contents of the first HTTP GET request from your browser to the server. Do you see an “IF-MODIFIED-SINCE” line in the HTTP GET?

![[Snipaste_240319_111842.png|500]]

没有看到 `If-Modified-Since`。

> 9\. Inspect the contents of the server response. Did the server explicitly return the contents of the file? How can you tell?

![[Snipaste_240319_111949.png|500]]

是的，服务器显式返回了文件内容。因为返回的状态码为 `200` 表示 OK，且可以看到实体体中的文件内容。

> 10\. Now inspect the contents of the second HTTP GET request from your browser to the server. Do you see an “IF-MODIFIED-SINCE:” line in the HTTP GET? If so, what information follows the “IF-MODIFIED-SINCE:” header?

![[Snipaste_240319_111417.png|500]]

可以看到 `If-Modified-Since`。

> 11\. What is the HTTP status code and phrase returned from the server in response to this second HTTP GET? Did the server explicitly return the contents of the file? Explain.

![[Snipaste_240319_111612.png|500]]

没有显式返回内容，而是返回了 `304 Not Modified`，且可以看到在 HTTP 报文之后没有实体体内容。这是因为发送了条件 GET 请求，收到“没有修改过”的返回信息，因此直接展示了缓存中的内容。

### 3. Retrieving Long Documents

> 12\. How many HTTP GET request messages did your browser send? Which packet number in the trace contains the GET message for the Bill of Rights?

![[Snipaste_240319_112402.png|500]]

发送了 4 个 GET 请求。

![[Snipaste_240319_112531.png|500]]

图中所示的请求得到了返回报文“THE BILL OF RIGHTS”。

> 13\. Which packet number in the trace contains the status code and phrase associated with the response to the HTTP GET request?

![[Snipaste_240319_112657.png|500]]

如图所示的包返回了状态码 `200`，表示 OK，是对 GET 请求的响应。

> 14\. What is the status code and phrase in the response?

如上图所示，状态码为 `200`，状态为 `OK`。

> 15\. How many data-containing TCP segments were needed to carry the single HTTP response and the text of the Bill of Rights?

![[Snipaste_240319_113232.png|450]]

使用了 4 个包含数据的 TCP 报文段。

### 4. HTML Documents with Embedded Objects

> 16\. How many HTTP GET request messages did your browser send? To which Internet addresses were these GET requests sent?

![[Snipaste_240319_113507.png|500]]

共 3 个 GET 请求，其中 2 个发往 `128.119.245.12`，还有 1 个发往 `178.79.137.164`。

> 17\. Can you tell whether your browser downloaded the two images serially, or whether they were downloaded from the two web sites in parallel? Explain.

![[Snipaste_240319_113635.png|500]]

根据 GET 的发送时间，似乎是线性顺序，而不是并行顺序。

### 5. HTTP Authentication

> 18\. What is the server’s response (status code and phrase) in response to the initial HTTP GET message from your browser?

![[Snipaste_240319_113938.png|500]]

第一个 GET 请求得到的响应为 `401 Unauthorized`。

> 19\. When your browser’s sends the HTTP GET message for the second time, what new field is included in the HTTP GET message?

![[Snipaste_240319_114129.png|450]]

第二个 GET 请求包含了新的字段 `Authorization`，其内容用 Base64 编码。