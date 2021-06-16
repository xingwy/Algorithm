### **LEETCODE 41-60**

#### **41. First Missing Positive**

Promise：Given an unsort interger array,find the smallest missing positive integer, your algorithm should run in O(n) time and uses constant space

Example 1:

```text
Input: [1,2,0]
Output: 3
```

Example 2:

```text
Input: [3,4,-1,1]
Output: 2
```

Example 3:

```text
Input: [7,8,9,11,12]
Output: 1
```

Solutions:

```text
According to this promise content, we know that we need control our algorithm run in O(n), that meaning we can't sort the array. when we find the smallest interval, it must between 1 and the size of this array. so we only need sort or find the interval which meet the conditions.
```

Code:

```c++
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            while (nums[i] > 0 && nums[i] <= n && nums[nums[i] - 1] != nums[i]) {
                swap(nums[i], nums[nums[i] - 1]);
            }
        }
        for (int i = 0; i < n; ++i) {
            if (nums[i] != i + 1) return i + 1;
        }
        return n + 1;
    }
};
```

#### **[42.Trapping Rain Water](https://leetcode-cn.com/problems/trapping-rain-water/)**

Problem：

```markdown
Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.
```

![rainwatertrap_42](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/rainwatertrap_42.png)

Example：

```markdown
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
```

```js

/**
 * @param {number[]} height
 * @return {number}
 */
var trap = function(height) {
    let leftWall = 0;
    let rightWall = 0;
    let sum = 0;
    let i = 0;
    let j = height.length-1;
    while(i < j) {
        if (height[i] < height[j]) {
            height[i] >= leftWall ? (leftWall = height[i]) : (sum += (leftWall-height[i]));
            i++;
        } else {
            height[j] >= rightWall ? (rightWall = height[j]) : (sum += (rightWall-height[j]));
            j--;
        }
    }
    return sum;
    
};
```

#### **[43.Multiply Strings](https://leetcode-cn.com/problems/multiply-strings/)**

Problem：

```markdown
Given two non-negative integers num1 and num2 represented as strings, return the product of num1 and num2, also represented as a string.

Note: You must not use any built-in BigInteger library or convert the inputs to integer directly.
```

Example：

```markdown
Input: num1 = "2", num2 = "3"
Output: "6"
```



#### **55. Jump Game**

Problem: Given an array of non-negative intergers, you are initially positioned at the first index of the array.

Each element int the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.

Example 1:

```text
Input: [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index
```

Example 2:

```text
Input: [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what.Its maximum jump length is 0,which makes it impossable to reach the last index.
```

Solutions:

```text
We can Maintain a tmp item named 'max' when it check element form the first index to the last index. Of course, its initialization is 0. when current element puls its index is greater than max, set max equal to this element puls its index. Its time complexity is O(n). My code runtime beats 98.82 % of CPP submissions.
```

Code:

```c++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int max = 0, res = false;
        for(int i=0; i < nums.size(); i++){
            if((nums[i] + i) > max && max >= i){
                max = nums[i] + i;
            }
        }
        return max >= (nums.size() - 1);
    }
};
```

#### **56. Merge Intervals**

Given a collection of intervals, merge all overlapping intervals.

Example 1:

```text
Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
```

Example 2:

```text
Input: [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

Solutions:

```text
My first idea is start at the first element,set type [left, right], judging current element(A) and the previous element(B),merge A and B when A's right is greater than B's left.
```

Code:

```js
var merge = function(intervals) {
    let res = [], tmp;
    intervals.sort((a,b) => {
        return a[0] - b[0]
    })
    if(intervals.length > 1){
        tmp = intervals[0];
    }else{
        return intervals
    }
    for(let i=1; i<intervals.length; i++){
        if(tmp[1] >= intervals[i][0]){
            tmp[1] = intervals[i][1] > tmp[1] ? intervals[i][1] : tmp[1];
        }else{
            res.push(tmp);
            tmp = intervals[i];
        }
        if(i >= intervals.length-1){
            res.push(tmp);
            break;
        }
    }
    return res; 
};
```

#### **57. Insert Interval**

Given a set of non-overlapping intervals, insert a new interval into the intervals(merge if necessary).

You will assume that the intervals were initially sorted according tp theri start times.

Example 1:

```text
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
```

Example 2:

```text
Iput: intervals = [[1,2],[3,5],[6,9],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],12,16]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10]
```

Solutions:

```text
We can get the answer of this problem as same as Code problem 56, in face, it is correct.but it isn't the best way solution. because it is non-overlapping, so we need only find the best index and insert newInterval.
```

Code 1:

```js
var insert = function(intervals, newInterval) {
    let res = [], tmp;
    intervals.push(newInterval)
    intervals.sort((a,b) => {
        return a[0] - b[0]
    })
    if(intervals.length > 1){
        tmp = intervals[0];
    }else{
        return intervals
    }
    for(let i=1; i<intervals.length; i++){
        if(tmp[1] >= intervals[i][0]){
            tmp[1] = intervals[i][1] > tmp[1] ? intervals[i][1] : tmp[1];
        }else{
            res.push(tmp);
            tmp = intervals[i];
        }
        if(i >= intervals.length-1){
            res.push(tmp);
            break;
        }
    }
    return res; 
};
```













