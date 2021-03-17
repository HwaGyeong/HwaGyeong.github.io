---
layout: post
title : "[백준 11659] 좌표 정렬하기"
subtitle: sort 활용
categories:
  - algorithm
tags:
  - [Algorithm, BOJ ,sort ]
comments: true
---

### 좌표 정렬하기 
> 문제설명
![image](https://user-images.githubusercontent.com/55472510/111427509-8c337400-8739-11eb-956b-5025ce373929.png)

2차원 평면 위의 점 N개가 주어진다. 좌표를 x좌표가 증가하는 순으로, x좌표가 같으면 y좌표가 증가하는 순서로 정렬한 다음 출력하는 프로그램을 작성하시오.

***
> 알고리즘
1. 테스트 케이스 수를 입력 받고 수만큼 반복하여 2차원 좌표를 입력 받는다.
2. algorithm 헤더의 sort 함수를 사용하여 정렬한다.

```cpp
#include <stdio.h>
#include <iostream>
#include <algorithm>
#include <vector>
#include <utility>
using namespace std;

bool comp(const pair <int, int> & a, const pair<int, int>&b)
{
	if (a.first == b.first)
		return a.second < b.second;
	return a.first < b.first;
}
int main()
{	
	int num = 0;
	cin >> num;
	vector <pair<int, int>> v;
	while (num)
	{
		int a, b;
		cin >> a >> b;
		v.push_back({ a,b });
		num--;
	}
	sort(v.begin(), v.end(), comp);
	for (int i = 0; i < v.size(); i++)
		cout << v[i].first << " " << v[i].second << "\n";
	return 0;
}
```
***
> 고찰
알고리즘 헤더를 사용하여 정렬하면 간단하게 해결되는 문제였다. 
sort 함수를 사용할 때 다음과 같이 함수를 정의해서 비교하여 값이 나오도록 한다. 
이번에는 벡터에 `pair형태`로 선언해서 x좌표와 y좌표를 받았는데 오름차순을 기준으로 x좌표가 동일한 경우 y좌표의 오름차순으로 정렬이 필요하다. sort함수 사용 방식을 아래와 같이 정리해 놓았다. 

*** 
> sort 정리
```cpp
sort(v.begin(), v.end(), comp);// sort 함수 실행

//비교함수 : 사용자 정의 함수 
bool comp(const pair <int, int> & a, const pair<int, int>&b)
{
	if (a.first == b.first)//첫 번째 원소가 같을 경우
		return a.second < b.second;//두 번째 원소를 기준으로 오름 차순 정렬
	return a.first < b.first;//같지 않은 경우 첫 번째 원소 기준 오름차순 정렬
}
```
compare 하는 함수로 사용하지 않으면 기본적으로 오름차순으로 정렬된다.   

인자로 비교하고자 하는 두 매개 변수를 pair형태로 넣어준다.   
비교하고자하는 첫 번째 원소나 두 번째 원소가 같을 때의 비교기준을 만들어 줄 수 있다. 
그리고 기본적인 비교 기준을 만들어 줄 수 도 있다. 

#### _<그 외의 형태>_
```cpp
sort(arr, arr+n);
sort(v.begin(), v.end(), compare);                //사용자 정의 함수 사용

sort(v.begin(), v.end(), greater<자료형>());    //내림차순 (Descending order)

sort(v.begin(), v.end(), less<자료형>()); 
//오름차순 (default = Ascending order)

```