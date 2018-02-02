## 题

说给你一个array，返回它的全排列。i没有duplication，ii有duplication

## 思路

考虑这么一个情况： [1, 2', 2''] 

答案应该是：[1, 2', 2''], [2', 2'', 1], [2', 1, 2'']

我们希望2'永远出现在2''之前。这样就是有序的。只要不跳过2'去取2''，就不会重复。

[1, 2''] <--2''压根就不应该进来，这种情况要排除掉的。

所以呢，对数组进来之前进行排序，确保一样的数挨着了。然后用一个if判断过滤掉这种情况。

对于visited/ map这样数组的维护，什么时候应该加，什么时候应该减？

**回溯是在递归前后做镜像操作。**

所以递归之后减，递归之前加。


## 代码 for permutations-ii

```
var permuteUnique = function(nums) {
    let results = [];
	if(!nums || nums.length === 0) return results;
	nums.sort((a, b) => a - b);
	
	const helper = (results, nums, subset, visited) => {
		if(subset.length === nums.length) results.push(subset.slice());

		for(let i = 0; i < nums.length; i++) {
			if(visited[i] === true) continue;
			if(i !== 0 && nums[i] === nums[i - 1] && !visited[i - 1]) continue; // !visited[i - 1] 就是说2'被跳过了
      
      subset.push(nums[i]);
			visited[i] = true;
			helper(results, nums, subset, visited);
			subset.pop();
			delete visited[i];
		}
	}
	helper(results, nums, [], {})
	return results;
};
```
