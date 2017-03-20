# library
用户功能


#include"library.h"

Book *B_name(Book *Bhead,char *Name)//按书名查找
{
	Book *p;
	p = Bhead;
    while(p&&(strcmp(p->name,Name)!=0))
		p=p->next;
    return p;
} 
//zhai

int B_author(Book *Bhead,char *Author,Book *s[20])//按作者查找
{
    int i=0;
	Book *p;
	p = Bhead;
	while (1)
	{
		while (p&&(strcmp(p->author,Author)!=0))
			p = p->next;
		if (p != NULL)
		{
			s[i] = p;
			i++;
			p = p->next;
		}
		else
			break;
	}
	return i;
}						
//zhai

Book *B_number(Book *Bhead,long num,keynode *Khead)//按书编号用检索表查询
{
	Book *p;
	keynode *k;
	k = Khead;
	p= Bhead;
	while (k->next!=NULL)
	{
		if(k->next->adress<=num)
			k = k->next;
		else  break;
	}
	if (k == NULL)     return NULL;
	else
	{
		p = k->adress;
		while (p&&p->number != num)
		{
			p = p->next;
		}
		return p;//返回指向该书籍的指针
	}
}		
//zhai

void SEEK(Book *Bhead,Reader *Rhead,Reader *reader,keynode *Khead)//查找书籍操作（借阅图书查找方式选择）
{
	int i,N;
	long number1;
	char Name[20], Author[20]; 
	Book *p, *s[20];
	p = NULL;
	for (i = 0;i < 20; i++)
		s[i] = NULL;
	int N1;
	while(1)
	{    
	printf("****************************************************************************\n");
	printf("*                               查询方式                                   *\n");
	printf("*                                                                          *\n");
	printf("*                        1、按书籍名称查找                                 *\n");
	printf("*                        2、按作者名字查找                                 *\n");
	printf("*                        3、按书籍编号查找                                 *\n");
	printf("*                        4、返回上一步                                     *\n");
	printf("*                                                                          *\n");
	printf("****************************************************************************\n");
	printf("请输入操作编号：");
	scanf("%d",&N1);
	switch (N1)//选择方式（考虑重复部分构造一个子函数）
	{
		case 1:
			printf("请输入书籍名称：");
			scanf("%s", Name);
			p = B_name(Bhead, Name);
			if (p != NULL)
			{
				printf("书籍信息：");
				printf("\n┌────────┬────────┬────────┬────────┬───────┐");
				printf("\n│书籍编号        │书籍名称        │作者名字        │ 现存量         │ 总量         │");
				printf("\n├────────┼────────┼────────┼────────┼───────┤");
				printf("\n│%11d     │%12s    │%8s        │%10d      │%10d    │", p->number, p->name, p->author, p->xiancun, p->zongku);
				printf("\n└────────┴────────┴────────┴────────┴───────┘\n");
				PD(Bhead, p, Rhead, reader,Khead);
			}
			else
			{
				printf("未找到该书籍请重新选择。\n");
				SEEK(Bhead,Rhead, reader,Khead);
			}
			break;
		case 2:
			printf("请输入作者名字：");
			scanf("%s", Author);
			N = B_author(Bhead, Author, s);
			if (s[0] != NULL)
			{
				printf("书籍信息：");
				printf("\n┌────────┬────────┬────────┬────────┬───────┐");
				printf("\n│书籍编号        │书籍名称        │作者名字        │ 现存量         │ 总量         │");
				printf("\n├────────┼────────┼────────┼────────┼───────┤\n");
				for (i = 0;i < N;i++)
				{
					printf("│%11d     │%12s    │%8s        │%10d      │%10d    │\n", s[i]->number, s[i]->name, s[i]->author, s[i]->xiancun, s[i]->zongku);
				}
				printf("└────────┴────────┴────────┴────────┴───────┘\n");
				printf("\n");
				printf("请输入书籍编号，查询详细信息(输入0返回上一层）：");
				scanf("%d", &number1);
				if (number1 != 0)
				{
					p = B_number(Bhead, number1, Khead);
					PD(Bhead, p, Rhead, reader, Khead);
				}
				else
				{
					system("cls");
					B_reader(Bhead, Rhead, reader, Khead);
				}
			}
			else
			{
				printf("未找到该书籍请重新选择。\n");
				SEEK(Bhead, Rhead, reader, Khead);
			}
			break;
		case 3:
			printf("请输入书籍编号：");
			scanf("%d", &number1);
			p = B_number(Bhead, number1,Khead);
			if (p != NULL)
			{
				printf("书籍信息：");
				printf("\n┌────────┬────────┬────────┬────────┬───────┐");
				printf("\n│书籍编号        │书籍名称        │作者名字        │ 现存量         │ 总量         │");
				printf("\n├────────┼────────┼────────┼────────┼───────┤");
				printf("\n│%11d     │%12s    │%8s        │%10d      │%10d    │", p->number, p->name, p->author, p->xiancun, p->zongku);
				printf("\n└────────┴────────┴────────┴────────┴───────┘\n");
				PD(Bhead, p, Rhead, reader, Khead);
			}
			else
			{
				printf("未找到该书籍请重新选择。\n");
				SEEK(Bhead, Rhead, reader, Khead);
			}
			break;
		case 4://返回上一步
			system("cls");
			B_reader(Bhead, Rhead,reader,Khead);
		default:
			printf("输入错误，请按提示重新输入。");
	}
	}
}   
//li

