---
title: LintCode笔记本
mathjax: true
---

## 158. 两个字符串是变位词 | two-strings-are-anagrams

> 写出一个函数 anagram(s, t) 判断两个字符串是否可以通过改变字母的顺序变成一样的字符串。

也就是说，要判断两个词的字母“成分”是否一致。那么我的思路是数一下source和target中每种字母的出现次数，如果相同，那么就是一个变位词。

{% highlight python %}
class Solution:
    """
    @param s: The first string
    @param b: The second string
    @return true or false
    """
    
    def anagram(self, s, t):
        bucket_s = {}
        bucket_t = {}
        for char in s:
            if char not in bucket_s:
                bucket_s[char] = 1
            else:
                bucket_s[char] += 1
    
        for char in t:
            if char not in bucket_t:
                bucket_t[char] = 1
            else:
                bucket_t[char] += 1
        return bucket_s == bucket_t

{% endhighlight %}
去找了一下其他一些网站比如[这里](http://www.jiuzhang.com/solutions/two-strings-are-anagrams/)的解法，发现还有一种思路也用的比较多，就是将两个字符串打散成数组以后排序，然后判断是否相等。写出来更简洁一些。

{% highlight python %}
class Solution:
    """
    @param s: The first string
    @param b: The second string
    @return true or false
    """
    def anagram(self, s, t):
        # write your code here
        ss = [i for i in s]
        tt = [i for i in t]
        ss.sort()
        tt.sort()
        return "".join(ss) == "".join(tt)
{% endhighlight %}

不过讲道理用排序的话复杂度不就起码是O(nlogn)了么……统计字母出现次数的方法应该是O(n)

顺便一个比较残念的地方是虽然LintCode支持中文界面，但是没有讨论区……

## 55. 比较字符串 | compare-strings

>比较两个字符串A和B，确定A中是否包含B中所有的字符。字符串A和B中的字符都是**大写字母**。在 A 中出现的 B 字符串里的字符不需要连续或者有序。

那么和158类似，思路还是统计字母的出现频数。不过这次只要一个字典就够了，处理B的时候进行-1计数。

{% highlight python %}

class Solution:

    """
    @param A : A string includes Upper Case letters
    @param B : A string includes Upper Case letters
    @return :  if string A contains all of the characters in B return True else return False
    """
    
    def compareStrings(self, A, B):
        bucket = {}
        for char in A:
            if char not in bucket:
                bucket[char] = 1
            else:
                bucket[char] += 1
    
        try:
            for char in B:
                bucket[char] -= 1
        except KeyError:
            return False
    
        flag = True
        for char in bucket:
            if bucket[char] < 0:
                flag = False
                break
    
        return flag

{% endhighlight %}

这里用了一个try-except来应付B中出现了A里没有的字符的情况。

## 13. 字符串查找 | strstr

>对于一个给定的 source 字符串和一个 target 字符串，你应该在 source 字符串中找出 target 字符串出现的第一个位置(从0开始)。如果不存在，则返回 -1。

{% highlight python %}
class Solution:

    def strStr(self, source, target):
        if source is None or target is None:
            return -1
        return source.find(target)

{% endhighlight %}

来和我一起念：Python大法好！

<small>#开玩笑的</small>

理论上来说不用Python内置的方法的话也很容易想出O(n<sup>2</sup>)的算法。不过O(n)的算法也是有的([KMP算法](https://zh.wikipedia.org/wiki/%E5%85%8B%E5%8A%AA%E6%96%AF-%E8%8E%AB%E9%87%8C%E6%96%AF-%E6%99%AE%E6%8B%89%E7%89%B9%E7%AE%97%E6%B3%95))，可惜我暂时还不会_(:з」∠)_

## 171. 乱序字符串 | anagrams

>给出一个字符串数组S，找到其中所有的乱序字符串(Anagram)。如果一个字符串是乱序字符串，那么他存在一个字母集合相同，但顺序不同的字符串也在S中。所有的字符串都只包含小写字母。
>
>例如对于字符串数组`["lint","intl","inlt","code"]`返回`["lint","inlt","intl"]`

应用了一下158中字符串排序的思路，统计每个单词的字符组合的出现次数，根据题目约定，出现多于一次的那个就对应要求的Anagram。

{% highlight python %}
class Solution:
    # @param strs: A list of strings
    # @return: A list of strings

    def anagrams(self, strs):
        bucket = {}
        anag = []
        for word in strs:
            chars = [c for c in word]
            chars.sort()
            chars = "".join(chars)
            if chars not in bucket:
                bucket[chars] = 1
            else:
                bucket[chars] += 1

        for word in strs:
            chars = [c for c in word]
            chars.sort()
            chars = "".join(chars)
            if bucket[chars] > 1:
                anag.append(word)

        return anag

{% endhighlight %}

## 78. 最长公共前缀 | longest-common-prefix

> 给k个字符串，求出他们的最长公共前缀(LCP)
>  
> eg. 在 "ABCD" "ABEF" 和 "ACEF" 中,  LCP 为 "A"  
> 在 "ABCDEFG", "ABCEFG", "ABCEFA" 中, LCP 为 "ABC"

那么基本思路就是逐个比对每个字符串的第n个字符了。这是“纵向优先”的方法。

|  0   |  1   |  2   |  3   |  4   |  5   |  6   |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|  A   |  B   |  C   |  D   |  E   |  F   |  G   |
|  A   |  B   |  C   |  E   |  F   |  G   |      |
|  A   |  B   |  C   |  E   |  F   |  A   |      |

{% highlight python %}
class Solution:
    # @param strs: A list of strings
    # @return: The longest common prefix

    def longestCommonPrefix(self, strs):
        if len(strs) == 0:
            return ""
        if len(strs) == 1:
            return strs[0]
        length = min([len(s) for s in strs])
        if length == 0:
            return ""
        for n in range(0, length):
            nthchars = [word[n] for word in strs]
            flag = True
            for i in range(0, len(nthchars) - 1):
                flag = flag and (nthchars[i] == nthchars[i + 1])
            if not flag:
                return strs[0][:n]
        return strs[0]

{% endhighlight %}

这里把每一列的字符都抽出来作为了一个数组，实际上应该还可以再简化一些。

“横向优先”的思路也是可以的，[这里](http://www.chenguanghe.com/lintcode-longest-common-prefix/)有个例子：

{% highlight java %}
public String longestCommonPrefix(String[] strs) {
    if(strs == null || strs.length == 0)
        return "";
    String prefix = strs[0];
    for(int i = 1; i < strs.length; i++) {
        int j = 0;
        while(j < prefix.length() && j < strs[i].length() && prefix.charAt(j)==strs[i].charAt(j))
            j++;
        prefix = prefix.substring(0,j);
    }
    return prefix;
}
{% endhighlight %}

顺便这题有的testcase有点坑的感觉。会有`[]` `["ABC"]` `["ABC","","AB"]`这样的情况出现，要注意一下（被这些testcase给WA了好多次的我无奈地说道）

## 79. 最长公共子串 | longest-common-substring

>给出两个字符串，找到最长公共子串，并返回其长度。  
>子串的字符应该连续的出现在原字符串中，这与子序列有所不同。  
> 
>eg. 给出`A=“ABCD”`，`B=“CBCE”`，返回`2`  

乍一看以为和最长公共子序列差不多，改造一下就好，于是先顺手实现了递归版的最长公共子序列。

{% highlight python %}
def LCS(A, B):
    if A == "" or B == "":
        return ""

    if A[-1:] == B[-1:]:
        return LCS(A[:-1], B[:-1]) + A[-1:]
    else:
        x = LCS(A[:-1], B)
        y = LCS(A, B[:-1])
        if len(x) > len(y):
            return x
        else:
            return y

{% endhighlight %}

然后发现好像没那么好改……

而且讲道理，用递归来算LCS会导致大量的重复计算，这个就是递归最痛苦的，而且这个效率efficiency……

还是应该用动态规划(?)来做。

{% highlight python %}
class Solution:
    # @param A, B: Two string.
    # @return: the length of the longest common substring.

    def longestCommonSubstring(self, A, B):
        if A == "" or B == "":
            return 0

        arr = [[0 for j in range(len(A))] for i in range(len(B))]

        for i in range(len(A)):
            if A[i] == B[0]:
                arr[0][i] = 1
            else:
                arr[0][i] = 0

        for j in range(len(B)):
            if A[0] == B[j]:
                arr[j][0] = 1
            else:
                arr[j][0] = 0

        for i in range(1, len(A)):
            for j in range(1, len(B)):
                if A[i] == B[j]:
                    arr[j][i] = arr[j - 1][i - 1] + 1

        return max(list(map(max, arr)))

{% endhighlight %}

这里相当于列了一张二维表格`arr[i][j]`来记录了`A[:j]`和`B[:i]`的最长公共子串的长度。

|       |      | **A** | **B** | **A** | **B** |
| :---: | :--: | :---: | :---: | :---: | :---: |
|       |  0   |   0   |   0   |   0   |   0   |
| **B** |  0   |   0   | **1** |   0   |   1   |
| **A** |  0   | **1** |   0   | **2** |   0   |
| **B** |  0   |   0   | **2** |   0   |   3   |
| **A** |  0   |   1   |   0   | **3** |   0   |

列出计算方程的话就是这样的

$$
\mathit{LCSuffix}(S_{1..p}, T_{1..q}) =
\begin{cases}
       \mathit{LCSuffix}(S_{1..p-1}, T_{1..q-1}) + 1  & \mathrm{if } \; S[p] = T[q] \\
       0                                            & \mathrm{otherwise}.
\end{cases}
$$

$$
\mathit{LCSubstr}(S, T) = \max_{1 \leq i \leq m, 1 \leq j \leq n} \mathit{LCSuff}(S_{1..i}, T_{1..j}) \;
$$

此外做这道题的时候最痛苦的就是i和j方向经常弄混……保持头脑清醒很重要啊╮（﹀＿﹀）╭