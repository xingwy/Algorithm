### **LEETCODE 1-20**

#### **[1.两数之和](https://leetcode-cn.com/problems/two-sum/)**

题述：在给定数组里找出两个值x,y满足 x+y=target

思路：直接使用每次遍历数组取值x,y再计算的话时间复杂度为O(n^2),不妨令y=target-x,遍历x并每次查找数组是否包含y。时间复杂度为O(n)

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int x,y;
        vector<int> res;
        int min = 0;
        
        min = 709347;
        int *p = new int[min*2+5]();
        memset(p, -1, sizeof(int) * (min*2+5));
        for(int i=0;i<nums.size();i++){
            if(p[(nums[i]%72347)+min] != -1){
                if(target == 2*nums[i]){
                    res.push_back(p[nums[i]%72347+min]);
                    res.push_back(i);
                    return res;
                }
            }else{
                p[nums[i]%72347+min] = i;
            }   
        }
        for(int i=0;i<nums.size();i++){
            if (target / 2 == nums[i]) {
			    continue;
		    }
            if(p[target-(nums[i]%72347)+min] != -1){
                x=i;
                y=p[target-(nums[i]%72347)+min];
                break;
            }
        }
        res.push_back(x);
        res.push_back(y);
        return res;
    }
};
```

#### **[2.两数相加](https://leetcode-cn.com/problems/add-two-numbers/)**

题述：给定两个链表，按照规则加上链表的值。

比如l1:(2->4->3) l2:(5->6->4) l1+l2=(7->0->8)

思路：基础数据结构题，考察链表的使用。注意进位即可

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *res,*p;
        if(l1 == NULL && l2 == NULL)
            return res;
        p = new ListNode(0);   //首节点
        res = p;
        int index = 0;   //进位
        while(true){
            int value1, value2;
            //取值并更新
            value1 = (l1 == NULL ? 0 : l1->val);
            l1 = (l1 == NULL ? 0 : l1->next);
            value2 = (l2 == NULL ? 0 : l2->val);
            l2 = (l2 == NULL ? 0 : l2->next);
            //赋值并加入链表
            p->val = (value1+value2+index) % 10;
            index = (value1+value2+index) / 10;
            if(l1 == NULL && l2 == NULL){
                if(index > 0){
                    p->next = new ListNode(index);
                }
                break;
            }
            else{
                p->next = new ListNode(0);
                p = p->next;
            }
        }
        return res;
    }
};
```

#### **[3.无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)**

题述：在一个给定的字符串找出最长的子字符串，满足子字符串字母不重复。

