---
layout: single
title: "다익스트라 알고리즘 공부"
categories: Graph
toc: true
toc_sticky : true
sidebar:
     nav: "docs"
---

# 다익스트라 알고리즘
- 다익스트라 알고리즘은 최단 거리를 구하는 알고리즘입니다.
- **특정한 노드에서 출발하여 다른 모든 노드로 가는 최단 경로**를 계산합니다.
    - **음의 간선이 없을 때** 정상적으로 동작하는 알고리즘입니다.
- 이 글은 **다익스트라 알고리즘의 동작 과정과 코드 구현 방법**에 대해서 정리한 내용입니다.

<br>

# 동작 과정
1. 출발 노드를 설정합니다.
2. 최단 거리 테이블을 초기화합니다.
3. 방문하지 않은 노드 중에서 **최단 거리가 가장 짧은 노드를 선택**합니다.
4. **해당 노드를 거쳐 다른 노드로 가는 비용을 계산하여 최단 거리 테이블을 갱신**합니다.
5. 위 과정에서 3번과 4번을 반복합니다.

## 예시

![sample](\images\2025-10-21-Study_Dijkstra\0.png)
- 그래프를 준비하고 **출발 노드를 1로 설정**하여 우선순위 큐에 삽입합니다.
    - 우선순위 큐를 사용하는 이유는 **방문하지 않은 노드 중에서 최단 거리가 가장 짧은 노드를 선택하는 시간을 개선**하기 위해서 입니다.

![sample](\images\2025-10-21-Study_Dijkstra\1.png)
- **우선순위 큐에서 1번 노드를 꺼냅니다.**
- 1번 노드는 아직 방문하지 않았으므로 처리합니다.
    - **노드 1번의 거리는 0**이고, **우선순위 큐에서 꺼낸 거리는 0**입니다.
    - 최단 거리를 계산하지 않았기 때문에 방문하지 않은 노드입니다.
- 인접 노드 2번을 탐색합니다.
    - 노드 2번의 거리는 INF이고, 현재 노드에서 거쳐간 비용은 3입니다.
    - 갱신이 가능하기 때문에 **노드 2번의 거리를 3으로 갱신**합니다.
    - 우선순위 큐에 거리와 노드를 삽입합니다.
- 인접 노드 3번을 탐색합니다.
    - 노드 3번의 거리는 INF이고, 현재 노드에서 거쳐간 비용은 6입니다.
    - 갱신이 가능하기 때문에 **노드 3번의 거리를 6으로 갱신**합니다.
    - 우선순위 큐에 거리와 노드를 삽입합니다.
- 인접 노드 4번을 탐색합니다.
    - 노드 4번의 거리는 INF이고, 현재 노드에서 거쳐간 비용은 7입니다.
    - 갱신이 가능하기 때문에 **노드 4번의 거리를 7로 갱신**합니다.
    - 우선순위 큐에 거리와 노드를 삽입합니다.

![sample](\images\2025-10-21-Study_Dijkstra\2.png)
- **우선순위 큐에서 2번 노드를 꺼냅니다.**
- 2번 노드는 아직 방문하지 않았으므로 처리합니다.
    - **노드 2번의 거리는 3**이고, **우선순위 큐에서 꺼낸 거리는 3**입니다.
    - 최단 거리를 계산하지 않았기 때문에 방문하지 않은 노드입니다.
- 인접 노드 1번을 탐색합니다.
    - 노드 1번의 거리는 0이고, 현재 노드에서 거쳐간 비용은 6이기 때문에 갱신하지 않습니다.
- 인접 노드 3번을 탐색합니다.
    - 노드 3번의 거리는 6이고, 현재 노드에서 거쳐간 비용은 4입니다.
    - 갱신이 가능하기 때문에 **노드 3번의 거리를 4로 갱신**합니다.
    - 우선순위 큐에 거리와 노드를 삽입합니다.

