```text
题库: 2 19 21 23 24 25 61 82 83 86 92 116 117 142 143 146 148 203 206 234 237 328 369 445 460 725 876 1171 1669 2095 2487 2807
LCR: 26
LCR: 24 123 140 141 142
面试: 02.05 02.07
```

#### [2. 两数相加](https://leetcode.cn/problems/add-two-numbers/)

@2023.09.18

🟡

```java
/**
 * 模拟
 * Somnia1337
 */
public ListNode addTwoNumbers(ListNode l1, ListNode l2)
{
	int c = 0;
	ListNode ret = new ListNode(), it = ret;
	while (l1 != null || l2 != null)
	{
		int v1 = l1 != null ? l1.val : 0, v2 = l2 != null ? l2.val : 0, s = v1 + v2 + c;
		c = s / 10;
		it.next = new ListNode(s % 10);
		it = it.next;
		l1 = l1 != null ? l1.next : null;
		l2 = l2 != null ? l2.next : null;
	}
	if (c > 0) it.next = new ListNode(c);
	return ret.next;
}
```

#### [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

@2023.09.13

🟡

```java
/**
 * 双指针
 * 代码随想录
 */
public ListNode removeNthFromEnd(ListNode head, int n)
{
	ListNode dummyNode = new ListNode(0);
	dummyNode.next = head;
	ListNode fastIndex = dummyNode;
	ListNode slowIndex = dummyNode;
	
	for (int i = 0; i < n; i++)
	{
		fastIndex = fastIndex.next;
	}
	while (fastIndex.next != null)
	{
		fastIndex = fastIndex.next;
		slowIndex = slowIndex.next;
	}
	
	slowIndex.next = slowIndex.next.next;
	return dummyNode.next;
}
```

#### [21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)

@2023.09.18

🟢

1. 迭代

```java
/**
 * 迭代
 * Somnia1337
 */
public ListNode mergeTwoLists(ListNode list1, ListNode list2)
{
	ListNode dummy = new ListNode(), it = dummy;
	while (list1 != null || list2 != null)
	{
		if (list2 == null || list1 != null && list1.val < list2.val)
		{
			it.next = new ListNode(list1.val);
			list1 = list1.next;
		}
		else
		{
			it.next = new ListNode(list2.val);
			list2 = list2.next;
		}
		it = it.next;
	}
	return dummy.next;
}
```

2. 递归

```java
/**
 * 递归
 * 力扣官方题解
 */
public ListNode mergeTwoLists(ListNode list1, ListNode list2)
{
	if (list1 == null) return list2;
	else if (list2 == null) return list1;
	else if (list1.val < list2.val)
	{
		list1.next = mergeTwoLists(list1.next, list2);
		return list1;
	}
	else
	{
		list2.next = mergeTwoLists(list1, list2.next);
		return list2;
	}
}
```

#### [23. 合并 K 个升序链表](https://leetcode.cn/problems/merge-k-sorted-lists/)

@2023.10.29

🔴

```java
/**
 * 迭代
 * Somnia1337
 */
public ListNode mergeKLists(ListNode[] lists)
{
	ListNode it = new ListNode(0), dummy = new ListNode(0, it);
	while (true)
	{
		int min = 10000, minIdx = -1;
		for (int i = 0; i < lists.length; i++)
		{
			if (lists[i] == null) continue;
			if (lists[i].val < min)
			{
				min = lists[i].val;
				minIdx = i;
			}
		}
		if (minIdx == -1) break;
		it.next = lists[minIdx];
		lists[minIdx] = lists[minIdx].next;
		it = it.next;
	}
	return dummy.next.next;
}
```

```java
/**
 * 堆
 * 灵茶山艾府
 */
public ListNode mergeKLists(ListNode[] lists)
{
	Queue<ListNode> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a.val));
	for (ListNode head : lists)
	{
		if (head != null) pq.offer(head);
	}
	ListNode dummy = new ListNode();
	ListNode cur = dummy;
	while (!pq.isEmpty())
	{
		ListNode node = pq.poll();
		if (node.next != null) pq.offer(node.next);
		cur.next = node;
		cur = cur.next;
	}
	return dummy.next;
}
```

```java
/**
 * 分治
 * Sweetiee 🍬
 */
public ListNode mergeKLists(ListNode[] lists)
{
	if (lists.length == 0) return null;
	return merge(lists, 0, lists.length - 1);
}

private ListNode merge(ListNode[] lists, int low, int high)
{
	if (low == high) return lists[low];
	int mid = low + (high - low) / 2;
	ListNode l1 = merge(lists, low, mid);
	ListNode l2 = merge(lists, mid + 1, high);
	return merge2Lists(l1, l2);
}

private ListNode merge2Lists(ListNode l1, ListNode l2)
{
	if (l1 == null) return l2;
	if (l2 == null) return l1;
	if (l1.val < l2.val)
	{
		l1.next = merge2Lists(l1.next, l2);
		return l1;
	}
	else
	{
		l2.next = merge2Lists(l1, l2.next);
		return l2;
	}
}
```

#### [24. 两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/)

@2023.09.13

🟡

1. 迭代

```java
/**
 * 迭代
 * Somnia1337
 */
public ListNode swapPairs(ListNode head)
{
	ListNode dummy = new ListNode(0, head);
	ListNode former = dummy;
	while (head != null)
	{
		ListNode next = head.next;
		if (next == null) break;
		head.next = next.next;
		former.next = next;
		next.next = head;
		former = head;
		head = head.next.next;
	}
	return dummy.next;
}
```

2. 递归

```java
/**
 * 递归
 * 力扣官方题解
 */
public ListNode swapPairs(ListNode head)
{
	if (head == null || head.next == null)
	{
		return head;
	}
	ListNode newHead = head.next;
	head.next = swapPairs(newHead.next);
	newHead.next = head;
	return newHead;
}
```

