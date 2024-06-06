---
title: Algorithms Baisc
description: "Algorithms Baisc and algorithms Baisc and algorithms Baisc and algorithms Baisc and algorithms Baisc and algorithms"
publishDate: 06 June 2024
tags: [algorithms]
draft: false
---

## 1. **线性搜索 (Linear Search)**

**复杂度**: O(n)

**特点和场景**:

- 适用于小规模数据集。
- 无需数据预处理。
- 简单易懂，但在大数据集上效率较低。

**思路**: 从头到尾逐个检查元素，直到找到目标元素或遍历完所有元素。

**典型题目**:

- **查找数组中的某个元素**: 给定一个数组和一个目标值，判断目标值是否在数组中。

**解题思路**:

```jsx
function linearSearch(arr, target) {
	for (let i = 0; i < arr.length; i++) {
		if (arr[i] === target) {
			return i; // 返回目标值的索引
		}
	}
	return -1; // 未找到目标值
}
```

## 2. **二分查找 (Binary Search)**

**复杂度**: O(log n)

**特点和场景**:

- 适用于有序数组。
- 高效，适合大规模数据集。
- 需要数据预处理（排序）。

**思路**: 每次将搜索范围缩小一半，直到找到目标元素或搜索范围为空。

**典型题目**:

- **查找有序数组中的某个元素**: 给定一个有序数组和一个目标值，判断目标值是否在数组中。

**解题思路**:

```jsx
function binarySearch(arr, target) {
	let left = 0,
		right = arr.length - 1;
	while (left <= right) {
		const mid = Math.floor((left + right) / 2);
		if (arr[mid] === target) {
			return mid; // 返回目标值的索引
		} else if (arr[mid] < target) {
			left = mid + 1;
		} else {
			right = mid - 1;
		}
	}
	return -1; // 未找到目标值
}
```

## 3. **冒泡排序 (Bubble Sort)**

**复杂度**: O(n^2)

**特点和场景**:

- 简单易实现。
- 适用于小规模数据集。
- 效率较低，不推荐在大规模数据集上使用。

**思路**: 反复交换相邻的元素，如果它们的顺序错误，直到整个数组有序。

**典型题目**:

- **对数组进行排序**: 给定一个数组，对其进行升序排序。

**解题思路**:

```jsx
function bubbleSort(arr) {
	const n = arr.length;
	for (let i = 0; i < n - 1; i++) {
		for (let j = 0; j < n - 1 - i; j++) {
			if (arr[j] > arr[j + 1]) {
				[arr[j], arr[j + 1]] = [arr[j + 1], arr[j]]; // 交换元素
			}
		}
	}
	return arr;
}
```

## 4. **选择排序 (Selection Sort)**

**复杂度**: O(n^2)

**特点和场景**:

- 简单易实现。
- 适用于小规模数据集。
- 每次选择最小（或最大）的元素放到已排序部分的末尾。

**思路**: 反复从未排序部分选择最小元素，将其放到已排序部分的末尾。

**典型题目**:

- **对数组进行排序**: 给定一个数组，对其进行升序排序。

**解题思路**:

```jsx
function selectionSort(arr) {
	const n = arr.length;
	for (let i = 0; i < n - 1; i++) {
		let minIndex = i;
		for (let j = i + 1; j < n; j++) {
			if (arr[j] < arr[minIndex]) {
				minIndex = j;
			}
		}
		[arr[i], arr[minIndex]] = [arr[minIndex], arr[i]]; // 交换元素
	}
	return arr;
}
```

## 5. **插入排序 (Insertion Sort)**

**复杂度**: O(n^2)

**特点和场景**:

- 简单易实现。
- 适用于小规模数据集或部分有序的数据集。
- 在数据量较小时效率较高。

**思路**: 将数组分为已排序和未排序两部分，从未排序部分取出元素插入到已排序部分的适当位置。

**典型题目**:

- **对数组进行排序**: 给定一个数组，对其进行升序排序。

**解题思路**:

```jsx
function insertionSort(arr) {
	const n = arr.length;
	for (let i = 1; i < n; i++) {
		let key = arr[i];
		let j = i - 1;
		while (j >= 0 && arr[j] > key) {
			arr[j + 1] = arr[j];
			j--;
		}
		arr[j + 1] = key;
	}
	return arr;
}
```

## 6. **归并排序 (Merge Sort)**

**复杂度**: O(n log n)

**特点和场景**:

- 稳定排序算法。
- 适用于大规模数据集。
- 需要额外的空间来存储中间结果。

**思路**: 将数组分成两部分，分别排序后再合并。