思路：需要一次遍历，需要记录当前所走过的子字符串的最长满足值。

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int mark[127] = {0};
	    int start = 1, res = 0;
	    for (int i = 0; i < s.size(); i++) {
		    if (mark[s[i]] == 0 || mark[s[i]] < start) {
		    	//当前子串不重复 加入
		    	if ((i - start + 2) > res)
		    		res = (i + 1) - start + 1;
		    	mark[s[i]] = i + 1;  //标志这个字符的出现下标
	    	}
	    	else {
	    		//当前字符有重复 先舍弃之前再加入
	    		start = mark[s[i]] + 1;
                //更新这个字符的下标
	    		mark[s[i]] = i + 1;
	    	}
	    }
	    return res;
    }
};
```

#### **[4.寻找两个正序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)**

题述：在给定的两个有序数组中找出中间数。

例子：mid([1,3], [1,2,5])  = 2, mid([1,3,4],[1,2,5])=2.5

思路：直接默认考虑数组元素量很大，使用二分查找的方法。时间复杂度O(logn)

```c++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int len1 = nums1.size(), len2 = nums2.size();
	    int midLen = (len1 + len2 + 1) / 2;
	    int start1 = 0, start2 = 0;    //start为两个数组的中间值前面下标
	    double res = 0 ,index = 1.0;
	    bool t = true;
	    if ((len1 + len2) % 2) {
	    	t = false;
    	}
	    while (true) {
	    	//剩下的第一个数
	    	//cout << midLen << endl;
	    	if (midLen == 1) {
				if (start1 == len1) {
					res += nums2[start2];
					start2++;
				}else if (start2 == len2) {
					res += nums1[start1];
					start1++;
				}
				else {
					if (nums1[start1] < nums2[start2]) {
						res += nums1[start1];
						start1++;
					}
					else {
						res += nums2[start2];
						start2++;
					}
				}
				if (t) {
					t = false;
					index = 2.0;
					continue;
				}
				res /= index;
				break;
			}
		else {
			int mid = midLen / 2;
			int offset1, offset2;
			offset1 = (start1 + mid) > len1 ? len1 - start1 : mid;
			offset2 = (start2 + mid) > len2 ? len2 - start2 : mid;
            if (offset1 <= 0) {
				start2 += offset2;
				midLen -= offset2;
				continue;
			}
			if (offset2 <= 0) {
				start1 += offset1;
				midLen -= offset1;
				continue;
			}
			if (nums1[start1 + offset1 - 1] < nums2[start2 + offset2 - 1]) {
				start1 += offset1;
				midLen -= offset1;
			}
			else {
				start2 += offset2;
				midLen -= offset2;
			}
		}
	}
	return res;
    }                                
};
```

#### **[5.最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)**

题述：给定字符串找出最大回文字符串。长度maximum为1000

例子：Iput：“babacd”,Ouput:"aba"

思路：数据量不多，直接分子字符串长度奇偶情况，以中点向两边扩散，时间复杂度O(n^2)。当数据量大时使用Manacher算法，复杂度O(n)，会有明显提升。

```c++
class Solution {
public:
    void getLR(string s, int &l, int &r) {
    	if (r >= s.size()) {
    		r = l;
    	}
    	if (l != r && s[l] != s[r]) {
    		r--;
	    	return;
    	}
    	while (true) {
	    	l--;
	    	r++;
	    	if (l < 0 || r >= s.size()) {
	    		l++;
	    		r--;
	    		break;
	    	}
	    	if (s[l] != s[r]) {
	    		l++;
	    		r--;
    			break;
	    	}
    	}
    }
    string longestPalindrome(string s) {
	    if (s.size() == 0) {
	    	return "";
    	}
    	int resl = 0, resr = 0;
    	for (int i = 0; i < s.size(); i++) {
	    	int l, r;
	    	l = r = i;
	    	getLR(s, l, r);
	    	if ((r - l) > (resr - resl)) {
	    		resr = r;
	    		resl = l;
	    	}
	    	l = i;
	    	r = i + 1;
	    	getLR(s, l, r);
	    	if ((r - l) > (resr - resl)) {
		    	resr = r;
		    	resl = l;
	    	}
	    }
	    string res;
	    for (int i = resl; i <= resr; i++) {
	    	res.push_back(s[i]);
	    }
	    return res;
    }
};
```

#### **[6.Z 字形变换](https://leetcode-cn.com/problems/zigzag-conversion/)**

题述：给定一个字符串，要求按规则重新排列字符串。

```
Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"

