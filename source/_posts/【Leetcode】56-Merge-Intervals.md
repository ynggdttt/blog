---
title: 【Leetcode】56. Merge Intervals
date: 2019-02-24 23:07:06
tags:
  - 数组
  - 中等
categories: leetcode
copyright: false
---
# 题目 合并区间
给出一个区间的集合，请合并所有重叠的区间。

## 案例
示例 1:
```
输入: [[1,3],[2,6],[8,10],[15,18]]
输出: [[1,6],[8,10],[15,18]]
解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
```
示例 2:
```
输入: [[1,4],[4,5]]
输出: [[1,5]]
解释: 区间 [1,4] 和 [4,5] 可被视为重叠区间。
```

# 思路

首先将当前的区间按照start从低到高排序，如果后面一个区间的start比前一个的end要大，那么两者是没有重复的，直接将第二个放到当前的结果中；否则就是存在重复的区间，那就将当前res中最后一个区间的end，换成当前的end和原本的end的最大值；

# 代码
``` golang
/**
 * Definition for an interval.
 * struct Interval {
 *     int start;
 *     int end;
 *     Interval() : start(0), end(0) {}
 *     Interval(int s, int e) : start(s), end(e) {}
 * };
 */
class Solution {
public:
    vector<Interval> merge(vector<Interval>& intervals) {
        int n = intervals.size();
        vector<Interval> res;
        if(n == 0) return res;
        sort(intervals.begin(), intervals.end(), cmp);
        res.push_back(intervals[0]);
        for(int i = 1; i < n; i++){
            if(intervals[i].start > res.back().end)
                res.push_back(intervals[i]);
            else{
                res.back().end = max(res.back().end, intervals[i].end);
            }
        }
        return res;
    }
    static bool cmp(Interval a, Interval b){
        return a.start < b.start;
    }
};
```