**典型题目**:

- **对数组进行排序**: 给定一个数组，对其进行升序排序。

**解题思路**:

```jsx
function mergeSort(arr) {
	if (arr.length <= 1) {
		return arr;
	}
	const mid = Math.floor(arr.length / 2);
	const left = mergeSort(arr.slice(0, mid));
	const right = mergeSort(arr.slice(mid));
	return merge(left, right);
}

function merge(left, right) {
	let result = [],
		i = 0,
		j = 0;
	while (i < left.length && j < right.length) {
		if (left[i] < right[j]) {
			result.push(left[i]);
			i++;
		} else {
			result.push(right[j]);
			j++;
		}
	}
	return result.concat(left.slice(i)).concat(right.slice(j));
}
```

## 7. **快速排序 (Quick Sort)**

**复杂度**: 平均O(n log n)，最坏O(n^2)

**特点和场景**:

- 在大多数情况下比归并排序更快。
- 不稳定排序算法。
- 适用于大规模数据集。

**思路**: 选择一个基准元素，将数组分成两部分，一部分小于基准，一部分大于基准，递归排序两部分。

**典型题目**:

- **对数组进行排序**: 给定一个数组，对其进行升序排序。

**解题思路**:

```jsx
function quickSort(arr) {
	if (arr.length <= 1) {
		return arr;
	}
	const pivot = arr[Math.floor(arr.length / 2)];
	const left = arr.filter((x) => x < pivot);
	const right = arr.filter((x) => x > pivot);
	const center = arr.filter((x) => x === pivot);
	return quickSort(left).concat(center).concat(quickSort(right));
}
```

## 8. **广度优先搜索 (BFS)**

**复杂度**: O(V + E) (V是顶点数，E是边数)

**特点和场景**:

- 适用于图的遍历。
- 可以找到最短路径（无权图）。
- 需要使用队列。

**思路**: 从起始节点开始，逐层遍历节点。

**典型题目**:

- **图的遍历**: 给定一个图，从某个节点开始遍历所有节点。

**解题思路**:

```jsx
function bfs(graph, start) {
	let queue = [start];
	let visited = new Set();
	visited.add(start);

	while (queue.length > 0) {
		let node = queue.shift();
		console.log(node); // 处理节点
		for (let neighbor of graph[node]) {
			if (!visited.has(neighbor)) {
				visited.add(neighbor);
				queue.push(neighbor);
			}
		}
	}
}
```

## 9. **深度优先搜索 (DFS)**

**复杂度**: O(V + E) (V是顶点数，E是边数)

**特点和场景**:

- 适用于图的遍历。
- 可以用于检测图中的连通分量、环等。
- 需要使用栈（递归调用栈或显式栈）。

**思路**: 从起始节点开始，尽可能深入地遍历节点，直到无法深入为止，然后回溯。

**典型题目**:

- **图的遍历**: 给定一个图，从某个节点开始遍历所有节点。

**解题思路**:

```jsx
function dfs(graph, start) {
	let stack = [start];
	let visited = new Set();
	visited.add(start);

	while (stack.length > 0) {
		let node = stack.pop();
		console.log(node); // 处理节点
		for (let neighbor of graph[node]) {
			if (!visited.has(neighbor)) {
				visited.add(neighbor);
				stack.push(neighbor);
			}
		}
	}
}
```

## 10. **堆排序 (Heap Sort)**

**复杂度**: O(n log n)

**特点和场景**:

- 不稳定排序算法。
- 适用于大规模数据集。
- 不需要额外的存储空间（原地排序）。

**思路**: 构建一个最大堆，然后逐个取出堆顶元素（最大值），调整堆以保持最大堆性质。

**典型题目**:

- **对数组进行排序**: 给定一个数组，对其进行升序排序。

**解题思路**:

```jsx
function heapSort(arr) {
	const n = arr.length;

	// 构建最大堆
	for (let i = Math.floor(n / 2) - 1; i >= 0; i--) {
		heapify(arr, n, i);
	}

	// 逐个取出堆顶元素
	for (let i = n - 1; i > 0; i--) {
		[arr[0], arr[i]] = [arr[i], arr[0]]; // 交换
		heapify(arr, i, 0);
	}
	return arr;
}

function heapify(arr, n, i) {
	let largest = i;
	let left = 2 * i + 1;
	let right = 2 * i + 2;

	if (left < n && arr[left] > arr[largest]) {
		largest = left;
	}

	if (right < n && arr[right] > arr[largest]) {
		largest = right;
	}

	if (largest !== i) {
		[arr[i], arr[largest]] = [arr[largest], arr[i]]; // 交换
		heapify(arr, n, largest);
	}
}
```

