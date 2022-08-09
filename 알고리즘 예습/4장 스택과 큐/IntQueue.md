```C
// int형 IntQueue
#include <stdio.h>
#include <stdlib.h>

typedef struct {
	int max; // 큐의 최대 용량
	int num; // 현재의 요소 개수
	int front; // 프런트
	int rear; // 리어
	int* que; // 큐의 맨 앞 요소에 대한 포인터

} IntQueue;

// 큐 초기화
int Initialize(IntQueue* q, int max)
{
	q->num = q->front = q->rear = 0;
	if ((q->que = (int*)malloc(max * sizeof(int))) == NULL)
	{
		q->max = 0;
		return -1;
	}
	q->max = max;
	return 0;
}

// 큐에 데이터를 인큐
int Enque(IntQueue* q, int x)
{
	if (q->num >= q->max)
	{
		return -1;
	}

	else
	{
		q->num++;
		q->que[q->rear++] = x;

		if (q->rear == q->max)
			q->rear = 0;
		return 0;
	}
}

// 큐에 데이터를 디큐
int Deque(IntQueue* q, int *x)
{
	if (q->num <= 0)
		return -1;
	else
	{
		q->num--;
		*x = q->que[q->front++];
		
		if (q->front == q->max)
			q->front = 0;
		return 0;
	}

}

// 큐에서 데이터를 피크
int Peek(const IntQueue* q, int* x)
{
	if (q->num <= 0)
		return -1;
	else
	{
		*x = q->que[q->front];
		return 0;
	}
}


// 모든 데이터 삭제
void Clear(IntQueue* q)
{
	q->num = q->front = q->rear = 0;

}

// 큐의 최대 용량
int Capacity(const IntQueue* q)
{
	return q->max;
}

// 큐애 저장된 데이터 개수
int Size(const IntQueue* q)
{
	return q->num;
}

// 큐가 비어 있는가?
int IsEmpty(const IntQueue* q)
{
	return q->num <= 0;
}

// 큐가 가득 찼는가?
int IsFull(const IntQueue* q)
{
	return q->num >= q->max;
}

// 큐에서 검색
int Search(const IntQueue* q, int x)
{
	int i, idx;
	for (i = q->front; i < q->rear; i++)
	{
		idx = (q->front + i) % q->max;
		if (q->que[idx] == x)
			return idx;
	}
	return -1;
}

// 모든 데이터 출력
void Print(const IntQueue* q)
{
	int i, idx;
	for (i = q->front; i < q->rear; i++)
	{
		idx = (q->front + i) % q->max;
		printf("%d ", q->que[i]);
	}
	printf("\n");
}

// 큐 종료
void Terminate(IntQueue* q)
{
	if (q->que != NULL)
		free(q->que);
	q->max = q->front = q->rear = 0;
}

int main()
{
	IntQueue que;
	
	if (Initialize(&que, 64) == -1)
	{
		puts("큐의 생성에 실패하였습니다.");
		return 1;
	}
	while (1)
	{
		int m, x;

		printf("현재 데이터 수 : %d / %d \n", Size(&que), Capacity(&que));
		printf("(1)인큐 (2)디큐 (3)피크 (4)출력 (0)종료 : ");
		scanf("%d", &m);

		if (m == 0)
			break;

		switch (m)
		{
		case 1: // 인큐
			printf("데이터 : "); scanf("%d", &x);
			if (Enque(&que, x) == -1)
				puts("\a오류 : 인큐에 실패하였습니다.");
			break;

		case 2: // 디큐
			if (Deque(&que, &x) == -1)
				puts("\a오류 : 디큐에 실패하였습니다.");
			else
				printf("디큐한 데이터는 %d입니다.\n", x);
			break;

		case 3: // 피크
			if (Peek(&que, &x) == -1)
				puts("\a오류 : 피크에 실패하였습니다.");
			else
				printf("피크한 데이터는 %d입니다.\n", x);
			break;

		case 4: // 출력
			Print(&que);
			break;
		}

	}
	Terminate(&que);
	return 0;
}
```
