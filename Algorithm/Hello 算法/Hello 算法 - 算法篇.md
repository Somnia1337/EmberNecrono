## 1. 搜索

#### 二分查找

关键点：

- 防溢出：`mid = left + (right - left) / 2`。
- 循环条件：`left <= right`。
- 更新范围：`left = mid + 1`，`right = mid - 1`。

左闭右开写法：

- 循环条件：`left < right`。
- 更新范围：`left = mid + 1`，`right = mid`。

适用性：大量、有序数据，数组形式。

## 2. 排序

### 1) $O(N^2)$ 的比较排序

#### 选择排序

数组左侧为已排序部分，右侧为未排序部分。每次从已排序部分的右侧选择最小元素，与未排序部分的首元素交换，扩展已排序部分。

> 选择排序在任何情况下复杂度均为 $O(N^2)$，且为不稳定排序。

#### 冒泡排序

数组右侧为已排序部分，左侧为未排序部分。每次从左端开始遍历，找到比其右侧元素大的元素就交换两元素，直到遍历至已排序部分。

可以使用标志变量监测每次遍历，如果没有交换元素，说明已完成排序，跳过剩余循环。

> 冒泡排序基于元素交换，每次交换涉及 3 步操作，开销较高。

#### 插入排序

数组左侧为已排序部分，右侧为未排序部分。每次操作未排序部分的首元素，遍历已排序部分，找到其目标位置并插入，扩展已排序部分。

> 插入排序在数据量较小时通常更快，甚至快过 $O(N \log N)$，因此许多语言（如 Java）的内置排序函数部分采用插入排序，用于排序短数组。它也是 3 种 $O(N^2)$ 排序算法中最常用的。

### 2) $O(N \log N)$ 的比较排序

#### 快速排序

每次选取左端元素作为基准，用左右双指针遍历数组，当左指针所指元素大于基准 / 右指针所指元素小于基准时，交换两元素，直到两指针相遇，最后交换左端元素与两指针所指元素。这会将原数组划分为两个子数组，对它们递归进行以上过程。

```java
void quickSort(int[] nums, int left, int right)
{
	if (left >= right) return;
	int pivot = partition(nums, left, right);
	quickSort(nums, left, pivot - 1);
	quickSort(nums, pivot + 1, right);
}

void swap(int[] nums, int i, int j)
{
	int tmp = nums[i];
	nums[i] = nums[j];
	nums[j] = tmp;
}

int partition(int[] nums, int left, int right)
{
	int i = left, j = right;
	while (i < j)
	{
		while (i < j && nums[j] >= nums[left]) j--;
		while (i < j && nums[i] <= nums[left]) i++;
		swap(nums, i, j);
	}
	swap(nums, i, left);
	return i;
}
```

如果数组为倒序排列，快速排序将退化为冒泡排序。可以进行**基准选取优化**，将数组左端、中间、右端三个元素的平均值作为基准，降低最坏情况出现的概率。

```java
/*基准选取优化*/
int partition(int[] nums, int left, int right)
{
	int med = medianThree(nums, left, (left + right) / 2, right);
	swap(nums, left, med);
	int i = left, j = right;
	while (i < j)
	{
		while (i < j && nums[j] >= nums[left]) j--;
		while (i < j && nums[i] <= nums[left]) i++;
		swap(nums, i, j);
	}
	swap(nums, i, left);
	return i;
}

int medianThree(int[] nums, int left, int mid, int right)
{
	if ((nums[left] < nums[mid]) ^ (nums[left] < nums[right])) return left;
	else if ((nums[mid] < nums[left]) ^ (nums[mid] < nums[right])) return mid;
	else return right;
}
```

当数组倒序排序时，另一个问题是占用内存较多（划分后右子数组为空，递归树高度达 `n - 1`）。可以进行 **尾递归优化**，划分后比较两个子数组的长度，仅对较短子数组递归。

