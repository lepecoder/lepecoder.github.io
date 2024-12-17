---
title: leetCoder-wordBreak判断能否分词
date: 2017-08-01T14:18:00
tags:
categories: 动态规划
---

## 题目

>Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, determine if s can be segmented into a space-separated sequence of one or more dictionary words. You may assume the dictionary does not contain duplicate words.

>For example, given

s = "leetcode",

dict = ["leet", "code"].

Return true because "leetcode" can be segmented as "leet code".

__UPDATE (2017/1/4):__

The wordDict parameter had been changed to a list of strings (instead of a set of strings). Please reload the code definition to get the latest changes.





## 分析

给出一段话,判断能否分词(被dict里的单词分割)



简单动态规划题,使用`dp[i]`表示前`i`个单词能否被分词,则状态转移方程为

```

dp[j] = dp[i] && s[i:j]∈dict

```

如果前`i`个单词可以被分词,且`i-j`在`dict`里,则前`j`个单词可以被分词





## AC代码



```cpp

class Solution {

public:

    bool wordBreak(string s, vector<string>& wordDict) {

        int n = s.length();

        vector<bool> dp(n + 1, false);

        dp[0] = true;

        for(int i=0;i<n;i++){

            for(int j = i; dp[i]&&j<n; j++){

                auto f = find(wordDict.begin(), wordDict.end(), s.substr(i,j-i+1));

                if(f != wordDict.end())//找到

                    dp[j+1] = true;

            }

        }

        return dp[n];

    }

};

```
    