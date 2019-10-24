# C语言培训下

~~                                                          ~~

+ 结构体
  + 初始结构体
  + 结构体的使用
    + 结构体与函数
  + 结构体的应用——链表
    + 结构体变量指针
    + 链表概述
    + 链表操作
+ 文件
  + 为什么要有文件
  + 文件基本操作

## 结构体

**初识结构体**

_结构体是什么？_

就比如说，你要写一个个人信息的录入系统，它会包含很多内容（姓名、性别、身高、体重、身份证号、年龄、财产、爱好、性取向 等），这个时候你如果一个一个定义就会很麻烦。

```c
#include "stdio.h"
int main()
{
    char name[20] = "lier";
    int height = 175;
    int weight = 70;
    char sex = 'm';
    short age = 19;
    long  wealth  = 300000;
    printf(“李二的个人信息：\n");
    printf("姓名：%s,身高：%d,性别：%c,年龄：%d,财产：%d\n",name,height,sex,age,wealth);
    return 0;
}
```

**这个时候结构体就能发挥它的优势**

```c
struct Stu{   // struct 关键字   Stu名称
    int height;//身高
    int weight;//体重
    char sex;//性别
    int age; //年龄
    long wealth;
};
```

_这里说一下**typedef**这是一个重命名的关键字，如果结构体这样写_

```c
typedef struct Stu{   // struct 关键字   Stu名称
    int height;//身高
    int weight;//体重
    char sex;//性别
    int age; //年龄
    long wealth;
}N;

typedef + 数据类型 + 你想要重命名的英文
```


**就表明将这个定义的结构体重新命名为N**

+ 结构体变量的初始化
  

结构体也是一种数据类型，从某种程度上说与int等类似，属于同级，所以定义变量的方式也是一样的。

```c
struct Stu stu1,stu2;  //这里定义了量的Stu类型的变量
```

+ 结构体成员的赋值

结构体成员的获取形式为：

```c
结构体变量名.成员名;
```

**例如**

```c
Stu stu1;
stu1.age = 19;
stu1.height = 175;
stu1.sex = 'm';
stu1.wealth = 30000;
stu1.weight = 70;
printf("身高：%d,性别：%c,年龄：%d,财产：%d\n",stu1.height,stu1.sex,stu1.age,stu1.wealth);
```

## 结构体的使用

+ 结构体与数组

**结构体中的成员变量可以是数组，这样可以省略很多定义，这就是数组的用处了，在这里就不赘述了**

+ 结构体与指针

**结构体可以作为函数的参数传进子函数中，然后再子函数中使用**

#### 下面是一个输出函数

**Node 是一个结构体，print()是一个子函数，这个字函数有一个Node类型的参数**

```c
void print(Node *head) 
{
    Node *p = head;
    if (!p)
	    printf("\n链表是空的！");
    else {
	    while (p) {
		    printf("%d->", p->info);
	    	p = p->next;
	    }
    }
    printf("\n");
}
```

## 函数的应用———链表

结构体变量指针

+ 结构体变量指向自身

_也就是说定义了一个结构体类型的指针，这个指针指向了结构体本身_



