# library
管理员功能


#include"library.h"

Book *zairubook()
{
	FILE *fp;
	Book *p,*q,*Bhead;
	Bhead = p = q = NULL;
	long number1,xiancun1,zongku1;
	char name1[20], author1[20];
	fp = fopen("Book.txt", "r");
	if (fp == NULL)
		printf("没有打开Book.txt.");
	else
	{
		if (fscanf(fp, "%d%s%s%d%d", &number1, name1, author1, &xiancun1, &zongku1) != EOF)
		{
			//fscanf(fp, "%d%s%s%d%d", &number1, name1, author1, &xiancun1, &zongku1);
			q = (Book*)malloc(sizeof(Book));
			q->number = number1;
			strcpy(q->name, name1);
			strcpy(q->author, author1);
			q->xiancun = xiancun1;
			q->zongku = zongku1;
			q->next = NULL;
			Bhead = q;
			while (1)
			{
				if (fscanf(fp, "%d%s%s%d%d", &number1, name1, author1, &xiancun1, &zongku1) == EOF)   break;
				p = (Book*)malloc(sizeof(Book));
				p->number = number1;
				strcpy(p->name, name1);
				strcpy(p->author, author1);
				p->xiancun = xiancun1;
				p->zongku = zongku1;
				p->next = NULL;
				q->next = p;
				q = p;
			}
			fclose(fp);
			return Bhead;
		}
		else
		{
			fclose(fp);
			return NULL;
		}
	}
}        //          从文件载入书籍信息
//chang

Reader *zairureader()
{
	FILE *fp;
	Reader *p, *q, *Rhead;
	Rhead = p = q = NULL;
	int booknum1,i;
	long number1, jiebook1[10];
	char name1[20], sex1[20],password1[20];
	fp = fopen("Reader.txt", "r");
	if (fp == NULL)
		printf("没有打开Reader.txt.");
	else
	{
		if (fscanf(fp, "%d%s%s%s%d", &number1, name1, sex1, password1, &booknum1)!=EOF)
		{
			q = (Reader *)malloc(sizeof(Reader));
			//fscanf(fp, "%d%s%s%s%d", &number1, name1, sex1, password1, booknum1);
			for (i = 0;i < 10;i++)
				fscanf(fp, "%d", &jiebook1[i]);
			q->number = number1;
			strcpy(q->name, name1);
			strcpy(q->sex, sex1);
			strcpy(q->password, password1);
			q->booknum = booknum1;
			for (i = 0;i < 10;i++)
				q->jiebook[i] = jiebook1[i];
			q->next = NULL;
			Rhead = q;
			while (1)
			{
				if (fscanf(fp, "%d%s%s%s%d", &number1, name1, sex1, password1, &booknum1) == EOF)   break;
				for (i = 0;i < 10;i++)
					fscanf(fp, "%d", &jiebook1[i]);
				p = (Reader*)malloc(sizeof(Reader));
				p->number = number1;
				strcpy(p->name, name1);
				strcpy(p->sex, sex1);
				strcpy(p->password, password1);
				p->booknum = booknum1;
				for (i = 0;i < 10;i++)
					p->jiebook[i] = jiebook1[i];
				p->next = NULL;
				q->next = p;
				q = p;
			}
			fclose(fp);
			return Rhead;
		}
		else
		{
			fclose(fp);
			return Rhead;
		}
	}
}        //从文件载入读者信息
//chang

