#  c语言培训下
+  指针   
    + 指针及其使用方法
         + 初识指针
         + 指针能干什么
         + 指针的基本运算
    + 指向数组的指针
         + 指向一维数组的指针
         + 指向二维数组的指针
    + 指针数组
    + 数组指针
    + 二级指针
    + 指针在函数中的作用
       + 指针作为函数参数
       + 指针作为函数的返回值
       + 指向函数的指针

## 指针


**指针及其使用方法**

**初识指针**

指针是什么？

如果在程序中定义了一个变量，在对程序进行编译时，系统就会给这个变量分配内存单元。编译系统根据程序中定义的变量类型，分配一定长度的空间。内存区的每一个字节有一个编号，这就是“**地址**”。

由于通过地址能找到所需的变量单元，可以说，地址指向该变量单元，将地址形象化地称为“**指针**”。

_如何定义指针变量？_

+ 指针本身就是一个变量，它存储的是数据在内存中的地址而不是数据的值。

    ```c
    数据类型* 变量名 或 数据类型 *变量名 //两者均可
    
    int *pointer;
    char *name;
    ```

+ 这样就定义了两个指针变量，int 和 char 是这两个指针变量的数据类型，*表示这是指针变量
+ **指针变量的初始化：** 每一个变量都有一个内存位置，每一个内存位置都定义了可使用连字号（&）运算符访问的地址，它表示了在内存中的一个地址。
  
    ```c
    int a = 10,*p;
    p = &a;
    //或者
    int a = 10;
  int *p = &a;
  ```
  + 第一种我们可以理解为要定义一个int 型的指针 p，然后再对这个指针p取a的地址。
  + 第二种方法其实和第一种的意思是一样的，这种方法可以理解为在定义指针变量p的同时将a的地址赋给p。
  + 备注：在变量声明的时候，如果指针变量没有确切的地址可以赋值，为指针变量赋值NULL，养成一个良好的编程习惯。被赋值为NULL的指针被称为空指针。NULL指针是定义在标准库中的值为零的常量。
        
    ```c
    int  *ptr = NULL;
    printf("ptr 的地址是 %p\n", ptr  );
    return 0;
    /*
    结果：
    ptr 的地址是 0x0
    */
    ```



_理解 “&” 和 “*” （取地址和指针运算符）_

```c
int a=100,b=10;
//定义整型变量a,b，并初始化
int *pointer_1,*pointer_2;
//定义指向整型数据的指针变量pointer_1, pointer_2
pointer_1=&a;	//把变量a的地址赋给指针变量pointer_1
pointer_2=&b;	//把变量b的地址赋给指针变量pointer_2 
printf("a=%d,b=%d\n",a,b);	//输出变量a和b的值
printf("*pointer_1=%d,*pointer_2=%d\n",*pointer_1,*pointer_2);

/*
a=100,b==10
*pointer_1=100,*pointer_2=10
*/
```

**指针基本运算**

指针就是地址，地址在内存中也是以数的形式存在，所以指针也能进行基本运算。

```c
int a;
int *p = &a;
printf("%p\n",p);
p++;
printf("%p\n",p);
p -= 2;
printf("%p\n",p);
return 0;
/*
000000000062FE44
000000000062FE48
000000000062FE40
*/
```

## 指向数组的指针

**指向一维数组的指针**

+ 数组中的每个数据都会保存在一个储存单元里面，只要是储存单元就会有地址，所以就可以用指针保存数组储存单元的地址。
  

_为指针赋数组数据的地址_

```c
    int *p = NULL;
    int num[5] = {1,2,3,4,5};
    for(int i = 0;i < 5; i++)
    {
        p = &num[i];
        printf("%d ",*p);
    }
```

_使用数组名为指针赋值_

+ 第一种
        
    ```java
    int num[5] = {1,2,3,4,5};
    int *p;
    p = &num[0];
  ```
  
+  第二种：直接将数组名赋予指针，指针需要储存的数据就是地址，而num就代表该数组的首地址。

    ```python
    p = num;
    ```

