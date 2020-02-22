---
layout: post
title:  "Leetcode双周赛"
date:   2020-02-22 14:48:05 -0500
categories: 题解
---
### 题意

[双周赛20](https://leetcode-cn.com/contest/biweekly-contest-20)

#### Problem1
只要修改一下sort的comparator就好，c++用lambda函数是一种比较简便的写法，看讨论区发现其实库函数里有求1的个数的。（这不重要了）
```cpp
int count_one(int t) {
    int res = 0;
    while(t > 0) {
        res += t & 1;
        t >>= 1;
    }
    return res;
}

class Solution {
public:
    vector<int> sortByBits(vector<int>& arr) {
        sort(arr.begin(), arr.end(), [](int a, int b) {
            if(count_one(a) == count_one(b)) return a < b;
            return count_one(a) < count_one(b);
        });
        return arr;
    }
};
```

```cpp
//使用库函数
class Solution {
public:
    vector<int> sortByBits(vector<int>& arr) {
        sort(arr.begin(), arr.end(), [](int a, int b) {
            if (__builtin_popcount(a) == __builtin_popcount(b))
                return a < b;
            else 
                return __builtin_popcount(a) < __builtin_popcount(b);
        });
        return arr;
    }
};
```

#### Problem2
快乐模拟题，用一个hash表保存product和price的映射关系，idx维护用户的次序即可。
```cpp
class Cashier {
public:
    unordered_map<int, int> hash;
    int freq;
    int idx;
    int dis;
    Cashier(int n, int discount, vector<int>& products, vector<int>& prices) {
        hash.clear();
        int m = products.size();
        for(int i = 0; i < m; i++) {
            hash[products[i]] = prices[i];
        }
        freq = n;
        idx = 0;
        dis = discount;
    }
    
    double getBill(vector<int> product, vector<int> amount) {
        int sum = 0;
        double disco = 1;
        idx++;
        if(idx % freq == 0) {
            disco = (100 - (double)dis) / 100;
        }
        for(int i = 0; i < product.size(); i++) {
            sum += amount[i] * hash[product[i]];
        }
        return sum * disco;
    }
};
```

#### Problem3
数据范围是`5*1e4`，`O(n2)`可能会炸，考虑更好的做法，这个题暴力做法是直接两重循环，枚举左端点和右端点。我们可以枚举右端点，可以发现左端点其实是单调的（用`i`表示右端点，`j`表示左端点，如果`[j,i]`不包含`abc`，那么`[j-1, i]`一定不包含，我们可以得到每个右端点至少左端点为什么时候才是合法的，对于每个右端点，计算`s.size() - i`可以得到相对应左端点的合法字符串数目，加起来就行了)
```cpp
class Solution {
public:
    int numberOfSubstrings(string s) {
        int count[3] = {0};
        int count_char = 0;
        int num = 0;
        int j = 0;
        for(int i = 0; i < s.size(); i++) {
            //枚举右端点
            if(count[s[i] - 'a'] == 0) {
                count_char++;
            }
            count[s[i] - 'a']++;
            while(j < i && count_char == 3) {
                //得到在右端点为i情况下，相对应合法的最近左端点。
                //计算合法数目
                num += s.size() - i;
                count[s[j] - 'a']--;
                if(count[s[j] - 'a'] == 0) {
                    count_char--;
                }
                j++;
            }
        }
        return num;
    }
};
```


#### Problem 4
不会做的一道题，其实很简单，考虑递推得到结果。`.`表示放置位置，`o`表示已经放置好的。

当n=1的情况下，显然只有一种情况 
```
P1, D1
```
当n=2的情况下，可以先考虑P2放在哪里
```
.o.o.
```
有三个可以放的地方，在考虑每一种放置情况下D2的
```
1 P.o.o. 2 oP.o.  3 ooP.
```
为 3 + 2 + 1共6种（每个D的位置被P限制），可以再往下推一层。（不想推了）


得到递推公式下层的贡献为 `从1到2n - 1`的等差数列，和为
```(2n-1) * n```，
所以递推公式为
```S[n] = S[n-1] * (2n-1) * n``` 

```
class Solution {
public:
    const int mod = 1e9 + 7;
    typedef long long i64;
    int countOrders(int n) {
        i64 s = 1;
        for(int i = 2; i <= n; i++) {
            s = s * (2 * i - 1) * i;
            s = s % mod;
        }
        return s % mod;
    }
};
```