![sample](\images\2025-10-21-Study_Dijkstra\3.png)
- **우선순위 큐에서 3번 노드를 꺼냅니다.**
- 3번 노드는 아직 방문하지 않았으므로 처리합니다.
    - **노드 3번의 거리는 4**이고, **우선순위 큐에서 꺼낸 거리는 4**입니다.
    - 최단 거리를 계산하지 않았기 때문에 방문하지 않은 노드입니다.
- 인접 노드 1번을 탐색합니다.
    - 노드 1번의 거리는 0이고, 현재 노드에서 거쳐간 비용은 10이기 때문에 갱신하지 않습니다.
- 인접 노드 2번을 탐색합니다.
    - 노드 2번의 거리는 3이고, 현재 노드에서 거쳐간 비용은 5이기 때문에 갱신하지 않습니다.
- 인접 노드 4번을 탐색합니다.
    - 노드 4번의 거리는 7이고, 현재 노드에서 거쳐간 비용은 5입니다.
    - 갱신이 가능하기 때문에 **노드 4번의 거리를 5로 갱신**합니다.
    - 우선순위 큐에 거리와 노드를 삽입합니다.

![sample](\images\2025-10-21-Study_Dijkstra\4.png)
- **우선순위 큐에서 4번 노드를 꺼냅니다.**
- 4번 노드는 아직 방문하지 않았으므로 처리합니다.
    - **노드 4번의 거리는 5**이고, **우선순위 큐에서 꺼낸 거리는 5**입니다.
    - 최단 거리를 계산하지 않았기 때문에 방문하지 않은 노드입니다.
- 인접 노드 1번을 탐색합니다.
    - 노드 1번의 거리는 0이고, 현재 노드에서 거쳐간 비용은 12이기 때문에 갱신하지 않습니다.
- 인접 노드 3번을 탐색합니다.
    - 노드 3번의 거리는 4이고, 현재 노드에서 거쳐간 비용은 5이개 때문에 갱신하지 않습니다.

![sample](\images\2025-10-21-Study_Dijkstra\5.png)
- **우선순위 큐에서 3번 노드를 꺼냅니다.**
- 3번 노드는 이미 방문하기 때문에 처리하지 않습니다.
    - **노드 3번의 거리는 4**이고, **우선순위 큐에서 꺼낸 거리는 6**이기 때문에 최단 거리를 이미 계산한 노드입니다.

![sample](\images\2025-10-21-Study_Dijkstra\6.png)
- **우선순위 큐에서 4번 노드를 꺼냅니다.**
- 4번 노드는 이미 방문하기 때문에 처리하지 않습니다.
    - **노드 4번의 거리는 5**이고, **우선순위 큐에서 꺼낸 거리는 7**이기 때문에 최단 거리를 이미 계산한 노드입니다.
- 이렇게 **출발 노드 1번에서 모든 노드와의 최단 거리**를 구했습니다.

<br>

# 코드 구현
## 다익스트라 함수 구현
### 함수 내 변수 선언
~~~c++
// 최단 거리가 짧은 노드를 찾아내는 큐
priority_queue < pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
dist[start] = 0; // 자기 자신의 최단 거리는 0
pq.push({ 0,start }); // 출발 노드 데이터 큐에 삽입
~~~
- **최단 거리가 짧은 노드를 찾아내기 위해 우선순위 큐**를 사용합니다.
    - c++에서 우선순위 큐는 기본적으로 최대 힙이기 때문에 **greater<>**를 넣어서 선언해야 합니다.
- 최단 거리가 짧은 노드를 찾아내기 위해서 **거리를 첫번째 원소**로 해서 큐에 삽입합니다.

### while문 내부
~~~c++
// 현재 노드와 거리
int curNode = pq.top().second;
int curDist = pq.top().first;
pq.pop(); // 현재 노드 데이터 제거

// 현재 노드의 거리가 최단 거리보다 크면 다음으로 넘어간다.
// 이미 방문한 노드로 판단
if (dist[curNode] < curDist) continue;
~~~
- **최단 거리를 계산할 노드가 없을 때까지 반복**합니다.
- 큐의 상단에 있는 노드와 거리로 이미 방문한 노드인지 확인합니다.
    - 현재 노드의 거리가 최단 거리보다 크다는 것은 **이미 최단 거리가 정해졌다는 의미**입니다.

