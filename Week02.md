# 呆毛算法小分队 Week 2

* [呆毛算法小分队 Week 2](#呆毛算法小分队-week-2)
  * [155. Min Stack](#155-min-stack)
  * [007. Reverse Integer](#007-reverse-integer)
  * [005. Longest Palindromic Substring](#005-longest-palindromic-substring)
  * [104. Maximum Depth of Binary Tree](#104-maximum-depth-of-binary-tree)
  * [206. Reverse Linked List](#206-reverse-linked-list)
  * [008. String to Integer (atoi)](#008-string-to-integer-atoi)
  * [191. Number of 1 Bits](#191-number-of-1-bits)
  * [141. Linked List Cycle](#141-linked-list-cycle)
  * [013. Roman to Integer](#013-roman-to-integer)
  * [169. Majority Element](#169-majority-element)
  * [237. Delete Node in a Linked List](#237-delete-node-in-a-linked-list)
  * [020. Valid Parentheses](#020-valid-parentheses)

## [155. Min Stack](https://leetcode.com/problems/min-stack/)

>Design a stack that supports push, pop, top, and retrieving the minimumelement in constant time.
>* push(x) -- Push element x onto stack. 
>  * pop() -- Removes the element on top of the stack. 
>  * top() -- Get the top element. 
>  * getMin() -- Retrieve the minimum element in the stack.
>  
>**Example:**
MinStack minStack = new MinStack();
    minStack.push(-2);
    minStack.push(0);
    minStack.push(-3);
    minStack.getMin();   --> Returns -3.
    minStack.pop();
    minStack.top();      --> Returns 0.
    minStack.getMin();   --> Returns -2.

@Xinyi

* Naive解法，用一个min stack来记录当前栈里的最小值： /// beat 32%
    ``` c++
    void push(int x) {
        st.push(x);
        if(min_st.empty() || min_st.top() >= x) min_st.push(x);
    }
    ```
    ```c++
    void pop() {
        st.pop();
        if(min_st.top() == st.top()) min_st.pop();
    }
    ```
* 稍微好一点的解法，用一个min值来记录当前栈里的最小值： /// beat 74%
    ``` 
	push(x) : 
	if(x <= min) 在栈里保存当前min值；更新min值；插入x
	
	|  x  |  <--- 更新为当前min值
	|-----|
	| min’|  <--- 旧min值为当前栈的次小值，在栈里保存下来
	|-----|
	|     |
	|-----|
	
	pop() : 
	if(st.top() == min) 由于当前最小值的下面就是次小值，pop以后把min更新为当前top值即可
	
	|     |  
	|-----|
	| min’|  <--- pop以后，找到了之前保存的次小值，更新成min
	|-----|
	|     |
	|-----|
	```
	```c++
	/*** 码长这样 ***/
	
    void push(int x) {
        if(x <= min) {
            st.push(min);
            min = x;
        }
        st.push(x);
    }

    void pop() {
        if(st.top() == min) {
            st.pop();
            min = st.top();
        }
        st.pop();
    }
	```

// 为啥第二种更快一点


## [007. Reverse Integer](https://leetcode.com/problems/reverse-integer/)


>Reverse digits of an integer.
>* **Example1:** x = 123, return 321  
>* **Example2:** x = -123, return -321
>
>
>**Have you thought about this?**
>
>Here are some good questions to ask before coding. Bonus points for you if you have already thought through this!
>
> If the integer's last digit is 0, what should the output be? ie, cases such as 10, 100.
>
>Did you notice that the reversed integer might overflow? Assume the input is a 32-bit integer, then the reverse of 1000000003 overflows. How should you handle such cases?
>
>For the purpose of this problem, assume that your function returns 0 when thereversed integer overflows.

@Weichen


// 这是谁写的 @_@ <- Xinyi报到
判断溢出：
long a = 0;
if(a < INT_MIN || a > INT_MAX) overflow;

```python
class Solution(object):
    def reverse(self, x):
        """
        :type x: int
        :rtype: int
        """
        res = 0
        neg = -1 if x < 0 else 1
        x = abs(x)
        while x:
            res = res * 10 + x % 10
            x /= 10
        res *= neg
        return res if res <= 2147483648 - 1 and res >= -2147483648 else 0
```


## [005. Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)

> Given a string _S_, find the longest palindromic substring in _S_. You may assume that the maximum length of _S_ is 1000, and there exists one unique longest palindromic substring.


@Chris

:smirk:

```C++
class Solution {
public:
    string longestPalindrome(string s) {
    	// 'a' or ''
    	if(s.size() < 2)
    		return s;
    	vector<int> myVec;
    	int identical_length = 0;
    	int length = 1;
    	int leftCursor = 0;
    	int rightCursor = 0;
    	int startCursor = 0;
    	int maximum_length = 1;
    	// Now we iterate 
    	for(int i = 0; i < s.size(); ++i)
    	{
    		length = 1;
   			leftCursor = rightCursor = i;
			//我们先考虑有重复字符的情况 比如 “aa”
    		while(rightCursor < s.size() && s[rightCursor] == s[rightCursor + 1])
    		{
    			++rightCursor;
    		}
			//接下来我们再往两边儿找
    		while(s[leftCursor - 1] == s[rightCursor + 1] && leftCursor >= 1 && rightCursor <= s.size() - 2)
    		{
    			//两边都延展
    			length += 2;
    			--leftCursor;
    			++rightCursor;
    		}
    		//更新最长字符串
    		if(maximum_length < rightCursor - leftCursor + 1)
    		{
    			maximum_length = rightCursor - leftCursor + 1;
    			startCursor = leftCursor;
    		}
    	}
    	return s.substr(startCursor, maximum_length);
    }
};
击败了56.69%...= =
```

## [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

>Given a binary tree, find its maximum depth.
The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

@WenXin

==DFS== ==BFS== 
* Max Depth:
  * 完全树：
    * DFS: Space O(logn)
    * BFS: Space O(n)
  * 链表形：
    * DFS: Space O(n)
    * BFS: Space O(1)
  * 结论：Depends！ 

* Min Depth:
  * DFS: Time = O(n)
  * BFS: Time < O(n) 

```C++
   //DFS version
   int maxDepth(TreeNode* root) {
        return !root ? 0 : max(maxDepth(root->left), maxDepth(root->right)) + 1;
   }
```
 

## [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

> Reverse a singly linked list.
>
>**Hint:**
A linked list can be reversed either iteratively or recursively. Could you implement both?

@Cecilia

```C++
   NULL         A -> B -> C -> D
  (last)       (cur)

    A -> NULL     B -> C -> D
  (last)        (cur)


ListNode* reverseIterative(ListNode* head) {
    ListNode last = nullptr;
    while (head) {
        ListNode* next = head->next;
        head->next = last;
        last = head;
        head = next;
     }
    return last;
}
```
```C++
  A -> B -> C -> ...      ↓↓↓ reverse(head->next)
          (head)
                          F -> E -> D -> NULL
                        (cur)  (head->next)
			   
  A -> B -> C -> F -> E -> D -> C -> NULL
               (cur)  
			   

ListNode* reverseRecursive(ListNode* head) {
    if (!head || !head->next) return head;
    ListNode* cur = reverseRecursive(head->next);
    head->next->next = head;
    head->next = NULL;
    return cur;
}
```
:+1: 

## [008. String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/)

> Implement atoi to convert a string to an integer.
>
>  **Hint:** Carefully consider all possible input cases. If you want a challenge, please do not see below and ask yourself what are the possible input cases.
> 
>   **Notes:** It is intended for this problem to be specified vaguely (ie, no given input specs). You are responsible to gather all the input requirements up front.

@Zhenghui 


```C++
Two ways of skip whitespace at the beginning:
string str;
// 1st way:
// if no such character exist, return -1
int i = str.find_first_not_of(' ');
/*
 * Some similar function:
 * /
str.find_first_of(ch);
str.find_last_of(ch);
str.find_last_not_of(ch);


// 2st way:
istringstream iss(str);
iss >> str;
/* 
 * output iss until the end
 */
 while(true) {
 	if(!iss.good()) {
		break;
	}
	iss >> s;
	cout << s << endl;
 }

```
```C++
//8ms algorithm
  int myAtoi(string str) {
        int i = str.find_first_not_of(' ');
        if(i == -1) return 0;
        
        int sign = 1;
        int res = 0;
        
        if(str[i] == '-' || str[i] == '+') {
            sign = (str[i++]=='-') ? -1 : 1;
        }
        
        while(str[i] >= '0' && str[i] <= '9') {
            if ((res > INT_MAX/10) || (str[i]-'0' > INT_MAX % 10) && (res == INT_MAX/10)) {
                return sign == 1 ? INT_MAX : INT_MIN;
            } else {
                res = res*10 + str[i++]-'0';
            }
        }
        return res*sign;
    }
```

long res = 0;
if(res < INT_MIN || res > INT_MAX) overflow;

## [191. Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)

>Write a function that takes an unsigned integer and returns the number of ’1' bits it has (also known as the Hamming weight).
>
>For example, the 32-bit integer ’11' has binary representation`00000000000000000000000000001011`, so the function should return 3.

@Xinyi
``` c++
用一只萌萌哒mask，每次取一位：
num:  xxxxxxxx
mask: 00000001
      00000010
	  ...
mask里的1蹦着蹦着就蹦出去了，mask就变成0了，while就结束了=w=

int mask = 1, ans = 0;
while(mask != 0) {
    ans += (n & mask) == 0 ? 0 : 1;
    // ans += !!(n & mask);  <-- 这个比下面那个快0.2% （哈哈哈哈有病ww
    // ans += (n & mask) ? 1 : 0;
    mask <<= 1;
}
```
## [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)

>Given a linked list, determine if it has a cycle in it.
**Follow up:**
Can you solve it without using extra space?

@Weichen

```python

#1 比较好想到的是使用 hash map，记录一下以前访问过的node

#2 如果可以更改数据结构，哈

        if head is None:
            return False
        
        cur = head.next
        
        while cur is not None:
            if cur is head:
                return True
            prev = cur
            cur = cur.next
            prev.next = head
        return False
        
 #3 赛跑游戏：以不同的速度访问后面的节点，有circle的话会相遇

        if head is None or head.next is None:
            return False
        
        slow = head
        fast = head.next
        while slow != fast:
            if fast is None or fast.next is None:
                return False
            
            slow = slow.next
            fast = fast.next.next
        
        return True
 
```


## [013. Roman to Integer](https://leetcode.com/problems/roman-to-integer/)

>Given a roman numeral, convert it to an integer.
Input is guaranteed to be within the range from 1 to 3999.



@Chris

```c++
/*
I 1
V 5
X 10
L 50
C 100
D 500
M 1,000
*/

/*
1)相同的数字连写，所表示的数等于这些数字相加得到的数，如 Ⅲ=3；
2)小的数字在大的数字的右边，所表示的数等于这些数字相加得到的数，如 Ⅷ=8、Ⅻ=12；
3)小的数字（限于 Ⅰ、X 和 C）在大的数字的左边，所表示的数等于大数减小数得到的数，如 Ⅳ=4、Ⅸ=9；
4)在一个数的上面画一条横线，表示这个数增值 1,000 倍，如 /V =5000 这道题到3999 所以不适应此规则
*/
class Solution {
public:
    int romanToInt(string s) {
    	// 左减不能跨越等级 99 XCIX 而不是 IC 3999 MMMCMXCIX
    	int result = 0;
    	// 首先我建一个hashtable来存储字符与数字的转换规则 嘻嘻
    	unordered_map<char, int> code ( {{'I', 1}, {'V', 5}, {'X', 10}, {'L', 50}, {'C', 100}, {'D', 500}, {'M', 1000}} );
    	// 遍历给定字符串
    	for(int i = 0; i < s.size() - 1; ++i)//现在还没算最后一位
    	{
    		//我的后面有比我大的 那就是减我 嘻嘻
    		if(code[s[i]] < code[s[i + 1]])
    			result -= code[s[i]];
    		else
    			result += code[s[i]];
    	}
    	//处理最后一位
    	result += code[s[s.size() - 1]];
    	return result;
    }
};
```

## [169. Majority Element](https://leetcode.com/problems/majority-element/)

>Given an array of size n, find the majority element. The majority element is the element that appears more than `⌊ n/2 ⌋` times.
>
>You may assume that the array is non-empty and the majority element always exist in the array.

@WenXin


1. 如果target的数目大于一半，排序后中点永远都是target  Space O(1)  Time O(nlogn)

```C++
	int majorityElement(vector<int>& nums) {
		sort(nums.begin(), nums.end());
		return nums[nums.size() / 2];
	}
```

2. 指针 + counter解法   Space O(1)  Time O(n)
    * 两个部落打架

```C++
    int majorityElement(vector<int>& nums) {
        int target = nums[0], cnt = 1;
        for(int i = 1; i < nums.size(); ++i){
            target == nums[i] ? ++cnt : --cnt;
            if(cnt < 0){ 
                target = nums[i];  
                cnt = 1;
            }
        }
        return target;
     } 
```


## [237. Delete Node in a Linked List](https://leetcode.com/problems/delete-node-in-a-linked-list/)

>Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.
>
>Supposed the linked list is `1 -> 2 -> 3 -> 4` and you are given the third node with value `3`, the linked list should become `1 -> 2 -> 4` after calling your function.

@Cecilia
``` C++
void deleteNode(ListNode* node) {
    *node = *node->next;  // 就是不想delete你咬我呀
}

```

## [020. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

>Given a string containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.
The brackets must close in the correct order, `"()"` and `"()[]{}"` are all valid but `"(]"` and `"([)]"` are not.

@Zhenghui

```
一道非常经典的用stack求解的问题
Note： 用 switch 比 if...else...节省时间！！！
(From 2ms to 0ms)
```

