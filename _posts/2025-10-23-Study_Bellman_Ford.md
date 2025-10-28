---
layout: single
title: "벨만-포드 알고리즘 공부"
categories: Graph
toc: true
toc_sticky : true
sidebar:
     nav: "docs"
---

# 벨만-포드 알고리즘
- 벨만-포드 알고리즘은 **특정 출발 노드에서 다른 모든 노드까지의 최단 경로**를 구하는 알고리즘입니다.
    - 다익스트라 알고리즘과 달리 **음수 간선이 포함된 상황에서 최단 거리 문제**를 해결할 수 있습니다.
    - 전체 그래프에서 **음수 사이클 존재 여부를 판단**할 때 사용되는 알고리즘이기도 합니다.
- 이 글은 **벨만-포드 알고리즘의 동작 과정과 코드 구현 방법**에 대해서 정리한 내용입니다.

<br>

# 동작 과정
1. 간선 리스트로 그래프를 구현하고, 최단 경로 리스트 초기화합니다.
    - 벨만-포드 알고리즘은 **간선 리스트를 사용**해서 동작합니다.
    - 최단 경로 리스트의 **출발 노드는 0으로, 나머지 노드는 무한대로 초기화**합니다.
2. 모든 간선을 확인해서 최단 경로 리스트를 갱신합니다.
    - 갱신 반복 횟수는 **노드 개수 -1**
    - **D[S] != INF 이고 D[e] > D[s] + w** 일 때 **D[e] = D[s] + w** 조건으로 갱신합니다.
3. 음수 사이클 유무를 확인합니다.
    - **2번 과정의 과정을 한번 더 수행**했을 때 **최단 거리 리스트가 갱신된다면 음수 간선 순환이 존재하는 것입니다.**

## 예시
![sample](\images\2025-10-23-Study_Bellman_Ford\0.png)
- 주어진 그래프를 바탕으로 간선 리스트와 최단 거리 배열을 초기화합니다.
- **노드 개수가 3이기 때문에 2번 반복 수행합니다.**

![sample](\images\2025-10-23-Study_Bellman_Ford\c1_1.png)
- 첫번째 반복을 시작합니다.
- 간선 리스트에 **1->2 간선을 확인**합니다.
- dist[1] = 0, dist[2] = INF, INF > 0 + 4이기 때문에 **dist[2] = 4**입니다.

![sample](\images\2025-10-23-Study_Bellman_Ford\c1_2.png)
- 간선 리스트에 **1->3 간선**을 확인합니다.
- d[1] = 0, dist[3] = INF, INF > 0 + 3이기 때문에 **dist[3] = 3**입니다.

![sample](\images\2025-10-23-Study_Bellman_Ford\c1_3.png)
- 간선 리스트에 **2->3 간선**을 확인합니다.
- d[2] = 4, dist[3] = 3, 3 > 4 - 4이기 때문에 **dist[3] = 0**입니다.

![sample](\images\2025-10-23-Study_Bellman_Ford\c1_4.png)
- 간선 리스트에 **3->1 간선**을 확인합니다.
- d[3] = 0, dist[1] = 0, 0 > 0 - 2이기 때문에 **dist[1] = -2**입니다.

![sample](\images\2025-10-23-Study_Bellman_Ford\c2.png)
- 두번째 반복을 시작하고, 모든 간선을 확인합니다.

![sample](\images\2025-10-23-Study_Bellman_Ford\c3.png)
- 노드 개수가 3이기 때문에 2번 수행했을 때 최단 경로를 정해집니다.
- **음수 사이클 유무를 확인하기 위해 세번째 반복을 시작하고, 모든 간선을 확인합니다.**
- 최단 거리 배열이 갱신되었기 때문에 **음수 사이클이 존재하는 것을 확인할 수 있습니다.**


<br>

# 코드 구현
## 벨만-포드 알고리즘 구현
~~~c++
// 벨만-포드 알고리즘 수행하는 함수
bool bellman_ford(int start) {
	// 출발 노드 설정
	dist[start] = 0; 

	// N번 반복
	for (int i = 1; i <= num; i++) {
		// 매 반복마다 모든 간선을 확인
		for (int j = 0; j < graph.size(); j++) {
			int start = graph[j][1]; // 출발 노드
			int end = graph[j][2]; // 도착 노드
			int value = graph[j][0]; // 가중치

			// 출발 노드가 무한이면 다음으로 넘어간다.
			// 출발 노드의 거쳐간 값보다 도착 노드이 작으면 다음으로 넘어간다.
			if (dist[start] == INF) continue;
			if (dist[end] <= dist[start] + value) continue;

			dist[end] = dist[start] + value; // 최단 거리 갱신
			
			// N반 반복했을 때 갱신이 발생하면 음수 사이클이 발동했다는 의미
			if (i == num) return true;
		}
		print();
		cout << "------------------------------------------------\n";
	}

	return false;

}
~~~
- 벨만-포드 알고리즘의 과정은 **모든 간선을 확인하고, 최단 거리 배열의 값을 갱신하는 과정**을 **N-1 번 반복** 입니다.
    - 코드에서 **N번 반복하는 코드**로 구현한 이유는 **음수 사이클 유무도 같이 확인**하기 위해서 구현했습니다.

