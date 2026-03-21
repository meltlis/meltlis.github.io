---
title: '每日一题2025-1-1'
date: '2025-01-01 12:46:16'
authors:
  - me
categories:
  - '算法题'
  - 'leetcode'
  - '简单'
toc: true
---

新年第一题，难度简单，虽然不知道二进制的库函数但手搓秒了。  
<!--more-->
[3280.将日期转换为二进制表示](https://leetcode.cn/problems/convert-date-to-binary/description/ "跳转至题目")
```cpp
class Solution {
public:
    string convertDateToBinary(string date) {
        string return_date, temp;
        int i = 0, num, len = date.size();
        for(int j = 0; j < 3; j++){
            num = 0;
            while (i < len && date[i] != '-'){
                num = num * 10 + (date[i]-'0');
                ++i;
            }
            while(num > 0){
                temp += (num % 2 + '0');
                num /= 2;
            }   
            for(int k = temp.size()-1; k >=0; k--){
                return_date += temp[k];
            }
            temp = "";
            if (j != 2){
                return_date += '-';
            }
            i++;
        }
        return return_date;
    }
};
```
![图片](/image/lc20250101.png "结果")