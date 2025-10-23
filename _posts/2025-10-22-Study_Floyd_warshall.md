---
layout: single
title: "플로이드-워셜 알고리즘 공부"
categories: Graph
toc: true
toc_sticky : true
sidebar:
     nav: "docs"
---

# 플로이드-워셜 알고리즘
- 플로이드-워셜 알고리즘은 **모든 노드에서 다른 모든 노드까지 최단 경로**를 구하는 알고리즘입니다.
- 이 글은 **플로이드-워셜 알고리즘의 동작 과정과 코드 구현 방법**에 대해서 정리한 내용입니다.


# 동작 과정
1. 최단 거리 배열을 선언하고 초기화를 합니다.
2. 최단 거리 배열에 그래프 데이터를 저장합니다.
3. 점화식으로 최단 거리 배열의 데이터를 갱신합니다.
	- 각 단계마다 **특정 노드 k를 거쳐 가는 경우를 확인**합니다.
	- s에서 e로 가는 최단 거리보다 a에서 k를 거쳐 e로 가는 거리가 짧은지 검사합니다.
    - 점화식은 **D[s][e] = Min(D[s][e], D[s][k]+D[k][e])**입니다.

## 예시

![sample](\images\2025-10-22-Study_Floyd_warshall\0.png)
- 주어진 그래프를 바탕으로 최단 거리 배열을 초기화합니다.
    - 인접한 노드라면 그 거리를 값으로 넣습니다.
    - 인접한 노드가 아니라면 무한이라는 값을 넣습니다.
    - 자기 자신에서 자기 자신으로 향하는 배열은 0을 넣습니다.

![sample](\images\2025-10-22-Study_Floyd_warshall\1.png)
- 1번 노드를 거쳐 가는 경우를 고려합니다.
- 점화식을 통해서 계산합니다.
    - D[2][3] = Min(D[2][3],D[2][1]+D[1][3]) = min(1,3+6) = 1
    - D[2][4] = Min(D[2][4],D[2][1]+D[1][4]) = min(INF,3+7) = 10
    - D[3][2] = Min(D[3][2],D[3][1]+D[1][2]) = min(1,6+3) = 1
    - D[3][4] = Min(D[3][4],D[3][1]+D[1][4]) = min(1,6+7) = 1
    - D[4][2] = Min(D[4][2],D[4][1]+D[1][2]) = min(INF,7+3) = 10
    - D[4][3] = Min(D[4][3],D[4][1]+D[1][3]) = min(1,7+6) = 1

![sample](\images\2025-10-22-Study_Floyd_warshall\2.png)
- 2번 노드를 거쳐 가는 경우를 고려합니다.
- 마찬가지로 점화식을 통해서 계산합니다.

![sample](\images\2025-10-22-Study_Floyd_warshall\3.png)
- 3번 노드를 거쳐 가는 경우를 고려합니다.
- 마찬가지로 점화식을 통해서 계산합니다.

![sample](\images\2025-10-22-Study_Floyd_warshall\4.png)
- 4번 노드를 거쳐 가는 경우를 고려합니다.
- 마찬가지로 점화식을 통해서 계산합니다.
- 이렇게 **모든 노드에서 다른 모든 노드까지의 최단 경로**를 모두 계산하였습니다.


# 코드 구현
## 최단 거리 배열 초기화
~~~c++
// 최단 거리 배열 초기화
for (int i = 1; i <= 4; i++) {
	for (int j = 1; j <= 4; j++) {
		if (i == j) dist[i][j] = 0;
		else if (graph[i][j]) dist[i][j] = graph[i][j];
		else dist[i][j] = INF;
	}
}
~~~
- 최단 거리 배열 초기화합니다.
    - 자기 자신에서 자신 자신까지는 0으로 저장합니다.
    - 인접 행렬에 저장된 내용을 저장합니다.
    - 나머지는 무한으로 표시합니다.

## 플로이드-워셜 알고리즘 구현
~~~c++
// 플로이드-워셜 알고리즘 수행
for (int k = 1; k <= 4; k++) {
	for (int s = 1; s <= 4; s++) {
		for (int e = 1; e <= 4; e++) {
			dist[s][e] = min(dist[s][e], dist[s][k] + dist[k][e]);
		}
	}
}
~~~
- 점화식 **D[s][e] = Min(D[s][e], D[s][k]+D[k][e])**를 수행