## 11. **动态规划 (Dynamic Programming)**

**复杂度**: 具体问题具体分析，常见复杂度为 O(n^2), O(n\*m) 等

**特点和场景**:

- 适用于有重叠子问题和最优子结构性质的问题。
- 通过存储子问题的解来避免重复计算。
- 常用于求解最优化问题。

**思路**: 将问题分解为子问题，存储子问题的解，逐步构建原问题的解。

**典型题目**:

- **斐波那契数列**: 求第n个斐波那契数。
- **背包问题**: 给定一组物品，每个物品有重量和价值，在总重量不超过限制的情况下，求最大总价值。

**解题思路**:

```jsx
// 斐波那契数列
function fibonacci(n) {
	if (n <= 1) return n;
	let dp = [0, 1];
	for (let i = 2; i <= n; i++) {
		dp[i] = dp[i - 1] + dp[i - 2];
	}
	return dp[n];
}

// 背包问题
function knapsack(weights, values, capacity) {
	const n = weights.length;
	let dp = Array(n + 1)
		.fill()
		.map(() => Array(capacity + 1).fill(0));

	for (let i = 1; i <= n; i++) {
		for (let w = 0; w <= capacity; w++) {
			if (weights[i - 1] <= w) {
				dp[i][w] = Math.max(dp[i - 1][w], dp[i - 1][w - weights[i - 1]] + values[i - 1]);
			} else {
				dp[i][w] = dp[i - 1][w];
			}
		}
	}
	return dp[n][capacity];
}
```

## 12. **贪心算法 (Greedy Algorithm)**

**复杂度**: 具体问题具体分析，常见复杂度为 O(n log n), O(n) 等

**特点和场景**:

- 适用于局部最优能导出全局最优解的问题。
- 每一步选择中都采取当前状态下的最优选择。
- 常用于求解最优化问题。

**思路**: 从问题的某一初始解出发，逐步构建解，每一步都选择当前状态下的局部最优解，最终得到全局最优解。

**典型题目**:

- **活动选择问题**: 给定一组活动，每个活动有开始时间和结束时间，选择尽可能多的互不重叠的活动。
- **最小生成树**: 给定一个带权图，求最小生成树。

**解题思路**:

```jsx
// 活动选择问题
function activitySelection(activities) {
	activities.sort((a, b) => a.end - b.end);
	let selectedActivities = [activities[0]];
	let lastEnd = activities[0].end;

	for (let i = 1; i < activities.length; i++) {
		if (activities[i].start >= lastEnd) {
			selectedActivities.push(activities[i]);
			lastEnd = activities[i].end;
		}
	}
	return selectedActivities;
}

// 最小生成树 (Kruskal 算法)
class UnionFind {
	constructor(size) {
		this.parent = Array(size)
			.fill()
			.map((_, index) => index);
		this.rank = Array(size).fill(0);
	}

	find(x) {
		if (this.parent[x] !== x) {
			this.parent[x] = this.find(this.parent[x]);
		}
		return this.parent[x];
	}

	union(x, y) {
		const rootX = this.find(x);
		const rootY = this.find(y);

		if (rootX !== rootY) {
			if (this.rank[rootX] > this.rank[rootY]) {
				this.parent[rootY] = rootX;
			} else if (this.rank[rootX] < this.rank[rootY]) {
				this.parent[rootX] = rootY;
			} else {
				this.parent[rootY] = rootX;
				this.rank[rootX]++;
			}
		}
	}
}

function kruskal(edges, n) {
	edges.sort((a, b) => a[2] - b[2]);
	const uf = new UnionFind(n);
	const mst = [];

	for (let [u, v, weight] of edges) {
		if (uf.find(u) !== uf.find(v)) {
			uf.union(u, v);
			mst.push([u, v, weight]);
		}
	}
	return mst;
}
```

## 13. **回溯算法 (Backtracking)**

**复杂度**: 具体问题具体分析，常见复杂度为 O(2^n), O(n!) 等

**特点和场景**:

- 适用于需要搜索所有可能解的组合问题。
- 通过递归构建解，并在发现当前解不满足条件时回溯。
- 常用于求解排列、组合、子集等问题。

**思路**: 通过递归构建解，在每一步选择时尝试所有可能的选择，并在发现当前选择不满足条件时回溯。

**典型题目**:

- **N皇后问题**: 在N×N的棋盘上放置N个皇后，使得它们不能互相攻击。
- **全排列**: 给定一个数组，求所有可能的排列。

**解题思路**:

