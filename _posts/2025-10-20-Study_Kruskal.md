---
layout: single
title: "크루스칼 알고리즘 공부"
categories: Graph
toc: true
toc_sticky : true
sidebar:
     nav: "docs"
---

# 크루스칼 알고리즘
- 최소 신장 트리를 만들기 위한 알고리즘입니다.
- 이 글은 **크루스칼 알고리즘의 동작 과정, 코드 구현 방법**에 대해서 정리한 내용입니다.

<br>

# 동작 과정
1. 간선 데이터를 가중치 기준으로 **오름차순 정렬**합니다.
2. **가중치 낮은 간선부터 확인**하며 현재의 간선이 **사이클을 발생시키는지 확인**합니다.
    - **사이클이 발생하지 않는 경우 최소 신장 트리에 포함시킵니다.**
    - **사이클이 발생하는 경우 최소 신장 트리에 포함시키지 않습니다.**
3. 모든 간선에 대하여 2번 과정을 반복합니다.

## 예시
![sample](\images\2025-10-20-Study_Kruskal\0.png)
- 간선 리스트로 그래프를 구현합니다.
    - 정렬을 위해서 **(가중치, 시작 노드, 도착 노드)**로 저장합니다.
- 사이클 테이블을 초기화합니다.
    - 초기에는 아무도 연결하지 않았기 때문에 **자기 자신을 부모로 설정**합니다.

![sample](\images\2025-10-20-Study_Kruskal\1.png)
- 간선 데이터를 가중치 기준으로 **오름차순 정렬**합니다.

![sample](\images\2025-10-20-Study_Kruskal\2.png)
- **가중치가 2**인 **간선 (1,2)**를 확인합니다.
- 노드 1과 노드 2의 부모는 서로 다르기 때문에 Union 연산을 수행합니다.
    - 유니온 파인드 알고리즘에 따라 노드 2의 부모를 1로 설정합니다.

![sample](\images\2025-10-20-Study_Kruskal\3.png)
- **가중치가 4**인 **간선 (1,3)**을 확인합니다.
- 노드 1과 노드 3의 부모는 서로 다르기 때문에 Union 연산을 수행합니다.
    - 유니온 파인드 알고리즘에 따라 노드 3의 부모를 1로 설정합니다.

![sample](\images\2025-10-20-Study_Kruskal\4.png)
- **가중치가 5**인 **간선 (2,3)**을 확인합니다.
- 노드 2과 노드 3의 부모는 1로 같기 때문에 Union 연산을 수행하지 않습니다.

![sample](\images\2025-10-20-Study_Kruskal\5.png)
- **가중치가 7**인 **간선 (2,4)**를 확인합니다.
- 노드 2과 노드 4의 부모는 서로 다르기 때문에 Union 연산을 수행합니다.
    - 유니온 파인드 알고리즘에 따라 노드 4의 부모를 1로 설정합니다.

![sample](\images\2025-10-20-Study_Kruskal\6.png)
- **가중치가 10**인 **간선 (3,4)**를 확인합니다.
- 노드 3과 노드 4의 부모는 1로 같기 때문에 Union 연산을 수행하지 않습니다.
- 모든 간선을 확인한 결과 주어진 그래프에서 최소 신장 트리는 **가중치 합이 13**인 부분 그래프가 됩니다.

<br>

# 코드 구현
## 간선 리스트 구현
~~~c++
vector<array<int, 3>> list_e; // 간선 리스트

// 간선 리스트 초기화
list_e.push_back({ 2,1,2 });
list_e.push_back({ 4,1,3 });
list_e.push_back({ 10,3,4 });
list_e.push_back({ 7,2,4 });
list_e.push_back({ 5,2,3 });
~~~
- 간선 리스트를 **vector**를 사용해서 구현합니다.
- **(가중치, 시작 노드, 도착 노드)**로 리스트를 저장합니다.

## 사이클 테이블 구현
~~~c++
int parent[100001]; // 유니온 파인드의 사이클 테이블

// 유니온 파인드 초기화
for (int i = 1; i <= 4; i++)
	parent[i] = i;
~~~
- 사이클 테이블을 **1차원 배열**로 구현합니다.
- 초기에는 노드가 연결되어 있지 않기 때문에 **자기 자신을 부모로 설정**합니다.

## Find 함수, Union 함수 구현
~~~c++
// 특정 원소의 집합을 찾는 함수
int FindParent(int node) {
	if (parent[node] == node) return parent[node]; // 부모와 노드가 같으면 재귀 종료
	return parent[node] = FindParent(parent[node]); // 부모와 노드가 다르면 재귀 호출
}

