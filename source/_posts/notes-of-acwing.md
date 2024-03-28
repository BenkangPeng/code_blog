---
title: notes_of_acwing
tags: Algorithm
abbrlink: 830bf4be
date: 2022-03-06 08:56:02
---

......

<!-- more -->

# 一、基础算法

## 快排

```cpp
void quick_sort(std::vector<int> &q , int l , int r ){
    if(l >= r)   return;
    int i = l - 1 , j = r + 1 , x = q[(l+r)>>1] ;
    while(i < j){
        do i++ ; while(q[i] < x) ;
        do j-- ; while(q[j] > x) ;
        if(i < j)   std::swap(q[i] , q[j]) ;
    }
    quick_sort(q , l , j) ;
    quick_sort(q , j+1 , r) ;

}
```

## 快速选择(第k个数)

```cpp
int quick_sort(int *q , int l , int r , int k){
    if(l >= r)  return q[l];
    int i = l - 1 , j = r + 1 , x = q[(l+r)>>1] ;
    while(i < j){
        do i++ ; while(q[i] < x) ;
        do j-- ; while(q[j] > x) ;
        if(i < j) std::swap(q[i] , q[j]) ;
    }
    if(k <= (j - l + 1))    return quick_sort(q , l , j , k) ;
    else return quick_sort(q , j+1 , r , k - (j - l + 1)) ;
}
```

