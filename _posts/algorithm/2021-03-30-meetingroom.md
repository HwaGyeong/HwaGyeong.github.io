---
layout: post
title : "[백준 1931] 회의실 배정"
subtitle: 그리디 알고리즘
categories:
  - algorithm
tags:
  - [Algorithm, BOJ ,greedy]
comments: true
---

### 회의실 배정

> 문제설명   

![image](https://user-images.githubusercontent.com/55472510/112973677-923f4100-918c-11eb-99fd-4ed3f6d7df47.png)

회의의 수를 입력 받고, 각 회의마다 시작시간과 종료시간을 입력 받는다. 
최대한 많은 회의를 선택하여 그 갯수를 출력하면 된다.   

***
> 알고리즘
1. 필요한 입력 값들을 모두 입력 받는다.
2. 벡터에 담아 정렬하되, `종료시간을 기준으로 오름차순으로 정렬`한다. 
3. 정렬한 벡터를 차례대로 조회하여 `현재 회의 종료시간이 다음 회의 시작 시간과 겹치는지 확인`하고 겹치지 않으면 선택한다.
4. 선택할 때 회의의 수를 카운트하고 조회가 끝나면 그 수를 출력한다.

*** 
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
bool cmp(const pair <int, int> &a, const pair <int, int> &b)
{
	if (a.second == b.second)//종료시간이 같으면
		return a.first < b.first;//시작시간이 작은 순서대로 
	return a.second < b.second;//종료시간이 짧은 순서대로 정렬
}
int main(void)
{
	int N;
	cin >> N;
	vector <pair<int, int>> v;
	while (N)//회의실 정보 입력받기
	{
		int a, b;
		cin >> a >> b;
		v.push_back({ a,b });
		N--;
	}
	sort(v.begin(), v.end(),cmp);//값 정렬 
	int count = 1;//첫 회의실 
	int end = v[0].second;//첫 회의실 종료 시간
	for (int i = 1; i < v.size(); i++)
	{
		if (v[i].first>=end)//회의 종료시간이 다음 시작시간이랑 같거나 크면 선택
		{
			end = v[i].second;//종료시간 갱신
			count++;//회의 예약 카운트
		}		
	}

	cout << count << "\n";//회의 예약 수 출력
	return 0;
}
```
> 고찰   

그리디 알고리즘이여서 어려웠는데, 정렬만 잘하면 쉽게 결론낼 수 있는 과제였다.    

처음엔 갈피를 못잡아서 조건을 두가지 생각했다.
- 회의 시간이 짧은 회의
- 회의 시작시간이 빠른 회의   

하지만 위의 두가지를 고려하면 짤 수 없다. 최대한 많은 회의를 선정해야하니 빨리 끝나는 순으로 정렬하고 끝나는 시간이 동일하면 시작시간을 기준으로 오름차순으로 정렬하면 된다.
그리고 각각 저장한 부분을 조회하여 선정하고, 해당 회의의 종료시간이 다음 회의의 시작시간보다 작기만 하면 선정할 수 있는 구조로 짜여있다.

따라서 요약하자면 이번 문제의 핵심은 `정렬`인데, `회의 종료시간이 빠른순서대로, 같으면 시작시간이 빠른시간으로 정렬`하면된다.