![]( https://github.com/qinjiahao666/Study-notes/blob/master/C/pictures/3.png )

```c
struct table{
int i;
char c;
struct table *st;   //定义的结构体指针指向了本身
};
```

+ 指向其它结构变量

即将定义的两个结构体变量，比方说定义了 st1 和 st2两个结构体变量，只需要将st2 的地址 赋给 st1 的指针域，这样 st1 的指针就指向了 st2



![]( https://github.com/qinjiahao666/Study-notes/blob/master/C/pictures/4.png )

```c
int main()
{
    table st1 = {1,'a'};
    table st2 = {2,'b'};
    st1.st = &st2;

    //使用结构体变量输出st1自身的2个成员的值
    printf("%d %c\n",st1.i,st1.c);

    //使用结构体指针域所指向的结构体输出数值,即 st2 中的数值
    printf("%d %c\n",st1.st->i,st1.st->c);

    //使用结构体变量输出st2自身的2个成员的值
    printf("%d %c\n",st2.i,st2.c);

    return 0;
}
/*
1 a
2 b
2 b
*/
```

### 链表



![]( https://github.com/qinjiahao666/Study-notes/blob/master/C/pictures/4.png )

**链表的最小单元———节点**

```c
struct table
{
    int i;
    char c;
    struct table *next;
}
strcut table st1 = {1,'a'};
struct table st2 = {2,'b'};
st1.next = &st2;
```

**动态创建链表**

+ 构造一个结构类型，此结构类型必须包含至少一个成员指针，此指针要指向此结构类型，
+ 定义3个结构体类型的指针，按照用途可以命名为，p_head,p_rail,p_new
 + 动态生成新的结点，为各成员变量赋值，最后加到链表当中

    struct node
    {
        short i;   数据域
        char c;   ///数据域
        struct node *next;  //指针域，用于指向下一个结点
    }

定义结构体指针，不一定要在main函数中定义

```c
struct node *p_head,*p_rail,*p_new ;  //如果加上typedef就更完美了
```

**使用malloc函数冬天申请存储空间，声明形式**

```c
p_head = (struct node*)malloc(sizeof(struct node));  
```

+ (struct node*)类型
+ malloc()申请空间
+ sizeof() 申请的大小
+ 需要注意的是，在使用完这个结构体以后要将申请的空间释放，调用的函数为free();



![]( https://github.com/qinjiahao666/Study-notes/blob/master/C/pictures/9.png )

**实例**

构造结构体

```c
struct node {
short i;
char c;
struct node *next;
};
```

定义变量

```c
struct node node1 = {1,'A'};
struct node node2 = {2,'B'};
struct node node3 = {3,'C'};
node1.next = &node2;
node2.next = &node3;
```



![]( https://github.com/qinjiahao666/Study-notes/blob/master/C/pictures/8.png )

动态申请节点并添加到链表中

```c
struct node *p_new;
p_new = (struct node *)malloc(sizeof(struct node));
p_new->i = 4;
p_new->c = 'd';
node3.next = p_new;
```

**链表操作**

插入

+ 插入节点到第一个数据前面

    ```c
    struct node p_new = (struct node *)malloc(sizeof(struct node));  //创建新结点，并为其开辟空间
    scanf("%d%c",&(p_new->i),&(p_new->c));  //录入结点数据
    //插入节点
    p_new->next = p_head-next;
    p_head->next = p_new;
    ```



![]( https://github.com/qinjiahao666/Study-notes/blob/master/C/pictures/7.png )

+ 插入节点到链表中间

    ```c
    struct node p_new = (struct node *)malloc(sizeof(struct node));  //创建新结点，并为其开辟空间
    p_new->i = 2;
    p_new->c = 'B';
    
    struct node *p_front = p_head->next;
    p_new->next = p_front->next;
    p_front->next = p_new;
    ```



![]( https://github.com/qinjiahao666/Study-notes/blob/master/C/pictures/6.png )

+ 插入节点到末尾

    ```c
    while(1)
    {
    if(p-next == NULL)
    {
        p_rail = p;
        break;
    }
        p = p->next;
    }
    p_rail->next = p_new;
    p_tail = p_new;  
    ```



![]( https://github.com/qinjiahao666/Study-notes/blob/master/C/pictures/11.png )

+ 删除链表中的结点

    
    
    
    
    ![]( https://github.com/qinjiahao666/Study-notes/blob/master/C/pictures/10.png )
    
    ```c
    void del_list(struct node *p_head,int pos)
    {
        strct node *p_front,*p_del;
        p_front = p_head;
        for(int i = 0;i <= pos - 1;i ++)
        {
            p_front = p_front->next;
        }
        p_del = p_front->next;
        p_front->next = p_del->next;
        free(p_del);
    }
    ```

## 文件

**为什么要有文件操作**

两个没有解决的问题

+ 不得不再次运行程序

  + 我们运行计算机上的程序，然后不断输入数据给程序，然后得到程序对程序的处理结果，如果关掉程序的话，再想看到那些数据，就不得不再次运行程序。而且如果数据量过大的话，没办法留住这些数据
不得不重新输入数据

  + 每次在循行程序的时候吗，每运行一次都要重新的从简盘再次录入数据，而文件的运用帮我们解决了这个繁琐的问题

### 文件操作

**写入数据**

想要让程序在文件中写入文件，在程序与文件建立关联的时候，必须保证打开方是可写的，有 4 中方式可以将数据写入文件当中

+ 字符方式
+ 格式化方式
+ 字符串方式
+ 二进制方式

1. 字符方式

    程序可以以字符为单位，一个字符一个字符的将数据写入到文件当中，需要的函数是 fputc(),声明如下：

    ```c
    int fputc(char c,FILE *stream);
    ```

    + 参数c代表将要被写进去的字符
    + 参数stream是一个文件指针，只想被写入的文件

        ```c
        char ch; //定义一个字符串
        int i = 0;
        ch = getchar();
        while(ch != '\n');
        {
            i = fputc(ch,fp);  // 以字符为单位，写入到text.txt文件
            if(i == -1)
            {
                puts("字符写入失败！");
                exit(0);
            }
            ch = getchar();
        }
        ```

2. 格式化方式

    如果写入文件的内容有特定的格式要求，可以使用格式化的方式将数据写入到文本

    stdio.h 提供了一个库函数 fprintf(),可以达到这个目的，声明如下：

    ```c
    int fprintf(FILE *stream,const char *format[, argument ] ...);
    ```

    和 printf()的使用方法一致

    + 参数的 stream 是指向将要被写入数据的文件的文件指针
    + 参数 format 是格式化的字符串
    + 参数 argument 是可选的，如果 format 中有格式符，argument 就是对应的变量
    + 函数 fprintf() 返回实际输入到文件中的字符的个数

        ```c
        struct info
        {
            short no;
            char name[10];
            char sex[6];
    };
        
        struct info info_st[3] ={
            {1,"baoqianyue","men"},
            {2,"lihao","men"},
            {3,"wanghao","men"}
        };
        for(int i = 0;i < 3;i ++)
        {
            fprintf(fp,"No = %d\tname = %-8s\tsex = %-6s\n",info_st[i].no,
                info_st[i].name,info_st[i].sex);
        }
        ```

3. 字符串方式

    ```c
    char c[100];
    gets(c);
    int value = fputs(c,fp);  //fp指向文件
    if(value == -1)
    {c
        puts("字符串写入失败！\n");
        exit(0);
    }
    ```

4. 二进制方式

    ```c
    struct info
    {
        short no;
        char name[10];
        char sex[6];
};
    
    struct info info_st[3] ={
        {1,"baoqianyue","men"},
        {2,"lihao","men"},
        {3,"wanghao","men"}
};
    
int count = fwrite(info_st,sizeof(struct info),3,fp);  //写入数据到文件
    ```
    
     + info_st 结构体类型指针
     + sizeof 大小
     + 3 count 有几条数据
     + fp 指向文件

**读取数据**

1. 字符方式：fgets()函数
2. 格式化方式：fscanf()函数
3. 字符串方式：fgets()函数
4. 二进制方式：fread()函数