[215. 数组中的第K个最大元素 - 力扣（LeetCode）](https://leetcode.cn/problems/kth-largest-element-in-an-array/description/)

## 归并排序

**分治思想**

<img src = "https://pic.leetcode-cn.com/1614274007-nBQbZZ-Picture1.png"></img>

```cpp
#include<iostream>
#include<algorithm>
const int N = 1e5 + 10 ;
int q[N] ;
void merge_sort(int *q , int l , int r){
    if(l >= r)  return;
    int mid = l + r >> 1 , i = l , j = mid + 1 ;
    merge_sort(q , l , mid) ;
    merge_sort(q , mid+1 , r) ;
    int temp[r-l+1] , k = 0 ;
    while(i <= mid && j <= r){
        if(q[i] < q[j]) temp[k++] = q[i++] ;
        else temp[k++] = q[j++] ;
    }
    while(i <= mid) temp[k++] = q[i++] ;
    while(j <= r)   temp[k++] = q[j++] ;
    for(int i = l , j = 0 ; i <= r ; i++ , j++) 
        q[i] = temp[j] ;
}
int main()
{
    int n ;
    std::cin >> n ;
    for(int i = 0 ; i < n ; i++){
        std::scanf("%d" , &q[i]) ;
    }
    // std::sort(q , q+n) ;
    merge_sort(q , 0 , n-1);
    for(int i ; i < n ; i++)  std::printf("%d ", q[i]) ;
}
```



## 逆序对数量

```cpp
#include<iostream>
typedef long long LL ;
const int N = 1e5 + 10 ;
int q[N] ;
LL merge_sort(int *q , int l , int r){
    if(l >= r)  return 0 ;
    int mid = (l+r) >> 1 ;
    LL res = merge_sort(q , l , mid) + merge_sort(q , mid+1 , r) ;
    int i = l , j = mid+1 , k = 0 ;
    int temp[r-l+1] ;
    while(i <= mid && j <= r){
        if(q[i] <= q[j])    temp[k++] = q[i++] ;
        else{
            res += mid - i + 1 ;
            temp[k++] = q[j++] ;
        }

    }
    while(i <= mid) temp[k++] = q[i++] ;
    while(j <= r)   temp[k++] = q[j++] ;
    for(int i = l , k = 0 ; i <= r ; i++ , k++)
        q[i] = temp[k] ;
    return res ;
}

int main()
{
    int n ;
    std::cin >> n ;
    for(int i = 0 ; i < n ; i++)
        scanf("%d" , &q[i]) ;
    std::cout << merge_sort(q , 0 , n-1) ;
    return 0 ;
}
```

[LCR 170. 交易逆序对的总数 - 力扣（LeetCode）](https://leetcode.cn/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)



## 二分查找

```cpp
#include<iostream>
const int N = 1e5 + 10 ;
int q[N] ;
int SL(int x , int l , int r){
    //查找第一个≥x的数
    int mid ;
    while(l < r){
        mid = (l+r) >> 1 ;
        if(q[mid] >= x) r = mid ;
        else l = mid + 1 ;
    }
    if(q[l] == x)   return l ;
    else return -1 ;
}
int SR(int x , int l , int r){
    //查找最后一个≤x的数
    int mid ;
    while(l < r){
            mid = (l+r+1) >> 1 ;
            if(q[mid] <= x) l = mid ;
            else r = mid - 1 ;
    }
    if(q[l] == x) return l ;
    else return -1 ;
}
int main()
{
    int n , m ; 
    std::cin >> n >> m ;
    for(int i = 0 ; i < n ; i++)
        std::scanf("%d" , &q[i]) ;
    while(m--){
        int x ;
        std::cin >> x ;
        std::cout << SL(x , 0 , n-1) << " " << SR(x , 0 , n-1 ) << std::endl ;
        
    }

    return 0 ;
}
```

[69. x 的平方根 - 力扣（LeetCode）](https://leetcode.cn/problems/sqrtx/)

## 高精度计算

```cpp
#include<iostream>
#include<string>
#include<vector>
#include<algorithm>
using namespace std ;
string add(const string &a , const string &b){
    int t = 0 , i = a.size() - 1 , j = b.size() - 1 ;
    string c ;
    while(i >= 0 || j >= 0){
        if(i >= 0)  t += a[i--] - '0' ;
        if(j >= 0)  t += b[j--] - '0' ;
        c.push_back(t % 10 + '0') ;
        t /= 10 ;
    }
    if(t)   c.push_back(t + '0') ;
    reverse(c.begin() , c.end()) ;
    return c ;
}
bool cmp(const string& a , const string& b){
    if(a.size() != b.size())    return a.size() > b.size() ;
    else {
        int i ;
        for(i = 0 ; a[i] == b[i] ; i++) ;
        return a[i] > b[i] ;
    }
    return 0 ;
}
string sub(const string& a , const string& b){
    if(a == b)  return "0" ;
    string c ;
    if(cmp(a , b)){
           int i = a.size() - 1 , j = b.size() - 1 , t = 0 , k = 0 ;
           while(i >= 0 || j >= 0){
            t = k ;
            if(i >= 0)  t += a[i] - '0' ;
            if(j >= 0)  t = t - (b[j] - '0') ;
            c.push_back((t+10)%10 + '0') ;
            k = (t >= 0) ? 0 : -1 ;
            i-- , j-- ;
           }
    }
    else {
        c.push_back('-') ;
        c += sub(b,a) ; 
    }
    int i = c.size() - 1 ;
    while(c.size() > 1 && c[i--] == '0')  c.pop_back() ;
    if(c[0] != '-') reverse(c.begin() , c.end()) ;
    return c ;
}
string mul(const string &a , const string &b){
    if(a == "0" || b == "0")    return "0" ;
    int len_a = a.size() , len_b = b.size()  , t = 0 ;
    string c = string(len_a + len_b , '0') ;
    for(int i = len_a - 1 ; i >= 0 ; i--){
        for(int j = len_b - 1 ; j >= 0 ; j--){
            t = (a[i] - '0') * (b[j] - '0') + (c[i+j+1] - '0');
            c[i+j+1] = t % 10 + '0' ;
            c[i+j] += t / 10 ;
        }
    }
    int i ;
    for(i = 0 ; c[i] == '0' && i < len_a + len_b - 1 ; i++) ;
    c = c.substr(i) ;
    return c ;
}
string div(const string &a , int b , int& r){
    int r0 = 0 ;//ro is reminder .
    string c ;
    for(auto it = a.begin() ; it != a.end() ; it++){
        r0 = r0 * 10 + (*it - '0') ;
        c.push_back(r0 / b + '0') ;
        r0 %= b ;
    }
    int idx = 0 ;
    while(c[idx] == '0' && idx < c.size() - 1)    idx++ ;
    c = c.substr(idx) ;
    r = r0 ;
    return c ;
}

int main()
{
    
}
```

## 前缀和

```cpp
#include<iostream>
using namespace std ;

int main()
{
    int n , m , l , r ;
    const int N = 1e5 + 10 ;
    cin >> n >> m ;
    int q[N] , s[N] ;
    s[0] = 0 ;
    for(int i = 1 ; i <= n ; i++){
        scanf("%d" , &q[i]) ;
        s[i] = s[i-1] + q[i] ;
    }
    
    while(m--){
        cin >> l >> r ;
        cout << s[r] - s[l-1] << endl ;
    }
}
```

## 子矩阵的和

```cpp
#include<iostream>
using namespace std ;
const int N = 1010 ;
int main()
{
    int a[N][N] = {0} , s[N][N] = {0} ;
    int n , m , q , x1 , y1 , x2 , y2 ;
    cin >> n >> m >> q ;
    for(int i = 1 ; i <= n ; i++)
        for(int j = 1 ; j <= m ; j++){
            scanf("%d" , &a[i][j]) ;
            s[i][j] = s[i-1][j] + s[i][j-1] + a[i][j] - s[i-1][j-1] ;
        }
    while(q--){
        // cin >> x1 >> y1 >> x2 >> y2 ;
        scanf("%d %d %d %d" , &x1 , &y1 , &x2 , &y2) ;
        printf("%d\n" , s[x2][y2] - s[x2][y1-1] - s[x1-1][y2] + s[x1-1][y1-1]) ; 
    } 

}
```

## 差分

```cpp
#include<iostream>
using namespace std;
const int N = 1e5 + 10;
int a[N] , b[N];


int main(){
    int n , m;
    cin >> n >> m;
    for(int i = 1 ; i <= n ; i++){
        scanf("%d" , &a[i]);
        b[i] = a[i] - a[i-1];
    }
    while(m--){
        int l , r  , c ;
        scanf("%d %d %d" , &l , &r , &c);
        b[l] += c , b[r+1] -= c;
    }
    
    for(int i = 1 ; i <= n ; i++){
        a[i] = a[i-1] + b[i];
        printf("%d " , a[i]);
    }
}
```



## 双指针

**最长连续不重复子序列**

```cpp
#include<iostream>
using namespace std ;
int main()
{
    const int N = 1e5 + 10 ;
    int a[N] , s[N] = {0} , n , res = 0 ;
    cin >> n ;
    for(int i = 0 ; i < n ; i++)
        scanf("%d" , &a[i]) ;
    for(int i = 0 , j = 0 ; i < n ; ++i){
        s[a[i]]++ ;
        while(j < i && s[a[i]] > 1) s[a[j++]] -- ;
        res = res > (i-j+1) ? res : (i-j+1) ;
    }
    cout << res ;
    return 0 ;
}
```

[3. 无重复字符的最长子串 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-substring-without-repeating-characters/submissions/492530011/)

[88. 合并两个有序数组 - 力扣（LeetCode）](https://leetcode.cn/problems/merge-sorted-array/)

**数组元素的目标和**

```cpp
#include<iostream>
using namespace std ;
int main()
{
    int n , m , x ;
    const int N = 1e5 + 10 ;
    int a[N] , b[N] ;
    cin >> n >> m >> x ;
    for(int i = 0 ; i < n ; i++){
        scanf("%d" , &a[i]) ;
    }
    for(int i = 0 ; i < m ; i++)
        scanf("%d" , &b[i]) ;
    for(int i = 0 , j = m-1 ; i < n ; i++){
        while(j>=0 && a[i]+b[j] > x)    j-- ;
        if(j >= 0 && a[i]+b[j] == x){
            cout << i << ' ' << j ;
            break ;
        }
    }

}
```

**位运算**

```cpp
#include<iostream>
using namespace std ;
int main()
{
    int x ;
    cin >> x ;
    for(int k = 7 ; k >= 0 ; --k)//8位二进制表示输出
        cout << (x>>k & 1) ;
}
```

**lowbit**

求出最后一个二进制1所表示的数的大小(描述得不清楚)，看示例：

`24`的8位二进制表示为`0001_1000` , 则`lowbit(24) = 0000_1000` ;

`lowbit(26) = 0000_0010` ……

原理是`x&(-x)` , 本质是`x & (~x + 1)` , `x`与其补码相与。

`lowbit`常用于统计二进制数中`1`的个数

```cpp
#include<iostream>
using namespace std ;
int lowbit(int x){
    return x&(-x) ;
}
int main()
{
    int n , x , res = 0 ;
    cin >> n ;
    while(n--){
        cin >> x ;
        res =  0 ;
        while(x)    x -= lowbit(x) , res++ ;
        printf("%d " , res) ;
    }
}
```

[LCR 133. 位 1 的个数 - 力扣（LeetCode）](https://leetcode.cn/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/submissions/492746547/)

[461. 汉明距离 - 力扣（LeetCode）](https://leetcode.cn/problems/hamming-distance/)

利用`x&(x-1)`也可求二进制表示中1的个数

例如汉明距离：

```cpp
class Solution {
public:
    int hammingDistance(int x, int y) {
        int t = x ^ y , res = 0 ;
        while(t)    t = t & (t-1) , res++ ;
        return res ;
    }
};
```

## 离散化

使用场景：**值域大，但稀疏**

