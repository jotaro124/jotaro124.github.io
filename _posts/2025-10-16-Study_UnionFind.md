---
layout: single
title: "유니온 파인드 공부"
categories: Graph
toc: true
toc_sticky : true
sidebar:
     nav: "docs"
---

# 유니온 파인드 개념
- 여러 개의 노드의 노드가 존재할 때 두개의 노드를 선택해서 현재 이 두 노드가 서로 같은 그래프에 속하는지 판별하는 알고리즘입니다.
- 그래프에서는 **그래프의 사이클이 생성되었는지 판별**하는 알고리즘으로 사용됩니다.
- 이 글은 유니온 파이드의 동작 원리와 그래프 사이클 판별하는 내용을 정리했습니다.

<br>

# 동작 원리
## 초기화
![sample](\images\2025-10-16-Study_UnionFind\0.png)
- 처음에는 노드가 연결되어 있지 않기 때문에 자기 자신을 부모 노드로 설정합니다.

## Union 연산
- Union 연산은 **두 개의 원소가 포함된 집합을 하나의 집합으로 합치는 연산**입니다.
![sample](\images\2025-10-16-Study_UnionFind\1.png)
- 노드 1과 노드 4를 연결합니다.
- 각 부모를 비교하면 노드 4가 더 크기 때문에 노드 4의 부모를 1로 설정합니다.
    - 반대로도 가능하지만 관행적으로 작은 노드를 기준으로 설정합니다.

![sample](\images\2025-10-16-Study_UnionFind\2.png)
- 노드 1와 노드 2를 연결합니다.
- 각 부모를 비교하면 노드 2가 더 크기 때문에 노드 2의 부모를 1로 설정합니다.

## Find 연산
- Find 연산은 **특정한 원소가 속한 집합이 어떤 집합인지 알려주는 연산**입니다.

![sample](\images\2025-10-16-Study_UnionFind\3.png)
- 노드 2와 노드 3을 연결합니다.
- 이때 노드 2의 부모가 1이기 때문에 **재귀를 통해서 부모에 해당하는 노드를 탐색**합니다.
- 노드 1의 부모가 1인 자기자신이기 때문에 재귀를 종료하고, 노드 3이 더 크기 때문에 노드 3의 부모를 1로 설정합니다.

## 경로 압축
![sample](\images\2025-10-16-Study_UnionFind\8.png)
- 다음 예시와 같이 Union 연산이 편향되게 이루어지면 Find 연산이 비효율적으로 동작합니다.
- 최악의 경우 **모든 노드를 전부 확인하게 되어 시간초과가 발생할 수 있습니다.**
- Find 연산을 재귀적으로 실행한 뒤 **부모 테이블 값을 바로 갱신**하면 해결할 수 있습니다.

## 코드 구현
### Find 함수 구현
~~~c++
// 특정 원소가 속한 집합을 찾는 함수
int FindParent(int node) {
	if (parent[node] == node) return parent[node]; // 부모와 노드가 같으면 재귀 종료
	return parent[node] = FindParent(parent[node]); // 부모와 노드가 다르면 재귀 시작
}
~~~
- 부모와 노드가 같을 때까지 재귀 호출 합니다.
- 경로 압축으로 부모 테이블을 갱신합니다.


### Union 함수 구현
~~~c++
// 두 개의 원소가 포함된 집합을 하나의 집합으로 합치는 함수
void UnionParent(int node1, int node2) {
	node1 = FindParent(node1);
	node2 = FindParent(node2);

	// 부모가 가장 큰 노드의 부모를 변경
	if (node1 < node2) parent[node2] = node1;
	else parent[node1] = node2;
}
~~~
- 두 노드의 부모 노드를 찾아 비교하고 가장 큰 노드의 부모를 변경합니다.

### 전체 코드
~~~c++
#include <iostream>

using namespace std;

int parent[100001]; // 부모 테이블

// 특정 원소가 속한 집합을 찾는 함수
int FindParent(int node) {
	if (parent[node] == node) return parent[node]; // 부모와 노드가 같으면 재귀 종료
	return parent[node] = FindParent(parent[node]); // 부모와 노드가 다르면 재귀 시작
}

// 두 개의 원소가 포함된 집합을 하나의 집합으로 합치는 함수
void UnionParent(int node1, int node2) {
	node1 = FindParent(node1);
	node2 = FindParent(node2);

	// 부모가 가장 큰 노드의 부모를 변경
	if (node1 < node2) parent[node2] = node1;
	else parent[node1] = node2;
}


int main() {

	int v = 4; // 정점의 수

	// 초기화
	for (int i = 1; i <= 4; i++)
		parent[i] = i;
	
	// 노드 연결
	UnionParent(1, 4);
	UnionParent(2, 3);
	UnionParent(1, 2);

	// 각 원소가 속한 집합을 출력
	for (int i = 1; i <= 4; i++)
		cout << FindParent(i) << " ";

	cout << "\n";

	// 부모 테이블 내용을 출력
	for (int i = 1; i <= 4; i++) {
		cout << parent[i] << " ";
	}

	return 0;
}
~~~

