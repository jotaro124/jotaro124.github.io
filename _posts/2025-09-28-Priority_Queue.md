---
layout: single
title: "Priority Queue 기본 사용법 및 예제"
categories: C_CPP
toc: true
toc_sticky : true
sidebar:
     nav: "docs"
---

# Priority Queue의 개념
- 일반적인 큐(Queue)는 먼저 집어넣은 데이터가 먼저 나오는 FIFO 구조로 저장하는 선형 자료구조입니다.
- 하지만 **우선순위 큐(Priority Queue)**는 들어간 순서에 상관없이 **우선순위가 높은 데이터가 먼저 나오는 형태의 자료구조**입니다.
- 우선순위 큐는 일반적으로 **Heap**이라는 자료구조를 사용해서 구현합니다.

<br>

# Priority Queue의 사용법
- queue 헤더 파일을 포함해야 합니다.
- priority_queue<[데이터 타입]> pq; 으로 priority_queue를 선언합니다.
- 또는 priority_queue<[데이터 타입], Container<[데이터 타입]>, Compare> pq; 으로 선언할 수 있습니다.
    - **Container**는 벡터 같은 컨테이너를 말하고, **Compare**는 비교 함수 입니다.
- 기본적으로 우선순위 큐는 Max Heap으로 설정되어 있습니다.
    - 즉, Compare 함수는 내림차순 정렬하는 **less**을 기본적으로 사용합니다.

~~~c++
#include <queue>

priority_queue<int> pq;
priority_queue<int, vector<int>, less<int>> pq;
~~~

<br>

# Priority Queue 기본 함수
- **push()**
    - 우선순위 큐에 원소를 삽입합니다.
- **pop()**
    - 우선순위 큐에서 top의 원소를 삭제합니다.
- **top()**
    - 우선순위 큐에서 우선순위가 높은 원소를 반환합니다.
- **empty()**
    - 우선순위 큐가 비어있으면 true 아니면 false를 반환합니다.
- **size()**
    - 우선순위 큐의 원소 수를 반환합니다.

<br>

# Priority Queue 예시
## 기본 예시
~~~c++
#include <iostream>
#include <queue>

using namespace std;

int main() {

	//우선순위 큐 선언
	priority_queue<int> pq;

	// 우선순위 큐에 원소를 삽입
	pq.push(5);
	pq.push(7);
	pq.push(1);
	pq.push(10);

	cout << "우선순위 큐 사이즈: " << pq.size() << "\n";

	// 우선순위 큐에 원소가 없을 때까지 반복
	while (!pq.empty()) {
		cout << pq.top() << " ";
		pq.pop();
	}

	return 0;
}
~~~

- 결과

~~~
우선순위 큐 사이즈: 4
10 7 5 1
~~~

## Min Heap 예시
- 기본적으로 우선순위 큐는 Max Heap이기 때문에 Min Heap으로 사용하려면 **greater**을 less 대신 사용하면 가능합니다.
- 이처럼 정렬 기준을 임의로 바꿔야 할 경우, 축약한 선언은 불가능하고 필요한 값들을 전부 넣어서 선언해야 합니다.

~~~c++
priority_queue<int, vector<int>, greater<int>> pq;
~~~

- 전체 코드

~~~c++
#include <iostream>
#include <queue>

using namespace std;

int main() {

	// 우선순위 큐 선언
	priority_queue<int, vector<int>, greater<int>> pq;

	// 우선순위 큐에 원소를 삽입
	pq.push(5);
	pq.push(7);
	pq.push(1);
	pq.push(10);

	cout << "우선순위 큐 사이즈: " << pq.size() << "\n";

	// 우선순위 큐에 원소가 없을 때까지 반복
	while (!pq.empty()) {
		cout << pq.top() << " ";
		pq.pop();
	}

	return 0;
}
~~~

- 결과

~~~
우선순위 큐 사이즈: 4
1 5 7 10
~~~

## 구조체 예시
- less<>, greater<>을 사용하지 않고 임의로 정렬 기준을 변경하는 예시입니다.

