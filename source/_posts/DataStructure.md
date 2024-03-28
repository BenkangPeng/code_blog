---
title: DataStructure
abbrlink: ba907200
date: 2021-10-21 00:09:08
tags:
---

<p style = "color : #f03752 ; font-size : 24px">It's the note I made when learned data structure . </p>

Ref : [数据结构与算法基础（青岛大学-王卓）_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1nJ411V7bd/?spm_id_from=333.337.search-card.all.click&vd_source=da5120fea3f8bb8d2fe1984a02a9a745)

<!-- more -->

# Representation and Implementation of ADT

**EX :  Arithmetic Operations on Complex Numbers**

```cpp
#include <iostream>
using namespace std ;
typedef struct 
{
    float real;
    float imag;
} Complex;
void assign(Complex &A, float real, float imag) {
    A.real = real;
    A.imag = imag;
}
void print(Complex A){
    cout << A.real << '+' << A.imag << 'i' << endl ;
}
void add(Complex &A , Complex B , Complex C){
    A.real = B.real + C.real ;
    A.imag = B.imag + C.imag ;
}
void minus(Complex &A , Complex B , Complex C){
    A.real = B.real - C.real ;
    A.imag = B.imag - C.imag ;
}
void multiply(Complex &A , Complex B , Complex C){
    A.real = B.real * C.real - B.imag * C.imag ;
    A.imag = B.real * C.imag + B.imag * C.real ;
}
void divide(Complex &A , Complex B , Complex C){
    A.real = (B.real * C.real + B.imag * C.imag) / (C.real * C.real + C.imag * C.imag) ;
    A.imag = ( - B.real * C.imag + B.imag * C.real ) / (C.real * C.real + C.imag * C.imag) ;
}
int main() {
    Complex A , B , C ;
    assign(A , 1 , 2);
    assign(B , 3 , 4) ;
    assign(C , 5 , 6) ;
    divide(A , B , C) ;
    print(A) ;
    return 0;
}

```

# Linear List

## Squence Linear List

### **EX1. Horner's rules or Qin Jiushao(秦九韶)'s algorithm**

```cpp
#include<iostream>
#include<cmath>
#include<ctime>
#define N 9 
#define K 1e7 // numbers of loop
double a[10] = { 2.1 , 3.4 , 2 , 1.3 , 3 , 8.8 , 4.3 , 5.3 , 5.3 , 4.3} ;
double func1(double x , double *a ){
    double res = a[0] ;
    for(int i = 1 ; i <= N ; i++){
        res += a[i] * pow(x , i) ;
    }
    return res ;
    //std:: cout << "the result of polynominal by func1 is " << res << std::endl ; 
}
double func2(double x , double *a){
    // Qin Jiushao's algorithm
    double res = a[9] ;
    for(int i = N ; i > 0 ; i--){
        res = a[i - 1] + res * x ;
    }
    return res ;
    //std::cout << "the resulr of polynominal by func2 is " << res << std::endl ;
}
void time_func1(){
    clock_t start , stop ;
    start = clock() ;
    for(int i = 0 ; i < K ; i++){
        func1( 2.4 , a ) ;   
    }
    stop = clock() ;
    std::cout << "func1 cost " << (double(stop - start)) / CLK_TCK / K << " s per time" << std::endl ;
}
void time_func2(){
    clock_t start , stop ;
    start = clock() ;
    for(int i = 0 ; i < K ; i++){
        func2( 2.4 , a ) ;   
    }
    stop = clock() ;
    std::cout << "func2 cost " << (double(stop - start)) / CLK_TCK / K  << " s per time" << std::endl ;
}
int main(){
    double x ; 
    std::cin >> x ; 
    std::cout << "the result of func1 is " << func1(x , a) << std::endl ;    
    std::cout << "the result of func2 is " << func2(x , a) << std::endl ;    
    func2(x , a) ;
    time_func1() ;
    time_func2() ;
}
```



### C++ dynamic memory allocation

