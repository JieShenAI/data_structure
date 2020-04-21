## 栈(stack)

## 定义：
栈是一种只能在一段进行插入或删除操作的线性表。

可以插入删除的一端叫栈顶，另一端叫栈底。

## 逻辑结构：

栈的逻辑结构属于线性表，只不过在操作上加了一些约束。先进后出。

## 存储结构：

### 顺序栈：

```cpp
int stack[maxSize];
int top = -1;
//入栈
stack[++top] = 1;
//出栈
int x = stack[top--];
top == -1;//真，为栈空
top == maxSize - 1;//真，为栈满
```



### 链式栈：

**带头结点**写法

```cpp

LNode *head = (LNode*)malloc(sizeof(LNode));
head->next = NULL;
LNode *top = NULL;
//入栈（就是单链表的头插法）
top = (LNode*)malloc(sizeof(LNode));
top->next = NULL;
top->data = 'A';
top->next = head->next;
head -> next = top;
//出栈
x = top -> data;
head -> next = top -> next;
free(top);
top = head -> next;

//判空
head->next == NULL;//真，则栈空
//只要有足够内存，栈就不会满。
```

### 应用：

【例】：**不带头结点**的单链表存储链栈，设计初始化栈、判断栈空、进栈和出栈等相应的算法。

分析：

不带头结点的单链表lst为空的条件是lst==NULL。

```cpp
#include <iostream>
#define maxSize 100
using namespace std;
typedef struct LNode{
    int data;
    struct LNode *next;
}LNode;
void initStackl(LNode* &lst){
    lst = NULL;
}
int isEmptyl(LNode *lst){
    if(lst == NULL)
        return 1;
    else
        return 0;
}
//进栈
void pushl(LNode* &lst,int x){
    LNode *p;
    p = (LNode*)malloc(sizeof(LNode));
    p->data = x;
    p->next = lst;
    lst = p;
}
int popl(LNode* &lst,int &x){
   //别忘记栈空检查
    if(lst==NULL)
       return 0;
   x = lst->data;
   LNode *p = lst;
   lst = lst->next;
   free(p);
   return 1;
}
int main()
{
    LNode* lst;
    initStackl(lst);
    pushl(lst,1);
    pushl(lst,2);
    cout<<lst->data<<" "<<lst->next->data<<endl;
    int x;
    popl(lst,x);
    cout<<x<<" ";
    popl(lst,x);
    cout<<x<<endl;
    cout<<isEmptyl(lst);
    return 0;
}
```



## 队列（queue）：

队列是一种只能在一端插入元素（队尾），只能在另一端删除元素的线性表（队头）。

线性表：

队列的逻辑结构属于线性表，只不过在操作上加了一些约束。

队尾(Rear)进，队头(Front)出。先进先出，First in,First out.

### 存储结构

顺序队(也称循环队列)：

```cpp
int queue[maxSize];
int front = 0,rear = 0;
//front不指向当前元素，它的下一个元素才是当前的元素。
//rear指向的是队尾
//让队列变成一个环
//入队
rear = (rear + 1) % maxSize;
queue[rear] = x;
//出队
front = (front + 1) % maxSize;
x = queue[front];
//判空
rear = front;
```

#### 循环队列的要素：

1. 队空状态：`qu.rear == qu.front;`

2. 队满状态：`(qu.rear + 1)%maxSize = qu.front;`

3. 进队：

   ```cpp
   //判断队满之后
   qu.rear = (qu.rear+1)%maxSize;
   qu.data[qu.rear] = x;
   ```

4. 出队：

   ```cpp
   //判断队空之后
   qu.front = (qu.front+1)%maxSize;
   x = qu.data[qu.front];
   ```

   

#### 链队：

 特点：不存在队列满上溢的情况（只要内存够）。

链队的要素：

1. 队空状态

   ```cpp
   lqu->rear==NULL;
   //或者
   lqu->front==NULL;
   ```

2. 队满状态

   无队满状态。

3. 进队

   ```cpp
   lqu->rear->next = p;
   lqu->next = p;
   ```