#### [25. K 个一组翻转链表](https://leetcode.cn/problems/reverse-nodes-in-k-group/)

@2023.10.29

🔴

```java
/**
 * 递归
 * reedfan
 */
public ListNode reverseKGroup(ListNode head, int k)
{
	if (head == null || head.next == null) return head;
	ListNode tail = head;
	for (int i = 0; i < k; i++)
	{
		if (tail == null) return head;
		tail = tail.next;
	}
	ListNode newHead = reverse(head, tail);
	head.next = reverseKGroup(tail, k);
	return newHead;
}

private ListNode reverse(ListNode head, ListNode tail)
{
	ListNode pre = null;
	ListNode next;
	while (head != tail)
	{
		next = head.next;
		head.next = pre;
		pre = head;
		head = next;
	}
	return pre;
}
```

#### [61. 旋转链表](https://leetcode.cn/problems/rotate-list/)

@2023.09.29

🟡

1. 队列

```java
/**
 * 队列
 * Somnia1337
 */
public ListNode rotateRight(ListNode head, int k)
{
	if (head == null) return null;
	ListNode dummyHead = new ListNode(0, head);
	List<ListNode> nodes = new ArrayList<>();
	ListNode it = head;
	while (it != null)
	{
		nodes.add(it);
		it = it.next;
	}
	int len = nodes.size();
	k %= len;
	it = dummyHead;
	nodes.get(len - k - 1).next = null;
	for (int i = len - k; i < len * 2 - k; i++)
	{
		it.next = nodes.get(i % len);
		it = it.next;
	}
	return dummyHead.next;
}
```

2. 成环

```java
/**
 * 成环
 * 力扣官方题解
 */
public ListNode rotateRight(ListNode head, int k)
{
	if (k == 0 || head == null || head.next == null) return head;
	int n = 1;
	ListNode it = head;
	while (it.next != null)
	{
		it = it.next;
		n++;
	}
	int add = n - k % n;
	if (add == n) return head;
	it.next = head;
	while (add > 0)
	{
		it = it.next;
		add--;
	}
	ListNode ret = it.next;
	it.next = null;
	return ret;
}
```

#### [82. 删除排序链表中的重复元素 II](https://leetcode.cn/problems/remove-duplicates-from-sorted-list-ii/)

@2023.10.01

🟡

1. 迭代

```java
/**
 * 迭代
 * 宫水三叶
 */
public ListNode deleteDuplicates(ListNode head) {
	ListNode dummy = new ListNode(0, head), tail = dummy, it = head;
	while (it != null) {
		if (it.next == null || it.val != it.next.val) {
			tail.next = it;
			tail = it;
		}
		while (it.next != null && it.val == it.next.val) it = it.next;
		it = it.next;
	}
	tail.next = null;
	return dummy.next;
}
```

2. 递归

```java
/**
 * 递归
 * reedfan
 */
public ListNode deleteDuplicates(ListNode head) {
	if (head == null) return null;
	else if (head.next != null && head.val == head.next.val) {
		while (head.next != null && head.val == head.next.val) head = head.next;
		return deleteDuplicates(head.next);
	}
	else head.next = deleteDuplicates(head.next);
	return head;
}
```

#### [83. 删除排序链表中的重复元素](https://leetcode.cn/problems/remove-duplicates-from-sorted-list/)

@2023.10.01

🟢

1. 迭代

```java
/**
 * 迭代
 * Somnia1337
 */
public ListNode deleteDuplicates(ListNode head) {
	ListNode dummy = new ListNode(0, head), it = head, prev = dummy;
	while (it != null) {
		while (it.next != null && it.next.val == it.val) it = it.next;
		prev.next = it;
		it = it.next;
		prev = prev.next;
	}
	return dummy.next;
}
```

当 `it.val == it.next.val` 时，将 `it.next` 赋为 `it.next.next`，由此跳至 `it.val != it.next.val` 处。

```java
/**
 * 迭代
 * 力扣官方题解
 */
public ListNode deleteDuplicates(ListNode head) {
	if (head == null) return null;
	ListNode it = head;
	while (it.next != null) {
		if (it.val == it.next.val) it.next = it.next.next;
		else it = it.next;
	}
	return head;
}
```

2. 递归

```java
/**
 * 递归
 * 程序员小熊
 */
public ListNode deleteDuplicates(ListNode head) {
	if (head == null || head.next == null) return head;
	head.next = deleteDuplicates(head.next);
	return head.val == head.next.val ? head.next : head;
}
```

#### [86. 分隔链表](https://leetcode.cn/problems/partition-list/)

@2023.10.25

🟡

```java
/**
 * 模拟
 * Angus-Liu
 */
public ListNode partition(ListNode head, int x)
{
	ListNode dummy1 = new ListNode(0);
	ListNode dummy2 = new ListNode(0);
	ListNode node1 = dummy1;
	ListNode node2 = dummy2;
	while (head != null)
	{
		if (head.val < x)
		{
			node1.next = head;
			node1 = node1.next;
		}
		else
		{
			node2.next = head;
			node2 = node2.next;
		}
		head = head.next;
	}
	node1.next = dummy2.next;
	node2.next = null;
	return dummy1.next;
}
```

#### [92. 反转链表 II](https://leetcode.cn/problems/reverse-linked-list-ii/)

@2023.09.21

🟡

```java
/**
 * 栈
 * Somnia1337
 */
public ListNode reverseBetween(ListNode head, int left, int right)
{
	if (left == right) return head;
	ListNode dummyHead = new ListNode(0, head);
	ListNode it = dummyHead, pre = null, aft = null;
	Stack<ListNode> pres = new Stack<>();
	int count = 0;
	while (true)
	{
		count++;
		if (count > right) break;
		if (count == left)
		{
			pre = it;
		}
		else if (count == right)
		{
			aft = it.next.next;
		}
		it = it.next;
		if (count >= left)
		{
			pres.push(it);
		}
	}
	it = pre;
	pre.next = pres.peek();
	while (!pres.isEmpty())
	{
		it.next = pres.pop();
		it = it.next;
	}
	it.next = aft;
	return dummyHead.next;
}
```