P     I    N
A   L S  I G
Y A   H R
P     I
```

思路：逐行按规律更新res字符串。

```c++
class Solution {
public:
    string convert(string s, int numRows) {
        if(numRows == 1)
            return s;
        int len = s.size();
        string res;
        for(int i=1; i<=numRows; i++){
            int cur = i - 1;
            int index = 2*(numRows - 1),offset = 2*(numRows - i);
            while(true){
                if(cur >= s.size()){
                    break;
                }
                if(i == 1 || i == numRows){
                    res.push_back(s[cur]);
                    cur += index;
                }else{
                    res.push_back(s[cur]);
                    if((cur + offset) >= s.size()){
                        break;
                    }
                    res.push_back(s[cur + offset]);
                    cur += index;
                }
            }
        }
        return res;
    }
};
```

#### **[7.整数反转](https://leetcode-cn.com/problems/reverse-integer/)**

题述：反转一个int型数据。

```c++
class Solution {
public:
    int reverse(int x) {
        bool mark =true;
        int start = 0xffffffff, end = 0x7fffffff;
        if(x == 0)
            return x;
        else if(x < 0){
            x = -x;
            mark = false;
        }
        int index = 0;
        long long res = 0;
        while(x != 0){
            if(res > end || res < start)
                return 0;
            res = (res * 10) + (x % 10);
            x /= 10;
        }
        if(!mark)
            return int(-res);
        return int(res);
    }
};
```

#### **[8.字符串转换整数 (atoi)](https://leetcode-cn.com/problems/string-to-integer-atoi/)**

题述：给定字符串转int型

```c++
class Solution {
public:
    int myAtoi(string str) {
        bool mark = true;
        int index = 0;
        long long res = 0;
        while(str[index] == ' ' && index < str.size()){
            index++;
        }
        if(index >= str.size())
            return 0;
        if((str[index] < '0' || str[index] > '9') && str[index] != '-' && str[index] != '+'){
            return 0;
        }
        if(str[index] == '-'){
            mark = false;
            index++;
            if(index >= str.size())
                return 0;
        }else if(str[index] == '+'){
            mark = true;
            index++;
            if(index >= str.size())
                return 0;
        }
        while(str[index] >= '0' && str[index] <= '9' && index < str.size()){
            res = res*10 + (str[index]-'0');
            index++;
            if(res > 0x7fffffff)
                break;
        }
        if(mark){
            if(res > 0x7fffffff){
                return 0x7fffffff;
            }else{
                return res;
            }
        }else{
            if (res-1 > (0x7fffffff)) {
			    return -1*0x7fffffff - 1;
		    }
	    	else {
		    	return -res;
	    	}
        }
    }
};
```

#### **[9.回文数](https://leetcode-cn.com/problems/palindrome-number/)**

题述：给定整型，判断是否为回文数字

思路：就是常规的求其反数字，对比数字即可

```c++
class Solution {
public:
    bool isPalindrome(int x) {
	    int xx = x;
	    if (x < 0) {
	    	return false;
	    }
	    int a[50], index = 0;
    	while (x) {
    		a[index++] = (x % 10);
    		x /= 10;
    	}
    	int res = 0;
    	int i = 0;
    	while (i < index) {
    		res = res * 10 + a[i++];
    	}
    	if (xx == res)
    		return true;
    	else
    		return false;
    }
};
```

#### **10. Regular Expression Matching**

题述：拼接链表 规则如下

```c++
Input:
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6
```

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
	    ListNode *head = NULL, *p;
	    p = new ListNode(0);
	    head = p;
	    while (lists.size()) {
	    	int min = 0x7fffffff;
	    	int pos_i;
	    	for (int i = 0; i < lists.size();) {		
	    		if (lists[i] != NULL) {
	    			if (min > lists[i]->val) {
	    				min = lists[i]->val;
	    				pos_i = i;
	    			}
	    			i++;
	    		}
    			else {
	    			lists.erase(lists.begin() + i);
	    		}
	    	}
	    	if (lists.size() == 0)
	    		break;
	    	p->next = new ListNode(min);
	    	p = p->next;
	    	lists[pos_i] = lists[pos_i]->next;	
    	}
    	return head->next;
    }    
};
```

#### **[11.盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)**

题述：给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0) 。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

**说明：**你不能倾斜容器。

![question_11](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/question_11.jpg)

```js
输入：[1,8,6,2,5,4,8,3,7]
输出：49
```

```js
/**
 * @param {number[]} height
 * @return {number}
 */
var maxArea = function(height) {
    let l=0;
    let r=height.length-1;
    let l_wall = 0;
    let r_wall = height.length-1;

    let max = Math.min(height[l_wall], height[r_wall]) * (r_wall-l_wall);
    while(l<=r) {
        if (height[l] <= height[r]) {
            l++;
            if (height[l] > height[l_wall]) {
                l_wall = l;
                max = Math.max(Math.min(height[l_wall], height[r_wall]) * (r_wall-l_wall), max);
            }
        } else {
            r--;
            if(height[r] > height[r_wall]) {
                r_wall = r;
                max = Math.max(Math.min(height[l_wall], height[r_wall]) * (r_wall-l_wall), max);
            }
        }
    }
    return max;
};
```