4. 出队

   ```cpp
   p = lqu->front;//为了free(p)
   lqu->front = p->next;
   x = p->data;
   free(p);
   ```

   如果空队列，入队一个元素：

   ```cpp
   if(lqu->rear == NULL)
       lqu->front = lqu->rear = p;
   ```

   如果只剩下一个元素了，出队：

   ```cpp
   if(lqu->rear == NULL)
       return 0;
   if(lqu->rear == lqu->front)
       lqu->rear = lqu->front = NULL;
   ```

   ### 共享栈和双端队列

   1. 共享栈

      为了增加内存空间的利用率和减少溢出的可能性，当两个栈共享一片连续的内存空间的时，应将两栈的==栈底==分别设在这片内存空间的两端，扎样当==两个栈的栈顶在栈空间的某一位置相遇==时，才产生上溢。

   2. 双端队列

      （太难了，稍后更新！）

   

   ### 队列配置问题：

   前面介绍的是正常配置。

   #### 非正常配置

   优点：front指向的就是当前元素。

   简述：

   ​	队空是头在尾的前一个；

   ​	有元素之后，rear++，rear就不可能在front的后面了。

   ​	队满是front退两格。因为front退一格是队空，在队空再退一个就队满了，因为循环嘛。

   1. 队空(头在尾的前一个)

      ```cpp
      front == (rear+1)%maxSize;//为真 
      ```

   2. 队满

      ```cpp
      front = (rear+2)%maxSize;//为真，关系式牢记
      ```

   3. 入队

      ```cpp
      rear = (rear+1)%maxSize;
      queue[rear] = x;
      ```

   4. 出队

      ```cpp
      x = queue[front];
      front = (front+1)%maxSize;
      ```

      

   

   【例】（真题）：

   循环队列存在数组A[]中，队列非空时，front和rear分别指向队头和队尾。初始化队列为空，要求第一个进入队列的元素在A[0]处，初始时`front = 0`,`rear = n-1`。

   ## 真题仿造

   ​	（为了方便调试，给出的代码都包含main函数，**考试只需要写主要函数就行了**。）
   
   1. 实现共享栈。顺序栈S~0~、S~1~共享一个存储区elem[maxSize];
   
      定义:
   
      ```cpp
      typedef struct{
          int elem[maxSize];
          int top[2];//top[0]为S0栈顶,top[1]为S1栈顶。
      }SqStack;
      //入栈
      int push(SqStack &st,int stNo,int x);//stNo
      //出栈操作
      int pop(SqStack &st,int stNo,int &x);
      ```
   
      
   
   ```cpp
   #include <iostream>
   #define maxSize 100
   using namespace std;
   typedef struct{
       int elem[maxSize];
       int top[2];//top[0]为S0栈顶,top[1]为S1栈顶。
   }SqStack;
   void init_SqStack(SqStack &st){
       st.top[0] = -1;
       st.top[1] = maxSize;
   }
   //入栈
   int push(SqStack &st,int stNo,int x) {//stNo
       //先判断栈满
       if(st.top[1] - st.top[0] == 1)
           return 0;
       if(stNo == 0){
           st.top[0]++;
           st.elem[st.top[0]] = x;
       }else if(stNo == 1){
           st.top[1]--;
           st.elem[st.top[1]] = x;
       }
       return 1;
   }
   //出栈操作
   int pop(SqStack &st,int stNo,int &x){
       //记得判断栈空
       if(stNo == 0){
           if(st.top[0] == -1)
               return 0;
           x = st.elem[st.top[0]];
           st.top[0]--;
       } else if(stNo == 1){
           if(st.top[1] == maxSize)
               return 0;
           x = st.elem[st.top[1]];
           st.top[1]++;
       }
   }
   void print(SqStack st){
       cout<<"S0:\n";
       while(st.top[0]>=0){
           cout<<st.elem[st.top[0]]<<" ";
           st.top[0]--;
       }
       cout<<endl;
       cout<<"S2:\n";
       while(st.top[1]<maxSize){
           cout<<st.elem[st.top[1]]<<" ";
           st.top[1]++;
       }
       cout<<endl;
   }
   int main()
   {
       SqStack st;
       init_SqStack(st);
       for(int i=0;i<4;i++){
           push(st,0,i);
           push(st,1,i+5);
       }
       print(st);
       int x;
       pop(st,0,x);
       cout<<"x:"<<x<<endl;
       pop(st,1,x);
       cout<<"x:"<<x<<endl;
       print(st);
       return 0;
   }
   ```



