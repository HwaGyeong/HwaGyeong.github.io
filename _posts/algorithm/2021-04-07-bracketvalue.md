---
layout: post
title : "[백준 2504] 괄호의 값"
subtitle: stack을 사용하여 괄호의 값을 계산하자
categories:
  - algorithm
tags:
  - [Algorithm, BOJ, stack]
comments: true
---

### 괄호의 값

> 문제설명   

![image](https://user-images.githubusercontent.com/55472510/113821908-b071f600-97b7-11eb-8ded-6fca14d82ae8.png)

첫 줄에 식을 입력 받는다. 이때 식은 (),[]로 이루어져 있다.   
()는 2이고 []는 3이다. 주어진 식의 값을 알맞게 계산하여 값을 출력한다.   

> `알고리즘`
1. 문자열을 입력받는다.
2. 문자열을 각각 read 하고, 여는 괄호일 때는 `계산의 편의를 위해서 괄호 그대로 넣지 않고 '('일 때는 -1을 넣고 '['일때는 -3을 넣는다`
3. 짝이 맞지 않는 경우 바로 0을 리턴하여 종료 시킨다.
4. `닫는 괄호가 들어온 경우 stack의 맨 위값이 짝이 맞는지 확인하고 맞으면 (일 때 2를 넣고 [이면 3을 넣는다.`
5. 맨 위가 숫자이면 여는 괄호를 만날때 까지 전부 더한다. 
6. 전부 더한 뒤 다음 가장 위의 값이 짝이 맞으면 2나 3을 곱하고 그 값을 stack을 넣는다.
7. 2-6의 과정을 반복하고 stack을 다시 조회하고 내부에 숫자를 모두 더해 값을 출력한다.
8. 스택을 조회할 때 괄호의 값인 -1,-3이 있는 경우 짝이 맞지 않는 경우로 0을 출력한다.   
   

```cpp
#include <stdio.h>
#include <iostream>
#include <stack>
#include <string>
using namespace std;
long long calculator(string str)
{
	stack <int> s;
	long long ans = 0;

	for (int i = 0; i < str.length(); i++)
	{
		char temp = str[i];
		if (temp == '(')//여는 괄호면 push
			s.push(-1);
		else if (temp == '[')//여는 괄호면 push
			s.push(-3);
		else if (s.empty())//짝이 안 맞는 경우
			return 0;
		else // 닫는 괄호 ),]를 만난 경우
		{
			if (s.top() == -1 && temp==')') //맨위가 여는 괄호 ( 일 때 
			{
				s.pop();
				s.push(2);
			}
			else if (s.top() == -3 && temp == ']') //맨위가 여는 괄호 [ 일 때 
			{
				s.pop();
				s.push(3);
			}
			else //맨 위가 숫자인 경우 
			{
				int tmp = 0;
				while (s.top() != -1 && s.top() != -3)//여는 괄호를 만날 때 까지 숫자를 전부 더한다
				{
					tmp += s.top();
					s.pop();

					if (s.empty())//더이상 뺄 수 없는 경우
						return 0;
				}			
				if (s.top() == -1 && temp == ')')
				{
					tmp *= 2;
					s.pop();
				}
				else if (s.top() == -3 && temp == ']')
				{
					tmp *= 3;
					s.pop();
				}
				else //짝이 안맞는 경우 
					return 0;

				s.push(tmp);
			}
		}
	}

	while (!s.empty())//최종 값 계산
	{
		if (s.top() == -1 || s.top() == -3)
		{
			return 0;
		}
		ans += s.top();
		s.pop();
	}
	return ans;
}
// '(' 는 -1 ')'는 -2 '[' -3 이고 ']'는 -4 
int main(void)
{
	ios::sync_with_stdio(0);
	cin.tie(NULL);
	cout.tie(NULL);
	string str;
	cin >> str;
	long long ans=calculator(str);//함수 실행
	cout << ans;
	return 0;
}
```   
> 고찰   

이번 문제는 굉장히 복잡하고 고려할 게 많았던 문제였다. 
테스트 케이스는 돌아가는데 런타임 에러가 뜨는 경우도 있었기 때문에 결국 검색해서 풀 수 있었다. 아직 한참 모자란것 같다. 

- seg fault 원인: stack의 empty일 때 조회하면 안되는데 처리를 안해줘서 발생
- invalid argument 원인: c++17에서는 stoi함수가 적용되지 않는다. 따라서 괄호를 숫자로 치환해서 풀었다. 