```java
/*尾递归优化*/
void quickSort(int[] nums, int left, int right)
{
	while (left < right)
	{
		int pivot = partition(nums, left, right);
		if (pivot - left < right - pivot)
		{
			quickSort(nums, left, pivot - 1);
			left = pivot + 1;
		}
		else
		{
			quickSort(nums, pivot + 1, right);
			right = pivot - 1;
		}
	}
}
```

#### 归并排序

分为划分、合并两个阶段。划分阶段递归地将数组不断划分为子数组，直到每个子数组长为 1。合并阶段递归地按顺序合并相邻的子数组。

```java
/*归并排序*/
void mergeSort(int[] nums, int left, int right)
{
	if (left >= right) return;
	int mid = (left + right) / 2;
	mergeSort(nums, left, mid);
	mergeSort(nums, mid + 1, right);
	merge(nums, left, mid, right);
}

void merge(int[] nums, int left, int mid, int right)
{
	int[] tmp = Arrays.copyOfRange(nums, left, right + 1);
	int leftStart = 0, leftEnd = mid - left;
	int rightStart = mid + 1 - left, rightEnd = right - left;
	int i = leftStart, j = rightStart;
	for (int k = left; k <= right; k++)
	{
		if (i > leftEnd) nums[k] = tmp[j++];
		else if (j > rightEnd || tmp[i] <= tmp[j]) nums[k] = tmp[i++];
		else nums[k] = tmp[j++];
	}
}
```

#### 堆排序

待学习

### 3) 可达$O(N)$的非比较排序

#### 桶排序

设置一些有大小顺序的桶，每个桶对应一个数据范围，将元素平均分配到各个桶中，在每个桶内部进行排序，然后按顺序合并桶。

```java
/*桶排序*/
void bucketSort(float[] nums)
{
	int k = nums.length / 2;
	List<List<Float>> buckets = new ArrayList<>();
	for (int i = 0; i < k; i++)
	{
		buckets.add(new ArrayList<>());
	}
	for (float num : nums)
	{
		int i = (int) (num * k);
		buckets.get(i)
			   .add(num);
	}
	for (List<Float> bucket : buckets)
	{
		Collections.sort(bucket);
	}
	int i = 0;
	for (List<Float> bucket : buckets)
	{
		for (float num : bucket)
		{
			nums[i++] = num;
		}
	}
}
```

> 桶排序适合处理大量数据，如100万个数据，由于空间限制，内存无法一次性加载全部数据，利用桶排序可以将数据分为1000块，分别排序后再合并。
> 
> 桶排序的时间复杂度为$O(N + k)$。假设$N$个元素平均分布在$k$个桶中，每个桶的排序时间复杂度为$O(\frac{N}{k} \log \frac{N}{k})$，排序$N$个桶的时间复杂度为$O(N \log \frac{N}{k})$，当$k$较大时，该复杂度趋近于$O(N)$，合并时需要遍历，因此总复杂度为$O(N + k)$。

#### 计数排序

两次遍历原数组`nums`，第一次找到最大元素`m`，创建一个长为`m + 1`的计数数组`counts`，第二次遍历到每个`nums[i]`就对该计数数组`counts[nums[i]]++`。然后再遍历一次计数数组，对每个非零的`counts[num]`，用`num`覆盖原数组的当前元素。

> 前缀和计数排序：得到填充后的`counts`后，用其前缀和覆盖，此后，每个`counts[num] - 1`代表元素`num`在排序后的数组中最后一次出现的位置，因此倒序遍历`counts`，对原数组填入每个`num`即可。

```java
/*前缀和计数排序*/
void countingSort(int[] nums)
{
	int m = 0;
	for (int num : nums)
	{
		m = Math.max(m, num);
	}
	int[] counter = new int[m + 1];
	for (int num : nums)
	{
		counter[num]++;
	}
	for (int i = 0; i < m; i++)
	{
		counter[i + 1] += counter[i];
	}
	int n = nums.length;
	int[] res = new int[n];
	for (int i = n - 1; i >= 0; i--)
	{
		int num = nums[i];
		res[counter[num] - 1] = num;
		counter[num]--;
	}
	System.arraycopy(res, 0, nums, 0, n);
}
```

