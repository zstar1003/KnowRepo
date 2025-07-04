在刷题过程中，发现各种题解千奇百怪，不同的人有不同的代码风格。因此，有必要以一种统一的风格来记录题解，同时记录在刷题过程中的思考。

# 1.两数之和(t1)

简单难度，题解如下：

```c
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int i, j;
        for (i = 0; i < nums.size() - 1; i++) {
            for (j = i + 1; j < nums.size(); j++) {
                if (nums[i] + nums[j] == target) {
                    return {i, j};
                }
            }
        }
        return {};
    }
};
```

问题：`vector<int>& nums`为什么要加&？

在 C++ 里，& 有两个主要用法：

| 用法                  | 位置   | 作用                 |
| ------------------- | ---- | ------------------ |
| **取地址符号**           | 变量前面 | 获取一个变量的地址（内存位置）    |
| **引用符号** | 类型后面 | 让变量“引用”另一个变量（不是复制） |

这里是引用符号的用法。

举个例子：

```c
void sayHi(string name) {
    name = "Tom";
}
```

如果调用：
```c
string myName = "Alice";
sayHi(myName);
```

这样操作会把 myName 复制一份给了函数里的 name，两者互不影响。

如果这样定义该函数：
```c
void sayHi(string& name) {
    name = "Tom";
}
```
这时函数里的 name 和外面的 myName 是同一个变量的“别名”，所以修改 name 就会真的把 myName 改成 "Tom"。

因此，这样做可以节省内存，优化效率，从算法运行时间上也能体现出来：


