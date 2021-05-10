---
layout: post
title : "[백준 1629] 곱셈"
subtitle: 분할 정복 문제를 해결하자
categories:
  - algorithm
tags:
  - [Algorithm, BOJ, divide&conquer]
comments: true
---

### 곱셈

> 문제설명    

![image](https://user-images.githubusercontent.com/55472510/117609734-796a7800-b19b-11eb-8ea4-4b09ea378e84.png)

주어진 수 A,B,C가 입력되면 A를 B번 곱한 수를 C로 나눈 나머지를 출력하자

> `알고리즘`
1. 값을 각각 입력받고 `POW`함수에 인자를 전달해준다. 
2. B(제곱 수)가 0이면 1을 반환하고 1이면 A%C를 준다.
3. 위의 두 경우가 아니면 B를 2로 나누고 그 함수의 나머지를 구한다.
4. 위의 값을 짝수일때는 제곱해서 나머지를 구한다.
5. 홀수인 경우 다시 A를 곱해서 제곱수를 짝수로 만들어서 나머지를 구해준다.  

***
   
   

```cpp
#include <iostream>
#include <stdio.h>

//modular 연산 XYmodM=(XmodM*YmodM)mod M
using namespace std;
long long A, B, C;
long long int POW(long long A, long long B, long long C)
{
	if (B == 0)
		return 1;
	if (B == 1)
		return A % C;
	long long result = POW(A, B / 2, C) % C;
	if (B % 2 == 0)
		return (result*result) % C;
	else //홀수 일 때 -> 다시 원래수를 곱해야 함 (짝수로 만들어 줌) 
		return (((result*result) % C)*A)%C;
}
int main() {
	
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);cout.tie(NULL);
	cin >> A >> B >> C;

	long long int result = POW(A,B,C);
	
	cout << result;
	return 0;
}
```   
> 고찰   

이번 문제는 `분할정복`이 포인트인 문제였다. 당연하게 math헤더에 있는 pow함수를 이용하면 될것이라고 생각했지만 그렇게 단순할리 없었다.
먼저 modular 개념을 알고 있어야 했다.    
블로그 정리 된 글을 봤는데 봐도 이해하기 어려운 부분이 다소있었다. 아직 분할정복 알고리즘을 자유롭게 구사하는 능력이 부족한 것 같다.   
그리고 숫자의 범위 때문에 자료형을 long long int로 선언하는 것도 큰 포인트였다.
long long으로 하니까 답이 틀렸다고 나왔다. 

> Modular 개념
- XYmodM=(XmodM*YmodM)mod M   

위와 같이 나타낼 수 있다. 따라서 이번 문제의 경우에는 제곱수를 낮추기 위해서 B를 2로 나누어서 나머지를 구해서 result를 반환한다.   

아직 완벽하게 이해된 알고리즘은 아니지만, 이때 짝수와 홀수일때 경우를 나누어서 그 결과를 제곱하고 나머지를 구해주면 되었다.