​	2.请利用两个栈S~1~和S~2~来模拟一个队列，假设栈中元素为int型。

```cpp
//3个运算的定义
push(ST,x);//元素x入ST栈
pop(ST,&x);//ST栈顶元素出栈，赋给变量x
isEmpty(ST);
/*
利用上述3个运算来实现该队列的3个运算：enQueue(元素入队列)、deQueue(元素出队列)、isQueueEmpty(判断队列是否为空，空返回1，不空返回0)。
*/
```

【分析】：S~1~作为输入元素存放的地方，S~2~作为输出元素存放地方；

S~1~的元素**全部**（必须全部）倒进S~2~的两个条件，任意满足一个S~1~即可倒入S2：

1. S~1~栈满
2. 当出现pop命令但是S~2~为空。

S~2~不为空的时候，是因为已经有S~1~的元素倒入到S~2~了。

**当S~2~的所有元素没有pop完，不允许S~1~倒元素倒S~2~**，如果S~1~栈满，那就拒绝push操作。

```cpp
#include <iostream>
#define maxSize 5//设置小一点方便检查
using namespace std;
typedef struct{
    int data[maxSize];
    int top;
}SqStack;
void init_SqStack(SqStack &ST){
    ST.top = -1;
}
int push(SqStack &ST,int x) {//元素x入ST栈
    //检查栈满
    if(ST.top == maxSize - 1)
        return 0;
    ST.data[++ST.top] = x;
    return 1;
}
int pop(SqStack &ST,int &x) {//ST栈顶元素出栈，赋给变量x
    //检查栈空
    if(ST.top < 0)
        return 0;
    x = ST.data[ST.top];
    ST.top--;
}
int isEmpty(SqStack ST){
    if(ST.top == -1)
        return 1;
    else
        return 0;
}
int enQuenue(SqStack &s1,SqStack &s2,int x){
    //检查栈满
    //S1没有满，直接压
    if(s1.top != maxSize - 1){
        s1.data[++s1.top] = x;
    }else{
        //s1满了
        //s2非空就拒绝压栈
        if(s2.top != - 1)
            return 0;
        //s2是空栈，就把s1(记住是全部哦！)倒入到s2
        int y;
        while(s1.top != -1){
            pop(s1,y);
            push(s2,y);
        }
        //再把x压入空栈的s1
        push(s1,x);
        //
    }
}
int deQuenue(SqStack &s1, SqStack &s2, int &x){
    //s2有元素就直接弹出
    if(s2.top != -1)
        x = s2.data[s2.top--];
    else{
        //如果s1也没有元素，判定为空栈
        if(s1.top==-1)
            return 0;
        else {
            //把s1全部倒入s2，再在s2中出栈
            int y;
            while (s1.top != -1) {
                pop(s1,y);
                push(s2,y);
            }
            pop(s2,x);
        }
    }
}
int isQueueEmpty(SqStack s1,SqStack s2){
    if(s1.top==-1 && s2.top==-1)
        return 1;
    else
        return 0;
}
int main()
{
    SqStack s1;
    SqStack s2;
    init_SqStack(s1);
    init_SqStack(s2);
    int x;
    for(int i=0;i<8;i++){
        enQuenue(s1,s2,i);
        //当i==5时，出栈
        if(i==5) {
            deQuenue(s1,s2,x);
            cout << "pop value:" << x << endl;//出栈元素为：
        }
    }
    while(!isQueueEmpty(s1,s2)){
        deQuenue(s1,s2,x);
        cout<<x<<" ";
    }
    cout<<endl;
    return 0;
}
```