## 전체 코드
~~~c++
#include <iostream>
#include <array>
#include <vector>
#include <algorithm>

using namespace std;

vector<array<int, 3>> graph; // 간선 리스트로 그래프 표현
long long dist[4]; // 최단 거리 배열
int INF = 1e9; // 최단 거리 배열 초기값
int num = 3; // 주어진 노드의 수

// 최단 거리 배열 출력 함수
void print() {

	for (int i = 1; i <= num; i++) {
		if (dist[i] == INF)
			cout << "INF ";
		else
			cout << dist[i] << " ";
		
	}
	cout << "\n";
}

// 간선 리스트로 표현할 그래프 초기화 함수
void setGraph() {
	graph.push_back({ 4,1,2 });
	graph.push_back({ 3,1,3 });
	graph.push_back({ -4,2,3 });
	graph.push_back({ -2,3,1 });
}

// 벨만-포드 알고리즘 수행하는 함수
bool bellman_ford(int start) {
	// 출발 노드 설정
	dist[start] = 0; 

	// N번 반복
	for (int i = 1; i <= num; i++) {
		// 매 반복마다 모든 간선을 확인
		for (int j = 0; j < graph.size(); j++) {
			int start = graph[j][1]; // 출발 노드
			int end = graph[j][2]; // 도착 노드
			int value = graph[j][0]; // 가중치

			// 출발 노드가 무한이면 다음으로 넘어간다.
			// 출발 노드의 거쳐간 값보다 도착 노드이 작으면 다음으로 넘어간다.
			if (dist[start] == INF) continue;
			if (dist[end] <= dist[start] + value) continue;

			dist[end] = dist[start] + value; // 최단 거리 갱신
			
			// N개의 간선 확인 때 갱신이 발생하면 음수 사이클이 발동했다는 의미
			if (i == num) return true;
		}
		print();
		cout << "------------------------------------------------\n";
	}

	return false;

}

int main() {

	// 그래프 표현
	setGraph();

	// 최단 거리 노드 초기화
	fill(dist,dist+4, INF);

	if (bellman_ford(1))
		cout << "음수 사이클이 발생했습니다.\n";
	else
		cout << "음수 사이클이 발생하지 않았습니다.\n";

	return 0;
}
~~~
- 출력

~~~
-2 4 0
------------------------------------------------
-4 2 -2
------------------------------------------------
음수 사이클이 발생했습니다.

~~~

<br>

# 참조
- [코딩 테스트를 위한 벨만 포드 알고리즘 7분 핵심 요약](https://youtu.be/Ppimbaxm8d8?si=kSV91L4NiCoyUPuC)
- [알고리즘 코딩테스트 핵심이론 강의 - 벨만포드](https://youtu.be/061eXyAFRuI?si=RSqyEPv-0JLTO78r)
- [알고리즘 코딩테스트 문제풀이 강의 - 062. 타임머신으로 빨리 가기 (백준 11657)](https://youtu.be/SaxPonfjV5M?si=v31NVYXRcOnsqyP6)
- [[ 벨만포드 알고리즘 ] 개념과 구현방법 (C++)](https://yabmoons.tistory.com/365)
- [[알고리즘] 벨만-포드 알고리즘 (Bellman-Ford Algorithm)](https://velog.io/@kimdukbae/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%B2%A8%EB%A7%8C-%ED%8F%AC%EB%93%9C-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-Bellman-Ford-Algorithm)
- [벨만-포드 알고리즘(Bellman-Ford Algorithm)](https://8iggy.tistory.com/153)
- [[벨만-포드 알고리즘] 한 살도 이해하는 벨만-포드 알고리즘(Bellman-Ford Algorithm)](https://10000cow.tistory.com/entry/%EB%B2%A8%EB%A7%8C-%ED%8F%AC%EB%93%9C-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%ED%95%9C-%EC%82%B4%EB%8F%84-%EC%9D%B4%ED%95%B4%ED%95%98%EB%8A%94-%EB%B2%A8%EB%A7%8C-%ED%8F%AC%EB%93%9C-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98Bellman-Ford-Algorithm)
- [벨만 포드(Bellman Ford) 알고리즘](https://goo-gy.github.io/2021-08-15-Bellman-Ford)
