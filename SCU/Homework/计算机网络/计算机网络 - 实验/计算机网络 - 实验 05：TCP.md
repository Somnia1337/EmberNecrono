@2024.04.23 | Week 09

>1\. What is the IP address and TCP port number used by the client computer (source) that is transferring the file to `gaia.cs.umass.edu`? To answer this question, it's probably easiest to select an HTTP message and explore the details of the TCP packet used to carry this HTTP message, using the "details of the selected packet header window".

![[Snipaste_240423_174343.png|600]]

源 IP 地址为 `192.168.1.102`，端口号为 `1161`。

>2\. What is the IP address of `gaia.cs.umass.edu`? On what port number is it sending and receiving TCP segments for this connection?

![[Snipaste_240423_174526.png|600]]

目的地 IP 为 `128.119.245.12`，端口号为 `80`。

>3\. What is the IP address and TCP port number used by your client computer (source) to transfer the file to `gaia.cs.umass.edu`?

![[Snipaste_240423_174810.png|600]]

我本机发送文件时，IP 为 `10.134.112.241`，端口号为 `61704`。

>4\. What is the sequence number of the TCP *SYN* segment that is used to initiate the TCP connection between the client computer and `gaia.cs.umass.edu`? What is it in the segment that identifies the segment as a *SYN* segment?

![[Snipaste_240423_175013.png|600]]

`seq` 为 `0`，且 `Flags` 的 `Syn` 位为 `1`。

>5\. What is the sequence number of the *SYNACK* segment sent by `gaia.cs.umass.edu` to the client computer in reply to the *SYN*? What is the value of the `Acknowledgement` field in the *SYNACK* segment? How did `gaia.cs.umass.edu` determine that value? What is it in the segment that identifies the segment as a *SYNACK* segment?

![[Snipaste_240423_175323.png|600]]

`Seq` 为 `0`，`Acknowledgement` 为 `1`。

因为收到 `SYN` 报文，所以设置为 `1`。

`Ack` 为 `1`，且 `Syn` 和 `Acknowledgement` 位均为 `1`。

>6\. What is the sequence number of the TCP segment containing the HTTP `POST` command?

![[Snipaste_240423_175811.png|600]]

`Seq` 为 `164041`。

>7\. Consider the TCP segment containing the HTTP `POST` as the first segment in the TCP connection. What are the sequence numbers of the first six segments in the TCP connection (including the segment containing the HTTP `POST`)? At what time was each segment sent? When was the *ACK* for each segment received? Given the difference between when each TCP segment was sent, and when its acknowledgement was received, what is the *RTT* value for each of the six segments? What is the *EstimatedRTT* value (see Section 3.5.3, page 242 in text) after the receipt of each *ACK*? Assume that the value of the *EstimatedRTT* is equal to the measured *RTT* for the first segment, and then is computed using the *EstimatedRTT* equation on page 242 for all subsequent segments.

![[Snipaste_240423_180734.png|600]]

前 6 个报文的 `Seq` 见上图。

![[Snipaste_240423_180947.png|600]]

对应的 *ACK* 报文到达时间见上图。

![[Snipaste_240423_181524.png|500]]

软件自动生成的往返时间图示。

>8\. What is the length of each of the first six TCP segments?

![[Snipaste_240423_180734.png|600]]

见右侧 `Len` 列。

>9\. What is the minimum amount of available buffer space advertised at the received for the entire trace? Does the lack of receiver buffer space ever throttle the sender?

![[Snipaste_240423_181933.png|600]]

最小为 `1364`。

>10\. Are there any retransmitted segments in the trace file? What did you check for (in the trace) in order to answer this question?

![[Snipaste_240423_182203.png|500]]

没有重传的序列，因为由图可以看出序列号一直在递增。

>11\. How much data does the receiver typically acknowledge in an *ACK*? Can you identify cases where the receiver is *ACK*ing every other received segment (see Table 3.2 on page 250 in the text).

![[Snipaste_240423_182336.png|300]]

一次 *ACK* 确认大约 `1460` 字节的数据。

![[Snipaste_240423_182541.png|300]]

这里找到一处跳变，即为对前面 2 报文的累积确认。

>12\. What is the throughput (bytes transferred per unit time) for the TCP connection? Explain how you calculated this value.

![[Snipaste_240423_182818.png|600]]

第 1 个数据包的发送时间为 `0.041737`。

![[Snipaste_240423_182904.png|600]]

最后一个数据包的发送时间为 `5.201150`，最后确认的 `Seq` 为 `164091`。

总吞吐量为 $$\frac{164091 * 8}{5.201150 - 0.041737} \approx 254434\space {\rm bps}$$