---
layout: single
title: "BFS/DFS 공부"
categories: AlgorithmStudy
toc: true
toc_sticky : true
sidebar:
     nav: "docs"
---

# 그래프 표현
- 그래프는 그림을 이용하여 표현하는 것으로 가장 자연스럽고 이해하기에 가장 쉬운 방법이다. 
- 그러나 컴퓨터는 그림으로 표현된 정보를 이용할 수 없기 때문에 **인접 행렬**이나 **인접 리스트**에 의해 표현된다.

## 인접 행렬
![sample](\images\2025-07-02-StudyBFS_DFS\sample0.png)
- **2차원 배열**로 인접 행렬을 표현한다.
- 원래 정점의 개수가 N이면 N X N 행렬로 표현한다.
- 여기서 고려할 점은 배열 크기가 N이면 인덱스는 0 ~ N-1로 표현된다.
- **"정점 번호가 1번부터 N번까지"** 이기 때문에 인덱스 0을 제외하고 1부터 N까지 표현하기 위해서 배열 크기를 N+1로 한다.
- 정점이 양방향으로 연결되었다면 각각 1로 저장한다.

코드로 표현하면 다음과 같다
~~~c++
int map[5][5]; // 그래프 표현
int visited[5]; // 현재 방문한 정점 체크

// 간선 연결
for (int i = 0; i < M; i++) {
    cin >> a >> b;
    map[a][b] = 1;
    map[b][a] = 1;
}
~~~

## 인접 리스트
![sample](\images\2025-07-02-StudyBFS_DFS\sample1.png)
- **베열 Vector**를 사용해서 인접 리스트를 표현한다.
- 여기서 고려할 점은 배열 크기가 N이면 인덱스는 0 ~ N-1로 표현된다.
- **"정점 번호가 1번부터 N번까지"** 이기 때문에 인덱스 0을 제외하고 1부터 N까지 표현하기 위해서 배열 크기를 N+1로 한다.
- 정점이 양방향으로 연결되었다면 각각 1로 저장한다.

코드로 표현하면 다음과 같다
~~~c++
vector<int> map[5]; // 인접 리스트로 그래프 표현
int visited[5]; // 현재 방문한 정점

// 간선 연결
for (int i = 0; i < M; i++) {
    cin >> v1 >> v2;
    map[v1].push_back(v2);
    map[v2].push_back(v1);
}
~~~

<br>

# BFS/DFS
## BFS
~~~c++
void BFS(int v) {
    queue<int> q; // BFS용 큐 생성
    visited[next] = 1; // 방문기록
    q.push(V); // 시작노드 큐에 넣음

    while (!q.empty()) {
        int next = q.front(); // 큐 맨 앞에 값을 방문
        q.pop(); // 큐에서 제거

        for(int i=1; i<=N; i++) {
            if (map[next][i] == 1 && visited[i] == 0) {
                visited[i] = 1; // 방문기록
                q.push(i); // 큐에 넣음
            }
        }
    }
}
~~~
1. 시작 정점 v를 결정한다.
2. 시작 정점을 방문기록한다.
3. 시작 정점을 큐에 넣는다.
4. 큐가 공백인지 검사하고 공백이 될떄까지 반복한다.
5. 큐 맨 앞에 정점을 방문한다.
6. **방문한 정점은 큐에서 제거**한다.
7. 1번부터 N번까지 정점를 탐색한다.
8. 현재 정점과 연결되었고 방문하지 않은 정점이 있으면 방문기록하고 그 정점을 큐에 집어 넣었는다.

## DFS
~~~c++
void DFS(int v) {
    visited[v] = 1; // 방문기록

    // 그래프 탐색 방법
    for (int i = 1; i <= N; i++) {
        // 현재 정점과 연결되어있고 방문되지 않았으면 발동
        if (map[v][i] == 1 && visited[i] == 0)
            DFS(i);
        }
}
~~~
1. 시작 정점 v를 결정한다.
2. 현재 방문한 정점을 기록한다.
3. 1번부터 N번까지 정점을 탐색한다.
4. 현재 정점과 연결되었고 방문하지 않은 정점이 있으면 그 정점으로 **재귀호출**한다.

<br>

# 참조
- C로 배우는 쉬운 자료구조 4탄, 이지영, 한빛아카데미, 2022
- 4차 산업혁명 시대의 이산수학, 김대수, 생능출판, 2019
- [[자료구조] C++ vector로 그래프 구현하기 (포인터 없이 인접리스트 구현)](https://breakcoding.tistory.com/129)
- [[C++] 백준 2606 - 바이러스](https://velog.io/@hyunjoon0803/C-%EB%B0%B1%EC%A4%80-2606-%EB%B0%94%EC%9D%B4%EB%9F%AC%EC%8A%A4)
- [[백준][Python] 1260번 DFS와 BFS(DFS/BFS 기본 구현 자세히)](https://velog.io/@falling_star3/%EB%B0%B1%EC%A4%80Python-1260%EB%B2%88-DFS%EC%99%80BFS)
- [[백준] 1260 DFS와 BFS C++ (BFS/DFS)](https://velog.io/@matcha_/%EB%B0%B1%EC%A4%80-1260-DFS%EC%99%80-BFS-C-BFSDFS)