- 출력

~~~
1 1 1 1
1 1 1 1
~~~

<br>

# 사이클 판별
## 사이클 판별 과정
1. 각 간선을 하나씩 확인하며 두 노드의 부모를 확인합니다.
    - 부모가 서로 다르면 두 노드에 대하여 Union 연산을 수행합니다.
    - 부모가 서로 같으면 사이클이 발생한 것입니다.
2. 그래프에 포함되어 있는 모든 간선에 대하여 1번 과정을 반복합니다.

![sample](\images\2025-10-16-Study_UnionFind\4.png)
- 모든 노드에 대하여 자기 자신을 부모로 설정하는 형태로 부모 테이블을 초기화합니다.

![sample](\images\2025-10-16-Study_UnionFind\5.png)
- 간선 (1,2)를 확인합니다.
- 노드 1과 노드 2의 부모는 각각 1과 2로 서로 다릅니다.
- 더 큰 번호에 해당하는 노드 2의 부모를 1로 변경합니다.


![sample](\images\2025-10-16-Study_UnionFind\6.png)
- 간선 (1,3)을 확인합니다.
- 노드 1과 노드 3의 부모는 각각 1과 3으로 서로 다릅니다.
- 더 큰 번호에 해당하는 노드 3의 부모를 1로 변경합니다.


![sample](\images\2025-10-16-Study_UnionFind\7.png)
- 간선 (2,3)을 확인합니다.
- 노드 2와 노드 3의 부모는 모두 1로 서로 같습니다.
- 사이클이 발생한 것을 알 수 있습니다.

## 코드 구현

### 사이클 판별 부분
~~~c++
vector<pair<int, int>> es; // 주어진 간선의 정보
bool cycle = false; // 사이클 판별

// 노드 연결
for (pair<int, int> e : es) {
	if (FindParent(e.first) == FindParent(e.second)) {
		cycle = true;
		break;

	}
	else {
		UnionParent(e.first, e.second);
	}
}

if (cycle)
		cout << "사이클이 발생했습니다.\n";
	else
		cout << "사이클이 발생하지 않았습니다.\n";

~~~
- 간선의 정보를 바탕으로 부모를 비교해서 사이클 유무를 판별합니다.
- 부모가 다르면 연결하고, 부모가 같으면 사이클 발생을 알립니다.

### 전체 코드
~~~c++
#include <iostream>
#include <vector>

using namespace std;

int parent[100001]; // 부모 테이블

// 특정한 원소의 집합을 찾는 함수
int FindParent(int node) {
	if (parent[node] == node) return parent[node]; // 부모와 노드가 같으면 재귀 종료
	return parent[node] = FindParent(parent[node]); // 부모와 노드가 다르면 재귀 반복
}

// 두 원소의 집합을 합치는 함수
void UnionParent(int node1, int node2) {
	node1 = FindParent(node1);
	node2 = FindParent(node2);

	if (node1 < node2) parent[node2] = node1;
	else parent[node1] = node2;
}

int main() {

	int v = 3; // 주어진 정점의 수
	vector<pair<int, int>> es; // 주어진 간선의 정보
	bool cycle = false; // 사이클 판별

	es.push_back({ 1,2 });
	es.push_back({ 2,3 });
	es.push_back({ 1,3 });

	// 초기화
	for (int i = 1; i <= v; i++)
		parent[i] = i;

	// 노드 연결
	for (pair<int, int> e : es) {
		if (FindParent(e.first) == FindParent(e.second)) {
			cycle = true;
			break;

		}
		else {
			UnionParent(e.first, e.second);
		}
	}

	if (cycle)
		cout << "사이클이 발생했습니다.\n";
	else
		cout << "사이클이 발생하지 않았습니다.\n";


	return 0;
}
~~~
- 출력

~~~
사이클이 발생했습니다.
~~~

<br>

# 참조
- [알고리즘 코딩테스트 핵심이론 강의 - 그래프](https://youtu.be/MOZUzB8w2LA?si=JzCNd0CwYJvoweqv&t=95)
- [18강 - 합집합 찾기(Union-Find) [ 실전 알고리즘 강좌(Algorithm Programming Tutorial) #18 ]](https://youtu.be/AMByrd53PHM?si=OjKleUCgcAkZJxww)
- [알고리즘 코딩테스트 핵심이론 강의 - 유니온 파인드](https://youtu.be/klBh4ZglHYo?si=AH382lJEWSEx0ZzX)
- [(이코테 2021 강의 몰아보기) 8. 기타 그래프 이론](https://youtu.be/aOhhNFTIeFI?si=TPXpKE9I0n0eimdE)
