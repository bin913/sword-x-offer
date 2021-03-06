# 丑数

把只包含质因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含质因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。

 ## 顺序遍历所有
 
 时间复杂度太大，不通过

```cpp
class Solution {
    bool isUgly(int num) {
        while(num%2==0) num/=2;
        while(num%3==0) num/=3;
        while(num%5==0) num/=5;
        return num==1 ? true : false;
    }
public:
    int GetUglyNumber_Solution(int index) {
        if(index <= 0) return 0;
        int uglyFound = 0, n = 0;
        while(uglyFound < index)
            if(isUgly(++n)) uglyFound++;
        return n;
    }
};
```

## 基于已有丑数计算下一个

### 常规解法

```cpp
class Solution {
public:
    int GetUglyNumber_Solution(int index) {
        if(index<7) return index;
        vector<int> res(index);
        res[0] = 1;
        int t2 = 0, t3 = 0, t5 = 0;
        for(int i = 1; i < index; i++) {
            res[i] = min(res[t2]*2, min(res[t3]*3, res[t5]*5));
            if(res[i]==res[t2]*2) t2++;
            if(res[i]==res[t3]*3) t3++;
            if(res[i]==res[t5]*5) t5++;
        }
        return res[index-1];
    }
};
```

### 基于队列

```cpp
class Solution {
public:
    int GetUglyNumber_Solution(int index) {
        if(index < 1) return 0;
        int ugly = 0;
        queue<int> q2, q3, q5;
        q2.push(1);
        for(int i = 0; i < index; i++) {
            int v2 = q2.empty() ? INT_MAX : q2.front();
            int v3 = q3.empty() ? INT_MAX : q3.front();
            int v5 = q5.empty() ? INT_MAX : q5.front();
            ugly = min(v2, min(v3, v5));
            if(ugly == v2) {
                q2.pop();
                q2.push(2 * ugly);
                q3.push(3 * ugly);
            }
            else if(ugly == v3) {
                q3.pop();
                q3.push(3 * ugly);
            }
            else
                q5.pop();
            q5.push(5 * ugly);
        }
        return ugly;
    }
};
```
