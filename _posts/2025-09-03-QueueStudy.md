---
layout: single
title: "Queue 기본 사용법 및 예제"
categories: C_CPP
toc: true
toc_sticky : true
sidebar:
     nav: "docs"
---

# Queue의 개념
- 대표적인 **FIFO(First-In First-Out)** 방식입니다.
- 한쪽 끝에서 삽입이 일어나고 반대쪽 끝에서 삭제가 일어나는 선형 리스트입니다.

<br>

# Queue 사용법
- queue 헤더 파일을 포함해야 합니다.
- queue<[데이터 타입]> 이름; 으로 queue를 선언합니다.
~~~c++
#include <queue>
queue<int> q;
~~~

<br>

# Queue 기본 함수
- **queue.push(element)**
    - 큐의 뒤에 원소를 추가하는 함수

- **queue.pop()**
    - 큐의 앞에 원소를 삭제하는 함수

- **queue.front()**
    - 큐 제일 앞에 있는 원소를 반환하는 함수

- **queue.back()**
    - 큐 제일 뒤에 있는 원소를 반환하는 함수

- **queue.size()**
    - 큐의 사이즈를 반환하는 함수

- **queue.empty()**
    - 큐가 비어있으면 true 아니면 false를 반환하는 함수

<br>

# Queue 기본 사용 예시
~~~c++
#include <iostream>
#include <queue>

using namespace std;

int main(){

	// 큐 생성
	queue<int> q;

	// push
	q.push(1);
	q.push(2);
	q.push(3);
	q.push(4);
	q.push(5);
	q.push(6);

	// pop
	q.pop();
	q.pop();
	q.pop();

	// front
	cout << "front element: " << q.front() << '\n';

	// back
	cout << "back element: " << q.back() << '\n';

	// size
	cout << "queue size: " << q.size() << '\n';

	// empty
	cout << "Is it empty?: " << (q.empty() ? "Yes" : "No") << '\n';

	return 0;

}
~~~

- 결과

~~~
front element: 4
back element: 6
queue size: 3
Is it empty?: No
~~~

<br>

# 참조
- [[C++][STL] Queue 기본 사용법 및 예제](https://life-with-coding.tistory.com/408)
- [[C++] C++ STL queue 기본 사용법과 예제](https://twpower.github.io/76-how-to-use-queue-in-cpp)
- [[C++] queue container 정리 및 사용법](https://blockdmask.tistory.com/101)