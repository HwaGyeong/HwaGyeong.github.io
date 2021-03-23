---
layout: post
title : "[백준 2630] 색종이 만들기"
subtitle: 분할정복, 재귀
categories:
  - algorithm
tags:
- [Algorithm, BOJ , 분할정복 , 재귀]
comments: true
---

### 색종이 만들기

> 문제설명
![image](https://user-images.githubusercontent.com/55472510/111958349-786c8100-8b30-11eb-9de3-858b4e132095.png)
![image](https://user-images.githubusercontent.com/55472510/111958401-8c17e780-8b30-11eb-89db-949109febc98.png)

정사각형 형태의 색종이의 가로 세로 길이가 입력된다. 이때 항상 `N=2^k`이다. 색종이 각 칸은 `흰색(0) 또는 파란색(1)`로 칠해져 있고 종이를 잘랐을 때 모두 같은 색이 아니면 4등분 한다. 이 과정을 더 이상 자를 수 없을 때까지 반복한다.
그리고 흰색 종이의 수와 파란색 종이의 수를 출력한다.

***
> 알고리즘 
1. 색종이의 가로(=세로)길이를 입력 받고 각 칸마다 흰색은 0 파란색은 1로 입력 받는다.
2. 가로 길이와 0,0을 divide 함수로 전달한다. 
3. 만약 `num이 1인 경우에는 더 이상 나눌 수 없기 때문에` 색깔을 판별하여 올려준다. 
4. `divide 함수에서는 각 칸의 흰색칸과 파란색 칸의 수를 세어서 현재 함수의 num(가로, 세로)의 제곱 수와 일치하는지 확인한다.`
5. 일치하면 더 이상 나눌 필요 없으므로 흰색인지 파란색인지 판별한다.
6. 일치하지 않으면 4등분 하여 재귀적인 방식으로 함수를 호출한다.

***

```cpp
#include <stdio.h>
#include <iostream>
#include <vector>
using namespace std;
#define MAX 129
int arr[MAX][MAX];
int white, blue;//각각 색종이 수를 세기 위한 변수
void divide(int num, int x, int y)
{
	int cnt1 = 0, cnt2 = 0;
	if (num == 1)//한 칸짜리 색종이인 경우 색 판별 후 종료
	{
		if (arr[x][y]==0)
			white++;
		else if (arr[x][y]==1)
			blue++;

		return;
	}
	else// 한 칸 짜리가 아닌 경우 전체 판별
	{
		for (int i = x; i < x+num; i++)
		{
			for (int j = y; j < y+num; j++)
			{
				if (arr[i][j] == 0)
					cnt1++;
				if (arr[i][j] == 1)
					cnt2++;
			}
		}
		if (cnt1 == num * num)//흰색칸의 수가 자른 2^n * 2^n 인 경우 -> 더이상 자를 필요 없음
			white++;
		else if (cnt2 == num * num)//파란색칸의 수가 자른 2^n * 2^n 인 경우 -> 더이상 자를 필요 없음
			blue++;
		else //더 잘라야 하는 경우 (해당 칸에 서로 다른 색이 존재하는 경우)
		{
			divide(num / 2, x, y); //4구역중 왼쪽 위 부분 확인
			divide(num / 2, x, y + num / 2); //4구역 중 오른쪽 위 부분 확인
			divide(num / 2, x + num / 2, y); //4구역 중 왼쪽 아래 부분 확인 
			divide(num / 2, x + num / 2, y + num / 2); //4구역 중 오른쪽 아래 부분 확인
		}
	}
}

int main()
{	
	int num;//색종이의 가로 세로 길이 
	cin >> num;	
	for (int i = 0; i < num; i++)// 색종이 값 입력 받기
	{
		for (int j = 0; j < num; j++)
		{
			int n;
			cin >> n;
			arr[i][j] = n;
		}
	}

	divide(num,0,0);//색종이 크기와 시작 좌표
	cout << white << "\n" << blue ;
	return 0;
}
```
> 고찰   

분할 정복은 평소에 어렵게 생각했었는데, 이번에 풀면서 비교적 해소가 되었다. 
작은것 보다 더 작게 만들면 해결되는 문제였다. 