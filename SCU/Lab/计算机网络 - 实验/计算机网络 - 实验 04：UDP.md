@2024.04.16 | Week 08

> 1\. Select one UDP packet from your trace. From this packet, determine how many fields there are in the UDP header. (You shouldn’t look in the textbook! Answer these questions directly from what you observe in the packet trace.) Name these fields.

![[Snipaste_240416_113315.png|500]]

从上图中可以看到 4 个首部：

- `Source Port`
- `Destination Port`
- `Length`
- `Checksum`

> 2\. By consulting the displayed information in Wireshark’s packet content field for this packet, determine the length (in bytes) of each of the UDP header fields. 

![[Snipaste_240416_113831.png|500]]

4 个字段的大小均为 $2$ 字节，因此首部大小为 $4 \times 2 = 8$ 字节。

> 3\. The value in the Length field is the length of what? (You can consult the text for this answer). Verify your claim with your captured UDP packet. 

`Length` 指示了“首部 + 数据”的字节数。

![[Snipaste_240416_114046.png|500]]

验证：$4 \times 2 + 53 = 61$ 字节。

> 4\. What is the maximum number of bytes that can be included in a UDP payload? (Hint: the answer to this question can be determined by your answer to 2. above) 

总字节数为 $61$，减去 4 个首部的大小 $4 \times 2 = 8$，得到最大大小为 $53$ 字节。

> 5\. What is the largest possible source port number? (Hint: see the hint in 4.) 

2 个 `Port` 首部大小均为 $2$ 字节，因此最大可为 `FF FF`，即 `65535`。

> 6\. What is the protocol number for UDP? Give your answer in both hexadecimal and decimal notation. To answer this question, you’ll need to look into the Protocol field of the IP datagram containing this UDP segment (see Figure 4.13 in the text, and the discussion of IP header fields). 

UDP 的协议号为 `17` / `0x0011`。

> 7\. Examine a pair of UDP packets in which your host sends the first UDP packet and the second UDP packet is a reply to this first UDP packet. (Hint: for a second packet to be sent in response to a first packet, the sender of the first packet should be the destination of the second packet). Describe the relationship between the port numbers in the two packets.

![[Snipaste_240416_115245.png|600]]

如图，两端口字段互为相反。