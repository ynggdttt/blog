---
title: 【Leetcode】57.Insert Interval
date: 2019-02-25 15:45:10
tags:
  - 数组
  - 难
categories: leetcode
copyright: false
---
# 题目 合并区间
给出一个无重叠的 ，按照区间起始端点排序的区间列表。
在列表中插入一个新的区间，你需要确保列表中的区间仍然有序且不重叠（如果有必要的话，可以合并区间）。

## 案例

示例 1:
```
输入: intervals = [[1,3],[6,9]], newInterval = [2,5]
输出: [[1,5],[6,9]]
```
示例 2:
```
输入: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
输出: [[1,2],[3,10],[12,16]]
解释: 这是因为新的区间 [4,8] 与 [3,5],[6,7],[8,10] 重叠。
```

# 思路
这个题和56题很像，唯一不同的就是是需要插入一个区间；
思考这样的一个问题，这个区间应该插在哪里，实际上应该插在，该区间的start比原区间数组中的某一个元素的start刚好大于或者等于的情况；
找到这个位置之后，就要考虑当前的插入区间是不是能直接插入还是需要和上一个区间合并，如果该区间的start比上一个区间的end要大的话，那么可以直接插入，或者还有一种特殊情况就是res中还没有区间，那么这两种情况都直接插入，否则的话就需要进行一次合并，合并的原则和56题一样，就是将当前res尾部的区间的end修改成插入区间的end和res尾部的end中的较大的值；然后再对剩余的区间依次做合并操作，最终得到结果；

# 代码
```c++
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
    vector<Interval> insert(vector<Interval>& intervals, Interval newInterval) {
        vector<Interval> res;
        int n = intervals.size();
        if(n == 0) 
        {
            res.push_back(newInterval);
            return res;
        }
        int i = 0;
        for( ; i < n; i++){
            if(newInterval.start <= intervals[i].start)
                break;   
            res.push_back(intervals[i]);
        }
        if(i == 0 || newInterval.start > intervals[i - 1].end)
            res.push_back(newInterval);
        else{
            res.back().end = max(newInterval.end, res.back().end);
        } 
        for(; i < n; i++){
             if(res.back().end < intervals[i].start)
                 res.push_back(intervals[i]);
            else
               res.back().end = max(res.back().end, intervals[i].end);
        }
        return res;
    }
};
```
