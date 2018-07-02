---
layout: post
title: "快排里的Partition应用-找第K大的数字"
date: 2018-01-06 15:21
comments: true
reward: true
tags: 
	- leetcode
---
### Partition 算法

参考自：

[白话经典算法系列之六 快速排序 快速搞定](http://blog.csdn.net/morewindows/article/details/6684558)

### 挖数填坑：

![](http://ovuyz1070.bkt.clouddn.com/18-1-6/44870068.jpg)

```c++
int partition(vector<int>& nums, int low, int high)
{
    int x = nums[low];
    int i = low;
    int j = high-1;
    while(i<j)
    {
      while(i<j&&nums[j]<=x)
        j--;
      if(i<j)
        nums[i]=nums[j];
      while(i<j&&nums[i]>=x)
        i++;
      if(i<j)
        nums[j]=nums[i];
    }
    nums[i]=x;
    return i;
}
```

### FindKthNumber

```c
int findKthLargest(vector<int>& nums, int k) {
    int begin = 0, end = nums.size();
    int target_num = 0;
    while (begin <= end){
        int pos = partition(nums, begin, end);
        if(pos == k-1){
            target_num = nums[pos];
            break;
        }
        else if(pos > k-1){
            end = pos;
        }
        else{
            begin = pos + 1;
        }
    }
    return target_num;
}
```

### 全部代码

```c++
#include<cstdio>
#include<vector>
using namespace std;

int partition(vector<int>& nums, int low, int high)
{
    int x = nums[low];
    int i = low;
    int j = high-1;
    while(i<j)
    {
      while(i<j&&nums[j]<=x)
        j--;
      if(i<j)
        nums[i]=nums[j];
      while(i<j&&nums[i]>=x)
        i++;
      if(i<j)
        nums[j]=nums[i];
    }
    nums[i]=x;
    return i;
}
    
int findKthLargest(vector<int>& nums, int k) {
    int begin = 0, end = nums.size();
    int target_num = 0;
    while (begin <= end){
        int pos = partition(nums, begin, end);
        if(pos == k-1){
            target_num = nums[pos];
            break;
        }
        else if(pos > k-1){
            end = pos;
        }
        else{
            begin = pos + 1;
        }
    }
    return target_num;
}


int main()
{
  vector<int> nums={2, 1};
  printf("%d", findKthLargest(nums, 2));
  getchar();
}
```

[例题：Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/description/)