#### 基数排序

对所有数，按从最低位到最高位的顺序，每次对一位数字排序。

```java
/*基数排序*/
void radixSort(int[] nums)
{
	int m = Integer.MIN_VALUE;
	for (int num : nums)
		if (num > m) m = num;
	for (int exp = 1; exp <= m; exp *= 10)
		countingSortDigit(nums, exp);
}

void countingSortDigit(int[] nums, int exp)
{
	int[] counter = new int[10];
	int n = nums.length;
	for (int num : nums)
	{
		int d = digit(num, exp);
		counter[d]++;
	}
	for (int i = 1; i < 10; i++)
	{
		counter[i] += counter[i - 1];
	}
	int[] res = new int[n];
	for (int i = n - 1; i >= 0; i--)
	{
		int d = digit(nums[i], exp);
		int j = counter[d] - 1;
		res[j] = nums[i];
		counter[d]--;
	}
	System.arraycopy(res, 0, nums, 0, n);
}

int digit(int num, int exp)
{
	return (num / exp) % 10;
}
```

> 基数排序适合处理可以统一为同一位数的数据，例如学号等编号。

## 3. 分治

分治比直接解决原问题高效的原因：

- 操作数量优化：将数组划分为若干子数组的归并排序效率为$O(N \log N)$，划分更多个元素的桶排序则为$O(N + k)$。
- 并行计算优化：分治有利于操作系统的并行优化。

汉诺塔问题的解：

```java
void solveHanota(List<Integer> A, List<Integer> B, List<Integer> C)
{
	int n = A.size();
	dfs(n, A, B, C);
}

void dfs(int i, List<Integer> src, List<Integer> buf, List<Integer> tar)
{
	if (i == 1)
	{
		move(src, tar);
		return;
	}
	dfs(i - 1, src, tar, buf);
	move(src, tar);
	dfs(i - 1, buf, src, tar);
}

void move(List<Integer> src, List<Integer> tar)
{
	Integer pan = src.remove(src.size() - 1);
	tar.add(pan);
}
```

## 4. 回溯

回溯：从一个初始状态出发，暴搜所有可能答案。回溯通常用DFS遍历解空间，在搜索时，采用“尝试”与“回退”的策略，每当达到无法继续前进的状态，就撤销上一步的选择，退回上一个状态，并尝试其他可能选择。

回溯适合解决的问题：

- 搜索问题：全排列，子集和，汉诺塔。
- 约束满足问题：N皇后，数独，图着色。
- 组合优化问题（回溯通常不是最优方案）：0-1背包，旅行商，最大团。

#### 全排列问题

###### 情况1：无重复元素

剪枝：用一个布尔数组记录每个元素是否已被选择，每一轮选择元素时跳过已选择的元素。

![[Snipaste_230908_085228.png|500]]

```java
void backtrack(List<Integer> state, int[] choices, boolean[] selected, List<List<Integer>> res)
{
	if (state.size() == choices.length)
	{
		res.add(new ArrayList<>(state));
		return;
	}
	for (int i = 0; i < choices.length; i++)
	{
		int choice = choices[i];
		if (!selected[i])
		{
			selected[i] = true;
			state.add(choice);
			backtrack(state, choices, selected, res);
			selected[i] = false;
			state.remove(state.size() - 1);
		}
	}
}

List<List<Integer>> permutationsI(int[] nums)
{
	List<List<Integer>> res = new ArrayList<>();
	backtrack(new ArrayList<>(), nums, new boolean[nums.length], res);
	return res;
}
```

###### 情况2：有重复元素

剪枝：在每轮选择中，保证多个相同元素只被选择一次（哈希表实现）。

![[Snipaste_230908_085322.png|500]]

![[Snipaste_230908_085414.png|500]]

