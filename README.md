# SqList
#include<stdio.h>
#include<malloc.h>
#include<stdlib.h>
#define TRUE 1
#define FALSE 0
#define OK 1
#define ERROR 0
#define INFEASIBLE -1
#define OVERFLOW -2
typedef int Status;
#define LIST_INIT_SIZE 100
#define LISTINCREMENT 10
typedef char ElemType;
typedef struct {
	ElemType *elem;
	int length;
	int listsize;
}SqList;
Status InitList(SqList &L)
{
	L.elem = (ElemType *)malloc(LIST_INIT_SIZE * sizeof(ElemType));
	if (!L.elem) exit(OVERFLOW);
	L.length = 0;
	L.listsize = LIST_INIT_SIZE;
	return OK;
}
Status ListInsert(SqList &L, int i, ElemType e)
{
	char *p, *q;
	ElemType *newbase;
	if (i<1 || i>L.length + 1) return ERROR;
	if (L.length >= L.listsize)
	{
		newbase = (ElemType *)realloc(L.elem, (L.listsize + LISTINCREMENT) * sizeof(ElemType));
		if (!newbase) exit(OVERFLOW);
		L.elem = newbase;
		L.listsize += LISTINCREMENT;
	}
	q = &(L.elem[i - 1]);
	for (p = &(L.elem[L.length - 1]); p >= q; --p)
		*(p + 1) = *p;
	*q = e;
	++L.length;
	return OK;
}
Status DispList(SqList &L)
{
	int i;
	for (i = 0; i < L.length; i++)
		printf("%c", L.elem[i]);
	return OK;
}
Status ListLength(SqList &L)
{
	return L.length;
}
Status ListEmpty(SqList &L)
{
	if (L.length== 0) return OK;
	else return ERROR;
}
Status GetElem(SqList &L, int i, ElemType e)
{
	if (i<1 || i>L.length) return ERROR;
	e = L.elem[i];
	return OK;
}
Status LocateElem(SqList L, ElemType e)
{
	int i=1;
	while (i < L.length&&i != e)
	{
		++i;
	}
	if (i <= L.length) return i;
	else return ERROR;
}
Status ListDelete(SqList &L, int i, ElemType e)
{
	char *p, *q;
	if (i<1 || i>L.length) return ERROR;
	p = &(L.elem[i - 1]);
	e = *p;
	q = L.elem + L.length - 1;
	for (++p; p <= q; ++p)
		*(p - 1) = *p;
	--L.length;
	return OK;
}
Status DestroyList(SqList &L)
{
	free(L.elem);
	return OK;
}