![](https://files.mdnice.com/user/11855/1dc91678-88c4-4067-8ebe-3845276218d1.png)


# 2.字母异位词分组(t49)

中等难度，题目示例如下：

```bash
示例 1:

输入: strs = ["eat", "tea", "tan", "ate", "nat", "bat"]

输出: [["bat"],["nat","tan"],["ate","eat","tea"]]

解释：

在 strs 中没有字符串可以通过重新排列来形成 "bat"。
字符串 "nat" 和 "tan" 是字母异位词，因为它们可以重新排列以形成彼此。
字符串 "ate" ，"eat" 和 "tea" 是字母异位词，因为它们可以重新排列以形成彼此。
示例 2:

输入: strs = [""]

输出: [[""]]

示例 3:

输入: strs = ["a"]

输出: [["a"]]
```

标准题解：

```c
#include <iostream>
#include <vector>
#include <unordered_map>
#include <algorithm>

using namespace std;

class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> mp;
        vector<vector<string>> result;

        for (string& str : strs) {
            string key = str;
            sort(key.begin(), key.end());
            mp[key].push_back(str);
        }

        for (auto& pair : mp) {
            result.push_back(pair.second);
        }
        
        return result;
    }
};
```

这个解题思路是利用了哈希表的特性。

哈希表是一种通过**键值（key）→ 值（value）**方式存储数据的数据结构。

哈希表具有以下特点：

| 特性            | 说明                              |
| ------------- | ------------------------------- |
| ✅ **查找快**     | 平均 O(1) 时间复杂度。比数组、链表、树都快。       |
| ✅ **插入快**     | 也是平均 O(1)，很适合频繁插入的场景            |
| ✅ **key 唯一**  | 同一个 key 只会存一次，自动去重              |
| ❌ **key 无序**  | 元素顺序不固定，不可排序（这就是 `unordered`）   |
| ⚠️ **可能哈希冲突** | 不同 key 可能映射到同一个位置，需要解决冲突（比如用链表） |
| ⚠️ **内存占用较大** | 哈希表牺牲空间换时间，用空桶提升性能              |


假设输入内容为：
```c
vector<string> strs = {"eat", "tea", "tan", "ate", "nat", "bat"};
```

经过sort之后，`eat tea ate`的 key 值统一变成`aet`。

unordered_map 类型的 mp 内容如下：

```bash
key = aet → eat tea ate 
key = ant → tan nat 
key = abt → bat 
```

之后再取出其 value 值(pair.second)就能得到结果。

# 3.最长连续序列(t128)

中等难度，题目示例如下：

```bash
示例 1：

输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
示例 2：

输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9
示例 3：

输入：nums = [1,0,1,2]
输出：3
```

暴力解法：先对列表里的数字进行排序，然后进行进行逐项遍历，用一个变量记录连续最大值。

```c
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
       if  (nums.empty()) return 0;

       sort (nums.begin(), nums.end());
       
       int longest = 1;
       int currentLength = 1;
       
       for (int i = 1; i < nums.size(); i++){
        if (nums[i] == nums[i - 1]){
            continue;
        }
        else if (nums[i] == nums[i - 1] + 1){
            currentLength += 1;
        }
        else{
            longest = max(longest, currentLength);
            currentLength = 1;
        }
       }
       return max(longest, currentLength);
    }
};
```

sort排序的时间复杂度是 O(n log n)，遍历的时间复杂度为 O(n)。

总的时间复杂度为 O(n log n) + O(n) = O(n log n)

更好的思路：借助哈希表，查找速度快的特点，把所有数字放进一个集合（unordered_set），然后从每个数字往上找 x+1, x+2 是否存在，统计长度；

具体解法：

```c
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
       unordered_set<int> numSet(nums.begin(), nums.end());
       int longest = 0;

       for (int num : numSet){
        // 从最小值num开始查找
        if (!numSet.count(num -1)){
            int currentNum = num;
            int currentLength = 1;

            while (numSet.count(currentNum + 1)){
                currentNum += 1;
                currentLength += 1; 
            }
            
            longest = max(longest, currentLength);
        }
       }
       
       return longest;
    }
};
```

在这个解法中，每个元素最多访问一次，时间复杂度是O(n)。

# 1. 移动零(t283)

简单难度，题目示例如下：

```bash
给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

示例 1:

输入: nums = [0,1,0,3,12]
输出: [1,3,12,0,0]
示例 2:

输入: nums = [0]
输出: [0]
```

暴力解法：从头开始遍历，如果遇到非零元素，将其交换到末尾。

```c
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
       for (int i = 0; i < nums.size(); i++){
            if (nums[i] == 0){
                for (int j = i + 1; j < nums.size(); j++){
                    if (nums[j] != 0){
                        swap(nums[i], nums[j]);
                        break;
                    }
                }
            }
       }
    }
};
```

时间复杂度: O($n^2$)； 空间复杂度：O(1)

更好的解法：使用双指针的思路，左指针指向当前已经处理好的序列的尾部，右指针指向待处理序列的头部。右指针不断向右移动，每次右指针指向非零数，则将左右指针对应的数交换，同时左指针右移。

```c
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int n = nums.size(), left = 0, right = 0;
        while (right < n){
            if (nums[right]){
                swap(nums[left], nums[right]);
                left++;
            }
            right++;
        }
    }
};
```

时间复杂度: O(n)； 空间复杂度：O(1)

# 2. 盛最多水的容器(t11)

中等难度，题目示例如下：
```bash
给定一个长度为 n 的整数数组 height 。有 n 条垂线，第 i 条线的两个端点是 (i, 0) 和 (i, height[i]) 。

找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

返回容器可以储存的最大水量。

```

![](https://files.mdnice.com/user/11855/99a5ecb2-d941-4540-8287-4a035e70c70d.png)

解法思路：这套题的基础思路很简单，用双指针一头一尾。但是指针的移动条件，比较巧妙，通过比较左右两块板的高度，如果一侧较低，就移动。一句话概括：尽量手持大的板，去找更大的板。

```bash
class Solution {
public:
    int maxArea(vector<int>& height) {
        int n = height.size(), left = 0, right = n - 1; 
        int area = 0, max_area = 0;
        while (left < right){
            area = min(height[left], height[right]) * (right - left);
            if (area > max_area) max_area = area;
            if (height[left] < height[right]){
                left ++;
            }
            else{
                right --;
            }
        }
        return max_area;
    }
};
```

时间复杂度: O(n)； 空间复杂度：O(1)。

看到题解下面的有条评论说得很精彩，摘录如下：

> by Quirky Satoshi4QI:
> 这个思路我理解了很久。本质上这是一个如何去简化两个for循环的问题。可以这么来理解：假设本身要用
> 
> x y作为数组下标分别遍历才能完成所有情况的枚举，那么怎么才能简化呢？我们假设x是从左往右递增的，y是从右往左递减的，那么当x的木板比y的木板短时，说明这一轮的y的遍历提前结束，后面不论y如何向左移动都没用了。 当这一轮结束后，x向右指向一个，就变成了求解同样的一个新问题，只不过求解的范围缩小了而已。 最终xy相遇之后，所有的该遍历的都遍历到了，被简化掉的遍历项目就是我们这个算法的收益。

# 3. 三数之和(t15)

中等难度，题目示例如下：

```bash
给你一个整数数组 nums ，判断是否存在三元组 [nums[i], nums[j], nums[k]] 满足 i != j、i != k 且 j != k ，同时还满足 nums[i] + nums[j] + nums[k] == 0 。请你返回所有和为 0 且不重复的三元组。

示例 1：

输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。
示例 2：

输入：nums = [0,1,1]
输出：[]
解释：唯一可能的三元组和不为 0 。
示例 3：

输入：nums = [0,0,0]
输出：[[0,0,0]]
解释：唯一可能的三元组和为 0 。
```

解题思路：这题涉及到三元组，初看没想到如何求解，看了题解才清楚思路。这道题采用双指针的思路，核心解决两个问题：1.如何去重？2.双指针如何设置？

具体思路是先对数组进行从小到大排序，遍历每一个元素位置，双指针区间初始设置在该元素后面，通过相邻元素判断来解决去重问题。

```bash
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        int n = nums.size();
        sort(nums.begin(), nums.end());
        vector<vector<int>> ans;
        
        for (int i = 0; i < n; i++){
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            
            int l = i + 1, r = n - 1;
            
            while (l < r){
                if (nums[l] + nums[r] + nums[i] == 0){
                    ans.push_back({nums[i], nums[l], nums[r]});
                    l++;
                    r--;
                    while (l < r && nums[l] == nums[l - 1]) l++;
                    while (l < r && nums[r] == nums[r + 1]) r--;
                }
                else if (nums[l] + nums[r] + nums[i] < 0){
                    l++;
                }
                else {
                    r--;
                }
            }
        }
        return ans;
    }
}; 
```

时间复杂度：O($N^2$)，引入双指针的核心作用就是让原本需要三次循环减少到两次，左右指针往中心靠近的过程，减少了一次循环带来的计算量。

空间复杂度：O(logN)主要来自于排序。

# 4. 接雨水(t42)

困难难度，题目示例如下：

![](https://files.mdnice.com/user/11855/882c2b05-7077-4d03-a03b-5d7e90d0265c.png)

```bash
给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 
示例 2：

输入：height = [4,2,0,3,2,5]
输出：9
```

解题思路：题目涉及物理模拟，初见比较难，没什么思路，直接看题解，官方提供了三种解题思路。

思路1：动态规划

动态规划的核心就是做问题拆解，因此，可以将问题拆成三个步骤：
- 1.假设右边无限高，那么接水量只取决于左边，先统计这种情况下每列最多能接多少水。
- 2.假设左边无限高，那么接水量只取决于右边，再统计这种情况下每列最多能接多少水。
- 3.最终实际的接水量就是两者最小值 - 本身容器高度。


![](https://files.mdnice.com/user/11855/c2151ab9-e796-447a-aebc-2bc6c89514d8.png)


```c
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size();
        if (n == 0) {
            return 0;
        }
        vector<int> leftMax(n);
        leftMax[0] = height[0];
        for (int i = 1; i < n; i++) {
            leftMax[i] = max(leftMax[i - 1], height[i]);
        }

        vector<int> rightMax(n);
        rightMax[n - 1] = height[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            rightMax[i] = max(rightMax[i + 1], height[i]);
        }

        int ans = 0;
        for (int i = 0; i < n; i++) {
            ans += min(leftMax[i], rightMax[i]) - height[i];
        }
        return ans;
    }
};
```

时间复杂度：O(n)，共遍历数组三次。

空间复杂度：O(n)，额外创建 leftMax 和 rightMax 两个数组。

思路2：单调栈

单调栈是指栈内元素按照单调顺序排列的栈。例如，单调递减栈，如果遇到比栈顶大的元素，就开始出栈，直到再次保持单调递减。

单调递减栈做这道题很合适，从左往右遍历，当遇到比栈顶还高的柱子时，就说明右边界出现了，可以开始计算中间能接多少水。

![](https://files.mdnice.com/user/11855/8b90d4d9-534f-4c80-8f69-113d0c57cef2.png)

```bash
class Solution {
public:
    int trap(vector<int>& height) {
        int ans = 0;
        stack<int> stk;
        int n = height.size();
        for (int i = 0; i < n; i++) {
            while (!stk.empty() && height[i] > height[stk.top()]) {
                int top = stk.top();
                stk.pop();
                if (stk.empty()) {
                    break;
                }
                int left = stk.top();
                int currWidth = i - left - 1;
                int currHeight = min(height[left], height[i]) - height[top];
                ans += currWidth * currHeight;
            }
            stk.push(i);
        }
        return ans;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(n)

思路3：双指针

双指针实际上是对思路1的优化，通过左右两个指针不断往中间靠近，减小循环次数。核心思路是：如果左边更矮，那么当前左边所能接的水只受左边的最大值控制；如果右边更矮，就由右边最大值控制。非常巧妙，边移动边计算。

![](https://files.mdnice.com/user/11855/5482fd22-2a49-4eca-ab48-94145f433790.png)

```bash
class Solution {
public:
    int trap(vector<int>& height) {
        int ans = 0;
        int left = 0, right = height.size() - 1;
        int leftMax = 0, rightMax = 0;
        while (left < right) {
            leftMax = max(leftMax, height[left]);
            rightMax = max(rightMax, height[right]);
            if (height[left] < height[right]) {
                ans += leftMax - height[left];
                ++left;
            } else {
                ans += rightMax - height[right];
                --right;
            }
        }
        return ans;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)

# 1. 无重复字符的最长子串(t3)

中等难度，题目示例如下：

```bash
给定一个字符串 s ，请你找出其中不含有重复字符的最长子串的长度。

示例 1:

输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
示例 2:

输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
示例 3:

输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

解题思路：第一个想法就用纯暴力的方式去做，挨个元素开始遍历，用unordered_set来去重，maxLen记录每轮循环的最大值。

```c
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int maxLen = 0;
        int n = s.size();

        for (int i = 0; i < n; i++){
            unordered_set<char> charSet;
            for (int j = i; j < n; j++){
                if (charSet.count(s[j])) break;
                charSet.insert(s[j]);
                if (maxLen < (j - i + 1)) maxLen = j - i + 1;
            }
        }
        
        return maxLen;
    }
};
```

这样需要两个循环，时间复杂度是O($N^2$)

这道题官方更推荐采用**滑动窗口**的思路去做：滑动窗口有点类似于双指针的思路，通过两个指针表示字符串中的某个子串（窗口）的左右边界，左指针从起始位为开始遍历，右指针不断向右拓展，找到集合中无重复的字符就加入窗口范围。

```bash
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_set<char> charSet;
        int n = s.size();
       // 右指针，初始值为 -1，相当于我们在字符串的左边界的左侧，还没有开始移动
        int rk = -1, ans = 0;
        // 枚举左指针的位置，初始值隐性地表示为 -1
        for (int i = 0; i < n; i++){
            if (i != 0) {
                // 左指针向右移动一格，移除一个字符
                charSet.erase(s[i - 1]);
            }
            while (rk + 1 < n && !charSet.count(s[rk + 1])) {
                // 不断地移动右指针
                charSet.insert(s[rk + 1]);
                rk++;
            }
            // 第 i 到 rk 个字符是一个极长的无重复字符子串
            ans = max(ans, rk - i + 1);
        }
        
        return ans;
    }
};
```

下面的例子能够更好地理解滑动窗口的思路：

对于字符串`abcabcbb`

暴力解法：
- i = 0 → 检查 abc → 合法

- i = 1 → 检查 bca → 合法，但其实“bc”已经在之前窗口检查过了，又重新检查一次（重复计算）

滑动窗口：
- i = 0 → 构建 abc

- i = 1 → 只移除 'a'，窗口右边已构建好，直接继续滑动，无需重建窗口

因此，滑动窗口最大优化点就是窗口内本身的元素不需要重复访问，这就让复杂度减少了一倍，时间复杂度降为 O(N)。

# 2. 找到字符串中所有字母异位词(t438)

中等难度，题目示例如下：

```bash
给定两个字符串 s 和 p，找到 s 中所有 p 的 异位词 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。

字母异位词是通过重新排列不同单词或短语的字母而形成的单词或短语，并使用所有原字母一次。

示例 1:

输入: s = "cbaebabacd", p = "abc"
输出: [0,6]
解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。

示例 2:

输入: s = "abab", p = "ab"
输出: [0,1,2]
解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的异位词。
```

这道题思路和上面一题如出一辙，主要区别是异形词的判断会比重复词更复杂一些。遇事不决先来个纯暴力的方式，异形词的判断直接通过排序，如果排序后一致，则表明是异形词。

```bash
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        vector<int> result;
        int sLen = s.size(), pLen = p.size();
        if (sLen < pLen) return result;

        // 对 p 排序作为比较基准
        string sortedP = p;
        sort(sortedP.begin(), sortedP.end());

        // 枚举 s 中所有长度为 pLen 的子串
        for (int i = 0; i <= sLen - pLen; ++i) {
            string sub = s.substr(i, pLen);
            sort(sub.begin(), sub.end());  // 排序子串
            if (sub == sortedP) {
                result.push_back(i);  // 是异位词
            }
        }

        return result;
    }
};
```
时间复杂度是 O(n * m log m)，即接近 n 次循环，每次循环的排序时间复杂度为 m log m。

提交上去，超时。

下面用滑动窗口的思路进行优化，核心思想是不用排序，而采用计数数组，因为异形词本身就需要交换顺序，因此用统计窗口内26个字母的相应数量和p一致就行了。这里也用到了独热编码的思想，计数数组构建成一个长度为26的数组。

```bash
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        vector<int> result;
        int sLen = s.size(), pLen = p.size();
        if (sLen < pLen) return result;

        // 统计 p 中每个字符的频率
        vector<int> countP(26), countS(26);
        for (char c : p) {
            countP[c - 'a']++;
        }

        // 初始化前 pLen 长度的窗口
        for (int i = 0; i < pLen; ++i) {
            countS[s[i] - 'a']++;
        }
        if (countS == countP) result.push_back(0);

        // 开始滑动窗口
        for (int i = pLen; i < sLen; ++i) {
            // 移出窗口左边的字符
            countS[s[i - pLen] - 'a']--;
            // 加入窗口右边的新字符
            countS[s[i] - 'a']++;

            if (countS == countP) {
                result.push_back(i - pLen + 1);
            }
        }

        return result;
    }
};
```

这样时间复杂度变成 O(n + 2p)，n是循环遍历的时间，p是初始化频率数组和初始化窗口的时间。