```java
void backtrack(List<Integer> state, int[] choices, boolean[] selected, List<List<Integer>> res)
{
	if (state.size() == choices.length)
	{
		res.add(new ArrayList<>(state));
		return;
	}
	Set<Integer> duplicated = new HashSet<>();
	for (int i = 0; i < choices.length; i++)
	{
		int choice = choices[i];
		if (!selected[i] && !duplicated.contains(choice))
		{
			duplicated.add(choice);
			selected[i] = true;
			state.add(choice);
			backtrack(state, choices, selected, res);
			selected[i] = false;
			state.remove(state.size() - 1);
		}
	}
}

List<List<Integer>> permutationsII(int[] nums)
{
	List<List<Integer>> res = new ArrayList<>();
	backtrack(new ArrayList<>(), nums, new boolean[nums.length], res);
	return res;
}
```

`selected`和`duplicated`剪枝的范围不同：

- `selected`避免某个元素在`state`中重复出现。
- `duplicated`保证相同元素只被选择一次。

![[Snipaste_230908_085938.png|500]]

#### 子集和问题

给定集合`nums`和目标和`target`，每个`nums[i]`可以多次使用，找出使若干`nums[i]`之和为`target`的所有组合。例如，`nums = {3, 4, 5}, target = 9`，输出`[3, 3, 3], [4, 5]`。

越界剪枝：

![[Snipaste_230909_091641.png|500]]

```java
List<List<Integer>> subsetSumINaive(int[] nums, int target)
{
	List<Integer> state = new ArrayList<>();
	int total = 0;
	List<List<Integer>> res = new ArrayList<>();
	backtrack(state, target, total, nums, res);
	return res;
}

void backtrack(List<Integer> state, int target, int total, int[] choices, List<List<Integer>> res)
{
	if (total == target)
	{
		res.add(new ArrayList<>(state));
		return;
	}
	for (int choice : choices)
	{
		if (total + choice > target)
		{
			continue;
		}
		state.add(choice);
		backtrack(state, target, total + choice, choices, res);
		state.remove(state.size() - 1);
	}
}
```

重复子集剪枝：

![[Snipaste_230909_092003.png|500]]

```java
List<List<Integer>> subsetSumI(int[] nums, int target)
{
	List<Integer> state = new ArrayList<>();
	Arrays.sort(nums);
	int start = 0;
	List<List<Integer>> res = new ArrayList<>();
	backtrack(state, target, nums, start, res);
	return res;
}

void backtrack(List<Integer> state, int target, int[] choices, int start, List<List<Integer>> res)
{
	if (target == 0)
	{
		res.add(new ArrayList<>(state));
		return;
	}
	for (int i = start; i < choices.length; i++)
	{
		if (target - choices[i] < 0)
		{
			break;
		}
		state.add(choices[i]);
		backtrack(state, target - choices[i], choices, i, res);
		state.remove(state.size() - 1);
	}
}
```

进阶：要求相同元素只能被选择一次。

剪枝：

![[Snipaste_230909_092445.png|500]]

```java
List<List<Integer>> subsetSumII(int[] nums, int target)
{
	List<Integer> state = new ArrayList<>();
	Arrays.sort(nums);
	int start = 0;
	List<List<Integer>> res = new ArrayList<>();
	backtrack(state, target, nums, start, res);
	return res;
}

void backtrack(List<Integer> state, int target, int[] choices, int start, List<List<Integer>> res)
{
	if (target == 0)
	{
		res.add(new ArrayList<>(state));
		return;
	}
	for (int i = start; i < choices.length; i++)
	{
		if (target - choices[i] < 0)
		{
			break;
		}
		if (i > start && choices[i] == choices[i - 1])
		{
			continue;
		}
		state.add(choices[i]);
		backtrack(state, target - choices[i], choices, i + 1, res);
		state.remove(state.size() - 1);
	}
}
```

## 5. 动态规划

动态规划通过将问题分解为一系列子问题，并存储子问题的解而避免重复计算。