### for문의 인접 노드 탐색
~~~c++
// 연결된 노드 확인
for (pair<int, int> data : graph[curNode]) {
	// 인접한 노드와 거리
	int nextNode = data.first;
	int nextDist = data.second;

	// 현재 노드를 거친 비용
	int cost = curDist + nextDist;

	// 비용이 최단 거리보다 작으면 최단 거리 갱신
	if (cost < dist[nextNode]) {
		dist[nextNode] = cost;
		pq.push({ cost,nextNode });
	}
}
~~~
- **현재 노드를 거친 비용**이 **현재 최단 거리보다 작으면 최단 거리를 갱신**합니다.
    - **인접 리스트를 (노드, 거리)**로 구현하고, **우선순위 큐를 (거리, 노드)**로 구현해서 처리할 때 실수하는 경우가 발생했습니다. 
    - 그래서 **인접 리스트를 (거리,노드)** 형식으로 통일해서 구현하기로 결정했습니다.

### 다익스트라 함수 전체 코드
~~~c++
// 한 노드에서 모든 노드와의 최단 거리 계산하는 함수
void dijkstra(int start) {
	// 최단 거리가 짧은 노드를 찾아내는 큐
	priority_queue < pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
	dist[start] = 0; // 자기 자신의 최단 거리는 0
	pq.push({ 0,start }); // 출발 노드 데이터 큐에 삽입

	// 최단 거리를 계산할 노드가 없을 때까지 반복
	while (!pq.empty()) {
		// 현재 노드와 가중치
		int curNode = pq.top().second;
		int curDist = pq.top().first;
		pq.pop(); // 현재 노드 데이터 제거

		// 현재 노드의 거리가 최단 거리보다 크면 다음으로 넘어간다.
		// 이미 방문한 노드로 판단
		if (dist[curNode] < curDist) continue;

		// 연결된 노드 확인
		for (pair<int, int> data : graph[curNode]) {
			// 인접한 노드와 거리
			int nextNode = data.first;
			int nextDist = data.second;

			// 현재 노드를 거친 비용
			int cost = curDist + nextDist;

			// 비용이 최단 거리보다 작으면 최단 거리 갱신
			if (cost < dist[nextNode]) {
				dist[nextNode] = cost;
				pq.push({ cost,nextNode });
			}
		}
	}

}
~~~

## 전체 코드
~~~c++
#include <iostream>
#include <vector>
#include <queue>
#include <climits>

using namespace std;

vector<pair<int, int>> graph[5]; // 그래프 표현 인접 리스트
int dist[5]; // 최단 거리 배열
int INF = INT_MAX; // 초기 최단 거리 배열의 초기값에 사용

void print(priority_queue<pair<int,int>,vector<pair<int,int>>,greater<pair<int,int>>> pq) {
	cout << "우선순위 큐\n";
	while (!pq.empty()) {
		cout << "거리: " << pq.top().first<<", ";
		cout << "노드: " << pq.top().second << "\n";
		pq.pop();
	}

	cout << "--------------------------------------\n";
	for (int i = 1; i <= 4; i++) {
		if (dist[i] == INF)
			cout << "INF ";
		else
			cout << dist[i] << " ";
	}
	cout << "\n";
	cout << "--------------------------------------\n";
	cout << "\n";
}

// 한 노드에서 모든 노드와의 최단 거리 계산하는 함수
void dijkstra(int start) {
	// 최단 거리가 짧은 노드를 찾아내는 큐
	priority_queue < pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
	dist[start] = 0; // 자기 자신의 최단 거리는 0
	pq.push({ 0,start }); // 출발 노드 데이터 큐에 삽입

	// 최단 거리를 계산할 노드가 없을 때까지 반복
	while (!pq.empty()) {
		print(pq);
		// 현재 노드와 가중치
		int curNode = pq.top().second;
		int curDist = pq.top().first;
		pq.pop(); // 현재 노드 데이터 제거

		// 현재 노드의 거리가 최단 거리보다 크면 다음으로 넘어간다.
		// 이미 방문한 노드로 판단
		if (dist[curNode] < curDist) continue;

		// 연결된 노드 확인
		for (pair<int, int> data : graph[curNode]) {
			// 인접한 노드와 가중치
			int nextNode = data.first;
			int nextDist = data.second;

			// 현재 노드를 거친 비용
			int cost = curDist + nextDist;

			// 비용이 최단 거리보다 작으면 최단 거리 갱신
			if (cost < dist[nextNode]) {
				dist[nextNode] = cost;
				pq.push({ cost,nextNode });
			}
		}
	}
}

