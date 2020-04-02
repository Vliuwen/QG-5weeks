# C语言链表的操作

学习完链表，对链表内容做一个总结归纳。从生成链表到链表的简单操作到复杂操作。有什么不对地方也希望大家可以指出来，希望各位见谅。现在步入总结。

## 单链表的创建

链表的创建首先得定义结构体，然后创建头节点，最后生成链表。

### 定义结构体

```c
//单链表结点
typedef struct NODE {
	int data;				//数据域 
	struct NODE* next;	    //指针域，指向下一个结点 
}Node;						//Node结构体别名
```

用<u>typedef简化</u>可以方便很多。

定义结构体后可以用<u>预处理命令将结构体的长度简化表示</u>：

`#define LEN sizeof(struct NODE)`



### 头结点创建c

结构体生成后就是创建头结点，我创建的链表是**头结点不储存数据**的，方便后续一些操作。声明：<u>表头称为头结点，储存数据的第一个结点称为首元结点</u>。创建头结点代码如下：

```c
//初始化一个链表结点
Node* init_node()
{
	Node* node = (Node*)malloc(LEN);
	if (node == NULL)
		printf("内存不足，建立失败！\n");
	else
	{
		node->data = 0;
		node->next = NULL;				//next为空
		printf("表头创建成功！\n"); 
		return node;
	}
}
```

头结点创建好了后就是生成链表，生成链表有两种方法：**头插法和尾插法**。

### 头插法

<u>头插法建立链表就是新创建的结点位于首元结点</u>。链表H->A，H为头节点，此时创建新结点B，则新结点的位置应该是H->B->A。再创建新结点C，则为H->C->B->A。

可以发现，头插法的特点为**后来居上**。代码如下：

```c
//头插法建立单链表(数据会反向) 
Node* createLink_head(Node* head,int n)		//n为需要建立多长的链表长度
{
	int i;
	for (i = 0; i < n; i++)
	{
		Node* node = (Node*)malloc(LEN);
		printf("请输入一个数： \n");
		scanf("%d", &node->data);
		node->next = head->next;			//将首元结点的地址存入新结点的指针域
		head->next = node;					//将新结点的地址存入头结点的指针域
	}
	printf("链表创建成功！\n");
	return head;
}
```



### 尾插法

<u>尾插法建立链表就是新结点位于表尾</u>。链表H->A,H为头结点，此时创建新结点B，B位于链表尾，则链表为：H->A->B。再创建新结点C，链表为H->A->B->C。代码如下：

```c
//尾插法建立单链表
Node* createLink_tail(Node* head, int n)	////n为需要建立多长的链表长度
{
	int i;
	Node* p;								//p为标志，标志链表最后一个结点
	p = head;								
	for (i = 0; i < n; i++)
	{
		Node* node = (Node*)malloc(LEN);
		printf("请输入一个数： \n");
		scanf("%d", &node->data);			
		node->next = p->next;				//将新结点的指针域置为空
		p->next = node;						//将新结点的地址存入最后一个结点的指针域
		p = node;							//标志后移
	}
	printf("链表创建成功！\n");
	return head;
}
```





## 单链表的简单操作

单链表的简单操作有：**插入结点、删除结点、修改结点数据、输出链表、释放链表、判断链表长度**。

### 插入结点：

```c
//单链表指定位置插入新结点
Node* insert_node(Node* head,int n,int m)	//n为指定结点，在其后插入新结点；m为新结点的数据
{
	Node* p = head;
	int i=0;
	while (i != n )
	{
		p = p->next;						//找到指定结点的地址
		i++;
	}
	Node* q = (Node*)malloc(LEN);			//创建新结点
	q->data = m;
	q->next = p->next;						//将指定结点的下一结点地址存入新结点的指针域
	p->next = q;							//将新结点的地址存入指定结点的指针域 完成插入
	printf("插入成功！\n");
	return head;
}
```



### 删除结点：

```c
//单链表指定结点的删除
void delete_node(Node* head, int n)			//n为指定结点
{
	Node* p = head->next;
	int i = 1;
	while (i != n - 1)
	{
		p = p->next;						//找到指定结点的上一结点地址
		i++;
	}
	Node* q = p->next;						//记住删除结点的地址，以便删除后释放该结点
	p->next = q->next;						
	free(q);								//释放所删除的结点
	printf("删除成功！\n");
}
```

删除结点后注意要将该节点**释放**。



### 修改结点数据：

```c
//单链表修改指定结点的数据
void change_node(Node* head, int n,int m)	//n为修改结点位置 m为修改后的数据
{
	Node* p = head;
	int i = 0;
	while (i != n)
	{
		p = p->next;						//找到指定结点的地址
		i++;
	}
	p->data = m;							//修改
	printf("修改成功！\n");
}
```



### 输出链表：

```c
//输出单链表
void printLink(Node* head)
{
	Node* p = head->next;				//头结点不储存数据
	while (p)
	{
		printf("%d ", p->data);
		p = p->next;					//指针后移
	}
	printf("\n");
}
```



### 释放链表：

```c
/单链表的释放
void releaseLink(Node* head)
{
	Node* p;
	while (head)
	{
		p = head->next;					//记住下一结点的地址
		free(head);
		head = p;
	}
	printf("链表释放成功！\n");
}
```

利用辅助变量记住下一结点地址，不会因释放而丢失地址。



