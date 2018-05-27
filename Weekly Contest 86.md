## Weekly Contest 86
---
这次周赛又是比较凉，只写出前两道题...  

查了下资料又看了眼其他大牛的代码，下面给出自己的解答
### 矩阵中的幻方
3 x 3 的幻方是一个填充有**从 1 到 9 **的不同数字的 3 x 3 矩阵，其中每行，每列以及两条对角线上的各数之和都相等。  

给定一个由整数组成的 N × N 矩阵，其中有多少个 3 × 3 的 “幻方” 子矩阵？（每个子矩阵都是连续的）。  

**示例:**  
> **输入:** [ [4,3,8,4],  
> 　　　[9,5,1,9],  
> 　　　[2,7,6,2] ]    
> **输出:** 1   
> **解释:**   
> 下面的子矩阵是一个 3 x 3 的幻方：  
> 438  
> 951  
> 276  
> 而这一个不是：  
> 384  
> 519  
> 762  
> 总的来说，在本示例所给定的矩阵中只有一个 3 x 3 的幻方子矩阵。　

**提示:**  
1. 1 <= grid.length = grid[0].length <= 10
2. 0 <= grid[i][j] <= 15

**Sollution:**  
这道题是easy难度的，比较简单。想法分为两部分：
1. 从输入矩阵中提取所有 3 x 3 的子矩阵
2. 判断 3 x 3 的子矩阵 是否为幻方矩阵
3. 需要注意的是，每个元素的取值。幻方矩阵中元素的值必须为 1 - 9，而输入矩阵元素的值为 0 - 15

```cpp
bool isHuanfang(vector<vector<int>>& g) {
    if (g[0][0] + g[2][2] != g[2][0] + g[0][2]) return false;
    int sum = g[0][0] + g[1][1] + g[2][2];
    for (int i = 0; i < 3; i++) {
        if (g[i][0] + g[i][1] + g[i][2] != sum) {
            return false;
        }
        if (g[0][i] + g[1][i] + g[2][i] != sum) {
            return false;
        }
    }
    return true;
}

class Solution {
public:
    int numMagicSquaresInside(vector<vector<int>>& grid) {
        int n = grid.size();
        int m = grid[0].size();
        if (n < 3 || m < 3) {
            return 0;
        }
        int count = 0;
        for (int i = 0; i < n - 2; i++) {
            for (int j = 0; j < m - 2; j++) {
                bool big = true;
                vector<vector<int> > subgrid;
                for (int x = 0; x < 3; x++) {
                    vector<int> temp;
                    for (int y = 0; y < 3; y++) {
                        if (grid[i + x][j + y] < 1 || grid[i + x][j + y] > 9)  big = false;
                        temp.push_back(grid[i + x][j + y]);
                    }
                    subgrid.push_back(temp);
                }
                if (isHuanfang(subgrid) && big) count++;
            }
        }
        return count;
    }
};
```

---
### 钥匙和房间
有 N 个房间，开始时你位于 0 号房间。每个房间有不同的号码：0，1，2，...，N-1，并且房间里可能有一些钥匙能使你进入下一个房间。  

在形式上，对于每个房间 i 都有一个钥匙列表 rooms[i]，每个钥匙 rooms[i][j] 由 [0,1，...，N-1] 中的一个整数表示，其中 N = rooms.length。 
钥匙 rooms[i][j] = v 可以打开编号为 v 的房间。  

最初，除 0 号房间外的其余所有房间都被锁住。  

你可以自由地在房间之间来回走动。  

如果能进入每个房间返回 true，否则返回 false。  

**示例1:**  
> **输入:** [[1],[2],[3],[]]     
> **输出:** true   
> **解释:**   
> 我们从 0 号房间开始，拿到钥匙 1。  
> 之后我们去 1 号房间，拿到钥匙 2。  
> 然后我们去 2 号房间，拿到钥匙 3。  
> 最后我们去了 3 号房间。  
> 由于我们能够进入每个房间，我们返回 true。  

**示例2:**  
> **输入:** [[1,3],[3,0,1],[2],[0]]  
> **输出:** false   
> **解释:** 我们不能进入 2 号房间。

**Solution:**  
感觉第二题比第一题更简单一些。思路如下：
1. 初始化每个房间的状态，除了0号房间可达，其他房间均不可达
2. 从0号房间开始遍历，更新每个房间的状态。
3. 遍历完成后，如果仍有房间不可达，则返回false; 如果所有房间均可达，返回true

```cpp
class Solution {
public:
    bool canVisitAllRooms(vector<vector<int>>& rooms) {
        int n = rooms.size();
        vector<int> available;
        available.push_back(1);
        for (int i = 1; i < n; i++) available.push_back(0);
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (available[j]) {
                    int len = rooms[j].size();
                    for (int k = 0; k < len; k++) {
                        available[rooms[j][k]] = 1;
                    }
                }
            }
        }
        for (int i = 1; i < n; i++) {
            if (!available[i]) return false;
        }
        return true;
    }
};
```

