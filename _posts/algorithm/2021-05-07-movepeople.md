---
layout: post
title : "[백준 16234] 인구 이동"
subtitle: dfs와 bfs 알고리즘
categories:
  - algorithm
tags:
  - [Algorithm, BOJ, bfs]
comments: true
---

### 인구 이동

> 문제설명   
![image](https://user-images.githubusercontent.com/55472510/117404792-8726c000-af45-11eb-995e-14de242f3104.png)


각 도시간의 인구이동이 일어나는 데 필요한 일자를 구하자 

> `알고리즘`
1. 값을 각각 graph 이차원 배열에 입력 받는다.
2. 무한루프 안에서 여러번 인구이동을 확인하기 위해서 visited 를 초기화하고, 방문 여부 체크를 초기화한다.
3. 방문한적 없는 도시를 만나면 그 도시의 인구수, 현재 연합된 도시의 수, 그리고 도시의 좌표를 저장한다.
4. bfs함수를 실행한다.
5. bfs함수에서는 4방향 다 방문해보고 gap이 L과 R사이에 있는 경우 방문하고 인구수를 구하고 방문 표시를 한다.
6. bfs함수를 돌고 난 뒤 총 나라가 2개이상이면 연합이 결성되었다는 의미로 인구 재분배를 해준다.
7. 연합이 결성되면 결과를 1 증가시키기 위해서 checker를 true해준다.
8. 각 점을 다 돌고 나오면 checker여부를 확인해서 결과값을 증가 시키고 연합이 결성되지 않았으면 프로그램이 종료된다.   

***
   
   

```cpp
#include <stdio.h>
#include <iostream>
#include <queue>
#include <math.h>
#define MAX 55
using namespace std;
int N, L, R;
int graph[MAX][MAX];
bool visited[MAX][MAX];
int dx[4] = { 0,0,1,-1 };
int dy[4] = { 1,-1,0,0 };
int result, people, cnt;
vector <pair<int, int>> location;

void bfs(int x, int y)
{
	queue<pair<int, int>> q;
	q.push({ x, y });
	visited[x][y] = true;

	while (!q.empty())
	{
		int x_cur = q.front().first;
		int y_cur = q.front().second;

		q.pop();

		for (int i = 0; i < 4; i++)
		{
			int nx = x_cur + dx[i];//위치이동 반경 계산
			int ny = y_cur + dy[i];
			if (nx >= 0 && ny >= 0 && nx < N && ny < N)//범위내에 해당하는 부분이면 
			{
				int gap = abs(graph[nx][ny] - graph[x_cur][y_cur]);
				if (gap >= L && gap <= R && visited[nx][ny] == 0)
				{
					location.push_back({ nx,ny });//위치값 저장
					people += graph[nx][ny];//사람수 구하기
					visited[nx][ny] = true;//현재 나라를 방문 했음을 의미
					cnt++;
					q.push({ nx,ny });
				}
			}
		}
	}
}

void combination()
{
	int total = people / cnt;//인구 이동 후 인구수

	for (int i = 0; i < location.size(); i++)//연합후 인구수로 변경
	{
		int a = location[i].first;
		int b = location[i].second;
		graph[a][b] = total;//인구 재분배
	}
}
int main(void)
{
	ios::sync_with_stdio(0);
	cin.tie(NULL);
	cout.tie(NULL);
	cin >> N >> L >> R;

	for (int i = 0; i < N; i++)//값 입력
	{
		for (int j = 0; j < N; j++)
			cin >> graph[i][j];
	}

	while (1)
	{
		for (int i = 0; i < MAX; i++)//방문 초기화 
		{
			for (int j = 0; j < MAX; j++)
				visited[i][j] = false;
		}
		bool checker = false;
		for (int i = 0; i < N; i++)//값 입력
		{
			for (int j = 0; j < N; j++)
			{
				if (visited[i][j] == 0)
				{
					people = graph[i][j];
					cnt = 1;
					location.clear();
					location.push_back({ i,j });//위치 넣어주기
					bfs(i, j);

					if (cnt >= 2)//연합이 존재할 때
					{
						combination();
						checker = true;
					}
				}
			}
		}
		if (checker)
			result++;
		else
			break;
	}

	cout << result;
	return 0;
}

```   
> 고찰   

이번 문제는 이상한 곳에서 삽질을 많이했다. 요건대로 제대로 구현해놓고 이상하게 백준에서 3%에서 안올라가길래 visited초기화 부분이 이상해서 직접 반복문으로 해주니까 맞았다고 나왔다.
fill함수를 제대로 못읽는 것 같았다.
정확한 이유를 모르겠어서 앞으로 초기화할때 memset을 사용해야겠다.