void B_reader(Book *Bhead,Reader *Rhead,Reader *reader,keynode *Khead)			//显示单个读者信息
{
	int i;
	int N5;
	Reader *p;
	p = reader;
    printf("用户信息：");
    printf("\n┌────────┬────────┬────────┬───────────┬────────────────┐");
    printf("\n│学生编号        │学生姓名        │ 性别           │持有书籍量            │ 已借阅书籍编号                 │");
    printf("\n├────────┼────────┼────────┼───────────┼────────────────┤");
    printf("\n│%11d     │%12s    │ %11s    │%14d        │    %20d        │\n",p->number,p->name,p->sex,p->booknum,p-> jiebook[0]);
    for (i=1;i<p->booknum;i++)
    {
		printf("│                │                │                │                      │      %18d        │\n",p->jiebook[i]);
    }
	printf("└────────┴────────┴────────┴───────────┴────────────────┘\n");
	printf("\n");
	printf("****************************************************************************************************\n");
	printf("*                                                                                                  *\n");
	printf("*                 1、借阅书籍                2、归还书籍                3、退出                    *\n");
	printf("*                                                                                                  *\n");
	printf("****************************************************************************************************\n");
	printf("请输入操作编号：");
	scanf("%d", &N5);
	switch(N5)
	{
	case 1:
		system("cls");
		SEEK(Bhead,Rhead, reader,Khead);//跳转到书籍查询界面
		break;
	case 2://（添加修改书籍信息函数）
		revert(Bhead, Rhead, reader,Khead);														//归还书籍函数
		break;
	case 3://跳转到初始界面
		system("cls");
		firstmain(Bhead,Rhead,Khead);
		break;
	}
}  
//bai

void PD(Book *Bhead,Book *p,Reader *Rhead,Reader *reader,keynode *Khead)//判断是否借阅？
{
	int N4;
	while (1)
	{
		printf("\n\n\n");
		printf("****************************************************************************************************\n");
		printf("*                                                                                                  *\n");
		printf("*—————————————————————是否借阅？———————————————————————*\n");
		printf("*                                                                                                  *\n");
		printf("*                    1、是                                           2、否                         *\n");
		printf("*                                                                                                  *\n");
		printf("****************************************************************************************************\n");
		printf("请输入操作编号：");
		scanf("%d", &N4);
		getchar();
		switch (N4)
		{
		case 1://（添加修改书籍信息函数）
			p->xiancun = p->xiancun - 1;
			reader->jiebook[reader->booknum] = p->number;
			reader->booknum = reader->booknum + 1;
			system("cls");
			printf("借阅成功，欢迎再次借阅(按任意键返回读者信息）");
			getchar();
			B_reader(Bhead,Rhead, reader,Khead);
			break;
		case 2://添加返回主菜单（添加主菜单函数）
			printf("按任意键返回读者信息。");
			getchar();
			system("cls");
			B_reader(Bhead,Rhead, reader,Khead);
			break;
		default:
			printf("请按提示重新输入。");
		}
	}
}	
//li

void login(Book *Bhead, Reader *Rhead,keynode *Khead)//用户登录界面
{
	Reader *p,*reader;
	reader = p = NULL;
	int i = 0;
	long number1;
	char pass[20],ch;
	printf("**************************************************************************\n");
	printf("**                账号：");
	scanf("%d", &number1);
	printf("**                密码：");
	while (1)
	{
		ch = getch();
		if (ch == '\r')  break;
		else
		{
			pass[i] = ch;
			i++;
			printf("*");
		}
	}
	pass[i] = '\0';
	p = searchreader(Rhead, number1);//按账号进行查找
	if (p == NULL)
	{
		printf("对不起，该用户不存在，请注册后登录");
		REGISTER(Bhead,Rhead,reader,Khead);//注册界面函数
	}
	else
	{
		if (strcmp(p->password, pass) != 0)
		{
			while (strcmp(p->password, pass) != 0)
			{
				printf("对不起，密码错误，请重新输入:");
				i = 0;
				while (1)
				{
					ch = getch();
					if (ch == '\r')  break;
					else
					{
						pass[i] = ch;
						i++;
						printf("*");
					}
				}
				pass[i] = '\0';
			}
			system("cls");
			B_reader(Bhead,Rhead, p,Khead);
		}
		else
		{
			system("cls");
			B_reader(Bhead,Rhead, p,Khead);
		}
	}
}			
//bai

