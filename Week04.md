# 呆毛算法小分队 Week 4

* [呆毛算法小分队 Week 4](呆毛算法小分队-Week-4)
	* [010. Regular Expression Matching](#010-regular-expression-matching)
	* [050. Pow(x, n)](#050-pow(x,-n))
	* [023. Merge k Sorted Lists](#023-merge-k-sorted-lists)
	* [160. Intersection of Two Linked Lists](#160-intersection-of-two-linked-lists)
	* [027. Remove Element](#027-remove-element)
	* [139. Word Break](#139-word-break)
	* [011. Container With Most Water](#011-container-with-most-water)
	* [198. House Robber](#198-house-robber)
	* [283. Move Zeroes](#283-move-zeroes)
	* [217. Contains Duplicate](#217-contains-duplicate)
	* [026. Remove Duplicates from Sorted Array](#026-remove-duplicates-from-sorted-array)
	* [189. Rotate Array](#189-rotate-array)


## [010. Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/)

> Implement regular expression matching with support for `'.'` and `'*'`.

@Laijie

﻿当星星和点点连在一起简直报复社会。
年幼无知的时候我也是试过hardcode这道题的。（系统提示：你的队友已阵亡）

**Solution 1: BFS**

虽然已经不算Brute-Force了……
100ms. Time $O(2^n)$, Space $O(n)$. 已然是NP问题。

* Recursive 
  ```c++
  isMatch(string &s, string &p, int i, int j)
  ```

* 下一个字符不是`*`，判断当前`s[i]`和`p[j]`是否匹配，并递归判断后续字符串是否匹配
  ```c++
  return (s[i]==p[j] || p[j]=='.') && isMatchBF(s, p, i+1, j+1)
  ```
* 下一个字符是`*`，判断匹配一个或匹配两个，以及后续字符串
  ```c++
  return isMatchBF(s, p, i, j+2) 
         || (s[i]==p[j] || p[j]=='.') && isMatchBF(s, p, i+1, j);
  ```


**Solution 2: DP**

4ms. Time $O(n^2)$, Space $O(n^2)$.
所以才说DP拯救世界嘻嘻。

> '.' Matches any single character.
> '*' Matches zero or more of the preceding element.
```
Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "a*") → true
isMatch("aa", ".*") → true
isMatch("abcdefghijklmn", ".*") → true
         ..............
isMatch("aab", "c*a*b") → true
```

s: abcde
p: abcde~~f*~~

s: aaa
p: a*

dp[0][0] = 1;
dp[0][2] = 1;
dp[1][2] = dp[0][2] && s[0]和p[1]匹配

* Subproblem: `bool dp[i][j]` 表示`s[0, i)`匹配`p[0, j)`
* Base: `dp[0][0] = true;`
        `dp[i][0] = false;`
* Recursion:
  * `dp[i][j] = dp[i-1][j-1]`，如果`p[j-1]`不是星星，并且`s[i-1]`和`p[j-1]`匹配。
  * `dp[i][j] = dp[i][j-2]`，如果`p[j-1]`是星星，并且星星匹配了0个字符。pattern: x*
  * `dp[i][j] = dp[i-1][j]`，如果`p[j-1]`是星星，匹配了至少1个字符，并且星星的pattern匹配`s[i-1]`，或者星星的pattern是点点。

简而言之：
```c++
for (i)
  for (j)
    dp[i][j] = p[j-1] == '*'
           ? (dp[i][j-2] || dp[i-1][j] && ( s[i-1]==p[j-2] || p[j-2]=='.' ))
           : (dp[i-1][j-1] && (s[i-1]==p[j-1] || p[j-1]=='.'));
```

**Solution 3:**

正确姿势Automation~


a+cda
ab

a+c
a+c(db)*
a+c(daa+c)*

扩展阅读：
![汪](https://upload.wikimedia.org/wikipedia/commons/thumb/2/2a/CPT-FSM-abcd.svg/489px-CPT-FSM-abcd.svg.png)
* [Deterministic Finite Automaton](https://en.wikipedia.org/wiki/Deterministic_finite_automaton)
* [Nondeterministic Finite Automaton](https://en.wikipedia.org/wiki/Nondeterministic_finite_automaton)
* [Thompson's Construction Algorithm](https://en.wikipedia.org/wiki/Thompson%27s_construction)  (Convert Regex to NFA)
* [Convert NFA to DFA](http://web.cecs.pdx.edu/~harry/compilers/slides/LexicalPart3.pdf) 哈哈哈哈哈哈哈这个PSU的教授名字叫哈利波特


## [050. Pow(x, n)](https://leetcode.com/problems/powx-n/)

>Implement pow(_x_, _n_).

@Weichen

* Naive - multiply x `n times`
* think of shift operations - `log(n)`
    * eg. 10 = `1010` = $2^3 + 2^1$ -> $x^{10} = x^{2^3} * x^{2^1}$
    * right shift `n`, calculate $x^{2^i}$, multiply it to the result every time there is 1 in the lowest digit of shifted `n`
* Be careful when `n < 0` and `n = 0`

```python
class Solution(object):
    def myPow(self, x, n):
        """
        :type x: float
        :type n: int
        :rtype: float
        """
        
        if n == 0: return 1
        
        res = 1
        absN = abs(n)
        while absN > 0:
            if absN & 1 == 1:
                res *= x
            absN >>= 1
            x *= x
        
        return res if n > 0 else 1/res
```

## [023. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

>Merge _k_ sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

@Chris

哼哼~ k枚列表，每个长度m

**Solution 1: Min Heap**

Time $O(mk\text{ log}(mk))$, Space $O(mk)$

堆排序：建堆O(n), pop O(logn)
Priority Queue的实现方式之一。

1. 暴力把LinkedList的所有item复制到到vector里。O(mk)
2. 暴力建堆。O(mk)
3. 依次pop到新的LinkedList里。O(mklogmk)

**Solution 2: 依次Merge**

看起来像是Time $O(mk\text{ log}(k))$, Space $O(1)$
8个list ----(merge一轮)---> 4个 -> 2 -> 1
mk + 2*m(k/2) + 4*m(k/4) + .... k * m
↑ logk
* 超时风险极大
```
for(i = 0:2:n)
   merge();
```
**Solution 3: Merge + MinHeap**

Time $O(mk\text{ log}k)$, Space $O(k)$

* 取所有List的第一个数，丢进Heap排一遍，再插到result中。O(klogk)
* 重复以上，m次。

## [160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

> Write a program to find the node at which the intersection of two singly linked lists begins.
> ```
> A:          a1 → a2
>                   ↘
>                     c1 → c2 → c3
>                   ↗            
> B:     b1 → b2 → b3
> ```

@Cecilia

哼哼，我们来假设分开部分长度是m，合并部分长度是n

**Solution 0:**

1. 遍历一遍求长度lenA, lenB
2. 判断结尾不相等就返回nullptr
3. lenA>lenB，把headA往后移(lenA-lenB)，保证headA headB后面长度相等
4. 向后遍历，返回第一个相等的节点

**Solution 1:** Time: 2 * O(m + n), Space: O(1)

1. pa, pb两个指针，从头开始分别遍历.
2. pa到达listA结尾后继续遍历listB，pb到达listB结尾后继续遍历listA.
3. 返回第一个相等的节点.


证明：
* 假设不重合的部分的长度是A和B，重合的长度是C
A+C+B = B+C+A

```c++
ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
    ListNode *pa = headA, *pb = headB;
    while (pa != pb) { 
        pa = pa ? pa->next : headB;
        pb = pb ? pb->next : headA;
    }
    return pa;
}
```

**Solution 2:** Time: O(m), Space: O(1)
哈哈哈哈下班往家溜达的时候忽然想起来小天才可爱得不要命的Find Cycle in LinkedList，我们来愉快地玩耍一下wwww
* 把访问过的节点都指向一个X节点，这样，遇到的第一个已经指向X节点的就是交点

（玩儿坏了公司数据表的PO主哭着被fire了

```c++
ListNode *getIntersectionNode(ListNode *hA, ListNode *hB) {
    ListNode abyss(999), *lastA, *lastB;
    while ((lastA = hA) && (lastB = hB)) {
        if (hA->next == &abyss) return hA;
        if (hB->next == &abyss) return hB;
        hA = hA->next;
        hB = hB->next;
        lastA->next = lastB->next = &abyss;
    }
    return nullptr;
}
```

……它不让我这么玩儿
```
ERROR: linked structure was modified.
```

## [027. Remove Element](https://leetcode.com/problems/remove-element/)

>
>Given an array and a value, remove all instances of that value in place and return the new length.
>
>Do not allocate extra space for another array, you must do this in place with constant memory.
>
>The order of elements can be changed. It doesn't matter what you leave beyond the new length.
>
>**Example:**  
Given input array _nums_ = `[3,2,2,3]`, _val_ = `3`
>Your function should return length = 2, with the first two elements of _nums_
being 2.
>1. Try two pointers.
>2. Did you use the property of "the order of elements can be changed"?
>3. What happens when the elements to remove are rare?

@WenXin


* 双指针
 p - 头指针 q - 尾指针 
 * 规则
 q 始终指向从数组尾部起第一个值不为`val`的结点
 p 从头开始遍历
``` 
原数组里面有m个要remove，n不用

swap: m
1110100
i     j

ij
swap: n
[3,2,2,3]
 2 

```
```C++
int removeElement(vector<int>& nums, int val) {
	int p = 0, q = nums.size()-1;
	while(q >= p && nums[q] == val)
		q--; //初始化Q
	while(q >= p){
		if(nums[p] == val){
			swap(nums[p], nums[q]);
			while(q >= p && nums[q] == val)
				q--;
		}
		++p;
	}
	// nums.resize(++q);
	// return nums.size();
	return q+1;
}
```
```c++
int removeElement(vector<int>& nums, int val) {
    int len = nums.size();
    for(int i=0; i<len; i++)
        while(nums[i] == val && i<len)
            swap(nums[i], nums[--len]);
    return len;
}
```

```python
        newIndex = 0
        
        for i, num in enumerate(nums):
            if num != val:
                nums[newIndex] = num
                newIndex += 1
        
        return newIndex
```


## [139. Word Break](https://leetcode.com/problems/word-break/)

>
>Given a string _s_ and a dictionary of words _dict_, determine if _s_ can be segmented into a space-separated sequence of one or more dictionary words.

@Xinyi

* 函数参数：
  * @string s: leetcode
  * @unordered_set<string>& wordDict: ['leet', 'code']

leetcode -> leet code

* 可以用BFS/DFS搜索（NP时间）
  * 从i开始，遍历0~n，找到所有以i开头的word
    `-----i---X----X--X---` 
* 也可以用DP解法，子问题分割：
  * 若`s[0~j]`和`s[j~i]`都可以被space分割，则`s[0~i]`可以被space分割
  `--X----X--X---i------`
  * `f[i]`代表截止到第i个字符(`s[0~i]`)可以被space分割
  * `f[i] = f[j] && wordDict.count(s.substr(j, i - j));`


```c++
class Solution {
public:
    bool wordBreak(string s, unordered_set<string>& wordDict) {
        const int n = s.length();
        // vector<bool> f(n + 1, false);
        bool f[n]; 
        f[0] = true;
        for (int i = 1; i <= n; i ++) { // break word at i-th letter
            for (int j = 0; j < i; j ++) {  // break word at j-th letter 
                f[i] =  f[j] && wordDict.count(s.substr(j, i - j));
                if (f[i]) break;
            }
        }
        return f[n];
    }
};

```
	

## [011. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

>Given _n_ non-negative integers _a1_, _a2_, ..., _an_, where each represents a point at coordinate (_i_, _ai_). _n_ vertical lines are drawn such that the two endpoints of line _i_ is at (_i_, _ai_) and (_i_, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.
>
>Note: You may not slant the container.

@Laijie

我们来从两边往里挤wwww
Greedy，核心思想：遍历一遍，用hMax来记录遇到过的最大值，O(n)

```
X       Y  4
  X     Y  6
  X Y      2
  
    |   |
  | |   |
| | | | |
---------
X       Y
```

1. Brute-force: O(n^2)

2. X在最左，Y在最右，算一遍
   * if X比右边的小，移动X
   * else if Y比左边的小，移动Y
   * else 移动小的那个
   
3. X在最左，Y在右边
   * 判断X和Y哪个小，谁小挪谁
   * 思路：每次把宽度缩小1

## [198. House Robber](https://leetcode.com/problems/house-robber/)

>
>You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.
>
>Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.

@Weichen

* Can be solved by DP
* Subproblem. `s[i]`: max with picking the ith item
* Time: O(n) - ~~two passes~~; Space: O(n)
* Optimize: use constant space to store the last 3 items

```python
class Solution(object):
    def rob(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
                
        n = len(nums)
        s = [0] * n
        
        if n == 0: return 0
        if n == 1 or n == 2: return max(nums)
        
        # base case
        s[0] = nums[0]
        s[1] = nums[1]
        s[2] = nums[2] + nums[0]
        
        for i in range(3, n):
            # recursive relation
            s[i] = max(s[i-2]+nums[i], s[i-3]+nums[i])
        
        # final solution
        # return max(s) 
        return max(s[n-1], s[n-2])
```

```
        # optimize space
        s = [0] * 3
        
        # base case
        s[0] = nums[0]
        s[1] = nums[1]
        s[2] = nums[2] + nums[0]
        
        maxNum = max(s)
        
        # recursive relation
        for i in range(3, n):
            s += [max(s[1]+nums[i], s.pop(0)+nums[i])]
            maxNum = max(s[2], maxNum)

        # final result
        return maxNum
```

http://www.bilibili.com/video/av53296/index_2.html#page=12

## [283. Move Zeroes](https://leetcode.com/problems/move-zeroes/)

>
>Given an array `nums`, write a function to move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.
>
>For example, given `nums = [0, 1, 0, 3, 12]`, after calling your function, `nums` should be `[1, 3, 12, 0, 0]`.
>
>**Note**:
>1. You must do this **in-place** without making a copy of the array.
>2. Minimize the total number of operations.


@Chris

po主首先试用了erase所有zero然后在vector后面补0的无脑巨污算法...但是...他竟然击败了20%...


```

class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        // first we set two pointers from the end
        for(auto iter = nums.end() - 1; iter != nums.begin() - 1; --iter)
        {
        	if(*iter == 0)
        	{          	
        	    nums.erase(iter);
				nums.push_back(0);
        	}

        }
    }
};


```

好...让我们撸起袖子...认真起来

```
```

## [217. Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)

>Given an array of integers, find if the array contains any duplicates. Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.

@Cecilia

**Sort:** Time O(nlogn), Space O(1)
* 排序，判断相邻两个是否相等。如果要求不破坏原始数组，需要Space O(n)

**HashMap:** Time O(n), Space O(n)
* `for (int n : nums) if (++map[n] > 1) return true;`

**HashSet:** Time O(n), Space O(n)
* 因为不需要记录有多少个重复，可以把map直接换成set.
* `if (set.find(n) != set.end()) return true;`

**STL一行大法:** Time O(n), Space O(n)
* Range Constructor：`set(Iterator first, Iterator last)`
```c++
return set<int>(nums.begin(), nums.end()).size() < nums.size();
```

## [026. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)[.](https://youtu.be/gIuz-5MOZ28)

>Given a sorted array, remove the duplicates in place such that each element appear only _once_ and return the new length.
>
>Do not allocate extra space for another array, you must do this in place with constant memory.
>
>For example,
>Given input array _nums_ = `[1,1,2]`,
>Your function should return length = `2`, with the first two elements of _nums_ being `1` and `2` respectively. It doesn't matter what you leave beyond the new length.

@WenXin


提到双指针一定会想到的最初
```c++
int removeDuplicates(vector<int>& nums) {
	int p = 0, q = 0;
	if(!nums.size())
		return 0;
	while(q < nums.size())
		nums[q] == nums[p] ? q++ : nums[++p] = nums[q++];
	nums.resize(++p);
	return p;
}

```
http://www.bilibili.com/video/av53296/index_2.html#page=16
## [189. Rotate Array](https://leetcode.com/problems/rotate-array/)

>Rotate an array of _n_ elements to the right by _k_ steps.
>For example, with _n_ = 7 and _k_ = 3, the array `[1,2,3,4,5,6,7]` is rotated to `[5,6,7,1,2,3,4]`.
>
>**Note:**  
Try to come up as many solutions as you can, there are at least 3 different ways to solve this problem.
>
>**Hint:**  
Could you do it in-place with O(1) extra space?
>Related problem: [Reverse Words in a String II](/problems/reverse-words-in-a-string-ii/)

@Xinyi
http://www.bilibili.com/video/av53296/index_18.html
* 在discuss里看到一个美丽无比的解法。。。让其他一切swap的解法都相形见拙。。。。。
	* Step 1: rotate the whole array
	  [7,6,5,4,3,2,1]
	* Step 2: rotate the first `k` elements
	  [5,6,7,4,3,2,1]
	* Step 3: rotate the last `n - k` elements
	  [5,6,7,1,2,3,4]

```C++
class Solution {
public:
    void reverse(vector<int>& nums, int start, int end) {
        for(int i = start; i <= (end + start) / 2; i++) {
            int tmp = nums[i];
            nums[i] = nums[end - i + start];
            nums[end - i + start] = tmp;
        }    
    }
    
    void rotate(vector<int>& nums, int k) {
        int n = nums.size();
        k = k % n;
        if(k == 0) return;
        
        reverse(nums, 0, n - 1);
        reverse(nums, 0, k - 1);
        reverse(nums, k, n - 1);
    }
};

```

```
reverse(nums.begin(), nums.end());
int n[6];
reverse(n, n+6);

swap() {
   move()
}
```

