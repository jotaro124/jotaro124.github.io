---
layout: single
title: "투 포인터 공부 "
categories: TwoPointer
toc: true
toc_sticky : true
sidebar:
     nav: "docs"
---

# 투 포인터
- 두 개의 포인터를 만들어서, 각각이 가리키는 원소에 의미를 부여하여 요구하는 문제를 해결하는 알고리즘입니다.
- 정확히는 배열이나 리스트에서 **두 개의 포인터**를 사용하여 **특정 조건을 만족하는 구간**을 효율적으로 탐색하는 알고리즘입니다. 일반적으로 정렬되어 있을 때 사용됩니다.

## 특징
- 이중 for문으로 O(N^2)에 처리되는 작업을 두개의 포인터의 움직임으로 O(N)에 해결할 수 있습니다.
- 문제를 보고 직관적으로 생각하기 어려워서 여러 문제를 공부해야 합니다.
     - **O(N^2)으로 구현했을 때 시간 제한을 초과**하면 연속하는 특징을 활용할 수 있는지 확인합니다.
     - 또한 문제에서 **두 수**라는 단어가 나오면 투 포인터로 해결가능하는지 확인합니다.

## 수행 단계
1. 배열 또는 리스트에 **첫 번째 포인터와 두 번째 포인터**를 설정합니다.
2. 두 포인터 사이의 구간을 조사하고 조건을 확인합니다.
3. 조건을 만족할 경우, 원하는 결과를 얻었으므로 알고리즘을 종료합니다.
4. 조건을 만족하지 않을 경우, 첫 번째 또는 두 번째 포인터를 이동시킵니다.
5. 다시 2번 단계로 돌아가 반복합니다.

## 유형
- 투 포인터가 같은 방향으로 진행하는 경우
- 투 포인터가 양끝에서 반대 방향으로 진행하는 경우

<br>

