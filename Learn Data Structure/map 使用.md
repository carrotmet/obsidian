有效的字母异位词
map：不可重复、底层红黑树  |  unorderd_map  (hash)
set：  unordered_set （hash）

### 三种hash
- 数组模拟hash---27英文字母，做一个0-26 的数组，就是一个简单hash
- set模拟hash --- 一元，一般不重复  只增删，不修改 set map有序不重 mutimap/mutiset有序 可重unorderedset/unorderedmap 无排序 不重

mp.insert({a,b})
判空 if(map.find(i) == mp.end())  则空

如果 value 为 int  可直接 map[ i ]++


``` C++

class Solution {

public:

    bool isAnagram(string s, string t) {

        map<char,int> mp;

        if(s.length() != t.length()) return false;

        for(auto i : s){

           mp[i]++;

        }

        for(auto j : t){

            if(mp.find(j) == mp.end())

            return false;

          mp[j]--;

        }

    for(const auto& pair : mp){  //mp中本质存储pair类型,

        if(pair.second != 0) return false;

    }

        return true;

    }

};
```

``` C++
计算快乐数（按位相加 + 通过重复判断快乐数）
#include <bits/stdc++.h>
#include <stdio.h>
#include <stdlib.h> 
 int getSum(int n){   //这是一个计算各位元素平方和的函数，主要巧用了 n%10 n/=10
 int sum = 0;
 while(n){
sum += (n%10) * (n%10);
n/=10;
 }
 return sum;
 }

bool iaHappy(int n){   //第一个数平方和的结果是第二个数，所以考虑
unordered_set<int> a;
while(1){   //这是一个无限循环，只要没遇到return，就会一直执行，所以使用要保证能退出
int sum=getSum(n);
if(sum == 1) return true;
if(a.find(sum) != set.end()){
return false;
}else{set.insert(sum);}
}
n=sum;
}
```

``` C++
两数之和，返回下标
无重复 unordered_set/map  快
```


```C++
四数相加：两数存一个map，剩下两数find map

 int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {

        unordered_map<int,int> A;

        unordered_map<int,int> C;

        for(int a: nums1){

            for(int b : nums2){

                A[a+b]++;

            }

        }

        int count = 0;

        for(int c: nums3){

            for(int d: nums4){

                if(A.find(0-c-d) !=A.end())  count+=A[0-c-d];

            }

        }

  

return count;

  

    }

```