void Administer(Book *B, Reader *R,keynode *Khead)
{
	int x,j=0,y;
	char password[5];
	printf("输入管理员密码（admin)|错误三次退出|\n密码：");
	while (1)
	{
		for (x = 0;x < 5;x++)
			scanf("%c", &password[x]);
		if (password[0] != 'a' || password[1] != 'd' || password[2] != 'm' || password[3] != 'i' || password[4] != 'n')
		{
			printf("输入错误,请重新输入：");
			j++;
			if (j == 3)
				exit(0);
			getchar();
		}
		else break;
	}
	system("cls");
	while (1)
	{
		printf("\n\n\n\n\n");
		printf("************************************************************************************************\n");
		printf("***                                   请选择管理项目                                         ***\n");
		printf("***                                   1.新书籍的入库。                                       ***\n");
		printf("***                                   2.现存书籍的消除。                                     ***\n");
		printf("***                                   3.显示库存所有书籍。                                   ***\n");
		printf("***                                   4.显示所有用户信息。                                   ***\n");
		printf("***                                   5.返回上一界面。                                       ***\n");
		printf("************************************************************************************************\n");
		printf("\n——选择：");
		scanf("%d", &y);
		getchar();
		switch (y)
		{
		case 1:
			system("cls");
			incertbook(B,R,Khead);break;
		case 2:
			system("cls");
			deletebook(B,R,Khead);break;
		case 3:
			system("cls");
			showbook(B,R,Khead);break;
		case 4:
			system("cls");
			showreader(B,R,Khead);break;
		case 5:
			system("cls");
			firstmain(B,R,Khead);
		default:
			printf("输入错误，请按提示重新输入：");
		}		
	}
}     //管理员登录界面
//chang

void incertbook(Book *Bhead,Reader *Rhead,keynode *Khead)
{
	Book *p,*q,*j;
	long number1, num;
	char a,name1[20], author1[20];
	p = q = NULL;
	p = Bhead;
	printf("输入添加书籍书号:");
	scanf("%d", &number1);
	printf("输入添加书籍书名:");
	scanf("%s", name1);
	printf("输入书籍著者:");
	scanf("%s", author1);
	printf("输入添加书籍数量:");
	scanf("%d", &num);				//继续完成的东西是通过书号遍历一遍书籍，寻找是否有该书，若有该书，直接增加现存量和总库存
	if (Bhead == NULL)
	{
		Bhead = (Book*)malloc(sizeof(Book));
		Bhead->number = number1;
		strcpy(Bhead->name, name1);
		strcpy(Bhead->author, author1);
		Bhead->xiancun = num;
		Bhead->zongku = num;
		Bhead->next = NULL;
	}
	else
	{
		while (p&&p->number < number1)
		{
			q = p;
			p = p->next;
		}
		if (p == NULL)
		{
			j = (Book*)malloc(sizeof(Book));
			j->number = number1;
			strcpy(j->name, name1);
			strcpy(j->author, author1);
			j->xiancun = num;
			j->zongku = num;
			j->next = NULL;
			q->next = j;
		}
		else
		{
			if (p->number == number1)
			{
				p->xiancun = p->xiancun + num;
				p->zongku = p->zongku + num;
			}
			else
			{
				j = (Book*)malloc(sizeof(Book));
				j->number = number1;
				strcpy(j->name, name1);
				strcpy(j->author, author1);
				j->xiancun = num;
				j->zongku = num;
				j->next = p;
				q->next = j;
			}
		}
	}
	getchar();                  //不能去，去掉跳转界面不知道就去哪了
	printf("书籍入库成功。是否继续进行书籍入库？Y/N:");
	scanf("%c", &a);
	getchar();
	if (a == 'Y')
	{
		system("cls");
		incertbook(Bhead, Rhead,Khead);
	}
	else
	{
		Khead = initindex(Bhead);
		printf("按任意键返回上层。");
		getchar();
		Administer(Bhead, Rhead,Khead);
	}
}         //插入一本书
//chang

void deletebook(Book *Bhead,Reader *Rhead,keynode *Khead)
{
	Book *p,*q;
	long number1;
	char a;
	p = Bhead;
	printf("请输入要删除书籍书号：");
	scanf("%d", &number1);
	if (p == NULL)
	{
		printf("书库中无书籍，无法删除，按任意键返回上一层");
		getchar();
		Administer(Bhead, Rhead,Khead);
	}
	else
	{
		while (p&&p->number != number1)
		{
			q = p;
			p = p->next;
		}
		if (p == NULL)
		{
			printf("书库中无该书，无法删除，按任意键返回上一层。");
			getchar();
			Administer(Bhead, Rhead,Khead);
		}
		else
		{
			q->next = p->next;
			printf("删除书籍成功。\n");
			printf("是否继续删除？Y/N:");
			scanf("%c", &a);
			getchar();
			if (a == 'Y')
			{
				system("cls");
				deletebook(Bhead, Rhead,Khead);
			}
			else
			{
				Khead = initindex(Bhead);
				printf("按任意键返回上层。");
				getchar();
				Administer(Bhead, Rhead,Khead);
			}
		}
	}

}                //删除书籍
//chang

