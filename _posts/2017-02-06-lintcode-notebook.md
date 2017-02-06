---
title: LintCode笔记本
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

