---
layout: single
title: "배열 초기화 방법 공부"
categories: AlgorithmStudy
toc: true
toc_sticky : true
sidebar:
     nav: "docs"
---

코딩 테스트를 할 때 배열을 선언하고 초기화 하는 경우가 많습니다.
전역 변수로 선언하면 직접 초기화할 필요가 없지만, **한 번 실행 후 테스트 케이스가 여러 개인 경우, 배열을 매 케이스마다 초기화할 필요가 있습니다.** 
c/c++에서는 이를 방지하기 위해 **memset과 fill** 두 가지의 배열 초기화 함수를 제공합니다.

# memset() 함수
- **"cstring" 또는 "string.h" 헤더** 사용합니다.
- 배열 원소를 초기화하는 것이 아니라 메모리 값을 초기화하기 때문에 **0과 -1만 사용가능합니다.** 그 외의 수는 원하는 값으로 초기화 되지 않습니다.

~~~c++
void memset(void *ptr, int value, size_t num)
~~~
- **void *ptr**: 배열이나 어떤 container의 자료형의 시작 포인터
- **int value**: 배열이나 어떤 container에 채울 자료 (이 값은 unsigned char로 변환되어 삽입)
- **size_t num**: 배열이나 어떤 container에 int value 값을 얼마나 넣을지 결정

## 1차원 배열

~~~c++
// (초기화 해줄 배열의 주소, 초기화 할 값, 배열의 크기)
memset(arr, 0, sizeof(arr));
~~~
- 1차원 배열을 초기화하는 방법은 **배열의 이름**을 매개변수로 넣으면 됩니다.

### 예시 코드

~~~c++
#include <iostream>
#include <cstring>

using namespace std;

int main() {

	int arr[5] = { 1,2,3,4,5 };

	memset(arr, 0, sizeof(arr));

	for (int num : arr)
		cout << num << " ";

	return 0;
}
~~~
- 출력 결과

~~~
0 0 0 0 0
~~~

## 2차원 배열

~~~c++
// (초기화 해줄 배열의 주소, 초기화 할 값, 배열의 크기)
memset(arr, 0, sizeof(arr));
~~~

- 2차원 배열을 초기화하는 방법은 **배열의 이름**을 매개변수로 넣으면 됩니다.

### 예시 코드

~~~c++
#include <iostream>
#include <cstring>

using namespace std;

int main() {

	int arr[3][3] = { {1,2,3}, {4,5,6}, {7,8,9} };

	memset(arr, 0, sizeof(arr));

	for (int i = 0; i < 3; i++) {
		for (int j = 0; j < 3; j++) {
			cout << arr[i][j] << " ";
		}
		cout << "\n";
	}

	return 0;
}
~~~
- 출력 결과

~~~
0 0 0
0 0 0
0 0 0
~~~

<br>

# fill() 함수
- **"algorithm" 헤더**를 사용합니다.
- memset과 다르게 **value 자료형에서의 오버플로우가 아니라면 원하는 값을 채울 수 있습니다.**

~~~c++
void fill(ForwardIt first, ForwardIt last, const T& value)
~~~
- **ForwardIt first**: 배열이나 어떤 container 시작 Iterator, 배열의 경우 첫 포인터
- **ForwardIt last**: 배열이나 어떤 container 끝 Iterator, 배열의 경우 마지막 포인터
- **const T& value**: 배열이나 어떤 container 자료형에 넣을 값

## 1차원 배열

~~~c++
// (시작할 배열의 주소, 끝나는 주소, 초기화 할 값)
fill(arr, arr+5, 0);
~~~
- 1차원 배열을 초기화하는 방법은 **배열의 이름**을 매개변수로 넣으면 됩니다.

### 예시 코드

~~~c++
#include <iostream>
#include <algorithm>

using namespace std;

int main() {

	int arr[5] = { 1,2,3,4,5 };

	fill(arr, arr + 5, 0);

	for (int num : arr)
		cout << num << " ";

	return 0;
}
~~~
- 출력 결과

~~~
0 0 0 0 0
~~~

## 2차원 배열

~~~c++
// (시작할 배열의 주소, 끝나는 주소, 초기화 할 값)
fill(&arr[0][0], &arr[0][0] + 3 * 3, 0);
~~~

- 2차원 배열을 초기화하는 방법은 **&arr[0][0]**을 매개변수로 넣으면 됩니다.

### 예시 코드

~~~c++
#include <iostream>
#include <algorithm>

using namespace std;

int main() {

	int arr[3][3] = { {1,2,3}, {4,5,6}, {7,8,9} };

	fill(&arr[0][0], &arr[0][0] + 3 * 3, 0);

	for (int i = 0; i < 3; i++) {
		for (int j = 0; j < 3; j++) {
			cout << arr[i][j] << " ";
		}
		cout << "\n";
	}

	return 0;
}
~~~
- 출력 결과

~~~
0 0 0
0 0 0
0 0 0
~~~

<br>

# 참조
- [C++에서 배열 초기화하기](https://blog.naver.com/bestowing/221823736677)
- [(C++)배열의 초기화 방법 및 memset함수 사용시 주의 점](https://dev-sbee.tistory.com/101)
- [C++에서 배열을 초기화 하는 3가지 방법](https://almond0115.tistory.com/entry/C%EC%97%90%EC%84%9C-%EB%B0%B0%EC%97%B4%EC%9D%84-%EC%B4%88%EA%B8%B0%ED%99%94-%ED%95%98%EB%8A%94-3%EA%B0%80%EC%A7%80-%EB%B0%A9%EB%B2%95)
- [[백준] C++ 1012번 유기농 배추](https://dangdangee.tistory.com/entry/%EB%B0%B1%EC%A4%80-C-1012%EB%B2%88-%EC%9C%A0%EA%B8%B0%EB%86%8D-%EB%B0%B0%EC%B6%94)
- [[백준 1012번 유기농 배추 문제]](https://ayun-programmer.tistory.com/33)
- [(C++) 백준 1012번 [유기농 배추]](https://faceyourfear.tistory.com/46)
- [[C++][백준][DFS] 1012번 유기농 배추](https://code-kh-studio.tistory.com/5)