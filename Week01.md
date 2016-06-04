# 呆毛算法小分队 Week 1

* [呆毛算法小分队 Week 1](#呆毛算法小分队-week-1)
    * [001. Two Sum](#001-two-sum)
    * [002. Add Two Numbers](#002-add-two-numbers)
    * [151. Reverse Words in a String](#151-reverse-words-in-a-string)
    * [004. Median of Two Sorted Arrays](#004-median-of-two-sorted-arrays)
    * [136. Single number](#136-single-number)
    * [003. Longest Substring Without Repeating Characters](#003-longest-substring-without-repeating-characters)
    * [292. Nim Game](#292-nim-game)
    * [006. ZigZag Conversion](#006-zigzag-conversion)
    * [146. LRU Cache](#146-lru-cache)

##  [001. Two Sum](https://leetcode.com/problems/two-sum/)
> Given an array of integers, return indices of the two numbers such that they add up to a specific target.
You may assume that each input would have exactly one solution.

  * hashtable. O(n) space O(n)
    * 先建立哈希表：key为数值，value为在数组中的index
    * 遍历数组，若哈希表中存在target - nums[i], 解决战斗
  * sort & two pointers. O(nlgn) space O(1)
    * 排序后，一头一尾两指针撸一遍数组=w=
    * 汪: 本题要求返回角标，使用排序法需预存角标，Space其实是O(n) （抛开空间复杂度谈时间复杂度都是耍流氓
solution 1
```
vector<int> originalNums = nums
originalNums里面找两个位置不同的*front和*end
```
击败了99%的用户

  * `unordered_map` VS `map` 
    * unordered map: 哈希表，无序，随机访问O(1)，处理稀疏数据时请极其慎重使用 --> array O(1) hash=1 hash=2 hash=10000
    * (key, value) --> hash的地址  当地址冲突时 链表
    * map: 红黑树，有序，随机访问O(logn)

 **3-Sum**

* **xinyi的算法. O(n^2), Space O(n)**
    * 给nums排序
    * 建个哈希表dict存每个数出现了几次，再建一个pos存每个数第一次出现的位置
    * 把nums里重复的数删掉 
    * 两重循环遍历数组：若target - nums[i] - nums[j]出现在dict里则加入最终答案
    * 为防止添加重复答案，需满足i <= j <= pos
    * [target - nums[i] - nums[j]]
    *  #以上算法击败了2.93%的用户。。。欲语泪先流。。。


 **k-Sum (k >= 2)**
* **3-Sum: 排序双指针. Time O(n2) Space O(1)**
	1. 排序后，定义角标 a < b < c
	2. a指针遍历数组，b c在a右侧做two pointer. O(n2)
	3. 移动指针时，跳过相同数字，即可避免重复
	4. 对a进行边界判定可以节约80%运算
		* A ∈ (target - 2*n, target / 3)
		* 需要讨论二分法找每层边界的必要性，虽然不影响最坏情况复杂度，但似乎可以把平均复杂度降个logn？
  * 以上可作为**k-Sum**通用解法，使用k-1层循环，底层是双指针. O(n^(k-1))

```
Time O(n2), Space O(1)
要求unique答案，同一个答案中三个数可以相同   (0,0,0)
vec<vec<>> func(int* nums) {
    // i j k
	sort();
	for (int i=0; nums[i] <= 0; i++) {
	   int j = i+1; k = nums.size() - 1;
	   while (j < k)
	   {
	     int a = ..., b = ..., c = ...;
	   	 else if (找到惹) {
		     ans.push_back(solution);
			 while (nums[j] == b) ++j;
			 while (nums[k] == c) --k;
		 }
	   }
	}
} 
//holy fuck
```
k-Sum的lower bound是O(n^(k-1))

```c++
    // Solution: 4Sum = n * 3sum = n * n * 2sum
    // Optimization: add boundary conditions, runtime: 128ms -> 16ms
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> ans;
        int n = nums.size();
        // 4Sum
        for(int a=0; a<n-3; ++a) {
            if (a>0 && nums[a]==nums[a-1]) continue;
            if (nums[a] + nums[a+1] + nums[a+2] + nums[a+3] > target) break;
            if (nums[a] + nums[n-1] + nums[n-2] + nums[n-3] < target) continue;
            // 3Sum
            for(int b=a+1; b<n-2; ++b) {
                if (b>a+1 && nums[b]==nums[b-1]) continue;
                if (nums[a] + nums[b] + nums[b+1] + nums[b+2] > target) break;
                if (nums[a] + nums[b] + nums[n-1] + nums[n-2] < target) continue;
                // 2Sum
                int c = b+1, d = n-1;
                while (c < d) {
                    int sum = nums[a] + nums[b] + nums[c] + nums[d];
                    if (sum < target) { ++c; continue; }
                    if (sum > target) { --d; continue; }
                    ans.push_back({nums[a], nums[b], nums[c], nums[d]});
                    do { ++c; } while (nums[c] == nums[c-1]);
                    do { --d; } while (nums[d] == nums[d+1]);
                }
            }
        }
        return ans;
    }
```

        X              Y                Z
    --------|---------------------|------------
    当前做法：O(X) + O(Y)*       --> O(n)
    二分查找：O(logn) + O(Y)*    --> O(logn)  --> depends on input

Related:
[015. 3Sum](https://leetcode.com/problems/3sum/)
[016. 3Sum Closest](https://leetcode.com/problems/3sum-closest/) 
[018. 4Sum](https://leetcode.com/problems/4sum/)

## [002. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

> You are given two linked lists representing two non-negative numbers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

> `Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)`
> `Output: 7 -> 0 -> 8`

 Time O(n), Space O(1)
* 思路同mergesort. 用bool记录进位
* 可以用两个指针：一个dummyhead用来记录开头，用于最终找返回值；一个curr记录当前算加法的位置
* 代码层面优化：
```
while(A || B || 进位){
   if(A)
   if(B)
}
```
可写为
```
while(A&&B){ ... }
while(A){ ... }
while(B){ ... }
```
省了2n个贵得要命的if判断，顿时觉得自己有钱了ヽ(●´ω｀●)ﾉ
* 代码层面优化之二：
```
while(A||B) {
    x = A != NULL ? A : 0; //反正就是快  //喵酱说 跟 if else 不同
    y = B != NULL ? B : 0;
    blabla
}

```

这样写代码好像好看很多，不过跟用if判断并木有本质区别=w=

```
switch(a)
1:  ....
2:  ....
3:  ....

if (a == 1)
else if (a == 2)
else if ....
```

[43. Multiply Strings](https://leetcode.com/problems/multiply-strings/)

## [151. Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)

> Given an input string, reverse the string word by word.

* xinyi扯淡：人畜无害的一道题=w= 表示我在微软 & Amazon第一轮碰到过，有同学在fb第一轮也碰到过

**(1) O(n), space O(1)**

  * 从头到尾撸一遍，去掉头尾空格和句子中多余的空格，我的判断方法是若下一个字符是空格则去掉当前空格
  * 写一个helper函数：helper(string &s, int start, int end)， 翻转start～end部分的字符串
  * 先从头到尾翻转一遍
  * 再从头开始遍历，一旦发现一个单词就用helper进行翻转
  * 算法很简单，实现的时候去掉头尾＋多余空格特别蛋疼很容易写错，主要问题是for循环遍历的时候要考虑到一旦抹去某个空格，index和字符串长度都会变

**(2) One pass solution. Time O(n), Space O(1)**

1. for 0 ~ n
   * 记录start index
   * 处理空格，同时记录end index
   * 当遇到词尾，即s[end]是空格时
     * 翻转词 reverse(start, end)
     * start = end + 1;
2. 删除被移到句尾的空格们 `s.erase(s+end);` 
3. 翻转整个句子`reverse(s, s+s.length());` 

`reverse(vec.begin(),vec.end()); int a[10]; reverse(a, a+10);`

```
func() {// "   abc" "abc bc"
        //cur          cur i end
	int cur = 0, size;
	for (int i=0; i<size; ++i) {
		if (s[i]==' ') continue;
		if (cur) s[cur++] = ' ';
		int end = i; // end记录word结尾
		while (end<size && s[end]!=' ') 
			s[cur++] = s[end++];
		reverse(s.begin()+cur-(end-i), s.begin()+cur);
		i = end;
	}
	s.erase(s.begin()+cur);
	reverse(s.begin(), s.end());
}
```

**(3) StringStream. Time O(n), Space O(n)**

虽然我不是constant space但是我短呀ww
```
istringstream ss(s)
ss >> s;
string tmp;
while (ss >> tmp) s = tmp + ' ' + s;
```
```
2. string和int的类型转换：
std::stringstream ss;
int a = 100;
ss << a
ss.str()      //“100"
int num;
ss >> num;   //num = 100
ss.str(“”);      //把ss清空
```


简直爱不释手ww 比如有一道用字符串建树和把树转换成字符串的题

## [004. Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

**(1) Brute force. O(m+n)**
  1. 把这个思路讲一遍.
  2. 跟面试官说我觉得linear time弱爆了我们来试试binary search吧.
（论装逼犯的正确答题姿势

O(log(m+n))

**(2) Binary search. O(log min(m,n))**

证明：

  * 把两个list分成Set left和Set right：
    ```
    nums1:   A[0] ~ A[i-1]  |  A[i] ~ A[m]
    nums2:   B[0] ~ B[j-1]  |  B[j] ~ B[n]
    ```
  * 要保证左右总数相等，这样mid就在分界线上
   ```
   int left = 0, right = m;
   while (left < right) {
       i = (left + right) >> 1;
	   .....
   }
   ```
  	* 要求：`i + j = (m + n) / 2` 
  	* 求得：`j = (m + n) / 2 - i`
  * 要保证左边set中的所有数小于右边set中的所有数，因此要求：
  	* `A[i-1] <= B[j]`
  	* `B[j-1] <= A[i]`
  * 按照以上条件，对A做binary search
  * 得到结果后，中位数在分界线上：
  	* 总数是奇数，`mid = max(A[i-1], B[j-1])`
  	* 总数是偶数，`mid = ( max(A[i-1], B[j-1]) + min(A[i], B[j])) / 2`


## [136. Single number](https://leetcode.com/problems/single-number/) 

Time O(n), Space O(1)
  * `A^B^A=B`

**[137. Single number II](https://leetcode.com/problems/single-number-ii/)**

> Given an array of integers, every element appears three times except for one. Find that single one.

其它数都出现了3次，找到只出现1次的那个.
k次的通用解法. Time O(n), Space O(1)：
* 用bitsum[32]存储每一位上所有数的bit sum
     * 方法一
		```
		while(mask != 0) {
			bitsum[bit++] += ((nums[i] & mask) == 0) ? 0 : 1;	
			mask <<= 1;
		}
		```		
	 * 方法二
		 ```
		for(int j = 0; j < 32; j++) {
				bitsum[bit++] += nums[i] & 1;
				nums[i] >>= 1;
		}
		 ```
     * 前者快很多
  
* 要找的数在第i位的值 = bitsum[i] % k;

可能会涉及C++运算符优先级问题：
```
a = 1;
a = a << 1 + 1;   //4
a = (a << 1) + 1;   //3
```
```
`if (!(k&1))`

++, *, +, <<, ==, &, |, &&, ||, ? :
完整优先级列表
    括号成员第一; //括号运算符[]() 成员运算符. ->
	
	！ is here right?
　　全体单目第二; //所有的单目运算符比如++ -- +(正) -(负) 指针运算*&
　　乘除余三,加减四; //这个"余"是指取余运算即%
　　移位五，关系六; //移位运算符：<< >> ，关系：> < >= <= 等
　　等于(与)不等排第七; //即== !=
　　位与异或和位或; //这几个都是位运算: 位与(&)异或(^)位或(|) 
　　"三分天下"八九十; 
　　逻辑或跟与; //逻辑运算符:|| 和 &&
　　十二和十一; //注意顺序:优先级(||) 底于 优先级(&&) 
　　条件高于赋值, //三目运算符优先级排到 13 位只比赋值运算符和","高//需要注意的是赋值运算符很多！
　　逗号运算级最低! //逗号运算符优先级最低：
 http://www.cppblog.com/aqazero/archive/2006/06/08/8284.html
```

**[260. Single number III](https://leetcode.com/problems/single-number-iii/)**

其它所有数都出现了2次，找到两个只出现1次的数.
Time O(n), Space O(1)
```
   int a = 0, ab = 0;
   for (n : nums) ab ^= n;          // 计算A^B
   int x = (ab&(ab-1))^ab;          // 计算A和B不同的某一位（最低位）
   // 1001100   ab
   // 1001011   ab-1
   // 1001000   ab&(ab-1) 把ab最后一个1变成0
   // 0000100   x
   for (n: nums) if (n&x) a ^= n;  // 计算A
   return {a, ab^a};
```

## [003. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
**记录每个字母上一次出现的位置**
Time O(n), Space O(1)
* offset：记录没有重复字符的最靠前的位置
* pos[i]：记录字符i最后出现的角标，初始-1
* 每当遇到重复字符，更新offset和maxLength

## [292. Nim Game](https://leetcode.com/problems/nim-game/)
4的倍数会死 (..•˘_˘•..)
* 剩1~3块可以一波带走.
* 剩下4块就输定了，不管拿走1/2/3块，都会剩下3/2/1块让对面一波带走.
* 因此当总数不是4的倍数时，你可以各种姿势留给对面4的倍数块砖.
* 当总数是4的倍数时，对面也会各种姿势留给你4的倍数砖.

同理，当一次可以拿走1至k块儿砖的时候，遇到k+1的倍数的输~

//(…•˘_˘•…)
## [006. ZigZag Conversion](https://leetcode.com/problems/zigzag-conversion/)
```
len = n*2-2

0           6           12          18        -> 0, len, 2*len, 3*len ...
  1       5   7       11  13      17  19      -> 1, len-1, len+1, 2*len-1, 2*len+1
     2   4       8   10      14  16      20   -> 2, len-2, len+2, ...
       3           9           15             -> n, n + len, n + 2*len...
```
Refer to
```
AA DOVLYA REWSNFER
```

## [146. LRU Cache](https://leetcode.com/problems/lru-cache/)

Analyze
  * Find required item in O(1): `hashmap`
  * Update: remove item and insert it to the front in O(1): `linked list`
  * Remove the last item in O(1): `doubly linked list`

So the solution will be using `doubly linked list` + `hashmap<key, listNode>`. all operations are O(1). Space O(n). perfect ヽ(●´ω｀●)ﾉ

P.S. dont use linkedlist in STL it's super super slow! STL list -> 172ms
手写双链表 72ms 击败了97%的主人哟


https://github.com/cecilia-cx/leetcode/blob/master/146_LRUCache.cpp

http://www.cnblogs.com/dolphin0520/p/3741519.html

```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> map;
        for (int i=0; i<nums.size(); ++i)
            map.insert({nums[i], i});
        sort(nums.begin(), nums.end());
        int *front = &nums[0];
        int *end = &nums[nums.size() - 1];
        while (front != end) {
            int sum = *front + *end;
            if (sum < target) ++front;
            else if (sum > target) --end;
            else break;
        }
        auto const &frontIndex = map.find(*front);
        auto const &endIndex = map.find(*end);
        return {frontIndex->second, endIndex->second};
    }
};
```