```java
/**
 * 
 * 灵茶山艾府
 */
public ListNode reverseBetween(ListNode head, int left, int right)
{
	ListNode dummy = new ListNode(0, head), p0 = dummy;
	for (int i = 0; i < left - 1; ++i)
	{
		p0 = p0.next;
	}
	ListNode pre = null, cur = p0.next;
	for (int i = 0; i < right - left + 1; ++i)
	{
		ListNode nxt = cur.next;
		cur.next = pre;
		pre = cur;
		cur = nxt;
	}
	p0.next.next = cur;
	p0.next = pre;
	return dummy.next;
}
```

#### [116. 填充每个节点的下一个右侧节点指针](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/)

@2023.09.20

🟡

```java
/**
 * 迭代
 * Somnia1337
 */
public Node connect(Node root)
{
	if (root == null) return root;
	Queue<Node> queue = new LinkedList<>();
	queue.offer(root);
	while (!queue.isEmpty())
	{
		int size = queue.size();
		while (size > 0)
		{
			Node current = queue.poll();
			if (size > 1 && !queue.isEmpty() && queue.peek() != null) current.next = queue.peek();
			if (current.left != null) queue.offer(current.left);
			if (current.right != null) queue.offer(current.right);
			size--;
		}
	}
	return root;
}
```

#### [117. 填充每个节点的下一个右侧节点指针 II](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node-ii/)

@2023.09.20

🟡

1. 迭代

[116. 填充每个节点的下一个右侧节点指针](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/)的代码，一个字都不用改…

```java
/**
 * 迭代
 * Somnia1337
 */
public Node connect(Node root)
{
	if (root == null) return root;
	Queue<Node> queue = new LinkedList<>();
	queue.offer(root);
	while (!queue.isEmpty())
	{
		int size = queue.size();
		while (size > 0)
		{
			Node current = queue.poll();
			if (size > 1 && !queue.isEmpty() && queue.peek() != null) current.next = queue.peek();
			if (current.left != null) queue.offer(current.left);
			if (current.right != null) queue.offer(current.right);
			size--;
		}
	}
	return root;
}
```

2. 递归

> 想请教一下，connect 函数中，最后一个 if 为什么不是 `if (root.left == null && root.right != null)` 呢？我试了这段代码，发现少了 node7，但是不明白为什么。谢谢！
> 
> 无论左子树是否为空，我们都需要找到右子树的next结点
> 
> 为什么要先递归右树呢？
> 
> 如果先递归左子树，会出现左边无法与右边相连的case: \[2,1,3,0,7,9,1,2,null,1,0,null,null,8,8,null,null,null,null,7]

```java
/**
 * 递归
 * Cyan
 */
public Node connect(Node root)
{
	if (root == null) return root;
	if (root.left != null && root.right != null) root.left.next = root.right;
	else if (root.left != null && root.right == null) root.left.next = getNext(root.next);
	if (root.right != null) root.right.next = getNext(root.next);
	connect(root.right);
	connect(root.left);
	return root;
}

public Node getNext(Node root)
{
	if (root == null) return null;
	if (root.left != null) return root.left;
	if (root.right != null) return root.right;
	if (root.next != null) return getNext(root.next);
	return null;
}
```

#### [141. 环形链表](https://leetcode.cn/problems/linked-list-cycle/)

@2023.10.13

🟢

```java
/**
 * 双指针
 * Somnia1337
 */
public boolean hasCycle(ListNode head)
{
	if (head == null) return false;
	ListNode fast = head, slow = head;
	while (fast != null && fast.next != null)
	{
		fast = fast.next.next;
		slow = slow.next;
		if (fast == slow) return true;
	}
	return false;
}
```

```java
/**
 * 双指针
 * 力扣官方题解
 */
public boolean hasCycle(ListNode head)
{
	if (head == null) return false;
	ListNode fast = head.next, slow = head;
	while (slow != fast)
	{
		if (fast == null || fast.next == null) return false;
		slow = slow.next;
		fast = fast.next.next;
	}
	return true;
}
```

#### [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)

@2023.09.16

🟡

```java
/**
 * 双指针
 * Somnia1337
 */
public ListNode detectCycle(ListNode head)
{
	if (head == null) return null;
	ListNode slow = head, fast = head;
	do
	{
		if (fast == null || fast.next == null) return null;
		fast = fast.next.next;
		slow = slow.next;
	} while (slow != fast);
	slow = head;
	while (slow != fast)
	{
		slow = slow.next;
		fast = fast.next;
	}
	return slow;
}
```

#### [143. 重排链表](https://leetcode.cn/problems/reorder-list/)

@2023.10.18

🟡

1. 递归

2302ms，笑了…

```java
/**
 * 递归
 * Somnia1337
 */
public void reorderList(ListNode head)
{
	if (head == null || head.next == null) return;
	ListNode oldNext = head.next;
	ListNode newNext = getLast(head.next);
	if (newNext == null) return;
	head.next = newNext;
	newNext.next = oldNext;
	reorderList(oldNext);
}

private ListNode getLast(ListNode node)
{
	if (node.next != null && node.next.next != null) return getLast(node.next);
	ListNode ret = node.next;
	node.next = null;
	return ret;
}
```

2. 辅助数据结构

```java
/**
 * 辅助数据结构
 * 力扣官方题解
 */
public void reorderList(ListNode head)
{
	if (head == null) return;
	List<ListNode> list = new ArrayList<ListNode>();
	ListNode node = head;
	while (node != null)
	{
		list.add(node);
		node = node.next;
	}
	int i = 0, j = list.size() - 1;
	while (i < j)
	{
		list.get(i).next = list.get(j);
		i++;
		if (i == j) break;
		list.get(j).next = list.get(i);
		j--;
	}
	list.get(i).next = null;
}
```

