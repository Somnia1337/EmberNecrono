### DFS 文件夹

```java
private void dfs(File folder) {
	if (folder.isDirectory()) { // 为文件夹
		File[] files = folder.listFiles(); // 获取子文件 / 子文件夹列表
		if (files != null) { // 判空
			for (File file : files) {
				if (file.isFile()) { // 为文件, 处理
					if (file.getName().endsWith(".md")) { // 条件
						// 处理
					}
				}
				else if (file.isDirectory()) dfs(file); // 为文件夹, 递归
			}
		}
	}
}
```

### 读取文本文件

```java
private List<String> read(File file) {
	List<String> ret = new ArrayList<>();
	try (BufferedReader reader = new BufferedReader(new FileReader(file))) {
		String line; // while 外置定义 line
		while ((line = reader.readLine()) != null) // 非 null
		{
			// 处理
		}
	}
	catch (IOException e) {
		e.printStackTrace();
	}
	return ret;
}
```