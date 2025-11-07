---
layout: single
title: "배열 복사 방법 공부"
categories: AlgorithmStudy
toc: true
toc_sticky : true
sidebar:
     nav: "docs"
---

# memcpy() 함수
- **"cstring" 또는 "string.h" 헤더** 사용합니다.

~~~c++
void* memcpy (void* dest, const void* source, size_t num)
~~~
- **void* dest**: 복사받을 곳을 가리키는 포인터
- **const void* source**: 복사할 메모리를 가리키는 포인터
- **size_t num**: 복사할 데이터의 크기(바이트 단위)

## 1차원 배열

~~~c++
memcpy(arr_copy, arr, sizeof(arr));
~~~
- 1차원 배열을 복사하는 방법은 **배열의 이름**을 매개변수로 넣으면 됩니다.

### 예시 코드

~~~c++
#include <iostream>
#include <cstring>

using namespace std;

int main() {

	int arr[5] = { 1,2,3,4,5 };
	int arr_copy[5] = { 0 };

	cout << "복사 전\n";
	for (int num : arr_copy)
		cout << num << " ";
	cout << "\n";

	memcpy(arr_copy, arr, sizeof(arr));
	cout << "\n";

	cout << "복사 후\n";
	for (int num : arr_copy)
		cout << num << " ";
	cout << "\n";

	return 0;
}
~~~
- 출력 결과

~~~
복사 전
0 0 0 0 0

복사 후
1 2 3 4 5
~~~

<br>

### 주의점

~~~c++
#include <iostream>
#include <cstring>

using namespace std;

int main() {

	char arr[5] = "1234";
	char arr_copy1[6] = "10000";
	char arr_copy2[6] = "10000";
	
	cout << "복사 전\n";
	cout << arr_copy1 << "\n";
	cout << arr_copy2 << "\n";

	// arr의 길이만 복사
	memcpy(arr_copy1, arr, sizeof(char) * 4);

	// arr의 길이 + 1 만큼 복사
	// sizeof(arr)과 크기 동일
	memcpy(arr_copy2, arr, sizeof(char) * 4 + 1);
	cout << "\n";

	cout << "복사 후\n";
	cout << arr_copy1 << "\n";
	cout << arr_copy2 << "\n";

	return 0;
}
~~~

- 출력 결과

~~~
복사 전
10000
10000

복사 후
12340
1234
~~~
- char형태의 데이터를 복사할 때에는 **\0을 포함**해줘야 하기 때문에 **size에 1을 더해주어야 합니다.**

## 2차원 배열

~~~c++
memcpy(arr_copy, arr, sizeof(arr));
~~~
- 2차원 배열을 복사하는 방법은 **배열의 이름**을 매개변수로 넣으면 됩니다.

### 예시 코드

~~~c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

int main() {

	int arr[3][3] = { {1,2,3}, {4,5,6}, {7,8,9} };
	int arr_copy[3][3] = { 0 };

	cout << "복사 전\n";
	for (int i = 0; i < 3; i++) {
		for (int j = 0; j < 3; j++) {
			cout << arr_copy[i][j] << " ";
		}
		cout << "\n";
	}

	cout << "\n";

	memcpy(arr_copy, arr, sizeof(arr));
	cout << "\n";

	cout << "복사 후\n";
	for (int i = 0; i < 3; i++) {
		for (int j = 0; j < 3; j++) {
			cout << arr_copy[i][j] << " ";
		}
		cout << "\n";
	}

	cout << "\n";

	return 0;
}
~~~
- 출력 결과

~~~
복사 전
0 0 0
0 0 0
0 0 0


복사 후
1 2 3
4 5 6
7 8 9
~~~

<br>

# copy() 함수
- **"algorithm" 헤더**를 사용합니다.

~~~c++
OutputIt copy( InputIt first, InputIt last, OutputIt d_first );
~~~
- **InputIt first**: 복사할 메모리를 가리키는 포인터
- **InputIt last**: 복사할 메모리의 마지막을 가리키는 포인터
- **OutputIt d_first**: 복사받을 메모리를 가리키는 포인터

## 1차원 배열
~~~c++
copy(arr, arr + 5, arr_copy);
~~~
- 1차원 배열을 복사하는 방법은 **배열의 이름**을 매개변수로 넣으면 됩니다.

### 예시 코드

~~~c++
#include <iostream>
#include <algorithm>

using namespace std;

int main() {

	int arr[5] = { 1,2,3,4,5 };
	int arr_copy[5] = { 0 };

	cout << "복사 전\n";
	for (int num : arr_copy)
		cout << num << " ";
	
	cout << "\n";

	copy(arr, arr + 5, arr_copy);
	cout << "\n";

	cout << "복사 후\n";
	for (int num : arr_copy)
		cout << num << " ";
	
	cout << "\n";

	return 0;
}
~~~
- 출력 결과

~~~
복사 전
0 0 0 0 0

복사 후
1 2 3 4 5
~~~

## 2차원 배열

~~~c++
copy(&arr[0][0], &arr[0][0] + 3 * 3, &arr_copy[0][0]);
~~~
- 2차원 배열을 복사하는 방법은 **&arr[0][0]**을 매개변수로 넣으면 됩니다.

### 예시 코드

~~~c++
#include <iostream>
#include <algorithm>

using namespace std;

int main() {

	int arr[3][3] = { {1,2,3}, {4,5,6}, {7,8,9} };
	int arr_copy[3][3] = { 0 };

	cout << "복사 전\n";
	for (int i = 0; i < 3; i++) {
		for (int j = 0; j < 3; j++) {
			cout << arr_copy[i][j] << " ";
		}
		cout << "\n";
	}

	cout << "\n";

	copy(&arr[0][0], &arr[0][0] + 3 * 3, &arr_copy[0][0]);
	cout << "\n";

	cout << "복사 후\n";
	for (int i = 0; i < 3; i++) {
		for (int j = 0; j < 3; j++) {
			cout << arr_copy[i][j] << " ";
		}
		cout << "\n";
	}

	cout << "\n";

	return 0;
}
~~~
- 출력 결과

~~~
복사 전
0 0 0
0 0 0
0 0 0


복사 후
1 2 3
4 5 6
7 8 9
~~~

<br>

# 참조
- [[개념정리] c++ 메모리 영역 복사 (memcpy, copy)](https://dbstndi6316.tistory.com/282)
- [[c++] std::copy 2차원 배열 복사](https://velog.io/@min_gi1123/c-stdcopy-2%EC%B0%A8%EC%9B%90-%EB%B0%B0%EC%97%B4-%EB%B3%B5%EC%82%AC)
- [[C/C++] memcpy 사용 방법](https://zoosso.tistory.com/1064)
- [[C++] 문자열 string vs char 배열 선언 방식 비교](https://jimmy-ai.tistory.com/277)


