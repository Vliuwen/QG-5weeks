# c语言链表的操作完整代码：



```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#define LEN sizeof(struct NODE)
#define DLEN sizeof(struct DNODE)

//单链表结点
typedef struct NODE {
	int data;				//数据域 
	struct NODE* next;	    //指针域，指向下一个结点 
}Node;				//Node结构体别名

//双链表结点
typedef struct DNODE {
	int data;				//数据域
	struct Dnode* next;		//下一结点指针域
	struct Dnode* last;		//上一结点指针域
}Dnode;

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

//判断链表的长度
void lenLink(Node* head)
{
	int len = 0;
	Node* p = head->next;
	while (p)
	{
		len += 1;
		p = p->next;
	}
	printf("链表长度为：%d\n",len);
}
 
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

//输出单链表
void printLink(Node* head)
{
	Node* p = head->next;
	while (p)
	{
		printf("%d ", p->data);
		p = p->next;
	}
	printf("\n");
}

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

//单链表修改指定结点的数据
void change_node(Node* head, int n,int m)
{
	Node* p = head;
	int i = 0;
	while (i != n)
	{
		p = p->next;						//找到指定结点的地址
		i++;
	}
	p->data = m;
	printf("修改成功！\n");
}

//单链表的释放
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

//单链表的反转(头插法思想）
Node* inverseLink1(Node* head)
{
	Node* p = head->next, * q;			
	head->next = NULL;					//将头结点打破
	while (p != NULL)
	{
		q = p->next;					//记下下一结点的地址
		p->next = head->next;			//用头插法思想将结点一个个插入，由此第一个结点就到了最后一个结点
		head->next = p;
		p = q;							//结点往前
	}
	printf("链表反转成功！\n");
	return head;

}

//单链表的反转(递归思想)(出现一些意外，思路对了，但是无法处理表头) 
/*现在需要把A->B->C->D进行反转，可以先假设B->C->D已经反转好，已经成为了D->C->B。
那么接下来要做的事情就是将D->C->B看成一个整体，让这个整体的next指向A，所以问题转化了反转B->C->D。
那么，可以先假设C->D已经反转好，已经成为了D->C。
那么接下来要做的事情就是将D->C看成一个整体，让这个整体的next指向B。所以问题转化了反转C->D。
那么，可以先假设D(其实是D->NULL)已经反转好，已经成为了D(其实是head->D)
那么接下来要做的事情就是将D(其实是head->D)看成一个整体，让这个整体的next指向C，所以问题转化了反转D。
有点类似于尾插法。
*/
/*Node* inverseLink2(Node *head)					//头结点是不储存数据的，因此处理时需注意
{
	if (head == NULL || head->next == NULL)		//递归返回条件
		return head;
	Node* p = inverseLink2(head->next);			
	head->next->next = head;					//将当前传入结点置为表尾
	head->next = NULL;
	return p;									//返回新链表表头
}
*/

//单链表奇偶调换(两两交换)
Node* changeLink_Two(Node* head)
{
	if (!head || !head->next)	return head;	
	Node* p1 = head;							//p1总是指向要交换结点的前一个结点
	while (p1->next && p1->next->next)			
	{
		Node* p2 = p1->next->next;				//p2指向p1的下两个结点
		p1->next->next = p2->next;				//若用1->2->3->4 则是让1、3相连，再2、1相连，最后后移p1
		p2->next = p1->next;
		p1->next = p2;
		p1 = p2->next;
	}
	printf("链表两两调换成功！\n"); 
	return head;

} 

//找到单链表中点(快慢指针法）
Node* middle_Node(Node* head)
{
	Node* fast = head;
	Node* low = head;
	while (fast != NULL && fast->next != NULL)
	{
		fast = fast->next->next;					//快指针走两步慢指针走一步 快指针走到表尾时慢指针刚好在中点
		low = low->next;
	}
	printf("中间链表的数据为：%d\n",low->data);
	return low;

}

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

int main()
{	
	int a, num, len, data;
	int run = 1;
	Node* head=NULL;
	Node* p = NULL;
	Dnode* Dhead = NULL;
	printf("请选择需要的操作：\n");
	printf("	1.初始化一个单链表表头\n");
	printf("	2.用头插法创建一个链表\n");
	printf("	3.判断链表长度\n");
	printf("	4.单链表指定位置插入新结点\n");
	printf("	5.输出单链表\n");
	printf("	6.单链表指定结点删除\n");
	printf("	7.单链表修改指定结点数据\n");
	printf("	8.单链表的释放\n");
	printf("	9.链表的反转\n");
	printf("	10.递归法链表的反转\n");
	printf("	11.单链表的奇偶调换\n");
	printf("	12.找到单链表的中点\n");
	printf("	13.检测链表是否成环\n");
	printf("	14.双向链表的建立\n");
	printf("	15.用尾插法创建一个链表\n"); 
	printf("	0.退出\n");
	while (run)
	{
		scanf("%d", &a);
		switch (a)
		{
		case 1:
			head = init_node();
			break;
		case 2:
			printf("请输入需要建立的链表长度：\n");
			scanf("%d", &len);
			head = createLink_head(head, len);
			break;
		case 3:
			lenLink(head);
			break;
		case 4:
			printf("请输入结点位置及数据： \n");
			scanf("%d %d", &num, &data);
			head=insert_node(head, num, data);
			break;
		case 5:
			printLink(head);
			break;
		case 6:
			printf("请输入需要删除的结点的位置：\n");
			scanf("%d", &num);
			delete_node(head, num);
			break;
		case 7:
			printf("请输入需要修改的结点位置和数据： \n");
			scanf("%d %d", &num, &data);
			change_node(head, num, data);
			break;
		case 8:
			releaseLink(head);
			break;
		case 9:
			head = inverseLink1(head);
			break;
		/*case 10:
			head=head->next;
			head = inverseLink2(head);
			break;*/
		case 11:
			head = changeLink_Two(head);
			break;
		case 12:
			p = middle_Node(head);
			break;
		case 13:
			judgeCycle(head);
			break;
		case 14:
			printf("请输入需要建立链表的长度：\n");
			scanf("%d", len);
			Dhead = createDlink(len);
			break;
		case 15:
			printf("请输入需要建立的链表长度：\n");
			scanf("%d",&len);
			createLink_tail(head, len);
			break;
		case 0:
			run = 0;
			break;
		default:
			printf("输入错误！\n"); 
		}
			
	}
	return 0; 
	
}
```

