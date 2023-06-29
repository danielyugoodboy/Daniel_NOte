# CPP Usage

## 1. Base Concept
* Patern Architecture
    ```
    Patern_Test Folder
    |_ Patern_Test.pro
    |
    |_ Header Folder
    |   |_ Patern_Test.h
    |   |_ patern_test_global.h
    |
    |_ Source Folder
        |_ Patern_Test.cpp

    ```
* 每個文件都有相同的“.pro”文件和“_global.h”文件，但文件內的patern名稱不同。

## 2. 條件式編譯
* 參考(務必看):https://ithelp.ithome.com.tw/articles/10283174

    條件編譯就是根據已經定義的macro進行選擇性判斷的語句，它會在compiler進行編譯前完成，主要由預處理器負責。

    和一般的條件語句不同的是，條件編譯在compile之前就已經決定，相反的，正常的條件語句(if, else if, else...)需要我們在執行時(run time)才能進行判斷

    ![Alt text](CPP_compile_img.png)

* note: "#" 這是前置處理器

    最簡單的綜合用法:
    ```cpp
    #include <stdio.h>

    /*若a沒有被定義就定義它*/
    #ifndef a
    #define a 1
    #endif

    int main(){
        #if (a == 1)
            printf("a == 1\n");
        #else
            printf("a != 1\n");
        #endif
        
        return 0;
    }
    ```
### 2.1 C++中 ```#if / #elif / #else / #endif``` 的用法
* example
    ```cpp
    #include <stdio.h>
    #define test1 10
    // #define test2 1

    int main(){
    #if (test1 > 8) && (test1 < 15) && defined(test2)
        printf("Macro test meet the requirement");
    #elif(test1 > 15)
        printf("Macro test meet the requirement, but way too big");
    #else
        printf("Macro test doesn't meet the requirement");
    #endif
    }
    ```
    output: Macro test doesn't meet the requirement

### 2.2 C++中 ```#ifndef / #define / #endif``` 的用法
* 定義

    其實#ifdef就是#if defined()；#ifndef就是#if !defined()，使用目的當然也是用來判斷macro是否被定義，它的使用邏輯如下:

    a. 若macro有定義:

        #ifdef()會判斷為true
        #ifndef()會判斷為false

    b. 若macro沒有定義
    
        #ifdef()會判斷為false
        #ifndef()會判斷為true

* example:
    ```cpp
    #include <stdio.h>

    #define test1 1
    #define test2 0
    int main(){
        #ifndef test1 // #if !defined(test1)
            printf("test1 is not defined...\n");         
        #else
            printf("test1 is defined...\n");   
        #endif
        
        #ifdef test2 // #if defined(test2)
            printf("test2 is defined...\n");         
        #else
            printf("test2 is not defined...\n");   
        #endif
        
        return 0;
    }
    ```
    output:

    test1 is defined...
    
    test2 is defined...

### 2.3 標頭守衛
* example:
    ```cpp
    /*test1.h*/
    #ifndef __TEST1_H
    #define __TEST1_H


    // ...

    #endif // __TEST1_H
    ```

## 3. C++ this 指標
* 定義

    在 C++ 中，this 指針是一個特殊的指針，它指向當前對象的實例。
    
    在 C++ 中，每一個對象都能通過 this 指針來訪問自己的地址。

    this是一個隱藏的指針，可以在類的成員函數中使用，它可以用來指向調用對象。

    當一個對象的成員函數被調用時，編譯器會隱式地傳遞該對象的地址作為 this 指針。

    友元函數沒有 this 指針，因為友元不是類的成員，只有成員函數才有 this 指針。

    下面的實例有助於更好地理解 this 指針的概念：

    (This 用在class定義的程式碼區間中使用，指向自己的值)

* Example
    ```cpp
    #include <iostream>
    
    using namespace std;
    
    class Box
    {
    public:
        // 构造函数定义
        Box(double l=2.0, double b=2.0, double h=2.0)
        {
            cout <<"调用构造函数。" << endl;
            length = l;
            breadth = b;
            height = h;
        }
        double Volume()
        {
            return length * breadth * height;
        }
        int compare(Box box)
        {
            return this->Volume() > box.Volume();
        }
    private:
        double length;     // 宽度
        double breadth;    // 长度
        double height;     // 高度
    };
    
    int main(void)
    {
    Box Box1(3.3, 1.2, 1.5);    // 声明 box1
    Box Box2(8.5, 6.0, 2.0);    // 声明 box2
    
    if(Box1.compare(Box2))
    {
        cout << "Box2 的体积比 Box1 小" <<endl;
    }
    else
    {
        cout << "Box2 的体积大于或等于 Box1" <<endl;
    }
    return 0;
    }
    ```

    output:

    调用构造函数。
    
    调用构造函数。
    
    Box2 的体积大于或等于 Box1
