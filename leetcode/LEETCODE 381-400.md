### **LEETCODE 381-400**

#### **[381. Insert Delete GetRandom O(1) - Duplicates allowed](https://leetcode-cn.com/problems/insert-delete-getrandom-o1-duplicates-allowed/)**

Problem：

Implement the RandomizedCollection class:

- RandomizedCollection() Initializes the RandomizedCollection object.
- bool insert(int val) Inserts an item val into the multiset if not present. Returns true if the item was not present, false otherwise.
- bool remove(int val) Removes an item val from the multiset if present. Returns true if the item was present, false otherwise. Note that if val has multiple occurrences in the multiset, we only remove one of them.
- int getRandom() Returns a random element from the current multiset of elements (it's guaranteed that at least one element exists when this method is called). The probability of each element being returned is linearly related to the number of same values the multiset contains.

You must implement the functions of the class such that each function works in average O(1) time complexity.

Example：

```markdown
["RandomizedCollection", "insert", "insert", "insert", "getRandom", "remove", "getRandom"]
[[], [1], [1], [2], [], [1], []]
Output
[null, true, false, true, 2, true, 1]
```

Explanation

- RandomizedCollection randomizedCollection = new RandomizedCollection();
- randomizedCollection.insert(1);   // return True. Inserts 1 to the collection. Returns true as the collection did not contain 1.
- randomizedCollection.insert(1);   // return False. Inserts another 1 to the collection. Returns false as the collection contained 1. Collection now contains [1,1].
- randomizedCollection.insert(2);   // return True. Inserts 2 to the collection, returns true. Collection now contains [1,1,2].
- randomizedCollection.getRandom(); // getRandom should return 1 with the probability 2/3, and returns 2 with the probability 1/3.
- randomizedCollection.remove(1);   // return True. Removes 1 from the collection, returns true. Collection now contains [1,2].
- randomizedCollection.getRandom(); // getRandom should return 1 and 2 both equally likely.

```js
/**
 * Initialize your data structure here.
 */
var RandomizedCollection = function() {
    this._list = [];
    this.len = 0;
    this._map = new Map();
};

/**
 * Inserts a value to the collection. Returns true if the collection did not already contain the specified element. 
 * @param {number} val
 * @return {boolean}
 */
RandomizedCollection.prototype.insert = function(val) {
    if (!this._map.has(val)) {
        this._map.set(val, new Set());
    }
    let v = this._map.get(val);
    this._list[this.len] = val;
    this.len += 1;
    v.add(this.len-1);
    if (v.size <= 1){
        return true;
    } else {
        return false;
    }
};

/**
 * Removes a value from the collection. Returns true if the collection contained the specified element. 
 * @param {number} val
 * @return {boolean}
 */
RandomizedCollection.prototype.remove = function(val) {
    let indexs = this._map.get(val);
    if (!indexs || indexs.size == 0) {
        return false;
    }
    // 随机弹出一个
    let it = indexs.entries();
    let index = it.next().value[0];
    indexs.delete(index);
    let v = this._list[this.len-1];
    // if (v != val) {
        let v_indexs = this._map.get(v);
        
        v_indexs.add(index);
        v_indexs.delete(this.len-1);
        this._list[index] = this._list[this.len-1];
    // }
    
    this.len--;
    return true;
};

/**
 * Get a random element from the collection.
 * @return {number}
 */
RandomizedCollection.prototype.getRandom = function() {
    let index = Math.floor(Math.random() * this.len);
    return this._list[index];
};
```



#### **[392. Is Subsequence](https://leetcode-cn.com/problems/is-subsequence/)**

Problem：

Given two strings s and t, return true if s is a subsequence of t, or false otherwise.

A subsequence of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., "ace" is a subsequence of "abcde" while "aec" is not).

Example：

```markdown
Input: s = "abc", t = "ahbgdc"
Output: true
```

Explanation: [4,9] is also accepted.

```js
/**
 * @param {string} s
 * @param {string} t
 * @return {boolean}
 */
var isSubsequence = function(s, t) {
    let index = 0;
    for (let i=0; i<t.length; i++) {
        if (t[i] == s[index]) {
            index++;
        }
        if (index == s.length) {
            return true;
        }
    }
    return index == s.length;
};
```

#### **[393. UTF-8 Validation](https://leetcode-cn.com/problems/utf-8-validation/)**

Problem：

Given an integer array data representing the data, return whether it is a valid UTF-8 encoding.

A character in UTF8 can be from 1 to 4 bytes long, subjected to the following rules:

For a 1-byte character, the first bit is a 0, followed by its Unicode code.
For an n-bytes character, the first n bits are all one's, the n + 1 bit is 0, followed by n - 1 bytes with the most significant 2 bits being 10.

This is how the UTF-8 encoding would work:

```markdown
  Char. number range  |        UTF-8 octet sequence
      (hexadecimal)    |              (binary)
   --------------------+---------------------------------------------
   0000 0000-0000 007F | 0xxxxxxx
   0000 0080-0000 07FF | 110xxxxx 10xxxxxx
   0000 0800-0000 FFFF | 1110xxxx 10xxxxxx 10xxxxxx
   0001 0000-0010 FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx
```

Note: The input is an array of integers. Only the least significant 8 bits of each integer is used to store the data. This means each integer represents only 1 byte of data.

Example：

```markdown
Input: data = [197,130,1]
Output: true
```

Explanation: data represents the octet sequence: 11000101 10000010 00000001.
It is a valid utf-8 encoding for a 2-bytes character followed by a 1-byte character.

```js
/**
 * @param {number[]} data
 * @return {boolean}
 */
var validUtf8 = function(data) {
    let unicode = function(n) {
        if ((0b10000000 & n) == 0b00000000) {
            return 1;
        } else if ((0b11100000 & n) == 0b11000000) {
            return 2;
        } else if ((0b11110000 & n) == 0b11100000) {
            return 3;
        } else if ((0b11111000 & n) == 0b11110000) {
            return 4;
        } else if ((0b11000000 & n) == 0b10000000) {
            return 5;
        } else {
            return 6;
        }
    }
    let index = 0;
    while(index <data.length) {
        let base = unicode(data[index++]);
        if ([1,2,3,4].indexOf(base) != -1) {
            if (data.length - index >= base-1) {
                for (let j=1; j<base; j++) {
                    if (unicode(data[index++]) != 5) {
                        return false;
                    }
                }
            } else {
                return false;
            }
        } else {
            return false;
        }
    }
    return true;
};
```

#### **[394. Decode String](https://leetcode-cn.com/problems/decode-string/)**

Problem：

Given an encoded string, return its decoded string.

The encoding rule is: k[encoded_string], where the encoded_string inside the square brackets is being repeated exactly k times. Note that k is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, k. For example, there won't be input like 3a or 2[4].

Example：

```markdown
Input: s = "3[a]2[bc]"
Output: "aaabcbc"
```

```js
/**
 * @param {string} s
 * @return {string}
 */
var decodeString = function(s) {
    let index = 0;
    let parse = function() {
        let num = "";
        let str = "";
        while(index < s.length) {
            if (s[index] == "]") {
                index++;
                return str;
            } else if(s[index] <= '9' && s[index] >= "0") {
                num += s[index++];
            } else if (s[index] == "[") {
                index++;
                num = Number(num);
                let v = parse();
                for (let j=0; j<num; j++) {
                    str += v;
                }
                num = "";
            } else {
                str += s[index++];
            }
        }
        return str;
    }

    return parse();
};
```

#### **[398. Random Pick Index](https://leetcode-cn.com/problems/random-pick-index/)**

Problem：


Given an integer array `nums` with possible **duplicates**, randomly output the index of a given `target` number. You can assume that the given target number must exist in the array.

Implement the `Solution` class:

- `Solution(int[] nums)` Initializes the object with the array `nums`.
- `int pick(int target)` Picks a random index `i` from `nums` where `nums[i] == target`. If there are multiple valid i's, then each index should have an equal probability of returning.

Example：

```markdown
Input
["Solution", "pick", "pick", "pick"]
[[[1, 2, 3, 3, 3]], [3], [1], [3]]
Output
[null, 4, 0, 2]
```

Explanation
Solution solution = new Solution([1, 2, 3, 3, 3]);
solution.pick(3); // It should return either index 2, 3, or 4 randomly. Each index should have equal probability of returning.
solution.pick(1); // It should return 0. Since in the array only nums[0] is equal to 1.
solution.pick(3); // It should return either index 2, 3, or 4 randomly. Each index should have equal probability of returning.

```js
/**
 * @param {number[]} nums
 */
var Solution = function(nums) {
    this.nums = nums;
};

/** 
 * @param {number} target
 * @return {number}
 */
Solution.prototype.pick = function(target) {
    let result = [];
    for (let i=0; i<this.nums.length; i++) {
        if(target == this.nums[i]) {
            result.push(i);
        }
    }
    return result[Math.floor(Math.random()*result.length)];
};

/**
 * Your Solution object will be instantiated and called as such:
 * var obj = new Solution(nums)
 * var param_1 = obj.pick(target)
 */
```