3. ？

```java
/**
 * ?
 * 灵茶山艾府
 */
public void reorderList(ListNode head)
{
	ListNode mid = middleNode(head);
	ListNode head2 = reverseList(mid);
	while (head2.next != null)
	{
		ListNode nxt = head.next;
		ListNode nxt2 = head2.next;
		head.next = head2;
		head2.next = nxt;
		head = nxt;
		head2 = nxt2;
	}
}

// 876. 链表的中间结点
private ListNode middleNode(ListNode head)
{
	ListNode slow = head, fast = head;
	while (fast != null && fast.next != null)
	{
		slow = slow.next;
		fast = fast.next.next;
	}
	return slow;
}

// 206. 反转链表
private ListNode reverseList(ListNode head)
{
	ListNode pre = null, cur = head;
	while (cur != null)
	{
		ListNode nxt = cur.next;
		cur.next = pre;
		pre = cur;
		cur = nxt;
	}
	return pre;
}
```

```java
/**
 * DFS
 * ?
 */
public void reorderList(ListNode head)
{
	dfs(head, head.next);
}

private ListNode dfs(ListNode slow, ListNode fast)
{
	ListNode ret;
	if (fast == null)
	{
		ret = slow.next;
		slow.next = null;
		return ret;
	}
	if (fast.next == null)
	{
		ret = slow.next.next;
		slow.next.next = null;
		return ret;
	}
	ListNode tail = dfs(slow.next, fast.next.next);
	ret = tail.next;
	tail.next = slow.next;
	slow.next = tail;
	return ret;
}
```

#### [146. LRU 缓存](https://leetcode.cn/problems/lru-cache/)

@2023.09.24

🟡

```java
/**
 * 双向链表
 * 力扣官方题解
 */
private Map<Integer, DLinkedNode> cache = new HashMap<Integer, DLinkedNode>();
private int size;
private int capacity;
private DLinkedNode head, tail;

public LRUCache(int capacity)
{
	this.size = 0;
	this.capacity = capacity;
	// 使用伪头部和伪尾部节点
	head = new DLinkedNode();
	tail = new DLinkedNode();
	head.next = tail;
	tail.prev = head;
}

public int get(int key)
{
	DLinkedNode node = cache.get(key);
	if (node == null)
	{
		return -1;
	}
	// 如果 key 存在，先通过哈希表定位，再移到头部
	moveToHead(node);
	return node.value;
}

public void put(int key, int value)
{
	DLinkedNode node = cache.get(key);
	if (node == null)
	{
		// 如果 key 不存在，创建一个新的节点
		DLinkedNode newNode = new DLinkedNode(key, value);
		// 添加进哈希表
		cache.put(key, newNode);
		// 添加至双向链表的头部
		addToHead(newNode);
		++size;
		if (size > capacity)
		{
			// 如果超出容量，删除双向链表的尾部节点
			DLinkedNode tail = removeTail();
			// 删除哈希表中对应的项
			cache.remove(tail.key);
			--size;
		}
	}
	else
	{
		// 如果 key 存在，先通过哈希表定位，再修改 value，并移到头部
		node.value = value;
		moveToHead(node);
	}
}

private void addToHead(DLinkedNode node)
{
	node.prev = head;
	node.next = head.next;
	head.next.prev = node;
	head.next = node;
}

private void removeNode(DLinkedNode node)
{
	node.prev.next = node.next;
	node.next.prev = node.prev;
}

private void moveToHead(DLinkedNode node)
{
	removeNode(node);
	addToHead(node);
}

private DLinkedNode removeTail()
{
	DLinkedNode res = tail.prev;
	removeNode(res);
	return res;
}

class DLinkedNode
{
	int key;
	int value;
	DLinkedNode prev;
	DLinkedNode next;

	public DLinkedNode() {}

	public DLinkedNode(int _key, int _value)
	{
		key = _key;
		value = _value;
	}
}
```

#### [148. 排序链表](https://leetcode.cn/problems/sort-list/)

@2023.09.22

🟡

```java
/**
 * 辅助列表
 * Somnia1337
 */
public ListNode sortList(ListNode head)
{
	if (head == null) return null;
	List<ListNode> nodes = new ArrayList<>();
	while (head != null)
	{
		nodes.add(head);
		head = head.next;
	}
	nodes.sort(Comparator.comparingInt(a -> a.val));
	for (int i = 1; i < nodes.size(); i++)
	{
		nodes.get(i - 1).next = nodes.get(i);
	}
	nodes.get(nodes.size() - 1).next = null;
	return nodes.get(0);
}
```

```java
/**
 * 归并排序
 * 力扣官方题解
 */
public ListNode sortList(ListNode head)
{
	if (head == null) return head;
	int len = 0;
	ListNode node = head;
	while (node != null)
	{
		len++;
		node = node.next;
	}
	ListNode dummyHead = new ListNode(0, head);
	for (int subLength = 1; subLength < len; subLength <<= 1)
	{
		ListNode prev = dummyHead, curr = dummyHead.next;
		while (curr != null)
		{
			ListNode head1 = curr;
			for (int i = 1; i < subLength && curr.next != null; i++)
			{
				curr = curr.next;
			}
			ListNode head2 = curr.next;
			curr.next = null;
			curr = head2;
			for (int i = 1; i < subLength && curr != null && curr.next != null; i++)
			{
				curr = curr.next;
			}
			ListNode next = null;
			if (curr != null)
			{
				next = curr.next;
				curr.next = null;
			}
			prev.next = merge(head1, head2);
			while (prev.next != null)
			{
				prev = prev.next;
			}
			curr = next;
		}
	}
	return dummyHead.next;
}

public ListNode merge(ListNode head1, ListNode head2)
{
	ListNode dummyHead = new ListNode(0);
	ListNode temp = dummyHead, temp1 = head1, temp2 = head2;
	while (temp1 != null && temp2 != null)
	{
		if (temp1.val <= temp2.val)
		{
			temp.next = temp1;
			temp1 = temp1.next;
		}
		else
		{
			temp.next = temp2;
			temp2 = temp2.next;
		}
		temp = temp.next;
	}
	if (temp1 != null)
	{
		temp.next = temp1;
	}
	else if (temp2 != null)
	{
		temp.next = temp2;
	}
	return dummyHead.next;
}
```