#### **[12.整数转罗马数字](https://leetcode-cn.com/problems/integer-to-roman/)**

题述：罗马数字包含以下七种字符： `I`， `V`， `X`， `L`，`C`，`D` 和 `M`。

```markdown
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：

I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 
C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。
给你一个整数，将其转为罗马数字。

```markdown
输入: num = 3
输出: "III"
```

```js
/**
 * @param {number} num
 * @return {string}
 */
var intToRoman = function(num) {
    let base = [[1, "I"], [4, "IV"], [5, "V"], [9, "IX"], [10, "X"], [40, "XL"], [50, "L"], [90, "XC"], [100, "C"],[400, "CD"], [500, "D"], [900, "CM"], [1000, "M"]];
    let res = "";
    for (let i = base.length-1; i>=0; --i) {
        if (num >= base[i][0]) {
            let index = Math.floor(num / base[i][0]);
            for (let j=0; j<index; j++) {
                res += base[i][1];
            }
            num %= base[i][0];
            if (num <= 0) {
                break;
            }
        }
    }
    return res;
};
```

#### **[13.罗马数字转整数](https://leetcode-cn.com/problems/roman-to-integer/)**

题述：罗马数字包含以下七种字符: `I`， `V`， `X`， `L`，`C`，`D` 和 `M`。

```markdown
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：

I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 
C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。
给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。

```js
/**
 * @param {string} s
 * @return {number}
 */
var romanToInt = function(s) {
    const _map = {
        "IV": 4,
        "IX": 9,
        "XL": 40,
        "XC": 90,
        "CD": 400,
        "CM": 900,
    }
    const _base = {
        "I": 1,
        "V": 5,
        "X": 10,
        "L": 50,
        "C": 100,
        "D": 500,
        "M": 1000,
    }

    let v = "";
    let result = 0;
    for (let i=0; i<s.length; i++) {
        if (!v) {
            v = s[i];
            continue;
        }
        if (_map[v+s[i]]) {
            result += _map[v+s[i]];
            v = "";
        } else {
            result += _base[v];
            v = s[i];
        }
    }

    if (v) {
        result += _base[v];
    }
    return result;
};
```

#### **[14.最长公共前缀](https://leetcode-cn.com/problems/longest-common-prefix/)**

题述：编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 `""`。

```markdown
输入：strs = ["flower","flow","flight"]
输出："fl"
```

```js
/**
 * @param {string[]} strs
 * @return {string}
 */
var longestCommonPrefix = function(strs) {
    let offset = 0;
    let str = "";
    let c = "";
    while(true) {
        for (let s of strs) {
            if (s.length <= offset) {
                return str;
            }

            if (!c) {
                c = s[offset];
                continue;
            }

            if (s[offset] != c) {
                return str;
            }
        }
        offset++;
        str += c;
        c = '';
    }
};
```

#### **[15.三数之和](https://leetcode-cn.com/problems/3sum/)**

题述：给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有和为 0 且不重复的三元组。

```markdown
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
```

```js
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var threeSum = function(nums) {
    nums.sort((a,b)=>a-b);
    let res = [];
    let _set = new Set();
    for (let i =0; i<nums.length-2; i++) {
        if (i > 0 && nums[i] == nums[i - 1]) {
            continue;
        }
        let l = i+1;
        let r = nums.length-1;
        let target = -nums[i];
        while(l<r) {
            if (l>i+1 && nums[l-1] == nums[l]) {
                l++
            } else {
                if (nums[l] + nums[r] == target) {
                    res.push([nums[i],nums[l],nums[r]]);
                    l++;
                } else {
                    nums[l]+nums[r] > target? r--: l++;
                }
            }
            
        }
    }
    return res;
};
```

#### **[16.最接近的三数之和](https://leetcode-cn.com/problems/3sum-closest/)**

题述：给定一个包括 n 个整数的数组 nums 和 一个目标值 target。找出 nums 中的三个整数，使得它们的和与 target 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