# 투 포인터가 같은 방향으로 진행하는 경우
[수들의 합 2(백준 2003](https://www.acmicpc.net/problem/2003)를 예시로 설명하겠습니다.

![sample](\images\2025-08-11-Study_TwoPointer\S.png)
- **SUM < M**이기 때문에 SUM이 더 커져야 하는 상황입니다.
- **E를 더 오른쪽**으로 가야하는 상항입니다.

![sample](\images\2025-08-11-Study_TwoPointer\S1.png)
- **SUM < M**이기 때문에 SUM이 더 커져야 하는 상황입니다.
- **E를 더 오른쪽**으로 가야하는 상항입니다.

![sample](\images\2025-08-11-Study_TwoPointer\S2.png)
- **SUM > M**이기 때문에 SUM이 더 작아져야 하는 상황입니다.
- **S를 더 오른쪽**으로 가야하는 상황입니다.

![sample](\images\2025-08-11-Study_TwoPointer\S3.png)
- **SUM = M**이기 때문에 SUM이 커져야 하는 상황입니다.
- **E를 오른쪽**으로 가야하는 상황입니다.

![sample](\images\2025-08-11-Study_TwoPointer\S4.png)
- **SUM > M**이기 때문에 SUM이 작아져야 하는 상황입니다.
- **S를 더 오른쪽**으로 가야하는 상황입니다.

![sample](\images\2025-08-11-Study_TwoPointer\S5.png)
- 이 과정을 **E가 주어진 배열의 범위**를 넘어갈 때까지 반복하면, **SUM과 M 사이의 관계**를 통해서 문제에서 요구하는 값을 얻을 수 있습니다.

## 의사코드
~~~
1. 주어진 N과 M을 입력한다.

2. 주어진 수열 A를 입력한다.

3. start = 0 // 시작점이 가리키는 인덱스

4. end = 0 // 끝점이 가리키는 인덱스

5. sum = A의 첫번째 원소

6. while end < N
     6.1 if sum > M then
          6.1.1 sum = sum - start가 가리키는 A의 원소
          6.1.2 다음 반복으로 넘어간다.

     6.2 if sum과 M이 같으면 경우의 수 1 추가한다.

     6.3 if end < N-1 then
          6.3.1 end = end + 1
          6.3.2 sum = sum + end가 가리키는 A의 원소
     6.4 else
          6.4.1 end = end + 1

7. 경우의 수를 출력한다.

~~~
- **배열의 크기가 N**인 배열이 주어졌을 때 배열의 인덱스는 **0 ~ N-1**까지 처리하기 때문에 배열에 접근하는 **포인터는 N이 되어서는 안됩니다.**

## 전체코드

~~~c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int N, M; // 수의 개수, 목표
vector<int> A; // 주어진 수열
int result = 0; // 경우의 수

int main() {

	// 수의 개수, 목표 입력
	cin >> N >> M;

	// 주어진 수열의 원소 입력
	A.resize(N);
	for (int i = 0; i < N; i++)
		cin >> A[i];

	int start = 0; // 첫번째 포인터
	int end = 0; // 두번째 포인터
	int sum = A[0]; // 누적합

	// 경우의 수 계산
	while (end < N) {
		
		// 누적합이 목표보다 크면 start를 이동
		if (sum > M) {
			sum -= A[start++];
			continue;
		}

		// 누적합과 목표가 같으면 result 증가
		if (sum == M) result++;

		// 누적합이 목표보다 작으면 end를 이동
		// end 포인터에 원소가 있는지 주의
		if (end < N - 1) sum += A[++end];
		else end++;

	}

	// 결과 출력
	cout << result;

	return 0;
}
~~~

<br>

# 투 포인터가 양끝에서 반대 방향으로 진행하는 경우
[용액 (백준 2467)](https://www.acmicpc.net/problem/2467)를 예시로 설명하겠습니다.

![sample](\images\2025-08-11-Study_TwoPointer\D.png)
- **SUM < M**이기 때문에 SUM이 커져야 하는 상황입니다.
- **S를 오른쪽**으로 가야하는 상황입니다.

![sample](\images\2025-08-11-Study_TwoPointer\D1.png)
- **SUM > M**이기 때문에 SUM이 작아져야 하는 상황입니다.
- **E를 왼쪽**으로 가야하는 상황입니다.

![sample](\images\2025-08-11-Study_TwoPointer\D2.png)
- **SUM > M**이기 때문에 SUM이 작아져야 하는 상황입니다.
- **E를 왼쪽**으로 가야하는 상황입니다.

![sample](\images\2025-08-11-Study_TwoPointer\D3.png)
- **SUM < M**이기 때문에 SUM이 커져야 하는 상황입니다.
- **S를 오른쪽**으로 가야하는 상황입니다.

![sample](\images\2025-08-11-Study_TwoPointer\D4.png)
- **S와 E의 위치가 같아질 때**까지 반복하면, **SUM과 M 사이의 관계**를 통해서 문제에서 요구하는 값을 얻을 수 있습니다.

## 의사코드
~~~
1. 전체 용액의 수 N 입력한다.

2. 용액의 특성값을 나타내는 N개의 정수가 빈칸 사이에 두고 오름차순으로 입력한다.

3. start = 0 // 시작점을 배열의 첫번째로 한다.

4. end = N-1 // 끝점을 배열의 마지막으로 한다.

5. while start < end
     5.1 혼합된 특성값 = 시작점의 배열 + 끝점의 배열

     5.2 if 최솟값> 혼합된 특성값의 절대값 then
          5.2.1 최솟값 = 혼합된 특성값의 절대값

          5.2.2  0에 가까운 용액을 만들어내는 두 용액의 특성값을 저장한다.

          5.2.3 만약 혼합된 특성값이 0이면 반복문 종료

     5.3 if 혼합된 특성값 > 0 then
          5.3.1 end = end - 1 // 왼쪽으로 이동시킨다.
     5.4 else
          5.4.1 start = start + 1 // 오른쪽으로 이동시킨다.

6. 0에 가까운 용액을 만들어내는 두 용액의 특성값을 출력한다.
~~~

## 전체코드

~~~c++
#include <iostream>
#include <vector>
#include <climits>
#include <algorithm>

using namespace std;

int N; // 전체 용액의 수
int result[2]; // 결과 배열
vector<int> A; // 용액 특성값 배열
int Min = INT_MAX; // 가장 0에 가까운 값

int main() {

	// 전체 용액의 수 입력
	cin >> N;
	A.resize(N);

	// 용액 특성값 입력
	for (int& a : A)
		cin >> a;

	int start = 0; // 배열의 시작점
	int end = N - 1; // 배열의 끝점

	// 특성값이 0에 가까운 혼합 시작
	while (start < end) {
		// 혼합된 특성값
		int sum = A[start] + A[end];

		// 절대값 기준으로 가장 작은 값을 찾는다.
		if (Min > abs(sum)) {
			Min = abs(sum);
			result[0] = A[start];
			result[1] = A[end];

			// 두 합이 0이면 반복문을 나온다.
			if (sum == 0)
				break;
		}

		// 두 합이 양수이면 end--
		// 두 합이 음수이면 start++
		if (sum > 0)
			end--;
		else
			start++;
	}

	// 결과 출력
	cout << result[0] << " " << result[1];

	return 0;
}
~~~

<br>

# 참조
- [(C++) 투포인터 알고리즘, 슬라이딩 윈도우 알고리즘, Prefix Sum 구간합](https://ansohxxn.github.io/algorithm/twopointer/#-%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%94%A9-%EC%9C%88%EB%8F%84%EC%9A%B0)
- [[알고리즘 강의] Two pointers algorithm, 투 포인터 기법](https://youtu.be/iSjvuixMPmQ?si=8oDu3vcpqOd3D2AZ)
- [코딩테스트 알고리즘 - 5. 투포인터](https://youtu.be/U0TXIFiCIu0?si=1tWMBu9_0wvpizLq)
- [[Java/알고리즘] 투 포인터 알고리즘(Two Pointer Algorithm) 이해하기 -1 : 종류, 활용방안](https://adjh54.tistory.com/384)
- [알고리즘 코딩테스트 문제풀이 강의 - 008. 좋은 수 구하기 (백준 1253)](https://youtu.be/esesL0D9vsE?si=wxkdIZRxEId3AHhh)


