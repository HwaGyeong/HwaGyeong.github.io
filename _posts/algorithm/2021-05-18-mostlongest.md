---
layout: post
title : "[백준 11053] 가장 긴 증가하는 부분 수열"
subtitle: 다이나믹 프로그래밍 문제 해결
categories:
  - algorithm
tags:
  - [Algorithm, BOJ, dp]
comments: true
---

### 가장 긴 증가하는 부분 수열

> 문제설명    

![image](https://user-images.githubusercontent.com/55472510/118619935-ffbc3500-b7ff-11eb-9bb8-1119a67e4106.png)

가장 긴 증가하는 수열의 길이를 구한다.

> `알고리즘`
1. 값을 각각 입력받고 dp의 값을 길이만큼 1로 초기화 해준다.
2. 이중 for문 안에서 arr값을 하나 불러오고 밖의 for문의 인덱스 만큼 다시 0부터 인덱스까지 값을 조회한다.
3. 값을 비교하여 현재 구하고자 하는 곳의 값이 이전의 수열 인덱스 값보다 큰 경우 max값을 찾아서 업데이트한다.
4. 업데이트할 때 현재의 dp값, 그리고 이전의 dp값 중에 가장 큰 값에 1을 더하고 큰 값을 저장한다.
5. dp중에 가장 큰 값이 최장길이이다.

***
   
   

```cpp
#include <iostream>
#include <stdio.h>
#include <algorithm>
#define MAX 1001
using namespace std;
int arr[MAX];
int dp[MAX];

int main() {
	ios_base::sync_with_stdio(0);
	cin.tie(0);

	int N;
	cin >> N;

	for (int i = 0; i < N; i++)
	{
		cin >> arr[i];
		dp[i] = 1;
	}

	for (int i = 0; i < N; i++)
	{
		for (int j = 0; j < i; j++)
		{
			if (arr[i] > arr[j])//이 전의 값보다 크니 업데이트 필요
				dp[i] = max(dp[i],dp[j]+1); //그 전의 길이중 가장 긴 값+1 
		}
	}

	cout<<*max_element(dp, dp+N); //최대 값 출력
	
	return 0;
}

```   
> 고찰   

이번 문제는 dp문제로 처음에는 막막한 편이 있었다. 
1차원 dp를 선언하고 모두 1로 초기화하는데, 자기자신만을 포함한 길이이다. 그리고 이차원 반복문으로 자신보다 인덱스가 작은 값 수열을 모두 조회해서 현재 값이 이전의 수열 값 보다 작은 경우 dp를 업데이트 해준다. 업데이트 할때 max를 찾는 것이 포인트이다. 그냥 업데이트를 하면 더 작아질 수 도 있으므로 현재까지 만든 dp값 중에 가장 큰 값이나, 현재 값 중 더 큰 값을 찾아 업데이트 하여 값을 저장한다. 