void REGISTER(Book *Bhead, Reader *Rhead,Reader *reader,keynode *Khead)
{
	Reader *p,*q,*j;
	int i;
	long number1;
	char name1[20], sex1[20], password1[20];
	char sexw[] = "女";
	char sexm[] = "男";
	p = q = j = NULL;
	p = Rhead;
	printf("\n\n\n");
	printf("\t\t\t****************************************************************\n");
	printf("\t\t\t**请输入注册账号：");
	scanf("%d", &number1);
	printf("\t\t\t**请输入姓名：");
	scanf("%s", name1);
	getchar();
	while (1)
	{
		printf("\t\t\t**请输入性别：");
		scanf("%s", sex1);
		getchar();
		if ((!strcmp(sex1, sexm)) || (!strcmp(sex1, sexw)))     break;//字符相同时返回1，否则为0
		else
		{
			printf("\t\t\t**阁下非男非女吗？请重新认真输入。\n");
		}
	}
	printf("\t\t\t**请设置密码：");
	scanf("%s", password1);
	//getchar();
	if (Rhead == NULL)
	{
		Rhead = (Reader *)malloc(sizeof(Reader));
		Rhead->number = number1;
		strcpy(Rhead->name, name1);
		strcpy(Rhead->sex, sex1);
		strcpy(Rhead->password, password1);
		Rhead->booknum = 0;
		for (i = 0;i < 10;i++)
			Rhead->jiebook[i] = 0;
		Rhead->next = NULL;
		reader = Rhead;
		printf("注册成功，按任意键跳入读者信息。");
		getchar();
		B_reader(Bhead, Rhead,reader,Khead);
	}
	else
	{
		while (p&&p->number != number1)//按读者账号查找
		{
			j = p;
			p = p->next;
		}
		if (p == NULL)
		{
			q = (Reader*)malloc(sizeof(Reader));
			q->number = number1;
			strcpy(q->name, name1);
			strcpy(q->sex, sex1);
			strcpy(q->password, password1);
			q->booknum = 0;
			for (i = 0;i < 10;i++)
				q->jiebook[i] = 0;
			q->next = NULL;
			j->next = q;
			printf("注册成功，按任意键跳入读者信息。");
			getchar();
			B_reader(Bhead,Rhead, q,Khead);        //注册成功后的读者的信息
		}
		else
		{
			printf("该账号已被注册，请重新输入。");
			firstmain(Bhead,Rhead,Khead);
		}
	}
}		
//bai

void revert(Book *Bhead,Reader *Rhead, Reader *reader,keynode *Khead)
{
	Reader *Rp;
	Book *Bp;
	int i;
	long temp;
	Rp = reader;
	Bp = Bhead;
	long number1;
	if (Rp->booknum == 0)
	{
		system("cls");
		printf("您未借阅图书，不能归还。（返回读者信息）");
		B_reader(Bhead,Rhead, reader,Khead);
	}
	while (1)
	{
		printf("\n\n\n请输入归还书籍书号：");
		scanf("%d", &number1);
		for (i = 0;i < Rp->booknum; i++)
		{
			if (Rp->jiebook[i] == number1)
				break;
		}
		if (i == Rp->booknum)    //如果i指向读者当前借书量，说明已经查找完了，且没有找到
			printf("输入的书号错误，请重新输入。");
		else
			break;
	}
	getchar();
	while (Bp&&Bp->number != Rp->jiebook[i])      //没找到且还没找完
	{
		Bp = Bp->next;       //继续查找
	}
	if (i == Rp->booknum - 1)      //找到所要归还的书籍，如果是所借书的最后一本就直接把他的元素置0
		Rp->jiebook[i] = 0;
	else
	{
		Rp->jiebook[i] = 0;             //如果所还的书籍的位置不是最后一本，则把他的位置置0之后，
		for (;i < Rp->booknum - 1;i++)    //和他后面的书籍的位置调换，直至最后的元素为0
		{
			temp = Rp->jiebook[i];
			Rp->jiebook[i] = Rp->jiebook[i + 1];
			Rp->jiebook[i + 1] = temp;
		}
	}
	Rp->booknum = Rp->booknum - 1;
	Bp->xiancun = Bp->xiancun + 1;
	system("cls");
	printf("还书成功（按任意键返回用户界面）");
	getchar();
	B_reader(Bhead, Rhead, reader,Khead);
}			
//li 