### 判断链表长度：

```c
//判断链表的长度
void lenLink(Node* head)
{
	int len = 0;
	Node* p = head->next;			//头结点不储存数据
	while (p)
	{
		len += 1;
		p = p->next;				
	}
	printf("链表长度为：%d\n",len);
}
```



## 单链表的复杂操作

单链表的复杂操作有：**链表的反转、链表的奇偶调换（两两交换）、找到链表中点、检测链表是否成环**。其实这些操作也不会复杂，只要明白了原理，将图画出来，就能够很好的理解。

### 链表的反转：

链表的反转其实不难，从头插法建立链表的特点**“后来居上”**可以得到启发。可以将原链表的头结点打断，再将<u>后面的结点按链表顺序一个一个用头插法插入到头结点之后形成新表</u>。其间用辅助变量记下插入结点的下一结点地址，防止丢失链表。

H->A->B->C->D

H->	A->B->C->D			<!--打断头节点-->

H->A	B->C->D 				<!--用辅助变量记住B结点地址，将A插入-->

H->B->A	C->D				<!--用辅助变量记住C结点地址，将B插入-->	

H->C->B->A 	D

H->D->C->B->A				<!--链表就反转成功了-->			

代码如下：

```c
//单链表的反转(头插法思想）
Node* inverseLink1(Node* head)
{
	Node* p = head->next, * q;			
	head->next = NULL;					//将头结点打破
	while (p != NULL)
	{
		q = p->next;					//记下下一结点的地址	防止丢失链表
		p->next = head->next;		//头插法思想将结点一个个插入，由此第一个结点就到了最后一个结点
		head->next = p;
		p = q;							//结点往前
	}
	printf("链表反转成功！\n");
	return head;

}
```



### 链表的奇偶调换：

链表的奇偶调换的意思就是原链表为：H->1->2->3->4 	调换后为：H->2->1->4->3

H->1->2->3->4 

H->1->3->4 	2		<!--记住2，让13相连-->

H->2->1->3->4 

```c
//单链表奇偶调换(两两交换)
Node* changeLink_Two(Node* head)
{
	if (!head || !head->next)	return head;	
	Node* p1 = head;							//p1总是指向要交换结点的前一个结点
	while (p1->next && p1->next->next)			
	{
		Node* p2 = p1->next->next;				//p2总是指向p1的下两个结点
		p1->next->next = p2->next;	//若用1->2->3->4 则是让1、3相连，再2、1相连，最后后移p1
		p2->next = p1->next;
		p1->next = p2;
		p1 = p2->next;
	}
	printf("链表两两调换成功！\n"); 
	return head;

} 
```



### 找到链表中点：

用快慢指针法，<u>快指针每次走两个结点，慢指针每次都一个结点</u>，当快指针走完链表时慢指针刚好到达中点。

代码如下：

```c
//找到单链表中点(快慢指针法）
Node* middle_Node(Node* head)
{
	Node* fast = head;
	Node* low = head;
	while (fast != NULL && fast->next != NULL)
	{
		fast = fast->next->next;	//快指针走两步慢指针走一步 快指针走到表尾时慢指针刚好在中点
		low = low->next;
	}
	printf("中间链表的数据为：%d\n",low->data);
	return low;

}
```



### 检测链表是否成环：

利用快慢指针，<u>快指针每次走两个结点，慢指针每次都一个结点</u>。如果链表成环，快慢指针必会**相遇**。不成环则快指针会遇到**空**。代码如下：

```c
//检测链表是否成环(快慢指针法)
void judgeCycle(Node* head)
{
	Node* fast = head;
	Node* low = head;
	while (fast != NULL && fast->next != NULL)
	{
		low = low->next;
		fast = fast->next->next;
		if (low == fast)				//如果成环则快慢指针会相遇，不成环则快指针会遇到表尾
		{
			printf("链表成环！\n");					
		}
	}
	if (fast == NULL && fast->next == NULL)
	printf("链表不成环！\n"); 
}
```



## 双链表的创建

双链表的每个结点有两个指针，分别指向直接后继和直接前驱（上一结点和下一结点）

首先定义结构体，接着创建双链表。

### 定义结构体：

```c
//双链表结点
typedef struct DNODE {
	int data;				//数据域
	struct Dnode* next;		//下一结点指针域
	struct Dnode* last;		//上一结点指针域
}Dnode;
```

用<u>typedef简化</u>可以方便很多。

定义结构体后可以用<u>预处理命令将结构体的长度简化表示</u>：

`#define DLEN sizeof(struct DNODE)`



### 尾插法创建双链表：

既要使表尾结点指向新结点，又要让新结点指向表尾结点。代码如下：

```c
//双向链表的建立(尾插法)
Dnode* createDlink(int n)
{
	Dnode* head, * q, * p;
	int i;
	head = (Dnode*)malloc(DLEN);
	head->last = NULL;
	head->next = NULL;
	q = head;
	for (i = 0; i < n; i++)
	{
		p = (Dnode*)malloc(DLEN);
		printf("请输入数据： \n");
		scanf("%d", &p->data);
		q->next = p;					//表尾指向新结点
		p->last = q;					//新结点指向表尾
		q = p;							//结点后移
	}
	q->next = NULL;
	printf("链表创建成功！\n");
	return head;
}
```



暂时先写这么多操作，还有一些操作后续再补充

谢谢大家！



