@2024.04.02 | Week 06

### 1. `nslookup`

> 1\. Run nslookup to obtain the IP address of a Web server in Asia. What is the IP address of that server?

![[Snipaste_240402_103933.png|450]]

> 2\. Run nslookup to determine the authoritative DNS servers for a university in Europe.

![[Snipaste_240402_104632.png|450]]

> 3\. Run nslookup so that one of the DNS servers obtained in Question 2 is queried for the mail servers for Yahoo! mail. What is its IP address?

![[Snipaste_240402_104917.png|450]]

### 3. Tracing DNS with Wireshark

#### `http://www.ietf.org`

> 4\. Locate the DNS query and response messages. Are they sent over UDP or TCP?

![[Snipaste_240402_105711.png|450]]

UDP。

> 5\. What is the destination port for the DNS query message? What is the source port of DNS response message?

![[Snipaste_240402_105913.png|450]]

![[Snipaste_240402_105947.png|450]]

都是 `53` 端口。

> 6\. To what IP address is the DNS query message sent? Use ipconfig to determine the IP address of your local DNS server. Are these two IP addresses the same?

![[Snipaste_240402_110029.png|600]]

![[Snipaste_240402_110154.png|450]]

都是 `192.168.102.201`。

> 7\. Examine the DNS query message. What “Type” of DNS query is it? Does the query message contain any “answers”?

![[Snipaste_240402_110257.png|450]]

类型为 `A`，不包含答案。

> 8\. Examine the DNS response message. How many “answers” are provided? What do each of these answers contain?

![[Snipaste_240402_110431.png|450]]

包含了一个回答，其中有规范主机名。

> 9\. Consider the subsequent TCP SYN packet sent by your host. Does the destination IP address of the SYN packet correspond to any of the IP addresses provided in the DNS response message?

![[Snipaste_240402_111154.png|450]]

是的，TCP `SYN` 报文的目的地址与此前的回答是对应的。

> 10\. This web page contains images. Before retrieving each image, does your host issue new DNS queries?

![[Snipaste_240402_111247.png|450]]

此后没有再次进行 DNS 查询。

#### `nslookup www.mit.edu`

> 11\. What is the destination port for the DNS query message? What is the source port of DNS response message?

![[Snipaste_240402_111519.png|450]]

DNS 查询的目标端口为 `53`。

![[Snipaste_240402_111605.png|450]]

DNS 回答的源端口为 `53`。

> 12\. To what IP address is the DNS query message sent? Is this the IP address of your default local DNS server?

![[Snipaste_240402_111652.png|450]]

![[Snipaste_240402_111717.png|450]]

它们都是 `192.168.102.201`。

> 13\. Examine the DNS query message. What “Type” of DNS query is it? Does the query message contain any “answers”?

![[Snipaste_240402_111831.png|450]]

类型为 `A`，不包含任何答案。

> 14\. Examine the DNS response message. How many “answers” are provided? What do each of these answers contain?

![[Snipaste_240402_111919.png|450]]

包含了一个回答，其中有规范主机名。

> 15\. Provide a screenshot.

上文中每个问题的回答都已经含有截图。

#### `nslookup –type=NS mit.edu`

> 16\. To what IP address is the DNS query message sent? Is this the IP address of your default local DNS server?

![[Snipaste_240402_113019.png|450]]

仍为本地 DNS 服务器 `192.168.102.201`。

> 17\. Examine the DNS query message. What “Type” of DNS query is it? Does the query message contain any “answers”?

![[Snipaste_240402_113041.png|450]]

类型为 `A`，不包含任何答案。

> 18\. Examine the DNS response message. What MIT nameservers does the response message provide? Does this response message also provide the IP addresses of the MIT namesers?

![[Snipaste_240402_113146.png|450]]

包含。

> 19\. Provide a screenshot.

上文中每个问题的回答都已经含有截图。

#### `nslookup www.aiit.or.kr bitsy.mit.edu`

> 20\. To what IP address is the DNS query message sent? Is this the IP address of your default local DNS server? If not, what does the IP address correspond to?

![[Snipaste_240402_113812.png|450]]

不是本地 DNS 服务器，而是指令中指定的、经查询得到的服务器地址。

> 21\. Examine the DNS query message. What “Type” of DNS query is it? Does the query message contain any “answers”?

![[Snipaste_240402_113918.png|450]]

类型为 `A`，不包含任何答案。

> 22\. Examine the DNS response message. How many “answers” are provided? What does each of these answers contain?

![[Snipaste_240402_111919.png|450]]

包含了一个回答，其中有规范主机名。

> 23\. Provide a screenshot.

上文中每个问题的回答都已经含有截图。