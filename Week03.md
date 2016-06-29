# 呆毛算法小分队 Week 3

* [呆毛算法小分队 Week 3](呆毛算法小分队-Week-3)
	* [014. Longest Common Prefix](#014-longest-common-prefix)
	* [066. Plus One](#066-plus-one)
	* [070. Climbing Stairs](#070-climbing-stairs)
	* [149. Max Points on a Line](#149-max-points-on-a-line)
	* [021. Merge Two Sorted Lists](#021-merge-two-sorted-lists)
	* [148. Sort List](#148-sort-list)
	* [009. Palindrome Number](#009-palindrome-number)
	* [121. Best Time to Buy and Sell Stock](#121-best-time-to-buy-and-sell-stock)
	* [053. Maximum Subarray](#053-maximum-subarray)
	* [038. Count and Say](#038-count-and-say)
	* [226. Invert Binary Tree](#226-invert-binary-tree)
	* [258. Add Digits](#258-add-digits)


## [014. Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)

> Write a function to find the longest common prefix string amongst an array of strings.

@Cecilia

等同于：minDepth
BFS √
DFS

**Solution 1: Brute Force**
哼哼，定义n为单个字符串的最大长度，m为LCP长度，k为字符串个数。

逐行搜索：Time O(nk), Space O(1)
逐列搜索：Time O(mk), Space O(1)

并不觉得有任何耍花枪的必要。
于是我们来逐列。

```java
string longestCommonPrefix(vector<string>& s) {
    if (strs.empty()) return "";
	
    for (int j = 0; j < s[0].size(); j++)
        for (int i = 1; i < s.size(); i++)
            if(j >= s[i].size() || s[i][j] != s[0][j])
                return s[0].substr(0, j);

    return strs[0];
}
```

**Solution 2: Trie (??!!!)**
频繁的插入0 0新字符
Build: Time O(nk), Space O(m)
Find: Time O(m), Space O(1)

平均英文单词长度是5个字母
时间复杂度平均是O(5) Space O(25^5)

shell her sea sheep
```
       #
    S    H
  H   E    E
  E   A    R
 L E
 L P
```
 
字典树0 0

说着，po主挽了个枪花（嘻嘻不服你来打我呀　　~~po主  扑街~~

```c++
class TrieNode {
private:
    TrieNode* child[25];
public:
    TrieNode();
    ~TrieNode();
    TrieNode* get(char);
    TrieNode* add(char);
};

class Trie {
private:
    TrieNode root;
public:
    Trie();
    void addWord(string);
    string getPrefix();
};

string longestCommonPrefix(vector<string>& strs) {
    Trie t;
    for (string s : strs) t.addWord(s);
    return t.getPrefix();
}
```


## [066. Plus One](https://leetcode.com/problems/plus-one/)

>
>Given a non-negative number represented as an array of digits, plus one to the
number.
>The digits are stored such that the most significant digit is at the head of
the list.




```
@Chris

```
```
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        for(int size = digits.size() - 1;size >= 0;--size)
		{
		    // this digit is < 9
		    if(digits[size] < 9)
		    {
			    digits[size] += 1;
			    return digits;
		    }
		    // this digit is 9
		    else {
			    digits[size] = 0;
		    }
        }
        //9，99，999...的情况记得补个1
        digits.insert(digits.begin(), 1);
        return digits;
    }
};
有没有好算法呢 这个只击败了百分之10
```
// 这道题击败了10%的  难道不是和剩下的90%并列第一吗hhh

## [070. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

>
>You are climbing a stair case. It takes _n_ steps to reach to the top.
>Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?



```
@Xinyi

```

时间空间复杂度均为O(n)

动归方程为：
```
ans[n] = ans[n - 1] + ans[n - 2];
```
// 嘻嘻，为何不试试Time O(n), Space O(1)呢wwww
// Hint: 如何用constant space获得第n个Fibonacci数

## [149. Max Points on a Line](https://leetcode.com/problems/max-points-on-a-line/)

>
>Given _n_ points on a 2D plane, find the maximum number of points that lie on the same straight line.

@Weichen

* 喵老师说她想了一个暴力的 O(n^2) 没想到就是最优解，我表示我想到的暴力的是 O(n^3) 的（我好萌

* O(n^2) 的解法就是固定一个点 i，求其他所有点和当前点连线的斜率$k_j (j=i+1, \cdots, n)$。对于每个点都能求得一个最多的k的数目，然后再对所有点取最大的数目+1就好。
    * 注意「重合」和「斜率不存在」的情况
    * 还有斜率要是float型的（感觉python要被黑了...

* 优化：喵老师说，内层循环中不用再重新求以前求过的点了。因为如果以前的某点和当前点在这条通过最多点的直线上，那么以前已经求得最大值了。


```python
# 代码是不是有点丑啊

# Definition for a point.
# class Point(object):
#     def __init__(self, a=0, b=0):
#         self.x = a
#         self.y = b

class Solution(object):
    def maxPoints(self, points):
        """
        :type points: List[Point]
        :rtype: int
        """
        
        n = len(points)
        
        if n <= 1: return n
        
        maxNum = 1
        for i in range(n):
            x1, y1 = points[i].x, points[i].y
            slp = {}
            
            for j in range(i+1, n):
                x2, y2 = points[j].x, points[j].y
                if x1 == x2:
                    if y1 == y2:
                        k = "o"
                    else:
                        k = "v"
                else:
                    k = (y1-y2) / (float(x1)-x2)
                if k in slp:
                    slp[k] += 1
                else:
                    slp[k] = 1
            
            overlap = slp.pop("o") if "o" in slp else 0
            
            if len(slp) == 0:
                maxNum = max(overlap+1, maxNum)
            else:
                maxNum = max(slp[max(slp, key=slp.get)]+overlap+1, maxNum)
        
        return maxNum
```

## [021. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

>Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.


```
@Zhenghui

```

基础版的 merge sort， 应用与linked list， 插入可以直接修改指针的指向，不需要重新建立`ListNode`。

```C++
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* dummy = new ListNode(0);
        ListNode* head = dummy;
        while(l1 && l2) {
            if(l1->val < l2->val) {
                head->next = l1;
                l1 = l1->next;
            } else  {
                head->next = l2;
                l2 = l2->next;
            }
            head = head->next;
        }
        
        if(l1) {
            head->next = l1;
        } else {
            head->next = l2;
        }
        return dummy->next;
    }
```
// 记得`delete dummy;`
// 或者试试: `ListNode dummy(0);` ... `return dummy.next;`

## [148. Sort List](https://leetcode.com/problems/sort-list/)

>Sort a linked list in _O_(_n_ log _n_) time using constant space complexity.

```
@WenXin

```

```c++
class Solution {
public:
    int countLength(ListNode *node){
        int n = 0;
        while (node != NULL){
            node = node->next;
            ++n;
        }
        return n;
    }
    ListNode *sortList(ListNode *head) {
        int mergeSize = 1, n = countLength(head), startIndex = 0, length1, length2;
        ListNode virtualHead(0);
        ListNode *last = NULL, *it = NULL, *merge1 = NULL, *merge2 = NULL, *tmp = NULL;
        virtualHead.next = head;
        while (mergeSize < n){
            startIndex = 0;
            last = &virtualHead;
            it = virtualHead.next;
            while (startIndex <  n){
                length1 = min(n - startIndex, mergeSize);
                length2 = min(n - startIndex - length1, mergeSize);

                merge1 = it;
                if (length2 > 0){
                    for (int i = 0; i < length1 - 1; ++i) it = it->next;
                    merge2 = it->next;
                    it->next = NULL;
                    it = merge2;

                    for (int i = 0; i < length2 - 1; ++i) it = it->next;
                    tmp = it->next;
                    it->next = NULL;
                    it = tmp;
                }

                while (merge1 || merge2){
                    if (merge2 == NULL || (merge1 != NULL && merge1->val <= merge2->val)){
                        last->next = merge1;
                        last = last->next;
                        merge1 = merge1->next;
                    } else {
                        last->next = merge2;
                        last = last->next;
                        merge2 = merge2->next;
                    }
                }
                startIndex += length1 + length2;
            }
            mergeSize <<= 1;
        }
        return virtualHead.next;
    }
};
```

## [009. Palindrome Number](https://leetcode.com/problems/palindrome-number/)

>
>Determine whether an integer is a palindrome. Do this without extra space.


@Cecilia

**Solution 1:** X整个翻转成Y. `O(n)`
**Solution 2:** X去掉后一半翻转成Y. `1/2 * O(n)` ~~反正去掉头剩下的全都可以吃~~

```java
bool isPalindrome(int x) {
    if ( x < 0 || x > INT_MAX || x && !(x%10) ) return false;  // 负数/溢出/末尾是0
    int y = 0;
    while(x > y) {
        y = y * 10 + x % 10;  // 把x的末端掰下来放到y的末端
        x /= 10;              // ...↑什么鬼你是端粒酶吗哈哈哈哈
    }
    return x == y || x == y/10;
}
```

## [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

>
>Say you have an array for which the _i_ th element is the price of a given stock on day _i_. If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.




```
@Chris

```
```c++
/*
从前向后遍历数组，记录当前出现过的最低价格，
作为买入价格，并计算以当天价格出售的收益，作为可能的最大收益，
整个遍历过程中，出现过的最大收益就是所求。
O(n)时间，O(1)空间。
*/
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        
        if(prices.size() < 2)
            return 0;

        int maxProfit = 0;
		//记录买入价最低的情况
        int curMin = prices[0];

        for(int i = 1; i < prices.size(); ++i)
        {
            curMin = min(curMin, prices[i]);
            maxProfit = max(maxProfit, prices[i] - curMin);
        }

        return maxProfit;
    }
};

说着说着 po主被限定 1次2次3次...k次交易xia xing le
```

## [053. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

> Find the contiguous subarray within an array (containing at least one number) which has the largest sum.

```
@Xinyi

```

动态规划，Time O(n), Space O(1)
动归方程：
```
sum[0, n] = num[n] + (sum[0, n - 1] > 0 ? sum[0, n - 1] : 0)
maxsum[0, n] = max(sum[0, n], maxsum[0, n - 1])
```

``` C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int sum = nums[0], maxs = nums[0];
        for(int i = 1; i < nums.size(); i++) {
            sum = (sum > 0 ? sum : 0) + nums[i];
            maxs = sum > maxs ? sum : maxs;
        }
        return maxs;
    }
};
```

// 同爬楼梯，试试O(1) Space 嘻嘻
// Hint: 计算sum[n]时，只需要用到sum[n-1]。前面n-2个int都是没用哒

## [038. Count and Say](https://leetcode.com/problems/count-and-say/)

>The count-and-say sequence is the sequence of integers beginning as follows:  
`1, 11, 21, 1211, 111221, ...`
>`1` is read off as `"one 1"` or `11`.  
`11` is read off as `"two 1s"` or `21`.  
`21` is read off as `"one 2`, then `one 1"` or `1211`.
>Given an integer _n_, generate the _n_th sequence.
>Note: The sequence of integers will be represented as a string.



@Weichen

- 好像没啥规律，就一个一个推吧（根据前一个求当前的）。

```python

class Solution(object):
    def countAndSay(self, n):
        """
        :type n: int
        :rtype: str
        """
        
        if n == 1: return "1"
        
        res = "1"
        
        for i in range(n-1):
            res += " "
            cnt = 1
            tmp = ""
            
            for j in range(1, len(res)):
                if res[j-1] == res[j]:
                    cnt += 1
                else:
                    tmp += str(cnt) + res[j-1]
                    cnt = 1
            
            res = tmp
            
        return res
```

## [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)

```
Invert a binary tree.
         4
       /   \
      2     7
     / \   / \
    1   3 6   9
    
to
         4
       /   \
      7     2
     / \   / \
    9   6 3   1
```



```
@Zhenghui

```
Binary Tree 的一个有意思的特点是 从中间任意节点往下看 仍然是一个 binary tree. 这道题目recursive的算法是非常容易的。

```C++
TreeNode* invertTree(TreeNode* root) {
        if(!root) return root;
        
        TreeNode* curLeft = invertTree(root->left);
        TreeNode* curRight = invertTree(root->right);
        root->left = curRight;
        root->right = curLeft;
        
        return root;
    }
```

## [258. Add Digits](https://leetcode.com/problems/add-digits/)

>
>Given a non-negative integer `num`, repeatedly add all its digits until the
result has only one digit.
>For example:
>Given `num = 38`, the process is like: `3 + 8 = 11`, `1 + 1 = 2`. Since `2`
has only one digit, return it.
>**Follow up:**  
Could you do it without any loop/recursion in O(1) runtime?
>1. A naive implementation of the above process is trivial. Could you come up with other methods? 
>   2. What are all the possible results?
>   3. How do they occur, periodically or randomly?
>   4. You may find this [Wikipedia article](https://en.wikipedia.org/wiki/Digital_root) useful.





```
@WenXin

```


```c++
 int addDigits(int num) {
        return 1 + (num - 1) % 9;
 }
```
呵呵哒
```C++
class Solution {
public:
    int addDigits(int num) {
        while(num > 9) {
            int tmp = 0;
            while(num > 0) {
                tmp += num % 10;
                num /= 10;
            }
            num = tmp;
        }
        return num;
    }
};
```