~~~c++
#include <iostream>
#include <queue>

using namespace std;

// 오름차순으로 정렬
struct cmp {
	bool operator()(int a, int b) {
		return a > b;
	}
};

int main() {

	// 우선순위 큐 선언
	priority_queue<int, vector<int>, cmp> pq;

	// 우선순위 큐에 원소를 삽입
	pq.push(5);
	pq.push(7);
	pq.push(1);
	pq.push(10);

	cout << "우선순위 큐 사이즈: " << pq.size() << "\n";

	// 우선순위 큐에 원소가 없을 때까지 반복
	while (!pq.empty()) {
		cout << pq.top() << " ";
		pq.pop();
	}

	return 0;
}
~~~

- 결과

~~~
우선순위 큐 사이즈: 4
1 5 7 10
~~~

<br>

# Priority Queue와 Pair 사용법
- 우선순위 큐에 pair를 사용해야 하는 경우가 있습니다.
- 기본적으로 **pair<int,int>**를 넣으면 pair에서 first값을 기준으로 Max Heap을 동작합니다.
- 만약 Min Heap으로 사용하고자 한다면 **greater<>**를 사용하면 됩니다.

~~~c++
priority_queue<pair<int,int>> pq;
priority_queue<pair<int,int>,vector<pair<int,int>>,greater<pair<int,int>>> pq;
~~~

- 예시 코드

~~~c++
#include <iostream>
#include <queue>

using namespace std;

struct cmp {
struct cmp {
	bool operator() (pair<int,int> a, pair<int,int> b) {
		if (a.first == b.first)
			return a.second > b.second;
		else
			return a.first < b.first;
	}
};
};

int main() {

	// 우선순위 큐 선언
	priority_queue<pair<int, int>> pq;

	// 우선순위 큐에 원소 삽입
	pq.push({ 1,3 });
	pq.push({ 1,4 });
	pq.push({ 2,3 });
	pq.push({ 4,3 });

	cout << "우선순위 큐 사이즈: " << pq.size() << "\n";

	// 우선순위 큐에 원소가 없을 때까지 반복
	cout << "기본 결과" << "\n";
	while (!pq.empty()) {
		cout << pq.top().first << ", " << pq.top().second << "\n";
		pq.pop();
	}

	cout << "-------------------------------" << "\n";
	cout << "정렬 기준 변경" << "\n";

	//우선순위 큐 선언
	priority_queue<pair<int, int>, vector<pair<int, int>>, cmp> pq2;

	// 우선순위 큐에 원소 삽입
	pq2.push({ 1,3 });
	pq2.push({ 1,4 });
	pq2.push({ 2,3 });
	pq2.push({ 4,3 });

	// 우선순위 큐에 원소가 없을 때까지 반복
	while (!pq2.empty()) {
		cout << pq2.top().first << ", " << pq2.top().second << "\n";
		pq2.pop();
	}

	return 0;
}
~~~

- 결과

~~~
우선순위 큐 사이즈: 4
기본 결과
4, 3
2, 3
1, 4
1, 3
-------------------------------
정렬 기준 변경
4, 3
2, 3
1, 3
1, 4
~~~

<br>

# 참조
- [[자료구조] 우선순위 큐와 힙 (Priority Queue & Heap)](https://suyeon96.tistory.com/31)
- [[Data Structure] 우선순위 큐(Priority Queue)란?](https://tmdrnr96.tistory.com/32)
- [우선순위 큐 (Priority Queue) 개념 및 구현](https://yoongrammer.tistory.com/81)
- [[C++ STL] Priority_queue 사용법](https://kbj96.tistory.com/15)
- [c++ 우선순위 큐 priority_queue 사용법 [컴공과고씨]](https://hagisilecoding.tistory.com/60)
- [알고리즘 - c++의 priority_queue 사용법](https://chanhuiseok.github.io/posts/algo-54/)
- [[C++] Priority Queue & Pair 사용법](https://8156217.tistory.com/11)
- [[c++][자료구조] heap 구현 / STL / Priority Queue 총 정리](https://8156217.tistory.com/10)