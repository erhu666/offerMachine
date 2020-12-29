- [十四.细节实现专题](#十四细节实现专题)
        - [7.整数反转](#7整数反转)
        - [9.回文数](#9回文数)
        - [56.合并区间](#56合并区间)
        - [57.插入区间](#57插入区间)
        - [76.最小覆盖子串](#76最小覆盖子串)
        - [415.字符串相加](#415字符串相加)
        - [43.字符串相乘](#43字符串相乘)
        - [30.串联所有单词的子串](#30串联所有单词的子串)
        - [118.杨辉三角](#118杨辉三角)
        - [119.杨辉三角2](#119杨辉三角2)
        - [54.螺旋矩阵](#54螺旋矩阵)
        - [59.螺旋矩阵2](#59螺旋矩阵2)
        - [6.Z字形变换](#6z字形变换)
        - [29.两数相除](#29两数相除)
        - [191. 位1的个数](#191-位1的个数)
        - [50.Pow(x,n)](#50powxn)
        - [68.文本左右对齐](#68文本左右对齐)
        - [149.直线上最多的点数](#149直线上最多的点数)


# 十四.细节实现专题

---------------------------
##### 7.整数反转
>题目描述:给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。
假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−2^31,  2^31 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/reverse-integer

解题思路：不断对x%10 再/10，将结果累加，并判断有没有越界。

时间复杂度：O(logX)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int reverse(int x) {
        long long ans = 0;
        while (x)
        {
            ans = ans * 10 + x%10;
            x /= 10;
            if (ans > INT32_MAX)
                return 0;
            if (ans < INT32_MIN)
                return 0;
        }
        return ans;
    }
};

```

<br>


---------------------------
##### 9.回文数
>题目描述:判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。
你能不将整数转为字符串来解决这个问题吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/palindrome-number

解题思路：该题可以用上一题的解法，将数字进行反转再判断是否相同；注意负数不是回文数。

时间复杂度：O(logX)

空间复杂度：O(1)

```cpp
class Solution {
public:
    bool isPalindrome(int x) {
        if (x < 0)
            return false;
        long res = 0, old = x;
        while (x)
        {
            res = 10 * res + x % 10;
            x /= 10;
        }
        return res==old;
    }
};

```

<br>


---------------------------
##### 56.合并区间
>题目描述:给出一个区间的集合，请合并所有重叠的区间。
intervals[i][0] <= intervals[i][1]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/merge-intervals

解题思路：直接遍历集合，比较ans.back() 是否与 遍历的元素相连，若相连接的话 修改， 没有连接的话则加入。

时间复杂度：O(NlogN)

空间复杂度：O(1)

```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end());
        vector<vector<int>> ans;
        ans.push_back(intervals[0]);
        for (int i = 1; i < intervals.size(); i++)
        {
            if (ans.back()[1] >= intervals[i][0]) //连在一起
            {
                if (intervals[i][1] > ans.back()[1])
                    ans.back()[1] = intervals[i][1];
            }else //没有连在一起
                ans.push_back(intervals[i]);
        }
        return ans;
    }
};

```

<br>



---------------------------
##### 57.插入区间
>题目描述:给出一个无重叠的 ，按照区间起始端点排序的区间列表。
在列表中插入一个新的区间，你需要确保列表中的区间仍然有序且不重叠（如果有必要的话，可以合并区间）。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/insert-interval

解题思路：直接适用上一题的方法即可，并且可以不用排序，直接找到位置插入即可。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        int i = 0;
        for (; i < intervals.size(); i++)
            if (intervals[i][0] >= newInterval[0])
                break;
        intervals.insert(intervals.begin()+i, newInterval);

        vector<vector<int>> ans;
        ans.push_back(intervals[0]);
        for (int i = 1; i < intervals.size(); i++)
        {
            if (ans.back()[1] >= intervals[i][0]) //连在一起
            {
                if (intervals[i][1] > ans.back()[1])
                    ans.back()[1] = intervals[i][1];
            }else //没有连在一起
                ans.push_back(intervals[i]);
        }
        return ans;        
    }
};

```

<br>



---------------------------
##### 76.最小覆盖子串
>题目描述:给你一个字符串 s 、一个字符串 t 。返回 s 中涵盖 t 所有字符的最小子串。如果 s 中不存在涵盖 t 所有字符的子串，则返回空字符串 "" 。
注意：如果 s 中存在这样的子串，我们保证它是唯一的答案。
s 和 t 由英文字母组成
进阶：你能设计一个在 o(n) 时间内解决此问题的算法吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/minimum-window-substring

解题思路： 先想到暴力法，两次 for循环，比较后找到最小的子串，需要用hashmap来存储t串中的元素。
再想到用滑动窗口的优化法，设置双指针i j ，当滑动窗口中包含t串中的所有元素时，i++； 没有包含t串中的所有元素时，j++。

时间复杂度：O(N) N为s串的长度

空间复杂度：O(M) M为t串的长度

```cpp
class Solution {
public:
 
bool check(unordered_map<char, int> &Smap, unordered_map<char, int> &Tmap)
{
    for (auto &e : Tmap)
    {
        if (Smap[e.first] < e.second)
            return false;
    }
    return true;
}

string minWindow(string s, string t)
{
    unordered_map<char, int> Smap, Tmap;
    for (auto &e : t)
        Tmap[e]++;
    int i = 0, j = -1, n = s.size(), minI = 0, minJ = -1, minLen = INT32_MAX;
    while (j < n)
    {
        if (check(Smap, Tmap)) //滑动窗口已经覆盖子串t , i++
        {
            while (!Tmap.count(s[i]))
                i++;
            if (j - i + 1 < minLen)
            {
                minLen = j - i + 1;
                minI=  i;
                minJ = j;
            }
            Smap[s[i]]--;
            i++;
        }
        else //滑动窗口未覆盖子串t, j++
        {
            j++;
            if (Tmap.count(s[j]))
                Smap[s[j]]++;
        }
    }
    return s.substr(minI, minJ - minI + 1);
}

};

```

<br>

---------------------------
##### 415.字符串相加
>题目描述:给定两个字符串形式的非负整数 num1 和num2 ，计算它们的和。
提示：
num1 和num2 的长度都小于 5100
num1 和num2 都只包含数字 0-9
num1 和num2 都不包含任何前导零
你不能使用任何內建 BigInteger 库， 也不能直接将输入的字符串转换为整数形式

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/add-strings

解题思路：设置进位carry,从后往前逐位进行累加即可

时间复杂度：O(max(M,N))

空间复杂度：O(1)

```cpp
class Solution {
public:
    string addStrings(string num1, string num2) {
        int i = num1.size()-1, j = num2.size()-1, carry = 0;
        string ans;
        while (i>=0 || j>=0 || carry)
        {
            int x = i>=0 ? num1[i--]-'0' : 0;
            int y = j>=0 ? num2[j--]-'0' : 0;
            int sum = x + y + carry;
            ans = to_string(sum%10) + ans;
            carry = sum/10;
        }
        return ans;
    }
};
```

<br>


---------------------------
##### 43.字符串相乘
>题目描述:给定两个以字符串形式表示的非负整数 num1 和 num2，返回 num1 和 num2 的乘积，它们的乘积也表示为字符串形式。
num1 和 num2 的长度小于110。
num1 和 num2 只包含数字 0-9。
num1 和 num2 均不以零开头，除非是数字 0 本身。
不能使用任何标准库的大数类型（比如 BigInteger）或直接将输入转换为整数来处理。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/multiply-strings

解题思路：两个for循环进行累乘，将结果进行累加

时间复杂度：O(M*N)

空间复杂度：O(M+N)

```cpp
class Solution {
public:
    string multiply(string num1, string num2) {
        if ('0'==num1[0] || '0'==num2[0])
            return "0";
        reverse(num1.begin(), num1.end());
        reverse(num2.begin(), num2.end());
        vector<int> temp(num1.size() + num2.size(), 0);
        for (int j = 0; j < num2.size(); j++)
        {
            for (int i = 0; i < num1.size(); i++)
            {
                int multi = (num1[i]-'0') * (num2[j]-'0');
                multi += temp[i+j];  //还得加上原来已有的
                temp[i+j] = multi%10;
                temp[i+j+1] += multi / 10;
            }
        }
        
        string ans;
        int i = temp.size()-1;
        while (0 == temp[i])  //跳过前面的0
            i--;
        while (i >= 0)
        {
            ans.push_back(temp[i] + '0');
            i--;
        }
        return ans;
    }
};
```

<br>




---------------------------
##### 30.串联所有单词的子串
>题目描述:给定一个字符串 s 和一些长度相同的单词 words。找出 s 中恰好可以由 words 中所有单词串联形成的子串的起始位置。
注意子串要与 words 中的单词完全匹配，中间不能有其他字符，但不需要考虑 words 中单词串联的顺序。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/substring-with-concatenation-of-all-words

解题思路：暴力法，将words看做一个字符串进行比较即可

时间复杂度：O(N^2)

空间复杂度：O(N)

```cpp
class Solution {
public:
    bool check(string s, unordered_map<string,int>& wordMap, int n, int m)
    {
        unordered_map<string,int> hashmap;
        for (int i = 0; i < s.size(); i+=m)
            hashmap[s.substr(i, m)]++;
        return hashmap==wordMap;
    }

    vector<int> findSubstring(string s, vector<string>& words) {
        unordered_map<string, int> wordMap;
        for (auto& e: words)
            wordMap[e]++;
        int n = words.size(), m = words[0].size(), wordsLen = m*n;
        vector<int> ans;

        for (int i = 0; i < s.size()-wordsLen +1; i++)
        {
            string s2 = s.substr(i, wordsLen);
            if (check(s2, wordMap, n, m))
                ans.push_back(i);
        }
        return ans;
    }
};

```

<br>


---------------------------
##### 118.杨辉三角
>题目描述:给定一个非负整数 numRows，生成杨辉三角的前 numRows 行。
在杨辉三角中，每个数是它左上方和右上方的数的和。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/pascals-triangle

解题思路：杨辉三角的本质是动态规划，直接在for循环中创建数组再加入即可。

时间复杂度：O(N^2)

空间复杂度：O(1)

```cpp
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> ans;
        for (int i = 0; i < numRows; i++)
        {
            vector<int> vec(i+1,  1);
            for (int j = 1; j < i; j++)
                vec[j] = ans[i-1][j] + ans[i-1][j-1];
            ans.push_back(std::move(vec));
        }
        return ans;
    }
};
```

<br>




---------------------------
##### 119.杨辉三角2
>题目描述:给定一个非负索引 k，其中 k ≤ 33，返回杨辉三角的第 k 行。
在杨辉三角中，每个数是它左上方和右上方的数的和。
进阶：
你可以优化你的算法到 O(k) 空间复杂度吗？
注意此题有一个坑，那就是k是从0开始取的，在程序中我们先将k+1即可。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/pascals-triangle-ii

解题思路：动态规划优化法，用两个交替的数组即可。

时间复杂度：O(K^2)

空间复杂度：O(K)

```cpp
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        rowIndex++;
        vector<int> dp0(rowIndex, 1);
        for (int i = 0; i < rowIndex; i++)
        {
            vector<int> dp1(rowIndex, 1);
            for (int j = 1; j < i; j ++)
                dp1[j] = dp0[j-1] + dp0[j];
            dp0 = dp1;
        }
        return dp0;
    }
};

```

<br>




---------------------------
##### 54.螺旋矩阵
>题目描述:给定一个包含 m x n 个元素的矩阵（m 行, n 列），请按照顺时针螺旋顺序，返回矩阵中的所有元素。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/spiral-matrix

解题思路：设置上下左右边界即可。

时间复杂度：O(M*N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        if (matrix.empty())
            return {};
        vector<int> ans;
        int M = matrix.size(), N = matrix[0].size();
        int up = 0, down = M-1, l = 0, r = N-1;//设置上下左右边界
        while (1)
        {
            for (int j = l; j <= r; j++)
                ans.push_back(matrix[up][j]);
            if (++up > down)
                break;
            for (int i = up; i <= down; i++)
                ans.push_back(matrix[i][r]);
            if (--r < l)
                break;
            for (int j = r; j >= l; j--)
                ans.push_back(matrix[down][j]);
            if (--down < up)
                break;
            for (int i = down; i >= up; i--)
                ans.push_back(matrix[i][l]);
            if (++l > r)
                break;
        }
        return ans;
    }
};

```

<br>

---------------------------
##### 59.螺旋矩阵2
>题目描述:给定一个正整数 n，生成一个包含 1 到 n^2 所有元素，且元素按顺时针顺序螺旋排列的正方形矩阵。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/spiral-matrix-ii

解题思路：螺旋遍历的方法，与上一题解法相同。

时间复杂度：O(N^2)

空间复杂度：O(1)

```cpp
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> matrix(n, vector<int>(n));
        int up = 0, down = n-1, l = 0, r = n-1;//设置上下左右边界
        int cnt = 0;
        while (1)
        {
            for (int j = l; j <= r; j++)
                matrix[up][j] = ++cnt;
            if (++up > down)
                break;
            for (int i = up; i <= down; i++)
                matrix[i][r] = ++cnt;
            if (--r < l)
                break;
            for (int j = r; j >= l; j--)
                matrix[down][j] = ++cnt;
            if (--down < up)
                break;
            for (int i = down; i >= up; i--)
                matrix[i][l] = ++cnt;
            if (++l > r)
                break;
        }
        return matrix;
    }
};

```

<br>




---------------------------
##### 6.Z字形变换
>题目描述:将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。
比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：
L   C   I   R
E T O E S I I G
E   D   H   N
之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。
请你实现这个将字符串进行指定行数变换的函数：
string convert(string s, int numRows);

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zigzag-conversion

解题思路：创建一个vector<string>数组存储Z字形变换，然后按照行进行读取即可。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    string convert(string s, int numRows) {
        if (numRows == 1)
            return s;
        vector<string> vec(numRows);
        int i = 0, add = 1;
        for (auto& e: s)
        {
            vec[i].push_back(e);
            i += add;
            if (i == numRows-1 || i==0)
                add = -add;
        }
        string ans;
        for (auto& e: vec)
            ans += e;
        return ans;
    }
};

```

<br>

---------------------------
##### 29.两数相除
>题目描述:给定两个整数，被除数 dividend 和除数 divisor。将两数相除，要求不使用乘法、除法和 mod 运算符。
返回被除数 dividend 除以除数 divisor 得到的商。
整数除法的结果应当截去（truncate）其小数部分，例如：truncate(8.345) = 8 以及 truncate(-2.7335) = -2
被除数和除数均为 32 位有符号整数。
除数不为 0。
假设我们的环境只能存储 32 位有符号整数，其数值范围是 [−2^31,  2^31 − 1]。本题中，如果除法结果溢出，则返回 2^31 − 1。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/divide-two-integers

解题思路：既然不能够使用乘除法和mod法，那么就使用不断相减的策略，计算减了多少次即可，需要用快速幂运算进行优化，其中需要注意dividend , divisor的正负号。

时间复杂度：O(logx)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int divide(int dividend, int divisor) {
        long a = dividend>=0 ? dividend : -(long)dividend;
        long b = divisor>=0 ? divisor : -(long)divisor;
        long ans = 0;
        while (a >= b)
        {
            int i = 0;
            while (a >= (b<<i))
            {
                a -= (b<<i);
                ans += (1<<i);
                i++;
            }
        } 
        ans =  ((dividend>>31)^(divisor>>31)) ? -ans : ans;
        return ans>INT32_MAX ? INT32_MAX : ans;
    }
};

```

<br>



---------------------------
##### 191. 位1的个数
>题目描述:编写一个函数，输入是一个无符号整数（以二进制串的形式），返回其二进制表达式中数字位数为 '1' 的个数（也被称为汉明重量）。
提示：
请注意，在某些语言（如 Java）中，没有无符号整数类型。在这种情况下，输入和输出都将被指定为有符号整数类型，并且不应影响您的实现，因为无论整数是有符号的还是无符号的，其内部的二进制表示形式都是相同的。
在 Java 中，编译器使用二进制补码记法来表示有符号整数。因此，在上面的 示例 3 中，输入表示有符号整数 -3。
输入必须是长度为 32 的 二进制串 。
进阶：
如果多次调用这个函数，你将如何优化你的算法？
 

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/number-of-1-bits

解题思路：可以用二进制右移不断记录二进制末尾的1即可，这样的时间复杂度为O(logX)；还可以进行优化，我们知道 n-1 可以将 n最右边的 1 变为 0111.. 这样的格式，也就是令其减少一个二进制位1，那么再 n&(n-1) 即可得到消去一个 1 之后的数，假设该数在二进制位中有k个1，那么时间复杂度为 O(k)

时间复杂度：O(k)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int ans = 0;
        while (n)
        {
            n = n&(n-1);
            ans ++;
        }
        return ans;
    }
};
```

<br>


---------------------------
##### 50.Pow(x,n)
>题目描述:实现 pow(x, n) ，即计算 x 的 n 次幂函数。
-100.0 < x < 100.0
n 是 32 位有符号整数，其数值范围是 [−2^31, 2^31 − 1] 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/powx-n

解题思路：将n看做二进制进行不断往右移位操作，当二进制n最右边为1时，说明此时可以进行累乘。

时间复杂度：O(logN)

空间复杂度：O(1)

```cpp
class Solution {
public:
    double Pow(double x, long n)
    {
        double ans = 1;
        while (n)
        {
            if (n & 0x1)
                ans *= x;
            n >>= 1;
            x *= x;
        }
        return ans;
    }

    double myPow(double x, int n) {
        if (0 == n)
            return 1;
        if (1 == n)
            return x;
        if (0 == x)
            return 0;
        if (1 == x)
            return 1;

        if (n < 0)
            return 1.0 / Pow(x, -(long)n);
        else
            return Pow(x, n);
    }
};

```

<br>


---------------------------
##### 68.文本左右对齐
>题目描述:给定一个单词数组和一个长度 maxWidth，重新排版单词，使其成为每行恰好有 maxWidth 个字符，且左右两端对齐的文本。
你应该使用“贪心算法”来放置给定的单词；也就是说，尽可能多地往每行中放置单词。必要时可用空格 ' ' 填充，使得每行恰好有 maxWidth 个字符。
要求尽可能均匀分配单词间的空格数量。如果某一行单词间的空格不能均匀分配，则左侧放置的空格数要多于右侧的空格数。
文本的最后一行应为左对齐，且单词之间不插入额外的空格。
说明:
单词是指由非空格字符组成的字符序列。
每个单词的长度大于 0，小于等于 maxWidth。
输入单词数组 words 至少包含一个单词。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/text-justification

解题思路：

时间复杂度：

空间复杂度：

```cpp

```

<br>

---------------------------
##### 149.直线上最多的点数
>题目描述:给定一个二维平面，平面上有 n 个点，求最多有多少个点在同一条直线上。
示例 1:
输入: [[1,1],[2,2],[3,3]]
输出: 3
解释:
^
|
|        o
|     o
|  o  
+------------->
0  1  2  3  4
示例 2:
输入: [[1,1],[3,2],[5,3],[4,1],[2,3],[1,4]]
输出: 4
解释:
^
|
|  o
|     o        o
|        o
|  o        o
+------------------->
0  1  2  3  4  5  6

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/max-points-on-a-line

解题思路：

时间复杂度：

空间复杂度：

```cpp

```

<br>

