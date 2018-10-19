---
title: Manacher' Algorithm
key: 201809029
tags: 算法
---

谈到之前寻找最长回文子串，采用以某个字符为中心向两边延伸，算到最长的子串，这样的时间复杂度为$O(n^2)$，后来aper的好朋友TYP同学告诉本:hot_pepper::chicken:处理回文子串有一种线性的方法叫做马拉车，于是写篇文章来学习这个算法。

为了将奇回文和偶回文都同时转化成奇回文，将所有的字符之间都插入'#'，比如abab->a#b#a#b，aba->a#b#a。为了防止溢出，在最前面添加一个符号'-'（最后面因为有'\0'所以不必要）。manacher的思想就是从左到右求出以每个位置为中心的最长字符串（由于回文串很明显的包含和对称性质，以同一位置为中心的字符串自然是包含在最长回文串里），利用回文串的**对称性质**，对每一个位置利用之前的信息来快速得到答案。近似于扫一遍，所以均摊$O(n)$

我们记向右延伸最远的回文串（有多个的话，选中心点最靠左的），令其右端点为mx，中心点的下标为id。我们需要处理数组pal[i]，表示以i为中心的回文串的右端到i的长度。然后分别有三种情况进行讨论

1.i<=mx，由于对称性可得

![iwIRHA.png](https://s1.ax1x.com/2018/10/19/iwIRHA.png)

根据图可以直接得到pal[i]=pal[2*id-i]

2.i<=mx，但是其对应点的范围超出了id的范围，在id范围内的可以通过p[2*id-1]来求得，但是之后的要通过暴力自己去求解。

![iwI2Bd.png](https://s1.ax1x.com/2018/10/19/iwI2Bd.png)

3.i>mx，与方法2相同，看有没有在id范围内。

![iwIfAI.png](https://s1.ax1x.com/2018/10/19/iwIfAI.png)

```c
void manacher(){
    int mx=0,id;
    for(int i=1;i<=m;i++){
        if(mx>=i) pal[i]=min(mx-i+1,pal[2*id-i]);
        else pal[i]=1;
        while(s[i-pal[i]]==s[i+pal[i]]) pal[i]++;
        if(i+pal[[i]-1>mx)
            mx=i+pal[i]-1,id=i;
        }
    }
}

```