![]( https://github.com/qinjiahao666/Study-notes/blob/master/C/pictures/1.png )

+ 对于*(num + i),num 是数组的首地址，指向数组的首元素，而num + i则是数组的第i个元素的地址，再加上指针运算符* 就得到了该元素的值



+ array每次加一的时候，它的值都会增加sizeof(int)，加i的时候就增加i *sizeof(int)
  
    ```c
    int num[5] = {2,4,6,8,10};
    for(int i = 0;i < 5;i ++)
    {
        //通过数组下标遍历数组
        printf("%d",num[i]);
        //通过指针变量遍历数组
        printf("%d",*(num + i));
    }
    ```

**指向二维数组的指针**

跟一维数组相似

```c
int num[3][2] = {{1,2},{3,4},{5,6}};
int *p = &num[0][0];
```

+ **注意：** 不能为指针直接赋予二维数组的数组名，即上面的代码不能写成： int *p = num;

+ 假设定义一个二维数组：num[m][n];一个指针p指向了这个二维数组的首地址，那么对于数组的数据 num[i][j](0 <= i < m,0 <= j < n) ，指针变量p要想指向这个数据,那么指针变量 p = p + n * i + j;

    ```c
    double arr[4][3] = {
    {78.4,72.1,41.2},
    {56.4,12.4,45.1},
    {12.5,14.6,20.4},
    {23.5,34.6,67.8}
    }
    double *p_d = &arr[0][0]; //指针变量的类型必须要跟数组类型一致
    printf("二维数组中arr[3][2]位置上的数据为：%6.11f\n",*(p_d + 3 * 3 + 2));
    ```

### 指针数组 
**顾名思义：** 保存指针的数组

+ 一维指针数组的定义形式为：类型名 *数组名p[数组长度];
        
    
```c
  int *prt_array[10];
  ```
  
+ 括号里面ptr_array[10]表示的是一个长度为10的数组，然后括号外面的* 说明数组的元素类型是 int* 的指针类型
+ 实例
  
    ```c
    int main()
    {
     int a = 16, b = 932, c = 100;
    //定义一个指针数组
     int *arr[3] = {&a, &b, &c};
    printf("%d %d %d\n", *arr[0], *arr[1], *arr[2]);
    return 0;
    }
    /*
    16 932 100
    */
    ```

### 数组指针
 **顾名思义：** 指向数组的指针

如果一个指针指向了数组，就称它为数组指针。

```c
int a[4][3] = {{0,2,3},{1,5,6},{2,3,4},{7,8,9}};
```

在概念上的矩阵是像这种矩阵的样子：

```c
0 2 3
1 5 6
2 3 4
7 8 9
```

但实际上它在内存中是链式存储的：

```c
0 2 3 1 5 6 2 3 4 7 8 9
```

二维数组可以分解成多个一维数组，a[0]包括a[0][0]、 a[0][1]、a[0][2] 三个元素

```c
          a[][0]  a[][1]  a[][2]
    a[0]    0       2       3
    a[1]    1       5       6
    a[2]    2       3       4
    a[3]    7       8       9
```

这里的a 就是那四个一维数组的组名，接着 定义一个数组指针

```c
int (*p)[3] = a;
```

_括号里面的*代表p是一个指针，[3]代表这个 指针 p指向了类型为int[3]的数组_


+ p指向数组a的开头，就是指向数组的第0行元素，p + 1 指向数组的第一行元素
+ 所以 *(p+1) 就表示数组的第一行元素的值，有多个数据
+  *(p+1) + 1表示第一行的第一个数据的地址

### 二级指针
**顾名思义：** 指向指针的指针

假设有一个 int 类型的变量 a，p1是指向 a 的指针变量，p2 又是指向 p1 的指针变量，它们的关系如下图所示：

用代码形式展现就是：

![]( https://github.com/qinjiahao666/Study-notes/blob/master/C/pictures/2.PNG )    

```c
int a = 100;
int *p1 = &a;
int **p2 = &p1;
```

指针变量也是一种变量，也会占用存储空间，也可以使用&获取它的地址。C语言不限制指针的级数，每增加一级指针，在定义指针变量时就得增加一个星号*。p1 是一级指针，指向普通类型的数据，定义时有一个*；p2 是二级指针，指向一级指针 p1，定义时有两个*。

_同样的道理也会有三级指针、四级（司机）指针等等_

### 指针在函数中的作用

**指针作为函数的参数**

在c语言中实参和形参之间的数据传输是单向的“值传递”方式，也就是实参可以影响形参，而形参不能影响实参。指针变量作为参数也不例外，但是可以改变实参指针变量所指向的变量的值。


```c
void swap2(int *px,int *py){
    int t;
    t=*px;
    *px=*py;
    *py=t;
}

int main()
{
    int a=1,b=2;
    int *pa=&a,*pb=&b;

    swap2(pa,pb);
    printf("a=%d,b=%d\n",a,b);

}

/*
a=2,b=1
*/
```



**指针作为函数的返回值**

**指针函数**

C语言允许函数的返回值是一个指针（地址），我们将这样的函数称为指针函数。下面的例子定义了一个函数 strlong()，用来返回两个字符串中较长的一个：

```c
#include <stdio.h>
#include <string.h>
char *strlong(char *str1, char *str2){
    if(strlen(str1) >= strlen(str2)){
        return str1;
    }else{
        return str2;
    }
}
int main(){
    char str1[30], str2[30], *str;
    gets(str1);
    gets(str2);
    str = strlong(str1, str2);
    printf("Longer string: %s\n", str);
    return 0;
}
/*
android
android-lab
Longer string: android-lab

*/
```

需要注意的是：**函数运行结束后会销毁在它内部定义的所有局部数据，包括局部变量、局部数组和形式参数。**