int main() {

	// 그래프 표현
	graph[1].push_back({ 2,3 });
	graph[1].push_back({ 3,6 });
	graph[1].push_back({ 4,7 });

	graph[2].push_back({ 1,3 });
	graph[2].push_back({ 3,1 });
	
	graph[3].push_back({ 1,6 });
	graph[3].push_back({ 2,1 });
	graph[3].push_back({ 4,1 });

	graph[4].push_back({ 1,7 });
	graph[4].push_back({ 3,1 });

	// 최단거리 배열 초기화
	for (int i = 1; i <= 4; i++)
		dist[i] = INF;

	// 1번 노드에서 모든 노드와의 최단 거리 계산
	dijkstra(1);

	// 1번 노드에서 모든 모든 노드와의 최단 거리 출력
	cout << "1번 노드에서 모든 노드와의 최단 거리 출력\n";
	for (int i = 1; i <= 4; i++)
		cout << dist[i] << " ";

	return 0;
}
~~~
- 출력

~~~
우선순위 큐
거리: 0, 노드: 1
--------------------------------------
0 INF INF INF
--------------------------------------

우선순위 큐
거리: 3, 노드: 2
거리: 6, 노드: 3
거리: 7, 노드: 4
--------------------------------------
0 3 6 7
--------------------------------------

우선순위 큐
거리: 4, 노드: 3
거리: 6, 노드: 3
거리: 7, 노드: 4
--------------------------------------
0 3 4 7
--------------------------------------

우선순위 큐
거리: 5, 노드: 4
거리: 6, 노드: 3
거리: 7, 노드: 4
--------------------------------------
0 3 4 5
--------------------------------------

우선순위 큐
거리: 6, 노드: 3
거리: 7, 노드: 4
--------------------------------------
0 3 4 5
--------------------------------------

우선순위 큐
거리: 7, 노드: 4
--------------------------------------
0 3 4 5
--------------------------------------

1번 노드에서 모든 노드와의 최단 거리 출력
0 3 4 5
~~~

<br>

# 참조
- [25강 - 다익스트라 알고리즘(Dijkstra Algorithm) [ 실전 알고리즘 강좌(Algorithm Programming Tutorial) #25 ]](https://youtu.be/611B-9zk2o4?si=dNx74dB2k9Q_WZgp)
- [(이코테 2021 강의 몰아보기) 7. 최단 경로 알고리즘](https://youtu.be/acqm9mM1P6o?si=zEoZseEAGzEh552j&t=91)
- [알고리즘 코딩테스트 핵심이론 강의 - 다익스트라](https://youtu.be/XIwiZZr2l5I?si=hFEI6I1MQw89bXeU)
- [23. 다익스트라(Dijkstra) 알고리즘](https://m.blog.naver.com/ndb796/221234424646)
- [[알고리즘] 다익스트라(Dijkstra) 알고리즘](https://velog.io/@717lumos/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%8B%A4%EC%9D%B5%EC%8A%A4%ED%8A%B8%EB%9D%BCDijkstra-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)
- [[알고리즘] 다익스트라 - 최단 경로 구하기(Dijkstra), C/C++](https://meojiktard.tistory.com/13)
- [[필수 알고리즘] 다익스트라 알고리즘(Dijkstra Algorithm) 이해](https://cobi-98.tistory.com/46)
- [[ 다익스트라 알고리즘 ] 개념과 구현방법 (C++)](https://yabmoons.tistory.com/364)
- [[Java]다익스트라 알고리즘(Dijkstra Algorithm)](https://sskl660.tistory.com/59)