```jsx
// N皇后问题
function solveNQueens(n) {
	const board = Array(n)
		.fill()
		.map(() => Array(n).fill("."));
	const res = [];

	function isValid(board, row, col) {
		for (let i = 0; i < row; i++) {
			if (board[i][col] === "Q") return false;
		}
		for (let i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
			if (board[i][j] === "Q") return false;
		}
		for (let i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
			if (board[i][j] === "Q") return false;
		}
		return true;
	}

	function backtrack(board, row) {
		if (row === n) {
			res.push(board.map((row) => row.join("")));
			return;
		}
		for (let col = 0; col < n; col++) {
			if (isValid(board, row, col)) {
				board[row][col] = "Q";
				backtrack(board, row + 1);
				board[row][col] = ".";
			}
		}
	}

	backtrack(board, 0);
	return res;
}

// 全排列
function permute(nums) {
	const res = [];
	const used = Array(nums.length).fill(false);

	function backtrack(path) {
		if (path.length === nums.length) {
			res.push(Array.from(path));
			return;
		}
		for (let i = 0; i < nums.length; i++) {
			if (used[i]) continue;
			path.push(nums[i]);
			used[i] = true;
			backtrack(path);
			path.pop();
			used[i] = false;
		}
	}

	backtrack([]);
	return res;
}
```

## 14. **分治算法 (Divide and Conquer)**

**复杂度**: 具体问题具体分析，常见复杂度为 O(n log n), O(log n) 等

**特点和场景**:

- 适用于可以将问题分解为子问题并分别解决的情况。
- 通过递归解决子问题并合并结果。
- 常用于排序、搜索、矩阵乘法等问题。

**思路**: 将问题分解为若干子问题，分别解决子问题，然后合并子问题的解得到原问题的解。

**典型题目**:

- **归并排序**: 将数组分成两部分，分别排序后再合并。
- **快速排序**: 选择一个基准元素，将数组分成两部分，一部分小于基准，一部分大于基准，递归排序两部分。

**解题思路**:

```jsx
// 归并排序
function mergeSort(arr) {
	if (arr.length <= 1) {
		return arr;
	}
	const mid = Math.floor(arr.length / 2);
	const left = mergeSort(arr.slice(0, mid));
	const right = mergeSort(arr.slice(mid));
	return merge(left, right);
}

function merge(left, right) {
	let result = [],
		i = 0,
		j = 0;
	while (i < left.length && j < right.length) {
		if (left[i] < right[j]) {
			result.push(left[i]);
			i++;
		} else {
			result.push(right[j]);
			j++;
		}
	}
	return result.concat(left.slice(i)).concat(right.slice(j));
}

// 快速排序
function quickSort(arr) {
	if (arr.length <= 1) {
		return arr;
	}
	const pivot = arr[Math.floor(arr.length / 2)];
	const left = arr.filter((x) => x < pivot);
	const right = arr.filter((x) => x > pivot);
	const center = arr.filter((x) => x === pivot);
	return quickSort(left).concat(center).concat(quickSort(right));
}
```

## 15. **拓扑排序 (Topological Sort)**

**复杂度**: O(V + E) (V是顶点数，E是边数)

**特点和场景**:

- 适用于有向无环图（DAG）。
- 用于解决依赖关系问题，如任务调度。
- 可以通过DFS或Kahn算法实现。

**思路**: 将图中的所有顶点排序，使得对于每条有向边 (u, v)，顶点 u 在顶点 v 之前。

**典型题目**:

- **课程表**: 给定课程和先修课的关系，判断是否可以完成所有课程，并给出课程的学习顺序。

**解题思路**:

```jsx
// 拓扑排序 (Kahn 算法)
function topologicalSort(graph) {
	const inDegree = Array(graph.length).fill(0);
	const queue = [];
	const result = [];

	// 计算每个节点的入度
	for (let i = 0; i < graph.length; i++) {
		for (let neighbor of graph[i]) {
			inDegree[neighbor]++;
		}
	}

	// 将入度为0的节点加入队列
	for (let i = 0; i < inDegree.length; i++) {
		if (inDegree[i] === 0) {
			queue.push(i);
		}
	}

	// 处理队列中的节点
	while (queue.length > 0) {
		const node = queue.shift();
		result.push(node);

		for (let neighbor of graph[node]) {
			inDegree[neighbor]--;
			if (inDegree[neighbor] === 0) {
				queue.push(neighbor);
			}
		}
	}

	// 检查是否存在环
	if (result.length !== graph.length) {
		throw new Error("Graph has a cycle");
	}

	return result;
}
```