---
### 将数组拆分成斐波那契序列
给定一个数字字符串 S，比如 S = "123456579"，我们可以将它分成斐波那契式的序列 [123, 456, 579]。  

形式上，斐波那契式序列是一个非负整数列表 F，且满足：

- 0 <= F[i] <= 2^31 - 1，（也就是说，每个整数都符合 32 位有符号整数类型）；
- F.length >= 3；
- 对于所有的0 <= i < F.length - 2，都有 F[i] + F[i+1] = F[i+2] 成立。
另外，请注意，将字符串拆分成小块时，每个块的数字一定不要以零开头，除非这个块是数字 0 本身。

返回从 S 拆分出来的所有斐波那契式的序列块，如果不能拆分则返回 []。  

**示例1:**  
> **输入:** "123456579"     
> **输出:** [123,456,579]   

**示例2:**  
> **输入:** “11235813"     
> **输出:** [1,1,2,3,5,8,13]   

**示例3:**  
> **输入:** "112358130"     
> **输出:** []   
> **解释：** 这项任务无法完成

**示例4:**  
> **输入:** "0123"     
> **输出:** []   
> **解释：**每个块的数字不能以零开头，因此 "01"，"2"，"3" 不是有效答案。

**示例5:**  
> **输入:** "1101111"     
> **输出:** [110, 1, 111]   
> **解释：**输出 [11,0,11,11] 也同样被接受。

### 猜猜这个单词
这个问题是 LeetCode 平台新增的交互式问题 。

我们给出了一个由一些独特的单词组成的单词列表，每个单词都是 6 个字母长，并且这个列表中的一个单词将被选作秘密。

你可以调用 master.guess(word) 来猜单词。你所猜的单词应当是存在于原列表并且由 6 个小写字母组成的类型字符串。

此函数将会返回一个整型数字，表示你的猜测与秘密单词的准确匹配（值和位置同时匹配）的数目。此外，如果你的猜测不在给定的单词列表中，它将返回 -1。

对于每个测试用例，你有 10 次机会来猜出这个单词。当所有调用都结束时，如果您对 master.guess 的调用不超过 10 次，并且至少有一次猜到秘密，那么您将通过该测试用例。

除了下面示例给出的测试用例外，还会有 5 个额外的测试用例，每个单词列表中将会有 100 个单词。这些测试用例中的每个单词的字母都是从 'a' 到 'z' 中随机选取的，并且保证给定单词列表中的每个单词都是唯一的。

**示例1:**  
> **输入:** secret = "acckzz", wordlist = ["acckzz","ccbazz","eiowzz","abcczz"]  
> **解释:**    
> master.guess("aaaaaa") 返回 -1, 因为 "aaaaaa" 不在 wordlist 中.  
master.guess("acckzz") 返回 6, 因为 "acckzz" 就是秘密，6个字母完全匹配。  
master.guess("ccbazz") 返回 3, 因为 "ccbazz" 有 3 个匹配项。  
master.guess("eiowzz") 返回 2, 因为 "eiowzz" 有 2 个匹配项。  
master.guess("abcczz") 返回 4, 因为 "abcczz" 有 4 个匹配项。  
我们调用了 5 次master.guess，其中一次猜到了秘密，所以我们通过了这个测试用例。  

**Solution：**
其实这道题蛮可惜的，哎，就差一个用例没有通过。
思路：
1. 从输入的单词列表中，随机选择一项作为secret
2. 将单词列表中的每一项都与secret进行比较，如果比较的结果与master.guess()的结果一致，存入候选队列
3. 从候选队列中随机选取一项组为secret
4. 重复2和3，直到完成10次猜测

之前代码没通过的原因：  
每次选取，都是选取候选队列的第一项作为secret... 果然还是随机选取好

```cpp
/**
 * // This is the Master's API interface.
 * // You should not implement it, or speculate about its implementation
 * class Master {
 *   public:
 *     int guess(string word);
 * };
 */
int isSame(string s1, string s2) {
    int count = 0;
    for (int i = 0; i < 6; i++) {
        if (s1[i] == s2[i]) count++;
    }
    return count;
}

class Solution {
public:
    void findSecretWord(vector<string>& wordlist, Master& master) {
        int len = wordlist.size();
        int sel = rand() % wordlist.size();
        string secret = wordlist[sel];
        int same = master.guess(secret);
        vector<string> indicate;
        for (int i = 0; i < len; i++) {
            if (isSame(wordlist[i], secret) == same) indicate.push_back(wordlist[i]);
        }
        int count = 9;
        while (count > 0) {
            sel = rand() % indicate.size();
            secret = indicate[sel];
            same = master.guess(secret);
            for (int i = 0; i < indicate.size(); i++) {
                if (isSame(indicate[i], secret) != same) {
                    indicate.erase(indicate.begin() + i);
                    i--;
                }
            }
            count--;
        }
    }
};
```
