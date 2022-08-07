```C
#include <stdio.h>
#include <stdlib.h>

typedef struct
{
	int max; // 스택 용량
	int ptr; // 스택 포인터
	int* stk; // 스택의 첫 요소에 대한 포인터

}IntStack;

// 스택 초기화
int Initialize(IntStack* s, int max)
{
	s->ptr = 0;
	if ((s->stk = (int*)malloc(max*sizeof(int))) == NULL)
	{
		s->max = 0; // 배열의 생성에 실패
		return -1;
	}

	s->max = max;
	return 0;

}

// 스택에 데이터를 푸시
int Push(IntStack* s, int x)
{
	if (s->ptr >= s->max) // 스택이 가득 참
		return -1;
	s->stk[s->ptr++] = x;
	return 0;

}

// 스택에서 데이터를 팝
int Pop(IntStack* s, int* x)
{
	if (s->ptr <= 0) // 스택이 비어 있음
		return -1;
	*x = s->stk[((s->ptr)-1)];
	s->ptr--;
	return 0;
}

// 스택에서 데이터를 피크
int Peek(const IntStack* s, int* x)
{
	if (s->ptr <= 0) // 스택이 비어 있음
		return -1;
	*x = s->stk[(s->ptr) - 1];
	return 0;
}

// 스택 비우기
void Clear(IntStack* s)
{
	s->ptr = 0;
}

// 스택 용량
int Capacity(const IntStack* s)
{
	return s->max;
}

// 스택에 쌓여 있는 데이터 수
int Size(const IntStack* s)
{
	return s->ptr;
}

// 스택이 비어 있는가?
int IsEmpty(const IntStack* s)
{
	return s->ptr <= 0;
}

// 스택은 가득 찼는가?
int IsFull(const IntStack* s)
{
	return s->ptr >= s->max;
}

// 스택에서 검색
int Search(const IntStack* s, int x)
{
	int i;
	for (i = s->ptr - 1; i >= 0; i--) // 꼭대기 -> 바닥으로 선형 검색
		if (s->stk[i] == x) 
			return i; // 검색 성공
	return -1; // 검색 실패
}

// 모든 데이터 출력
void Print(const IntStack* s)
{
	int i;
	for (i = 0; i < s->ptr; i++) // 바닥 -> 꼭대기로 스캔
		printf("%d ", s->stk[i]);
	putchar('\n');
}

// 스택 종료
void Terminate(IntStack* s)
{
	if (s->stk != NULL)
		free(s->stk); // 배열을 삭제
	s->max = s->ptr = 0;
}

int main()
{
	IntStack s;
	if (Initialize(&s, 64) == -1)
	{
		puts("스택 생성에 실패하였습니다.");
		return 1;
	}

	while (1)
	{
		int menu, x;
		printf("현재 데이터 수 : %d / %d\n", Size(&s), Capacity(&s));
		printf("(1) 푸시 (2) 팝 (3) 피크 (4) 출력 (0)종료 : ");
		scanf("%d", &menu);

		if (menu == 0) break;
		switch (menu)
		{
		case 1 : // 푸시
			printf("데이터 : ");
			scanf("%d", &x);
			if (Push(&s, x) == -1)
				puts("\a오류 : 푸시에 실패하였습니다.");
			break;

		case 2: // 팝
			if (Pop(&s, &x) == -1)
				puts("\a오류 : 팝에 실패하였습니다.");
			else
				printf("팝 데이터는 %d입니다.\n", x);
			break;

		case 3: // 피크
			if (Peek(&s, &x) == -1)
				puts("\a오류 : 피크에 실패하였습니다.");
			else
				printf("피크 데이터는 %d입니다.\n", x);
			break;

		case 4: // 출력
			Print(&s);
			break;
		}

	}
	Terminate(&s);
	return 0;
}
```
