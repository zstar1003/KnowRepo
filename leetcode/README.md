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