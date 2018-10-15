---
title: Manacher' Algorithm
key: 201809029
tags: 算法
---

谈到之前寻找最长回文子串，采用以某个字符为中心向两边延伸，算到最长的子串，这样的时间复杂度为$O(n^2)$，后来aper的好朋友TYP同学告诉本:hot_pepper::chicken:处理回文子串有一种线性的方法叫做马拉车，于是写篇文章来学习这个算法。

为了处理方便，我们先在字符串中插入某个不存在的字符，使得所有的回文都转化为奇回文。比如:aabb->a#a#b#b#，aacbb->a#a#c#b#b。然后设Len[i]为以s[i]为中心的回文字符的半径，如果Len[i]=1，那么s[i]的回文就是其本身。

| i    | 0    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    | 10   | 11   | 12   | 13   | 14   | 15   | 16   | 17   | 18   | 19   | 20    | 21   | 22   | 23   | 24   | 25   | 26   |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ----- | ---- | ---- | ---- | ---- | ---- | ---- |
| s[i] | $    | #    | a    | #    | b    | #    | b    | #    | a    | #    | T    | #    | N    | #    | T    | #    | a    | #    | b    | #    | **c** | #    | b    | #    | a    | #    | \0   |
| p[i] |      | 1    | 2    | 1    | 2    | 5    | 2    | 1    | 2    | 1    | 2    | 1    | 8    | 1    | 2    | 1    | 2    | 1    | 2    | 1    | **6** | 1    | 2    | 1    | 2    | 1    |      |

首先从左往右依次计算$Len[i]$，当计算$Len[i]$时，$Len[j](0<=j<i$已经计算完毕。设$P$为之前计算中最长回文子串的右端点的最大值，并且设取得这个最大值的位置为$p_{o}$，分两种情况：

第一种情况：$i<=P$

那么找到i相对于$p_o$的对称位置，设为$j$，那么如果$Len[j]<P-i$，如下图：

![](http://p73rf095s.bkt.clouddn.com/18-10-11/87351599.jpg)




那么说明以j为中心的回文串一定在以$p_{o}$为中心的回文串的内部，且j和i关于位置po对称，由回文串的定义可知，一个回文串反过来还是一个回文串，所以以i为中心的回文串的长度至少和以j为中心的回文串一样，即$Len[i]>=Len[j]$。因为Len[j]<P-i$,所以说$$i+Len[j]<P$。由对称性可知$Len[i]=Len[j]$。

如果$Len[j]>=P-i,$由对称性，说明以$i$为中心的回文串可能会延伸到$P$之外，而大于$P$的部分我们还没有进行匹配，所以要从$P+1$位置开始一个一个进行匹配，直到发生失配，从而更新P和对应的$p_o$以及$Len[i]$。

第二种情况: $i>P$

6

如果$i$比$P$还要大，说明对于中点为i的回文串还一点都没有匹配，这个时候，就只能老老实实地一个一个匹配了，匹配完成后要更新P的位置和对应的$p_o$以及$Len[i]$。



```c++
string Manacher(string s)
{
    string sNew = "$#";
    for (auto iter = s.cbegin(); iter != s.cend(); iter++) {
        sNew += *iter;
        sNew += "#";
    }
    int iNewSize = sNew.size();
    int iMaxSubStringLength = -1; // 最长回文子串的长度
    int iMaxSubStringPos = -1;    // 最长回文子串中心点的位置
    vector<int> p(iNewSize, 0);
    int id = 0;
    int mx = 0;
    for (int i = 1; i < iNewSize; i++) {
        if (i < mx) {
            p[i] = min(p[2 * id - i], mx - i);
        } else {
            p[i] = 1;
        }
        while (sNew[i - p[i]] == sNew[i + p[i]]) // 最左边sNew[0]='$',最右边sNew[sNew.size()] = '\0'，无需判断边界
        {
            p[i]++;
        }
        if (mx < i + p[i]) //我们每走一步i，都要和mx比较，我们希望mx尽可能的远，这样才能更有机会执行if (i <
                           //mx)这句代码，从而提高效率
        {
            id = i;
            mx = i + p[i];
        }
        if (p[i] - 1 > iMaxSubStringLength) {
            iMaxSubStringLength = p[i] - 1;
            iMaxSubStringPos = i;
        }
    }
    auto iStart = s.cbegin() + (iMaxSubStringPos - iMaxSubStringLength - 1) / 2; // 将最长回文子串起始位置转换回原串
    return string(iStart, iStart + iMaxSubStringLength);
}
```

