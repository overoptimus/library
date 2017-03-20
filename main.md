# library
图书管理

#include"stdio.h"
#include"string.h"
#include"stdlib.h"
#define key 10;

typedef struct Reader {
	long number;           //读者账号
	char name[20];         //读者名字
	char sex[20];          //读者性别
	char password[20];     //读者密码
	int  booknum;          //读者当前借书量
	long jiebook[10];      //读者借阅图书书号
	struct Reader *next;
}Reader;

typedef struct Book {
	long number;           //书号
	char name[20];         //书名
	char author[20];       //著者
	long xiancun;          //现存量
	long zongku;           //总库存
	struct Book *next;
}Book;

typedef struct KEY {
	Book *adress;
	struct KEY *next;
}keynode;

Book *zairubook();
Reader *zairureader();
void Administer(Book *B, Reader *R,keynode *Khead);
void incertbook(Book *Bhead, Reader *Rhead,keynode *Khead);
void deletebook(Book *Bhead, Reader *Rhead,keynode *Khead);
void showbook(Book *Bhead, Reader *Rhead,keynode *Khead);
void showreader(Book *Bhead, Reader *Rhead,keynode *Khead);
void firstmain(Book *Bhead,Reader *Rhead,keynode *Khead);
void baocun(Book *Bhead, Reader *Rhead);
Reader *searchreader(Reader *Rhead, long number1);
keynode *initindex(Book *Bhead);//初始化建立索引表


Book *B_name(Book *Bhead, char *Name);
int B_author(Book *Bhead, char *Author, Book *s[20]);
Book *B_number(Book *Bhead, long num,keynode *Khead);
void SEEK(Book *Bhead,Reader *Rhead, Reader *reader,keynode *Khead);
void B_reader(Book *Bhead,Reader *Rhead, Reader *reader,keynode *Khead);
void PD(Book *Bhead, Book *p,Reader *Rhead, Reader *reader,keynode *Khead);
void login(Book *Bhead, Reader *Rhead,keynode *Khead);
void REGISTER(Book *Bhead, Reader *Rhead,Reader *reader,keynode *Khead);
void revert(Book *Bhead,Reader *Rhead, Reader *reader,keynode *Khead);

#include"library.h"

int main()
{
	Book *Bhead;
	Reader *Rhead;
	keynode *Khead=NULL;
	Bhead = zairubook();
	Rhead = zairureader();
	Khead = initindex(Bhead);
	while (1)
	{
		firstmain(Bhead,Rhead,Khead);
	}
	system("pause");
	return 0;
}


