```cpp
int* p = new int ;
int* p2 = new int[10] ; //allocate 10 int memory to p2
delete p , p2;
```

### Convenient way of passing values in C++ 

```cpp
void swap_1(int& a , int& b){
    int temp = a ;
    a = b ;
    b = temp ;
}
int main(){
    int a = 2 , b = 9 ;
    swap_1(a , b) ;
    cout << "a = " << a << " b = " << b << endl ;
    return 0 ;
}
```

### Representation of LinearList

```cpp
#include<iostream>
#define ElementType int 
#define MaxSize 20 
using namespace std ;
typedef struct SqList{
    ElementType  *element ;
    int length ;
}SqList ;


void InitList_Sq(SqList &L){
    //Initial the Squence Linear List
    L.element = new ElementType[MaxSize] ;
    if(!L.element)  {
        exit(0);
        L.length = 0 ;
    }
}
void DestroyList(SqList &L){
    if(L.element)   delete L.element ;
}
void ClearList(SqList &L){
    L.length = 0 ;
}
int GetLength(SqList &L){
    return  L.length ;
}
int IsEmpty(SqList &L){
    if(L.length)    return 0 ;
    else    return 1 ;
}
int GetEle(SqList &L , int i , ElementType &e){
    if(i < 1 || i > L.length)   return 0 ;
    else e = L.element[i] ;
    return 1 ;
}
int LocateEle (SqList &L , ElementType e){
    for(int i = 0 ; i < L.length ; i++){
        if(L.element[i] == e)   return i ;
    }
    return 0 ;
}
int ListInsert_Sq(SqList &L , ElementType e , int i ){
    if(i < 0 || i > L.length || (L.length + 1) > MaxSize)   return 0 ;
    for(int j = L.element[L.length - 1] ; j >= i ; j--){
        L.element[j + 1] = L.element[j] ;
    }
    L.element[i] = e ;
    L.length++ ;
    return 1 ;
}
void PrintList_Sq(SqList &L){
    for(int i = 0 ; i < L.length ; i++){
        cout << L.element[i] << " " ;
    }
    cout << endl ;
}
int ListDelete_Sq(SqList &L , int i ){
    if(i < 0 || i > L.length - 1) return 0 ;
    for(int j = i + 1 ; j < L.length ; j++){
        L.element[j - 1] = L.element[j] ; 
    }
    L.length-- ;
    return 1 ;
}
void union_list(SqList &La , SqList Lb){
    int length = GetLength(Lb) ;
    for(int i = 0 ; i < GetLength(Lb) ; i++){
        if(LocateEle(La , Lb.element[i]) == -1) {
            ListInsert_Sq(La , Lb.element[i] , GetLength(La)) ;
        }
    }
}
void merge_list_Sq(SqList &Lc , SqList La , SqList Lb ){
    int length_a = GetLength(La) , length_b = GetLength(Lb) ;
    Lc.length = length_a + length_b ;
    Lc.element = new ElementType[Lc.length] ;
    int i = 0 , j = 0 , k = 0 ;
    while(i < length_a && j < length_b){
        if(La.element[i] < Lb.element[j]){
            Lc.element[k++] = La.element[i++] ;
        }
        else Lc.element[k++] = Lb.element[j++] ;
    }
    pc->next = pa ? pa : pb ;
}
int main(){
    SqList L ;
    InitList_Sq(L) ; 
    int a[5] = {1 , 3 , 5 , 7 , 9} ;
    for(int i = 0 ; i < 5 ; i++){
        L.element[i] = a[i] ;
    }
    L.length = 5 ;
    ListDelete_Sq(L , 4) ;
    PrintList_Sq(L) ;
}
```

**Advantages and Disadvantages of Squence Linear List :**

**Advantages :**

- **Storing physical location relationships represents logical relationships .**
- **Can get any elements quickly .**

**Disadvantage:**

- **Too many steps to move elements  in the option of Insert and Delete elements .**

## Linked Linear List

I write a new article about linked linear list [HERE]()

