## 题

在一个n*n的棋盘上有n个皇后，每个皇后可以米字型往八个方向走任意步，并且攻击在这个范围内的其他皇后。请问给定任意n，皇后们不打架的放法有哪些？ return如下结构：

ii的话不用画图，统计有多少个放法，return一个integer就行了。是i的阉割版。
```
当n = 4, 返回：

[
 [".Q..",  // Solution 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // Solution 2
  "Q...",
  "...Q",
  ".Q.."]
]

```

## 思路

首先这个题是一个permutation。n个皇后不能往前后左右走 === 长度为n的array里放n个数，每个数只出现一次的全排列。（把index看成行，数看成列。一个index只塞一个数不就是一行只放一个数嘛。一个数用一遍就是同列没有第二个数嘛）。

这样前后左右就解决了。那斜线四个方向怎么办呢？根据坐标我们发现，同在y = x 这条线上的点，自己的横纵坐标之**和**是一个定值，等于x + y。同在y = -x这条线上的点，自己的横纵坐标之**差**是一个定值，也是x + y。
所以对于一个subset，如果里面有两个点a,b, 如果a +/- index of a === b +/- index of b，说明会出现斜线攻击。排除掉这个情况。


**注意一定是自己加减自己，对方也是自己加减自己，然后比是否相等**

最后做图。


## 代码

```

const nQueens = n => {
	if (n === 1) return [['Q']];
		const validate = subset => {
		for(let i = 0; i < subset.length; i++) {
			for(let j = 1; j < subset.length; j++) {
				if(i === j) continue;
				if(subset[j] - j === subset[i] - i) return false;
				if(subset[j] + j === subset[i] + i) return false;
			}
		}
		return true; 
	}
 
//撸一轮深搜，找到n个数可能的全排列，用validate过滤掉斜线攻击的情况
	const findPermutation = n => {
		let results = [];
		const helper = (n, results, map, subset) => {
			if(subset.length === n && validate(subset)) results.push(subset.slice());
			for(let i = 0; i < n; i++) {
				if(map[i]) continue;
				subset.push(i);
				map[i] = true;
				helper(n, results, map, subset);
				delete map[i];
				subset.pop();
			}
		}
		helper(n, results, {}, []);
		return results;
	}
	let results = findPermutation(n)
	
	const drawGraph = results => {
		let solution = [];
		for(let i = 0; i < results.length; i++) {
			let subSolution = [];
			for(let j = 0; j < results[0].length; j++) {
				let str = "";
				for(let k = 0; k < results[0].length; k++) {
					if(k === results[i][j]) str += "Q";
					else str += ".";	
				}
				subSolution.push(str);
			}
			solution.push(subSolution);
		}
		return solution;
	}
	return drawGraph(results);
};


```