#### [160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/)

@2023.10.13

🟢

```java
/**
 * 双指针
 * Somnia1337
 */
public boolean hasCycle(ListNode head)
{
	if (head == null) return false;
	ListNode fast = head.next, slow = head;
	while (slow != fast)
	{
		if (fast == null || fast.next == null) return false;
		slow = slow.next;
		fast = fast.next.next;
	}
	return true;
}
```

#### [203. 移除链表元素](https://leetcode.cn/problems/remove-linked-list-elements/)

@2023.09.12

🟢

1. 迭代

```java
/**
 * 迭代
 * Somnia1337
 */
public ListNode removeElements(ListNode head, int val)
{
	ListNode dummyHead = new ListNode(0, head), prev = dummyHead;
	while (head != null)
	{
		if (head.val == val)
		{
			prev.next = head.next;
		}
		else
		{
			prev.next = head;
			prev = prev.next;
		}
		head = head.next;
	}
	return dummyHead.next;
}
```

```java
/**
 * 迭代
 * 力扣官方题解
 */
public ListNode removeElements(ListNode head, int val)
{
	ListNode dummyHead = new ListNode(0, head), prev = dummyHead;
	while (prev.next != null)
	{
		if (prev.next.val == val)
		{
			prev.next = prev.next.next;
		}
		else
		{
			prev = prev.next;
		}
	}
	return dummyHead.next;
}
```

2. 递归

```java
/**
 * 递归
 * 力扣官方题解
 */
public ListNode removeElements(ListNode head, int val)
{
	if (head == null) return null;
	head.next = removeElements(head.next, val);
	return head.val == val ? head.next : head;
}
```

#### [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)

@2023.09.12

🟢

1. 双指针

```java
/**
 * 双指针
 * 代码随想录
 */
public ListNode reverseList(ListNode head)
{
	ListNode prev = null;
	ListNode cur = head;
	ListNode temp;
	while (cur != null)
	{
		temp = cur.next;
		cur.next = prev;
		prev = cur;
		cur = temp;
	}
	return prev;
}
```

2. 递归

```java
/**
 * 递归
 * 代码随想录
 */
public ListNode reverseList(ListNode head)
{
	return reverse(null, head);
}

private ListNode reverse(ListNode prev, ListNode cur)
{
	if (cur == null)
	{
		return prev;
	}
	ListNode temp;
	temp = cur.next;
	cur.next = prev;
	return reverse(cur, temp);
}
```

#### [234. 回文链表](https://leetcode.cn/problems/palindrome-linked-list/)

@2023.10.16

🟢

1. 模拟

```java
/**
 * 模拟
 * Somnia1337
 */
public boolean isPalindrome(ListNode head)
{
	StringBuilder str = new StringBuilder();
	while (head != null)
	{
		str.append(head.val);
		head = head.next;
	}
	return isPalindrome(str.toString());
}

public boolean isPalindrome(String s)
{
	int len = s.length();
	for (int i = 0; i < len / 2; i++)
	{
		if (s.charAt(i) != s.charAt(len - i - 1)) return false;
	}
	return true;
}
```

2. 双指针

```java
/**
 * 双指针
 * 芜湖大司码
 */
public boolean isPalindrome(ListNode head)
{
	if (head == null || head.next == null) return true;
	ListNode slow = head, fast = head;
	ListNode pre = head, prepre = null;
	while (fast != null && fast.next != null)
	{
		pre = slow;
		slow = slow.next;
		fast = fast.next.next;
		pre.next = prepre;
		prepre = pre;
	}
	if (fast != null) slow = slow.next;
	while (pre != null && slow != null)
	{
		if (pre.val != slow.val) return false;
		pre = pre.next;
		slow = slow.next;
	}
	return true;
}
```

#### [237. 删除链表中的节点](https://leetcode.cn/problems/delete-node-in-a-linked-list/)

@2023.09.30

🟡

1. 迭代

```java
/**
 * 迭代
 * Somnia1337
 */
public void deleteNode(ListNode node)
{
	while (node != null)
	{
		node.val = node.next.val;
		if (node.next.next == null) node.next = null;
		node = node.next;
	}
}
```

2. 节点交换

```java
/**
 * 节点交换
 * 力扣官方题解
 */
public void deleteNode(ListNode node)
{
	node.val = node.next.val;
	node.next = node.next.next;
}
```

#### [328. 奇偶链表](https://leetcode.cn/problems/odd-even-linked-list/)

@2023.09.17

🟡

```java
/**
 * 
 * Angus-Liu
 */
public ListNode oddEvenList(ListNode head)
{
	if (head == null || head.next == null) return head;
	ListNode oddEnd = head;
	ListNode evenHead = head.next;
	ListNode evenEnd = evenHead;
	
	while (oddEnd.next != null && evenEnd.next != null)
	{
		oddEnd.next = evenEnd.next;
		oddEnd = oddEnd.next;
		evenEnd.next = oddEnd.next;
		evenEnd = evenEnd.next;
	}
	
	oddEnd.next = evenHead;
	return head;
}
```

#### [369. 给单链表加一](https://leetcode.cn/problems/plus-one-linked-list/)

@2023.10.17

🟡