```markdown
输入：nums = [-1,2,1,-4], target = 1
输出：2
解释：与 target 最接近的和是 2 (-1 + 2 + 1 = 2) 。
```

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var threeSumClosest = function(nums, target) {
    nums.sort((a, b) => a-b);
    let max = Infinity;
    let result;

    for (let i=0; i<nums.length-2; i++) {
        let pb = i+1;
        let pc = nums.length-1;
        while (pb < pc) {
            let v = nums[i] + nums[pb] + nums[pc];
            if (max > Math.abs(v - target)) {
                result = v;
                max = Math.abs(v - target);
            };
            if (v == target) {
                return target;
            }

            if (v >= target) {
                pc--;
            } else {
                pb++;
            }
        }
    }
    return result;
};
```

#### **[17.电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)**

题述：给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。答案可以按 任意顺序 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

  ![17_telephone_keypad](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/17_telephone_keypad.png)

```markdown
输入：digits = "23"
输出：["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

```js
/**
 * @param {string} digits
 * @return {string[]}
 */
var letterCombinations = function(digits) {
    if (!digits.length) {
        return [];
    }
    let base = ["abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"];
    let res = [];
    let dfs = function(index, list) {
        if (index == digits.length) {
            res.push(list.join(""));
            return;
        } else {
            for (let j=0; j<base[digits[index]-2].length; j++) {
                list.push(base[digits[index]-2][j]);
                dfs(index+1, list);
                list.pop();
            }
        }
    }
    dfs(0,[]);
    return res;
};
```

#### **[18.四数之和](https://leetcode-cn.com/problems/4sum/)**

题述：给定一个包含 n 个整数的数组 nums 和一个目标值 target，判断 nums 中是否存在四个元素 a，b，c 和 d ，使得 a + b + c + d 的值与 target 相等？找出所有满足条件且不重复的四元组。

注意：答案中不可以包含重复的四元组。

```markdown
输入：nums = [1,0,-1,0,-2,2], target = 0
输出：[[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
```

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[][]}
 */
var fourSum = function(nums, target) {
    nums.sort((a,b)=>a-b);
    let res = [];
    for (let i = 0; i<nums.length-3; i++) {
        if (i > 0 && nums[i] == nums[i - 1]) {
            continue;
        }
        for (let j =i+1; j<nums.length-2; j++) {
            if (j > i+1 && nums[j] == nums[j - 1]) {
                continue;
            }
            let l = j+1;
            let r = nums.length-1;
            while(l<r) {
                if (l>j+1 && nums[l-1] == nums[l]) {
                    l++
                } else {
                    if (nums[i] + nums[j] + nums[l] + nums[r] == target) {
                        res.push([nums[i],nums[j],nums[l],nums[r]]);
                        l++;
                    } else {
                        nums[i] + nums[j] + nums[l] + nums[r] > target? r--: l++;
                    }
                }
            }
        }
    }
    return res;
};

```

#### **[19.删除链表的倒数第 N 个结点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)**

题述：给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

要求用一趟扫描实现。

![question_19](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/question_19.jpg)

```markdown
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} n
 * @return {ListNode}
 */
var removeNthFromEnd = function(head, n) {
    let first = head;
    let second;
    for (let i=0; i<n; i++) {
        first = first.next;
    }
    second = head;

    if (!first) {
        return head.next;
    }

    while (first.next) {
        first = first.next;
        second = second.next;
    }

    second.next = second.next.next;
    return head;
};
```

#### **[20.有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)**

题述：给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。

```markdown
输入：s = "()"
输出：true
```

```js
/**
 * @param {string} s
 * @return {boolean}
 */
var isValid = function(s) {
    let stack = [];
    for (let i=0; i<s.length; i++) {
        if (["{", "[", "("].indexOf(s[i]) != -1) {
            stack.push(s[i]);
        } else {
            switch(s[i]) {
                case ")":
                    if(stack.pop() != "(") {
                        return false;
                    }
                    break;
                case "]":
                    if(stack.pop() != "[") {
                        return false;
                    }
                    break;
                case "}":
                    if(stack.pop() != "{") {
                        return false;
                    }
                    break;
            }
        }
    }
    return stack.length == 0;
};
```

