---
title: Cpp新知
tags: C++
abbrlink: 3ecf09f5
date: 2022-03-06 08:35:17
---

......

<!-- more -->

### C++矩阵运算(运算符重载)

[Link](https://mp.weixin.qq.com/s/C1c5XChVyVT4OSeNvpmaqQ)

```cpp
#include<iostream>

#include<vector>

class Matrix{

    private:

        int rows , cols ;

        std::vector<std::vector<int>> data ;

    public:

        Matrix(int rows, int cols , std::vector<std::vector<int>> data): rows(rows), cols(cols) , data(data){}

  

        int get_elem(int row , int col){

            return data[row][col];

        }

        void display(){

            for(int i = 0 ; i < rows ; i++){

                printf("\n");

                for(int j = 0 ; j < cols ; j++){

                    printf("%d  " , data[i][j]);

                }

            }

        }

        Matrix operator+(const Matrix& A) const{

            Matrix result(rows , cols , data);

            for(int i = 0 ; i < rows ; i++)

                for(int j = 0 ; j < cols ; j++){

                    result.data[i][j] += A.data[i][j];

                }

            return result;

        }

  

        Matrix operator-(const Matrix& A) const{

            Matrix result(rows , cols , data);

            for(int i = 0 ; i < rows ; i++)

                for(int j = 0 ; j < cols ; j++){

                    result.data[i][j] -= A.data[i][j];

                }

            return result;

        }

  

        Matrix operator*(const Matrix& A) const{

            if(cols != A.rows){

                std::cout << "Invalid matrix dimensions" << std::endl;

                exit(-1);

            }

            Matrix result(rows , A.cols , std::vector<std::vector<int>> (rows , std::vector<int>(A.cols,0)));

            for(int i = 0 ; i < rows ; i++)

                for(int j = 0 ; j < A.cols ; j++)

                    for(int k = 0 ; k < cols ; k++){

                        result.data[i][j] += data[i][k] * A.data[k][j];

                    }

            return result;

        }

  

        Matrix transpose()const{

            Matrix res(cols , rows , data);

            for(int i = 0 ; i < cols ; i++)

                for(int j = 0 ; j < rows ; j++){

                    res.data[i][j] = data[j][i];

                }

                return res;

        }

};

  

int main()

{

    std::vector<std::vector<int>> data = { {1,2,3} , {4,5,6} , {7,8,9}};

    std::vector<std::vector<int>> data1 = { {1,2,3} , {4,5,6} , {7,8,9} , {6,6,6}};

    std::vector<std::vector<int>> data2 = { {1,2,3,4} , {4,5,6,7} , {7,8,9,10}};

    Matrix A(3,4,data2);

    Matrix B(4,3,data1);

    Matrix C(3,3,data);

    // C = A + B;

    C = A * B;

    A.display();

    B.display();

    C.display();

    C.transpose().display();

}
```

### C++11 tuple

[Link](https://mp.weixin.qq.com/s/oPOqr5J0ES0NGO-L3eKvzA)

```cpp
#include<iostream>
#include<tuple>
#include<string>

int main()
{
    std::string name = "Alice";
    int age = 21;
    double grade = 100.0;
    auto student_tuple = std::make_tuple(name , age , grade);

    std::string name_from_student = std::get<0>(student_tuple);
    int age_of_student = std::get<1>(student_tuple);
    double grade_of_student = std::get<2>(student_tuple);

    std::cout << "name : " << name_from_student << std::endl;
    std::cout << "age  : " << age_of_student    << std::endl;
    std::cout << "grade: " << grade_of_student  << std::endl;
    return 0;
}
```

### 自动补全背后的算法(字典树)

[Link](https://mp.weixin.qq.com/s/L42BCgw-WczbWsiY9C0MeA)

Trie树（又称前缀树或字典树）是一种用于快速检索字符串数据集中的键的树形数据结构。这种结构非常适合处理字符串集合，特别是实现自动补全或拼写检查等功能。下面，我将通过一个具体的例子来解释Trie树的结构和工作原理。

假设我们有一个Trie树，我们要在其中存储单词“CAT”，“CAN”，和“BAT”。Trie树的结构将如下所示：

```
       Root
      /    \
     B      C
     |      |
     A      A
    / \    / \
   T   *  T   N
  /         /
 *         *  
```

在这个图形化表示中：

- 每个节点代表一个字符。
- 根节点（Root）不包含字符，它是所有单词的起始点。
- 每条从根节点到某个标记为“*”的节点的路径代表一个单词。例如，从根节点到第一个“*”的路径是“BAT”。
- 分支表示单词的不同字符。例如，“BAT”和“CAT”共享第一个字符，但第二个字符不同，因此在第二层分开。

现在，如果我们要添加另一个单词比如“CAR”，Trie树将会更新为：

```
       Root
      /    \
     B      C
     |      |
     A      A
    / \    / \
   T   *  T   R
  /       /   |
 *       *    *
              |
              *  
```

在这个新的结构中，“CAT”和“CAR”共享前两个字符“CA”，但第三个字符不同，在“CAR”中为“R”，因此在第三层分叉。

**操作说明**

1. **插入**：当插入一个新单词时，从根节点开始，为单词的每个字符创建一个新的子节点（除非该字符已经存在）。到达单词的最后一个字符时，标记这个节点表示一个单词的结束。
2. **搜索**：为了查找一个单词，从根节点开始，沿着单词的字符移动。如果能够在每一步都找到相应的字符，并且最后一个字符被标记为一个单词的结尾，那么该单词存在于Trie树中。
3. **前缀搜索**：前缀搜索与全词搜索类似，但不需要最后一个字符被标记为单词的结尾。如果能够顺着前缀的字符在树中移动到最后一个字符，那么这个前缀存在于树中。

Trie树在处理大量字符串，尤其是进行快速前缀查找和词汇自动补全时非常有效。由于它基于共享前缀来存储单词，因此相比于其他数据结构，它在空间效率上通常更优。

**TrieNode 设计**

`TrieNode` 用于表示Trie树中的每个节点。接下来详细解释这个代码片段，包括`TrieNode`的设计以及每个成员的作用。

```cpp
class trie_node{
    public:
        bool is_end_of_word;
        std::unordered_map<char , trie_node*> children;

        trie_node(): is_end_of_word(false){}
};
```

1. `bool isEndOfWord;`：这是一个布尔型成员变量，用于标记当前节点是否代表一个单词的结束。如果`isEndOfWord`为`true`，则表示从根节点到当前节点的路径构成一个完整的单词。
2. `unordered_map<char, TrieNode*> children;`：这是一个无序映射（`unordered_map`），用于存储当前节点的子节点。Trie树的一个关键特性是每个节点可以有多个子节点，每个子节点对应一个字符。这个`unordered_map`允许我们将字符映射到相应的子节点。
3. `TrieNode() : isEndOfWord(false) {}`：这是`TrieNode`类的构造函数。构造函数用于初始化新创建的`TrieNode`对象。在这里，构造函数将`isEndOfWord`初始化为`false`，表示节点默认不是单词的结束。

现在让我们通过一个示例来解释如何使用这个`TrieNode`类来构建Trie树：

假设我们要在Trie树中插入单词"CAT"，首先我们创建一个根节点。然后，我们从根节点开始，在每个字符位置上创建一个新的`TrieNode`对象，将`isEndOfWord`设置为`false`。在这个过程中，我们将字符映射到相应的子节点，如下所示：

1. 创建根节点，`isEndOfWord`为`false`。
2. 在根节点下创建字符'C'对应的子节点，`isEndOfWord`为`false`。
3. 在字符'C'对应的子节点下创建字符'A'对应的子节点，`isEndOfWord`为`false`。
4. 在字符'A'对应的子节点下创建字符'T'对应的子节点，`isEndOfWord`为`true`，因为"CAT"的最后一个字符表示单词的结束。

这就是如何使用`TrieNode`类来构建Trie树的一部分。这个类的设计允许我们轻松地在每个节点上附加字符和标记单词的结束。随着插入更多的单词，Trie树的结构会不断扩展。

**完整代码**

这段代码实现了一个称为“Trie”（也被称为前缀树或字典树）的数据结构:

```cpp
#include<iostream>
#include<unordered_map>
#include<string>
class trie_node{
    public:
        bool is_end_of_word;
        std::unordered_map<char , trie_node*> children;

        trie_node(): is_end_of_word(false){}
};

class trie{
    private:
        trie_node* root;
        //Recursively cleans up memory
        void clear_memory(trie_node* node){
            for(auto pair : node->children)
                clear_memory(pair.second);
            delete node;
        }
    public:
        trie(){
            root = new trie_node;
        }

        void insert(std::string word){
            trie_node* current = root;
            for(char ch : word){
                if(current->children.find(ch) == current->children.end()){
                    current->children[ch] = new trie_node;
                }
                current = current->children[ch];
            }
            current->is_end_of_word = true;
        }
        //search whether word is in trie tree or not
        bool search(std::string word){
            trie_node* current = root;
            for(char ch : word){
                if(current->children.find(ch) == current->children.end())
                    return false;
                current = current->children[ch]; 
            }
            return current != nullptr && current->is_end_of_word;
        }

        //search whether prefix is the prefix of the word in trie tree or not
        bool start_with(std::string prefix){
            trie_node* current = root;
            for(char ch : prefix){
                if(current->children.find(ch) == current->children.end()){
                    return false;
                }
                current = current->children[ch];
            }
            return true;
        }


        ~trie(){
            clear_memory(root);
        }

};

int main()
{
    trie trie_tree;
    trie_tree.insert("nudt");
    trie_tree.insert("hust");
    std::cout << trie_tree.search("hust") << std::endl;
    std::cout << trie_tree.start_with("nud") << std::endl;
    return 0;
}
```

### c++/Python调用shell命令

[参考链接](https://mp.weixin.qq.com/s/ahXrzTlIJcQGW_ONPvRgJQ)
`C++`调用`shell`命令:

```cpp
char* command = malloc(sizeof(char)*100);
	snprintf(command , 100 , "sed -i '13c `define M_c %d' ../sim/testbench/mau_sau_tb_fp64.sv" , max_row);
	system(command);
	free(command);
```

`python`调用`shell`命令

```python
os.system("echo 'hello world'")
```



### 遍历数组的背后

[数组有序与否对遍历的影响](https://mp.weixin.qq.com/s/36ouxHNRoX4ZZJ4dxmkjeg)

[数组按行与按列遍历的差距](https://mp.weixin.qq.com/s/Mm8PqPM1vULK9Yr8tOnOgg) 

上面这篇文章写得很详细了，以下是总结与代码实现。

- 数组有序与否影响的是CPU的动态预测，有序的数组动态预测准确度很高。

```cpp
#include<iostream>

#include<vector>

#include<ctime>

#include<algorithm>

#define SIZE 1e8

int calc(const std::vector<int> &q){

    int a = 0 , b = 0 , len = q.size() ;

    for(int i = 0 ; i < len ; i++){

        if(q[i] < 500)  a++ ;

        else b++ ;

    }

    return a - b ;

}

int main()

{

   std::vector<int> q ;

   for(int i = 0 ; i < SIZE ; i++){

        q.push_back(rand() % 1000) ; //无序数组

        // q.push_back(i) ; //有序数组

   }

    clock_t start = clock() ;

    calc(q) ;

    clock_t end = clock() ;    

    double cost = (double) (end - start) / CLOCKS_PER_SEC ;

    std::printf("Time cost : %.3fs\n" , cost) ;

    //After sort

    std::sort(q.begin() , q.end() ) ;

    clock_t start1 = clock() ;

    calc(q) ;

    clock_t end1 = clock() ;    

    double cost1 = (double) (end1 - start1) / CLOCKS_PER_SEC ;

    std::printf("Time cost after sort : %.3fs\n" , cost1) ;

    return 0 ;

}
```

> Time cost : 0.570s
> Time cost after sort : 0.129s



### 传参尽量传引用

传参用引用！

- 如果我们用以下方式传参

```cpp
void func(vector<vector<int>> M)
```

子函数中就会初始化一个变量`M`，并将形参`M`复制一份给新的`M` , 这样会损失性能。

**传引用的本质就是传地址** ， 这样可以避免复制

```cpp
void func(vector<vector<int>> &M)
```

如果考虑到为防止在`func`中误操作修改了主函数的`M` ， 可以这样：

```cpp
void func(const vector<vector<int>> &M)
```

这样在`func`的作用域中，M便不可修改，不会影响主函数中的`M` .

- 实例

```cpp
typedef vector<int> vi ;
typedef vector<float> vf ;
typedef vector<vf> matrix ;
void encode_CS45D(const matrix &M , vf &val , vi &nr , vi &ptr , vi &idx){
    int m = M.size() , n = M[0].size() ;
    int NNZ = 0 , nr_t = -1 , t = 0 ;
    //NNZ : numble of zeros in each slash
    //nr_t : nr_temp
    //t indicate whenther there are all zeros in the slash
    for(int i = 0 , j = 0 ; i < m ; i++){
        t = 0 , nr_t++ ;
        for(int k = i , l = 0 ; (l < n && k >= 0) ; k-- , l++){
            if(M[k][l] != 0){
                val.push_back(M[k][l]) ;
                idx.push_back(l) ;
                NNZ++ , t = 1 ;
            }
        }
        if(t)   nr.push_back(nr_t) ;
        if(NNZ != ptr.back())   ptr.push_back(NNZ) ;
    }

    for(int i = m -1 , j = 1 ; j < n ; j++){
        t = 0 , nr_t++ ;
        for(int k = i , l = j ; l < n ; l++ , k--){
            if(M[k][l] != 0){
                val.push_back(M[k][l]) ;
                idx.push_back(l) ;
                NNZ++ , t = 1 ;
            }
        }
        if(t)   nr.push_back(nr_t) ;
        if(NNZ != ptr.back())   ptr.push_back(NNZ) ;
    }
    
}
```

如果在传参中，M的传参方式是`matrix M` , 在`Gdb`调试中，无法`step`进`encode_CS45D`这个函数，而是会进入`M`的**拷贝子程序**中(未知，较为底层)
#cpp 