```java
/**
 * 递归
 * Somnia1337
 */
private int add;

public ListNode plusOne(ListNode head)
{
	add = 1;
	ListNode dummyHead = new ListNode(0, head);
	plusBackwards(head);
	if (add > 0)
	{
		dummyHead.next = new ListNode(add, head);
	}
	return dummyHead.next;
}

private void plusBackwards(ListNode node)
{
	if (node.next != null) plusBackwards(node.next);
	node.val += add;
	add = 0;
	if (node.val >= 10)
	{
		add = node.val / 10;
		node.val %= 10;
	}
}
```

#### [445. 两数相加 II](https://leetcode.cn/problems/add-two-numbers-ii/)

@2023.10.09

🟡

偷懒，用 [415. 字符串相加](https://leetcode.cn/problems/add-strings/) 的代码模拟。

```java
/**
 * 模拟
 * Somnia1337
 */
public ListNode addTwoNumbers(ListNode l1, ListNode l2)
{
	StringBuilder a = new StringBuilder();
	StringBuilder b = new StringBuilder();
	while (l1 != null)
	{
		a.append(l1.val);
		l1 = l1.next;
	}
	while (l2 != null)
	{
		b.append(l2.val);
		l2 = l2.next;
	}
	String sum = addStrings(a.toString(), b.toString());
	ListNode dummyHead = new ListNode(0), ret = dummyHead;
	for (int i = 0; i < sum.length(); i++)
	{
		dummyHead.next = new ListNode(sum.charAt(i) - '0');
		dummyHead = dummyHead.next;
	}
	return ret.next;
}

public String addStrings(String num1, String num2)
{
	int len1 = num1.length(), len2 = num2.length(), len = Math.max(len1, len2);
	if (len1 < len) num1 = "0".repeat(len2 - len1) + num1;
	else if (len2 < len) num2 = "0".repeat(len1 - len2) + num2;
	StringBuilder sb = new StringBuilder();
	char carry = '0';
	for (int i = len - 1; i >= 0; i--)
	{
		char ch1 = num1.charAt(i), ch2 = num2.charAt(i), ch = (char) (ch1 - '0' + ch2 - '0' + carry);
		carry = '0';
		while (ch > '9')
		{
			ch -= 10;
			carry += 1;
		}
		sb.append(ch);
	}
	sb.append(carry);
	String ret = sb.reverse()
				   .toString();
	if (ret.charAt(0) == '0') ret = ret.replaceFirst("^0+", "");
	return ret.isEmpty() ? "0" : ret;
}
```

#### [460. LFU 缓存](https://leetcode.cn/problems/lfu-cache/)

@2023.09.25

🔴

```java
/**
 * 哈希表
 * 灵茶山艾府
 */
private final int capacity;
private final Map<Integer, Node> keyToNode = new HashMap<>();
private final Map<Integer, Node> freqToDummy = new HashMap<>();
private int minFreq;

public LFUCache(int capacity)
{
	this.capacity = capacity;
}

public int get(int key)
{
	Node node = getNode(key);
	return node != null ? node.value : -1;
}

public void put(int key, int value)
{
	Node node = getNode(key);
	if (node != null)
	{ // 有这本书
		node.value = value; // 更新 value
		return;
	}
	if (keyToNode.size() == capacity)
	{ // 书太多了
		Node dummy = freqToDummy.get(minFreq);
		Node backNode = dummy.prev; // 最左边那摞书的最下面的书
		keyToNode.remove(backNode.key);
		remove(backNode); // 移除
		if (dummy.prev == dummy)
		{ // 这摞书是空的
			freqToDummy.remove(minFreq); // 移除空链表
		}
	}
	node = new Node(key, value); // 新书
	keyToNode.put(key, node);
	pushFront(1, node); // 放在「看过 1 次」的最上面
	minFreq = 1;
}

private Node getNode(int key)
{
	if (!keyToNode.containsKey(key))
	{ // 没有这本书
		return null;
	}
	Node node = keyToNode.get(key); // 有这本书
	remove(node); // 把这本书抽出来
	Node dummy = freqToDummy.get(node.freq);
	if (dummy.prev == dummy)
	{ // 抽出来后，这摞书是空的
		freqToDummy.remove(node.freq); // 移除空链表
		if (minFreq == node.freq)
		{
			minFreq++;
		}
	}
	pushFront(++node.freq, node); // 放在右边这摞书的最上面
	return node;
}

// 创建一个新的双向链表
private Node newList()
{
	Node dummy = new Node(0, 0); // 哨兵节点
	dummy.prev = dummy;
	dummy.next = dummy;
	return dummy;
}

// 在链表头添加一个节点（把一本书放在最上面）
private void pushFront(int freq, Node x)
{
	Node dummy = freqToDummy.computeIfAbsent(freq, k -> newList());
	x.prev = dummy;
	x.next = dummy.next;
	x.prev.next = x;
	x.next.prev = x;
}

// 删除一个节点（抽出一本书）
private void remove(Node x)
{
	x.prev.next = x.next;
	x.next.prev = x.prev;
}

private static class Node
{
	int key, value, freq = 1; // 新书只读了一次
	Node prev, next;

	Node(int key, int value)
	{
		this.key = key;
		this.value = value;
	}
}
```

#### [725. 分隔链表](https://leetcode.cn/problems/split-linked-list-in-parts/)

@2023.12.28

🟡

用 `Math::ceil` 计算每段长度，遍历，将每段头节点加入答案。

```java
/**
 * 模拟
 * Somnia1337
 */
public ListNode[] splitListToParts(ListNode head, int k) {
	int n = 0;
	ListNode it = head;
	while (it != null) {
		it = it.next;
		n++;
	}
	
	int cur = (int) Math.ceil((double) n / k), kHold = k;
	it = head;
	ListNode[] ans = new ListNode[k];
	for (int i = 0; i < kHold && cur > 0 && it != null; i++) {
		ans[i] = it;
		for (int c = 0; c < cur - 1; c++) it = it.next;
		if (it != null) {
			ListNode hold = it.next;
			it.next = null;
			it = hold;
		}
		cur = (int) Math.ceil((double) (n -= cur) / (--k));
	}
	return ans;
}
```

#### [876. 链表的中间结点](https://leetcode.cn/problems/middle-of-the-linked-list/)

@2023.12.12

🟢

```java
/**
 * 双指针
 * Somnia1337
 */
public ListNode middleNode(ListNode head) {
	ListNode fast = head, slow = head;
	while (fast != null) {
		fast = fast.next;
		if (fast == null) return slow;
		fast = fast.next;
		if (fast == null) return slow.next;
		slow = slow.next;
	}
	return null;
}
```

#### [1171. 从链表中删去总和值为零的连续节点](https://leetcode.cn/problems/remove-zero-sum-consecutive-nodes-from-linked-list/)

@2023.12.29

🟡

```java
/**
 * 哈希表
 * Somnia1337
 */
public ListNode removeZeroSumSublists(ListNode head) {
	ListNode dummy = new ListNode(0, head);
	ListNode[] arr;
	boolean rem;
	do {
		rem = false;
		arr = new ListNode[1000];
		Map<Integer, Integer> pos = new HashMap<>();
		pos.put(0, -1);
		ListNode it = dummy.next;
		int sum = 0, i = 0;
		while (it != null) {
			sum += it.val;
			arr[i] = it;
			if (pos.containsKey(sum)) {
				int p = pos.get(sum);
				if (p >= 0) arr[p].next = it.next;
				else dummy.next = it.next;
				rem = true;
				break;
			}
			pos.put(sum, i++);
			it = it.next;
		}
	} while (rem && dummy.next != null);
	return dummy.next;
}
```

```java
/**
 * 哈希表
 * shane
 */
public ListNode removeZeroSumSublists(ListNode head) {
	ListNode dummy = new ListNode(0, head);
	Map<Integer, ListNode> pos = new HashMap<>();
	int sum = 0;
	for (ListNode d = dummy; d != null; d = d.next) {
		sum += d.val;
		pos.put(sum, d);
	}
	sum = 0;
	for (ListNode d = dummy; d != null; d = d.next) {
		sum += d.val;
		d.next = pos.get(sum).next;
	}
	return dummy.next;
}
```

#### [1669. 合并两个链表](https://leetcode.cn/problems/merge-in-between-linked-lists/)

@2023.12.22

🟡

```java
/**
 * 模拟
 * Somnia1337
 */
public ListNode mergeInBetween(ListNode list1, int a, int b, ListNode list2) {
	ListNode dummy = new ListNode(0, list1);
	while (--a > 0) {
		list1 = list1.next;
		b--;
	}
	ListNode hold = list1.next;
	while (--b > 0) hold = hold.next;
	list1.next = list2;
	while (list2.next != null) list2 = list2.next;
	list2.next = hold.next;
	return dummy.next;
}
```

#### [2095. 删除链表的中间节点](https://leetcode.cn/problems/delete-the-middle-node-of-a-linked-list/)

@2023.09.17

🟡

1. 遍历

```java
/**
 * 遍历
 * Somnia1337
 */
public ListNode deleteMiddle(ListNode head)
{
	if (head.next == null) return null;
	int len = 0;
	ListNode it = head;
	while (it != null)
	{
		it = it.next;
		len++;
	}
	len = len / 2 - 1;
	ListNode target = head;
	while (len > 0)
	{
		target = target.next;
		len--;
	}
	target.next = target.next.next;
	return head;
}
```

2. 双指针

```java
/**
 * 双指针
 * Somnia1337
 */
public ListNode deleteMiddle(ListNode head)
{
	if (head.next == null) return null;
	ListNode slow = new ListNode(0, head);
	ListNode fast = head;
	while (fast != null)
	{
		fast = fast.next;
		if (fast == null) break;
		fast = fast.next;
		slow = slow.next;
	}
	if (slow.next != null) slow.next = slow.next.next;
	return head;
}
```

```java
/**
 * 双指针
 * 力扣官方题解
 * @translator Somnia1337
 */
public ListNode deleteMiddle(ListNode head)
{
	if (head.next == null) return null;
	ListNode slow = head, fast = head, pre = null;
	while (fast != null && fast.next != null)
	{
		fast = fast.next.next;
		pre = slow;
		slow = slow.next;
	}
	pre.next = pre.next.next;
	return head;
}
```

#### [2487. 从链表中移除节点](https://leetcode.cn/problems/remove-nodes-from-linked-list/)

@2024.01.03

1. 单调栈

```java
/**
 * 单调栈
 * Somnia1337
 */
public ListNode removeNodes(ListNode head) {
	Deque<ListNode> dq = new ArrayDeque<>();
	while (head != null) {
		while (!dq.isEmpty() && dq.peek().val < head.val) dq.poll();
		dq.push(head);
		head = head.next;
	}
	ListNode next = null;
	while (dq.size() > 1) {
		ListNode p = dq.poll();
		p.next = next;
		next = p;
	}
	head = dq.poll();
	head.next = next;
	return head;
}
```

2. 递归

```java
/**
 * 递归
 * Liftoff
 */
public ListNode removeNodes(ListNode head) {
	if (head == null) return null;
	ListNode node = removeNodes(head.next);
	if (node == null || head.val >= node.val) {
		head.next = node;
		return head;
	}
	return node;
}
```

```java
/**
 * 递归
 * MRevenger
 */
public ListNode removeNodes(ListNode head) {
	if (head == null || head.next == null) return head;
	head.next = removeNodes(head.next);
	if (head.next != null && head.val < head.next.val) return head.next;
	return head;
}
```

#### [2807. 在链表中插入最大公约数](https://leetcode.cn/problems/insert-greatest-common-divisors-in-linked-list/)

@2024.01.06

```java
/**
 * 迭代
 * Somnia1337
 */
public ListNode insertGreatestCommonDivisors(ListNode head) {
	ListNode dummy = new ListNode(0, head);
	while (head.next != null) {
		int a = head.val, b = head.next.val;
		ListNode next = head.next;
		head.next = new ListNode(gcd(a, b));
		head.next.next = next;
		head = next;
	}
	return dummy.next;
}

private int gcd(int x, int y) {
	return y == 0 ? x : gcd(y, x % y);
}
```

```java
/**
 * 迭代
 * 灵茶山艾府
 */
public ListNode insertGreatestCommonDivisors(ListNode head) {
	for (ListNode cur = head; cur.next != null; cur = cur.next.next) {
		cur.next = new ListNode(gcd(cur.val, cur.next.val), cur.next);
	}
	return head;
}

private int gcd(int x, int y) {
	return y == 0 ? x : gcd(y, x % y);
}
```

#### [LCR 026. 重排链表](https://leetcode.cn/problems/LGjMqU/)

@2023.12.08

🟡

与 [143. 重排链表](https://leetcode.cn/problems/reorder-list/) 相同。

```java
/**
 * DFS
 * ?
 */
public void reorderList(ListNode head)
{
	dfs(head, head.next);
}

private ListNode dfs(ListNode slow, ListNode fast)
{
	ListNode ret;
	if (fast == null)
	{
		ret = slow.next;
		slow.next = null;
		return ret;
	}
	if (fast.next == null)
	{
		ret = slow.next.next;
		slow.next.next = null;
		return ret;
	}
	ListNode tail = dfs(slow.next, fast.next.next);
	ret = tail.next;
	tail.next = slow.next;
	slow.next = tail;
	return ret;
}
```

#### [LCR 024. 反转链表](https://leetcode.cn/problems/UHnkqh/)

@2023.12.13

🟢

与 [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/) 相同。

```java
/**
 * 双指针
 * Somnia1337
 */
public ListNode reverseList(ListNode head) {
	ListNode pre = null, cur = head, temp;
	while (cur != null) {
		temp = cur.next;
		cur.next = pre;
		pre = cur;
		cur = temp;
	}
	return pre;
}
```

#### [LCR 123. 图书整理 I](https://leetcode.cn/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

@2023.12.09

🟢

```java
/**
 * 两次遍历
 * Somnia1337
 */
public int[] reverseBookList(ListNode head)
{
	int l = 0;
	ListNode it = head;
	while (it != null)
	{
		it = it.next;
		l++;
	}
	int[] ans = new int[l];
	for (int i = l - 1; i >= 0; i--)
	{
		ans[i] = head.val;
		head = head.next;
	}
	return ans;
}
```

```java
/**
 * 一次遍历
 * DerekLIU
 */
private int[] ans;
private int l, i;

public int[] reverseBookList(ListNode head)
{
	l = 0;
	i = 0;
	solve(head);
	return ans;
}

private void solve(ListNode head)
{
	if (head == null)
	{
		ans = new int[l];
		return;
	}
	l++;
	solve(head.next);
	ans[i] = head.val;
	i++;
}
```

#### [LCR 140. 训练计划 II](https://leetcode.cn/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

@2023.12.09

🟢

```java
/**
 * 后序遍历
 * Somnia1337
 */
private int l;
private ListNode ans;

public ListNode trainingPlan(ListNode head, int cnt)
{
	l = cnt;
	solve(head);
	return ans;
}

private void solve(ListNode node)
{
	if (node == null) return;
	solve(node.next);
	if (ans != null) return;
	if (--l == 0) ans = node;
}
```

#### [LCR 141. 训练计划 III](https://leetcode.cn/problems/fan-zhuan-lian-biao-lcof/)

@2023.12.09

🟢

与 [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/) 相同。

```java
/**
 * 双指针
 * Somnia1337
 */
public ListNode trainningPlan(ListNode head)
{
	ListNode prev = null;
	ListNode cur = head;
	ListNode temp;
	while (cur != null)
	{
		temp = cur.next;
		cur.next = prev;
		prev = cur;
		cur = temp;
	}
	return prev;
}
```

#### [LCR 142. 训练计划 IV](https://leetcode.cn/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

@2023.12.09

🟢

与 [21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/) 相同。

```java
/**
 * 迭代
 * Somnia1337
 */
public ListNode trainningPlan(ListNode l1, ListNode l2)
{
	ListNode dummy = new ListNode(), it = dummy;
	while (l1 != null || l2 != null)
	{
		if (l2 == null || l1 != null && l1.val < l2.val)
		{
			it.next = new ListNode(l1.val);
			l1 = l1.next;
		}
		else
		{
			it.next = new ListNode(l2.val);
			l2 = l2.next;
		}
		it = it.next;
	}
	return dummy.next;
}
```

#### [面试题 02.05. 链表求和](https://leetcode.cn/problems/sum-lists-lcci/)

@2023.12.08

🟡

与 [2. 两数相加](https://leetcode.cn/problems/add-two-numbers/) 相同。

```java
/**
 * 模拟
 * Somnia1337
 */
public ListNode addTwoNumbers(ListNode l1, ListNode l2)
{
	int c = 0;
	ListNode ret = new ListNode(), it = ret;
	while (l1 != null || l2 != null)
	{
		int v1 = l1 != null ? l1.val : 0, v2 = l2 != null ? l2.val : 0, s = v1 + v2 + c;
		c = s / 10;
		it.next = new ListNode(s % 10);
		it = it.next;
		l1 = l1 != null ? l1.next : null;
		l2 = l2 != null ? l2.next : null;
	}
	if (c > 0) it.next = new ListNode(c);
	return ret.next;
}
```

#### [面试题 02.07. 链表相交](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/)

@2023.09.15

🟢

```java
/**
 * 双指针
 * Krahets
 */
public ListNode getIntersectionNode(ListNode headA, ListNode headB)
{
	ListNode A = headA, B = headB;
	while (A != B)
	{
		A = A != null ? A.next : headB;
		B = B != null ? B.next : headA;
	}
	return A;
}
```