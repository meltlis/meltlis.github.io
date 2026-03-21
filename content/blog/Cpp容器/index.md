---
title: 'C++ STL unordered,个人理解总结'
date: '2024-01-30 12:16:57'
authors:
  - me
categories:
  - '学习'
  - 'C++'
toc: true
---

很早以前写的。  
<!--more-->
## 1.unordered_set & unoderded_map
```cpp 
unordered_set(int(hashvalue))hashname
```
仅存储value不存储key(=0or1)。
```cpp
unordered_map(int(hashkey),int(hashvalue))hashname
```
存储value与key

#### 使用例 力扣第2670题
> 给你一个下标从 0 开始的数组 nums ，数组长度为 n 。
> nums 的 不同元素数目差 数组可以用一个长度为 n 的数组 diff 表示，其中 diff[i] 等于前缀 nums[0, ..., i] 中不同元素的数目 减去 后缀 nums[i + 1, ..., n - 1] 中不同元素的数目。
> 返回 nums 的 不同元素数目差 数组。
> 注意 nums[i, ..., j] 表示 nums 的一个从下标 i 开始到下标 j 结束的子数组（包含下标 i 和 j 对应元素）。特别需要说明的是，如果 i > j ，则 nums[i, ..., j] 表示一个空子数组。
```c++
    vector<int> distinctDifferenceArray(vector<int>& nums) {
        int i, j, len = nums.size(), a =0;
        unordered_map<int,int> nums1;
        unordered_map<int,int> nums2;
        vector<int> ans(len);
        for(i = 0; i < len; i++){
            if(!nums2[nums[i]]){
                nums2[nums[i]]++;
                a--;
            }
            else nums2[nums[i]]++;
        }
        for(i = 0; i < len; i++){
            if(nums2[nums[i]]){
                nums2[nums[i]]--;
                if(!nums2[nums[i]])a++;
            }
            if(!nums1[nums[i]]){
                a++;
                nums1[nums[i]]++;
            }
            ans[i] = a;
        }
        return ans;
    }
```
```c++
    vector<int> distinctDifferenceArray(vector<int>& nums) {
        int i, j, len = nums.size(), a = 0;
        unordered_set<int> num;
        vector<int> ans(len,0);
        for(i = len -1; i > 0; --i){
            num.insert(nums[i]);
            ans[i - 1] -= num.size();
        }
        num.clear();
        for(i = 0; i < len; i++){
            num.insert(nums[i]);
            ans[i] += num.size();
        }
        return ans;
    }
```

## 2.operator()与.at()方法的区别
```c++
cout<<hashname[hashkey]<<" "<<hashname.at(hashkey);
```
都用于输出输出hash对应key的值。区别在于当key不存在时，at()会抛出了超出范围的异常，而operator []会用默认值进行初始化，例``hashname.at(noexisthashkey); //error hashname[noexisthashkey];`` 创建一个值为默认值0，键为noexisthashkey的对
## 3.auto&[a, b]：n与auto& n:pair
``for(auto&[a, b]:n)``与``for(auto& n:pair)``均为遍历键对。
前者a为键，b为键值。而后者通过n.first访问键，n.second访问键值

