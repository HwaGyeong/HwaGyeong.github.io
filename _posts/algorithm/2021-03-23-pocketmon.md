---
layout: post
title : "[백준 1620] 나는야 포켓몬 마스터 이다솜"
subtitle: 자료구조, 문자열
categories:
  - algorithm
tags:
  - [Algorithm, BOJ ,문자열]
comments: true
---

### 나는야 포켓몬 마스터 이다솜


> 문제설명
![image](https://user-images.githubusercontent.com/55472510/112261355-0a01fd00-8caf-11eb-861b-bcf3c0c7edeb.png)

포켓몬의 수 N과 테스트 수 M을 입력받는다. N번 포켓몬의 이름을 입력 받고 M번테스트 할때 포켓몬의 번호가 입력되면 이름을, 포켓몬의 이름이 입력되면 번호를 출력한다.    

***   

> 알고리즘
1. N,M을 입력받고 포켓몬을 입력받는다.
2. 포켓몬의 이름과 번호를 저장하기 위한 벡터를 pair로 선언하고, 이름만 저장하는 벡터 또한 만든다.
3. 이름과 번호가 저장된 벡터를 정렬해준다.
4. M번 반복해서 숫자인 경우 이름만 저장된 벡터에서 값을 출력하고, 이름이 입력된 경우 이분탐색으로 찾아준다.    

***   

```cpp
#include <stdio.h>
#include <iostream>
#include <vector>
#include <string>
#include <utility>
#include <algorithm>
using namespace std;

int N, M;
int main()
{		
	ios::sync_with_stdio(0);
	cin.tie(NULL);
	cout.tie(NULL);
	vector <pair <string, int>>v;//이름과 인덱스 저장
	vector <string > vs;//이름만 저장
	cin >> N >> M;
	
	for (int i = 0; i < N; i++)//값 입력
	{
		string s;
		cin >> s;
		vs.push_back(s);
		v.push_back({ s, i + 1 });
	}

    sort(v.begin(), v.end());//이름, 인덱스 있는 부분 정렬
	for (int i = 0; i < M; i++)
	{
		string s;
		cin >> s;
		int num = 0;

		if (s[0] >= '0' && s[0] <= '9')//숫자가 입력되는 경우 처리
		{
			num = stoi(s);
			cout << vs[num-1] << "\n";
		}
		else//속도를 위해 이진 탐색 
		{
			int left = 0;
			int right = N - 1;

			while (left <= right)
			{
				int mid = (left + right) / 2;
				if (v[mid].first == s)
				{
					cout << v[mid].second << "\n";
					break;
				}
				else if (v[mid].first > s)
					right = mid - 1;
				else
					left = mid + 1;
			}
		}

	}

	return 0;
}
```   

> 고찰   

시간제한이 걸리는 문제여서 단순히 for반복문으로 돌면 시간이 초과된다.
따라서 이름이 들어왔을 때 그 숫자를 찾을 때는 이분 탐색알고리즘을 이용해서 찾아주어야 한다. 