void showbook(Book *Bhead,Reader *Rhead,keynode *Khead)
{
	Book *p;
	char a,name1[20];
	p = Bhead;
	if (p == NULL)
		printf("没有存放书籍。");
	else
	{
		printf("\n┌────────┬────────┬────────┬────────┬───────┐");
		printf("\n│书籍编号        │书籍名称        │著者名字        │ 现存量         │ 总量         │");
		printf("\n├────────┼────────┼────────┼────────┼───────┤\n");
		while (p != NULL)
		{
		printf("│%8d        │%12s    │%10s      │%10d      │%10d    │\n", p->number,p->name,p->author,p->xiancun,p->zongku);
		p = p->next;
		}
		printf("└────────┴────────┴────────┴────────┴───────┘\n");
		printf("是否按书名查询单本书详细信息？Y/N\n");
		scanf("%c", &a);
		getchar();
		if (a == 'Y')
		{
			printf("输入书名:");
			scanf("%s", name1);
			p = B_name(Bhead, name1);				//按书名进行查找
			printf("\n┌────────┬────────┬────────┬────────┬───────┐");
			printf("\n│书籍编号        │书籍名称        │著者名字        │ 现存量         │ 总量         │");
			printf("\n├────────┼────────┼────────┼────────┼───────┤\n");
			printf("│%8d        │%12s    │%10s      │%10d      │%10d    │\n", p->number, p->name, p->author, p->xiancun, p->zongku);
			printf("└────────┴────────┴────────┴────────┴───────┘\n");
		}
		else
		{
			printf("按任意键返回上一层。");
			getchar();
			system("cls");
			Administer(Bhead, Rhead,Khead);
		}
	}
}      //显示所有书籍
//bai

void showreader(Book *Bhead,Reader *Rhead,keynode *Khead)
{
	Reader *p;//          
	int i;
	char a;
	long number1;
	p = Rhead;
	if (p == NULL)
		printf("没有读者信息。");
	else
	{
		printf("\n                    ┌────────┬────────┬────────┬────────┐");
		printf("\n                    │账号            │姓名            │性别            │ 已借书量       │ ");
		printf("\n                    ├────────┼────────┼────────┼────────┤\n");
		while (p != NULL)
		{
		printf("                    │%8d      │%9s       │%9s       │%9d       │\n", p->number, p->name, p->sex, p->booknum);
		p = p->next;
		}
		printf("                    └────────┴────────┴────────┴────────┘\n");
		printf("\n是否查询单个读者详细信息？Y/N\n");
		scanf("%c", &a);
		getchar();
		if (a == 'Y')
		{
			printf("输入读者账号：");
			scanf("%d", &number1);
			p = searchreader(Rhead, number1);				//按读者账号进行查找
			if (p == NULL)
			{
				printf("没有该读者信息。");
			}
			else
			{
				printf("用户信息：");
				printf("\n┌────────┬────────┬────────┬───────────┬────────────────┐");
				printf("\n│学生编号        │学生姓名        │ 性别           │持有书籍量            │ 已借阅书籍编号                 │");
				printf("\n├────────┼────────┼────────┼───────────┼────────────────┤");
				printf("\n│%11d     │%12s    │ %11s    │%14d        │    %20d        │\n", p->number, p->name, p->sex, p->booknum, p->jiebook[0]);
				for (i = 1;i<p->booknum;i++)
				{
					printf("│                │                │                │                      │      %18d        │\n", p->jiebook[i]);
				}
				printf("└────────┴────────┴────────┴───────────┴────────────────┘\n");
				printf("\n");
			}
		}
		else
		{
			printf("按任意键返回上一层。");
			getchar();
			system("cls");
			Administer(Bhead,Rhead,Khead);
		}
	}
}          //显示所有读者
//bai