## 전체 코드
~~~c++

#include <iostream>
#include <algorithm>

using namespace std;

int graph[5][5]; // 인접 행렬로 그래프 표현
int INF = 1e9; // 최단 거리 초기화 값
int dist[5][5]; // 최단 거리 배열

// 인접 행렬 초기화하는 함수
void setGraph() {
	graph[1][2] = 3;
	graph[1][3] = 6;
	graph[1][4] = 7;
	graph[2][1] = 3;
	graph[2][3] = 1;
	graph[3][1] = 6;
	graph[3][2] = 1;
	graph[3][4] = 1;
	graph[4][1] = 7;
	graph[4][3] = 1;
}

int main() {

	// 인접 행렬 초기화
	setGraph();

	// 최단 거리 배열 초기화
	for (int i = 1; i <= 4; i++) {
		for (int j = 1; j <= 4; j++) {
			if (i == j) dist[i][j] = 0;
			else if (graph[i][j]) dist[i][j] = graph[i][j];
			else dist[i][j] = INF;
		}
	}
	
	// 플로이드-워셜 알고리즘 수행
	for (int k = 1; k <= 4; k++) {
		for (int s = 1; s <= 4; s++) {
			for (int e = 1; e <= 4; e++) {
				dist[s][e] = min(dist[s][e], dist[s][k] + dist[k][e]);
			}
		}
	}

	cout << "모든 노드에 모든 노드까지의 최단 경로\n";
	for (int i = 1; i <= 4; i++) {
		for (int j = 1; j <= 4; j++) {
            if (dist[i][j] == (int)1e9)
				cout << "INF" << " ";
			else
				cout << dist[i][j] << " ";
		}
		cout << "\n";
	}

	return 0;
}
~~~
- 출력

~~~
모든 노드에 모든 노드까지의 최단 경로
0 3 4 5
3 0 1 2
4 1 0 1
5 2 1 0
~~~


<br>

# 참조
- [26강 - 플로이드 와샬 알고리즘(Floyd Warshall Algorithm) [ 실전 알고리즘 강좌(Algorithm Programming Tutorial) #26 ]](https://youtu.be/9574GHxCbKc?si=OY2r9EY4-kzkmWoI)
- [코딩테스트 알고리즘 - 11. 플로이드](https://youtu.be/H3HrTqKB0u8?si=dvM7_VVJAcyuTjtK)
- [(이코테 2021 강의 몰아보기) 7. 최단 경로 알고리즘](https://youtu.be/acqm9mM1P6o?si=wkzdp5Y_BwYCWJDS&t=2606)
- [알고리즘 코딩테스트 핵심이론 강의 - 플로이드워셜](https://youtu.be/ibYzw9XAzyc?si=_D6Z7vp8faSVN93n)
- [[알고리즘] 플로이드 워셜 알고리즘 (Floyd-Warshall Algorithm)](https://velog.io/@kimdukbae/%ED%94%8C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EC%9B%8C%EC%85%9C-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-Floyd-Warshall-Algorithm)
- [알고리즘 - 플로이드-워셜(Floyd-Warshall) 알고리즘](https://chanhuiseok.github.io/posts/algo-50/)
- [[Algorithm] 플로이드-워셜(Floyd-Warshall) 알고리즘](https://trillium.tistory.com/20)
- [[알고리즘] 플로이드 워셜 with Python](https://doing7.tistory.com/77)
- [플로이드-워셜 알고리즘(Floyd-Warshall Algorithm)](https://8iggy.tistory.com/154)
- [플로이드-워셜(Floyd-Warshall) 알고리즘](https://limecoding.tistory.com/224)
- [[알고리즘] Floyd-Warshall Algorithm : 플로이드 워셜 알고리즘이란?](https://olrlobt.tistory.com/43)
- [플로이드-워셜(Floyd-Warshall) 알고리즘 이론과 파이썬 구현](https://it-garden.tistory.com/247)

