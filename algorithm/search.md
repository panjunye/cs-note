# 搜索定义
```C++
#include <vector>
#include <iostream>

using namespace std;

typedef int Element;

void print(vector<Element> &A) {
    for (Element &it : A) {
        cout << it << ' ';
    }
    cout << endl;
}
```

## 顺序查找
```C++
int sequentialSearch(vector<Element> &A, Element &x) {
    for (int i = 0; i < A.size(); ++i) {
        if (A[i] == x)
            return i;
    }
    return -1;
}
```

## 二分查找
```C++
int binarySearch(vector<Element> &A, Element &x) {
    int start = 0;
    int end = A.size() - 1;

    while (start <= end) {
        int m = (start + end) / 2;
        if (A[m] == x)
            return m;
        else if (A[m] < x)
            start = m + 1;
        else
            end = m - 1;
    }
    return -1;
}
```

## 分块查找
*算法思想:* 将$n$个元素"按块有序"分为$m$块，块$H_k$中的所有元素小于块$H_{k+1}$的任意元素，
*算法流程:* 
1. 先取各块的最大关键字构成一个索引表
2. 查找分为两个过程
    * 先对索引表进行二分查找或顺序查找，确定元素在哪个块中
    * 然后对确定的块进行顺序查找。
```C++
int blockSearch(vector<Element> &A, Element &x){
    // todo
}
```