void firstmain(Book *Bhead,Reader *Rhead,keynode *Khead)          //首界面
{
	Reader *reader;
	reader = NULL;
	int choose;
	while (1)
	{
		printf("\n\n\n\t\t\t****************************************************************************************************\n");
		printf("\t\t\t*                                                                                                  *\n");
		printf("\t\t\t*                                                                                                  *\n");
		printf("\t\t\t*                                                                                                  *\n");
		printf("\t\t\t*                                                                                                  *\n");
		printf("\t\t\t*                                                                                                  *\n");
		printf("\t\t\t*                                    欢迎登陆图书馆管理系统                                        *\n");
		printf("\t\t\t*                                                                                                  *\n");
		printf("\t\t\t*                                                                                                  *\n");
		printf("\t\t\t*                                                                                                  *\n");
		printf("\t\t\t*                                                                                                  *\n");
		printf("\t\t\t*                                                                                                  *\n");
		printf("\t\t\t****************************************************************************************************\n");
		printf("\t\t\t*                                                                                                  *\n");
		printf("\t\t\t*                1、管理员登录       2、用户登录         3、注册      4、退出                      *\n");
		printf("\t\t\t*                                                                                     制作人：常江 *\n");
		printf("\t\t\t****************************************************************************************************\n");
		printf("\t\t\t请输入操作编号:");
		system("color 1f");
		scanf("%d", &choose);
		getchar();
		switch (choose)
		{
			case 1:
				system("cls");
				Administer(Bhead, Rhead,Khead);
				break;
			case 2:
				system("cls");
				login(Bhead,Rhead,Khead);										//用户登录函数
				break;
			case 3:
				system("cls");
				REGISTER(Bhead,Rhead,reader,Khead);											//用户注册函数
				break;
			case 4:
				system("cls");
				baocun(Bhead, Rhead);
				exit(0);
			default:
				printf("请按要求输入");
		}
	}
}
//li

void baocun(Book *Bhead, Reader *Rhead)       //把内存中信息保存到文件中
{
	FILE *Bfp, *Rfp;
	Book *Bp;
	Reader *Rp;
	int i;
	Bp = Rp = NULL;
	Bfp=fopen("Book.txt", "w+");
	Bp = Bhead;
	Rp = Rhead;
	while (Bp)
	{
		fprintf(Bfp, "%18d  %14s  %14s  %18d  %18d\n", Bp->number, Bp->name, Bp->author, Bp->xiancun, Bp->zongku);
		Bp = Bp->next;
	}
	fclose(Bfp);
	Rfp = fopen("Reader.txt", "w+");
	while (Rp)
	{
		fprintf(Rfp, "%18d  %14s  %12s  %18s  %8d  ", Rp->number, Rp->name, Rp->sex, Rp->password, Rp->booknum);
		for (i = 0;i < 10;i++)
		{
			fprintf(Rfp, "%14d   ", Rp->jiebook[i]);
		}
		fprintf(Rfp, "\n");
		Rp = Rp->next;
	}
	
	fclose(Rfp);
}
//chang

Reader *searchreader(Reader *Rhead, long number1)       //寻找一个读者的指针
{
	Reader *p;
	p = Rhead;
	while (p&&p->number != number1)
	{
		p = p->next;
	}
	return p;
}
//zhai

keynode *initindex(Book *Bhead)				//初始化建立索引表
{
	int i;
	Book *j;
	j = Bhead;
	keynode *p, *q,*Khead;
	Khead = p = q = NULL;
	if (Bhead == NULL)
		printf("书籍为空无法初始化索引表。");
	else
	{
		Khead = (keynode*)malloc(sizeof(keynode));
		Khead->adress = j;
		Khead->next = NULL;
		p = Khead;
		while (1)
		{
			for (i = 0;i <=10 ;i++)
			{
				j = j->next;
				if (j == NULL)  break;
			}
			if (j != NULL)
			{
				q = (keynode*)malloc(sizeof(keynode));
				q->adress = j;
				q->next = NULL;
				p->next = q;
				p = q;
			}
			else   break;
		}
		printf("初始化索引表完成。");
		return Khead;
	}
}
//zhai

