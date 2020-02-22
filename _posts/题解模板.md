---
layout: post
title:  "题解模板"
date:   2020-02-20 14:07:05 -0500
categories: 题解
---

## 题意

见[USACO Greedy Algorithm](https://train.usaco.org/usacotext2?a=16FqG4xVdTC&S=greedy)

### 1.复述一遍题意
blabla 
### 2.分析 + 想法 + 原理 
blabla
### 3.代码

{% highlight cpp %}
/*
ID: zt2219
TASK: dualpal
LANG: C++
*/
/* LANG can be C++11 or C++14 for those more recent releases */
#include <iostream>
#include <fstream>
#include <string>
#include <unordered_map>
#include <vector>
#include <algorithm>
#include <cstring>

using namespace std;
//#define cin fin
//#define cout fout

using namespace std;

ofstream fout("palsquare.out");
ifstream fin("palsquare.in");

string toB(int x, int B) {
    string res = "";
    while(x > 0) {
        int d = x % B;
        if(d < 10) {
            res += '0' + d;
        } else {
            res += 'A' + d - 10;
        }
        x /= B;
    }
    reverse(res.begin(), res.end());
    return res;
}

bool isP(string s) {
    int l = 0; int r = s.size() - 1;
    while(l < r) {
        if(s[l] != s[r]) {
            return false;
        }
        l++, r--;
    }
    return true;
}

int main() {
    int n, s;
    cin >> n >> s;
    while(n > 0) {
        int j = s + 1;
        while(true) {
            int cnt = 0;
            for(int i = 2; i <= 10; i++) {
                if(isP(toB(j, i))) cnt++;
            }
            if(cnt >= 2) {
                cout << j << endl;
                n--; break;
            }
            j++;
        }
        s = j;
    }
    return 0;
}
{% endhighlight %}

###不同题型对应
1. 模板题 总结，写注释
2. dp题 状态定义 转移 优化
3. 贪心 如何想到贪心，简单证明
4. 图论 建模，建模理由