// 두 노드를 합치는 함수
void UnionParent(int node1, int node2) {
    // 노드의 부모를 탐색
	node1 = FindParent(node1);
	node2 = FindParent(node2);

	// 부모를 비교했을 때 부모가 큰 노드의 부모를 작은 노드의 부모 값에 넣는다.
	if (node1 < node2) 
		parent[node2] = node1;
	else 
		parent[node1] = node2;
}
~~~
- 특정 원소의 집합을 찾는 **Find 함수를 구현**합니다.
    - 부모와 노드가 같으면 재귀를 종료합니다.
    - 부모와 노드가 다르면 해당 노드의 부모를 찾기 위해 재귀 호출합니다.
    - **경로 압축**으로 재귀를 종료하면 **부모의 값을 바로 수정하게 설정합니다.**
- 두 노드를 합치는 **Union 함수를 구현**합니다.
    - 두 노드의 부모를 비교합니다.
    - **부모가 큰 노드의 부모를 작은 노드의 부모로 설정합니다.**

### 사이클 판별 부분
~~~c++
// 최소 신장 트리 구성
// 사이클이 발생하면 트리에 포함하지 않음
// 사이클이 발생하지 않으면 트리에 포함
int sum = 0; // 가중치 합
for (array<int, 3> edge : list_e) {
	if (FindParent(edge[1]) != FindParent(edge[2])) {
		sum += edge[0];
		UnionParent(edge[1], edge[2]);
	}
}
~~~
- 가중치가 낮은 간선부터 확인합니다.
- **시작 노드의 부모와 도착 노드의 부모가 다르면 Union 연산을 수행**합니다.

## 전체 코드
~~~c++
#include <iostream>
#include <vector>
#include <array>
#include <algorithm>

using namespace std;

int parent[100001]; // 유니온 파인드의 사이클 테이블
vector<array<int, 3>> list_e; // 간선 리스트

// 특정 원소의 집합을 찾는 함수
int FindParent(int node) {
	if (parent[node] == node) return parent[node]; // 부모와 노드가 같으면 재귀 종료
	return parent[node] = FindParent(parent[node]); // 부모와 노드가 다르면 재귀 호출
}

// 두 노드를 합치는 함수
void UnionParent(int node1, int node2) {
	// 노드의 부모를 탐색
	node1 = FindParent(node1);
	node2 = FindParent(node2);

	// 부모를 비교했을 때 부모가 큰 노드의 부모를 작은 노드의 부모 값에 넣는다.
	if (node1 < node2) 
		parent[node2] = node1;
	else 
		parent[node1] = node2;
}

int main() {

	// 간선 리스트 초기화
	list_e.push_back({ 2,1,2 });
	list_e.push_back({ 4,1,3 });
	list_e.push_back({ 10,3,4 });
	list_e.push_back({ 7,2,4 });
	list_e.push_back({ 5,2,3 });

	// 유니온 파인드 초기화
	for (int i = 1; i <= 4; i++)
		parent[i] = i;

	// 간선 리스트 가중치 기준 정렬
	sort(list_e.begin(), list_e.end());

	// 최소 신장 트리 구성
	// 사이클이 발생하면 트리에 포함하지 않음
	// 사이클이 발생하지 않으면 트리에 포함
	int sum = 0; // 가중치 합
	for (array<int, 3> edge : list_e) {
		if (FindParent(edge[1]) != FindParent(edge[2])) {
			sum += edge[0];
			UnionParent(edge[1], edge[2]);
		}
	}

	// 가중치 합 출력
	cout <<"가중치 합: " <<sum << "\n";

	return 0;
}
~~~
- 출력

~~~
가중치 합: 13
~~~

<br>

# 참조
- [19강 - 크루스칼 알고리즘(Kruskal Algorithm) [ 실전 알고리즘 강좌(Algorithm Programming Tutorial) #19 ]](https://youtu.be/LQ3JHknGy8c?si=YDgMLYHb8-mMSWe4)
- [(이코테 2021 강의 몰아보기) 8. 기타 그래프 이론](https://youtu.be/aOhhNFTIeFI?si=I1EM05pbkZH-KtPV&t=1303)
- [알고리즘 코딩테스트 핵심이론 강의 - 최소신장트리(MST)](https://youtu.be/N9xLuHLMIN8?si=3Di4zp5HZCbSRN-U)
- [[그래프 알고리즘] 크루스칼 알고리즘 Kruskal Algorithm](https://velog.io/@sy508011/%EA%B7%B8%EB%9E%98%ED%94%84-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%ED%81%AC%EB%A3%A8%EC%8A%A4%EC%B9%BC-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-Kruskal-Algorithm)
- [알고리즘 - 크루스칼 알고리즘(Kruskal Algorithm), 최소 신장 트리(MST)](https://chanhuiseok.github.io/posts/algo-33/)
- [[Java]크루스칼 알고리즘(Kruskal Algorithm)](https://sskl660.tistory.com/72)